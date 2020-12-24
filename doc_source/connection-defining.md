# AWS Glue Connection Properties<a name="connection-defining"></a>

When you define a connection on the AWS Glue console, you must provide values for the following properties:

**Connection name**  
Type a unique name for your connection\.

**Connection type**  
Choose **JDBC** or one of the specific connection types\.   
For details about the JDBC connection type, see [AWS Glue JDBC Connection Properties](#connection-properties-jdbc)  
Choose **Network** to connect to a data source within an Amazon Virtual Private Cloud environment \(Amazon VPC\)\)\.  
Depending on the type that you choose, the AWS Glue console displays other required fields\. For example, if you choose **Amazon RDS**, you must then choose the database engine\.

**Require SSL connection**  
When you select this option, AWS Glue must verify that the connection to the data store is connected over a trusted Secure Sockets Layer \(SSL\)\.  
For more information, including additional options that are available when you select this option, see [AWS Glue Connection SSL Properties](#connection-properties-SSL)\.

## AWS Glue JDBC Connection Properties<a name="connection-properties-jdbc"></a>

AWS Glue can connect to the following data stores through a JDBC connection:
+ Amazon Redshift
+ Amazon Aurora
+ Microsoft SQL Server
+ MySQL
+ Oracle
+ PostgreSQL

**Important**  
Currently, an ETL job can use JDBC connections within only one subnet\. If you have multiple data stores in a job, they must be on the same subnet\.

The following are additional properties for the JDBC connection type\.

**JDBC URL**  
Type the URL for your JDBC data store\. For most database engines, this field is in the following format\. In this format, replace *protocol*, *host*, *port*, and *db\_name* with your own information\.  
 ` jdbc:protocol://host:port/db_name `   
Depending on the database engine, a different JDBC URL format might be required\. This format can have slightly different use of the colon \(:\) and slash \(/\) or different keywords to specify databases\.   
For JDBC to connect to the data store, a `db_name` in the data store is required\. The `db_name` is used to establish a network connection with the supplied `username` and `password`\. When connected, AWS Glue can access other databases in the data store to run a crawler or run an ETL job\.  
The following JDBC URL examples show the syntax for several database engines\.  
+ To connect to an Amazon Redshift cluster data store with a `dev` database:

   `jdbc:redshift://xxx.us-east-1.redshift.amazonaws.com:8192/dev` 
+ To connect to an Amazon RDS for MySQL data store with an `employee` database:

   `jdbc:mysql://xxx-cluster.cluster-xxx.us-east-1.rds.amazonaws.com:3306/employee` 
+ To connect to an Amazon RDS for PostgreSQL data store with an `employee` database:

   `jdbc:postgresql://xxx-cluster.cluster-xxx.us-east-1.rds.amazonaws.com:5432/employee` 
+ To connect to an Amazon RDS for Oracle data store with an `employee` service name:

   `jdbc:oracle:thin://@xxx-cluster.cluster-xxx.us-east-1.rds.amazonaws.com:1521/employee` 

  The syntax for Amazon RDS for Oracle can follow the following patterns\. In these patterns, replace *host*, *port*, *service\_name*, and *SID* with your own information\.
  + `jdbc:oracle:thin://@host:port/service_name`
  + `jdbc:oracle:thin://@host:port:SID`
+ To connect to an Amazon RDS for Microsoft SQL Server data store with an `employee` database:

   `jdbc:sqlserver://xxx-cluster.cluster-xxx.us-east-1.rds.amazonaws.com:1433;databaseName=employee` 

  The syntax for Amazon RDS for SQL Server can follow the following patterns\. In these patterns, replace *server\_name*, *port*, and *db\_name* with your own information\. 
  + `jdbc:sqlserver://server_name:port;database=db_name`
  + `jdbc:sqlserver://server_name:port;databaseName=db_name`

**Username**  
Provide a user name that has permission to access the JDBC data store\.

**Password**  
Type the password for the user name that has access permission to the JDBC data store\.

**Port**  
Type the port used in the JDBC URL to connect to an Amazon RDS Oracle instance\. This field is only shown when **Require SSL connection** is selected for an Amazon RDS Oracle instance\.

**VPC**  
Choose the name of the virtual private cloud \(VPC\) that contains your data store\. The AWS Glue console lists all VPCs for the current Region\.

**Subnet**  
Choose the subnet within the VPC that contains your data store\. The AWS Glue console lists all subnets for the data store in your VPC\. 

**Security groups**  
Choose the security groups that are associated with your data store\. AWS Glue requires one or more security groups with an inbound source rule that allows AWS Glue to connect\. The AWS Glue console lists all security groups that are granted inbound access to your VPC\. AWS Glue associates these security groups with the elastic network interface that is attached to your VPC subnet\.

## AWS Glue Connection SSL Properties<a name="connection-properties-SSL"></a>

The following are details about the **Require SSL connection** property of AWS Glue connections\.

 If this option is not selected, AWS Glue ignores failures when it uses SSL to encrypt a connection to the data store\. See the documentation for your data store for configuration instructions\. When you select this option, if AWS Glue cannot connect using SSL, the job run, crawler, or ETL statements in a development endpoint fail\.

This option is validated on the AWS Glue client side\. For JDBC connections, AWS Glue only connects over SSL with certificate and host name validation\. SSL connection support is available for: 
+ Oracle Database
+ Microsoft SQL Server
+ PostgreSQL
+ Amazon Redshift
+ MySQL \(Amazon RDS instances only\)
+ Aurora MySQL \(Amazon RDS instances only\)
+ Aurora Postgres \(Amazon RDS instances only\)
+ Kafka, which includes Amazon Managed Streaming for Apache Kafka

**Note**  
To enable an **Amazon RDS Oracle** data store to use **Require SSL connection**, you must create and attach an option group to the Oracle instance\.  
Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.
Add an **Option group** to the Amazon RDS Oracle instance\. For more information about how to add an option group on the Amazon RDS console, see [Creating an Option Group](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithOptionGroups.html#USER_WorkingWithOptionGroups.Create)
Add an **Option** to the option group for **SSL**\. The **Port** you specify for SSL is later used when you create an AWS Glue JDBC connection URL for the Amazon RDS Oracle instance\. For more information about how to add an option on the Amazon RDS console, see [Adding an Option to an Option Group](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithOptionGroups.html#USER_WorkingWithOptionGroups.AddOption) in the *Amazon RDS User Guide*\. For more information about the Oracle SSL option, see [Oracle SSL](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.SSL.html) in the *Amazon RDS User Guide*\. 
On the AWS Glue console, create a connection to the Amazon RDS Oracle instance\. In the connection definition, select **Require SSL connection**\. When requested, enter the **Port** that you used in the Amazon RDS Oracle SSL option\. 

The following additional optional properties are available when **Require SSL connection** is selected for a connection:

**Custom JDBC certificate in S3**  
If you have a certificate that you are currently using for SSL communication with your on\-premises or cloud databases, you can use that certificate for SSL connections to AWS Glue data sources or targets\. Enter an Amazon Simple Storage Service \(Amazon S3\) location that contains a custom root certificate\. AWS Glue uses this certificate to establish an SSL connection to the database\. AWS Glue handles only X\.509 certificates\. The certificate must be DER\-encoded and supplied in base64 encoding PEM format\.  
If this field is left blank, the default certificate is used\.

**Custom JDBC certificate string**  
Enter certificate information specific to your JDBC database\. This string is used for domain matching or distinguished name \(DN\) matching\. For Oracle Database, this string maps to the `SSL_SERVER_CERT_DN` parameter in the security section of the `tnsnames.ora` file\. For Microsoft SQL Server, this string is used as `hostNameInCertificate`\.  
The following is an example for the Oracle Database `SSL_SERVER_CERT_DN` parameter\.  

```
cn=sales,cn=OracleContext,dc=us,dc=example,dc=com
```

**Custom Kafka certificate in S3**  
If you have a certificate that you are currently using for SSL communication with your Kafka data store, you can use that certificate with your AWS Glue connection\. This option is required for Kafka data stores, and optional for Amazon Managed Streaming for Apache Kafka data stores\. Enter an Amazon Simple Storage Service \(Amazon S3\) location that contains a custom root certificate\. AWS Glue uses this certificate to establish an SSL connection to the Kafka data store\. AWS Glue handles only X\.509 certificates\. The certificate must be DER\-encoded and supplied in base64 encoding PEM format\. 

**Skip certificate validation**  
Select the **Skip certificate validation** check box to skip validation of the custom certificate by AWS Glue\. If you choose to validate, AWS Glue validates the signature algorithm and subject public key algorithm for the certificate\. If the certificate fails validation, any ETL job or crawler that uses the connection fails\.  
The only permitted signature algorithms are SHA256withRSA, SHA384withRSA, or SHA512withRSA\. For the subject public key algorithm, the key length must be at least 2048\.