# Managing Partitions for ETL Output in AWS Glue<a name="aws-glue-programming-etl-partitions"></a>

Partitioning is an important technique for organizing datasets so they can be queried efficiently\. It organizes data in a hierarchical directory structure based on the distinct values of one or more columns\.

For example, you might decide to partition your application logs in S3 by date, broken down by year, month, and day\. Files corresponding to a single day's worth of data would then be placed under a prefix such as `s3://my_bucket/logs/year=2018/month=01/day=23/`\. Systems such as Amazon Athena, Amazon Redshift Spectrum, and now AWS Glue can use these partitions to filter data by partition value without having to read all the underlying data from S3\.

Crawlers not only infer file types and schemas, they also automatically identify the partition structure of your dataset when they populate the Data Catalog\. The resulting partition columns are available for querying in Glue ETL jobs or query engines like Amazon Athena\.

Once you have crawled a table, you can view the partitions that the crawler created by navigating to the table in the AWS Glue Console and selecting **View Partitions**\.

For Hive\-style partitioned paths in `key=val` style, crawlers automatically populate the column name in the Data Catalog using default names like partition\_0, partition\_1, and so on\. You can change the default names in the AWS Glue console by navigating to the table, selecting **Edit Schema**, and modifying the names of the partition columns there\.

In your ETL scripts, you can then filter on the partition columns\.

## Pre\-filtering Using Pushdown Predicates<a name="aws-glue-programming-etl-partitions-pushdowns"></a>

In many cases you can use a pushdown predicate to filter on partitions without having to list and read all the files in your data set\. Instead of reading the entire data set and then filtering in a DynamicFrame, you can apply the filter directly on the partition metadata in the catalog, and only list and read what you actually need into a DynamicFrame\.

For example, in Python you could write:

```
glue_context.create_dynamic_frame.from_catalog(
    database = "my_S3_data_set",
    table_name = "catalog_data_table",
    push_down_predicate = my_partition_predicate)
```

This would create a DynamicFrame that loaded only the partitions in the catalog that satisfied the predicate expression\. Depending on how small a subset of your data you are loading, this can save a great deal of processing time\.

The predicate expression can be any Boolean expression supported by SparkSQL\. Anything you could put in a `WHERE` clause in a SparkSQL SQL query will work\. See the [Spark SQL documentation](https://spark.apache.org/docs/2.1.1/sql-programming-guide.html), and in particular the [Scala SQL functions reference](https://spark.apache.org/docs/2.1.1/api/scala/index.html#org.apache.spark.sql.functions$), for more information\.

In addition to Hive\-style partitioning for S3 paths, Parquet and ORC file formats further partition each file into blocks of data that represent column values\. Each block also stores statistics for the records that it contains, such as min/max for column values\. AWS Glue supports pushdown precicates for both Hive\-style partitions and block partitions in these formats\. In this way, you can prune unnecessary S3 partitions in Parquet and ORC formats, and skip blocks determined unnecessary using column statistics\.

## Writing Partitions<a name="aws-glue-programming-etl-partitions-writing"></a>

By default, a DynamicFrame is not partitioned when it is written\. All of the output files are written at the top level of the specified output path\. Until recently the only way to write a DynamicFrame into partitions was to convert it to a Spark SQL DataFrame before writing\.

However, DynamicFrames now support native partitioning using a sequence of keys, using the `partitionKeys` option when creating a sink\. For example, the following Python code writes out a dataset to S3 in the parquet format, into directories partitioned by the type field\. From there, these partitions can be processed using other systems such as Amazon Athena\.

```
glue_context.write_dynamic_frame.from_options(
    frame = projectedEvents,
    connection_options = {"path": "$outpath", "partitionKeys": ["type"]},
    format = "parquet")
```