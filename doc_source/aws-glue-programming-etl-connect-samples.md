# Examples: Setting Connection Types and Options<a name="aws-glue-programming-etl-connect-samples"></a>

The sample code in this section demonstrates how to set connection types and connection options when connecting to extract, transform, and load \(ETL\) sources and sinks\. The code shows how to specify connection types and connection options in both Python and Scala for connections to MongoDB and Amazon DocumentDB \(with MongoDB compatibility\)\. The code is similar for connecting to other data stores that AWS Glue supports\.

**Note**  
When you create an ETL job that connects to Amazon DocumentDB, for the `Connections` job property, you must designate a connection object that specifies the virtual private cloud \(VPC\) in which Amazon DocumentDB is running\. For the connection object, the connection type must be `JDBC`, and the `JDBC URL` must be `mongo://<DocumentDB_host>:27017`\.

**Topics**
+ [Connecting with Python](#etl-connect-with-python)
+ [Connecting with Scala](#etl-connect-with-scala)

## Connecting with Python<a name="etl-connect-with-python"></a>

The following Python script demonstrates using connection types and connection options for reading and writing to MongoDB and Amazon DocumentDB\.

```
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext, SparkConf
from awsglue.context import GlueContext
from awsglue.job import Job
import time

## @params: [JOB_NAME]
args = getResolvedOptions(sys.argv, ['JOB_NAME'])

sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session

job = Job(glueContext)
job.init(args['JOB_NAME'], args)

output_path = "s3://some_bucket/output/" + str(time.time()) + "/"
mongo_uri = "mongodb://<mongo-instanced-ip-address>:27017"
mongo_ssl_uri = "mongodb://<mongo-instanced-ip-address>:27017"
documentdb_uri = "mongodb://<mongo-instanced-ip-address>:27017"
write_uri = "mongodb://<mongo-instanced-ip-address>:27017"
documentdb_write_uri = "mongodb://<mongo-instanced-ip-address>:27017"

read_docdb_options = {
    "uri": documentdb_uri,
    "database": "test",
    "collection": "coll",
    "username": "username",
    "password": "1234567890",
    "ssl": "true",
    "ssl.domain_match": "false",
    "partitioner": "MongoSamplePartitioner",
    "partitionerOptions.partitionSizeMB": "10",
    "partitionerOptions.partitionKey": "_id"
}

read_mongo_options = {
    "uri": mongo_uri,
    "database": "test",
    "collection": "coll",
    "username": "username",
    "password": "pwd",
    "partitioner": "MongoSamplePartitioner",
    "partitionerOptions.partitionSizeMB": "10",
    "partitionerOptions.partitionKey": "_id"}

ssl_mongo_options = {
    "uri": mongo_ssl_uri,
    "database": "test",
    "collection": "coll",
    "ssl": "true",
    "ssl.domain_match": "false"
}

write_mongo_options = {
    "uri": write_uri,
    "database": "test",
    "collection": "coll",
    "username": "username",
    "password": "pwd"
}
write_documentdb_options = {
    "uri": documentdb_write_uri,
    "database": "test",
    "collection": "coll",
    "username": "username",
    "password": "pwd"
}

# Get DynamicFrame from MongoDB and DocumentDB
dynamic_frame = glueContext.create_dynamic_frame.from_options(connection_type="mongodb",
                                                              connection_options=read_mongo_options)
dynamic_frame2 = glueContext.create_dynamic_frame.from_options(connection_type="documentdb",
                                                               connection_options=read_docdb_options)

# Write DynamicFrame to MongoDB and DocumentDB
glueContext.write_dynamic_frame.from_options(dynamic_frame, connection_type="mongodb", connection_options=write_mongo_options)
glueContext.write_dynamic_frame.from_options(dynamic_frame2, connection_type="documentdb",
                                             connection_options=write_documentdb_options)

job.commit()
```

## Connecting with Scala<a name="etl-connect-with-scala"></a>

The following Scala script demonstrates using connection types and connection options for reading and writing to MongoDB and Amazon DocumentDB\.

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
  val DEFAULT_URI: String = "mongodb://<mongo-instanced-ip-address>:27017"
  val WRITE_URI: String = "mongodb://<mongo-instanced-ip-address>:27017"
  val DOC_URI: String = "mongodb://<mongo-instanced-ip-address>:27017"
  val DOC_WRITE_URI: String = "mongodb://<mongo-instanced-ip-address>:27017"
  lazy val defaultJsonOption = jsonOptions(DEFAULT_URI)
  lazy val documentDBJsonOption = jsonOptions(DOC_URI)
  lazy val writeJsonOption = jsonOptions(WRITE_URI)
  lazy val writeDocumentDBJsonOption = jsonOptions(DOC_WRITE_URI)
  def main(sysArgs: Array[String]): Unit = {
    val spark: SparkContext = new SparkContext()
    val glueContext: GlueContext = new GlueContext(spark)
    val args = GlueArgParser.getResolvedOptions(sysArgs, Seq("JOB_NAME").toArray)
    Job.init(args("JOB_NAME"), glueContext, args.asJava)

    // Get DynamicFrame from MongoDB and DocumentDB
    val resultFrame: DynamicFrame = glueContext.getSource("mongodb", defaultJsonOption).getDynamicFrame()
    val resultFrame2: DynamicFrame = glueContext.getSource("documentdb", documentDBJsonOption).getDynamicFrame()

    // Write DynamicFrame to MongoDB and DocumentDB
    glueContext.getSink("mongodb", writeJsonOption).writeDynamicFrame(resultFrame)
    glueContext.getSink("documentdb", writeJsonOption).writeDynamicFrame(resultFrame2)

    Job.commit()
  }

  private def jsonOptions(uri: String): JsonOptions = {
    new JsonOptions(
      s"""{"uri": "${uri}",
         |"database":"test",
         |"collection":"coll",
         |"username": "username",
         |"password": "pwd",
         |"ssl":"true",
         |"ssl.domain_match":"false",
         |"partitioner": "MongoSamplePartitioner",
         |"partitionerOptions.partitionSizeMB": "10",
         |"partitionerOptions.partitionKey": "_id"}""".stripMargin)
  }
}
```

For more information, see the following:
+ [Connection Types and Options for ETL in AWS Glue](aws-glue-programming-etl-connect.md)
+ [Amazon DocumentDB Developer Guide](https://docs.aws.amazon.com/documentdb/latest/developerguide)