# Tracking Processed Data Using Job Bookmarks<a name="monitor-continuations"></a>

AWS Glue tracks data that has already been processed during a previous run of an ETL job by persisting state information from the job run\. This persisted state information is called a *job bookmark*\. Job bookmarks help AWS Glue maintain state information and prevent the reprocessing of old data\. With job bookmarks, you can process new data when rerunning on a scheduled interval\.  A job bookmark is composed of the states for various elements of jobs, such as sources, transformations, and targets\. For example, your ETL job might read new partitions in an Amazon S3 file\. AWS Glue tracks which partitions the job has processed successfully to prevent duplicate processing and duplicate data in the job's target data store\.

Job bookmarks are implemented for some Amazon Simple Storage Service \(Amazon S3\) sources and the Relationalize transform\. The following table lists the Amazon S3 source formats that AWS Glue supports for job bookmarks\.


| AWS Glue version | Amazon S3 source formats | 
| --- | --- | 
| Version 0\.9 | JSON, CSV, Apache Avro, XML | 
| Version 1\.0 and later | JSON, CSV, Apache Avro, XML, Parquet, ORC | 

For information about Glue versions, see [Defining Job Properties](add-job.md#create-job)\.

Job bookmarks are implemented for a limited use case for a relational database \(JDBC connection\) input source\. For this input source, job bookmarks are supported only if the table's primary keys are in sequential order\. Also, job bookmarks search for new rows, but not updated rows\. This is because bookmarks look for the primary keys, which already exist\. 

**Topics**
+ [Using Job Bookmarks in AWS Glue](#monitor-continuations-implement)
+ [Using Job Bookmarks with the AWS Glue Generated Script](#monitor-continuations-script)
+ [Tracking Files Using Modification Timestamps](#monitor-continuations-timestamps)

## Using Job Bookmarks in AWS Glue<a name="monitor-continuations-implement"></a>

On the AWS Glue console, a job bookmark option is passed as a parameter when the job is started\. The following table describes the options for setting job bookmarks in AWS Glue\. 


****  

| Job bookmark | Description | 
| --- | --- | 
| Enable | Causes the job to update the state after a run to keep track of previously processed data\. If your job has a source with job bookmark support, it will keep track of processed data, and when a job runs, it processes new data since the last checkpoint\. | 
| Disable | Job bookmarks are not used, and the job always processes the entire dataset\. You are responsible for managing the output from previous job runs\. This is the default\. | 
| Pause | Process incremental data since the last successful run or the data in the range identified by the following sub\-options, without updating the state of last bookmark\. You are responsible for managing the output from previous job runs\. The two sub\-options are: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/monitor-continuations.html) The job bookmark state is not updated when this option set is specified\. The sub\-options are optional, however when used both the sub\-options needs to be provided\.  | 

For details about the parameters passed to a job, and specifically for a job bookmark, see [Special Parameters Used by AWS Glue](aws-glue-programming-etl-glue-arguments.md)\.

For Amazon S3 input sources, AWS Glue job bookmarks check the last modified time of the objects to verify which objects need to be reprocessed\. If your input source data has been modified since your last job run, the files are reprocessed when you run the job again\.

You can rewind your job bookmarks for your Glue Spark ETL jobs to any previous job run\. You can support data backfilling scenarios better by rewinding your job bookmarks to any previous job run, resulting in the subsequent job run reprocessing data only from the bookmarked job run\.

If you intend to reprocess all the data using the same job, reset the job bookmark\. To reset the job bookmark state, use the AWS Glue console, the [ResetJobBookmark Action \(Python: reset\_job\_bookmark\)](aws-glue-api-jobs-runs.md#aws-glue-api-jobs-runs-ResetJobBookmark) API operation, or the AWS CLI\. For example, enter the following command using the AWS CLI:

```
    aws glue reset-job-bookmark --job-name my-job-name
```

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

The following is an example of the generated script\. The script and its associated arguments illustrate the various elements that are required for using job bookmarks\. For more information about these elements see the [GlueContext Class](aws-glue-api-crawler-pyspark-extensions-glue-context.md) API, and the [DynamicFrameWriter Class](aws-glue-api-crawler-pyspark-extensions-dynamic-frame-writer.md) API\.

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
*job = Job(glueContext)*
*job.init(args['JOB_NAME'], args)*
## @type: DataSource
## @args: [database = "database", table_name = "relatedqueries_csv", transformation_ctx = "datasource0"]
## @return: datasource0
## @inputs: []
datasource0 = glueContext.create_dynamic_frame.from_catalog(database = "database", table_name = "relatedqueries_csv", *transformation_ctx = "datasource0")*
## @type: ApplyMapping
## @args: [mapping = [("col0", "string", "name", "string"), ("col1", "string", "number", "string")], transformation_ctx = "applymapping1"]
## @return: applymapping1
## @inputs: [frame = datasource0]
applymapping1 = ApplyMapping.apply(frame = datasource0, mappings = [("col0", "string", "name", "string"), ("col1", "string", "number", "string")], *transformation_ctx = "applymapping1"*)
## @type: DataSink
## @args: [connection_type = "s3", connection_options = {"path": "s3://input_path"}, format = "json", transformation_ctx = "datasink2"]
## @return: datasink2
## @inputs: [frame = applymapping1]
datasink2 = glueContext.write_dynamic_frame.from_options(frame = applymapping1, connection_type = "s3", connection_options = {"path": "s3://input_path"}, format = "json", *transformation_ctx = "datasink2"*)

*job.commit()*

Job Arguments :

*--job-bookmark-option, job-bookmark-enable*
*--JOB_NAME, name-1-s3-2-s3-encrypted*
```

## Tracking Files Using Modification Timestamps<a name="monitor-continuations-timestamps"></a>

For Amazon S3 input sources, AWS Glue job bookmarks check the last modified time of the files to verify which objects need to be reprocessed\. 

Consider the following example\. In the diagram, the X axis is a time axis, from left to right, with the left\-most point being T0\. The Y axis is list of files observed at time T\. The elements representing the list are placed in the graph based on their modification time\.

![\[The list of files observed at time T based on their modification time.\]](http://docs.aws.amazon.com/glue/latest/dg/images/monitor-continuations-timestamp.png)

In this example, when a job starts at modification timestamp 1 \(T1\), it looks for files that have a modification time greater than T0 and less than or equal to T1\. Those files are F2, F3, F4, and F5\. The job bookmark stores the timestamps T0 and T1 as the low and high timestamps respectively\.

When the job reruns at T2, it filters files that have a modification time greater than T1 and less than or equal to T2\. Those files are F7, F8, F9, and F10\. It thereby misses the files F3', F4', and F5'\. The reason that the files F3', F4', and F5', which have a modification time less than or equal to T1, show up after T1 is because of Amazon S3 list consistency\.

To account for Amazon S3 eventual consistency, AWS Glue includes a list of files \(or path hash\) in the job bookmark\. AWS Glue assumes that the Amazon S3 file list is only inconsistent up to a finite period \(dt\) before the current time\. That is, the file list for files with a modification time between T1 \- dt and T1 when listing is done at T1 is inconsistent\. However, the list of files with a modification time less than or equal to T1 \- d1 is consistent at a time greater than or equal to T1\.

You specify the period of time in which AWS Glue will save files \(and where the files are likely to be consistent\) by using the `MaxBand` option in the AWS Glue connection options\. The default value is 900 seconds \(15 minutes\)\. For more information about this property, see [Connection Types and Options for ETL in AWS Glue](aws-glue-programming-etl-connect.md)\.

When the job reruns at timestamp 2 \(T2\), it lists the files in the following ranges:
+ T1 \- dt \(exclusive\) to T1 \(inclusive\)\. This list includes F4, F5, F4', and F5'\. This list is a consistent range\. However, this range is inconsistent for a listing at T1 and has a list of files F3, F4, and F5 saved\. For getting the files to be processed at T2, the files F3, F4, and F5 will be removed\.
+ T1 \(exclusive\) to T2 \- dt \(inclusive\)\. This list includes F7 and F8\. This list is a consistent range\.
+ T2 \- dt \(exclusive\) \- T2 \(inclusive\)\. This list includes F9 and F10\. This list is an inconsistent range\.

The resultant list of files is F3', F4', F5', F7, F8, F9, and F10\.

The new files in the inconsistent list are F9 and F10, which are saved in the filter for the next run\.

For more information about Amazon S3 eventual consistency, see [Introduction to Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/Introduction.html#ConsistencyModel) in the *Amazon Simple Storage Service Developer Guide*\.

### Job Run Failures<a name="monitor-continuations-timestamps-failures"></a>

A job run version increments when a job fails\. For example, if a job run at timestamp 1 \(T1\) fails, and it is rerun at T2, it advances the high timestamp to T2\. Then, when the job is run at a later point T3, it advances the high timestamp to Amazon S3\.

If a job run fails before the `job.commit()` \(at T1\), the files are processed in a subsequent run, in which AWS Glue processes the files from T0 to T2\.