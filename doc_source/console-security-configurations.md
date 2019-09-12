# Working with Security Configurations on the AWS Glue Console<a name="console-security-configurations"></a>

A *security configuration* in AWS Glue contains the properties that are needed when you write encrypted data\. You create security configurations on the AWS Glue console to provide the encryption properties that are used by crawlers, jobs, and development endpoints\. 

To see a list of all the security configurations that you have created, open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/) and choose **Security configurations** in the navigation pane\.

The **Security configurations** list displays the following properties about each configuration:

**Name**  
The unique name you provided when you created the configuration\.

**S3 encryption mode**  
If enabled, the Amazon Simple Storage Service \(Amazon S3\) encryption mode such as `SSE-KMS` or `SSE-S3`\.

**CloudWatch logs encryption mode**  
If enabled, the Amazon S3 encryption mode such as `SSE-KMS`\.

**Job bookmark encryption mode**  
If enabled, the Amazon S3 encryption mode such as `CSE-KMS`\.

**Date created**  
The date and time \(UTC\) that the configuration was created\.

You can add or delete configurations in the **Security configurations** section on the console\. To see more details for a configuration, choose the configuration name in the list\. Details include the information that you defined when you created the configuration\.

## Adding a Security Configuration<a name="console-security-configurations-wizard"></a>

To add a security configuration using the AWS Glue console, on the **Security configurations** page, choose **Add security configuration**\. The wizard guides you through setting the required properties\.

To set up encryption of data and metadata with AWS Key Management Service \(AWS KMS\) keys on the AWS Glue console, add a policy to the console user\. This policy must specify the allowed resources as key ARNs that are used to encrypt Amazon S3 data stores, as in the following example:

```
{
"Version": "2012-10-17",
  "Statement": {
  "Effect": "Allow",
  "Action": [
    "kms:GenerateDataKey",
    "kms:Decrypt",    
    "kms:Encrypt"],
  "Resource": "arn:aws:kms:region:account-id:key/key-id"}
}
```

**Important**  
When a security configuration is attached to a crawler or job, the IAM role that is passed must have AWS KMS permissions\. For more information, see [Encrypting Data Written by Crawlers, Jobs, and Development Endpoints](encryption-security-configuration.md)\.

When you define a configuration, you can provide values for the following properties:

**S3 encryption**  
When you are writing Amazon S3 data, you use either server\-side encryption with Amazon S3 managed keys \(SSE\-S3\) or server\-side encryption with AWS KMS managed keys \(SSE\-KMS\)\. This field is optional\. To enable access to Amazon S3, choose an AWS KMS key, or choose **Enter a key ARN** and provide the Amazon Resource Name \(ARN\) for the key\. Enter the ARN in the form `arn:aws:kms:region:account-id:key/key-id`\. You can also provide the ARN as a key alias, such as `arn:aws:kms:region:account-id:alias/alias-name`\. 

**CloudWatch Logs encryption**  
Server\-side \(SSE\-KMS\) encryption is used to encrypt CloudWatch Logs\. This field is optional\. To enable it, choose an AWS KMS key, or choose **Enter a key ARN** and provide the ARN for the key\. Enter the ARN in the form `arn:aws:kms:region:account-id:key/key-id`\. You can also provide the ARN as a key alias, such as `arn:aws:kms:region:account-id:alias/alias-name`\. 

**Job bookmark encryption**  
Client\-side \(CSE\-KMS\) encryption is used to encrypt job bookmarks\. This field is optional\. The bookmark data is encrypted before it is sent to Amazon S3 for storage\. To enable it, choose an AWS KMS key, or choose **Enter a key ARN** and provide the ARN for the key\. Enter the ARN in the form `arn:aws:kms:region:account-id:key/key-id`\. You can also provide the ARN as a key alias, such as `arn:aws:kms:region:account-id:alias/alias-name`\.

For more information, see the following topics in the *Amazon Simple Storage Service Developer Guide*:
+ For information about `SSE-S3`, see [Protecting Data Using Server\-Side Encryption with Amazon S3\-Managed Encryption Keys \(SSE\-S3\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html)\. 
+ For information about `SSE-KMS`, see [Protecting Data Using Server\-Side Encryption with AWS KMS–Managed Keys \(SSE\-KMS\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html)\. 
+ For information about `CSE-KMS`, see [Using an AWS KMS–Managed Customer Master Key \(CMK\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingClientSideEncryption.html#client-side-encryption-kms-managed-master-key-intro)\. 