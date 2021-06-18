# Which Data Stores Can I Crawl?<a name="crawler-data-stores"></a>

Crawlers can crawl the following file\-based and table\-based data stores\.


| Access type that crawler uses | Data stores | 
| --- | --- | 
| Native client |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/crawler-data-stores.html)  | 
| JDBC |  Amazon Redshift Within Amazon Relational Database Service \(Amazon RDS\) or external to Amazon RDS: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/crawler-data-stores.html)  | 
| MongoDB client |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/crawler-data-stores.html)  | 

**Note**  
Currently AWS Glue does not support crawlers for data streams\.

For JDBC, MongoDB, and Amazon DocumentDB \(with MongoDB compatibility\) data stores, you must specify an AWS Glue *connection* that the crawler can use to connect to the data store\. For Amazon S3, you can optionally specify a connection of type Network\. A connection is a Data Catalog object that stores connection information, such as credentials, URL, Amazon Virtual Private Cloud information, and more\. For more information, see [Defining Connections in the AWS Glue Data Catalog](populate-add-connection.md)\.

The following are notes about the various data stores\.

**Amazon S3**  
You can choose to crawl a path in your account or in another account\. If all the Amazon S3 files in a folder have the same schema, the crawler creates one table\. Also, if the Amazon S3 object is partitioned, only one metadata table is created and partition information is added to the Data Catalog for that table\.

**Amazon S3 and Amazon DynamoDB**  
Crawlers use an AWS Identity and Access Management \(IAM\) role for permission to access your data stores\. *The role you pass to the crawler must have permission to access Amazon S3 paths and Amazon DynamoDB tables that are crawled*\.

**Amazon DynamoDB**  
When defining a crawler using the AWS Glue console, you specify one DynamoDB table\. If you're using the AWS Glue API, you can specify a list of tables\. You can choose to crawl only a small sample of the data to reduce crawler run times\.

**MongoDB and Amazon DocumentDB \(with MongoDB compatibility\)**  
MongoDB versions 3\.2 and later are supported\. You can choose to crawl only a small sample of the data to reduce crawler run times\.

**Relational database**  
Authentication is with a database user name and password\. Depending on the type of database engine, you can choose which objects are crawled, such as databases, schemas, and tables\.