# Setting Up Encryption in AWS Glue<a name="set-up-encryption"></a>

The following example workflow highlights the options to configure when you use encryption with AWS Glue\. The example demonstrates the use of specific AWS Key Management Service \(AWS KMS\) keys, but you might choose other settings based on your particular needs\. This workflow highlights only the options that pertain to encryption when setting up AWS Glue\.  

1. If the user of the AWS Glue console doesn't use a permissions policy that allows all AWS Glue API operations \(for example, `"glue:*"`\), confirm that the following actions are allowed:
   + `"glue:GetDataCatalogEncryptionSettings"`
   + `"glue:PutDataCatalogEncryptionSettings"`
   + `"glue:CreateSecurityConfiguration"`
   + `"glue:GetSecurityConfiguration"`
   + `"glue:GetSecurityConfigurations"`
   + `"glue:DeleteSecurityConfiguration"`

1. Any client that accesses or writes to an encrypted catalog—that is, any console user, crawler, job, or development endpoint—needs the following permissions:

   ```
   {
    "Version": "2012-10-17",
     "Statement": {
        "Effect": "Allow",
        "Action": [
              "kms:GenerateDataKey",
              "kms:Decrypt",  
              "kms:Encrypt"
         ],
        "Resource": "(key-arns-used-for-data-catalog)"
      }
   }
   ```

1. Any user or role that accesses an encrypted connection password needs the following permissions:

   ```
   {
    "Version": "2012-10-17",        
     "Statement": {
        "Effect": "Allow",
        "Action": [
              "kms:Decrypt"
             ],
        "Resource": "(key-arns-used-for-password-encryption)"
             }
   }
   ```

1. The role of any extract, transform, and load \(ETL\) job that writes encrypted data to Amazon S3 needs the following permissions:

   ```
   {
    "Version": "2012-10-17",
     "Statement": {
        "Effect": "Allow",
        "Action": [
              "kms:Decrypt",  
              "kms:Encrypt",
              "kms:GenerateDataKey"
         ],
        "Resource": "(key-arns-used-for-s3)"
      }
   }
   ```

1. Any ETL job or crawler that writes encrypted Amazon CloudWatch Logs requires the following permissions in the key policy \(not IAM policy\):

   ```
   {
    	"Effect": "Allow",
    	"Principal": {
    		"Service": "logs.region.amazonaws.com"
    	},
    	"Action": [
    		"kms:Encrypt*",
    		"kms:Decrypt*",
    		"kms:ReEncrypt*",
    		"kms:GenerateDataKey*",
    		"kms:Describe*"
    	],
    	"Resource": "arn of key used for ETL/crawler cloudwatch encryption"
    }
   ```

   For more information about key policies, see [Using Key Policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\.

1. Any ETL job that uses an encrypted job bookmark needs the following permissions:

   ```
   {
    "Version": "2012-10-17",
     "Statement": {
        "Effect": "Allow",
        "Action": [
              "kms:Decrypt",  
              "kms:Encrypt"
         ],
        "Resource": "(key-arns-used-for-job-bookmark-encryption)"
      }
   }
   ```

1. On the AWS Glue console, choose **Settings** in the navigation pane\. On the **Data catalog settings** page, encrypt your Data Catalog by selecting the **Metadata encryption** check box\. This option encrypts all the objects in the Data Catalog with the AWS KMS key that you choose\.

   When encryption is enabled, the client that is accessing the Data Catalog must have AWS KMS permissions\. 

1. In the navigation pane, choose **Security configurations**\. A security configuration is a set of security properties that can be used to configure AWS Glue processes\. Then choose **Add security configuration**\. In the configuration, choose any of the following options: 

   1. Select the **S3 encryption** check box\. For **Encryption mode**, choose **SSE\-KMS**\. For the **AWS KMS key**, choose **aws/s3** \(ensure that the user has permission to use this key\)\. This enables data written by the job to Amazon S3 to use the AWS managed AWS Glue AWS KMS key\.

   1. Select the **CloudWatch logs encryption** check box, and choose an AWS managed **AWS KMS key** \(ensure that the user has permission to use this key\)\.  This enables data written by the job to CloudWatch Logs with the AWS managed AWS Glue AWS KMS key\.

   1. Choose **Advanced properties**, and select the **Job bookmark encryption** check box\. For the **AWS KMS key**, choose **aws/glue** \(ensure that the user has permission to use this key\)\. This enables encryption of job bookmarks written to Amazon S3 with the AWS Glue AWS KMS key\.

1. In the navigation pane, choose **Connections**\. Choose **Add connection** to create a connection to the Java Database Connectivity \(JDBC\) data store that is the target of your ETL job\. To enforce that Secure Sockets Layer \(SSL\) encryption is used, select the **Require SSL connection** check box, and test your connection\.

1. In the navigation pane, choose **Jobs**\. Choose **Add job** to create a job that transforms data\. In the job definition, choose the security configuration that you created\. 

1. On the AWS Glue console, run your job on demand\. Verify that any Amazon S3 data written by the job, the CloudWatch Logs written by the job, and the job bookmarks are all encrypted\.