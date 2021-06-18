# Step 5: Create an IAM Role for Notebook Servers<a name="create-an-iam-role-notebook"></a>

If you plan to use notebooks with development endpoints, you need to grant the IAM role permissions\. You provide those permissions by using AWS Identity and Access Management IAM, through an IAM role\.

**Note**  
When you create an IAM role using the IAM console, the console creates an instance profile automatically and gives it the same name as the role to which it corresponds\.

**To create an IAM role for notebooks**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the left navigation pane, choose **Roles**\.

1. Choose **Create role**\.

1. For role type, choose **AWS Service**, find and choose **EC2**, and choose the **EC2** use case, then choose **Next: Permissions**\.

1. On the **Attach permissions policy** page, choose the policies that contain the required permissions; for example, **AWSGlueServiceNotebookRole** for general AWS Glue permissions and the AWS managed policy **AmazonS3FullAccess** for access to Amazon S3 resources\. Then choose **Next: Review**\.
**Note**  
Ensure that one of the policies in this role grants permissions to your Amazon S3 sources and targets\. Also confirm that your policy allows full access to the location where you store your notebook when you create a notebook server\. You might want to provide your own policy for access to specific Amazon S3 resources\. For more information about creating an Amazon S3 policy for your resources, see [Specifying Resources in a Policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/s3-arn-format.html)\.  
If you plan to access Amazon S3 sources and targets that are encrypted with SSE\-KMS, attach a policy that allows notebooks to decrypt the data\. For more information, see [Protecting Data Using Server\-Side Encryption with AWS KMS\-Managed Keys \(SSE\-KMS\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html)\.   
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

1. For **Role name**, enter a name for your role\. Create the role with the name prefixed with the string **AWSGlueServiceNotebookRole** to allow the role to be passed from console users to the notebook server\. AWS Glue provided policies expect IAM service roles to begin with **AWSGlueServiceNotebookRole**\. Otherwise you must add a policy to your users to allow the `iam:PassRole` permission for IAM roles to match your naming convention\. For example, enter **AWSGlueServiceNotebookRoleDefault**\.  Then choose **Create role**\. 