# Defining Crawlers<a name="add-crawler"></a>

You can use a crawler to populate the AWS Glue Data Catalog with tables\. This is the primary method used by most AWS Glue users\. A crawler can crawl multiple data stores in a single run\. Upon completion, the crawler creates or updates one or more tables in your Data Catalog\. Extract, transform, and load \(ETL\) jobs that you define in AWS Glue use these Data Catalog tables as sources and targets\. The ETL job reads from and writes to the data stores that are specified in the source and target Data Catalog tables\.

For more information about using the AWS Glue console to add a crawler, see [Working with Crawlers on the AWS Glue Console](console-crawlers.md)\.

## Which Data Stores Can I Crawl?<a name="crawler-data-stores"></a>

Crawlers can crawl both file\-based and table\-based data stores\.

Crawlers can crawl the following data stores through their respective native interfaces:
+ Amazon Simple Storage Service \(Amazon S3\)
+ Amazon DynamoDB

Crawlers can crawl the following data stores through a JDBC connection:
+ Amazon Redshift
+ Amazon Relational Database Service \(Amazon RDS\)
  + Amazon Aurora
  + MariaDB
  + Microsoft SQL Server
  + MySQL
  + Oracle
  + PostgreSQL
+ Publicly accessible databases
  + Aurora
  + MariaDB
  + SQL Server
  + MySQL
  + Oracle
  + PostgreSQL

For Amazon S3 and Amazon DynamoDB, crawlers use an AWS Identity and Access Management \(IAM\) role for permission to access your data stores\. *The role you pass to the crawler must have permission to access Amazon S3 paths and Amazon DynamoDB tables that are crawled*\. For JDBC connections, crawlers use user name and password credentials\. For more information, see [Adding a Connection to Your Data Store](populate-add-connection.md)\.

When you define an Amazon S3 data store to crawl, you can choose whether to crawl a path in your account or another account\. The output of the crawler is one or more metadata tables defined in the AWS Glue Data Catalog\. A table is created for one or more files found in your data store\. If all the Amazon S3 files in a folder have the same schema, the crawler creates one table\. Also, if the Amazon S3 object is partitioned, only one metadata table is created\.  

If the data store that is being crawled is a relational database, the output is also a set of metadata tables defined in the AWS Glue Data Catalog\. When you crawl a relational database, you must provide authorization credentials for a connection to read objects in the database engine\. Depending on the type of database engine, you can choose which objects are crawled, such as databases, schemas, and tables\.  

If the data store that is being crawled is one or more Amazon DynamoDB tables, the output is one or more metadata tables in the AWS Glue Data Catalog\. When defining a crawler using the AWS Glue console, you specify a DynamoDB table\. If you're using the AWS Glue API, you specify a list of tables\.

## What Happens When a Crawler Runs?<a name="crawler-running"></a>

When a crawler runs, it takes the following actions to interrogate a data store:
+ **Classifies data to determine the format, schema, and associated properties of the raw data** – You can configure the results of classification by creating a custom classifier\.
+ **Groups data into tables or partitions ** – Data is grouped based on crawler heuristics\.
+ **Writes metadata to the Data Catalog ** – You can configure how the crawler adds, updates, and deletes tables and partitions\.

The metadata tables that a crawler creates are contained in a database when you define a crawler\. If your crawler does not define a database, your tables are placed in the default database\. In addition, each table has a classification column that is filled in by the classifier that first successfully recognized the data store\.

If the file that is crawled is compressed, the crawler must download it to process it\. When a crawler runs, it interrogates files to determine their format and compression type and writes these properties into the Data Catalog\. Some file formats \(for example, Apache Parquet\) enable you to compress parts of the file as it is written\. For these files, the compressed data is an internal component of the file, and AWS Glue does not populate the `compressionType` property when it writes tables into the Data Catalog\. In contrast, if an *entire file* is compressed by a compression algorithm \(for example, gzip\), then the `compressionType` property is populated when tables are written into the Data Catalog\. 

The crawler generates the names for the tables that it creates\. The names of the tables that are stored in the AWS Glue Data Catalog follow these rules:
+ Only alphanumeric characters and underscore \(`_`\) are allowed\.
+ Any custom prefix cannot be longer than 64 characters\.
+ The maximum length of the name cannot be longer than 128 characters\. The crawler truncates generated names to fit within the limit\.
+ If duplicate table names are encountered, the crawler adds a hash string suffix to the name\.

If your crawler runs more than once, perhaps on a schedule, it looks for new or changed files or tables in your data store\. The output of the crawler includes new tables and partitions found since a previous run\.

### How Does a Crawler Determine When to Create Partitions?<a name="crawler-s3-folder-table-partition"></a>

When an AWS Glue crawler scans Amazon S3 and detects multiple folders in a bucket, it determines the root of a table in the folder structure and which folders are partitions of a table\. The name of the table is based on the Amazon S3 prefix or folder name\. You provide an **Include path** that points to the folder level to crawl\. When the majority of schemas at a folder level are similar, the crawler creates partitions of a table instead of two separate tables\. To influence the crawler to create separate tables, add each table's root folder as a separate data store when you define the crawler\.

For example, with the following Amazon S3 structure:

```
s3://bucket01/folder1/table1/partition1/file.txt
s3://bucket01/folder1/table1/partition2/file.txt
s3://bucket01/folder1/table1/partition3/file.txt
s3://bucket01/folder1/table2/partition4/file.txt
s3://bucket01/folder1/table2/partition5/file.txt
```

If the schemas for `table1` and `table2` are similar, and a single data store is defined in the crawler with **Include path** `s3://bucket01/folder1/`, the crawler creates a single table with two partition columns\. One partition column contains `table1` and `table2`, and a second partition column contains `partition1` through `partition5`\. To create two separate tables, define the crawler with two data stores\. In this example, define the first **Include path** as `s3://bucket01/folder1/table1/` and the second as `s3://bucket01/folder1/table2`\.

**Note**  
In Amazon Athena, each table corresponds to an Amazon S3 prefix with all the objects in it\. If objects have different schemas, Athena does not recognize different objects within the same prefix as separate tables\. This can happen if a crawler creates multiple tables from the same Amazon S3 prefix\. This might lead to queries in Athena that return zero results\. For Athena to properly recognize and query tables, create the crawler with a separate **Include path** for each different table schema in the Amazon S3 folder structure\. For more information, see [Best Practices When Using Athena with AWS Glue](https://docs.aws.amazon.com/athena/latest/ug/glue-best-practices.html) and this [AWS Knowledge Center article](https://aws.amazon.com/premiumsupport/knowledge-center/athena-empty-results/)\.