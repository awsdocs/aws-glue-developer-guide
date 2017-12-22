# Defining Tables in the AWS Glue Data Catalog<a name="tables-described"></a>

When you define a table in AWS Glue, you also specify the value of a classification field that indicates the type and format of the data that's stored in that table\. If a crawler creates the table, these classifications are determined by either a built\-in classifier or a custom classifier\. If you create a table manually in the console or by using an API, you specify the classification when you define the table\. For more information about creating a table using the AWS Glue console, see [Working with Tables on the AWS Glue Console](console-tables.md)\. 

When a crawler detects a change in table metadata, a new version of the table is created in the AWS Glue Data Catalog\. You can compare current and past versions of a table\.

The schema of the table contains its structure\. You can also edit a schema to create a new version of the table\.

The table's history is also maintained in the Data Catalog\. This history includes metrics that are gathered when a data store is updated by an extract, transform, and load \(ETL\) job\. You can find out the name of the job, when it ran, how many rows were added, and how long the job took to run\. The version of the schema that was used by an ETL job is also kept in the history\.

The AWS Glue Data Catalog also tracks when a table is used as a source or target in an ETL job\. This can give you some insight into the lineage of your data\. It tells you which jobs read the table as input and which ones write to your table as a data target\.

## Table Partitions<a name="tables-partition"></a>

An AWS Glue table definition of an Amazon Simple Storage Service \(Amazon S3\) folder can describe a partitioned table\. For example, to improve query performance, a partitioned table might separate monthly data into different files using the name of the month as a key\. In AWS Glue, table definitions include the partitioning key of a table\. When AWS Glue evaluates the data in Amazon S3 folders to catalog a table, it determines whether an individual table or a partitioned table is added\. 

All the following conditions must be true for AWS Glue to create a partitioned table for an Amazon S3 folder:

+ The schemas of the files are similar, as determined by AWS Glue\.

+ The data format of the files is the same\.

+ The compression format of the files is the same\.

For example, you might own an Amazon S3 bucket named `my-app-bucket`, where you store both iOS and Android app sales data\. The data is partitioned by year, month, and day\. The data files for iOS and Android sales have the same schema, data format, and compression format\. In the AWS Glue Data Catalog, the AWS Glue crawler creates one table definition with partitioning keys for year, month, and day\. 

The following Amazon S3 listing of `my-app-bucket` shows some of the partitions\. The `=` symbol is used to assign partition key values\. 

```
   my-app-bucket/Sales/year='2010'/month='feb'/day='1'/iOS.csv
   my-app-bucket/Sales/year='2010'/month='feb'/day='1'/Android.csv
   my-app-bucket/Sales/year='2010'/month='feb'/day='2'/iOS.csv
   my-app-bucket/Sales/year='2010'/month='feb'/day='2'/Android.csv
   ...
   my-app-bucket/Sales/year='2017'/month='feb'/day='4'/iOS.csv
   my-app-bucket/Sales/year='2017'/month='feb'/day='4'/Android.csv
```