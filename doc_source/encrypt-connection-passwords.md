# Encrypting Connection Passwords<a name="encrypt-connection-passwords"></a>

You can retrieve connection passwords in the AWS Glue Data Catalog by using the `GetConnection` and `GetConnections` API operations\. These passwords are stored in the Data Catalog connection and are used when AWS Glue connects to a Java Database Connectivity \(JDBC\) data store\. When the connection was created or updated, an option in the Data Catalog settings determined whether the password was encrypted, and if so, what AWS Key Management Service \(AWS KMS\) key was specified\.

On the AWS Glue console, you can enable this option on the **Data catalog settings** page:

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. Choose **Settings** in the navigation pane\. 

1. On the **Data catalog settings** page, select the **Encrypt connection passwords** check box\.

   For more information, see [Working with Data Catalog Settings on the AWS Glue Console](console-data-catalog-settings.md)\.