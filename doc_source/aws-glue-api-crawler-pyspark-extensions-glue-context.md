# GlueContext Class<a name="aws-glue-api-crawler-pyspark-extensions-glue-context"></a>

Wraps the Apache SparkSQL [SQLContext](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html#pyspark.sql.SQLContext) object, and thereby provides mechanisms for interacting with the Apache Spark platform\.

## Creating<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-_creating"></a>
+ [\_\_init\_\_](#aws-glue-api-crawler-pyspark-extensions-glue-context-__init__)
+ [getSource](#aws-glue-api-crawler-pyspark-extensions-glue-context-get-source)
+ [create\_dynamic\_frame\_from\_rdd](#aws-glue-api-crawler-pyspark-extensions-glue-context-create_dynamic_frame_from_rdd)
+ [create\_dynamic\_frame\_from\_catalog](#aws-glue-api-crawler-pyspark-extensions-glue-context-create_dynamic_frame_from_catalog)
+ [create\_dynamic\_frame\_from\_options](#aws-glue-api-crawler-pyspark-extensions-glue-context-create_dynamic_frame_from_options)

## \_\_init\_\_<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-__init__"></a>

**`__init__(sparkContext)`**
+ `sparkContext` – The Apache Spark context to use\.

## getSource<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-get-source"></a>

**`getSource(connection_type, transformation_ctx = "", **options)`**

Creates a `DataSource` object that can be used to read `DynamicFrames` from external sources\.
+ `connection_type` – The connection type to use, such as Amazon S3, Amazon Redshift, and JDBC\. Valid values include `s3`, `mysql`, `postgresql`, `redshift`, `sqlserver`, `oracle`, and `dynamodb`\.
+ `transformation_ctx` – The transformation context to use \(optional\)\.
+ `options` – A collection of optional name\-value pairs\. For more information, see [Connection Types and Options for ETL in AWS Glue](aws-glue-programming-etl-connect.md)\.

The following is an example of using `getSource`:

```
>>> data_source = context.getSource("file", paths=["/in/path"])
>>> data_source.setFormat("json")
>>> myFrame = data_source.getFrame()
```

## create\_dynamic\_frame\_from\_rdd<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-create_dynamic_frame_from_rdd"></a>

**`create_dynamic_frame_from_rdd(data, name, schema=None, sample_ratio=None, transformation_ctx="")`**

Returns a `DynamicFrame` that is created from an Apache Spark Resilient Distributed Dataset \(RDD\)\.
+ `data` – The data source to use\.
+ `name` – The name of the data to use\.
+ `schema` – The schema to use \(optional\)\.
+ `sample_ratio` – The sample ratio to use \(optional\)\.
+ `transformation_ctx` – The transformation context to use \(optional\)\.

## create\_dynamic\_frame\_from\_catalog<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-create_dynamic_frame_from_catalog"></a>

**`create_dynamic_frame_from_catalog(database, table_name, redshift_tmp_dir, transformation_ctx = "", push_down_predicate= "", additional_options = {}, catalog_id = None)`**

Returns a `DynamicFrame` that is created using a catalog database and table name\.
+ `Database` – The database to read from\.
+ `table_name` – The name of the table to read from\.
+ `redshift_tmp_dir` – An Amazon Redshift temporary directory to use \(optional\)\.
+ `transformation_ctx` – The transformation context to use \(optional\)\.
+ `push_down_predicate` – Filters partitions without having to list and read all the files in your dataset\. For more information, see [Pre\-Filtering Using Pushdown Predicates](aws-glue-programming-etl-partitions.md#aws-glue-programming-etl-partitions-pushdowns)\.
+ `additional_options` – Additional options provided to AWS Glue\.
+ `catalog_id` — The catalog ID \(account ID\) of the Data Catalog being accessed\. When None, the default account ID of the caller is used\. 

## create\_dynamic\_frame\_from\_options<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-create_dynamic_frame_from_options"></a>

**`create_dynamic_frame_from_options(connection_type, connection_options={}, format=None, format_options={}, transformation_ctx = "", push_down_predicate= "")`**

Returns a `DynamicFrame` created with the specified connection and format\.
+ `connection_type` – The connection type, such as Amazon S3, Amazon Redshift, and JDBC\. Valid values include `s3`, `mysql`, `postgresql`, `redshift`, `sqlserver`, `oracle`, and `dynamodb`\.
+ `connection_options` – Connection options, such as paths and database table \(optional\)\. For a `connection_type` of `s3`, a list of Amazon S3 paths is defined\.

  ```
  connection_options = {"paths": ["s3://aws-glue-target/temp"]}
  ```

  For JDBC connections, several properties must be defined\. Note that the database name must be part of the URL\. It can optionally be included in the connection options\.

  ```
  connection_options = {"url": "jdbc-url/database", "user": "username", "password": "password","dbtable": "table-name", "redshiftTmpDir": "s3-tempdir-path"} 
  ```

  The `dbtable` property is the name of the JDBC table\. For JDBC data stores that support schemas within a database, specify `schema.table-name`\. If a schema is not provided, then the default "public" schema is used\.

  For more information, see [Connection Types and Options for ETL in AWS Glue](aws-glue-programming-etl-connect.md)\.
+ `format` – A format specification \(optional\)\. This is used for an Amazon Simple Storage Service \(Amazon S3\) or an AWS Glue connection that supports multiple formats\. See [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md) for the formats that are supported\.
+ `format_options` – Format options for the specified format\. See [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md) for the formats that are supported\.
+ `transformation_ctx` – The transformation context to use \(optional\)\.
+ `push_down_predicate` – Filters partitions without having to list and read all the files in your dataset\. AWS Glue supports predicate pushdown only for Amazon S3 sources; and does not support JDBC sources\. For more information, see [Pre\-Filtering Using Pushdown Predicates](aws-glue-programming-etl-partitions.md#aws-glue-programming-etl-partitions-pushdowns)\.

## Writing<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-_writing"></a>
+ [getSink](#aws-glue-api-crawler-pyspark-extensions-glue-context-get-sink)
+ [write\_dynamic\_frame\_from\_options](#aws-glue-api-crawler-pyspark-extensions-glue-context-write_dynamic_frame_from_options)
+ [write\_from\_options](#aws-glue-api-crawler-pyspark-extensions-glue-context-write_from_options)
+ [write\_dynamic\_frame\_from\_catalog](#aws-glue-api-crawler-pyspark-extensions-glue-context-write_dynamic_frame_from_catalog)
+ [write\_dynamic\_frame\_from\_jdbc\_conf](#aws-glue-api-crawler-pyspark-extensions-glue-context-write_dynamic_frame_from_jdbc_conf)
+ [write\_from\_jdbc\_conf](#aws-glue-api-crawler-pyspark-extensions-glue-context-write_from_jdbc_conf)

## getSink<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-get-sink"></a>

**`getSink(connection_type, format = None, transformation_ctx = "", **options)`**

Gets a `DataSink` object that can be used to write `DynamicFrames` to external sources\. Check the SparkSQL `format` first to be sure to get the expected sink\.
+ `connection_type` – The connection type to use, such as Amazon S3, Amazon Redshift, and JDBC\. Valid values include `s3`, `mysql`, `postgresql`, `redshift`, `sqlserver`, and `oracle`\.
+ `format` – The SparkSQL format to use \(optional\)\.
+ `transformation_ctx` – The transformation context to use \(optional\)\.
+ `options` – A collection of option name\-value pairs\.

For example:

```
>>> data_sink = context.getSink("s3")
>>> data_sink.setFormat("json"),
>>> data_sink.writeFrame(myFrame)
```

## write\_dynamic\_frame\_from\_options<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-write_dynamic_frame_from_options"></a>

**`write_dynamic_frame_from_options(frame, connection_type, connection_options={}, format=None, format_options={}, transformation_ctx = "")`**

Writes and returns a `DynamicFrame` using the specified connection and format\.
+ `frame` – The `DynamicFrame` to write\.
+ `connection_type` – The connection type, such as Amazon S3, Amazon Redshift, and JDBC\. Valid values include `s3`, `mysql`, `postgresql`, `redshift`, `sqlserver`, and `oracle`\.
+ `connection_options` – Connection options, such as path and database table \(optional\)\. For a `connection_type` of `s3`, an Amazon S3 path is defined\.

  ```
  connection_options = {"path": "s3://aws-glue-target/temp"}
  ```

  For JDBC connections, several properties must be defined\. Note that the database name must be part of the URL\. It can optionally be included in the connection options\.

  ```
  connection_options = {"url": "jdbc-url/database", "user": "username", "password": "password","dbtable": "table-name", "redshiftTmpDir": "s3-tempdir-path"} 
  ```

  The `dbtable` property is the name of the JDBC table\. For JDBC data stores that support schemas within a database, specify `schema.table-name`\. If a schema is not provided, then the default "public" schema is used\.

  For more information, see [Connection Types and Options for ETL in AWS Glue](aws-glue-programming-etl-connect.md)\.
+ `format` – A format specification \(optional\)\. This is used for an Amazon Simple Storage Service \(Amazon S3\) or an AWS Glue connection that supports multiple formats\. See [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md) for the formats that are supported\.
+ `format_options` – Format options for the specified format\. See [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md) for the formats that are supported\.
+ `transformation_ctx` – A transformation context to use \(optional\)\.

## write\_from\_options<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-write_from_options"></a>

**`write_from_options(frame_or_dfc, connection_type, connection_options={}, format={}, format_options={}, transformation_ctx = "")`**

Writes and returns a `DynamicFrame` or `DynamicFrameCollection` that is created with the specified connection and format information\.
+ `frame_or_dfc` – The `DynamicFrame` or `DynamicFrameCollection` to write\.
+ `connection_type` – The connection type, such as Amazon S3, Amazon Redshift, and JDBC\. Valid values include `s3`, `mysql`, `postgresql`, `redshift`, `sqlserver`, and `oracle`\.
+ `connection_options` – Connection options, such as path and database table \(optional\)\. For a `connection_type` of `s3`, an Amazon S3 path is defined\.

  ```
  connection_options = {"path": "s3://aws-glue-target/temp"}
  ```

  For JDBC connections, several properties must be defined\. Note that the database name must be part of the URL\. It can optionally be included in the connection options\.

  ```
  connection_options = {"url": "jdbc-url/database", "user": "username", "password": "password","dbtable": "table-name", "redshiftTmpDir": "s3-tempdir-path"} 
  ```

  The `dbtable` property is the name of the JDBC table\. For JDBC data stores that support schemas within a database, specify `schema.table-name`\. If a schema is not provided, then the default "public" schema is used\.

  For more information, see [Connection Types and Options for ETL in AWS Glue](aws-glue-programming-etl-connect.md)\.
+ `format` – A format specification \(optional\)\. This is used for an Amazon Simple Storage Service \(Amazon S3\) or an AWS Glue connection that supports multiple formats\. See [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md) for the formats that are supported\.
+ `format_options` – Format options for the specified format\. See [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md) for the formats that are supported\.
+ `transformation_ctx` – A transformation context to use \(optional\)\.

## write\_dynamic\_frame\_from\_catalog<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-write_dynamic_frame_from_catalog"></a>

**`write_dynamic_frame_from_catalog(frame, database, table_name, redshift_tmp_dir, transformation_ctx = "", addtional_options = {}, catalog_id = None)`**

Writes and returns a `DynamicFrame` using a catalog database and a table name\.
+ `frame` – The `DynamicFrame` to write\.
+ `Database` – The database to read from\.
+ `table_name` – The name of the table to read from\.
+ `redshift_tmp_dir` – An Amazon Redshift temporary directory to use \(optional\)\.
+ `transformation_ctx` – The transformation context to use \(optional\)\.
+ `catalog_id` — The catalog ID \(account ID\) of the Data Catalog being accessed\. When None, the default account ID of the caller is used\. 

## write\_dynamic\_frame\_from\_jdbc\_conf<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-write_dynamic_frame_from_jdbc_conf"></a>

**`write_dynamic_frame_from_jdbc_conf(frame, catalog_connection, connection_options={}, redshift_tmp_dir = "", transformation_ctx = "", catalog_id = None)`**

Writes and returns a `DynamicFrame` using the specified JDBC connection information\.
+ `frame` – The `DynamicFrame` to write\.
+ `catalog_connection` – A catalog connection to use\.
+ `connection_options` – Connection options, such as path and database table \(optional\)\. For more information, see [Connection Types and Options for ETL in AWS Glue](aws-glue-programming-etl-connect.md)\.
+ `redshift_tmp_dir` – An Amazon Redshift temporary directory to use \(optional\)\.
+ `transformation_ctx` – A transformation context to use \(optional\)\.
+ `catalog_id` — The catalog ID \(account ID\) of the Data Catalog being accessed\. When None, the default account ID of the caller is used\. 

## write\_from\_jdbc\_conf<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-write_from_jdbc_conf"></a>

**`write_from_jdbc_conf(frame_or_dfc, catalog_connection, connection_options={}, redshift_tmp_dir = "", transformation_ctx = "", catalog_id = None)`**

Writes and returns a `DynamicFrame` or `DynamicFrameCollection` using the specified JDBC connection information\.
+ `frame_or_dfc` – The `DynamicFrame` or `DynamicFrameCollection` to write\.
+ `catalog_connection` – A catalog connection to use\.
+ `connection_options` – Connection options, such as path and database table \(optional\)\. For more information, see [Connection Types and Options for ETL in AWS Glue](aws-glue-programming-etl-connect.md)\.
+ `redshift_tmp_dir` – An Amazon Redshift temporary directory to use \(optional\)\.
+ `transformation_ctx` – A transformation context to use \(optional\)\.
+ `catalog_id` — The catalog ID \(account ID\) of the Data Catalog being accessed\. When None, the default account ID of the caller is used\. 

## Extracting<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-_extracting"></a>
+ [extract\_jdbc\_conf](#aws-glue-api-crawler-pyspark-extensions-glue-context-extract_jdbc_conf)

## extract\_jdbc\_conf<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-extract_jdbc_conf"></a>

**`extract_jdbc_conf(connection_name, catalog_id = None)`**

Returns a `dict` with keys `user`, `password`, `vendor`, and `url` from the connection object in the Data Catalog\.
+ `connection_name` – The name of the connection in the Data Catalog
+ `catalog_id` — The catalog ID \(account ID\) of the Data Catalog being accessed\. When None, the default account ID of the caller is used\. 