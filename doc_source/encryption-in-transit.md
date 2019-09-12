# Encryption in Transit<a name="encryption-in-transit"></a>

AWS provides Secure Sockets Layer \(SSL\) encryption for data in motion\. You can configure encryption settings for crawlers, ETL jobs, and development endpoints using [security configurations](https://docs.aws.amazon.com/glue/latest/dg/console-security-configurations.html) in AWS Glue\. You can enable AWS Glue Data Catalog encryption via the settings for the Data Catalog\.

As of September 4, 2018, AWS KMS \(*bring your own key* and *server\-side encryption*\) for AWS Glue ETL and the AWS Glue Data Catalog is supported\.