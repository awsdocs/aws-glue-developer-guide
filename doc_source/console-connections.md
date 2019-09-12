# Working with Connections on the AWS Glue Console<a name="console-connections"></a>

A connection contains the properties that are needed to access your data store\. To see a list of all the connections that you have created, open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/), and choose the **Connections** tab\.

The Connections list displays the following properties about each connection:

**Name**  
When you create a connection, you give it a unique name\.

**Type**  
The data store type and the properties that are required for a successful connection\. AWS Glue uses the JDBC protocol to access several types of data stores\.

**Date created**  
The date and time \(UTC\) that the connection was created\.

**Last updated**  
The date and time \(UTC\) that the connection was last updated\.

**Updated by**  
The user who created or last updated the connection\.

From the **Connections** tab in the AWS Glue console, you can add, edit, and delete connections\. To see more details for a connection, choose the connection name in the list\. Details include the information you defined when you created the connection\.

As a best practice, before you use a data store connection in an ETL job, choose **Test connection**\. AWS Glue uses the parameters in your connection to confirm that it can access your data store and reports back any errors\. Connections are required for Amazon Redshift, Amazon Relational Database Service \(Amazon RDS\), and JDBC data stores\. For more information, see [Connecting to a JDBC Data Store in a VPC](populate-add-connection.md#connection-JDBC-VPC)\. 

**Important**  
Currently, an ETL job can use only one JDBC connection\. If you have multiple data stores in a job, they must be on the same subnet\.

## Adding a JDBC Connection to a Data Store<a name="console-connections-wizard"></a>

To add a connection in the AWS Glue console, choose **Add connection**\. The wizard guides you through adding the properties that are required to create a JDBC connection to a data store\. If you choose Amazon Redshift or Amazon RDS, AWS Glue tries to determine the underlying JDBC properties to create the connection\. 

When you define a connection, values for the following properties are required:

**Connection name**  
Type a unique name for your connection\.

**Connection type**  
Choose either Amazon Redshift, Amazon RDS, or JDBC\.   
+ If you choose Amazon Redshift, choose a **Cluster**, **Database name**, **Username**, and **Password** in your account to create a JDBC connection\.
+ If you choose Amazon RDS, choose an **Instance**, **Database name**, **Username**, and **Password** in your account to create a JDBC connection\. The console also lists the supported database engine types\.

**Require SSL connection**  
Select this option to require AWS Glue to verify that the JDBC database connection is connected over a trusted Secure Socket Layer \(SSL\)\. This option is optional\. If not selected, AWS Glue can ignore failures when it uses SSL to encrypt a connection to a JDBC database\. See the documentation for your database for configuration instructions\. When you select this option, if AWS Glue cannot connect using SSL, the job run, crawler, or ETL statements in a development endpoint fail\.  
This option is validated on the AWS Glue client side\. AWS Glue only connects to JDBC over SSL with certificate and host name validation\. Support is available for:   
+ Oracle
+ Microsoft SQL Server
+ PostgreSQL
+ Amazon Redshift
+ MySQL \(Amazon RDS instances only\)
+ Aurora MySQL \(Amazon RDS instances only\)
+ Aurora Postgres \(Amazon RDS instances only\)
To enable an **Amazon RDS Oracle** data store to use **Require SSL connection**, you need to create and attach an option group to the Oracle instance\.  

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. Add an **Option group** to the Amazon RDS Oracle instance\. For more information about how to add an option group on the Amazon RDS console, see [Creating an Option Group](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithOptionGroups.html#USER_WorkingWithOptionGroups.Create)

1. Add an **Option** to the option group for **SSL**\. The **Port** you specify for SSL is later used when you create an AWS Glue JDBC connection URL for the Amazon RDS Oracle instance\. For more information about how to add an option on the Amazon RDS console, see [Adding an Option to an Option Group](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithOptionGroups.html#USER_WorkingWithOptionGroups.AddOption)\. For more information about the Oracle SSL option, see [Oracle SSL](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.SSL.html)\. 

1. On the AWS Glue console, create a connection to the Amazon RDS Oracle instance\. In the connection definition, select **Require SSL connection**, and when requested, enter the **Port** you used in the Amazon RDS Oracle SSL option\. 

**JDBC URL**  
Type the URL for your JDBC data store\. For most database engines, this field is in the following format\.  
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

  The syntax for Amazon RDS for Oracle can follow the following patterns:
  + `jdbc:oracle:thin://@host:port/service_name`
  + `jdbc:oracle:thin://@host:port:SID`
+ To connect to an Amazon RDS for Microsoft SQL Server data store with an `employee` database:

   `jdbc:sqlserver://xxx-cluster.cluster-xxx.us-east-1.rds.amazonaws.com:1433;databaseName=employee` 

  The syntax for Amazon RDS for SQL Server can follow the following patterns:
  + `jdbc:sqlserver://server_name:port;database=db_name`
  + `jdbc:sqlserver://server_name:port;databaseName=db_name`
+ To connect to a Microsoft SQL Server data store with an `employee` database:

  `jdbc:sqlserver://server_name\instance:1433;databaseName=employee`
**Important**  
When connecting to a Microsoft SQL database, the port number for the server needs to be set to 0 in case the SQL server is running the SQL browser\.

**Username**  
Provide a user name that has permission to access the JDBC data store\.

**Password**  
Type the password for the user name that has access permission to the JDBC data store\.

**Port**  
Type the port used in the JDBC URL to connect to an Amazon RDS Oracle instance\. This field is only shown when **Require SSL connection** is selected for an Amazon RDS Oracle instance\.

**VPC**  
Choose the name of the virtual private cloud \(VPC\) that contains your data store\. The AWS Glue console lists all VPCs for the current region\.

**Subnet**  
Choose the subnet within the VPC that contains your data store\. The AWS Glue console lists all subnets for the data store in your VPC\. 

**Security groups**  
Choose the security groups that are associated with your data store\. AWS Glue requires one or more security groups with an inbound source rule that allows AWS Glue to connect\. The AWS Glue console lists all security groups that are granted inbound access to your VPC\. AWS Glue associates these security groups with the elastic network interface that is attached to your VPC subnet\.