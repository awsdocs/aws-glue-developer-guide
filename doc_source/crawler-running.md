# How Crawlers Work<a name="crawler-running"></a>

When a crawler runs, it takes the following actions to interrogate a data store:
+ **Classifies data to determine the format, schema, and associated properties of the raw data** – You can configure the results of classification by creating a custom classifier\.
+ **Groups data into tables or partitions ** – Data is grouped based on crawler heuristics\.
+ **Writes metadata to the Data Catalog ** – You can configure how the crawler adds, updates, and deletes tables and partitions\.

When you define a crawler, you choose one or more classifiers that evaluate the format of your data to infer a schema\. When the crawler runs, the first classifier in your list to successfully recognize your data store is used to create a schema for your table\. You can use built\-in classifiers or define your own\. You define your custom classifiers in a separate operation, before you define the crawlers\. AWS Glue provides built\-in classifiers to infer schemas from common files with formats that include JSON, CSV, and Apache Avro\. For the current list of built\-in classifiers in AWS Glue, see [Built\-In Classifiers in AWS Glue ](add-classifier.md#classifier-built-in)\.  

The metadata tables that a crawler creates are contained in a database when you define a crawler\. If your crawler does not specify a database, your tables are placed in the default database\. In addition, each table has a classification column that is filled in by the classifier that first successfully recognized the data store\.

If the file that is crawled is compressed, the crawler must download it to process it\. When a crawler runs, it interrogates files to determine their format and compression type and writes these properties into the Data Catalog\. Some file formats \(for example, Apache Parquet\) enable you to compress parts of the file as it is written\. For these files, the compressed data is an internal component of the file, and AWS Glue does not populate the `compressionType` property when it writes tables into the Data Catalog\. In contrast, if an *entire file* is compressed by a compression algorithm \(for example, gzip\), then the `compressionType` property is populated when tables are written into the Data Catalog\. 

The crawler generates the names for the tables that it creates\. The names of the tables that are stored in the AWS Glue Data Catalog follow these rules:
+ Only alphanumeric characters and underscore \(`_`\) are allowed\.
+ Any custom prefix cannot be longer than 64 characters\.
+ The maximum length of the name cannot be longer than 128 characters\. The crawler truncates generated names to fit within the limit\.
+ If duplicate table names are encountered, the crawler adds a hash string suffix to the name\.

If your crawler runs more than once, perhaps on a schedule, it looks for new or changed files or tables in your data store\. The output of the crawler includes new tables and partitions found since a previous run\.

**Topics**
+ [How Does a Crawler Determine When to Create Partitions?](crawler-s3-folder-table-partition.md)
+ [Incremental Crawls in AWS Glue](incremental-crawls.md)