# DynamicFrameReader Class<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-reader"></a>

##  — Methods —<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-reader-_methods"></a>
+ [\_\_init\_\_](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-reader-__init__)
+ [from\_rdd](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-reader-from_rdd)
+ [from\_options](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-reader-from_options)
+ [from\_catalog](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-reader-from_catalog)

## \_\_init\_\_<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-reader-__init__"></a>

**`__init__(glue_context)`**
+ `glue_context` – The [GlueContext Class](aws-glue-api-crawler-pyspark-extensions-glue-context.md) to use\.

## from\_rdd<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-reader-from_rdd"></a>

**`from_rdd(data, name, schema=None, sampleRatio=None)`**

Reads a `DynamicFrame` from a Resilient Distributed Dataset \(RDD\)\.
+ `data` – The dataset to read from\.
+ `name` – The name to read from\.
+ `schema` – The schema to read \(optional\)\.
+ `sampleRatio` – The sample ratio \(optional\)\.

## from\_options<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-reader-from_options"></a>

**`from_options(connection_type, connection_options={}, format=None, format_options={}, transformation_ctx="", push_down_predicate="")`**

Reads a `DynamicFrame` using the specified connection and format\.
+ `connection_type` – The connection type\. Valid values include `s3`, `mysql`, `postgresql`, `redshift`, `sqlserver`, `oracle`, and `dynamodb`\.
+ `connection_options` – Connection options, such as path and database table \(optional\)\. For a `connection_type` of `s3`, Amazon S3 paths are defined in an array\.

  ```
  connection_options = {"paths": [ "s3://mybucket/object_a", "s3://mybucket/object_b"]}
  ```

  For JDBC connections, several properties must be defined\. Note that the database name must be part of the URL\. It can optionally be included in the connection options\.

  ```
  connection_options = {"url": "jdbc-url/database", "user": "username", "password": "password","dbtable": "table-name", "redshiftTmpDir": "s3-tempdir-path"} 
  ```

  For a JDBC connection that performs parallel reads, you can set the hashfield option\. For example:

  ```
  connection_options = {"url": "jdbc-url/database", "user": "username", "password": "password","dbtable": "table-name", "redshiftTmpDir": "s3-tempdir-path" , "hashfield": "month"} 
  ```

  For more information, see [Reading from JDBC Tables in Parallel](run-jdbc-parallel-read-job.md)\. 
+ `format` – A format specification \(optional\)\. This is used for an Amazon Simple Storage Service \(Amazon S3\) or an AWS Glue connection that supports multiple formats\. See [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md) for the formats that are supported\.
+ `format_options` – Format options for the specified format\. See [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md) for the formats that are supported\.
+ `transformation_ctx` – The transformation context to use \(optional\)\.
+ `push_down_predicate` – Filters partitions without having to list and read all the files in your dataset\. For more information, see [Pre\-Filtering Using Pushdown Predicates](aws-glue-programming-etl-partitions.md#aws-glue-programming-etl-partitions-pushdowns)\.

## from\_catalog<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-reader-from_catalog"></a>

**`from_catalog(name_space, table_name, redshift_tmp_dir="", transformation_ctx="", push_down_predicate="", additional_options={})`**

Reads a `DynamicFrame` using the specified catalog namespace and table name\.
+ `name_space` – The database to read from\.
+ `table_name` – The name of the table to read from\.
+ `redshift_tmp_dir` – An Amazon Redshift temporary directory to use \(optional\)\.
+ `transformation_ctx` – The transformation context to use \(optional\)\.
+ `push_down_predicate` – Filters partitions without having to list and read all the files in your dataset\. For more information, see [Pre\-Filtering Using Pushdown Predicates](aws-glue-programming-etl-partitions.md#aws-glue-programming-etl-partitions-pushdowns)\.
+ `additional_options` – Additional options provided to AWS Glue\. To use a JDBC connection that performs parallel reads, you can set the `hashfield`, `hashexpression`, or `hashpartitions` options\. For example:

  ```
  additional_options = {"hashfield": "month"} 
  ```

  For more information, see [Reading from JDBC Tables in Parallel](run-jdbc-parallel-read-job.md)\. 