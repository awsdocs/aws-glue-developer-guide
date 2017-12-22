# DynamicFrameWriter Class<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-writer"></a>

##  Methods<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-writer-_methods"></a>

+ [\_\_init\_\_](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-writer-__init__)

+ [from\_options](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-writer-from_options)

+ [from\_catalog](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-writer-from_catalog)

+ [from\_jdbc\_conf](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-writer-from_jdbc_conf)

## \_\_init\_\_<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-writer-__init__"></a>

**`__init__(glue_context)`**

+ `glue_context` – The [GlueContext Class](aws-glue-api-crawler-pyspark-extensions-glue-context.md) to use\.

## from\_options<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-writer-from_options"></a>

**`from_options(frame, connection_type, connection_options={}, format=None, format_options={}, transformation_ctx="")`**

Writes a `DynamicFrame` using the specified connection and format\.

+ `frame` – The `DynamicFrame` to write\.

+ `connection_type` – The connection type\. Valid values include `s3`, `mysql`, `postgresql`, `redshift`, `sqlserver`, and `oracle`\.

+ `connection_options` – Connection options, such as path and database table \(optional\)\. For a `connection_type` of `s3`, an Amazon S3 path is defined\. For example, `connection_options = {"path": "s3://aws-glue-target/temp"}`\.

+ `format` – A format specification such as JSON, CSV, or other format \(optional\)\. This is used for an Amazon Simple Storage Service \(Amazon S3\) or tape connection that supports multiple formats\.

+ `format_options` – Format options, such as delimiter \(optional\)\.

+ `transformation_ctx` – A transformation context to use \(optional\)\.

## from\_catalog<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-writer-from_catalog"></a>

**`from_catalog(frame, name_space, table_name, redshift_tmp_dir="", transformation_ctx="")`**

Writes a `DynamicFrame` using the specified catalog database and table name\.

+ `frame` – The `DynamicFrame` to write\.

+ `name_space` – The database to use\.

+ `table_name` – The `table_name` to use\.

+ `redshift_tmp_dir` – An Amazon Redshift temporary directory to use \(optional\)\.

+ `transformation_ctx` – A transformation context to use \(optional\)\.

## from\_jdbc\_conf<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-writer-from_jdbc_conf"></a>

**`from_jdbc_conf(frame, catalog_connection, connection_options={}, redshift_tmp_dir = "", transformation_ctx="")`**

Writes a `DynamicFrame` using the specified JDBC connection information\.

+ `frame` – The `DynamicFrame` to write\.

+ `catalog_connection` – A catalog connection to use\.

+ `connection_options` – Connection options, such as path and database table \(optional\)\.

+ `redshift_tmp_dir` – An Amazon Redshift temporary directory to use \(optional\)\.

+ `transformation_ctx` – A transformation context to use \(optional\)\.