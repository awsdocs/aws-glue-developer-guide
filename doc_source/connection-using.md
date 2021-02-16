# AWS Glue Connections<a name="connection-using"></a>

An AWS Glue connection is a Data Catalog object that stores connection information for a particular data store\. Connections store login credentials, URI strings, virtual private cloud \(VPC\) information, and more\. Creating connections in the Data Catalog saves the effort of having to specify all connection details every time you create a crawler or job\. You can use connections for both sources and targets\.

The following connection types are available:
+ JDBC
+ Amazon Relational Database Service \(Amazon RDS\)
+ Amazon Redshift
+ Amazon DocumentDB
+ MongoDB
+ Kafka
+ Network \(designates a connection to a data source within an Amazon Virtual Private Cloud environment \(Amazon VPC\)\)

When you create a crawler or extract, transform, and load \(ETL\) job for any of these data sources, you specify the connection to use\. You can also optionally specify a connection when creating a development endpoint\.

Typically, a connection is not required for Amazon Simple Storage Service \(Amazon S3\) sources or targets that are on the public Internet\. However, to access Amazon S3 from within your virtual private cloud \(VPC\), an Amazon S3 VPC endpoint is required\. For more information, see [Amazon VPC Endpoints for Amazon S3](vpc-endpoints-s3.md)\. 

Additionally, if you want to access Amazon S3 data sources located in your virtual private cloud \(VPC\), you must create a `Network` type connection\. 

In your connection information, you also must consider whether data is accessed through a VPC and then set up network parameters accordingly\.  AWS Glue requires a private IP for JDBC endpoints\. Connections to databases can be over a VPN and AWS Direct Connect because they provide private IP access to on\-premises databases\.

For information about how to connect to on\-premises databases, see [How to access and analyze on\-premises data stores using AWS Glue](http://aws.amazon.com/blogs/big-data/how-to-access-and-analyze-on-premises-data-stores-using-aws-glue/) at the AWS Big Data Blog website\.

##Looks like Glue connection does not support S3 interface endpoint, use S3 gateway endpoints instead
