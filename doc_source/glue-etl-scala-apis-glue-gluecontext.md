# AWS Glue Scala GlueContext APIs<a name="glue-etl-scala-apis-glue-gluecontext"></a>

**Package: com\.amazonaws\.services\.glue**

```
class GlueContext extends SQLContext(sc) (
           @transient val sc : SparkContext,
           val defaultSourcePartitioner : PartitioningStrategy )
```

`GlueContext` is the entry point for reading and writing a [DynamicFrame](glue-etl-scala-apis-glue-dynamicframe.md) from and to Amazon Simple Storage Service \(Amazon S3\), the AWS Glue Data Catalog, JDBC, and so on\. This class provides utility functions to create [DataSource trait](glue-etl-scala-apis-glue-datasource-trait.md) and [DataSink](glue-etl-scala-apis-glue-datasink-class.md) objects that can in turn be used to read and write `DynamicFrame`s\.

You can also use `GlueContext` to set a target number of partitions \(default 20\) in the `DynamicFrame` if the number of partitions created from the source is less than a minimum threshold for partitions \(default 10\)\.

## def getCatalogSink<a name="glue-etl-scala-apis-glue-gluecontext-defs-getCatalogSink"></a>

```
def getCatalogSink( database : String,
                    tableName : String,
                    redshiftTmpDir : String = "",
                    transformationContext : String = ""
                    additionalOptions: JsonOptions = JsonOptions.empty,
                    catalogId: String = null                                
                  ) : DataSink
```

Creates a [DataSink](glue-etl-scala-apis-glue-datasink-class.md) that writes to a location specified in a table that is defined in the Data Catalog\.
+ `database` — The database name in the Data Catalog\.
+ `tableName` — The table name in the Data Catalog\.
+ `redshiftTmpDir` — The temporary staging directory to be used with certain data sinks\. Set to empty by default\.
+ `transformationContext` — The transformation context that is associated with the sink to be used by job bookmarks\. Set to empty by default\.
+ `additionalOptions` – Additional options provided to AWS Glue\. 
+ `catalogId` — The catalog ID \(account ID\) of the Data Catalog being accessed\. When null, the default account ID of the caller is used\. 

Returns the `DataSink`\.

## def getCatalogSource<a name="glue-etl-scala-apis-glue-gluecontext-defs-getCatalogSource"></a>

```
def getCatalogSource( database : String,
                      tableName : String,
                      redshiftTmpDir : String = "",
                      transformationContext : String = ""
                      pushDownPredicate : String = " "
                      additionalOptions: JsonOptions = JsonOptions.empty,
                      catalogId: String = null
                    ) : DataSource
```

Creates a [DataSource trait](glue-etl-scala-apis-glue-datasource-trait.md) that reads data from a table definition in the Data Catalog\.
+ `database` — The database name in the Data Catalog\.
+ `tableName` — The table name in the Data Catalog\.
+ `redshiftTmpDir` — The temporary staging directory to be used with certain data sinks\. Set to empty by default\.
+ `transformationContext` — The transformation context that is associated with the sink to be used by job bookmarks\. Set to empty by default\.
+ `pushDownPredicate` – Filters partitions without having to list and read all the files in your dataset\. For more information, see [Pre\-Filtering Using Pushdown Predicates](aws-glue-programming-etl-partitions.md#aws-glue-programming-etl-partitions-pushdowns)\.
+ `additionalOptions` – Additional options provided to AWS Glue\. 
+ `catalogId` — The catalog ID \(account ID\) of the Data Catalog being accessed\. When null, the default account ID of the caller is used\. 

Returns the `DataSource`\.

## def getJDBCSink<a name="glue-etl-scala-apis-glue-gluecontext-defs-getJDBCSink"></a>

```
def getJDBCSink( catalogConnection : String,
                 options : JsonOptions,
                 redshiftTmpDir : String = "",
                 transformationContext : String = "",
                 catalogId: String = null
               ) : DataSink
```

Creates a [DataSink](glue-etl-scala-apis-glue-datasink-class.md) that writes to a JDBC database that is specified in a `Connection` object in the Data Catalog\. The `Connection` object has information to connect to a JDBC sink, including the URL, user name, password, VPC, subnet, and security groups\.
+ `catalogConnection` — The name of the connection in the Data Catalog that contains the JDBC URL to write to\.
+ `options` — A string of JSON name\-value pairs that provide additional information that is required to write to a JDBC data store\. This includes: 
  + *dbtable* \(required\) — The name of the JDBC table\. For JDBC data stores that support schemas within a database, specify `schema.table-name`\. If a schema is not provided, then the default "public" schema is used\. The following example shows an options parameter that points to a schema named `test` and a table named `test_table` in database `test_db`\.

    ```
    options = JsonOptions("""{"dbtable": "test.test_table", "database": "test_db"}""")
    ```
  + *database* \(required\) — The name of the JDBC database\.
  + Any additional options passed directly to the SparkSQL JDBC writer\. For more information, see [Redshift data source for Spark](https://github.com/databricks/spark-redshift)\.
+ `redshiftTmpDir` — A temporary staging directory to be used with certain data sinks\. Set to empty by default\.
+ `transformationContext` — The transformation context that is associated with the sink to be used by job bookmarks\. Set to empty by default\.
+ `catalogId` — The catalog ID \(account ID\) of the Data Catalog being accessed\. When null, the default account ID of the caller is used\. 

Example code:

```
getJDBCSink(catalogConnection = "my-connection-name", options = JsonOptions("""{"dbtable": "my-jdbc-table", "database": "my-jdbc-db"}"""), redshiftTmpDir = "", transformationContext = "datasink4")
```

Returns the `DataSink`\.

## def getSink<a name="glue-etl-scala-apis-glue-gluecontext-defs-getSink"></a>

```
def getSink( connectionType : String,
             options : JsonOptions,
             transformationContext : String = ""
           ) : DataSink
```

Creates a [DataSink](glue-etl-scala-apis-glue-datasink-class.md) that writes data to a destination like Amazon Simple Storage Service \(Amazon S3\), JDBC, or the AWS Glue Data Catalog\.
+ `connectionType` — The type of the connection\.
+ `options` — A string of JSON name\-value pairs that provide additional information to establish the connection with the data sink\.
+ `transformationContext` — The transformation context that is associated with the sink to be used by job bookmarks\. Set to empty by default\.

Returns the `DataSink`\.

## def getSinkWithFormat<a name="glue-etl-scala-apis-glue-gluecontext-defs-getSinkWithFormat"></a>

```
def getSinkWithFormat( connectionType : String,
                       options : JsonOptions,
                       transformationContext : String = "",
                       format : String = null,
                       formatOptions : JsonOptions = JsonOptions.empty
                     ) : DataSink
```

Creates a [DataSink](glue-etl-scala-apis-glue-datasink-class.md) that writes data to a destination like Amazon S3, JDBC, or the Data Catalog, and also sets the format for the data to be written out to the destination\.
+ `connectionType` — The type of the connection\. Refer to [DataSink](glue-etl-scala-apis-glue-datasink-class.md) for a list of supported connection types\.
+ `options` — A string of JSON name\-value pairs that provide additional information to establish a connection with the data sink\.
+ `transformationContext` — The transformation context that is associated with the sink to be used by job bookmarks\. Set to empty by default\.
+ `format` — The format of the data to be written out to the destination\.
+ `formatOptions` — A string of JSON name\-value pairs that provide additional options for formatting data at the destination\. See [Format Options](aws-glue-programming-etl-format.md)\.

Returns the `DataSink`\.

## def getSource<a name="glue-etl-scala-apis-glue-gluecontext-defs-getSource"></a>

```
def getSource( connectionType : String,
               connectionOptions : JsonOptions,
               transformationContext : String = ""
               pushDownPredicate
             ) : DataSource
```

Creates a [DataSource trait](glue-etl-scala-apis-glue-datasource-trait.md) that reads data from a source like Amazon S3, JDBC, or the AWS Glue Data Catalog\.
+ `connectionType` — The type of the data source\. Can be one of “s3”, “mysql”, “redshift”, “oracle”, “sqlserver”, “postgresql”, "dynamodb", “parquet”, or “orc”\. 
+ `connectionOptions` — A string of JSON name\-value pairs that provide additional information for establishing a connection with the data source\.
  + `connectionOptions` when the `connectionType` is "s3":
    + *paths* \(required\) — List of Amazon S3 paths to read\.
    + *compressionType* \(optional\) — Compression type of the data\. This is generally not required if the data has a standard file extension\. Possible values are “gzip” and “bzip”\.
    + *exclusions* \(optional\) — A string containing a JSON list of glob patterns to exclude\. For example "\[\\"\*\*\.pdf\\"\]" excludes all PDF files\.
    + *maxBand* \(optional\) — This advanced option controls the duration in seconds after which AWS Glue expects an Amazon S3 listing to be consistent\. Files with modification timestamps falling within the last maxBand seconds are tracked when using job bookmarks to account for Amazon S3 eventual consistency\. It is rare to set this option\. The default is 900 seconds\.
    + *maxFilesInBand* \(optional\) — This advanced option specifies the maximum number of files to save from the last maxBand seconds\. If this number is exceeded, extra files are skipped and processed only in the next job run\.
    + *groupFiles* \(optional\) — Grouping files is enabled by default when the input contains more than 50,000 files\. To disable grouping with fewer than 50,000 files, set this parameter to “inPartition”\. To disable grouping when there are more than 50,000 files, set this parameter to “none”\. 
    + *groupSize* \(optional\) — The target group size in bytes\. The default is computed based on the input data size and the size of your cluster\. When there are fewer than 50,000 input files, groupFiles must be set to “inPartition” for this option to take effect\. 
    + *recurse* \(optional\) — If set to true, recursively read files in any subdirectory of the specified paths\.
  + `connectionOptions` when the `connectionType` is "dynamodb":
    + *dynamodb\.input\.tableName* \(required\) — The DynamoDB table from which to read\.
    + *dynamodb\.throughput\.read\.percent* \(optional\) — The percentage of reserved capacity units \(RCU\) to use\. The default is set to "0\.5"\. Acceptable values are from "0\.1" to "1\.5", inclusive\.
  + `connectionOptions` when the `connectionType` is "parquet" or "orc":
    + *paths* \(required\) — List of Amazon S3 paths to read\.
    + Any additional options are passed directly to the SparkSQL DataSource\. For more information, see [Redshift data source for Spark](https://github.com/databricks/spark-redshift)\.
  + `connectionOptions` when the `connectionType` is "redshift":
    + *url* \(required\) — The JDBC URL for an Amazon Redshift database\.
    + *dbtable* \(required\) — The Amazon Redshift table to read\.
    + *redshiftTmpDir* \(required\) — The Amazon S3 path where temporary data can be staged when copying out of Amazon Redshift\.
    + *user* \(required\) — The username to use when connecting to the Amazon Redshift cluster\.
    + *password* \(required\) — The password to use when connecting to the Amazon Redshift cluster\.
+ `transformationContext` — The transformation context that is associated with the sink to be used by job bookmarks\. Set to empty by default\.
+ `pushDownPredicate` — Predicate on partition columns\.

Returns the `DataSource`\.

## def getSourceWithFormat<a name="glue-etl-scala-apis-glue-gluecontext-defs-getSourceWithFormat"></a>

```
def getSourceWithFormat( connectionType : String,
                         options : JsonOptions,
                         transformationContext : String = "",
                         format : String = null,
                         formatOptions : JsonOptions = JsonOptions.empty
                       ) : DataSource
```

Creates a [DataSource trait](glue-etl-scala-apis-glue-datasource-trait.md) that reads data from a source like Amazon S3, JDBC, or the AWS Glue Data Catalog, and also sets the format of data stored in the source\.
+ `connectionType` — The type of the data source\. Can be one of “s3”, “mysql”, “redshift”, “oracle”, “sqlserver”, “postgresql”, "dynamodb", “parquet”, or “orc”\. 
+ `options` — A string of JSON name\-value pairs that provide additional information for establishing a connection with the data source\.
+ `transformationContext` — The transformation context that is associated with the sink to be used by job bookmarks\. Set to empty by default\.
+ `format` — The format of the data that is stored at the source\. When the `connectionType` is "s3", you can also specify `format`\. Can be one of “avro”, “csv”, “grokLog”, “ion”, “json”, “xml”, “parquet”, or “orc”\. 
+ `formatOptions` — A string of JSON name\-value pairs that provide additional options for parsing data at the source\. See [Format Options](aws-glue-programming-etl-format.md)\.

Returns the `DataSource`\.

## def getSparkSession<a name="glue-etl-scala-apis-glue-gluecontext-defs-getSparkSession"></a>

```
def getSparkSession : SparkSession 
```

Gets the `SparkSession` object associated with this GlueContext\. Use this SparkSession object to register tables and UDFs for use with `DataFrame` created from DynamicFrames\.

Returns the SparkSession\.

## def this<a name="glue-etl-scala-apis-glue-gluecontext-defs-this-1"></a>

```
def this( sc : SparkContext,
          minPartitions : Int,
          targetPartitions : Int )
```

Creates a `GlueContext` object using the specified `SparkContext`, minimum partitions, and target partitions\.
+ `sc` — The `SparkContext`\.
+ `minPartitions` — The minimum number of partitions\.
+ `targetPartitions` — The target number of partitions\.

Returns the `GlueContext`\.

## def this<a name="glue-etl-scala-apis-glue-gluecontext-defs-this-2"></a>

```
def this( sc : SparkContext )
```

Creates a `GlueContext` object with the provided `SparkContext`\. Sets the minimum partitions to 10 and target partitions to 20\.
+ `sc` — The `SparkContext`\.

Returns the `GlueContext`\.

## def this<a name="glue-etl-scala-apis-glue-gluecontext-defs-this-3"></a>

```
def this( sparkContext : JavaSparkContext )
```

Creates a `GlueContext` object with the provided `JavaSparkContext`\. Sets the minimum partitions to 10 and target partitions to 20\.
+ `sparkContext` — The `JavaSparkContext`\.

Returns the `GlueContext`\.