# Step 7: Create an IAM Role for SageMaker Notebooks<a name="create-an-iam-role-sagemaker-notebook"></a>

If you plan to use SageMaker notebooks with development endpoints, you need to grant the IAM role permissions\. You provide those permissions by using AWS Identity and Access Management \(IAM\), through an IAM role\.

**To create an IAM role for SageMaker notebooks**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the left navigation pane, choose **Roles**\.

1. Choose **Create role**\.

1. For role type, choose **AWS Service**, find and choose **SageMaker**, and then choose the **SageMaker \- Execution** use case\. Then choose **Next: Permissions**\.

1. On the **Attach permissions policy** page, choose the policies that contain the required permissions; for example, **AmazonSageMakerFullAccess**\.   Choose **Next: Review**\.

   If you plan to access Amazon S3 sources and targets that are encrypted with SSE\-KMS, attach a policy that allows notebooks to decrypt the data, as shown in the following example\. For more information, see [Protecting Data Using Server\-Side Encryption with AWS KMS\-Managed Keys \(SSE\-KMS\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html)\. 

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

1. For **Role name**, enter a name for your role\. To allow the role to be passed from console users to SageMaker, use a name that is prefixed with the string **AWSGlueServiceSageMakerNotebookRole**\. AWS Glue provided policies expect IAM roles to begin with **AWSGlueServiceSageMakerNotebookRole**\. Otherwise you must add a policy to your users to allow the `iam:PassRole` permission for IAM roles to match your naming convention\. 

   For example, enter **AWSGlueServiceSageMakerNotebookRole\-Default**, and then choose **Create role**\. 

1. After you create the role, attach the policy that allows additional permissions required to create SageMaker notebooks from AWS Glue\.

   Open the role that you just created, **AWSGlueServiceSageMakerNotebookRole\-Default**, and choose **Attach policies**\. Attach the policy that you created named **AWSGlueSageMakerNotebook** to the role\. 