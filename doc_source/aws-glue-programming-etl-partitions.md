# Managing Partitions for ETL Output in AWS Glue<a name="aws-glue-programming-etl-partitions"></a>

Partitioning is an important technique for organizing datasets so they can be queried efficiently\. It organizes data in a hierarchical directory structure based on the distinct values of one or more columns\.

For example, you might decide to partition your application logs in Amazon Simple Storage Service \(Amazon S3\) by date, broken down by year, month, and day\. Files that correspond to a single day's worth of data are then placed under a prefix such as `s3://my_bucket/logs/year=2018/month=01/day=23/`\. Systems like Amazon Athena, Amazon Redshift Spectrum, and now AWS Glue can use these partitions to filter data by partition value without having to read all the underlying data from Amazon S3\.

Crawlers not only infer file types and schemas, they also automatically identify the partition structure of your dataset when they populate the AWS Glue Data Catalog\. The resulting partition columns are available for querying in AWS Glue ETL jobs or query engines like Amazon Athena\.

After you crawl a table, you can view the partitions that the crawler created by navigating to the table in the AWS Glue console and choosing **View Partitions**\.

For Apache Hive\-style partitioned paths in `key=val` style, crawlers automatically populate the column name using the key name\. Otherwise, it uses default names like `partition_0`, `partition_1`, and so on\. To change the default names on the console, navigate to the table, choose **Edit Schema**, and modify the names of the partition columns there\.

In your ETL scripts, you can then filter on the partition columns\.

## Pre\-Filtering Using Pushdown Predicates<a name="aws-glue-programming-etl-partitions-pushdowns"></a>

In many cases, you can use a pushdown predicate to filter on partitions without having to list and read all the files in your dataset\. Instead of reading the entire dataset and then filtering in a DynamicFrame, you can apply the filter directly on the partition metadata in the Data Catalog\. Then you only list and read what you actually need into a DynamicFrame\.

For example, in Python you could write the following:

```
glue_context.create_dynamic_frame.from_catalog(
    database = "my_S3_data_set",
    table_name = "catalog_data_table",
    push_down_predicate = my_partition_predicate)
```

This creates a DynamicFrame that loads only the partitions in the Data Catalog that satisfy the predicate expression\. Depending on how small a subset of your data you are loading, this can save a great deal of processing time\.

The predicate expression can be any Boolean expression supported by Spark SQL\. Anything you could put in a `WHERE` clause in a Spark SQL query will work\. For example, the predicate expression `pushDownPredicate = "(year=='2017' and month=='04')"` loads only the partitions in the Data Catalog that have both `year` equal to 2017 and `month` equal to 04\. For more information, see the [Apache Spark SQL documentation](https://spark.apache.org/docs/2.1.1/sql-programming-guide.html), and in particular, the [Scala SQL functions reference](https://spark.apache.org/docs/2.1.1/api/scala/index.html#org.apache.spark.sql.functions$)\.

In addition to Hive\-style partitioning for Amazon S3 paths, Apache Parquet and Apache ORC file formats further partition each file into blocks of data that represent column values\. Each block also stores statistics for the records that it contains, such as min/max for column values\. AWS Glue supports pushdown predicates for both Hive\-style partitions and block partitions in these formats\. In this way, you can prune unnecessary Amazon S3 partitions in Parquet and ORC formats, and skip blocks that you determine are unnecessary using column statistics\.

## Writing Partitions<a name="aws-glue-programming-etl-partitions-writing"></a>

By default, a DynamicFrame is not partitioned when it is written\. All of the output files are written at the top level of the specified output path\. Until recently, the only way to write a DynamicFrame into partitions was to convert it to a Spark SQL DataFrame before writing\.

However, DynamicFrames now support native partitioning using a sequence of keys, using the `partitionKeys` option when you create a sink\. For example, the following Python code writes out a dataset to Amazon S3 in the Parquet format, into directories partitioned by the type field\. From there, you can process these partitions using other systems, such as Amazon Athena\.

```
glue_context.write_dynamic_frame.from_options(
    frame = projectedEvents,
    connection_type = "s3",    
    connection_options = {"path": "$outpath", "partitionKeys": ["type"]},
    format = "parquet")
```