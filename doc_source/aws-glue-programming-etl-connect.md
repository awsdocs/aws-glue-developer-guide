# Connection Types and Options for ETL in AWS Glue<a name="aws-glue-programming-etl-connect"></a>

In AWS Glue, various PySpark and Scala methods and transforms specify the connection type using a `connectionType` parameter\. They specify connection options using a `connectionOptions` or `options` parameter\.

The `connectionType` parameter can take the values shown in the following table\. The associated `connectionOptions` \(or `options`\) parameter values for each type are documented in the following sections\. Except where otherwise noted, the parameters apply when the connection is used as a source or sink\.

For sample code that demonstrates setting and using connection options, see [Examples: Setting Connection Types and Options](aws-glue-programming-etl-connect-samples.md)\.


| `connectionType` | Connects To | 
| --- | --- | 
| [custom\.\*](#aws-glue-programming-etl-connect-market) | Spark, Athena, or JDBC data stores \(see [Custom and AWS Marketplace connectionType values](#aws-glue-programming-etl-connect-market)  | 
| [documentdb](#aws-glue-programming-etl-connect-documentdb) | [Amazon DocumentDB \(with MongoDB compatibility\)](https://docs.aws.amazon.com/documentdb/latest/developerguide/) database | 
| [dynamodb](#aws-glue-programming-etl-connect-dynamodb) | [Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/) database | 
| [kafka](#aws-glue-programming-etl-connect-kafka) |  [Kafka](https://kafka.apache.org/) or [Amazon Managed Streaming for Apache Kafka](https://docs.aws.amazon.com/msk/latest/developerguide/what-is-msk.html) | 
| [kinesis](#aws-glue-programming-etl-connect-kinesis) | [Amazon Kinesis Data Streams](https://docs.aws.amazon.com/streams/latest/dev/introduction.html) | 
| [marketplace\.\*](#aws-glue-programming-etl-connect-market) | Spark, Athena, or JDBC data stores \(see [Custom and AWS Marketplace connectionType values](#aws-glue-programming-etl-connect-market)\)  | 
| [mongodb](#aws-glue-programming-etl-connect-mongodb) | [MongoDB](https://www.mongodb.com/what-is-mongodb) database | 
| [mysql](#aws-glue-programming-etl-connect-jdbc) | [MySQL](https://www.mysql.com/) database \(see [JDBC connectionType Values](#aws-glue-programming-etl-connect-jdbc)\) | 
| [oracle](#aws-glue-programming-etl-connect-jdbc) | [Oracle](https://www.oracle.com/database/) database \(see [JDBC connectionType Values](#aws-glue-programming-etl-connect-jdbc)\) | 
| [orc](#aws-glue-programming-etl-connect-orc) | Files stored in Amazon Simple Storage Service \(Amazon S3\) in the [Apache Hive Optimized Row Columnar \(ORC\)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC) file format | 
| [parquet](#aws-glue-programming-etl-connect-parquet) | Files stored in Amazon S3 in the [Apache Parquet](https://parquet.apache.org/documentation/latest/) file format | 
| [postgresql](#aws-glue-programming-etl-connect-jdbc) |  [PostgreSQL](https://www.postgresql.org/) database \(see [JDBC connectionType Values](#aws-glue-programming-etl-connect-jdbc)\) | 
| [redshift](#aws-glue-programming-etl-connect-jdbc) | [Amazon Redshift](https://aws.amazon.com/redshift/) database \(see [JDBC connectionType Values](#aws-glue-programming-etl-connect-jdbc)\) | 
| [s3](#aws-glue-programming-etl-connect-s3) | [Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/) | 
| [sqlserver](#aws-glue-programming-etl-connect-jdbc) |  Microsoft SQL Server database \(see [JDBC connectionType Values](#aws-glue-programming-etl-connect-jdbc)\) | 

## "connectionType": "documentdb"<a name="aws-glue-programming-etl-connect-documentdb"></a>

Designates a connection to Amazon DocumentDB \(with MongoDB compatibility\)\. 

Connection options differ for a source connection and a sink connection\.

### "connectionType": "documentdb" as Source<a name="etl-connect-documentdb-as-source"></a>

Use the following connection options with `"connectionType": "documentdb"` as a source:
+ `"uri"`: \(Required\) The Amazon DocumentDB host to read from, formatted as `mongodb://<host>:<port>`\.
+ `"database"`: \(Required\) The Amazon DocumentDB database to read from\.
+ `"collection"`: \(Required\) The Amazon DocumentDB collection to read from\.
+ `"username"`: \(Required\) The Amazon DocumentDB user name\.
+ `"password"`: \(Required\) The Amazon DocumentDB password\.
+ `"ssl"`: \(Required if using SSL\) If your connection uses SSL, then you must include this option with the value `"true"`\.
+ `"ssl.domain_match"`: \(Required if using SSL\) If your connection uses SSL, then you must include this option with the value `"false"`\.
+ `"batchSize"`: \(Optional\): The number of documents to return per batch, used within the cursor of internal batches\.
+ `"partitioner"`: \(Optional\): The class name of the partitioner for reading input data from Amazon DocumentDB\. The connector provides the following partitioners:
  + `MongoDefaultPartitioner` \(default\)
  + `MongoSamplePartitioner`
  + `MongoShardedPartitioner`
  + `MongoSplitVectorPartitioner`
  + `MongoPaginateByCountPartitioner`
  + `MongoPaginateBySizePartitioner`
+ `"partitionerOptions"` \(Optional\): Options for the designated partitioner\. The following options are supported for each partitioner:
  + `MongoSamplePartitioner`: `partitionKey`, `partitionSizeMB`, `samplesPerPartition`
  + `MongoShardedPartitioner`: `shardkey`
  + `MongoSplitVectorPartitioner`: `partitionKey`, partitionSizeMB
  + `MongoPaginateByCountPartitioner`: `partitionKey`, `numberOfPartitions`
  + `MongoPaginateBySizePartitioner`: `partitionKey`, partitionSizeMB

  For more information about these options, see [Partitioner Configuration](https://docs.mongodb.com/spark-connector/master/configuration/#partitioner-conf) in the MongoDB documentation\. For sample code, see [Examples: Setting Connection Types and Options](aws-glue-programming-etl-connect-samples.md)\.

### "connectionType": "documentdb" as Sink<a name="etl-connect-documentdb-as-sink"></a>

Use the following connection options with `"connectionType": "documentdb"` as a sink:
+ `"uri"`: \(Required\) The Amazon DocumentDB host to write to, formatted as `mongodb://<host>:<port>`\.
+ `"database"`: \(Required\) The Amazon DocumentDB database to write to\.
+ `"collection"`: \(Required\) The Amazon DocumentDB collection to write to\.
+ `"username"`: \(Required\) The Amazon DocumentDB user name\.
+ `"password"`: \(Required\) The Amazon DocumentDB password\.
+ `"extendedBsonTypes"`: \(Optional\) If `true`, allows extended BSON types when writing data to Amazon DocumentDB\. The default is `true`\.
+ `"replaceDocument"`: \(Optional\) If `true`, replaces the whole document when saving datasets that contain an `_id` field\. If `false`, only fields in the document that match the fields in the dataset are updated\. The default is `true`\.
+ `"maxBatchSize"`: \(Optional\): The maximum batch size for bulk operations when saving data\. The default is 512\.

For sample code, see [Examples: Setting Connection Types and Options](aws-glue-programming-etl-connect-samples.md)\.

## "connectionType": "dynamodb"<a name="aws-glue-programming-etl-connect-dynamodb"></a>

Designates a connection to Amazon DynamoDB\.

Connection options differ for a source connection and a sink connection\.

### "connectionType": "dynamodb" as Source<a name="etl-connect-dynamodb-as-source"></a>

Use the following connection options with `"connectionType": "dynamodb"` as a source:
+ `"dynamodb.input.tableName"`: \(Required\) The DynamoDB table to read from\.
+ `"dynamodb.throughput.read.percent"`: \(Optional\) The percentage of read capacity units \(RCU\) to use\. The default is set to "0\.5"\. Acceptable values are from "0\.1" to "1\.5", inclusive\.
  + `0.5` represents the default read rate, meaning that AWS Glue will attempt to consume half of the read capacity of the table\. If you increase the value above `0.5`, AWS Glue increases the request rate; decreasing the value below `0.5` decreases the read request rate\. \(The actual read rate will vary, depending on factors such as whether there is a uniform key distribution in the DynamoDB table\.\)
  + When the DynamoDB table is in on\-demand mode, AWS Glue handles the read capacity of the table as 40000\. For exporting a large table, we recommend switching your DynamoDB table to on\-demand mode\.
+ `"dynamodb.splits"`: \(Optional\) Defines how many splits we partition this DynamoDB table into while reading\. The default is set to "1"\. Acceptable values are from "1" to "1,000,000", inclusive\.
  + `1` represents there is no parallelism\. We highly recommend that you specify a larger value for better performance by using the below formula\.
  + We recommend you to calculate `numSlots` using the following formula, and use it as `dynamodb.splits`\. If you need more performance, we recommend you to scale out your job by increasing the number of DPUs\.
    + `numExecutors =`
      + `(DPU - 1) * 2 - 1` if `WorkerType` is `Standard`
      + `(NumberOfWorkers - 1)` if `WorkerType` is `G.1X` or `G.2X`
    + `numSlotsPerExecutor =`
      + `4` if `WorkerType` is `Standard`
      + `8` if `WorkerType` is `G.1X`
      + `16` if `WorkerType` is `G.2X`
    + `numSlots = numSlotsPerExecutor * numExecutors`
+ `"dynamodb.sts.roleArn"`: \(Optional\) The IAM role ARN to be assumed for cross\-account access\. This parameter is available in AWS Glue 1\.0 or later\.
+ `"dynamodb.sts.roleSessionName"`: \(Optional\) STS session name\. The default is set to "glue\-dynamodb\-read\-sts\-session"\. This parameter is available in AWS Glue 1\.0 or later\.

**Note**  
AWS Glue supports reading data from another AWS account's DynamoDB table\. For more information, see [Cross\-Account Cross\-Region Access to DynamoDB Tables](aws-glue-programming-etl-dynamo-db-cross-account.md)\.

**Note**  
The DynamoDB reader does not support filters or pushdown predicates\.

### "connectionType": "dynamodb" as Sink<a name="etl-connect-dynamodb-as-sink"></a>

Use the following connection options with `"connectionType": "dynamodb"` as a sink:
+ `"dynamodb.output.tableName"`: \(Required\) The DynamoDB table to write to\.
+ `"dynamodb.throughput.write.percent"`: \(Optional\) The percentage of write capacity units \(WCU\) to use\. The default is set to "0\.5"\. Acceptable values are from "0\.1" to "1\.5", inclusive\.
  + `0.5` represents the default write rate, meaning that AWS Glue will attempt to consume half of the write capacity of the table\. If you increase the value above 0\.5, AWS Glue increases the request rate; decreasing the value below 0\.5 decreases the write request rate\. \(The actual write rate will vary, depending on factors such as whether there is a uniform key distribution in the DynamoDB table\)\.
  + When the DynamoDB table is in on\-demand mode, AWS Glue handles the write capacity of the table as `40000`\. For importing a large table, we recommend switching your DynamoDB table to on\-demand mode\.
+ `"dynamodb.output.numParallelTasks"`: \(Optional\) Defines how many parallel tasks write into DynamoDB at the same time\. Used to calculate permissive WCU per Spark task\. If you do not want to control these details, you do not need to specify this parameter\.
  + `permissiveWcuPerTask = TableWCU * dynamodb.throughput.write.percent / dynamodb.output.numParallelTasks`
  + If you do not specify this parameter, the permissive WCU per Spark task will be automatically calculated by the following formula:
    + `numPartitions = dynamicframe.getNumPartitions()`
    + `numExecutors =`
      + `(DPU - 1) * 2 - 1` if `WorkerType` is `Standard`
      + `(NumberOfWorkers - 1)` if `WorkerType` is `G.1X` or `G.2X`
    + `numSlotsPerExecutor =`
      + `4` if `WorkerType` is `Standard`
      + `8` if `WorkerType` is `G.1X`
      + `16` if `WorkerType` is `G.2X`
    + `numSlots = numSlotsPerExecutor * numExecutors`
    + `numParallelTasks = min(numPartitions, numSlots)`
  + Example 1\. DPU=10, WorkerType=Standard\. Input DynamicFrame has 100 RDD partitions\.
    + `numPartitions = 100`
    + `numExecutors = (10 - 1) * 2 - 1 = 17`
    + `numSlots = 4 * 17 = 68`
    + `numParallelTasks = min(100, 68) = 68`
  + Example 2\. DPU=10, WorkerType=Standard\. Input DynamicFrame has 20 RDD partitions\.
    + `numPartitions = 20`
    + `numExecutors = (10 - 1) * 2 - 1 = 17`
    + `numSlots = 4 * 17 = 68`
    + `numParallelTasks = min(20, 68) = 20`
+ `"dynamodb.output.retry"`: \(Optional\) Defines how many retries we perform when there is a `ProvisionedThroughputExceededException` from DynamoDB\. The default is set to "10"\.
+ `"dynamodb.sts.roleArn"`: \(Optional\) The IAM role ARN to be assumed for cross\-account access\.
+ `"dynamodb.sts.roleSessionName"`: \(Optional\) STS session name\. The default is set to "glue\-dynamodb\-write\-sts\-session"\.

**Note**  
The DynamoDB writer is supported in AWS Glue version 1\.0 or later\.

**Note**  
AWS Glue supports writing data into another AWS account's DynamoDB table\. For more information, see [Cross\-Account Cross\-Region Access to DynamoDB Tables](aws-glue-programming-etl-dynamo-db-cross-account.md)\.

The following code examples show how to read from and write to DynamoDB tables\. They demonstrate reading from one table and writing to another table\.

------
#### [ Python ]

```
import sys
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job
from awsglue.utils import getResolvedOptions

args = getResolvedOptions(sys.argv, ["JOB_NAME"])
glue_context= GlueContext(SparkContext.getOrCreate())
job = Job(glue_context)
job.init(args["JOB_NAME"], args)

dyf = glue_context.create_dynamic_frame.from_options(
    connection_type="dynamodb",
    connection_options={
        "dynamodb.input.tableName": "test_source",
        "dynamodb.throughput.read.percent": "1.0",
        "dynamodb.splits": "100"
    }
)

print(dyf.getNumPartitions())

glue_context.write_dynamic_frame_from_options(
    frame=dyf,
    connection_type="dynamodb",
    connection_options={
        "dynamodb.output.tableName": "test_sink",
        "dynamodb.throughput.write.percent": "1.0"
    }
)

job.commit()
```

------
#### [ Scala ]

```
import com.amazonaws.services.glue.GlueContext
import com.amazonaws.services.glue.util.GlueArgParser
import com.amazonaws.services.glue.util.Job
import com.amazonaws.services.glue.util.JsonOptions
import com.amazonaws.services.glue.DynamoDbDataSink
import org.apache.spark.SparkContext
import scala.collection.JavaConverters._


object GlueApp {

  def main(sysArgs: Array[String]): Unit = {
    val glueContext = new GlueContext(SparkContext.getOrCreate())
    val args = GlueArgParser.getResolvedOptions(sysArgs, Seq("JOB_NAME").toArray)
    Job.init(args("JOB_NAME"), glueContext, args.asJava)
    
    val dynamicFrame = glueContext.getSourceWithFormat(
      connectionType = "dynamodb",
      options = JsonOptions(Map(
        "dynamodb.input.tableName" -> "test_source",
        "dynamodb.throughput.read.percent" -> "1.0",
        "dynamodb.splits" -> "100"
      ))
    ).getDynamicFrame()
    
    print(dynamicFrame.getNumPartitions())

    val dynamoDbSink: DynamoDbDataSink =  glueContext.getSinkWithFormat(
      connectionType = "dynamodb",
      options = JsonOptions(Map(
        "dynamodb.output.tableName" -> "test_sink",
        "dynamodb.throughput.write.percent" -> "1.0"
      ))
    ).asInstanceOf[DynamoDbDataSink]
    
    dynamoDbSink.writeDynamicFrame(dynamicFrame)

    Job.commit()
  }

}
```

------

## "connectionType": "kafka"<a name="aws-glue-programming-etl-connect-kafka"></a>

Designates a connection to a Kafka cluster or an Amazon Managed Streaming for Apache Kafka cluster\. 

You can use the following methods under the `GlueContext` object to consume records from a Kafka streaming source:
+ `getCatalogSource`
+ `getSource`
+ `getSourceWithFormat`

If you use `getCatalogSource`, then the job has the Data Catalog database and table name information, and can use that to obtain some basic parameters for reading from the Apache Kafka stream\. If you use `getSource`, you must explicitly specify these parameters:

You can specify these options using `connectionOptions` with `GetSource`, `options` with `getSourceWithFormat`, or `additionalOptions` with `getCatalogSource`\.

Use the following connection options with `"connectionType": "kafka"`:
+ `bootstrap.servers` \(Required\) A list of bootstrap server URLs, for example, as `b-1.vpc-test-2.o4q88o.c6.kafka.us-east-1.amazonaws.com:9094`\. This option must be specified in the API call or defined in the table metadata in the Data Catalog\.
+ `security.protocol` \(Required\) A Boolean value indicating whether to turn on or turn off SSL on an Apache Kafka connection\. The default value is "true"\. This option must be specified in the API call or defined in the table metadata in the Data Catalog\.
+ `topicName` \(Required\) The topic name as specified in Apache Kafka\. You must specify at least one of `"topicName"`, `"assign"` or `"subscribePattern"`\.
+ `"assign"`: \(Required\) The specific `TopicPartitions` to consume\. You must specify at least one of `"topicName"`, `"assign"` or `"subscribePattern"`\.
+ `"subscribePattern"`: \(Required\) A Java regex string that identifies the topic list to subscribe to\. You must specify at least one of `"topicName"`, `"assign"` or `"subscribePattern"`\.
+ `classification` \(Optional\)
+ `delimiter` \(Optional\)
+ `"startingOffsets"`: \(Optional\) The starting position in the Kafka topic to read data from\. The possible values are `"earliest"` or `"latest"`\. The default value is `"latest"`\.
+ `"endingOffsets"`: \(Optional\) The end point when a batch query is ended\. Possible values are either `"latest"` or a JSON string that specifies an ending offset for each `TopicPartition`\.

  For the JSON string, the format is `{"topicA":{"0":23,"1":-1},"topicB":{"0":-1}}`\. The value `-1` as an offset represents `"latest"`\.
+ `"pollTimeoutMs"`: \(Optional\) The timeout in milliseconds to poll data from Kafka in Spark job executors\. The default value is `512`\.
+ `"numRetries"`: \(Optional\) The number of times to retry before failing to fetch Kafka offsets\. The default value is `3`\.
+ `"retryIntervalMs"`: \(Optional\) The time in milliseconds to wait before retrying to fetch Kafka offsets\. The default value is `10`\.
+ `"maxOffsetsPerTrigger"`: \(Optional\) The rate limit on the maximum number of offsets that are processed per trigger interval\. The specified total number of offsets is proportionally split across `topicPartitions` of different volumes\. The default value is null, which means that the consumer reads all offsets until the known latest offset\.
+ `"minPartitions"`: \(Optional\) The desired minimum number of partitions to read from Kafka\. The default value is null, which means that the number of spark partitions is equal to the number of Kafka partitions\.

## "connectionType": "kinesis"<a name="aws-glue-programming-etl-connect-kinesis"></a>

Designates a connection to Amazon Kinesis Data Streams\.

You can use the following methods under the `GlueContext` object to consume records from a Kinesis streaming source:
+ `getCatalogSource`
+ `getSource`
+ `getSourceWithFormat`

If you use `getCatalogSource`, then the job has the Data Catalog database and table name information, and can use that to obtain some basic parameters for reading from the Kinesis streaming source\. If you use `getSource`, you must explicitly specify these parameters:

You can specify these options using `connectionOptions` with `GetSource`, `options` with `getSourceWithFormat`, or `additionalOptions` with `getCatalogSource`\.

Use the following connection options with `"connectionType": "kinesis"`: 
+ `endpointUrl` \(Required\) The URL for the Kinesis streaming source\. This option must be specified in the API call or defined in the table metadata in the Data Catalog\.
+ `streamName` \(Required\) The identifier of the data stream in the following format: `account-id:StreamName:streamCreationTimestamp`\. This option must be specified in the API call or defined in the table metadata in the Data Catalog\.
+ `classification` \(Optional\)
+ `delimiter` \(Optional\)
+ `"startingPosition"`: \(Optional\) The starting position in the Kinesis data stream to read data from\. The possible values are `"latest"`, `"trim_horizon"`, or `"earliest"`\. The default value is `"latest"`\.
+ `"maxFetchTimeInMs"`: \(Optional\) The maximum time spent in the job executor to fetch a record from the Kinesis data stream per shard, specified in milliseconds \(ms\)\. The default value is `1000`\.
+ `"maxFetchRecordsPerShard"`: \(Optional\) The maximum number of records to fetch per shard in the Kinesis data stream\. The default value is `100000`\.
+ `"maxRecordPerRead"`: \(Optional\) The maximum number of records to fetch from the Kinesis data stream in each `getRecords` operation\. The default value is `10000`\.
+ `"describeShardInterval"`: \(Optional\) The minimum time interval between two `ListShards` API calls for your script to consider resharding\. For more information, see [Strategies for Resharding](https://docs.aws.amazon.com/streams/latest/dev/kinesis-using-sdk-java-resharding-strategies.html) in *Amazon Kinesis Data Streams Developer Guide*\. The default value is `1s`\.
+ `"numRetries"`: \(Optional\) The maximum number of retries for Kinesis Data Streams API requests\. The default value is `3`\.
+ `"retryIntervalMs"`: \(Optional\) The cool\-off time period \(specified in ms\) before retrying the Kinesis Data Streams API call\. The default value is `1000`\.
+ `"maxRetryIntervalMs"`: \(Optional\) The maximum cool\-off time period \(specified in ms\) between two retries of a Kinesis Data Streams API call\. The default value is `10000`\.
+ `"avoidEmptyBatches"`: \(Optional\) Avoids creating an empty microbatch job by checking for unread data in the Kinesis data stream before the batch is started\. The default value is `"False"`\.

## "connectionType": "mongodb"<a name="aws-glue-programming-etl-connect-mongodb"></a>

Designates a connection to MongoDB\. Connection options differ for a source connection and a sink connection\.

### "connectionType": "mongodb" as Source<a name="etl-connect-mongodb-as-source"></a>

Use the following connection options with `"connectionType": "mongodb"` as a source:
+ `"uri"`: \(Required\) The MongoDB host to read from, formatted as `mongodb://<host>:<port>`\.
+ `"database"`: \(Required\) The MongoDB database to read from\. This option can also be passed in `additional_options` when calling `glue_context.create_dynamic_frame_from_catalog` in your job script\.
+ `"collection"`: \(Required\) The MongoDB collection to read from\. This option can also be passed in `additional_options` when calling `glue_context.create_dynamic_frame_from_catalog` in your job script\.
+ `"username"`: \(Required\) The MongoDB user name\.
+ `"password"`: \(Required\) The MongoDB password\.
+ `"ssl"`: \(Optional\) If `true`, initiates an SSL connection\. The default is `false`\.
+ `"ssl.domain_match"`: \(Optional\) If `true` and `ssl` is `true`, domain match check is performed\. The default is `true`\.
+ `"batchSize"`: \(Optional\): The number of documents to return per batch, used within the cursor of internal batches\.
+ `"partitioner"`: \(Optional\): The class name of the partitioner for reading input data from MongoDB\. The connector provides the following partitioners:
  + `MongoDefaultPartitioner` \(default\)
  + `MongoSamplePartitioner` \(Requires MongoDB 3\.2 or later\)
  + `MongoShardedPartitioner`
  + `MongoSplitVectorPartitioner`
  + `MongoPaginateByCountPartitioner`
  + `MongoPaginateBySizePartitioner`
+ `"partitionerOptions"` \(Optional\): Options for the designated partitioner\. The following options are supported for each partitioner:
  + `MongoSamplePartitioner`: `partitionKey`, `partitionSizeMB`, `samplesPerPartition`
  + `MongoShardedPartitioner`: `shardkey`
  + `MongoSplitVectorPartitioner`: `partitionKey`, `partitionSizeMB`
  + `MongoPaginateByCountPartitioner`: `partitionKey`, `numberOfPartitions`
  + `MongoPaginateBySizePartitioner`: `partitionKey`, `partitionSizeMB`

  For more information about these options, see [Partitioner Configuration](https://docs.mongodb.com/spark-connector/master/configuration/#partitioner-conf) in the MongoDB documentation\. For sample code, see [Examples: Setting Connection Types and Options](aws-glue-programming-etl-connect-samples.md)\.

### "connectionType": "mongodb" as Sink<a name="etl-connect-mongodb-as-sink"></a>

Use the following connection options with `"connectionType": "mongodb"` as a sink:
+ `"uri"`: \(Required\) The MongoDB host to write to, formatted as `mongodb://<host>:<port>`\.
+ `"database"`: \(Required\) The MongoDB database to write to\.
+ `"collection"`: \(Required\) The MongoDB collection to write to\.
+ `"username"`: \(Required\) The MongoDB user name\.
+ `"password"`: \(Required\) The MongoDB password\.
+ `"ssl"`: \(Optional\) If `true`, initiates an SSL connection\. The default is `false`\.
+ `"ssl.domain_match"`: \(Optional\) If `true` and `ssl` is `true`, domain match check is performed\. The default is `true`\.
+ `"extendedBsonTypes"`: \(Optional\) If `true`, allows extended BSON types when writing data to MongoDB\. The default is `true`\.
+ `"replaceDocument"`: \(Optional\) If `true`, replaces the whole document when saving datasets that contain an `_id` field\. If `false`, only fields in the document that match the fields in the dataset are updated\. The default is `true`\.
+ `"maxBatchSize"`: \(Optional\): The maximum batch size for bulk operations when saving data\. The default is 512\.

For sample code, see [Examples: Setting Connection Types and Options](aws-glue-programming-etl-connect-samples.md)\.

## "connectionType": "orc"<a name="aws-glue-programming-etl-connect-orc"></a>

Designates a connection to files stored in Amazon S3 in the [Apache Hive Optimized Row Columnar \(ORC\)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC) file format\.

Use the following connection options with `"connectionType": "orc"`:
+ `paths`: \(Required\) A list of the Amazon S3 paths to read from\.
+ *\(Other option name/value pairs\)*: Any additional options, including formatting options, are passed directly to the SparkSQL `DataSource`\. For more information, see [Redshift data source for Spark](https://github.com/databricks/spark-redshift)\.

## "connectionType": "parquet"<a name="aws-glue-programming-etl-connect-parquet"></a>

Designates a connection to files stored in Amazon S3 in the [Apache Parquet](https://parquet.apache.org/documentation/latest/) file format\.

Use the following connection options with `"connectionType": "parquet"`:
+ `paths`: \(Required\) A list of the Amazon S3 paths to read from\.
+ *\(Other option name/value pairs\)*: Any additional options, including formatting options, are passed directly to the SparkSQL `DataSource`\. For more information, see [Amazon Redshift data source for Spark](https://github.com/databricks/spark-redshift) on the GitHub website\.

## "connectionType": "s3"<a name="aws-glue-programming-etl-connect-s3"></a>

Designates a connection to Amazon S3\.

Use the following connection options with `"connectionType": "s3"`:
+ `"paths"`: \(Required\) A list of the Amazon S3 paths to read from\.
+ `"exclusions"`: \(Optional\) A string containing a JSON list of Unix\-style glob patterns to exclude\. For example, `"[\"**.pdf\"]"` excludes all PDF files\. For more information about the glob syntax that AWS Glue supports, see [Include and Exclude Patterns](https://docs.aws.amazon.com/glue/latest/dg/define-crawler.html#crawler-data-stores-exclude)\.
+ `"compressionType"`: or "`compression`": \(Optional\) Specifies how the data is compressed\. Use `"compressionType"` for Amazon S3 sources and `"compression"` for Amazon S3 targets\. This is generally not necessary if the data has a standard file extension\. Possible values are `"gzip"` and `"bzip"`\)\.
+ `"groupFiles"`: \(Optional\) Grouping files is turned on by default when the input contains more than 50,000 files\. To turn on grouping with fewer than 50,000 files, set this parameter to `"inPartition"`\. To disable grouping when there are more than 50,000 files, set this parameter to `"none"`\.
+ `"groupSize"`: \(Optional\) The target group size in bytes\. The default is computed based on the input data size and the size of your cluster\. When there are fewer than 50,000 input files, `"groupFiles"` must be set to `"inPartition"` for this to take effect\.
+ `"recurse"`: \(Optional\) If set to true, recursively reads files in all subdirectories under the specified paths\.
+ `"maxBand"`: \(Optional, advanced\) This option controls the duration in milliseconds after which the `s3` listing is likely to be consistent\. Files with modification timestamps falling within the last `maxBand` milliseconds are tracked specially when using `JobBookmarks` to account for Amazon S3 eventual consistency\. Most users don't need to set this option\. The default is 900000 milliseconds, or 15 minutes\.
+ `"maxFilesInBand"`: \(Optional, advanced\) This option specifies the maximum number of files to save from the last `maxBand` seconds\. If this number is exceeded, extra files are skipped and only processed in the next job run\. Most users don't need to set this option\.
+ `"isFailFast"`: \(Optional\) This option determines if an AWS Glue ETL job throws reader parsing exceptions\. If set to `true`, jobs fail fast if four retries of the Spark task fail to parse the data correctly\.

## JDBC connectionType Values<a name="aws-glue-programming-etl-connect-jdbc"></a>

These include the following:
+ `"connectionType": "sqlserver"`: Designates a connection to a Microsoft SQL Server database\.
+ `"connectionType": "mysql"`: Designates a connection to a MySQL database\.
+ `"connectionType": "oracle"`: Designates a connection to an Oracle database\.
+ `"connectionType": "postgresql"`: Designates a connection to a PostgreSQL database\.
+ `"connectionType": "redshift"`: Designates a connection to an Amazon Redshift database\.

The following table lists the JDBC driver versions that AWS Glue supports\.


| Product | JDBC Driver Version | 
| --- | --- | 
| Microsoft SQL Server | 6\.x | 
| MySQL | 5\.1 | 
| Oracle Database | 11\.2 | 
| PostgreSQL | 42\.x | 
| Amazon Redshift | 4\.1 | 

Use these connection options with JDBC connections:
+ `"url"`: \(Required\) The JDBC URL for the database\.
+ `"dbtable"`: The database table to read from\. For JDBC data stores that support schemas within a database, specify `schema.table-name`\. If a schema is not provided, then the default "public" schema is used\.
+ `"redshiftTmpDir"`: \(Required for Amazon Redshift, optional for other JDBC types\) The Amazon S3 path where temporary data can be staged when copying out of the database\.
+ `"user"`: \(Required\) The user name to use when connecting\.
+ `"password"`: \(Required\) The password to use when connecting\.
+ \(Optional\) The following options allow you to supply a custom JDBC driver\. Use these options if you must use a driver that AWS Glue does not natively support\. ETL jobs can use different JDBC driver versions for the data source and target, even if the source and target are the same database product\. This allows you to migrate data between source and target databases with different versions\. To use these options, you must first upload the jar file of the JDBC driver to Amazon S3\.
  + `"customJdbcDriverS3Path"`: Amazon S3 path of the custom JDBC driver\.
  + `"customJdbcDriverClassName"`: Class name of JDBC driver\.

All other option name/value pairs that are included in connection options for a JDBC connection, including formatting options, are passed directly to the underlying SparkSQL DataSource\. For more information, see [Redshift data source for Spark](https://github.com/databricks/spark-redshift)\.

The following code examples show how to read from and write to JDBC databases with custom JDBC drivers\. They demonstrate reading from one version of a database product and writing to a later version of the same product\.

------
#### [ Python ]

```
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext, SparkConf
from awsglue.context import GlueContext
from awsglue.job import Job
import time
from pyspark.sql.types import StructType, StructField, IntegerType, StringType

sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session

# Construct JDBC connection options
connection_mysql5_options = {
    "url": "jdbc:mysql://<jdbc-host-name>:3306/db",
    "dbtable": "test",
    "user": "admin",
    "password": "pwd"}

connection_mysql8_options = {
    "url": "jdbc:mysql://<jdbc-host-name>:3306/db",
    "dbtable": "test",
    "user": "admin",
    "password": "pwd",
    "customJdbcDriverS3Path": "s3://path/mysql-connector-java-8.0.17.jar",
    "customJdbcDriverClassName": "com.mysql.cj.jdbc.Driver"}

connection_oracle11_options = {
    "url": "jdbc:oracle:thin:@//<jdbc-host-name>:1521/ORCL",
    "dbtable": "test",
    "user": "admin",
    "password": "pwd"}

connection_oracle18_options = {
    "url": "jdbc:oracle:thin:@//<jdbc-host-name>:1521/ORCL",
    "dbtable": "test",
    "user": "admin",
    "password": "pwd",
    "customJdbcDriverS3Path": "s3://path/ojdbc10.jar",
    "customJdbcDriverClassName": "oracle.jdbc.OracleDriver"}

# Read from JDBC databases with custom driver
df_mysql8 = glueContext.create_dynamic_frame.from_options(connection_type="mysql",
                                                          connection_options=connection_mysql8_options)

# Read DynamicFrame from MySQL 5 and write to MySQL 8
df_mysql5 = glueContext.create_dynamic_frame.from_options(connection_type="mysql",
                                                          connection_options=connection_mysql5_options)
glueContext.write_from_options(frame_or_dfc=df_mysql5, connection_type="mysql",
                               connection_options=connection_mysql8_options)

# Read DynamicFrame from Oracle 11 and write to Oracle 18
df_oracle11 = glueContext.create_dynamic_frame.from_options(connection_type="oracle",
                                                            connection_options=connection_oracle11_options)
glueContext.write_from_options(frame_or_dfc=df_oracle11, connection_type="oracle",
                               connection_options=connection_oracle18_options)
```

------
#### [ Scala ]

```
import com.amazonaws.services.glue.GlueContext
import com.amazonaws.services.glue.MappingSpec
import com.amazonaws.services.glue.errors.CallSite
import com.amazonaws.services.glue.util.GlueArgParser
import com.amazonaws.services.glue.util.Job
import com.amazonaws.services.glue.util.JsonOptions
import com.amazonaws.services.glue.DynamicFrame
import org.apache.spark.SparkContext
import scala.collection.JavaConverters._


object GlueApp {
  val MYSQL_5_URI: String = "jdbc:mysql://<jdbc-host-name>:3306/db"
  val MYSQL_8_URI: String = "jdbc:mysql://<jdbc-host-name>:3306/db"
  val ORACLE_11_URI: String = "jdbc:oracle:thin:@//<jdbc-host-name>:1521/ORCL"
  val ORACLE_18_URI: String = "jdbc:oracle:thin:@//<jdbc-host-name>:1521/ORCL"

  // Construct JDBC connection options
  lazy val mysql5JsonOption = jsonOptions(MYSQL_5_URI)
  lazy val mysql8JsonOption = customJDBCDriverJsonOptions(MYSQL_8_URI, "s3://path/mysql-connector-java-8.0.17.jar", "com.mysql.cj.jdbc.Driver")
  lazy val oracle11JsonOption = jsonOptions(ORACLE_11_URI)
  lazy val oracle18JsonOption = customJDBCDriverJsonOptions(ORACLE_18_URI, "s3://path/ojdbc10.jar", "oracle.jdbc.OracleDriver")

  def main(sysArgs: Array[String]): Unit = {
    val spark: SparkContext = new SparkContext()
    val glueContext: GlueContext = new GlueContext(spark)
    val args = GlueArgParser.getResolvedOptions(sysArgs, Seq("JOB_NAME").toArray)
    Job.init(args("JOB_NAME"), glueContext, args.asJava)

    // Read from JDBC database with custom driver
    val df_mysql8: DynamicFrame = glueContext.getSource("mysql", mysql8JsonOption).getDynamicFrame()

    // Read DynamicFrame from MySQL 5 and write to MySQL 8
    val df_mysql5: DynamicFrame = glueContext.getSource("mysql", mysql5JsonOption).getDynamicFrame()
    glueContext.getSink("mysql", mysql8JsonOption).writeDynamicFrame(df_mysql5)

    // Read DynamicFrame from Oracle 11 and write to Oracle 18
    val df_oracle11: DynamicFrame = glueContext.getSource("oracle", oracle11JsonOption).getDynamicFrame()
    glueContext.getSink("oracle", oracle18JsonOption).writeDynamicFrame(df_oracle11)

    Job.commit()
  }

  private def jsonOptions(uri: String): JsonOptions = {
    new JsonOptions(
      s"""{"url": "${url}",
         |"dbtable":"test",
         |"user": "admin",
         |"password": "pwd"}""".stripMargin)
  }

  private def customJDBCDriverJsonOptions(uri: String, customJdbcDriverS3Path: String, customJdbcDriverClassName: String): JsonOptions = {
    new JsonOptions(
      s"""{"url": "${url}",
         |"dbtable":"test",
         |"user": "admin",
         |"password": "pwd",
         |"customJdbcDriverS3Path": "${customJdbcDriverS3Path}",
         |"customJdbcDriverClassName" : "${customJdbcDriverClassName}"}""".stripMargin)
  }
}
```

------

## Custom and AWS Marketplace connectionType values<a name="aws-glue-programming-etl-connect-market"></a>

These include the following:
+ `"connectionType": "marketplace.athena"`: Designates a connection to an Amazon Athena data store\. The connection uses a connector from AWS Marketplace\.
+ `"connectionType": "marketplace.spark"`: Designates a connection to an Apache Spark data store\. The connection uses a connector from AWS Marketplace\.
+ `"connectionType": "marketplace.jdbc"`: Designates a connection to a JDBC data store\. The connection uses a connector from AWS Marketplace\.
+ `"connectionType": "custom.athena"`: Designates a connection to an Amazon Athena data store\. The connection uses a custom connector that you upload to AWS Glue Studio\.
+ `"connectionType": "custom.spark"`: Designates a connection to an Apache Spark data store\. The connection uses a custom connector that you upload to AWS Glue Studio\.
+ `"connectionType": "custom.jdbc"`: Designates a connection to a JDBC data store\. The connection uses a custom connector that you upload to AWS Glue Studio\.

### Connection options for type custom\.jdbc or marketplace\.jdbc<a name="marketplace-jdbc-connect-options"></a>
+ `className` – String, required, driver class name\.
+ `connectionName` – String, required, name of the connection that is associated with the connector\.
+ `url` – String, required, JDBC URL with placeholders \(`${}`\) which are used to build the connection to the data source\. The placeholder `${secretKey}` is replaced with the secret of the same name in AWS Secrets Manager\. Refer to the data store documentation for more information about constructing the URL\. 
+ `secretId` or `user/password` – String, required, used to retrieve credentials for the URL\. 
+ `dbTable` or `query` – String, required, the table or SQL query to get the data from\. You can specify either `dbTable` or `query`, but not both\. 
+ `partitionColumn` – String, optional, the name of an integer column that is used for partitioning\. This option works only when it's included with `lowerBound`, `upperBound`, and `numPartitions`\. This option works the same way as in the Spark SQL JDBC reader\. For more information, see [JDBC To Other Databases](https://spark.apache.org/docs/latest/sql-data-sources-jdbc.html) in the *Apache Spark SQL, DataFrames and Datasets Guide*\.

  The `lowerBound` and `upperBound` values are used to decide the partition stride, not for filtering the rows in table\. All rows in the table are partitioned and returned\. 
**Note**  
When using a query instead of a table name, you should validate that the query works with the specified partitioning condition\. For example:   
If your query format is `"SELECT col1 FROM table1"`, then test the query by appending a `WHERE` clause at the end of the query that uses the partition column\. 
If your query format is "`SELECT col1 FROM table1 WHERE col2=val"`, then test the query by extending the `WHERE` clause with `AND` and an expression that uses the partition column\.
+ `lowerBound` – Integer, optional, the minimum value of `partitionColumn` that is used to decide partition stride\. 
+ `upperBound` – Integer, optional, the maximum value of `partitionColumn` that is used to decide partition stride\. 
+ `numPartitions` – Integer, optional, the number of partitions\. This value, along with `lowerBound` \(inclusive\) and `upperBound` \(exclusive\), form partition strides for generated `WHERE` clause expressions that are used to split the `partitionColumn`\. 
**Important**  
Be careful with the number of partitions because too many partitions might cause problems on your external database systems\. 
+ `filterPredicate` – String, optional, extra condition clause to filter data from source\. For example: 

  ```
  BillingCity='Mountain View'
  ```

  When using a *query* instead of a *table* name, you should validate that the query works with the specified `filterPredicate`\. For example: 
  + If your query format is `"SELECT col1 FROM table1"`, then test the query by appending a `WHERE` clause at the end of the query that uses the filter predicate\. 
  + If your query format is `"SELECT col1 FROM table1 WHERE col2=val"`, then test the query by extending the `WHERE` clause with `AND` and an expression that uses the filter predicate\.
+ `dataTypeMapping` – Dictionary, optional, custom data type mapping that builds a mapping from a **JDBC** data type to a **Glue** data type\. For example, the option `"dataTypeMapping":{"FLOAT":"STRING"}` maps data fields of JDBC type `FLOAT` into the Java `String` type by calling the `ResultSet.getString()` method of the driver, and uses it to build Glue Record\. The `ResultSet` object is implemented by each driver, so the behavior is specific to the driver you use\. Refer to the documentation for your JDBC driver to understand how the driver performs the conversions\. 
+ The AWS Glue data type supported currently are:
  + DATE
  + STRING
  + TIMESTAMP
  + INT
  + FLOAT
  + LONG
  + BIGDECIMAL
  + BYTE
  + SHORT
  + DOUBLE

   The JDBC data types supported are [Java8 java\.sql\.types](https://docs.oracle.com/javase/8/docs/api/java/sql/Types.html)\.

  The default data type mappings \(from JDBC to AWS Glue\) are:
  +  DATE \-> DATE
  +  VARCHAR \-> STRING
  +  CHAR \-> STRING
  +  LONGNVARCHAR \-> STRING
  +  TIMESTAMP \-> TIMESTAMP
  +  INTEGER \-> INT
  +  FLOAT \-> FLOAT
  +  REAL \-> FLOAT
  +  BIT \-> BOOLEAN
  +  BOOLEAN \-> BOOLEAN
  +  BIGINT \-> LONG
  +  DECIMAL \-> BIGDECIMAL
  +  NUMERIC \-> BIGDECIMAL
  +  TINYINT \-> SHORT
  +  SMALLINT \-> SHORT
  +  DOUBLE \-> DOUBLE

  If you use a custom data type mapping with the option `dataTypeMapping`, then you can override a default data type mapping\. Only the JDBC data types listed in the `dataTypeMapping` option are affected; the default mapping is used for all other JDBC data types\. You can add mappings for additional JDBC data types if needed\. If a JDBC data type is not included in either the default mapping or a custom mapping, then the data type converts to the AWS Glue `STRING` data type by default\. 

The following Python code example shows how to read from JDBC databases with AWS Marketplace JDBC drivers\. It demonstrates reading from a database and writing to an S3 location\. 

```
    import sys
    from awsglue.transforms import *
    from awsglue.utils import getResolvedOptions
    from pyspark.context import SparkContext
    from awsglue.context import GlueContext
    from awsglue.job import Job
     
    ## @params: [JOB_NAME]
    args = getResolvedOptions(sys.argv, ['JOB_NAME'])
     
    sc = SparkContext()
    glueContext = GlueContext(sc)
    spark = glueContext.spark_session
    job = Job(glueContext)
    job.init(args['JOB_NAME'], args)
    ## @type: DataSource
    ## @args: [connection_type = "marketplace.jdbc", connection_options = 
     {"dataTypeMapping":{"INTEGER":"STRING"},"upperBound":"200","query":"select id, 
       name, department from department where id < 200","numPartitions":"4",
       "partitionColumn":"id","lowerBound":"0","connectionName":"test-connection-jdbc"},
        transformation_ctx = "DataSource0"]
    ## @return: DataSource0
    ## @inputs: []
    DataSource0 = glueContext.create_dynamic_frame.from_options(connection_type = 
      "marketplace.jdbc", connection_options = {"dataTypeMapping":{"INTEGER":"STRING"},
      "upperBound":"200","query":"select id, name, department from department where 
       id < 200","numPartitions":"4","partitionColumn":"id","lowerBound":"0",
       "connectionName":"test-connection-jdbc"}, transformation_ctx = "DataSource0")
    ## @type: ApplyMapping
    ## @args: [mappings = [("department", "string", "department", "string"), ("name", "string",
      "name", "string"), ("id", "int", "id", "int")], transformation_ctx = "Transform0"]
    ## @return: Transform0
    ## @inputs: [frame = DataSource0]
    Transform0 = ApplyMapping.apply(frame = DataSource0, mappings = [("department", "string",
      "department", "string"), ("name", "string", "name", "string"), ("id", "int", "id", "int")], 
       transformation_ctx = "Transform0")
    ## @type: DataSink
    ## @args: [connection_type = "s3", format = "json", connection_options = {"path": 
     "s3://<S3 path>/", "partitionKeys": []}, transformation_ctx = "DataSink0"]
    ## @return: DataSink0
    ## @inputs: [frame = Transform0]
    DataSink0 = glueContext.write_dynamic_frame.from_options(frame = Transform0, 
      connection_type = "s3", format = "json", connection_options = {"path": 
      "s3://<S3 path>/", "partitionKeys": []}, transformation_ctx = "DataSink0")
    job.commit()
```

### Connection options for type custom\.athena or marketplace\.athena<a name="marketplace-athena-connect-options"></a>
+ `className` – String, required, driver class name\. When you're using the Athena\-CloudWatch connector, this parameter value is the prefix of the class Name \(for example, `"com.amazonaws.athena.connectors"`\)\. The Athena\-CloudWatch connector is composed of two classes: a metadata handler and a record handler\. If you supply the common prefix here, then the API loads the correct classes based on that prefix\.
+ `tableName` – String, required, the name of the CloudWatch log stream to read\. This code snippet uses the special view name `all_log_streams`, which means that the dynamic data frame returned will contain data from all log streams in the log group\.
+ `schemaName` – String, required, the name of the CloudWatch log group to read from\. For example, `/aws-glue/jobs/output`\.
+ `connectionName` – String, required, name of the connection that is associated with the connector\.

For additional options for this connector, see the [Amazon Athena CloudWatch Connector README](https://github.com/awslabs/aws-athena-query-federation/tree/master/athena-cloudwatch) file on GitHub\.

The following Python code example shows how to read from an Athena data store using an AWS Marketplace connector\. It demonstrates reading from Athena and writing to an S3 location\. 

```
    import sys
    from awsglue.transforms import *
    from awsglue.utils import getResolvedOptions
    from pyspark.context import SparkContext
    from awsglue.context import GlueContext
    from awsglue.job import Job
     
    ## @params: [JOB_NAME]
    args = getResolvedOptions(sys.argv, ['JOB_NAME'])
     
    sc = SparkContext()
    glueContext = GlueContext(sc)
    spark = glueContext.spark_session
    job = Job(glueContext)
    job.init(args['JOB_NAME'], args)
    ## @type: DataSource
    ## @args: [connection_type = "marketplace.athena", connection_options = 
     {"tableName":"all_log_streams","schemaName":"/aws-glue/jobs/output",
      "connectionName":"test-connection-athena"}, transformation_ctx = "DataSource0"]
    ## @return: DataSource0
    ## @inputs: []
    DataSource0 = glueContext.create_dynamic_frame.from_options(connection_type = 
      "marketplace.athena", connection_options = {"tableName":"all_log_streams",,
      "schemaName":"/aws-glue/jobs/output","connectionName":
      "test-connection-athena"}, transformation_ctx = "DataSource0")
    ## @type: ApplyMapping
    ## @args: [mappings = [("department", "string", "department", "string"), ("name", "string",
      "name", "string"), ("id", "int", "id", "int")], transformation_ctx = "Transform0"]
    ## @return: Transform0
    ## @inputs: [frame = DataSource0]
    Transform0 = ApplyMapping.apply(frame = DataSource0, mappings = [("department", "string",
      "department", "string"), ("name", "string", "name", "string"), ("id", "int", "id", "int")], 
       transformation_ctx = "Transform0")
    ## @type: DataSink
    ## @args: [connection_type = "s3", format = "json", connection_options = {"path": 
     "s3://<S3 path>/", "partitionKeys": []}, transformation_ctx = "DataSink0"]
    ## @return: DataSink0
    ## @inputs: [frame = Transform0]
    DataSink0 = glueContext.write_dynamic_frame.from_options(frame = Transform0, 
      connection_type = "s3", format = "json", connection_options = {"path": 
      "s3://<S3 path>/", "partitionKeys": []}, transformation_ctx = "DataSink0")
    job.commit()
```

### Connection options for type custom\.spark or marketplace\.spark<a name="marketplace-spark-connect-options"></a>
+ `className` – String, required, connector class name\. 
+ `secretId` – String, optional, used to retrieve credentials for the connector connection\.
+ `connectionName` – String, required, name of the connection that is associated with the connector\.
+ Other options depend on the data store\. For example, Elasticsearch configuration options start with the prefix `es`, as described in the [Elasticsearch for Apache Hadoop](https://www.elastic.co/guide/en/elasticsearch/hadoop/current/configuration.html) documentation\. Spark connections to Snowflake use options such as `sfUser` and `sfPassword`, as described in [Using the Spark Connector](https://docs.snowflake.com/en/user-guide/spark-connector-use.html) in the *Connecting to Snowflake* guide\.

The following Python code example shows how to read from an Elasticsearch data store using a `marketplace.spark` connection\.

```
    import sys
    from awsglue.transforms import *
    from awsglue.utils import getResolvedOptions
    from pyspark.context import SparkContext
    from awsglue.context import GlueContext
    from awsglue.job import Job
     
    ## @params: [JOB_NAME]
    args = getResolvedOptions(sys.argv, ['JOB_NAME'])
     
    sc = SparkContext()
    glueContext = GlueContext(sc)
    spark = glueContext.spark_session
    job = Job(glueContext)
    job.init(args['JOB_NAME'], args)
    ## @type: DataSource
    ## @args: [connection_type = "marketplace.spark", connection_options = {"path":"test",
      "es.nodes.wan.only":"true","es.nodes":"https://<AWS endpoint>",
      "connectionName":"test-spark-es","es.port":"443"}, transformation_ctx = "DataSource0"]
    ## @return: DataSource0
    ## @inputs: []
    DataSource0 = glueContext.create_dynamic_frame.from_options(connection_type = 
      "marketplace.spark", connection_options = {"path":"test","es.nodes.wan.only":
      "true","es.nodes":"https://<AWS endpoint>","connectionName":
      "test-spark-es","es.port":"443"}, transformation_ctx = "DataSource0")
    ## @type: DataSink
    ## @args: [connection_type = "s3", format = "json", connection_options = {"path": 
         "s3://<S3 path>/", "partitionKeys": []}, transformation_ctx = "DataSink0"]
    ## @return: DataSink0
    ## @inputs: [frame = DataSource0]
    DataSink0 = glueContext.write_dynamic_frame.from_options(frame = DataSource0, 
       connection_type = "s3", format = "json", connection_options = {"path": 
       "s3://<S3 path>/", "partitionKeys": []}, transformation_ctx = "DataSink0")
    job.commit()
```