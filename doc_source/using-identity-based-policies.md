# Identity\-Based Policies \(IAM Policies\) for Access Control<a name="using-identity-based-policies"></a>

Identity\-based policies are attached to an IAM identity \(user, group, role, or service\)\. This type of policy grants permissions for that IAM identity to access specified resources\.

AWS Glue supports identity\-based policies \(IAM policies\) for all AWS Glue operations\. By attaching a policy to a user or a group in your account, you can grant them permissions to create, access, or modify an AWS Glue resource, such as a table in the AWS Glue Data Catalog\.

By attaching a policy to an IAM role, you can grant cross\-account access permissions to IAM identities in other AWS accounts\. For more information, see [Granting Cross\-Account Access](cross-account-access.md)\.

The following is an example identity\-based policy that grants permissions for AWS Glue actions \(`glue:GetTable`, `GetTables`, `GetDatabase`, and `GetDatabases`\)\. The wildcard character \(`*`\) in the `Resource` value means that you are granting permission to these actions to obtain names and details of all the tables and databases in the Data Catalog\. If the user also has access to other catalogs through a resource policy, then it is given access to these resources too\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "GetTables",
      "Effect": "Allow",
      "Action": [
        "glue:GetTable",
        "glue:GetTables",
        "glue:GetDatabase",
        "glue:GetDataBases"          
      ],
      "Resource": "*"
    }
  ]
}
```

Here is another example, targeting the `us-west-2` Region and using a placeholder for the specific AWS account number\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "GetTablesActionOnBooks",
      "Effect": "Allow",
      "Action": [
        "glue:GetTable",
        "glue:GetTables"     
      ],
      "Resource": [
        "arn:aws:glue:us-west-2:123456789012:catalog",      
        "arn:aws:glue:us-west-2:123456789012:database/db1",
        "arn:aws:glue:us-west-2:123456789012:table/db1/books"
      ]
    }
  ]
}
```

This policy grants read\-only permission to a table named `books` in the database named `db1`\. Notice that to grant `Get` permission to a table that permission to the catalog and database resources is also required\.  

To deny access to a table, requires that you create a policy to deny a user access to the table, or its parent database or catalog\. This allows you to easily deny access to a specific resource that cannot be circumvented with a subsequent allow permission\. For example, if you deny access to table `books` in database `db1`, then if you grant access to database `db1`, access to table `books` is still denied\. The following is an example identity\-based policy that denies permissions for AWS Glue actions \(`glue:GetTables` and `GetTable`\) to database `db1` and all of the tables within it\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyGetTablesToDb1",
      "Effect": "Deny",
      "Action": [
        "glue:GetTables",
        "glue:GetTable"        
      ],
      "Resource": [      
        "arn:aws:glue:us-west-2:123456789012:database/db1"
      ]
    }
  ]
}
```

For more policy examples, see [Identity\-Based Policy Examples](glue-policy-examples-iam.md)\.

## Identity\-Based Policies \(IAM Policies\) with Tags<a name="glue-identity-based-policy-tags"></a>

You can also control access to certain types of AWS Glue resources using AWS tags\. For more information about tags in AWS Glue, see [AWS Tags](monitor-tags.md)\. 

 You can use the `Condition` element along with the `glue:resourceTag` context key in an IAM user policy to allow or deny access based on keys associated with crawlers, jobs, triggers, and development endpoints\. For example:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
         "Effect": "Allow",
         "Action": "glue:*",
         "Resource": "*",
         "Condition": {
             "StringEquals": {
                "glue:resourceTag/Name": "Tom"
              }
          }
         }
    ]
}
```

**Important**  
The condition context keys apply only to those AWS Glue API actions on crawlers, jobs, triggers, and development endpoints\. For more information about which APIs are affected, see [AWS Glue API Permissions: Actions and Resources Reference](api-permissions-reference.md)\.

For information about how to control access using tags, see [AWS Glue Identity\-Based \(IAM\) Access Control Policy with Tags Examples](glue-policy-examples-iam-tags.md)\.

## Resource\-Level Permissions Only Apply to Specific AWS Glue Objects<a name="glue-identity-based-policy-limitations"></a>

You can only define fine\-grained control for specific objects in AWS Glue\. Therefore you must write your client's IAM policy so that API operations that allow Amazon Resource Names \(ARNs\) for the `Resource` statement are not mixed with API operations that don't allow ARNs\. For example, the following IAM policy allows API operations for `GetClassifier` and `GetJobRun`\. It defines the `Resource` as `*` because AWS Glue doesn't allow ARNs for classifiers and job runs\. Because ARNs are allowed for specific API operations such as `GetDatabase` and `GetTable`, ARNs can be specified in the second half of the policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "glue:GetClassifier*",
                "glue:GetJobRun*"
            ],
            "Resource": "*"   
        },
        {
            "Effect": "Allow",
            "Action": [
                "glue:Get*"
            ],
            "Resource": [
                "arn:aws:glue:us-east-1:123456789012:catalog",
                "arn:aws:glue:us-east-1:123456789012:database/default",
                "arn:aws:glue:us-east-1:123456789012:table/default/e*1*",
                "arn:aws:glue:us-east-1:123456789012:connection/connection2"
            ]
        }
    ]
}
```

For a list of AWS Glue objects that allow ARNs, see [Resource ARNs](glue-specifying-resource-arns.md)\. 

## Permissions Required to Use the AWS Glue Console<a name="console-permissions"></a>

For a user to work with the AWS Glue console, that user must have a minimum set of permissions that allows them to work with the AWS Glue resources for their AWS account\. In addition to these AWS Glue permissions, the console requires permissions from the following services:
+ Amazon CloudWatch Logs permissions to display logs\.
+ AWS Identity and Access Management \(IAM\) permissions to list and pass roles\.
+ AWS CloudFormation permissions to work with stacks\.
+ Amazon Elastic Compute Cloud \(Amazon EC2\) permissions to list VPCs, subnets, security groups, instances, and other objects\.
+ Amazon Simple Storage Service \(Amazon S3\) permissions to list buckets and objects, and to retrieve and save scripts\.
+ Amazon Redshift permissions to work with clusters\.
+ Amazon Relational Database Service \(Amazon RDS\) permissions to list instances\.

For more information about the permissions that users require to view and work with the AWS Glue console, see [Step 3: Attach a Policy to IAM Users That Access AWS Glue](attach-policy-iam-user.md)\.

If you create an IAM policy that is more restrictive than the minimum required permissions, the console won't function as intended for users with that IAM policy\. To ensure that those users can still use the AWS Glue console, also attach the `AWSGlueConsoleFullAccess` managed policy to the user, as described in [AWS Managed \(Predefined\) Policies for AWS Glue](#access-policy-examples-aws-managed)\.

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the AWS Glue API\.

## AWS Managed \(Predefined\) Policies for AWS Glue<a name="access-policy-examples-aws-managed"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS managed policies grant necessary permissions for common use cases so that you can avoid having to investigate what permissions are needed\. For more information, see [AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\. 

The following AWS managed policies, which you can attach to users in your account, are specific to AWS Glue and are grouped by use case scenario:
+ **AWSGlueConsoleFullAccess** – Grants full access to AWS Glue resources when using the AWS Management Console\. If you follow the naming convention for resources specified in this policy, users have full console capabilities\. This policy is typically attached to users of the AWS Glue console\.
+ **AWSGlueServiceRole** – Grants access to resources that various AWS Glue processes require to run on your behalf\. These resources include AWS Glue, Amazon S3, IAM, CloudWatch Logs, and Amazon EC2\. If you follow the naming convention for resources specified in this policy, AWS Glue processes have the required permissions\. This policy is typically attached to roles specified when defining crawlers, jobs, and development endpoints\.
+ **AWSGlueServiceNotebookRole** – Grants access to resources required when creating a notebook server\. These resources include AWS Glue, Amazon S3, and Amazon EC2\. If you follow the naming convention for resources specified in this policy, AWS Glue processes have the required permissions\. This policy is typically attached to roles specified when creating a notebook server on a development endpoint\.
+ **AWSGlueConsoleSageMakerNotebookFullAccess** – Grants full access to AWS Glue and Amazon SageMaker resources when using the AWS Management Console\. If you follow the naming convention for resources specified in this policy, users have full console capabilities\. This policy is typically attached to users of the AWS Glue console who manage Amazon SageMaker notebooks\.

**Note**  
You can review these permissions policies by signing in to the IAM console and searching for specific policies there\.

You can also create your own custom IAM policies to allow permissions for AWS Glue actions and resources\. You can attach these custom policies to the IAM users or groups that require those permissions\. 