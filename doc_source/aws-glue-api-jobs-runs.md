# Job Runs<a name="aws-glue-api-jobs-runs"></a>

The Jobs Runs API describes the data types and API related to starting, stopping, or viewing job runs, and resetting job bookmarks, in AWS Glue\.

## Data Types<a name="aws-glue-api-jobs-runs-objects"></a>
+ [JobRun Structure](#aws-glue-api-jobs-runs-JobRun)
+ [Predecessor Structure](#aws-glue-api-jobs-runs-Predecessor)
+ [JobBookmarkEntry Structure](#aws-glue-api-jobs-runs-JobBookmarkEntry)
+ [BatchStopJobRunSuccessfulSubmission Structure](#aws-glue-api-jobs-runs-BatchStopJobRunSuccessfulSubmission)
+ [BatchStopJobRunError Structure](#aws-glue-api-jobs-runs-BatchStopJobRunError)

## JobRun Structure<a name="aws-glue-api-jobs-runs-JobRun"></a>

Contains information about a job run\.

**Fields**
+ `Id` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of this job run\.
+ `Attempt` – Number \(integer\)\.

  The number of the attempt to run this job\.
+ `PreviousRunId` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the previous run of this job\. For example, the `JobRunId` specified in the `StartJobRun` action\.
+ `TriggerName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the trigger that started this job run\.
+ `JobName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job definition being used in this run\.
+ `StartedOn` – Timestamp\.

  The date and time at which this job run was started\.
+ `LastModifiedOn` – Timestamp\.

  The last time that this job run was modified\.
+ `CompletedOn` – Timestamp\.

  The date and time that this job run completed\.
+ `JobRunState` – UTF\-8 string \(valid values: `STARTING` \| `RUNNING` \| `STOPPING` \| `STOPPED` \| `SUCCEEDED` \| `FAILED` \| `TIMEOUT`\)\.

  The current state of the job run\.
+ `Arguments` – A map array of key\-value pairs\.

  Each key is a UTF\-8 string\.

  Each value is a UTF\-8 string\.

  The job arguments associated with this run\. For this job run, they replace the default arguments set in the job definition itself\.

  You can specify arguments here that your own job\-execution script consumes, as well as arguments that AWS Glue itself consumes\.

  For information about how to specify and consume your own job arguments, see the [Calling AWS Glue APIs in Python](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-calling.html) topic in the developer guide\.

  For information about the key\-value pairs that AWS Glue consumes to set up your job, see the [Special Parameters Used by AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-glue-arguments.html) topic in the developer guide\.
+ `ErrorMessage` – UTF\-8 string\.

  An error message associated with this job run\.
+ `PredecessorRuns` – An array of [Predecessor](#aws-glue-api-jobs-runs-Predecessor) objects\.

  A list of predecessors to this job run\.
+ `AllocatedCapacity` – Number \(integer\)\.

  This field is deprecated\. Use `MaxCapacity` instead\.

  The number of AWS Glue data processing units \(DPUs\) allocated to this JobRun\. From 2 to 100 DPUs can be allocated; the default is 10\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://aws.amazon.com/glue/pricing/)\.
+ `ExecutionTime` – Number \(integer\)\.

  The amount of time \(in seconds\) that the job run consumed resources\.
+ `Timeout` – Number \(integer\), at least 1\.

  The `JobRun` timeout in minutes\. This is the maximum time that a job run can consume resources before it is terminated and enters `TIMEOUT` status\. The default is 2,880 minutes \(48 hours\)\. This overrides the timeout value set in the parent job\.
+ `MaxCapacity` – Number \(double\)\.

  The number of AWS Glue data processing units \(DPUs\) that can be allocated when this job runs\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://docs.aws.amazon.com/https://aws.amazon.com/glue/pricing/)\.

  Do not set `Max Capacity` if using `WorkerType` and `NumberOfWorkers`\.

  The value that can be allocated for `MaxCapacity` depends on whether you are running a Python shell job or an Apache Spark ETL job:
  + When you specify a Python shell job \(`JobCommand.Name`="pythonshell"\), you can allocate either 0\.0625 or 1 DPU\. The default is 0\.0625 DPU\.
  + When you specify an Apache Spark ETL job \(`JobCommand.Name`="glueetl"\), you can allocate from 2 to 100 DPUs\. The default is 10 DPUs\. This job type cannot have a fractional DPU allocation\.
+ `WorkerType` – UTF\-8 string \(valid values: `Standard=""` \| `G.1X=""` \| `G.2X=""`\)\.

  The type of predefined worker that is allocated when a job runs\. Accepts a value of Standard, G\.1X, or G\.2X\.
  + For the `Standard` worker type, each worker provides 4 vCPU, 16 GB of memory and a 50GB disk, and 2 executors per worker\.
  + For the `G.1X` worker type, each worker provides 4 vCPU, 16 GB of memory and a 64GB disk, and 1 executor per worker\.
  + For the `G.2X` worker type, each worker provides 8 vCPU, 32 GB of memory and a 128GB disk, and 1 executor per worker\.
+ `NumberOfWorkers` – Number \(integer\)\.

  The number of workers of a defined `workerType` that are allocated when a job runs\.

  The maximum number of workers you can define are 299 for `G.1X`, and 149 for `G.2X`\. 
+ `SecurityConfiguration` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the `SecurityConfiguration` structure to be used with this job run\.
+ `LogGroupName` – UTF\-8 string\.

  The name of the log group for secure logging that can be server\-side encrypted in Amazon CloudWatch using AWS KMS\. This name can be `/aws-glue/jobs/`, in which case the default encryption is `NONE`\. If you add a role name and `SecurityConfiguration` name \(in other words, `/aws-glue/jobs-yourRoleName-yourSecurityConfigurationName/`\), then that security configuration is used to encrypt the log group\.
+ `NotificationProperty` – A [NotificationProperty](aws-glue-api-jobs-job.md#aws-glue-api-jobs-job-NotificationProperty) object\.

  Specifies configuration properties of a job run notification\.
+ `GlueVersion` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #13](aws-glue-api-common.md#regex_13)\.

  Glue version determines the versions of Apache Spark and Python that AWS Glue supports\. The Python version indicates the version supported for jobs of type Spark\. 

  For more information about the available AWS Glue versions and corresponding Spark and Python versions, see [Glue version](https://docs.aws.amazon.com/glue/latest/dg/add-job.html) in the developer guide\.

  Jobs that are created without specifying a Glue version default to Glue 0\.9\.

## Predecessor Structure<a name="aws-glue-api-jobs-runs-Predecessor"></a>

A job run that was used in the predicate of a conditional trigger that triggered this job run\.

**Fields**
+ `JobName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job definition used by the predecessor job run\.
+ `RunId` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The job\-run ID of the predecessor job run\.

## JobBookmarkEntry Structure<a name="aws-glue-api-jobs-runs-JobBookmarkEntry"></a>

Defines a point that a job can resume processing\.

**Fields**
+ `JobName` – UTF\-8 string\.

  The name of the job in question\.
+ `Version` – Number \(integer\)\.

  The version of the job\.
+ `Run` – Number \(integer\)\.

  The run ID number\.
+ `Attempt` – Number \(integer\)\.

  The attempt ID number\.
+ `PreviousRunId` – UTF\-8 string\.

  The unique run identifier associated with the previous job run\.
+ `RunId` – UTF\-8 string\.

  The run ID number\.
+ `JobBookmark` – UTF\-8 string\.

  The bookmark itself\.

## BatchStopJobRunSuccessfulSubmission Structure<a name="aws-glue-api-jobs-runs-BatchStopJobRunSuccessfulSubmission"></a>

Records a successful request to stop a specified `JobRun`\.

**Fields**
+ `JobName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job definition used in the job run that was stopped\.
+ `JobRunId` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The `JobRunId` of the job run that was stopped\.

## BatchStopJobRunError Structure<a name="aws-glue-api-jobs-runs-BatchStopJobRunError"></a>

Records an error that occurred when attempting to stop a specified job run\.

**Fields**
+ `JobName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job definition that is used in the job run in question\.
+ `JobRunId` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The `JobRunId` of the job run in question\.
+ `ErrorDetail` – An [ErrorDetail](aws-glue-api-common.md#aws-glue-api-common-ErrorDetail) object\.

  Specifies details about the error that was encountered\.

## Operations<a name="aws-glue-api-jobs-runs-actions"></a>
+ [StartJobRun Action \(Python: start\_job\_run\)](#aws-glue-api-jobs-runs-StartJobRun)
+ [BatchStopJobRun Action \(Python: batch\_stop\_job\_run\)](#aws-glue-api-jobs-runs-BatchStopJobRun)
+ [GetJobRun Action \(Python: get\_job\_run\)](#aws-glue-api-jobs-runs-GetJobRun)
+ [GetJobRuns Action \(Python: get\_job\_runs\)](#aws-glue-api-jobs-runs-GetJobRuns)
+ [GetJobBookmark Action \(Python: get\_job\_bookmark\)](#aws-glue-api-jobs-runs-GetJobBookmark)
+ [GetJobBookmarks Action \(Python: get\_job\_bookmarks\)](#aws-glue-api-jobs-runs-GetJobBookmarks)
+ [ResetJobBookmark Action \(Python: reset\_job\_bookmark\)](#aws-glue-api-jobs-runs-ResetJobBookmark)

## StartJobRun Action \(Python: start\_job\_run\)<a name="aws-glue-api-jobs-runs-StartJobRun"></a>

Starts a job run using a job definition\.

**Request**
+ `JobName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job definition to use\.
+ `JobRunId` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of a previous `JobRun` to retry\.
+ `Arguments` – A map array of key\-value pairs\.

  Each key is a UTF\-8 string\.

  Each value is a UTF\-8 string\.

  The job arguments specifically for this run\. For this job run, they replace the default arguments set in the job definition itself\.

  You can specify arguments here that your own job\-execution script consumes, as well as arguments that AWS Glue itself consumes\.

  For information about how to specify and consume your own Job arguments, see the [Calling AWS Glue APIs in Python](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-calling.html) topic in the developer guide\.

  For information about the key\-value pairs that AWS Glue consumes to set up your job, see the [Special Parameters Used by AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-glue-arguments.html) topic in the developer guide\.
+ `AllocatedCapacity` – Number \(integer\)\.

  This field is deprecated\. Use `MaxCapacity` instead\.

  The number of AWS Glue data processing units \(DPUs\) to allocate to this JobRun\. From 2 to 100 DPUs can be allocated; the default is 10\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://docs.aws.amazon.com/https://aws.amazon.com/glue/pricing/)\.
+ `Timeout` – Number \(integer\), at least 1\.

  The `JobRun` timeout in minutes\. This is the maximum time that a job run can consume resources before it is terminated and enters `TIMEOUT` status\. The default is 2,880 minutes \(48 hours\)\. This overrides the timeout value set in the parent job\.
+ `MaxCapacity` – Number \(double\)\.

  The number of AWS Glue data processing units \(DPUs\) that can be allocated when this job runs\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://docs.aws.amazon.com/https://aws.amazon.com/glue/pricing/)\.

  Do not set `Max Capacity` if using `WorkerType` and `NumberOfWorkers`\.

  The value that can be allocated for `MaxCapacity` depends on whether you are running a Python shell job, or an Apache Spark ETL job:
  + When you specify a Python shell job \(`JobCommand.Name`="pythonshell"\), you can allocate either 0\.0625 or 1 DPU\. The default is 0\.0625 DPU\.
  + When you specify an Apache Spark ETL job \(`JobCommand.Name`="glueetl"\), you can allocate from 2 to 100 DPUs\. The default is 10 DPUs\. This job type cannot have a fractional DPU allocation\.
+ `SecurityConfiguration` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the `SecurityConfiguration` structure to be used with this job run\.
+ `NotificationProperty` – A [NotificationProperty](aws-glue-api-jobs-job.md#aws-glue-api-jobs-job-NotificationProperty) object\.

  Specifies configuration properties of a job run notification\.
+ `WorkerType` – UTF\-8 string \(valid values: `Standard=""` \| `G.1X=""` \| `G.2X=""`\)\.

  The type of predefined worker that is allocated when a job runs\. Accepts a value of Standard, G\.1X, or G\.2X\.
  + For the `Standard` worker type, each worker provides 4 vCPU, 16 GB of memory and a 50GB disk, and 2 executors per worker\.
  + For the `G.1X` worker type, each worker provides 4 vCPU, 16 GB of memory and a 64GB disk, and 1 executor per worker\.
  + For the `G.2X` worker type, each worker provides 8 vCPU, 32 GB of memory and a 128GB disk, and 1 executor per worker\.
+ `NumberOfWorkers` – Number \(integer\)\.

  The number of workers of a defined `workerType` that are allocated when a job runs\.

  The maximum number of workers you can define are 299 for `G.1X`, and 149 for `G.2X`\. 

**Response**
+ `JobRunId` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID assigned to this job run\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `ResourceNumberLimitExceededException`
+ `ConcurrentRunsExceededException`

## BatchStopJobRun Action \(Python: batch\_stop\_job\_run\)<a name="aws-glue-api-jobs-runs-BatchStopJobRun"></a>

Stops one or more job runs for a specified job definition\.

**Request**
+ `JobName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job definition for which to stop job runs\.
+ `JobRunIds` – *Required:* An array of UTF\-8 strings, not less than 1 or more than 25 strings\.

  A list of the `JobRunIds` that should be stopped for that job definition\.

**Response**
+ `SuccessfulSubmissions` – An array of [BatchStopJobRunSuccessfulSubmission](#aws-glue-api-jobs-runs-BatchStopJobRunSuccessfulSubmission) objects\.

  A list of the JobRuns that were successfully submitted for stopping\.
+ `Errors` – An array of [BatchStopJobRunError](#aws-glue-api-jobs-runs-BatchStopJobRunError) objects\.

  A list of the errors that were encountered in trying to stop `JobRuns`, including the `JobRunId` for which each error was encountered and details about the error\.

**Errors**
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## GetJobRun Action \(Python: get\_job\_run\)<a name="aws-glue-api-jobs-runs-GetJobRun"></a>

Retrieves the metadata for a given job run\.

**Request**
+ `JobName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the job definition being run\.
+ `RunId` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the job run\.
+ `PredecessorsIncluded` – Boolean\.

  True if a list of predecessor runs should be returned\.

**Response**
+ `JobRun` – A [JobRun](#aws-glue-api-jobs-runs-JobRun) object\.

  The requested job\-run metadata\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## GetJobRuns Action \(Python: get\_job\_runs\)<a name="aws-glue-api-jobs-runs-GetJobRuns"></a>

Retrieves metadata for all runs of a given job definition\.

**Request**
+ `JobName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job definition for which to retrieve all job runs\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation call\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum size of the response\.

**Response**
+ `JobRuns` – An array of [JobRun](#aws-glue-api-jobs-runs-JobRun) objects\.

  A list of job\-run metadata objects\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if not all requested job runs have been returned\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## GetJobBookmark Action \(Python: get\_job\_bookmark\)<a name="aws-glue-api-jobs-runs-GetJobBookmark"></a>

Returns information on a job bookmark entry\.

**Request**
+ `JobName` – *Required:* UTF\-8 string\.

  The name of the job in question\.
+ `Version` – Number \(integer\)\.

  The version of the job\.
+ `RunId` – UTF\-8 string\.

  The unique run identifier associated with this job run\.

**Response**
+ `JobBookmarkEntry` – A [JobBookmarkEntry](#aws-glue-api-jobs-runs-JobBookmarkEntry) object\.

  A structure that defines a point that a job can resume processing\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `ValidationException`

## GetJobBookmarks Action \(Python: get\_job\_bookmarks\)<a name="aws-glue-api-jobs-runs-GetJobBookmarks"></a>

Returns information on the job bookmark entries\. The list is ordered on decreasing version numbers\.

**Request**
+ `JobName` – *Required:* UTF\-8 string\.

  The name of the job in question\.
+ `MaxResults` – Number \(integer\)\.

  The maximum size of the response\.
+ `NextToken` – Number \(integer\)\.

  A continuation token, if this is a continuation call\.

**Response**
+ `JobBookmarkEntries` – An array of [JobBookmarkEntry](#aws-glue-api-jobs-runs-JobBookmarkEntry) objects\.

  A list of job bookmark entries that defines a point that a job can resume processing\.
+ `NextToken` – Number \(integer\)\.

  A continuation token, which has a value of 1 if all the entries are returned, or > 1 if not all requested job runs have been returned\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## ResetJobBookmark Action \(Python: reset\_job\_bookmark\)<a name="aws-glue-api-jobs-runs-ResetJobBookmark"></a>

Resets a bookmark entry\.

**Request**
+ `JobName` – *Required:* UTF\-8 string\.

  The name of the job in question\.
+ `RunId` – UTF\-8 string\.

  The unique run identifier associated with this job run\.

**Response**
+ `JobBookmarkEntry` – A [JobBookmarkEntry](#aws-glue-api-jobs-runs-JobBookmarkEntry) object\.

  The reset bookmark entry\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`