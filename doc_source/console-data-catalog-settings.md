# Working with Data Catalog Settings on the AWS Glue Console<a name="console-data-catalog-settings"></a>

The Data Catalog settings page contains options to set properties for the Data Catalog in your account\. 

To change the fine\-grained access control of the Data Catalog:

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1.  Choose **Settings**, and then in the **Permissions** editor, add the policy statement to control fine\-grained access control of the Data Catalog for your account\. Only one policy at a time can be attached to a Data Catalog\.

1. Choose **Save** to update your Data Catalog with any changes you made\.

You can also use AWS Glue API operations to put, get, and delete resouce policies\. For more information, see [Security APIs in AWS Glue](aws-glue-api-jobs-security.md)\.

The **Settings** page displays the following fields:

**Metadata encryption**  
Select this check box to encrypt the metadata in your Data Catalog\. Metadata is encrypted at rest using the AWS Key Management Service \(AWS KMS\) key that you specify\. For more information, see [Encrypting Your Data Catalog](encrypt-glue-data-catalog.md)\.

**Permissions**  
Add a resource policy to define fine\-grained access control of the Data Catalog\. You can paste a JSON resouce policy into this control\. For more information, see [Resource Policies](glue-resource-policies.md)\.