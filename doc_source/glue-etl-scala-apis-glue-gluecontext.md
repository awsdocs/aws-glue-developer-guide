# The AWS Glue Scala GlueContext APIs<a name="glue-etl-scala-apis-glue-gluecontext"></a>

**Package:   com\.amazonaws\.services\.glue**

```
class GlueContext extends SQLContext(sc) (
           @transient val sc : SparkContext,
           val defaultSourcePartitioner : PartitioningStrategy )
```

GlueContext is the entry point for reading and writing a \[\[DynamicFrame\]\] from and to S3, Data Catalog, JDBC, and so forth\. This class provides utility functions to create \[\[DataSource\]\] and \[\[DataSink\]\] objects that can in turn be used to read and write DynamicFrames\.

GlueContext can also be used to set a target number of partitions \(default 20\) in the DynamicFrame if the number of partitions created from the source is less than a minimum threshold for partitions \(default 10\)\.

## def getCatalogClient<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-getCatalogClient"></a>

```
def getCatalogClient : CatalogService 
```

Creates a \[\[CatalogService\]\] object using the IAM role provided in the Job definition\.

## def getCatalogSink<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-getCatalogSink"></a>

```
def getCatalogSink( database : String,
                    tableName : String,
                    redshiftTmpDir : String = "",
                    transformationContext : String = ""
                  ) : DataSink
```

Creates a \[\[DataSink\]\] that writes to a location specified in a table defined in the data catalog\.
+ `database`  —  Database name in the data catalog\.
+ `tableName`  —  Table name in the data catalog\.
+ `redshiftTmpDir`  —  Temporary staging directory to be used with certain data sinks\. Set to empty by default\.
+ `transformationContext`  —  Transformation context associated with the sink to be used by job bookmarks\. Set to empty by default\.

Returns the DataSink\.

## def getCatalogSource<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-getCatalogSource"></a>

```
def getCatalogSource( database : String,
                      tableName : String,
                      redshiftTmpDir : String = "",
                      transformationContext : String = ""
                    ) : DataSource
```

Creates a \[\[DataSource\]\] that reads data from a table definition in the data catalog\.
+ `database`  —  Database name in the data catalog\.
+ `tableName`  —  Table name in the data catalog\.
+ `redshiftTmpDir`  —  Temporary staging directory to be used with certain data sinks\. Set to empty by default\.
+ `transformationContext`  —  Transformation context associated with the sink to be used by job bookmarks\. Set to empty by default\.

Returns the DataSource\.

## def getJDBCSink<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-getJDBCSink"></a>

```
def getJDBCSink( catalogConnection : String,
                 options : JsonOptions,
                 redshiftTmpDir : String = "",
                 transformationContext : String = ""
               ) : DataSink
```

Creates a \[\[DataSink\]\] that writes to a JDBC database specified in a Connection object in the data catalog\. The Connection object has information to connect to a JDBC sink including URL, username, password, vpc, subnet and security groups\.
+ `catalogConnection`  —  Name of the connection in the data catalog\.
+ `options`  —  \[\[JsonOptions\]\] for additional information required to write to a JDBC database, including the table name\.
+ `redshiftTmpDir`  —  Temporary staging directory to be used with certain data sinks\. Set to empty by default\.
+ `transformationContext`  —  Transformation context associated with the sink to be used by job bookmarks\. Set to empty by default\.

Returns the DataSink\.

## def getSink<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-getSink"></a>

```
def getSink( connectionType : String,
             options : JsonOptions,
             transformationContext : String = ""
           ) : DataSink
```

Creates a \[\[DataSink\]\] that writes data to a destination like S3, JDBC or the data catalog\.
+ `connectionType`  —  Type of the connection\.
+ `options`  —  \[\[JsonOptions\]\] for additional information to establish connection with the data sink\.
+ `transformationContext`  —  Transformation context associated with the sink to be used by job bookmarks\. Set to empty by default\.

Returns the DataSink\.

## def getSinkWithFormat<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-getSinkWithFormat"></a>

```
def getSinkWithFormat( connectionType : String,
                       options : JsonOptions,
                       transformationContext : String = "",
                       format : String = null,
                       formatOptions : JsonOptions = JsonOptions.empty
                     ) : DataSink
```

Creates a \[\[DataSink\]\] that writes data to a destination like S3, JDBC or the data catalog and also sets the format for the data to be written out to the destination\.
+ `connectionType`  —  Type of the connection\. Refer to \[\[DataSink\]\] for a list of supported connection types\.
+ `options`  —  \[\[JsonOptions\]\] for additional information to establish connection with the data sink\.
+ `transformationContext`  —  Transformation context associated with the sink to be used by job bookmarks\. Set to empty by default\.
+ `format`  —  Format of the data to be written out to the destination\.
+ `formatOptions`  —  \[\[JsonOptions\]\] for additional options for formatting data at the destination\.

Returns the DataSink\.

## def getSource<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-getSource"></a>

```
def getSource( connectionType : String,
               connectionOptions : JsonOptions,
               transformationContext : String = ""
             ) : DataSource
```

Creates a \[\[DataSource\]\] that reads data from a source like S3, JDBC or the data catalog\.
+ `connectionType`  —  Type of the connection\.
+ `options`  —  \[\[JsonOptions\]\] for additional information to establish connection with the data source\.
+ `transformationContext`  —  Transformation context associated with the sink to be used by job bookmarks\. Set to empty by default\.

Returns the DataSource\.

## def getSourceWithFormat<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-getSourceWithFormat"></a>

```
def getSourceWithFormat( connectionType : String,
                         options : JsonOptions,
                         transformationContext : String = "",
                         format : String = null,
                         formatOptions : JsonOptions = JsonOptions.empty
                       ) : DataSource
```

Creates a \[\[DataSource\]\] that reads data from a source like S3, JDBC or the data catalog and also sets the format of data stored in the source\.
+ `connectionType`  —  Type of the connection\.
+ `options`  —  \[\[JsonOptions\]\] for additional information to establish connection with the data source\.
+ `transformationContext`  —  Transformation context associated with the sink to be used by job bookmarks\. Set to empty by default\.
+ `format`  —  Format of the data stored at the source\.
+ `formatOptions`  —  \[\[JsonOptions\]\] for additional options for parsing data at the source\.

Returns the DataSource\.

## def getSparkSession<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-getSparkSession"></a>

```
def getSparkSession : SparkSession 
```

Gets the \[\[SparkSession\]\] object associated with this GlueContext\. Use this SparkSession object to register tables and UDFs for use with \[\[DataFrame\]\] created from DynamicFrames\.

Returns the SparkSession\.

## def this<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-this-1"></a>

```
def this( sc : SparkContext,
          minPartitions : Int,
          targetPartitions : Int )
```

Creates a GlueContext object using the specified \[\[SparkContext\]\], minimum partitions and target partitions\.
+ `sc`  —  \[\[SparkContext\]\]
+ `minPartitions`  —  Minimum number of partitions\.
+ `targetPartitions`  —  Target number of partitions\.

Returns the GlueContext\.

## def this<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-this-2"></a>

```
def this( sc : SparkContext )
```

Creates a GlueContext object with the provided \[\[SparkContext\]\]\. Sets the minimum partitions to 10 and target partitions to 20\.
+ `sc`  —  \[\[SparkContext\]\]\.

Returns the GlueContext\.

## def this<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-this-3"></a>

```
def this( sparkContext : JavaSparkContext )
```

Creates a GlueContext object with the provided \[\[JavaSparkContext\]\]\. Sets the minimum partitions to 10 and target partitions to 20\.
+ `sparkContext`  —  \[\[JavaSparkContext\]\]\.

Returns the GlueContext\.