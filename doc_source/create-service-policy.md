# Step 1: Create an IAM Policy for the AWS Glue Service<a name="create-service-policy"></a>

For any operation that accesses data on another AWS resource, such as accessing your objects in Amazon S3, AWS Glue needs permission to access the resource on your behalf\. You provide those permissions by using AWS Identity and Access Management \(IAM\)\. 

**Note**  
You can skip this step if you use the AWS managed policy **AWSGlueServiceRole**\.

In this step, you create a policy that is similar to `AWSGlueServiceRole`\. You can find the most current version of `AWSGlueServiceRole` on the IAM console\.

**To create an IAM policy for AWS Glue**

This policy grants permission for some Amazon S3 actions to manage resources in your account that are needed by AWS Glue when it assumes the role using this policy\. Some of the resources that are specified in this policy refer to default names that are used by AWS Glue for Amazon S3 buckets, Amazon S3 ETL scripts, CloudWatch Logs, and Amazon EC2 resources\. For simplicity, AWS Glue writes some Amazon S3 objects into buckets in your account prefixed with `aws-glue-*` by default\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the left navigation pane, choose **Policies**\.

1. Choose **Create Policy**\.

1. On the **Create Policy** screen, navigate to a tab to edit JSON\. Create a policy document with the following JSON statements, and then choose **Review policy**\.
**Note**  
Add any permissions needed for Amazon S3 resources\. You might want to scope the resources section of your access policy to only those resources that are required\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "glue:*",
                   "s3:GetBucketLocation",
                   "s3:ListBucket",
                   "s3:ListAllMyBuckets",
                   "s3:GetBucketAcl",
                   "ec2:DescribeVpcEndpoints",
                   "ec2:DescribeRouteTables",
                   "ec2:CreateNetworkInterface",
                   "ec2:DeleteNetworkInterface",				
                   "ec2:DescribeNetworkInterfaces",
                   "ec2:DescribeSecurityGroups",
                   "ec2:DescribeSubnets",
                   "ec2:DescribeVpcAttribute",
                   "iam:ListRolePolicies",
                   "iam:GetRole",
                   "iam:GetRolePolicy",
                   "cloudwatch:PutMetricData"                
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
                   "s3:GetObject",
                   "s3:PutObject",
                   "s3:DeleteObject"				
               ],
               "Resource": [
                   "arn:aws:s3:::aws-glue-*/*",
                   "arn:aws:s3:::*/*aws-glue-*/*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "s3:GetObject"
               ],
               "Resource": [
                   "arn:aws:s3:::crawler-public*",
                   "arn:aws:s3:::aws-glue-*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "logs:CreateLogGroup",
                   "logs:CreateLogStream",
                   "logs:PutLogEvents",
                   "logs:AssociateKmsKey"                
               ],
               "Resource": [
                   "arn:aws:logs:*:*:/aws-glue/*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "ec2:CreateTags",
                   "ec2:DeleteTags"
               ],
               "Condition": {
                   "ForAllValues:StringEquals": {
                       "aws:TagKeys": [
                           "aws-glue-service-resource"
                       ]
                   }
               },
               "Resource": [
                   "arn:aws:ec2:*:*:network-interface/*",
                   "arn:aws:ec2:*:*:security-group/*",
                   "arn:aws:ec2:*:*:instance/*"
               ]
           }
       ]
   }
   ```

   The following table describes the permissions granted by this policy\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/create-service-policy.html)

1. On the **Review Policy** screen, enter your **Policy Name**, for example **GlueServiceRolePolicy**\. Enter an optional description, and when you're satisfied with the policy, choose **Create policy**\.