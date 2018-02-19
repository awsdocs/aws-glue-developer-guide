# Cataloging Tables with a Crawler<a name="add-crawler"></a>

You can use a crawler to populate the AWS Glue Data Catalog with tables\. This is the primary method used by most AWS Glue users\. You add a crawler within your Data Catalog to traverse your data stores\. The output of the crawler consists of one or more metadata tables that are defined in your Data Catalog\. Extract, transform, and load \(ETL\) jobs that you define in AWS Glue use these metadata tables as sources and targets\.

Your crawler uses an AWS Identity and Access Management \(IAM\) role for permission to access your data stores and the Data Catalog\. The role you pass to the crawler must have permission to access Amazon S3 paths that are crawled\. Some data stores require additional authorization to establish a connection\. For more information, see [Adding a Connection to Your Data Store](populate-add-connection.md)\.

For more information about using the AWS Glue console to add a crawler, see [Working with Crawlers on the AWS Glue Console](console-crawlers.md)\.

## Defining a Crawler in the AWS Glue Data Catalog<a name="define-crawler"></a>

When you define a crawler, you choose one or more classifiers that evaluate the format of your data to infer a schema\. When the crawler runs, the first classifier in your list to successfully recognize your data store is used to create a schema for your table\. You can use built\-in classifiers or define your own\. AWS Glue provides built\-in classifiers to infer schemas from common files with formats that include JSON, CSV, and Apache Avro\. For the current list of built\-in classifiers in AWS Glue, see [Built\-In Classifiers in AWS Glue ](add-classifier.md#classifier-built-in)\.  

## Which Data Stores Can I Crawl?<a name="crawler-data-stores"></a>

A crawler can crawl both file\-based and relational table\-based data stores\. Crawlers can crawl the following data stores:

+ Amazon Simple Storage Service \(Amazon S3\)

+ Amazon Redshift

+ Amazon Relational Database Service \(Amazon RDS\)

  + Amazon Aurora

  + MariaDB

  + Microsoft SQL Server

  + MySQL

  + Oracle

  + PostgreSQL

+ Publicly accessible databases

  + Amazon Aurora

  + MariaDB

  + Microsoft SQL Server

  + MySQL

  + Oracle

  + PostgreSQL

When you define an Amazon S3 data store to crawl, you can choose whether to crawl a path in your account or another account\. You choose the path to be crawled using the form `Bucket/Folder/File`\. The output of the crawler is one or more metadata tables defined in the AWS Glue Data Catalog\. A table is created for one or more files found in your data store\. If all the Amazon S3 files in a folder have the same schema, the crawler creates one table\. Also, if the Amazon S3 object is partitioned, only one metadata table is created\.  

If the data store that is being crawled is a relational database, the output is also a set of metadata tables defined in the AWS Glue Data Catalog\. You can choose all databases, schemas, and tables in your data store\. Alternatively, you can choose the tables to be crawled using the form `Database/Schema/Table`\. When you crawl a relational database, you must provide connection information for authorization credentials\.  

## Using Include and Exclude Patterns<a name="crawler-data-stores-exclude"></a>

When evaluating what to include or exclude in a crawl, a crawler starts by evaluating the required include path\. For every data store you want to crawl, you must specify a single include path\.

For Amazon S3 data stores, the syntax is `bucket-name/folder-name/file-name.ext`\. To crawl all objects in a bucket, you specify `%` for the `folder-name/file-name.ext` part of the include path\.

For JDBC data stores, the syntax is `database-name/schema-name/table-name`\. For database engines such as MySQL that don't have a `schema-name`, you can use `database-name/table-name` instead\. You can substitute the percent sign \(`%`\) for a schema or table in the include path to represent all schemas or all tables in a database\. You cannot substitute the percent sign \(`%`\) for database in the include path\.  For example, an include path of `MyDatabase/MySchema/%` includes all tables in database `MyDatabase` and schema `MySchema`\.

You can then exclude objects from the crawl that your include path would otherwise include by specifying one or more Unix\-style `glob` exclude patterns\.

AWS Glue supports the following kinds of `glob` patterns in the exclude pattern\. These patterns are applied to your include path to determine which objects are excluded:


| Exclude pattern | Description | 
| --- | --- | 
| \*\.csv | Matches an Amazon S3 path that represents an object name ending in \.csv | 
| \*\.\* | Matches all object names that contain a dot | 
| \*\.\{csv,avro\} | Matches object names ending with \.csv or \.avro | 
| foo\.? | Matches object names starting with foo\. that are followed by a single character extension | 
| /myfolder/\* | Matches objects in one level of subfolder from myfolder, such as /myfolder/mysource | 
| /myfolder/\*/\* | Matches objects in two levels of subfolders from myfolder, such as /myfolder/mysource/data | 
| /myfolder/\*\* | Matches objects in all subfolders of myfolder, such as /myfolder/mysource/mydata and /myfolder/mysource/data | 
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
| departments/\*\* | Excludes all files and folders below departments and includes the employees directory and its files | 
| departments/market\* | Excludes market\-us\.json, market\-emea\.json, and market\-ap\.json | 
| \*\*\.csv | Excludes all objects below myfolder that have a name ending with \.csv | 
| employees/\*\.csv | Excludes all \.csv files in the employees directory | 

**Example of Excluding a Subset of Amazon S3 Partitions**  
Suppose your data is partitioned by day so that each day in a year is in a separate Amazon S3 partition\. For January 2015, there are 31 partitions\. Now, to crawl data for only the first week of January, you need to exclude all partitions except days 1 through 7:  

```
 2015/01/{[!0],0[8-9]}**, 2015/0[2-9]/**, 2015/1[0-2]/**    
```
Let's look at the parts of this glob pattern\. The first part, ` 2015/01/{[!0],0[8-9]}**`, excludes all days that don't begin with a "0" as well as day 08 and day 09 from month 01 in year 2015\. Notice "\*\*" is used as the suffix to the day number pattern and crosses folder boundries to lower level folders\. If "\*" is used, lower folder levels are not excluded\.  
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

The metadata tables that a crawler creates are contained in a database as defined by the crawler\. If your crawler does not define a database, your tables are placed in the default database\. In addition, each table has a classification column that is filled in by the classifier that first successfully recognized the data store\.

The crawler can process both relational database and file data stores\. If the file that is crawled is compressed, the crawler must download it to process it\.

The crawler generates the names for the tables it creates\. The name of the tables that are stored in the AWS Glue Data Catalog follow these rules:

+ Only alphanumeric characters and underscore \(`_`\) are allowed\.

+ Any custom prefix cannot be longer than 64 characters\.

+ The maximum length of the name cannot be longer than 128 characters\. The crawler truncates generated names to fit within the limit\.

+ If duplicate table names are encountered, the crawler adds a hash string suffix to the name\.

If your crawler runs more than once, perhaps on a schedule, it looks for new or changed files or tables in your data store\. The output of the crawler includes new tables found since a previous run\.

## What Happens When a Crawler Detects Schema Changes?<a name="crawler-schema-changes"></a>

When a crawler runs against a previously crawled data store, it might discover that a schema is changed or that objects in the data store are now deleted\. The crawler logs schema changes as it runs\. You specify the behavior of the crawler when it finds changes in the schema\.

When a crawler runs, new tables and partitions are always created regardless of the schema change policy\. When the crawler finds a changed schema, you can choose one of the following actions:

+ Update the table in the AWS Glue Data Catalog\. This is the default\.

+ Ignore the change\. Do not modify the table in the Data Catalog\.

When the crawler finds a deleted object in the data store, you can choose one of the following actions:

+ Delete the table from the Data Catalog\.

+ Ignore the change\. Do not modify the table in the Data Catalog\.

+ Mark the table as deprecated in the Data Catalog\. This is the default\.

## What Happens When a Crawler Detects Partition Changes?<a name="crawler-partition-changes"></a>

When a crawler runs against a previously crawled data store, it might discover new or changed partitions\. By default, new partitions are added and existing partitions are updated if they have changed\. In addition, you can set a crawler configuration option to `InheritFromTable` \(this option is named **Update all new and existing partitions with metadata from the table** on the AWS Glue console\)\. When this option is set, partitions inherit metadata properties such as their classification, input format, output format, serde information, and schema from their parent table\. Any change in properties to the parent table are propagated to its partitions\. When this configuration option is set on an existing crawler, existing partitions are updated to match the properties of their parent table the next time the crawler runs\. 