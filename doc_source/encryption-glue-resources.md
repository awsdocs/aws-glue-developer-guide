# Encryption and Secure Access for AWS Glue<a name="encryption-glue-resources"></a>

You can encrypt metadata objects in your AWS Glue Data Catalog in addition to the data written to Amazon S3 and Amazon CloudWatch Logs by jobs, crawlers, and development endpoints\. You can enable encryption of the entire Data Catalog in your account\. When you create jobs, crawlers, and development endpoints in AWS Glue, you can provide encryption settings, such as a security configuration, to configure encryption for that process\.

With AWS Glue, you can encrypt data using keys that you manage with AWS Key Management Service \(AWS KMS\)\. With encryption enabled, when you add Data Catalog objects, run crawlers, run jobs, or start development endpoints, AWS KMS keys are used to write data at rest\. In addition, you can configure AWS Glue to only access Java Database Connectivity \(JDBC\) data stores through a trusted Secure Sockets Layer \(SSL\) protocol\. 

In AWS Glue, you control encryption settings in the following places:
+ The settings of your Data Catalog\.
+ The security configurations that you create\.
+ The server\-side encryption \(SSE\-S3\) setting that is passed as a parameter to your AWS Glue ETL \(extract, transform, and load\) job\.

For more information about how to set up encryption, see [Setting Up Encryption in AWS Glue](set-up-encryption.md)\. 


| Region | Code | 
| --- | --- | 
| Region | Code | 
| --- | --- | 
|   Encryption is available in the following AWS Regions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/encryption-glue-resources.html) Encryption is **not** available in the following AWS Regions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/encryption-glue-resources.html)   | 
| US East \(Ohio\) | `us-east-2` | 
| US East \(N\. Virginia\) | `us-east-1` | 
| US West \(Oregon\) | `us-west-2` | 
| Asia Pacific \(Tokyo\) | `ap-northeast-1` | 
| Asia Pacific \(Seoul\) | `ap-northeast-2` | 
| Asia Pacific \(Mumbai\) | `ap-south-1` | 
| Asia Pacific \(Singapore\) | `ap-southeast-1` | 
| Asia Pacific \(Sydney\) | `ap-southeast-2` | 
| EU \(Frankfurt\) | `eu-central-1` | 
| EU \(Ireland\) | `eu-west-1` | 
| EU \(London\) | `eu-west-2` | 
| Canada \(Central\) | `ca-central-1` | 

**Topics**
+ [Encrypting Your Data Catalog](encrypt-glue-data-catalog.md)
+ [Encrypting Data Written by Crawlers, Jobs, and Development Endpoints](encryption-security-configuration.md)