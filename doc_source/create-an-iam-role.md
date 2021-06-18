# Step 2: Create an IAM Role for AWS Glue<a name="create-an-iam-role"></a>

You need to grant your IAM role permissions that AWS Glue can assume when calling other services on your behalf\. This includes access to Amazon S3 for any sources, targets, scripts, and temporary directories that you use with AWS Glue\. Permission is needed by crawlers, jobs, and development endpoints\.

You provide those permissions by using AWS Identity and Access Management \(IAM\)\. Add a policy to the IAM role that you pass to AWS Glue\.

****To create an IAM role for ** AWS Glue**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the left navigation pane, choose **Roles**\.

1. Choose **Create role**\.

1. For role type, choose **AWS Service**, find and choose **Glue**, and choose **Next: Permissions**\.

1. On the **Attach permissions policy** page, choose the policies that contain the required permissions; for example, the AWS managed policy **AWSGlueServiceRole** for general AWS Glue permissions and the AWS managed policy **AmazonS3FullAccess** for access to Amazon S3 resources\. Then choose **Next: Review**\.
**Note**  
Ensure that one of the policies in this role grants permissions to your Amazon S3 sources and targets\. You might want to provide your own policy for access to specific Amazon S3 resources\. Data sources require `s3:ListBucket` and `s3:GetObject` permissions\. Data targets require `s3:ListBucket`, `s3:PutObject`, and `s3:DeleteObject` permissions\. For more information about creating an Amazon S3 policy for your resources, see [Specifying Resources in a Policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/s3-arn-format.html)\. For an example Amazon S3 policy, see [ Writing IAM Policies: How to Grant Access to an Amazon S3 Bucket](https://aws.amazon.com/blogs/security/writing-iam-policies-how-to-grant-access-to-an-amazon-s3-bucket/)\.   
If you plan to access Amazon S3 sources and targets that are encrypted with SSE\-KMS, attach a policy that allows AWS Glue crawlers, jobs, and development endpoints to decrypt the data\. For more information, see [Protecting Data Using Server\-Side Encryption with AWS KMS\-Managed Keys \(SSE\-KMS\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html)\.   
The following is an example\.  

   ```
   {  
      "Version":"2012-10-17",
      "Statement":[  
         {  
            "Effect":"Allow",
            "Action":[  
               "kms:Decrypt"
            ],
            "Resource":[  
               "arn:aws:kms:*:account-id-without-hyphens:key/key-id"
            ]
         }
      ]
   }
   ```

1. For **Role name**, enter a name for your role; for example, **AWSGlueServiceRoleDefault**\. Create the role with the name prefixed with the string **AWSGlueServiceRole** to allow the role to be passed from console users to the service\. AWS Glue provided policies expect IAM service roles to begin with **AWSGlueServiceRole**\. Otherwise, you must add a policy to allow your users the `iam:PassRole` permission for IAM roles to match your naming convention\.   Choose **Create Role**\.