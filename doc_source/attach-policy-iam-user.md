# Step 3: Attach a Policy to IAM Users That Access AWS Glue<a name="attach-policy-iam-user"></a>

Any IAM user that signs in to the AWS Glue console or AWS Command Line Interface \(AWS CLI\) must have permissions to access specific resources\. You provide those permissions by using AWS Identity and Access Management \(IAM\), through policies\.

When you finish this step, your IAM user has the following policies attached: 
+ The AWS managed policy **AWSGlueConsoleFullAccess** or the custom policy **GlueConsoleAccessPolicy**
+ **AWSGlueConsoleSageMakerNotebookFullAccess**
+ **CloudWatchLogsReadOnlyAccess**
+ **AWSCloudFormationReadOnlyAccess**
+ **AmazonAthenaFullAccess**

**To attach an inline policy and embed it in an IAM user**

You can attach an AWS managed policy or an inline policy to an IAM user to access the AWS Glue console\. Some of the resources specified in this policy refer to default names that are used by AWS Glue for Amazon S3 buckets, Amazon S3 ETL scripts, CloudWatch Logs, AWS CloudFormation, and Amazon EC2 resources\. For simplicity, AWS Glue writes some Amazon S3 objects into buckets in your account prefixed with `aws-glue-*` by default\. 
**Note**  
You can skip this step if you use the AWS managed policy **AWSGlueConsoleFullAccess**\.
**Important**  
AWS Glue needs permission to assume a role that is used to perform work on your behalf\. **To accomplish this, you add the `iam:PassRole` permissions to your AWS Glue users\.** This policy grants permission to roles that begin with `AWSGlueServiceRole` for AWS Glue service roles, and `AWSGlueServiceNotebookRole` for roles that are required when you create a notebook server\. You can also create your own policy for `iam:PassRole` permissions that follows your naming convention\.

In this step, you create a policy that is similar to `AWSGlueConsoleFullAccess`\. You can find the most current version of `AWSGlueConsoleFullAccess` on the IAM console\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**\.

1. In the list, choose the name of the user to embed a policy in\.

1. Choose the **Permissions** tab and, if necessary, expand the **Permissions policies** section\.

1. Choose the **Add Inline policy** link\.

1. On the **Create Policy** screen, navigate to a tab to edit JSON\. Create a policy document with the following JSON statements, and then choose **Review policy**\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "glue:*",
                   "redshift:DescribeClusters",
                   "redshift:DescribeClusterSubnetGroups",
                   "iam:ListRoles",
                   "iam:ListRolePolicies",
                   "iam:GetRole",
                   "iam:GetRolePolicy",
                   "iam:ListAttachedRolePolicies",
                   "ec2:DescribeSecurityGroups",
                   "ec2:DescribeSubnets",
                   "ec2:DescribeVpcs",
                   "ec2:DescribeVpcEndpoints",
                   "ec2:DescribeRouteTables",
                   "ec2:DescribeVpcAttribute",
                   "ec2:DescribeKeyPairs",
                   "ec2:DescribeInstances",
                   "rds:DescribeDBInstances",
                   "s3:ListAllMyBuckets",
                   "s3:ListBucket",
                   "s3:GetBucketAcl",
                   "s3:GetBucketLocation",
                   "cloudformation:DescribeStacks",
                   "cloudformation:GetTemplateSummary",
                   "dynamodb:ListTables",
                   "kms:ListAliases",
                   "kms:DescribeKey",
                   "cloudwatch:GetMetricData",
                   "cloudwatch:ListDashboards"                
               ],
               "Resource": [
                   "*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "s3:GetObject",
                   "s3:PutObject"
               ],
               "Resource": [
                   "arn:aws:s3:::aws-glue-*/*",
                   "arn:aws:s3:::*/*aws-glue-*/*",
                   "arn:aws:s3:::aws-glue-*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "tag:GetResources"
               ],
               "Resource": [
                   "*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "s3:CreateBucket"
               ],
               "Resource": [
                   "arn:aws:s3:::aws-glue-*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "logs:GetLogEvents"
               ],
               "Resource": [
                   "arn:aws:logs:*:*:/aws-glue/*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "cloudformation:CreateStack",
                   "cloudformation:DeleteStack"
               ],
               "Resource": "arn:aws:cloudformation:*:*:stack/aws-glue*/*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "ec2:RunInstances"
               ],
               "Resource": [
                   "arn:aws:ec2:*:*:instance/*",
                   "arn:aws:ec2:*:*:key-pair/*",
                   "arn:aws:ec2:*:*:image/*",
                   "arn:aws:ec2:*:*:security-group/*",
                   "arn:aws:ec2:*:*:network-interface/*",
                   "arn:aws:ec2:*:*:subnet/*",
                   "arn:aws:ec2:*:*:volume/*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "ec2:TerminateInstances",
                   "ec2:CreateTags",
                   "ec2:DeleteTags"
               ],
               "Resource": [
                   "arn:aws:ec2:*:*:instance/*"
               ],
               "Condition": {
                   "StringLike": {
                       "ec2:ResourceTag/aws:cloudformation:stack-id": "arn:aws:cloudformation:*:*:stack/aws-glue-*/*"
                   },
                   "StringEquals": {
                       "ec2:ResourceTag/aws:cloudformation:logical-id": "ZeppelinInstance"
                   }
               }
           },
           {
               "Action": [
                   "iam:PassRole"
               ],
               "Effect": "Allow",
               "Resource": "arn:aws:iam::*:role/AWSGlueServiceRole*",
               "Condition": {
                   "StringLike": {
                       "iam:PassedToService": [
                           "glue.amazonaws.com"
                       ]
                   }
               }
           },
           {
               "Action": [
                   "iam:PassRole"
               ],
               "Effect": "Allow",
               "Resource": "arn:aws:iam::*:role/AWSGlueServiceNotebookRole*",
               "Condition": {
                   "StringLike": {
                       "iam:PassedToService": [
                           "ec2.amazonaws.com"
                       ]
                   }
               }
           },
           {
               "Action": [
                   "iam:PassRole"
               ],
               "Effect": "Allow",
               "Resource": [
                   "arn:aws:iam::*:role/service-role/AWSGlueServiceRole*"
               ],
               "Condition": {
                   "StringLike": {
                       "iam:PassedToService": [
                           "glue.amazonaws.com"
                       ]
                   }
               }
           }
       ]
   }
   ```

   The following table describes the permissions granted by this policy\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/attach-policy-iam-user.html)

1. On the **Review policy** screen, enter a name for the policy, for example **GlueConsoleAccessPolicy**\. When you're satisfied with the policy, choose **Create policy**\. Ensure that no errors appear in a red box at the top of the screen\. Correct any that are reported\.
**Note**  
If **Use autoformatting** is selected, the policy is reformatted whenever you open a policy or choose **Validate Policy**\.

**To attach the AWSGlueConsoleFullAccess managed policy**

You can attach the **AWSGlueConsoleFullAccess** policy to provide permissions that are required by the AWS Glue console user\.
**Note**  
You can skip this step if you created your own policy for AWS Glue console access\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**\.

1. In the list of policies, select the check box next to the **AWSGlueConsoleFullAccess**\. You can use the **Filter** menu and the search box to filter the list of policies\.

1. Choose **Policy actions**, and then choose **Attach**\.

1. Choose the user to attach the policy to\. You can use the **Filter** menu and the search box to filter the list of principal entities\. After choosing the user to attach the policy to, choose **Attach policy**\.

**To attach the AWSGlueConsoleSageMakerNotebookFullAccess managed policy**

You can attach the **AWSGlueConsoleSageMakerNotebookFullAccess** policy to a user to manage Amazon SageMaker notebooks created on the AWS Glue console\. In addition to other required AWS Glue console permissions, this policy grants access to resources needed to manage Amazon SageMaker notebooks\. 

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**\. 

1. In the list of policies, select the check box next to the **AWSGlueConsoleSageMakerNotebookFullAccess**\. You can use the **Filter** menu and the search box to filter the list of policies\.

1. Choose **Policy actions**, and then choose **Attach**\.

1. Choose the user to attach the policy to\. You can use the **Filter** menu and the search box to filter the list of principal entities\. After choosing the user to attach the policy to, choose **Attach policy**\.

**To attach the CloudWatchLogsReadOnlyAccess managed policy**

You can attach the **CloudWatchLogsReadOnlyAccess** policy to a user to view the logs created by AWS Glue on the CloudWatch Logs console\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**\. 

1. In the list of policies, select the check box next to the **CloudWatchLogsReadOnlyAccess**\. You can use the **Filter** menu and the search box to filter the list of policies\.

1. Choose **Policy actions**, and then choose **Attach**\.

1. Choose the user to attach the policy to\. You can use the **Filter** menu and the search box to filter the list of principal entities\. After choosing the user to attach the policy to, choose **Attach policy**\.

**To attach the AWSCloudFormationReadOnlyAccess managed policy**

You can attach the **AWSCloudFormationReadOnlyAccess** policy to a user to view the AWS CloudFormation stacks used by AWS Glue on the AWS CloudFormation console\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**\. 

1. In the list of policies, select the check box next to the **AWSCloudFormationReadOnlyAccess**\. You can use the **Filter** menu and the search box to filter the list of policies\.

1. Choose **Policy actions**, and then choose **Attach**\.

1. Choose the user to attach the policy to\. You can use the **Filter** menu and the search box to filter the list of principal entities\. After choosing the user to attach the policy to, choose **Attach policy**\.

**To attach the AmazonAthenaFullAccess managed policy**

You can attach the **AmazonAthenaFullAccess** policy to a user to view Amazon S3 data in the Athena console\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**\. 

1. In the list of policies, select the check box next to the **AmazonAthenaFullAccess**\. You can use the **Filter** menu and the search box to filter the list of policies\.

1. Choose **Policy actions**, and then choose **Attach**\.

1. Choose the user to attach the policy to\. You can use the **Filter** menu and the search box to filter the list of principal entities\. After choosing the user to attach the policy to, choose **Attach policy**\.