# Encrypting Your Data Catalog<a name="encrypt-glue-data-catalog"></a>

You can enable encryption of your AWS Glue Data Catalog objects in the **Settings** of the Data Catalog on the AWS Glue console\. You can enable or disable encryption settings for the entire Data Catalog\. In the process, you specify an AWS KMS key that is automatically used when objects, such as tables, are written to the Data Catalog\. The encrypted objects include the following:
+ Databases
+ Tables
+ Partitions
+ Table versions
+ Connections
+ User\-defined functions

You can set this behavior using the AWS Management Console or AWS Command Line Interface \(AWS CLI\)\.

**To enable encryption using the console**

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. Choose **Settings** in the navigation pane\. 

1. On the **Data catalog settings** page, select the **Metadata encryption** check box, and choose an AWS KMS key\. 

When encryption is enabled, all future Data Catalog objects are encrypted\. The default key is the AWS Glue AWS KMS key that is created for your account by AWS\. If you clear this setting, objects are no longer encrypted when they are written to the Data Catalog\. Any encrypted objects in the Data Catalog can continue to be accessed with the key\. 

**To enable encryption using the SDK or AWS CLI**
+ Use the `PutDataCatalogEncryptionSettings` API operation\. If no key is specified, the default AWS Glue encryption key for the customer account is used\.


|  | 
| --- |
|   The AWS KMS key must remain available in the AWS KMS key store for any objects that are encrypted with it in the Data Catalog\. If you remove the key, the objects can no longer be decrypted\. You might want this in some scenarios to prevent access to Data Catalog metadata\.   | 

When encryption is enabled, the client that is accessing the Data Catalog must have the following AWS KMS permissions in its policy:
+ `kms:Decrypt`
+ `kms:Encrypt`
+ `kms:GenerateDataKey`

For example, when you define a crawler or a job, the IAM role that you provide in the definition must have these AWS KMS permissions:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "kms:Decrypt",
                "kms:Encrypt",
                "kms:GenerateDataKey"
            ],
            "Resource": "ARN-of-key-used-to-encrypt-data-catalog"
        }
    ]
}
```