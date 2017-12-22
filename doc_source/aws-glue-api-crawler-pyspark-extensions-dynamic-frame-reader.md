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

**`from_options(connection_type, connection_options={}, format=None, format_options={}, transformation_ctx="")`**

Reads a `DynamicFrame` using the specified connection and format\.

+ `connection_type` – The connection type\. Valid values include `s3`, `mysql`, `postgresql`, `redshift`, `sqlserver`, and `oracle`\.

+ `connection_options` – Connection options, such as path and database table \(optional\)\. For a `connection_type` of `s3`, Amazon S3 paths are defined in an array\. For example, `connection_options = {"paths": [ "s3://mybucket/object_a", "s3://mybucket/object_b"]}`\.

+ `format` – A format specification such as JSON, CSV, or other format \(optional\)\. This is used for an Amazon S3 or tape connection that supports multiple formats\.

+ `format_options` – Format options, such as delimiter \(optional\)\.

+ `transformation_ctx` – The transformation context to use \(optional\)\.

## from\_catalog<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-reader-from_catalog"></a>

**`from_catalog(name_space, table_name, redshift_tmp_dir = "", transformation_ctx="")`**

Reads a `DynamicFrame` using the specified catalog namespace and table name\.

+ `name_space` – The database to read from\.

+ `table_name` – The name of the table to read from\.

+ `redshift_tmp_dir` – An Amazon Redshift temporary directory to use \(optional\)\.

+ `transformation_ctx` – The transformation context to use \(optional\)\.