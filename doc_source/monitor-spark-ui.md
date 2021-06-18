# Monitoring Jobs Using the Apache Spark Web UI<a name="monitor-spark-ui"></a>

You can use the Apache Spark web UI to monitor and debug AWS Glue ETL jobs running on the AWS Glue job system, and also Spark applications running on AWS Glue development endpoints\. The Spark UI enables you to check the following for each job:
+ The event timeline of each Spark stage
+ A directed acyclic graph \(DAG\) of the job
+ Physical and logical plans for SparkSQL queries
+ The underlying Spark environmental variables for each job

You can enable the Spark UI using the AWS Glue console or the AWS Command Line Interface \(AWS CLI\)\. When you enable the Spark UI, AWS Glue ETL jobs and Spark applications on AWS Glue development endpoints can persist Spark event logs to a location that you specify in Amazon Simple Storage Service \(Amazon S3\)\. AWS Glue also provides a sample AWS CloudFormation template to start the Spark history server and show the Spark UI using the event logs\. The persisted event logs in Amazon S3 can be used with the Spark UI both in real time as the job is executing and after the job is complete\.

The following is an example of a Spark application which reads from two data sources, performs a join transform, and writes it out to Amazon S3 in Parquet format\.

```
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job
from pyspark.sql.functions import count, when, expr, col, sum, isnull
from pyspark.sql.functions import countDistinct
from awsglue.dynamicframe import DynamicFrame
 
args = getResolvedOptions(sys.argv, ['JOB_NAME'])
 
sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session
 
job = Job(glueContext)
job.init(args['JOB_NAME'])
 
df_persons = spark.read.json("s3://awsglue-datasets/examples/us-legislators/all/persons.json")
df_memberships = spark.read.json("s3://awsglue-datasets/examples/us-legislators/all/memberships.json")
 
df_joined = df_persons.join(df_memberships, df_persons.id == df_memberships.person_id, 'fullouter')
df_joined.write.parquet("s3://aws-glue-demo-sparkui/output/")
 
job.commit()
```

The following DAG visualization shows the different stages in this Spark job\.

![\[Screenshot of Spark UI showing 2 completed stages for job 0.\]](http://docs.aws.amazon.com/glue/latest/dg/images/spark-ui1.png)

The following event timeline for a job shows the start, execution, and termination of different Spark executors\.

![\[Screenshot of Spark UI showing the completed, failed, and active stages of different Spark executors.\]](http://docs.aws.amazon.com/glue/latest/dg/images/spark-ui2.png)

The following screen shows the details of the SparkSQL query plans:
+ Parsed logical plan
+ Analyzed logical plan
+ Optimized logical plan
+ Physical plan for execution

![\[SparkSQL query plans: parsed, analyzed, and optimized logical plan and physical plans for execution.\]](http://docs.aws.amazon.com/glue/latest/dg/images/spark-ui3.png)

You can still use AWS Glue continuous logging to view the Spark application log streams for Spark driver and executors\. For more information, see [Continuous Logging for AWS Glue Jobs](https://docs.aws.amazon.com/glue/latest/dg/monitor-continuous-logging.html)\.

**Topics**
+ [Enabling the Apache Spark Web UI for AWS Glue Jobs](monitor-spark-ui-jobs.md)
+ [Enabling the Apache Spark Web UI for Development Endpoints](monitor-spark-ui-dev-endpoints.md)
+ [Launching the Spark History Server](monitor-spark-ui-history.md)