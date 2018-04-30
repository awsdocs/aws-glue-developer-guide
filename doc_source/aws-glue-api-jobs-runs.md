# Job Runs<a name="aws-glue-api-jobs-runs"></a>

## Data Types<a name="aws-glue-api-jobs-runs-objects"></a>
+ [JobRun Structure](#aws-glue-api-jobs-runs-JobRun)
+ [Predecessor Structure](#aws-glue-api-jobs-runs-Predecessor)
+ [JobBookmarkEntry Structure](#aws-glue-api-jobs-runs-JobBookmarkEntry)
+ [BatchStopJobRunSuccessfulSubmission Structure](#aws-glue-api-jobs-runs-BatchStopJobRunSuccessfulSubmission)
+ [BatchStopJobRunError Structure](#aws-glue-api-jobs-runs-BatchStopJobRunError)

## JobRun Structure<a name="aws-glue-api-jobs-runs-JobRun"></a>

Contains information about a job run\.

**Fields**
+ `Id` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of this job run\.
+ `Attempt` – Number \(integer\)\.

  The number of the attempt to run this job\.
+ `PreviousRunId` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the previous run of this job\. For example, the JobRunId specified in the StartJobRun action\.
+ `TriggerName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the trigger that started this job run\.
+ `JobName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job definition being used in this run\.
+ `StartedOn` – Timestamp\.

  The date and time at which this job run was started\.
+ `LastModifiedOn` – Timestamp\.

  The last time this job run was modified\.
+ `CompletedOn` – Timestamp\.

  The date and time this job run completed\.
+ `JobRunState` – String \(valid values: `STARTING` \| `RUNNING` \| `STOPPING` \| `STOPPED` \| `SUCCEEDED` \| `FAILED` \| `TIMEOUT`\)\.

  The current state of the job run\.
+ `Arguments` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\.

  The job arguments associated with this run\. These override equivalent default arguments set for the job\.

  You can specify arguments here that your own job\-execution script consumes, as well as arguments that AWS Glue itself consumes\.

  For information about how to specify and consume your own job arguments, see the [Calling AWS Glue APIs in Python](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-calling.html) topic in the developer guide\.

  For information about the key\-value pairs that AWS Glue consumes to set up your job, see the [Special Parameters Used by AWS Glue](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-glue-arguments.html) topic in the developer guide\.
+ `ErrorMessage` – String\.

  An error message associated with this job run\.
+ `PredecessorRuns` – An array of [Predecessor](#aws-glue-api-jobs-runs-Predecessor)s\.

  A list of predecessors to this job run\.
+ `AllocatedCapacity` – Number \(integer\)\.

  The number of AWS Glue data processing units \(DPUs\) allocated to this JobRun\. From 2 to 100 DPUs can be allocated; the default is 10\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://aws.amazon.com/glue/pricing/)\.
+ `ExecutionTime` – Number \(Integer\)\.

  The amount of time \(in seconds\) that the job run consumed resources\.
+ `Timeout` – Number \(integer\)\.

  The JobRun timeout in minutes\. This is the maximum time that a job run can consume resources before it is terminated and enters `TIMEOUT` status\. The default is 2,880 minutes \(48 hours\)\. This overrides the timeout value set in the parent job\.

## Predecessor Structure<a name="aws-glue-api-jobs-runs-Predecessor"></a>

A job run that was used in the predicate of a conditional trigger that triggered this job run\.

**Fields**
+ `JobName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job definition used by the predecessor job run\.
+ `RunId` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The job\-run ID of the predecessor job run\.

## JobBookmarkEntry Structure<a name="aws-glue-api-jobs-runs-JobBookmarkEntry"></a>

Defines a point which a job can resume processing\.

**Fields**
+ `JobName` – String\.

  Name of the job in question\.
+ `Version` – Number \(integer\)\.

  Version of the job\.
+ `Run` – Number \(integer\)\.

  The run ID number\.
+ `Attempt` – Number \(integer\)\.

  The attempt ID number\.
+ `JobBookmark` – String\.

  The bookmark itself\.

## BatchStopJobRunSuccessfulSubmission Structure<a name="aws-glue-api-jobs-runs-BatchStopJobRunSuccessfulSubmission"></a>

Records a successful request to stop a specified JobRun\.

**Fields**
+ `JobName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job definition used in the job run that was stopped\.
+ `JobRunId` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The JobRunId of the job run that was stopped\.

## BatchStopJobRunError Structure<a name="aws-glue-api-jobs-runs-BatchStopJobRunError"></a>

Records an error that occurred when attempting to stop a specified job run\.

**Fields**
+ `JobName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job definition used in the job run in question\.
+ `JobRunId` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The JobRunId of the job run in question\.
+ `ErrorDetail` – An ErrorDetail object\.

  Specifies details about the error that was encountered\.

## Operations<a name="aws-glue-api-jobs-runs-actions"></a>
+ [StartJobRun Action \(Python: start\_job\_run\)](#aws-glue-api-jobs-runs-StartJobRun)
+ [BatchStopJobRun Action \(Python: batch\_stop\_job\_run\)](#aws-glue-api-jobs-runs-BatchStopJobRun)
+ [GetJobRun Action \(Python: get\_job\_run\)](#aws-glue-api-jobs-runs-GetJobRun)
+ [GetJobRuns Action \(Python: get\_job\_runs\)](#aws-glue-api-jobs-runs-GetJobRuns)
+ [ResetJobBookmark Action \(Python: reset\_job\_bookmark\)](#aws-glue-api-jobs-runs-ResetJobBookmark)

## StartJobRun Action \(Python: start\_job\_run\)<a name="aws-glue-api-jobs-runs-StartJobRun"></a>

Starts a job run using a job definition\.

**Request**
+ `JobName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the job definition to use\.
+ `JobRunId` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of a previous JobRun to retry\.
+ `Arguments` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\.

  The job arguments specifically for this run\. They override the equivalent default arguments set for in the job definition itself\.

  You can specify arguments here that your own job\-execution script consumes, as well as arguments that AWS Glue itself consumes\.

  For information about how to specify and consume your own Job arguments, see the [Calling AWS Glue APIs in Python](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-calling.html) topic in the developer guide\.

  For information about the key\-value pairs that AWS Glue consumes to set up your job, see the [Special Parameters Used by AWS Glue](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-glue-arguments.html) topic in the developer guide\.
+ `AllocatedCapacity` – Number \(integer\)\.

  The number of AWS Glue data processing units \(DPUs\) to allocate to this JobRun\. From 2 to 100 DPUs can be allocated; the default is 10\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://aws.amazon.com/glue/pricing/)\.
+ `Timeout` – Number \(integer\)\.

  The JobRun timeout in minutes\. This is the maximum time that a job run can consume resources before it is terminated and enters `TIMEOUT` status\. The default is 2,880 minutes \(48 hours\)\. This overrides the timeout value set in the parent job\.

**Response**
+ `JobRunId` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

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
+ `JobName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the job definition for which to stop job runs\.
+ `JobRunIds` – An array of UTF\-8 strings\. Required\.

  A list of the JobRunIds that should be stopped for that job definition\.

**Response**
+ `SuccessfulSubmissions` – An array of [BatchStopJobRunSuccessfulSubmission](#aws-glue-api-jobs-runs-BatchStopJobRunSuccessfulSubmission)s\.

  A list of the JobRuns that were successfully submitted for stopping\.
+ `Errors` – An array of [BatchStopJobRunError](#aws-glue-api-jobs-runs-BatchStopJobRunError)s\.

  A list of the errors that were encountered in tryng to stop JobRuns, including the JobRunId for which each error was encountered and details about the error\.

**Errors**
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## GetJobRun Action \(Python: get\_job\_run\)<a name="aws-glue-api-jobs-runs-GetJobRun"></a>

Retrieves the metadata for a given job run\.

**Request**
+ `JobName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  Name of the job definition being run\.
+ `RunId` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The ID of the job run\.
+ `PredecessorsIncluded` – Boolean\.

  True if a list of predecessor runs should be returned\.

**Response**
+ `JobRun` – A JobRun object\.

  The requested job\-run metadata\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## GetJobRuns Action \(Python: get\_job\_runs\)<a name="aws-glue-api-jobs-runs-GetJobRuns"></a>

Retrieves metadata for all runs of a given job definition\.

**Request**
+ `JobName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the job definition for which to retrieve all job runs\.
+ `NextToken` – String\.

  A continuation token, if this is a continuation call\.
+ `MaxResults` – Number \(integer\)\.

  The maximum size of the response\.

**Response**
+ `JobRuns` – An array of [JobRun](#aws-glue-api-jobs-runs-JobRun)s\.

  A list of job\-run metatdata objects\.
+ `NextToken` – String\.

  A continuation token, if not all reequested job runs have been returned\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## ResetJobBookmark Action \(Python: reset\_job\_bookmark\)<a name="aws-glue-api-jobs-runs-ResetJobBookmark"></a>

Resets a bookmark entry\.

**Request**
+ `JobName` – String\. Required\.

  The name of the job in question\.

**Response**
+ `JobBookmarkEntry` – A JobBookmarkEntry object\.

  The reset bookmark entry\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`