# Job Bookmarks<a name="monitor-continuations"></a>

AWS Glue keeps track of data that has already been processed by a previous run of an extract, transform, and load \(ETL\) job\. This persisted state information is called a *job bookmark*\.  A job bookmark is composed of the states for various elements of jobs, such as sources, transformations, and targets\. For example, your ETL job might read new partitions in an Amazon S3 file\. AWS Glue tracks which partitions have successfully been processed by the job to prevent duplicate processing and duplicate data in the job's target data store\.

Currently, job bookmarks are implemented for some Amazon Simple Storage Service \(Amazon S3\) sources and the Relationalize transform\. AWS Glue supports job bookmarks for Amazon S3 source formats of JSON, CSV, Avro, and XML\. Parquet and ORC are not supported\.

In the AWS Glue console, a job bookmark option is passed as a parameter when the job is started\.  


****  

| Job bookmark | Description | 
| --- | --- | 
| Enable | Keep track of previously processed data\. When a job runs, process new data since the last checkpoint\. | 
| Disable | Always process the entire dataset\. You are responsible for managing the output from previous job runs\. This is the default\. | 
| Pause | Process incremental data since the last run\. Don’t update the state information so that every subsequent run processes data since the last bookmark\. You are responsible for managing the output from previous job runs\. | 

For details about the parameters passed to a job, and specifically for a job bookmark, see [Special Parameters Used by AWS Glue](aws-glue-programming-etl-glue-arguments.md)\.

AWS Glue keeps track of job bookmarks by job\. If you delete a job, the job bookmark is deleted\. 

Many of the AWS Glue PySpark dynamic frame methods include an optional parameter named `transformation_ctx`, which is used to identify state information for a job bookmark\. For job bookmarks to work properly, enable the job bookmark parameter and set the `transformation_ctx`\. If you do not pass in the `transformation_ctx` parameter, then job bookmarks are not enabled for a dynamic frame or table used in the method\. For example, if you have an ETL job that reads and joins two Amazon S3 sources, you might choose to pass the `transformation_ctx` parameter only to those methods that you want to enable bookmarks\. If you reset the job bookmark for a job, it resets all transformations associated with the job regardless of the `transformation_ctx` used\. For more information about the `DynamicFrameReader` class, see [DynamicFrameReader Class](aws-glue-api-crawler-pyspark-extensions-dynamic-frame-reader.md)\. For more information about PySpark extensions, see [AWS Glue PySpark Extensions Reference](aws-glue-programming-python-extensions.md)\. 