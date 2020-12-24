# Encrypting Connection Passwords<a name="encrypt-connection-passwords"></a>

You can retrieve connection passwords in the AWS Glue Data Catalog by using the `GetConnection` and `GetConnections` API operations\. These passwords are stored in the Data Catalog connection and are used when AWS Glue connects to a Java Database Connectivity \(JDBC\) data store\. When the connection was created or updated, an option in the Data Catalog settings determined whether the password was encrypted, and if so, what AWS Key Management Service \(AWS KMS\) key was specified\.

On the AWS Glue console, you can enable this option on the **Data catalog settings** page\.

**To encrypt connection passwords**

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. Choose **Settings** in the navigation pane\. 

1. On the **Data catalog settings** page, select **Encrypt connection passwords**, and choose an AWS KMS key\.
**Important**  
AWS Glue supports only symmetric customer master keys \(CMKs\)\. The **AWS KMS key** list displays only symmetric keys\. However, if you select **Choose a KMS key ARN**, the console lets you enter an ARN for any key type\. Ensure that you enter only ARNs for symmetric keys\.

   For more information, see [Working with Data Catalog Settings on the AWS Glue Console](console-data-catalog-settings.md)\.