# Cataloging Tables with a Crawler<a name="add-crawler"></a>

You can use a crawler to populate the AWS Glue Data Catalog with tables\. This is the primary method used by most AWS Glue users\. You add a crawler within your Data Catalog to traverse your data stores\. The output of the crawler consists of one or more metadata tables that are defined in your Data Catalog\. Extract, transform, and load \(ETL\) jobs that you define in AWS Glue use these metadata tables as sources and targets\.

Your crawler uses an AWS Identity and Access Management \(IAM\) role for permission to access your data stores and the Data Catalog\. **The role you pass to the crawler must have permission to access Amazon S3 paths and Amazon DynamoDB tables that are crawled\.** For more information, see [Working with Crawlers on the AWS Glue Console](console-crawlers.md)\. Some data stores require additional authorization to establish a connection\. For more information, see [Adding a Connection to Your Data Store](populate-add-connection.md)\.

For more information about using the AWS Glue console to add a crawler, see [Working with Crawlers on the AWS Glue Console](console-crawlers.md)\.

## Defining a Crawler in the AWS Glue Data Catalog<a name="define-crawler"></a>

When you define a crawler, you choose one or more classifiers that evaluate the format of your data to infer a schema\. When the crawler runs, the first classifier in your list to successfully recognize your data store is used to create a schema for your table\. You can use built\-in classifiers or define your own\. AWS Glue provides built\-in classifiers to infer schemas from common files with formats that include JSON, CSV, and Apache Avro\. For the current list of built\-in classifiers in AWS Glue, see [Built\-In Classifiers in AWS Glue ](add-classifier.md#classifier-built-in)\.  

## Which Data Stores Can I Crawl?<a name="crawler-data-stores"></a>

A crawler can crawl both file\-based and table\-based data stores\. Crawlers can crawl the following data stores:
+ Amazon Simple Storage Service \(Amazon S3\)
+ Amazon Redshift
+ Amazon Relational Database Service \(Amazon RDS\)
  + Amazon Aurora
  + MariaDB
  + Microsoft SQL Server
  + MySQL
  + Oracle
  + PostgreSQL
+ Amazon DynamoDB
+ Publicly accessible databases
  + Aurora
  + MariaDB
  + SQL Server
  + MySQL
  + Oracle
  + PostgreSQL

When you define an Amazon S3 data store to crawl, you can choose whether to crawl a path in your account or another account\. The output of the crawler is one or more metadata tables defined in the AWS Glue Data Catalog\. A table is created for one or more files found in your data store\. If all the Amazon S3 files in a folder have the same schema, the crawler creates one table\. Also, if the Amazon S3 object is partitioned, only one metadata table is created\.  

If the data store that is being crawled is a relational database, the output is also a set of metadata tables defined in the AWS Glue Data Catalog\. When you crawl a relational database, you must provide authorization credentials for a connection to read objects in the database engine\. Depending on the type of database engine, you can choose which objects are crawled, such as databases, schemas, and tables\.  

If the data store that's being crawled is one or more Amazon DynamoDB tables, the output is one or more metadata tables in the AWS Glue Data Catalog\. When defining a crawler using the AWS Glue console, you specify a DynamoDB table\. If you're using the AWS Glue API, you specify a list of tables\.

## Using Include and Exclude Patterns<a name="crawler-data-stores-exclude"></a>

When evaluating what to include or exclude in a crawl, a crawler starts by evaluating the required include path for Amazon S3 and relational data stores\. For every data store that you want to crawl, you must specify a single include path\. 

For Amazon S3 data stores, the syntax is `bucket-name/folder-name/file-name.ext`\. To crawl all objects in a bucket, you specify just the bucket name in the include path\.

For JDBC data stores, the syntax is either `database-name/schema-name/table-name` or `database-name/table-name`\. The syntax depends on whether the database engine supports schemas within a database\. For example, for database engines such as MySQL or Oracle, don't specify a `schema-name` in your include path\. You can substitute the percent sign \(`%`\) for a schema or table in the include path to represent all schemas or all tables in a database\. You cannot substitute the percent sign \(`%`\) for database in the include path\.  

A crawler connects to a JDBC data store using an AWS Glue connection that contains a JDBC URI connection string\. The crawler only has access to objects in the database engine using the JDBC user name and password in the AWS Glue connection\. *The crawler can only create tables that it can access through the JDBC connection\.* After the crawler accesses the database engine with the JDBC URI, the include path is used to determine which tables in the database engine are created in the Data Catalog\. For example, with MySQL, if you specify an include path of `MyDatabase/%`, then all tables within `MyDatabase` are created in the Data Catalog\. When accessing Amazon Redshift, if you specify an include path of `MyDatabase/%`, then all tables within all schemas for database `MyDatabase` are created in the Data Catalog\. If you specify an include path of `MyDatabase/MySchema/%`, then all tables in database `MyDatabase` and schema `MySchema` are created\. 

After you specify an include path, you can then exclude objects from the crawl that your include path would otherwise include by specifying one or more Unix\-style `glob` exclude patterns\. These patterns are applied to your include path to determine which objects are excluded\. These patterns are also stored as a property of tables created by the crawler\. AWS Glue PySpark extensions, such as `create_dynamic_frame.from_catalog`, read the table properties and exclude objects defined by the exclude pattern\. 

AWS Glue supports the following kinds of `glob` patterns in the exclude pattern\. 


| Exclude pattern | Description | 
| --- | --- | 
| \*\.csv | Matches an Amazon S3 path that represents an object name ending in \.csv | 
| \*\.\* | Matches all object names that contain a dot | 
| \*\.\{csv,avro\} | Matches object names ending with \.csv or \.avro | 
| foo\.? | Matches object names starting with foo\. that are followed by a single character extension | 
| /myfolder/\* | Matches objects in one level of subfolder from myfolder, such as /myfolder/mysource | 
| /myfolder/\*/\* | Matches objects in two levels of subfolders from myfolder, such as /myfolder/mysource/data | 
| /myfolder/\*\* | Matches objects in all subfolders of myfolder, such as /myfolder/mysource/mydata and /myfolder/mysource/data | 
| /myfolder\*\* | Matches subfolder myfolder as well as files below myfolder, such as /myfolder and /myfolder/mydata\.txt | 
| Market\* | Matches tables in a JDBC database with names that begin with Market, such as Market\_us and Market\_fr | 

AWS Glue interprets `glob` exclude patterns as follows:
+ The slash \(`/`\) character is the delimiter to separate Amazon S3 keys into a folder hierarchy\.
+ The asterisk \(`*`\) character matches zero or more characters of a name component without crossing folder boundaries\.
+ A double asterisk \(`**`\) matches zero or more characters crossing folder or schema boundaries\.
+ The question mark \(`?`\) character matches exactly one character of a name component\.
+ The backslash \(`\`\) character is used to escape characters that otherwise can be interpreted as special characters\. The expression `\\` matches a single backslash, and `\{` matches a left brace\.
+ Brackets `[ ]` create a bracket expression that matches a single character of a name component out of a set of characters\. For example, `[abc]` matches `a`, `b`, or `c`\. The hyphen \(`-`\) can be used to specify a range, so `[a-z]` specifies a range that matches from `a` through `z` \(inclusive\)\. These forms can be mixed, so \[`abce-g`\] matches `a`, `b`, `c`, `e`, `f`, or `g`\. If the character after the bracket \(`[`\) is an exclamation point \(`!`\), the bracket expression is negated\. For example, `[!a-c]` matches any character except `a`, `b`, or `c`\.

  Within a bracket expression, the `*`, `?`, and `\` characters match themselves\. The hyphen \(`-`\) character matches itself if it is the first character within the brackets, or if it's the first character after the `!` when you are negating\.
+ Braces \(`{ }`\) enclose a group of subpatterns, where the group matches if any subpattern in the group matches\. A comma \(`,`\) character is used to separate the subpatterns\. Groups cannot be nested\.
+ Leading period or dot characters in file names are treated as normal characters in match operations\. For example, the `*` exclude pattern matches the file name `.hidden`\.

**Example of Amazon S3 Exclude Patterns**  
Each exclude pattern is evaluated against the include path\. For example, suppose that you have the following Amazon S3 directory structure:  

```
/mybucket/myfolder/
   departments/
      finance.json
      market-us.json
      market-emea.json
      market-ap.json
   employees/
      hr.json
      john.csv
      jane.csv
      juan.txt
```
Given the include path `s3://mybucket/myfolder/`, the following are some sample results for exclude patterns:


| Exclude pattern | Results | 
| --- | --- | 
| departments/\*\* | Excludes all files and folders below departments and includes the employees folder and its files | 
| departments/market\* | Excludes market\-us\.json, market\-emea\.json, and market\-ap\.json | 
| \*\*\.csv | Excludes all objects below myfolder that have a name ending with \.csv | 
| employees/\*\.csv | Excludes all \.csv files in the employees folder | 

**Example of Excluding a Subset of Amazon S3 Partitions**  
Suppose that your data is partitioned by day, so that each day in a year is in a separate Amazon S3 partition\. For January 2015, there are 31 partitions\. Now, to crawl data for only the first week of January, you must exclude all partitions except days 1 through 7:  

```
 2015/01/{[!0],0[8-9]}**, 2015/0[2-9]/**, 2015/1[0-2]/**    
```
Take a look at the parts of this glob pattern\. The first part, ` 2015/01/{[!0],0[8-9]}**`, excludes all days that don't begin with a "0" in addition to day 08 and day 09 from month 01 in year 2015\. Notice that "\*\*" is used as the suffix to the day number pattern and crosses folder boundaries to lower\-level folders\. If "\*" is used, lower folder levels are not excluded\.  
The second part, ` 2015/0[2-9]/**`, excludes days in months 02 to 09, in year 2015\.  
The third part, `2015/1[0-2]/**`, excludes days in months 10, 11, and 12, in year 2015\.

**Example of JDBC Exclude Patterns**  
Suppose that you are crawling a JDBC database with the following schema structure:  

```
MyDatabase/MySchema/
   HR_us
   HR_fr
   Employees_Table
   Finance
   Market_US_Table
   Market_EMEA_Table
   Market_AP_Table
```
Given the include path `MyDatabase/MySchema/%`, the following are some sample results for exclude patterns:


| Exclude pattern | Results | 
| --- | --- | 
| HR\* | Excludes the tables with names that begin with HR | 
| Market\_\* | Excludes the tables with names that begin with Market\_ | 
| \*\*\_Table | Excludes all tables with names that end with \_Table | 

## What Happens When a Crawler Runs?<a name="crawler-running"></a>

When a crawler runs, it takes the following actions to interrogate a data store:
+ **Classifies data to determine the format, schema, and associated properties of the raw data** – You can configure the results of classification by creating a custom classifier\.
+ **Groups data into tables or partitions ** – Data is grouped based on crawler heuristics\.
+ **Writes metadata to the Data Catalog ** – You can configure how the crawler adds, updates, and deletes tables and partitions\.

The metadata tables that a crawler creates are contained in a database when you define a crawler\. If your crawler does not define a database, your tables are placed in the default database\. In addition, each table has a classification column that is filled in by the classifier that first successfully recognized the data store\.

The crawler can process both relational database and file data stores\.

If the file that is crawled is compressed, the crawler must download it to process it\. When a crawler runs, it interrogates files to determine their format and compression type and writes these properties into the Data Catalog\. Some file formats \(for example, Apache Parquet\) enable you to compress parts of the file as it is written\. For these files, the compressed data is an internal component of the file and AWS Glue does not populate the `compressionType` property when it writes tables into the Data Catalog\. In contrast, if an **entire file** is compressed by a compression algorithm \(for example, gzip\), then the `compressionType` property is populated when tables are written into the Data Catalog\. 

The crawler generates the names for the tables it creates\. The names of the tables that are stored in the AWS Glue Data Catalog follow these rules:
+ Only alphanumeric characters and underscore \(`_`\) are allowed\.
+ Any custom prefix cannot be longer than 64 characters\.
+ The maximum length of the name cannot be longer than 128 characters\. The crawler truncates generated names to fit within the limit\.
+ If duplicate table names are encountered, the crawler adds a hash string suffix to the name\.

If your crawler runs more than once, perhaps on a schedule, it looks for new or changed files or tables in your data store\. The output of the crawler includes new tables found since a previous run\.

## Are Amazon S3 Folders Created as Tables or Partitions?<a name="crawler-s3-folder-table-partition"></a>

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
In Amazon Athena, each table corresponds to an Amazon S3 prefix with all the objects in it\. If objects have different schemas, Athena does not recognize different objects within the same prefix as separate tables\. This can happen if a crawler creates multiple tables from the same Amazon S3 prefix\. This might lead to queries in Athena that return zero results\. For Athena to properly recognize and query tables, create the crawler with a separate **Include path** for each different table schema in the Amazon S3 folder structure\. For more information, see [Best Practices When Using Athena with AWS Glue](https://docs.aws.amazon.com/athena/latest/ug/glue-best-practices.html)\.