# Crawler Properties<a name="define-crawler"></a>

When defining a crawler using the AWS Glue console or the AWS Glue API, you specify the following information:

**Crawler name and optional descriptors and settings**  
Settings include tags, security configuration, and custom classifiers\. You define custom classifiers before defining crawlers\. For more information, see the following:   
+ [AWS Tags in AWS Glue](monitor-tags.md)
+ [Adding Classifiers to a Crawler](add-classifier.md)

**Crawler source type**  
The crawler can access data stores directly as the source of the crawl, or it can use existing tables in the Data Catalog as the source\. If the crawler uses existing catalog tables, it crawls the data stores that are specified by those catalog tables\. For more information, see [Crawler Source Type](#crawler-source-type)\.

**Crawl only new folders for S3 data sources**  
If turned on, only Amazon S3 folders that were added since the last crawler run will be crawled\. For more information, see [Incremental Crawls in AWS Glue](incremental-crawls.md)\.

**Crawler sources: data stores or catalog tables**  
Choose from among the following:  
+ One or more data stores

  A crawler can crawl multiple data stores of different types \(Amazon S3, JDBC, and so on\)\. 
+ List of Data Catalog tables

  The catalog tables specify the data stores to crawl\. The crawler can crawl only catalog tables in a single run; it can't mix in other source types\. 
You can configure only one data store at a time\. After you have provided the connection information and include paths and exclude patterns, you then have the option of adding another data store\.  
For more information, see [Crawler Source Type](#crawler-source-type)\.

**Additional crawler source parameters**  
Each source type requires a different set of additional parameters\. The following is an incomplete list:    
**Connection**  
Select or add an AWS Glue connection\. For information about connections, see [Defining Connections in the AWS Glue Data Catalog](populate-add-connection.md)\.  
**Enable data sampling \(for Amazon DynamoDB, MongoDB, and Amazon DocumentDB data stores only\)**  
Select whether to crawl a data sample only\. If not selected the entire table is crawled\. Scanning all the records can take a long time when the table is not a high throughput table\.  
**Scanning rate \(for DynamoDB data stores only\)**  
Specify the percentage of the configured read capacity units to use by the AWS Glue crawler\. Read capacity units is a term defined by DynamoDB, and is a numeric value that acts as rate limiter for the number of reads that can be performed on that table per second\. Enter a value between 0\.1 and 1\.5\. If not specified, defaults to 0\.5% for provisioned tables and 1/4 of maximum configured capacity for on\-demand tables\.  
**Sample size \(optional\) \(for Amazon S3 data stores only\)**  
Specify the number of files in each leaf folder to be crawled when crawling sample files in a dataset\. When this feature is turned on, instead of crawling all the files in this dataset, the crawler randomly selects some files in each leaf folder to crawl\.   
The sampling crawler is best suited for customers who have previous knowledge about their data formats and know that schemas in their folders do not change\. Turning on this feature will significantly reduce crawler runtime\.  
A valid value is an integer between 1 and 249\. If not specified, all the files are crawled\.  
**Include path**    
For an Amazon S3 data store  
Choose whether to specify a path in your account or another account, and then browse to choose an Amazon S3 path\.  
For a JDBC data store  
Enter *<database>*/*<schema>*/*<table>* or *<database>*/*<table>*, depending on the database product\. Oracle Database and MySQL don’t support schema in the path\. You can substitute the percent \(%\) character for *<schema>* or *<table>*\. For example, for an Oracle database with a system identifier \(SID\) of `orcl`, enter `orcl/%` to import all tables to which the user named in the connection has access\.  
This field is case\-sensitive\.  
For a MongoDB or Amazon DocumentDB data store  
Enter *database*/*collection*\.
For more information, see [Include and Exclude Patterns](#crawler-data-stores-exclude)\.  
**Exclude patterns**  
These enable you to exclude certain files or tables from the crawl\. For more information, see [Include and Exclude Patterns](#crawler-data-stores-exclude)\.

**IAM role**  
The crawler assumes this role\. It must have permissions similar to the AWS managed policy `AWSGlueServiceRole`\. For Amazon S3 and DynamoDB sources, it must also have permissions to access the data store\. If the crawler reads Amazon S3 data encrypted with AWS Key Management Service \(AWS KMS\), then the role must have decrypt permissions on the AWS KMS key\.   
For an Amazon S3 data store, additional permissions attached to the role would be similar to the following:   

```
{
   "Version": "2012-10-17",
    "Statement": [
        {
          "Effect": "Allow",
          "Action": [
              "s3:GetObject",
              "s3:PutObject"
          ],
          "Resource": [
              "arn:aws:s3:::bucket/object*"
          ]
        }
    ]
}
```
For an Amazon DynamoDB data store, additional permissions attached to the role would be similar to the following:   

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:DescribeTable",
        "dynamodb:Scan"
      ],
      "Resource": [
        "arn:aws:dynamodb:region:account-id:table/table-name*"
      ]
    }
  ]
}
```
For more information, see [Step 2: Create an IAM Role for AWS Glue](create-an-iam-role.md) and [Managing Access Permissions for AWS Glue Resources](access-control-overview.md)\.

**Crawler schedule**  
You can run a crawler on demand or define a schedule for automatic running of the crawler\. For more information, see [Scheduling an AWS Glue Crawler](schedule-crawler.md)\.

**Destination database within the Data Catalog for the created catalog tables**  
For more information, see [Defining a Database in Your Data Catalog](define-database.md)\.

**Output configuration options**  
Options include how the crawler should handle detected schema changes, deleted objects in the data store, and more\. For more information, see [Setting Crawler Configuration Options](crawler-configuration.md)\.

**Recrawl Policy**  
When crawling an Amazon S3 data source after the first crawl is complete, choose whether to crawl the entire dataset again or to crawl only folders that were added since the last crawler run\. For more information, see [Incremental Crawls in AWS Glue](incremental-crawls.md)\.

## Crawler Source Type<a name="crawler-source-type"></a>

A crawler can access data stores directly as the source of the crawl or use existing catalog tables as the source\. If the crawler uses existing catalog tables, it crawls the data stores specified by those catalog tables\.

A common reason to specify a catalog table as the source is when you create the table manually \(because you already know the structure of the data store\) and you want a crawler to keep the table updated, including adding new partitions\. For a discussion of other reasons, see [Updating Manually Created Data Catalog Tables Using Crawlers](tables-described.md#update-manual-tables)\.

When you specify existing tables as the crawler source type, the following conditions apply:
+ Database name is optional\.
+ Only catalog tables that specify Amazon S3 or Amazon DynamoDB data stores are permitted\.
+ No new catalog tables are created when the crawler runs\. Existing tables are updated as needed, including adding new partitions\.
+ Deleted objects found in the data stores are ignored; no catalog tables are deleted\. Instead, the crawler writes a log message\. \(`SchemaChangePolicy.DeleteBehavior=LOG`\)
+ The crawler configuration option to create a single schema for each Amazon S3 path is enabled by default and cannot be disabled\. \(`TableGroupingPolicy`=`CombineCompatibleSchemas`\) For more information, see [How to Create a Single Schema for Each Amazon S3 Include Path](crawler-configuration.md#crawler-grouping-policy)\.
+ You can't mix catalog tables as a source with any other source types \(for example Amazon S3 or Amazon DynamoDB\)\.

## Include and Exclude Patterns<a name="crawler-data-stores-exclude"></a>

When evaluating what to include or exclude in a crawl, a crawler starts by evaluating the required include path\. For Amazon S3, MongoDB, Amazon DocumentDB \(with MongoDB compatibility\), and relational data stores, you must specify an include path\.

For Amazon S3 data stores, include path syntax is `bucket-name/folder-name/file-name.ext`\. To crawl all objects in a bucket, you specify just the bucket name in the include path\. The exclude pattern is relative to the include path

For MongoDB and Amazon DocumentDB \(with MongoDB compatibility\), the syntax is `database/collection`\.

For JDBC data stores, the syntax is either `database-name/schema-name/table-name` or `database-name/table-name`\. The syntax depends on whether the database engine supports schemas within a database\. For example, for database engines such as MySQL or Oracle, don't specify a `schema-name` in your include path\. You can substitute the percent sign \(`%`\) for a schema or table in the include path to represent all schemas or all tables in a database\. You cannot substitute the percent sign \(`%`\) for database in the include path\. The exclude path is relative to the include path\. For example, to exclude a table in your JDBC data store, type the table name in the exclude path\. 

A crawler connects to a JDBC data store using an AWS Glue connection that contains a JDBC URI connection string\. The crawler only has access to objects in the database engine using the JDBC user name and password in the AWS Glue connection\. *The crawler can only create tables that it can access through the JDBC connection\.* After the crawler accesses the database engine with the JDBC URI, the include path is used to determine which tables in the database engine are created in the Data Catalog\. For example, with MySQL, if you specify an include path of `MyDatabase/%`, then all tables within `MyDatabase` are created in the Data Catalog\. When accessing Amazon Redshift, if you specify an include path of `MyDatabase/%`, then all tables within all schemas for database `MyDatabase` are created in the Data Catalog\. If you specify an include path of `MyDatabase/MySchema/%`, then all tables in database `MyDatabase` and schema `MySchema` are created\. 

After you specify an include path, you can then exclude objects from the crawl that your include path would otherwise include by specifying one or more Unix\-style `glob` exclude patterns\. These patterns are applied to your include path to determine which objects are excluded\. These patterns are also stored as a property of tables created by the crawler\. AWS Glue PySpark extensions, such as `create_dynamic_frame.from_catalog`, read the table properties and exclude objects defined by the exclude pattern\. 

AWS Glue supports the following kinds of `glob` patterns in the exclude pattern\. 


| Exclude pattern | Description | 
| --- | --- | 
| \*\.csv | Matches an Amazon S3 path that represents an object name in the current folder ending in \.csv | 
| \*\.\* | Matches all object names that contain a dot | 
| \*\.\{csv,avro\} | Matches object names ending with \.csv or \.avro | 
| foo\.? | Matches object names starting with foo\. that are followed by a single character extension | 
| myfolder/\* | Matches objects in one level of subfolder from myfolder, such as /myfolder/mysource | 
| myfolder/\*/\* | Matches objects in two levels of subfolders from myfolder, such as /myfolder/mysource/data | 
| myfolder/\*\* | Matches objects in all subfolders of myfolder, such as /myfolder/mysource/mydata and /myfolder/mysource/data | 
| myfolder\*\* | Matches subfolder myfolder as well as files below myfolder, such as /myfolder and /myfolder/mydata\.txt | 
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