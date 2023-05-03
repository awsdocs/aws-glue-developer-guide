# GlueContext Class<a name="aws-glue-api-crawler-pyspark-extensions-glue-context"></a>

Wraps the Apache Spark [SparkContext](https://spark.apache.org/docs/latest/api/java/org/apache/spark/SparkContext.html) object, and thereby provides mechanisms for interacting with the Apache Spark platform\.

## Working with Datasets in Amazon S3<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-_storage_layer"></a>
+ [purge\_table](#aws-glue-api-crawler-pyspark-extensions-glue-context-purge_table)
+ [purge\_s3\_path](#aws-glue-api-crawler-pyspark-extensions-glue-context-purge_s3_path)
+ [transition\_table](#aws-glue-api-crawler-pyspark-extensions-glue-context-transition_table)
+ [transition\_s3\_path](#aws-glue-api-crawler-pyspark-extensions-glue-context-transition_s3_path)

## purge\_table<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-purge_table"></a>

**`purge_table(catalog_id=None, database="", table_name="", options={}, transformation_ctx="")`**

Deletes files from Amazon S3 for the specified catalog's database and table\. If all files in a partition are deleted, that partition is also deleted from the catalog\.

If you want to be able to recover deleted objects, you can turn on [object versioning](https://docs.aws.amazon.com/AmazonS3/latest/dev/ObjectVersioning.html) on the Amazon S3 bucket\. When an object is deleted from a bucket that doesn't have object versioning enabled, the object can't be recovered\. For more information about how to recover deleted objects in a version\-enabled bucket, see [How can I retrieve an Amazon S3 object that was deleted?](https://aws.amazon.com/premiumsupport/knowledge-center/s3-undelete-configuration/) in the AWS Support Knowledge Center\.
+ `catalog_id` – The catalog ID of the Data Catalog being accessed \(the account ID of the Data Catalog\)\. Set to `None` by default\. `None` defaults to the catalog ID of the calling account in the service\.
+ `database` – The database to use\.
+ `table_name` – The name of the table to use\.
+ `options` – Options to filter files to be deleted and for manifest file generation\.
  + `retentionPeriod` – Specifies a period in number of hours to retain files\. Files newer than the retention period are retained\. Set to 168 hours \(7 days\) by default\.
  + `partitionPredicate` – Partitions satisfying this predicate are deleted\. Files within the retention period in these partitions are not deleted\. Set to `""` – empty by default\.
  + `excludeStorageClasses` – Files with storage class in the `excludeStorageClasses` set are not deleted\. The default is `Set()` – an empty set\.
  + `manifestFilePath` – An optional path for manifest file generation\. All files that were successfully purged are recorded in `Success.csv`, and those that failed in `Failed.csv`
+ `transformation_ctx` – The transformation context to use \(optional\)\. Used in the manifest file path\.

**Example**  

```
glueContext.purge_table("database", "table", {"partitionPredicate": "(month=='march')", "retentionPeriod": 1, "excludeStorageClasses": ["STANDARD_IA"], "manifestFilePath": "s3://bucketmanifest/"})
```

## purge\_s3\_path<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-purge_s3_path"></a>

**`purge_s3_path(s3_path, options={}, transformation_ctx="")`**

Deletes files from the specified Amazon S3 path recursively\.

If you want to be able to recover deleted objects, you can turn on [object versioning](https://docs.aws.amazon.com/AmazonS3/latest/dev/ObjectVersioning.html) on the Amazon S3 bucket\. When an object is deleted from a bucket that doesn't have object versioning turned on, the object can't be recovered\. For more information about how to recover deleted objects in a bucket with versioning, see [How can I retrieve an Amazon S3 object that was deleted?](https://aws.amazon.com/premiumsupport/knowledge-center/s3-undelete-configuration/) in the AWS Support Knowledge Center\.
+ `s3_path` – The path in Amazon S3 of the files to be deleted in the format `s3://<bucket>/<prefix>/`
+ `options` – Options to filter files to be deleted and for manifest file generation\.
  + `retentionPeriod` – Specifies a period in number of hours to retain files\. Files newer than the retention period are retained\. Set to 168 hours \(7 days\) by default\.
  + `partitionPredicate` – Partitions satisfying this predicate are deleted\. Files within the retention period in these partitions are not deleted\. Set to `""` – empty by default\.
  + `excludeStorageClasses` – Files with storage class in the `excludeStorageClasses` set are not deleted\. The default is `Set()` – an empty set\.
  + `manifestFilePath` – An optional path for manifest file generation\. All files that were successfully purged are recorded in `Success.csv`, and those that failed in `Failed.csv`
+ `transformation_ctx` – The transformation context to use \(optional\)\. Used in the manifest file path\.

**Example**  

```
glueContext.purge_s3_path("s3://bucket/path/", {"retentionPeriod": 1, "excludeStorageClasses": ["STANDARD_IA"], "manifestFilePath": "s3://bucketmanifest/"})
```

## transition\_table<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-transition_table"></a>

**`transition_table(database, table_name, transition_to, options={}, transformation_ctx="", catalog_id=None)`**

Transitions the storage class of the files stored on Amazon S3 for the specified catalog's database and table\.

You can transition between any two storage classes\. For the `GLACIER` and `DEEP_ARCHIVE` storage classes, you can transition to these classes\. However, you would use an `S3 RESTORE` to transition from `GLACIER` and `DEEP_ARCHIVE` storage classes\.

If you're running AWS Glue ETL jobs that read files or partitions from Amazon S3, you can exclude some Amazon S3 storage class types\. For more information, see [Excluding Amazon S3 Storage Classes](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-storage-classes.html)\.
+ `database` – The database to use\.
+ `table_name` – The name of the table to use\.
+ `transition_to` – The [Amazon S3 storage class](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/s3/model/StorageClass.html) to transition to\.
+ `options` – Options to filter files to be deleted and for manifest file generation\.
  + `retentionPeriod` – Specifies a period in number of hours to retain files\. Files newer than the retention period are retained\. Set to 168 hours \(7 days\) by default\.
  + `partitionPredicate` – Partitions satisfying this predicate are transitioned\. Files within the retention period in these partitions are not transitioned\. Set to `""` – empty by default\.
  + `excludeStorageClasses` – Files with storage class in the `excludeStorageClasses` set are not transitioned\. The default is `Set()` – an empty set\.
  + `manifestFilePath` – An optional path for manifest file generation\. All files that were successfully transitioned are recorded in `Success.csv`, and those that failed in `Failed.csv`
  + `accountId` – The Amazon Web Services account ID to run the transition transform\. Mandatory for this transform\.
  + `roleArn` – The AWS role to run the transition transform\. Mandatory for this transform\.
+ `transformation_ctx` – The transformation context to use \(optional\)\. Used in the manifest file path\.
+ `catalog_id` – The catalog ID of the Data Catalog being accessed \(the account ID of the Data Catalog\) as string type\. Set to `None` by default\. `None` defaults to the catalog ID of the calling account in the service\.

**Example**  

```
glueContext.transition_table("database", "table", "STANDARD_IA", {"retentionPeriod": 1, "excludeStorageClasses": ["STANDARD_IA"], "manifestFilePath": "s3://bucketmanifest/", "accountId": "12345678901", "roleArn": "arn:aws:iam::123456789012:user/example-username"})
```

## transition\_s3\_path<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-transition_s3_path"></a>

**`transition_s3_path(s3_path, transition_to, options={}, transformation_ctx="")`**

Transitions the storage class of the files in the specified Amazon S3 path recursively\.

You can transition between any two storage classes\. For the `GLACIER` and `DEEP_ARCHIVE` storage classes, you can transition to these classes\. However, you would use an `S3 RESTORE` to transition from `GLACIER` and `DEEP_ARCHIVE` storage classes\.

If you're running AWS Glue ETL jobs that read files or partitions from Amazon S3, you can exclude some Amazon S3 storage class types\. For more information, see [Excluding Amazon S3 Storage Classes](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-storage-classes.html)\.
+ `s3_path` – The path in Amazon S3 of the files to be transitioned in the format `s3://<bucket>/<prefix>/`
+ `transition_to` – The [Amazon S3 storage class](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/s3/model/StorageClass.html) to transition to\.
+ `options` – Options to filter files to be deleted and for manifest file generation\.
  + `retentionPeriod` – Specifies a period in number of hours to retain files\. Files newer than the retention period are retained\. Set to 168 hours \(7 days\) by default\.
  + `partitionPredicate` – Partitions satisfying this predicate are transitioned\. Files within the retention period in these partitions are not transitioned\. Set to `""` – empty by default\.
  + `excludeStorageClasses` – Files with storage class in the `excludeStorageClasses` set are not transitioned\. The default is `Set()` – an empty set\.
  + `manifestFilePath` – An optional path for manifest file generation\. All files that were successfully transitioned are recorded in `Success.csv`, and those that failed in `Failed.csv`
  + `accountId` – The Amazon Web Services account ID to run the transition transform\. Mandatory for this transform\.
  + `roleArn` – The AWS role to run the transition transform\. Mandatory for this transform\.
+ `transformation_ctx` – The transformation context to use \(optional\)\. Used in the manifest file path\.

**Example**  

```
glueContext.transition_s3_path("s3://bucket/prefix/", "STANDARD_IA", {"retentionPeriod": 1, "excludeStorageClasses": ["STANDARD_IA"], "manifestFilePath": "s3://bucketmanifest/", "accountId": "12345678901", "roleArn": "arn:aws:iam::123456789012:user/example-username"})
```

## Creating<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-_creating"></a>
+ [\_\_init\_\_](#aws-glue-api-crawler-pyspark-extensions-glue-context-__init__)
+ [getSource](#aws-glue-api-crawler-pyspark-extensions-glue-context-get-source)
+ [create\_dynamic\_frame\_from\_rdd](#aws-glue-api-crawler-pyspark-extensions-glue-context-create_dynamic_frame_from_rdd)
+ [create\_dynamic\_frame\_from\_catalog](#aws-glue-api-crawler-pyspark-extensions-glue-context-create_dynamic_frame_from_catalog)
+ [create\_dynamic\_frame\_from\_options](#aws-glue-api-crawler-pyspark-extensions-glue-context-create_dynamic_frame_from_options)
+ [add\_ingestion\_time\_columns](#aws-glue-api-crawler-pyspark-extensions-glue-context-add-ingestion-time-columns)
+ [create\_data\_frame\_from\_catalog](#aws-glue-api-crawler-pyspark-extensions-glue-context-create-dataframe-from-catalog)
+ [create\_data\_frame\_from\_options](#aws-glue-api-crawler-pyspark-extensions-glue-context-create-dataframe-from-options)

## \_\_init\_\_<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-__init__"></a>

**`__init__(sparkContext)`**
+ `sparkContext` – The Apache Spark context to use\.

## getSource<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-get-source"></a>

**`getSource(connection_type, transformation_ctx = "", **options)`**

Creates a `DataSource` object that can be used to read `DynamicFrames` from external sources\.
+ `connection_type` – The connection type to use, such as Amazon Simple Storage Service \(Amazon S3\), Amazon Redshift, and JDBC\. Valid values include `s3`, `mysql`, `postgresql`, `redshift`, `sqlserver`, `oracle`, and `dynamodb`\.
+ `transformation_ctx` – The transformation context to use \(optional\)\.
+ `options` – A collection of optional name\-value pairs\. For more information, see [Connection Types and Options for ETL in AWS Glue](aws-glue-programming-etl-connect.md)\.

The following is an example of using `getSource`\.

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

Returns a `DynamicFrame` that is created using a Data Catalog database and table name\.
+ `Database` – The database to read from\.
+ `table_name` – The name of the table to read from\.
+ `redshift_tmp_dir` – An Amazon Redshift temporary directory to use \(optional\)\.
+ `transformation_ctx` – The transformation context to use \(optional\)\.
+ `push_down_predicate` – Filters partitions without having to list and read all the files in your dataset\. For more information, see [Pre\-Filtering Using Pushdown Predicates](aws-glue-programming-etl-partitions.md#aws-glue-programming-etl-partitions-pushdowns)\.
+ `additional_options` – A collection of optional name\-value pairs\. The possible options include those listed in [Connection Types and Options for ETL in AWS Glue](aws-glue-programming-etl-connect.md) except for `endpointUrl`, `streamName`, `bootstrap.servers`, `security.protocol`, `topicName`, `classification`, and `delimiter`\.
+ `catalog_id` — The catalog ID \(account ID\) of the Data Catalog being accessed\. When None, the default account ID of the caller is used\. 

## create\_dynamic\_frame\_from\_options<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-create_dynamic_frame_from_options"></a>

**`create_dynamic_frame_from_options(connection_type, connection_options={}, format=None, format_options={}, transformation_ctx = "")`**

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
+ `format` – A format specification \(optional\)\. This is used for an Amazon S3 or an AWS Glue connection that supports multiple formats\. See [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md) for the formats that are supported\.
+ `format_options` – Format options for the specified format\. See [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md) for the formats that are supported\.
+ `transformation_ctx` – The transformation context to use \(optional\)\.

## add\_ingestion\_time\_columns<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-add-ingestion-time-columns"></a>

**`add_ingestion_time_columns(dataFrame, timeGranularity = "")`**

Appends ingestion time columns like `ingest_year`, `ingest_month`, `ingest_day`, `ingest_hour`, `ingest_minute` to the input `DataFrame`\. This function is automatically generated in the script generated by the AWS Glue when you specify a Data Catalog table with Amazon S3 as the target\. This function automatically updates the partition with ingestion time columns on the output table\. This allows the output data to be automatically partitioned on ingestion time without requiring explicit ingestion time columns in the input data\.
+ `dataFrame` – The `dataFrame` to append the ingestion time columns to\.
+ `timeGranularity` – The granularity of the time columns\. Valid values are "`day`", "`hour`" and "`minute`"\. For example, if "`hour`" is passed in to the function, the original `dataFrame` will have "`ingest_year`", "`ingest_month`", "`ingest_day`", and "`ingest_hour`" time columns appended\.

Returns the data frame after appending the time granularity columns\.

Example:

```
dynamic_frame = DynamicFrame.fromDF(glueContext.add_ingestion_time_columns(dataFrame, "hour"))
```

## create\_data\_frame\_from\_catalog<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-create-dataframe-from-catalog"></a>

**`create_data_frame_from_catalog(database, table_name, transformation_ctx = "", additional_options = {})`**

Returns a `DataFrame` that is created using information from a Data Catalog table\. Use this function only with AWS Glue streaming sources\.
+ `database` – The Data Catalog database to read from\.
+ `table_name` – The name of the Data Catalog table to read from\.
+ `transformation_ctx` – The transformation context to use \(optional\)\.
+ `additional_options` – A collection of optional name\-value pairs\. The possible options include those listed in [Connection Types and Options for ETL in AWS Glue](aws-glue-programming-etl-connect.md) for streaming sources, such as `startingPosition`, `maxFetchTimeInMs`, and `startingOffsets`\.

**Example:**

```
df = glueContext.create_data_frame.from_catalog( 
    database = "MyDB", 
    table_name = "streaming_table", 
    transformation_ctx = "df", 
    additional_options = {"startingPosition": "TRIM_HORIZON", "inferSchema": "true"})
```

## create\_data\_frame\_from\_options<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-create-dataframe-from-options"></a>

**`create_data_frame_from_options(connection_type, connection_options={}, format=None, format_options={}, transformation_ctx = "")`**

Returns a `DataFrame` created with the specified connection and format\. Use this function only with AWS Glue streaming sources\.
+ `connection_type` – The streaming connection type\. Valid values include `kinesis` and `kafka`\.
+ `connection_options` – Connection options, which are different for Kinesis and Kafka\. You can find the list of all connection options for each streaming data source at [Connection Types and Options for ETL in AWS Glue](aws-glue-programming-etl-connect.md)\. Note the following differences in streaming connection options:
  + Kinesis streaming sources require `streamARN`, `startingPosition`, `inferSchema`, and `classification`\.
  + Kafka streaming sources require `connectionName`, `topicName`, `startingOffsets`, `inferSchema`, and `classification`\.
+ `format` – A format specification \(optional\)\. This is used for an Amazon S3 or an AWS Glue connection that supports multiple formats\. For information about the supported formats, see [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md)\.
+ `format_options` – Format options for the specified format\. For information about the supported format options, see [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md)\.
+ `transformation_ctx` – The transformation context to use \(optional\)\.

Example for Amazon Kinesis streaming source:

```
kinesis_options =
   { "streamARN": "arn:aws:kinesis:us-east-2:777788889999:stream/fromOptionsStream",
     "startingPosition": "TRIM_HORIZON", 
     "inferSchema": "true", 
     "classification": "json" 
   }
data_frame_datasource0 = glueContext.create_data_frame.from_options(connection_type="kinesis", connection_options=kinesis_options)
```

Example for Kafka streaming source:

```
kafka_options =
    { "connectionName": "ConfluentKafka", 
      "topicName": "kafka-auth-topic", 
      "startingOffsets": "earliest", 
      "inferSchema": "true", 
      "classification": "json" 
    }
data_frame_datasource0 = glueContext.create_data_frame.from_options(connection_type="kafka", connection_options=kafka_options)
```

## forEachBatch<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-forEachBatch"></a>

**`forEachBatch(frame, batch_function, options)`**

Applies the `batch_function` passed in to every micro batch that is read from the Streaming source\.
+ `frame` – The DataFrame containing the current micro batch\.
+ `batch_function` – A function that will be applied for every micro batch\.
+ `options` – A collection of key\-value pairs that holds information about how to process micro batches\. The following options are required:
  + `windowSize` – The amount of time to spend processing each batch\.
  + `checkpointLocation` – The location where checkpoints are stored for the streaming ETL job\.

**Example:**

```
glueContext.forEachBatch(
    frame = data_frame_datasource0,
    batch_function = processBatch, 
    options = {
        "windowSize": "100 seconds", 
        "checkpointLocation": "s3://kafka-auth-dataplane/confluent-test/output/checkpoint/"
    }
)
   
def processBatch(data_frame, batchId):
    if (data_frame.count() > 0):
        datasource0 = DynamicFrame.fromDF(
          glueContext.add_ingestion_time_columns(data_frame, "hour"), 
          glueContext, "from_data_frame"
        )
        additionalOptions_datasink1 = {"enableUpdateCatalog": True}
        additionalOptions_datasink1["partitionKeys"] = ["ingest_yr", "ingest_mo", "ingest_day"]
        datasink1 = glueContext.write_dynamic_frame.from_catalog(
          frame = datasource0, 
          database = "tempdb", 
          table_name = "kafka-auth-table-output", 
          transformation_ctx = "datasink1", 
          additional_options = additionalOptions_datasink1
        )
```

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
+ `format` – A format specification \(optional\)\. This is used for an Amazon S3 or an AWS Glue connection that supports multiple formats\. See [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md) for the formats that are supported\.
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
+ `format` – A format specification \(optional\)\. This is used for an Amazon S3 or an AWS Glue connection that supports multiple formats\. See [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md) for the formats that are supported\.
+ `format_options` – Format options for the specified format\. See [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md) for the formats that are supported\.
+ `transformation_ctx` – A transformation context to use \(optional\)\.

## write\_dynamic\_frame\_from\_catalog<a name="aws-glue-api-crawler-pyspark-extensions-glue-context-write_dynamic_frame_from_catalog"></a>

**`write_dynamic_frame_from_catalog(frame, database, table_name, redshift_tmp_dir, transformation_ctx = "", addtional_options = {}, catalog_id = None)`**

Writes and returns a `DynamicFrame` using information from a Data Catalog database and table\.
+ `frame` – The `DynamicFrame` to write\.
+ `Database` – The Data Catalog database that contains the table\.
+ `table_name` – The name of the Data Catalog table associated with the target\.
+ `redshift_tmp_dir` – An Amazon Redshift temporary directory to use \(optional\)\.
+ `transformation_ctx` – The transformation context to use \(optional\)\.
+ `additional_options` – A collection of optional name\-value pairs\.
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
