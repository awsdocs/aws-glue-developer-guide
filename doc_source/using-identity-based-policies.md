# Using Identity\-Based Policies \(IAM Policies\) for AWS Glue<a name="using-identity-based-policies"></a>

This topic provides examples of identity\-based policies that demonstrate how an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\) and thereby grant permissions to perform operations on AWS Glue resources\.

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available to manage access to your AWS Glue resources\. For more information, see [Overview of Managing Access Permissions to Your AWS Glue Resources](access-control-overview.md)\. 

The sections in this topic cover the following:
+ [Permissions Required to Use the AWS Glue Console](#console-permissions)
+ [AWS Managed \(Predefined\) Policies for AWS Glue](#access-policy-examples-aws-managed)

The following shows an example of a permissions policy for Amazon DynamoDB\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DescribeQueryScanBooksTable",
            "Effect": "Allow",
            "Action": [
                "dynamodb:DescribeTable",
                "dynamodb:Query",
                "dynamodb:Scan"
            ],
            "Resource": "arn:aws:dynamodb:us-west-2:account-id:table/Books"
        }
    ]
}
```

 The policy has one statement that grants permissions for three DynamoDB actions \(`dynamodb:DescribeTable`, `dynamodb:Query` and `dynamodb:Scan`\) on a table in the `us-west-2` region, which is owned by the AWS account specified by `account-id`\. The *Amazon Resource Name \(ARN\)* in the `Resource` value specifies the table to which the permissions apply\.

## Permissions Required to Use the AWS Glue Console<a name="console-permissions"></a>

For a user to work with the AWS Glue console, that user must have a minimum set of permissions that allows the user to work with the AWS Glue resources for their AWS account\. In addition to these AWS Glue permissions, the console requires permissions from the following services:
+ Amazon CloudWatch Logs permissions to display logs\.
+ AWS Identity and Access Management permissions to list and pass roles\.
+ AWS CloudFormation permissions to work with stacks\.
+ Amazon Elastic Compute Cloud permissions to list VPCs, subnets, security groups, instances, and other objects\.
+ Amazon Simple Storage Service permissions to list buckets and objects\. Also permission to retrieve and save scripts\.
+ Amazon Redshift permissions to work with clusters\.
+ Amazon Relational Database Service permissions to list instances\.

For more information on the permissions that your users require to view and work with the AWS Glue console, see [Step 3: Attach a Policy to IAM Users That Access AWS Glue](attach-policy-iam-user.md)\.

If you create an IAM policy that is more restrictive than the minimum required permissions, the console won't function as intended for users with that IAM policy\. To ensure that those users can still use the AWS Glue console, also attach the `AWSGlueConsoleFullAccess` managed policy to  the user, as described in [AWS Managed \(Predefined\) Policies for AWS Glue](#access-policy-examples-aws-managed)\.

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the AWS Glue API\.

## AWS Managed \(Predefined\) Policies for AWS Glue<a name="access-policy-examples-aws-managed"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS managed policies grant necessary permissions for common use cases so that you can avoid having to investigate what permissions are needed\. For more information, see [AWS Managed Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\. 

The following AWS managed policies, which you can attach to users in your account, are specific to AWS Glue and are grouped by use case scenario:
+ **AWSGlueConsoleFullAccess** – Grants full access to AWS Glue resources when using the AWS Management Console\. If you follow the naming convention for resources specified in this policy, users have full console capabilities\. This policy is typically attached to users of the AWS Glue console\.
+ **AWSGlueServiceRole** – Grants access to resources that various AWS Glue processes require to run on your behalf\. These resources include AWS Glue, Amazon S3, IAM, CloudWatch Logs, and Amazon EC2\. If you follow the naming convention for resources specified in this policy, AWS Glue processes have the required permissions\. This policy is typically attached to roles specified when defining crawlers, jobs, and development endpoints\.
+ **AWSGlueServiceNotebookRole** – Grants access to resources required when creating a notebook server\. These resources include AWS Glue, Amazon S3, and Amazon EC2\. If you follow the naming convention for resources specified in this policy, AWS Glue processes have the required permissions\. This policy is typically attached to roles specified when creating a notebook server on a development endpoint\.

**Note**  
You can review these permissions policies by signing in to the IAM console and searching for specific policies there\.

You can also create your own custom IAM policies to allow permissions for AWS Glue actions and resources\. You can attach these custom policies to the IAM users or groups that require those permissions\. 