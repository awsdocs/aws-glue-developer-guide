# Adding Streaming ETL Jobs in AWS Glue<a name="add-job-streaming"></a>

You can create streaming extract, transform, and load \(ETL\) jobs that run continuously, consume data from streaming sources like Amazon Kinesis Data Streams, Apache Kafka, and Amazon Managed Streaming for Apache Kafka \(Amazon MSK\)\. The jobs cleanse and transform the data, and then load the results into Amazon S3 data lakes or JDBC data stores\.

By default, AWS Glue processes and writes out data in 100\-second windows\. This allows data to be processed efficiently and permits aggregations to be performed on data arriving later than expected\. You can modify this window size to increase timeliness or aggregation accuracy\. AWS Glue streaming jobs use checkpoints rather than job bookmarks to track the data that has been read\.

**Note**  
AWS Glue bills hourly for streaming ETL jobs while they are running\.

Creating a streaming ETL job involves the following steps:

1. For an Apache Kafka streaming source, create an AWS Glue connection to the Kafka source or the Amazon MSK cluster\.

1. Manually create a Data Catalog table for the streaming source\.

1. Create an ETL job for the streaming data source\. Define streaming\-specific job properties, and supply your own script or optionally modify the generated script\.

For more information, see [Streaming ETL in AWS Glue](components-overview.md#streaming-etl-intro)\.

When creating a streaming ETL job for Amazon Kinesis Data Streams, you don't have to create an AWS Glue connection\. However, if there is a connection attached to the AWS Glue streaming ETL job that has Kinesis Data Streams as a source, then a virtual private cloud \(VPC\) endpoint to Kinesis is required\. For more information, see [Creating an interface endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) in the *Amazon VPC User Guide*\. When specifying a Amazon Kinesis Data Streams stream in another account, you must setup the roles and policies to allow cross\-account access\. For more information, see [ Example: Read From a Kinesis Stream in a Different Account](https://docs.aws.amazon.com/kinesisanalytics/latest/java/examples-cross.html)\.

**Topics**
+ [Creating an AWS Glue Connection for an Apache Kafka Data Stream](#create-conn-streaming)
+ [Creating a Data Catalog Table for a Streaming Source](#create-table-streaming)
+ [Defining Job Properties for a Streaming ETL Job](#create-job-streaming-properties)
+ [Streaming ETL Notes and Restrictions](#create-job-streaming-restrictions)

## Creating an AWS Glue Connection for an Apache Kafka Data Stream<a name="create-conn-streaming"></a>

To read from an Apache Kafka stream, you must create an AWS Glue connection\. 

**To create an AWS Glue connection for a Kafka source \(Console\)**

1. Open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, under **Data catalog**, choose **Connections**\.

1. Choose **Add connection**, and on the **Set up your connection’s properties** page, enter a connection name\.

1. For **Connection type**, choose **Kafka**\.

1. For **Kafka bootstrap servers URLs**, enter the host and port number for the bootstrap brokers for your Amazon MSK cluster or Apache Kafka cluster\. Use only Transport Layer Security \(TLS\) endpoints for establishing the initial connection to the Kafka cluster\. Plaintext endpoints are not supported\.

   The following is an example list of hostname and port number pairs for an Amazon MSK cluster\.

   ```
   myserver1.kafka.us-east-1.amazonaws.com:9094,myserver2.kafka.us-east-1.amazonaws.com:9094,
   myserver3.kafka.us-east-1.amazonaws.com:9094
   ```

   For more information about getting the bootstrap broker information, see [Getting the Bootstrap Brokers for an Amazon MSK Cluster](https://docs.aws.amazon.com/msk/latest/developerguide/msk-get-bootstrap-brokers.html) in the *Amazon Managed Streaming for Apache Kafka Developer Guide*\. 

1. If you want a secure connection to the Kafka data source, select **Require SSL connection**, and for **Kafka private CA certificate location**, enter a valid Amazon S3 path to a custom SSL certificate\.

   For an SSL connection to self\-managed Kafka, the custom certificate is mandatory\. It's optional for Amazon MSK\.

   For more information about specifying a custom certificate for Kafka, see [AWS Glue Connection SSL Properties](connection-defining.md#connection-properties-SSL)\.

1. Optionally enter a description, and then choose **Next**\.

1. For an Amazon MSK cluster, specify its virtual private cloud \(VPC\), subnet, and security group\. The VPC information is optional for self\-managed Kafka\.

1. Choose **Next** to review all connection properties, and then choose **Finish**\.

For more information about AWS Glue connections, see [AWS Glue Connections](connection-using.md)\.

## Creating a Data Catalog Table for a Streaming Source<a name="create-table-streaming"></a>

Before creating a streaming ETL job, you must manually create a Data Catalog table that specifies source data stream properties, including the data schema\. This table is used as the data source for the streaming ETL job\. 

If you don't know the schema of the data in the source data stream, you can create the table without a schema\. Then when you create the streaming ETL job, you can turn on the AWS Glue schema detection function\. AWS Glue determines the schema from the streaming data\.

Use the [AWS Glue console](https://console.aws.amazon.com/glue/), the AWS Command Line Interface \(AWS CLI\), or the AWS Glue API to create the table\. For information about creating a table manually with the AWS Glue console, see [Defining Tables in the AWS Glue Data Catalog](tables-described.md)\.

**Note**  
You can't use the AWS Lake Formation console to create the table; you must use the AWS Glue console\.

When creating the table, set the following streaming ETL properties \(console\)\.

**Type of Source**  
**Kinesis** or **Kafka**

**For a Kinesis source in the same account:**    
**Region**  
The AWS Region where the Amazon Kinesis Data Streams service resides\. The Region and Kinesis stream name are together translated to a Stream ARN\.  
Example: https://kinesis\.us\-east\-1\.amazonaws\.com  
**Kinesis stream name**  
Stream name as described in [Creating a Stream](https://docs.aws.amazon.com/streams/latest/dev/kinesis-using-sdk-java-create-stream.html) in the* Amazon Kinesis Data Streams Developer Guide*\.

**For a Kinesis source in another account, refer to [this example](https://docs.aws.amazon.com/kinesisanalytics/latest/java/examples-cross.html) to set up the roles and policies to allow cross\-account access\. Configure these settings:**    
**Stream ARN**  
The ARN of the Kinesis data stream that the consumer is registered with\. For more information, see [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *AWS General Reference*\.  
**Assumed Role ARN**  
The Amazon Resource Name \(ARN\) of the role to assume\.  
**Session name \(optional\)**  
An identifier for the assumed role session\.  
Use the role session name to uniquely identify a session when the same role is assumed by different principals or for different reasons\. In cross\-account scenarios, the role session name is visible to, and can be logged by the account that owns the role\. The role session name is also used in the ARN of the assumed role principal\. This means that subsequent cross\-account API requests that use the temporary security credentials will expose the role session name to the external account in their AWS CloudTrail logs\.

**For a Kafka source:**    
**Topic name**  
Topic name as specified in Kafka\.  
**Connection**  
An AWS Glue connection that references a Kafka source, as described in [Creating an AWS Glue Connection for an Apache Kafka Data Stream](#create-conn-streaming)\.

**To set streaming ETL properties for Amazon Kinesis Data Streams \(AWS Glue API or AWS CLI\)**
+ To set up streaming ETL properties for a Kinesis source in the same account, specify the `streamName` and `endpointUrl` parameters in the `StorageDescriptor` structure of the `CreateTable` API operation or the `create_table` CLI command\.

  ```
  "StorageDescriptor": {
  	"Parameters": {
  		"typeOfData": "kinesis",
  		"streamName": "sample-stream",
  		"endpointUrl": "https://kinesis.us-east-1.amazonaws.com"
  	}
  	...
  }
  ```

  Or, specify the `streamARN`\.  
**Example**  

  ```
  "StorageDescriptor": {
  	"Parameters": {
  		"typeOfData": "kinesis",
  		"streamARN": "arn:aws:kinesis:us-east-1:123456789:stream/sample-stream"
  	}
  	...
  }
  ```
+ To set up streaming ETL properties for a Kinesis source in another account, specify the `streamARN`, `awsSTSRoleARN` and `awsSTSSessionName` \(optional\) parameters in the `StorageDescriptor` structure in the `CreateTable` API operation or the `create_table` CLI command\.

  ```
  "StorageDescriptor": {
  	"Parameters": {
  		"typeOfData": "kinesis",
  		"streamARN": "arn:aws:kinesis:us-east-1:123456789:stream/sample-stream",
  		"awsSTSRoleARN": "arn:aws:iam::123456789:role/sample-assume-role-arn",
  		"awsSTSSessionName": "optional-session"
  	}
  	...
  }
  ```

Also consider the following information for streaming sources in Avro format or for log data that you can apply Grok patterns to\. 
+ [Notes and Restrictions for Avro Streaming Sources](#streaming-avro-notes)
+ [Applying Grok Patterns to Streaming Sources](#create-table-streaming-grok)

### Notes and Restrictions for Avro Streaming Sources<a name="streaming-avro-notes"></a>

The following notes and restrictions apply for streaming sources in the Avro format:
+ When schema detection is turned on, the Avro schema must be included in the payload\. When turned off, the payload should contain only data\.
+ Some Avro data types are not supported in dynamic frames\. You can't specify these data types when defining the schema with the **Define a schema** page in the create table wizard in the AWS Glue console\. During schema detection, unsupported types in the Avro schema are converted to supported types as follows:
  + `EnumType => StringType`
  + `FixedType => BinaryType`
  + `UnionType => StructType`
+ If you define the table schema using the **Define a schema** page in the console, the implied root element type for the schema is `record`\. If you want a root element type other than `record`, for example `array` or `map`, you can't specify the schema using the **Define a schema** page\. Instead you must skip that page and specify the schema either as a table property or within the ETL script\.
  + To specify the schema in the table properties, complete the create table wizard, edit the table details, and add a new key\-value pair under **Table properties**\. Use the key `avroSchema`, and enter a schema JSON object for the value, as shown in the following screenshot\.  
![\[Under the Table properties heading, there are two columns of text fields. The left-hand column heading is Key, and the right-hand column heading is Value. The key/value pair in the first row is classification/avro. The key/value pair in the second row is avroSchema/{"type":"array","items":"string"}.\]](http://docs.aws.amazon.com/glue/latest/dg/images/table_properties_avro.png)
  + To specify the schema in the ETL script, modify the `datasource0` assignment statement and add the `avroSchema` key to the `additional_options` argument, as shown in the following Python and Scala examples\.

------
#### [ Python ]

    ```
    SCHEMA_STRING = ‘{"type":"array","items":"string"}’
    datasource0 = glueContext.create_data_frame.from_catalog(database = "database", table_name = "table_name", transformation_ctx = "datasource0", additional_options = {"startingPosition": "TRIM_HORIZON", "inferSchema": "false", "avroSchema": SCHEMA_STRING})
    ```

------
#### [ Scala ]

    ```
    val SCHEMA_STRING = """{"type":"array","items":"string"}"""
    val datasource0 = glueContext.getCatalogSource(database = "database", tableName = "table_name", redshiftTmpDir = "", transformationContext = "datasource0", additionalOptions = JsonOptions(s"""{"startingPosition": "TRIM_HORIZON", "inferSchema": "false", "avroSchema":"$SCHEMA_STRING"}""")).getDataFrame()
    ```

------

### Applying Grok Patterns to Streaming Sources<a name="create-table-streaming-grok"></a>

You can create a streaming ETL job for a log data source and use Grok patterns to convert the logs to structured data\. The ETL job then processes the data as a structured data source\. You specify the Grok patterns to apply when you create the Data Catalog table for the streaming source\.

For information about Grok patterns and custom pattern string values, see [Writing Grok Custom Classifiers](custom-classifier.md#custom-classifier-grok)\.

**To add Grok patterns to the Data Catalog table \(console\)**
+ Use the create table wizard, and create the table with the parameters specified in [Creating a Data Catalog Table for a Streaming Source](#create-table-streaming)\. Specify the data format as Grok, fill in the **Grok pattern** field, and optionally add custom patterns under **Custom patterns \(optional\)**\.  
![\[*\]](http://docs.aws.amazon.com/glue/latest/dg/images/grok-data-format-create-table.png)

  Press **Enter** after each custom pattern\.

**To add Grok patterns to the Data Catalog table \(AWS Glue API or AWS CLI\)**
+ Add the `GrokPattern` parameter and optionally the `CustomPatterns` parameter to the `CreateTable` API operation or the `create_table` CLI command\.

  ```
   "Parameters": {
  ...
      "grokPattern": "string",
      "grokCustomPatterns": "string",
  ...
  },
  ```

  Express `grokCustomPatterns` as a string and use "\\n" as the separator between patterns\.

  The following is an example of specifying these parameters\.  
**Example**  

  ```
  "parameters": {
  ...
      "grokPattern": "%{USERNAME:username} %{DIGIT:digit:int}",
      "grokCustomPatterns": "digit \d",
  ...
  }
  ```

## Defining Job Properties for a Streaming ETL Job<a name="create-job-streaming-properties"></a>

When you define a streaming ETL job in the AWS Glue console, provide the following streams\-specific properties\. For descriptions of additional job properties, see [Defining Job Properties for Spark Jobs](add-job.md#create-job)\. For more information about adding a job using the AWS Glue console, see [Working with Jobs on the AWS Glue Console](console-jobs.md)\. 

**IAM role**  
Specify the AWS Identity and Access Management \(IAM\) role that is used for authorization to resources that are used to run the job, access streaming sources, and access target data stores\.  
For access to Amazon Kinesis Data Streams, attach the `AmazonKinesisFullAccess` AWS managed policy to the role, or attach a similar IAM policy that permits more fine\-grained access\. For sample policies, see [Controlling Access to Amazon Kinesis Data Streams Resources Using IAM](https://docs.aws.amazon.com/streams/latest/dev/controlling-access.html)\.  
For more information about permissions for running jobs in AWS Glue, see [Managing Access Permissions for AWS Glue Resources](access-control-overview.md)\.

**Type**  
Choose **Spark streaming**\.

**AWS Glue version**  
The AWS Glue version determines the versions of Apache Spark, and Python or Scala, that are available to the job\. Choose a selection for Glue Version 1\.0 or Glue Version 2\.0 that specifies the version of Python or Scala available to the job\. AWS Glue Version 2\.0 with Python 3 support is the default for streaming ETL jobs\.

**Job timeout**  
Optionally enter a duration in minutes\. The default value is blank, which means the job might run indefinitely\. 

**Data source**  
Specify the table that you created in [Creating a Data Catalog Table for a Streaming Source](#create-table-streaming)\.

**Data target**  
Do one of the following:  
+ Choose **Create tables in your data target** and specify the following data target properties\.  
**Data store**  
Choose Amazon S3 or JDBC\.  
**Format**  
Choose any format\. All are supported for streaming\.
+ Choose **Use tables in the data catalog and update your data target**, and choose a table for a JDBC data store\.

**Output schema definition**  
Do one of the following:  
+ Choose **Automatically detect schema of each record** to turn on schema detection\. AWS Glue determines the schema from the streaming data\.
+ Choose **Specify output schema for all records** to use the Apply Mapping transform to define the output schema\.

**Script**  
Optionally supply your own script or modify the generated script to perform operations that the Apache Spark Structured Streaming engine supports\. For information on the available operations, see [Operations on streaming DataFrames/Datasets](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html#operations-on-streaming-dataframesdatasets)\.

## Streaming ETL Notes and Restrictions<a name="create-job-streaming-restrictions"></a>

Keep in mind the following notes and restrictions:
+ When using schema detection, you cannot perform joins of streaming data\.
+ Your ETL script can use AWS Glue’s built\-in transforms and the transforms native to Apache Spark Structured Streaming\. For more information, see [Operations on streaming DataFrames/Datasets](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html#operations-on-streaming-dataframesdatasets) on the Apache Spark website or [Built\-In Transforms](built-in-transforms.md)\.
+ AWS Glue streaming ETL jobs use checkpoints to keep track of the data that has been read\. Therefore, a stopped and restarted job picks up where it left off in the stream\. If you want to reprocess data, you can delete the checkpoint folder referenced in the script\.
+ Job bookmarks aren't supported\.
+ You can't change the number of shards of an Amazon Kinesis data stream if an AWS Glue streaming job is running and consuming data from that stream\. Stop the job first, modify the stream shards, and then restart the job\.
+ You cannot register a job as a consumer for the enhanced fan\-out feature of Kinesis Data Streams\. 