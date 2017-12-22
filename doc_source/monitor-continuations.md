# Job Bookmarks<a name="monitor-continuations"></a>

AWS Glue keeps track of data that has already been processed by a previous run of an extract, transform, and load \(ETL\) job\. This persisted state information is called a *job bookmark*\.  A job bookmark is composed of the states for various elements of jobs, such as sources, transformations, and targets\. Currently, job bookmarks are implemented for Amazon Simple Storage Service \(Amazon S3\) sources and the Relationalize transform\. For example, your ETL job might read new partitions in an Amazon S3 file\. AWS Glue tracks which partitions have successfully been processed by the job to prevent duplicate processing and duplicate data in the job's target data store\. 

In the AWS Glue console, a job bookmark option is passed as a parameter when the job is started\.  


****  

| Job bookmark | Description | 
| --- | --- | 
| Enable | Keep track of previously processed data\. When a job runs, process new data since the last checkpoint\. | 
| Disable | Always process the entire dataset\. You are responsible for managing the output from previous job runs\. | 
| Pause | Process incremental data since the last run\. Don’t update the state information so that every subsequent run processes data since the last bookmark\. You are responsible for managing the output from previous job runs\. | 

For details about the parameters passed to a job, and specifically for a job bookmark, see [Special Parameters Used by AWS Glue](aws-glue-programming-python-glue-arguments.md)\.