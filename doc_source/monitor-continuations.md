# Tracking Processed Data Using Job Bookmarks<a name="monitor-continuations"></a>

AWS Glue tracks data that has already been processed during a previous run of an ETL job by persisting state information from the job run\. This persisted state information is called a *job bookmark*\. Job bookmarks help AWS Glue maintain state information and prevent the reprocessing of old data\. With job bookmarks, you can process new data when rerunning on a scheduled interval\. A job bookmark is composed of the states for various elements of jobs, such as sources, transformations, and targets\. For example, your ETL job might read new partitions in an Amazon S3 file\. AWS Glue tracks which partitions the job has processed successfully to prevent duplicate processing and duplicate data in the job's target data store\.

Job bookmarks are implemented for JDBC data sources, the Relationalize transform, and some Amazon Simple Storage Service \(Amazon S3\) sources\. The following table lists the Amazon S3 source formats that AWS Glue supports for job bookmarks\.


| AWS Glue version | Amazon S3 source formats | 
| --- | --- | 
| Version 0\.9 | JSON, CSV, Apache Avro, XML | 
| Version 1\.0 and later | JSON, CSV, Apache Avro, XML, Parquet, ORC | 

For information about AWS Glue versions, see [Defining Job Properties for Spark Jobs](add-job.md#create-job)\.

For JDBC sources, the following rules apply:
+ For each table, AWS Glue uses one or more columns as bookmark keys to determine new and processed data\. The bookmark keys combine to form a single compound key\.
+ You can specify the columns to use as bookmark keys\. If you don't specify bookmark keys, AWS Glue by default uses the primary key as the bookmark key, provided that it is sequentially increasing or decreasing \(with no gaps\)\.
+ If user\-defined bookmarks keys are used, they must be strictly monotonically increasing or decreasing\. Gaps are permitted\.
+ AWS Glue doesn't support using case\-sensitive columns as job bookmark keys\.

**Topics**
+ [Using Job Bookmarks in AWS Glue](#monitor-continuations-implement)
+ [Using Job Bookmarks with the AWS Glue Generated Script](#monitor-continuations-script)

## Using Job Bookmarks in AWS Glue<a name="monitor-continuations-implement"></a>

The job bookmark option is passed as a parameter when the job is started\. The following table describes the options for setting job bookmarks on the AWS Glue console\.


****  

| Job bookmark | Description | 
| --- | --- | 
| Enable | Causes the job to update the state after a run to keep track of previously processed data\. If your job has a source with job bookmark support, it will keep track of processed data, and when a job runs, it processes new data since the last checkpoint\. | 
| Disable | Job bookmarks are not used, and the job always processes the entire dataset\. You are responsible for managing the output from previous job runs\. This is the default\. | 
| Pause |  Process incremental data since the last successful run or the data in the range identified by the following sub\-options, without updating the state of last bookmark\. You are responsible for managing the output from previous job runs\. The two sub\-options are: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/monitor-continuations.html) The job bookmark state is not updated when this option set is specified\. The sub\-options are optional, however when used both the sub\-options needs to be provided\.  | 

For details about the parameters passed to a job on the command line, and specifically for job bookmarks, see [Special Parameters Used by AWS Glue](aws-glue-programming-etl-glue-arguments.md)\.

For Amazon S3 input sources, AWS Glue job bookmarks check the last modified time of the objects to verify which objects need to be reprocessed\. If your input source data has been modified since your last job run, the files are reprocessed when you run the job again\.

You can rewind your job bookmarks for your AWS Glue Spark ETL jobs to any previous job run\. You can support data backfilling scenarios better by rewinding your job bookmarks to any previous job run, resulting in the subsequent job run reprocessing data only from the bookmarked job run\.

If you intend to reprocess all the data using the same job, reset the job bookmark\. To reset the job bookmark state, use the AWS Glue console, the [ResetJobBookmark Action \(Python: reset\_job\_bookmark\)](aws-glue-api-jobs-runs.md#aws-glue-api-jobs-runs-ResetJobBookmark) API operation, or the AWS CLI\. For example, enter the following command using the AWS CLI:

```
    aws glue reset-job-bookmark --job-name my-job-name
```

When you rewind or reset a bookmark, AWS Glue does not clean the target files because there could be multiple targets and targets are not tracked with job bookmarks\. Only source files are tracked with job bookmarks\. You can create different output targets when rewinding and reprocessing the source files to avoid duplicate data in your output\.

AWS Glue keeps track of job bookmarks by job\. If you delete a job, the job bookmark is deleted\.

In some cases, you might have enabled AWS Glue job bookmarks but your ETL job is reprocessing data that was already processed in an earlier run\. For information about resolving common causes of this error, see [Troubleshooting Errors in AWS Glue](glue-troubleshooting-errors.md)\.

### Transformation Context<a name="monitor-continuations-implement-context"></a>

Many of the AWS Glue PySpark dynamic frame methods include an optional parameter named `transformation_ctx`, which is a unique identifier for the ETL operator instance\. The `transformation_ctx` parameter is used to identify state information within a job bookmark for the given operator\. Specifically, AWS Glue uses `transformation_ctx` to index the key to the bookmark state\. 

For job bookmarks to work properly, enable the job bookmark parameter and set the `transformation_ctx` parameter\. If you don't pass in the `transformation_ctx` parameter, then job bookmarks are not enabled for a dynamic frame or a table used in the method\. For example, if you have an ETL job that reads and joins two Amazon S3 sources, you might choose to pass the `transformation_ctx` parameter only to those methods that you want to enable bookmarks\. If you reset the job bookmark for a job, it resets all transformations that are associated with the job regardless of the `transformation_ctx` used\. 

For more information about the `DynamicFrameReader` class, see [DynamicFrameReader Class](aws-glue-api-crawler-pyspark-extensions-dynamic-frame-reader.md)\. For more information about PySpark extensions, see [AWS Glue PySpark Extensions Reference](aws-glue-programming-python-extensions.md)\. 

## Using Job Bookmarks with the AWS Glue Generated Script<a name="monitor-continuations-script"></a>

This section describes more of the operational details of using job bookmarks\. It also provides an example of a script that you can generate from AWS Glue when you choose a source and destination and run a job\.

Job bookmarks store the states for a job\. Each instance of the state is keyed by a job name and a version number\. When a script invokes `job.init`, it retrieves its state and always gets the latest version\. Within a state, there are multiple state elements, which are specific to each source, transformation, and sink instance in the script\. These state elements are identified by a transformation context that is attached to the corresponding element \(source, transformation, or sink\) in the script\. The state elements are saved atomically when `job.commit` is invoked from the user script\. The script gets the job name and the control option for the job bookmarks from the arguments\.

The state elements in the job bookmark are source, transformation, or sink\-specific data\. For example, suppose that you want to read incremental data from an Amazon S3 location that is being constantly written to by an upstream job or process\. In this case, the script must determine what has been processed so far\. The job bookmark implementation for the Amazon S3 source saves information so that when the job runs again, it can filter only the new objects using the saved information and recompute the state for the next run of the job\. A timestamp is used to filter the new files\.

In addition to the state elements, job bookmarks have a *run number*, an *attempt number*, and a *version number*\. The run number tracks the run of the job, and the attempt number records the attempts for a job run\. The job run number is a monotonically increasing number that is incremented for every successful run\. The attempt number tracks the attempts for each run, and is only incremented when there is a run after a failed attempt\. The version number increases monotonically and tracks the updates to a job bookmark\.

**Example**  
The following is an example of a generated script for an Amazon S3 data source\. The portions of the script that are required for using job bookmarks are shown in bold and italics\. For more information about these elements see the [GlueContext Class](aws-glue-api-crawler-pyspark-extensions-glue-context.md) API, and the [DynamicFrameWriter Class](aws-glue-api-crawler-pyspark-extensions-dynamic-frame-writer.md) API\.  

```
# Sample Script
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
## @args: [database = "database", table_name = "relatedqueries_csv", transformation_ctx = "datasource0"]
## @return: datasource0
## @inputs: []
datasource0 = glueContext.create_dynamic_frame.from_catalog(database = "database", table_name = "relatedqueries_csv", transformation_ctx = "datasource0")
## @type: ApplyMapping
## @args: [mapping = [("col0", "string", "name", "string"), ("col1", "string", "number", "string")], transformation_ctx = "applymapping1"]
## @return: applymapping1
## @inputs: [frame = datasource0]
applymapping1 = ApplyMapping.apply(frame = datasource0, mappings = [("col0", "string", "name", "string"), ("col1", "string", "number", "string")], transformation_ctx = "applymapping1")
## @type: DataSink
## @args: [connection_type = "s3", connection_options = {"path": "s3://input_path"}, format = "json", transformation_ctx = "datasink2"]
## @return: datasink2
## @inputs: [frame = applymapping1]
datasink2 = glueContext.write_dynamic_frame.from_options(frame = applymapping1, connection_type = "s3", connection_options = {"path": "s3://input_path"}, format = "json", transformation_ctx = "datasink2")

job.commit()
```

**Example**  
The following is an example of a generated script for a JDBC source\. The source table is an employee table with the `empno` column as the primary key\. Although by default the job uses a sequential primary key as the bookmark key if no bookmark key is specified, because `empno` is not necessarily sequential—there could be gaps in the values—it does not qualify as a default bookmark key\. Therefore, the script explicitly designates `empno` as the bookmark key\. That portion of the code is shown in bold and italics\.  

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
## @args: [database = "hr", table_name = "emp", transformation_ctx = "datasource0"]
## @return: datasource0
## @inputs: []
datasource0 = glueContext.create_dynamic_frame.from_catalog(database = "hr", table_name = "emp", transformation_ctx = "datasource0", additional_options = {"jobBookmarkKeys":["empno"],"jobBookmarkKeysSortOrder":"asc"})
## @type: ApplyMapping
## @args: [mapping = [("ename", "string", "ename", "string"), ("hrly_rate", "decimal(38,0)", "hrly_rate", "decimal(38,0)"), ("comm", "decimal(7,2)", "comm", "decimal(7,2)"), ("hiredate", "timestamp", "hiredate", "timestamp"), ("empno", "decimal(5,0)", "empno", "decimal(5,0)"), ("mgr", "decimal(5,0)", "mgr", "decimal(5,0)"), ("photo", "string", "photo", "string"), ("job", "string", "job", "string"), ("deptno", "decimal(3,0)", "deptno", "decimal(3,0)"), ("ssn", "decimal(9,0)", "ssn", "decimal(9,0)"), ("sal", "decimal(7,2)", "sal", "decimal(7,2)")], transformation_ctx = "applymapping1"]
## @return: applymapping1
## @inputs: [frame = datasource0]
applymapping1 = ApplyMapping.apply(frame = datasource0, mappings = [("ename", "string", "ename", "string"), ("hrly_rate", "decimal(38,0)", "hrly_rate", "decimal(38,0)"), ("comm", "decimal(7,2)", "comm", "decimal(7,2)"), ("hiredate", "timestamp", "hiredate", "timestamp"), ("empno", "decimal(5,0)", "empno", "decimal(5,0)"), ("mgr", "decimal(5,0)", "mgr", "decimal(5,0)"), ("photo", "string", "photo", "string"), ("job", "string", "job", "string"), ("deptno", "decimal(3,0)", "deptno", "decimal(3,0)"), ("ssn", "decimal(9,0)", "ssn", "decimal(9,0)"), ("sal", "decimal(7,2)", "sal", "decimal(7,2)")], transformation_ctx = "applymapping1")
## @type: DataSink
## @args: [connection_type = "s3", connection_options = {"path": "s3://hr/employees"}, format = "csv", transformation_ctx = "datasink2"]
## @return: datasink2
## @inputs: [frame = applymapping1]
datasink2 = glueContext.write_dynamic_frame.from_options(frame = applymapping1, connection_type = "s3", connection_options = {"path": "s3://hr/employees"}, format = "csv", transformation_ctx = "datasink2")
job.commit()
```

For more information about connection options related to job bookmarks, see [JDBC connectionType Values](aws-glue-programming-etl-connect.md#aws-glue-programming-etl-connect-jdbc)\.