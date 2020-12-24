# Defining Tables in the AWS Glue Data Catalog<a name="tables-described"></a>

You can add table definitions to the Data Catalog in the following ways:
+ Run a crawler that connects to one or more data stores, determines the data structures, and writes tables into the Data Catalog\. The crawler uses built\-in or custom classifiers to recognize the structure of the data\. You can run your crawler on a schedule\. For more information, see [Defining Crawlers](add-crawler.md)\.
+ You can run create new tables directly from Glue ETL job using setCatalogInfo API. For more information see https://docs.aws.amazon.com/glue/latest/dg/update-from-job.html
+ Use the AWS Glue console to manually create a table in the AWS Glue Data Catalog\. For more information, see Creating New Tables section here [Working with Tables on the AWS Glue Console](console-tables.md)\.
+ Use the `CreateTable` operation in the [AWS Glue API](aws-glue-api.md) to create a table in the AWS Glue Data Catalog\. For more information, see [CreateTable Action \(Python: create\_table\)](aws-glue-api-catalog-tables.md#aws-glue-api-catalog-tables-CreateTable)\.
+ Use AWS CloudFormation templates\. For more information, see [Populating the Data Catalog Using AWS CloudFormation Templates](populate-with-cloudformation-templates.md)\.
+ Migrate an Apache Hive metastore\. For more information, see [Migration between the Hive Metastore and the AWS Glue Data Catalog](https://github.com/aws-samples/aws-glue-samples/tree/master/utilities/Hive_metastore_migration) on GitHub\.

When you define a table manually using the console or an API, you specify the table schema and the value of a classification field that indicates the type and format of the data in the data source\. If a crawler creates the table, the data format and schema are determined by either a built\-in classifier or a custom classifier\. For more information about creating a table using the AWS Glue console, see [Working with Tables on the AWS Glue Console](console-tables.md)\.

**Topics**
+ [Table Partitions](#tables-partition)
+ [Table Resource Links](#tables-resource-links)
+ [Updating Manually Created Data Catalog Tables Using Crawlers](#update-manual-tables)
+ [Working with Tables on the AWS Glue Console](console-tables.md)
+ [Working with Partition Indexes](partition-indexes.md)

## Table Partitions<a name="tables-partition"></a>

An AWS Glue table definition of an Amazon Simple Storage Service \(Amazon S3\) folder can describe a partitioned table\. For example, to improve query performance, a partitioned table might separate monthly data into different files using the name of the month as a key\. In AWS Glue, table definitions include the partitioning key of a table\. When AWS Glue evaluates the data in Amazon S3 folders to catalog a table, it determines whether an individual table or a partitioned table is added\. 

You can create partition indexes on a table to fetch a subset of the partitions instead of loading all the partitions in the table\. For information about working with partition indexes, see [Working with Partition Indexes](partition-indexes.md)\.

All the following conditions must be true for AWS Glue to create a partitioned table for an Amazon S3 folder:
+ The schemas of the files are similar, as determined by AWS Glue\.
+ The data format of the files is the same\.
+ The compression format of the files is the same\.

For example, you might own an Amazon S3 bucket named `my-app-bucket`, where you store both iOS and Android app sales data\. The data is partitioned by year, month, and day\. The data files for iOS and Android sales have the same schema, data format, and compression format\. In the AWS Glue Data Catalog, the AWS Glue crawler creates one table definition with partitioning keys for year, month, and day\. 

The following Amazon S3 listing of `my-app-bucket` shows some of the partitions\. The `=` symbol is used to assign partition key values\. 

```
   my-app-bucket/Sales/year=2010/month=feb/day=1/iOS.csv
   my-app-bucket/Sales/year=2010/month=feb/day=1/Android.csv
   my-app-bucket/Sales/year=2010/month=feb/day=2/iOS.csv
   my-app-bucket/Sales/year=2010/month=feb/day=2/Android.csv
   ...
   my-app-bucket/Sales/year=2017/month=feb/day=4/iOS.csv
   my-app-bucket/Sales/year=2017/month=feb/day=4/Android.csv
```

## Table Resource Links<a name="tables-resource-links"></a>

The Data Catalog can also contain *resource links* to tables\. A table resource link is a link to a local or shared table\. Currently, you can create resource links only in AWS Lake Formation\. After you create a resource link to a table, you can use the resource link name wherever you would use the table name\. Along with tables that you own or that are shared with you, table resource links are returned by `glue:GetTables()` and appear as entries on the **Tables** page of the AWS Glue console\.

The Data Catalog can also contain database resource links\.

For more information about resource links, see [Creating Resource Links](https://docs.aws.amazon.com/lake-formation/latest/dg/creating-resource-links.html) in the *AWS Lake Formation Developer Guide*\.

## Updating Manually Created Data Catalog Tables Using Crawlers<a name="update-manual-tables"></a>

You might want to create AWS Glue Data Catalog tables manually and then keep them updated with AWS Glue crawlers\. Crawlers running on a schedule can add new partitions and update the tables with any schema changes\. This also applies to tables migrated from an Apache Hive metastore\.

To do this, when you define a crawler, instead of specifying one or more data stores as the source of a crawl, you specify one or more existing Data Catalog tables\. The crawler then crawls the data stores specified by the catalog tables\. In this case, no new tables are created; instead, your manually created tables are updated\.

The following are other reasons why you might want to manually create catalog tables and specify catalog tables as the crawler source:
+ You want to choose the catalog table name and not rely on the catalog table naming algorithm\.
+ You want to prevent new tables from being created in the case where files with a format that could disrupt partition detection are mistakenly saved in the data source path\.

For more information, see [Crawler Source Type](define-crawler.md#crawler-source-type)\.
