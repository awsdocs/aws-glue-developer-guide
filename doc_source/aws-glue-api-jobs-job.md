# Jobs<a name="aws-glue-api-jobs-job"></a>

The Jobs API describes the data types and API related to creating, updating, deleting, or viewing jobs in AWS Glue\.

## Data Types<a name="aws-glue-api-jobs-job-objects"></a>
+ [Job Structure](#aws-glue-api-jobs-job-Job)
+ [ExecutionProperty Structure](#aws-glue-api-jobs-job-ExecutionProperty)
+ [NotificationProperty Structure](#aws-glue-api-jobs-job-NotificationProperty)
+ [JobCommand Structure](#aws-glue-api-jobs-job-JobCommand)
+ [ConnectionsList Structure](#aws-glue-api-jobs-job-ConnectionsList)
+ [JobUpdate Structure](#aws-glue-api-jobs-job-JobUpdate)

## Job Structure<a name="aws-glue-api-jobs-job-Job"></a>

Specifies a job definition\.

**Fields**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name you assign to this job definition\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of the job\.
+ `LogUri` – UTF\-8 string\.

  This field is reserved for future use\.
+ `Role` – UTF\-8 string\.

  The name or Amazon Resource Name \(ARN\) of the IAM role associated with this job\.
+ `CreatedOn` – Timestamp\.

  The time and date that this job definition was created\.
+ `LastModifiedOn` – Timestamp\.

  The last point in time when this job definition was modified\.
+ `ExecutionProperty` – An [ExecutionProperty](#aws-glue-api-jobs-job-ExecutionProperty) object\.

  An `ExecutionProperty` specifying the maximum number of concurrent runs allowed for this job\.
+ `Command` – A [JobCommand](#aws-glue-api-jobs-job-JobCommand) object\.

  The `JobCommand` that executes this job\.
+ `DefaultArguments` – A map array of key\-value pairs\.

  Each key is a UTF\-8 string\.

  Each value is a UTF\-8 string\.

  The default arguments for this job, specified as name\-value pairs\.

  You can specify arguments here that your own job\-execution script consumes, as well as arguments that AWS Glue itself consumes\.

  For information about how to specify and consume your own Job arguments, see the [Calling AWS Glue APIs in Python](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-calling.html) topic in the developer guide\.

  For information about the key\-value pairs that AWS Glue consumes to set up your job, see the [Special Parameters Used by AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-glue-arguments.html) topic in the developer guide\.
+ `Connections` – A [ConnectionsList](#aws-glue-api-jobs-job-ConnectionsList) object\.

  The connections used for this job\.
+ `MaxRetries` – Number \(integer\)\.

  The maximum number of times to retry this job after a JobRun fails\.
+ `AllocatedCapacity` – Number \(integer\)\.

  This field is deprecated\. Use `MaxCapacity` instead\.

  The number of AWS Glue data processing units \(DPUs\) allocated to runs of this job\. You can allocate from 2 to 100 DPUs; the default is 10\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://aws.amazon.com/glue/pricing/)\.
+ `Timeout` – Number \(integer\), at least 1\.

  The job timeout in minutes\. This is the maximum time that a job run can consume resources before it is terminated and enters `TIMEOUT` status\. The default is 2,880 minutes \(48 hours\)\.
+ `MaxCapacity` – Number \(double\)\.

  The number of AWS Glue data processing units \(DPUs\) that can be allocated when this job runs\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://aws.amazon.com/glue/pricing/)\.

  Do not set `Max Capacity` if using `WorkerType` and `NumberOfWorkers`\.

  The value that can be allocated for `MaxCapacity` depends on whether you are running a Python shell job or an Apache Spark ETL job:
  + When you specify a Python shell job \(`JobCommand.Name`="pythonshell"\), you can allocate either 0\.0625 or 1 DPU\. The default is 0\.0625 DPU\.
  + When you specify an Apache Spark ETL job \(`JobCommand.Name`="glueetl"\), you can allocate from 2 to 100 DPUs\. The default is 10 DPUs\. This job type cannot have a fractional DPU allocation\.
+ `WorkerType` – UTF\-8 string \(valid values: `Standard=""` \| `G.1X=""` \| `G.2X=""`\)\.

  The type of predefined worker that is allocated when a job runs\. Accepts a value of Standard, G\.1X, or G\.2X\.
  + For the `Standard` worker type, each worker provides 4 vCPU, 16 GB of memory and a 50GB disk, and 2 executors per worker\.
  + For the `G.1X` worker type, each worker maps to 1 DPU \(4 vCPU, 16 GB of memory, 64 GB disk\), and provides 1 executor per worker\. We recommend this worker type for memory\-intensive jobs\.
  + For the `G.2X` worker type, each worker maps to 2 DPU \(8 vCPU, 32 GB of memory, 128 GB disk\), and provides 1 executor per worker\. We recommend this worker type for memory\-intensive jobs\.
+ `NumberOfWorkers` – Number \(integer\)\.

  The number of workers of a defined `workerType` that are allocated when a job runs\.

  The maximum number of workers you can define are 299 for `G.1X`, and 149 for `G.2X`\. 
+ `SecurityConfiguration` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the `SecurityConfiguration` structure to be used with this job\.
+ `NotificationProperty` – A [NotificationProperty](#aws-glue-api-jobs-job-NotificationProperty) object\.

  Specifies configuration properties of a job notification\.
+ `GlueVersion` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #13](aws-glue-api-common.md#regex_13)\.

  Glue version determines the versions of Apache Spark and Python that AWS Glue supports\. The Python version indicates the version supported for jobs of type Spark\. 

  For more information about the available AWS Glue versions and corresponding Spark and Python versions, see [Glue version](https://docs.aws.amazon.com/glue/latest/dg/add-job.html) in the developer guide\.

  Jobs that are created without specifying a Glue version default to Glue 0\.9\.

## ExecutionProperty Structure<a name="aws-glue-api-jobs-job-ExecutionProperty"></a>

An execution property of a job\.

**Fields**
+ `MaxConcurrentRuns` – Number \(integer\)\.

  The maximum number of concurrent runs allowed for the job\. The default is 1\. An error is returned when this threshold is reached\. The maximum value you can specify is controlled by a service limit\.

## NotificationProperty Structure<a name="aws-glue-api-jobs-job-NotificationProperty"></a>

Specifies configuration properties of a notification\.

**Fields**
+ `NotifyDelayAfter` – Number \(integer\), at least 1\.

  After a job run starts, the number of minutes to wait before sending a job run delay notification\.

## JobCommand Structure<a name="aws-glue-api-jobs-job-JobCommand"></a>

Specifies code executed when a job is run\.

**Fields**
+ `Name` – UTF\-8 string\.

  The name of the job command\. For an Apache Spark ETL job, this must be `glueetl`\. For a Python shell job, it must be `pythonshell`\.
+ `ScriptLocation` – UTF\-8 string\.

  Specifies the Amazon Simple Storage Service \(Amazon S3\) path to a script that executes a job\.
+ `PythonVersion` – UTF\-8 string, matching the [Custom string pattern #11](aws-glue-api-common.md#regex_11)\.

  The Python version being used to execute a Python shell job\. Allowed values are 2 or 3\.

## ConnectionsList Structure<a name="aws-glue-api-jobs-job-ConnectionsList"></a>

Specifies the connections used by a job\.

**Fields**
+ `Connections` – An array of UTF\-8 strings\.

  A list of connections used by the job\.

## JobUpdate Structure<a name="aws-glue-api-jobs-job-JobUpdate"></a>

Specifies information used to update an existing job definition\. The previous job definition is completely overwritten by this information\.

**Fields**
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Description of the job being defined\.
+ `LogUri` – UTF\-8 string\.

  This field is reserved for future use\.
+ `Role` – UTF\-8 string\.

  The name or Amazon Resource Name \(ARN\) of the IAM role associated with this job \(required\)\.
+ `ExecutionProperty` – An [ExecutionProperty](#aws-glue-api-jobs-job-ExecutionProperty) object\.

  An `ExecutionProperty` specifying the maximum number of concurrent runs allowed for this job\.
+ `Command` – A [JobCommand](#aws-glue-api-jobs-job-JobCommand) object\.

  The `JobCommand` that executes this job \(required\)\.
+ `DefaultArguments` – A map array of key\-value pairs\.

  Each key is a UTF\-8 string\.

  Each value is a UTF\-8 string\.

  The default arguments for this job\.

  You can specify arguments here that your own job\-execution script consumes, as well as arguments that AWS Glue itself consumes\.

  For information about how to specify and consume your own Job arguments, see the [Calling AWS Glue APIs in Python](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-calling.html) topic in the developer guide\.

  For information about the key\-value pairs that AWS Glue consumes to set up your job, see the [Special Parameters Used by AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-glue-arguments.html) topic in the developer guide\.
+ `Connections` – A [ConnectionsList](#aws-glue-api-jobs-job-ConnectionsList) object\.

  The connections used for this job\.
+ `MaxRetries` – Number \(integer\)\.

  The maximum number of times to retry this job if it fails\.
+ `AllocatedCapacity` – Number \(integer\)\.

  This field is deprecated\. Use `MaxCapacity` instead\.

  The number of AWS Glue data processing units \(DPUs\) to allocate to this job\. You can allocate from 2 to 100 DPUs; the default is 10\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://aws.amazon.com/glue/pricing/)\.
+ `Timeout` – Number \(integer\), at least 1\.

  The job timeout in minutes\. This is the maximum time that a job run can consume resources before it is terminated and enters `TIMEOUT` status\. The default is 2,880 minutes \(48 hours\)\.
+ `MaxCapacity` – Number \(double\)\.

  The number of AWS Glue data processing units \(DPUs\) that can be allocated when this job runs\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://aws.amazon.com/glue/pricing/)\.

  Do not set `Max Capacity` if using `WorkerType` and `NumberOfWorkers`\.

  The value that can be allocated for `MaxCapacity` depends on whether you are running a Python shell job or an Apache Spark ETL job:
  + When you specify a Python shell job \(`JobCommand.Name`="pythonshell"\), you can allocate either 0\.0625 or 1 DPU\. The default is 0\.0625 DPU\.
  + When you specify an Apache Spark ETL job \(`JobCommand.Name`="glueetl"\), you can allocate from 2 to 100 DPUs\. The default is 10 DPUs\. This job type cannot have a fractional DPU allocation\.
+ `WorkerType` – UTF\-8 string \(valid values: `Standard=""` \| `G.1X=""` \| `G.2X=""`\)\.

  The type of predefined worker that is allocated when a job runs\. Accepts a value of Standard, G\.1X, or G\.2X\.
  + For the `Standard` worker type, each worker provides 4 vCPU, 16 GB of memory and a 50GB disk, and 2 executors per worker\.
  + For the `G.1X` worker type, each worker maps to 1 DPU \(4 vCPU, 16 GB of memory, 64 GB disk\), and provides 1 executor per worker\. We recommend this worker type for memory\-intensive jobs\.
  + For the `G.2X` worker type, each worker maps to 2 DPU \(8 vCPU, 32 GB of memory, 128 GB disk\), and provides 1 executor per worker\. We recommend this worker type for memory\-intensive jobs\.
+ `NumberOfWorkers` – Number \(integer\)\.

  The number of workers of a defined `workerType` that are allocated when a job runs\.

  The maximum number of workers you can define are 299 for `G.1X`, and 149 for `G.2X`\. 
+ `SecurityConfiguration` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the `SecurityConfiguration` structure to be used with this job\.
+ `NotificationProperty` – A [NotificationProperty](#aws-glue-api-jobs-job-NotificationProperty) object\.

  Specifies the configuration properties of a job notification\.
+ `GlueVersion` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #13](aws-glue-api-common.md#regex_13)\.

  Glue version determines the versions of Apache Spark and Python that AWS Glue supports\. The Python version indicates the version supported for jobs of type Spark\. 

  For more information about the available AWS Glue versions and corresponding Spark and Python versions, see [Glue version](https://docs.aws.amazon.com/glue/latest/dg/add-job.html) in the developer guide\.

## Operations<a name="aws-glue-api-jobs-job-actions"></a>
+ [CreateJob Action \(Python: create\_job\)](#aws-glue-api-jobs-job-CreateJob)
+ [UpdateJob Action \(Python: update\_job\)](#aws-glue-api-jobs-job-UpdateJob)
+ [GetJob Action \(Python: get\_job\)](#aws-glue-api-jobs-job-GetJob)
+ [GetJobs Action \(Python: get\_jobs\)](#aws-glue-api-jobs-job-GetJobs)
+ [DeleteJob Action \(Python: delete\_job\)](#aws-glue-api-jobs-job-DeleteJob)
+ [ListJobs Action \(Python: list\_jobs\)](#aws-glue-api-jobs-job-ListJobs)
+ [BatchGetJobs Action \(Python: batch\_get\_jobs\)](#aws-glue-api-jobs-job-BatchGetJobs)

## CreateJob Action \(Python: create\_job\)<a name="aws-glue-api-jobs-job-CreateJob"></a>

Creates a new job definition\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name you assign to this job definition\. It must be unique in your account\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Description of the job being defined\.
+ `LogUri` – UTF\-8 string\.

  This field is reserved for future use\.
+ `Role` – *Required:* UTF\-8 string\.

  The name or Amazon Resource Name \(ARN\) of the IAM role associated with this job\.
+ `ExecutionProperty` – An [ExecutionProperty](#aws-glue-api-jobs-job-ExecutionProperty) object\.

  An `ExecutionProperty` specifying the maximum number of concurrent runs allowed for this job\.
+ `Command` – *Required:* A [JobCommand](#aws-glue-api-jobs-job-JobCommand) object\.

  The `JobCommand` that executes this job\.
+ `DefaultArguments` – A map array of key\-value pairs\.

  Each key is a UTF\-8 string\.

  Each value is a UTF\-8 string\.

  The default arguments for this job\.

  You can specify arguments here that your own job\-execution script consumes, as well as arguments that AWS Glue itself consumes\.

  For information about how to specify and consume your own Job arguments, see the [Calling AWS Glue APIs in Python](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-calling.html) topic in the developer guide\.

  For information about the key\-value pairs that AWS Glue consumes to set up your job, see the [Special Parameters Used by AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-glue-arguments.html) topic in the developer guide\.
+ `Connections` – A [ConnectionsList](#aws-glue-api-jobs-job-ConnectionsList) object\.

  The connections used for this job\.
+ `MaxRetries` – Number \(integer\)\.

  The maximum number of times to retry this job if it fails\.
+ `AllocatedCapacity` – Number \(integer\)\.

  This parameter is deprecated\. Use `MaxCapacity` instead\.

  The number of AWS Glue data processing units \(DPUs\) to allocate to this Job\. You can allocate from 2 to 100 DPUs; the default is 10\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://aws.amazon.com/glue/pricing/)\.
+ `Timeout` – Number \(integer\), at least 1\.

  The job timeout in minutes\. This is the maximum time that a job run can consume resources before it is terminated and enters `TIMEOUT` status\. The default is 2,880 minutes \(48 hours\)\.
+ `MaxCapacity` – Number \(double\)\.

  The number of AWS Glue data processing units \(DPUs\) that can be allocated when this job runs\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://aws.amazon.com/glue/pricing/)\.

  Do not set `Max Capacity` if using `WorkerType` and `NumberOfWorkers`\.

  The value that can be allocated for `MaxCapacity` depends on whether you are running a Python shell job or an Apache Spark ETL job:
  + When you specify a Python shell job \(`JobCommand.Name`="pythonshell"\), you can allocate either 0\.0625 or 1 DPU\. The default is 0\.0625 DPU\.
  + When you specify an Apache Spark ETL job \(`JobCommand.Name`="glueetl"\), you can allocate from 2 to 100 DPUs\. The default is 10 DPUs\. This job type cannot have a fractional DPU allocation\.
+ `SecurityConfiguration` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the `SecurityConfiguration` structure to be used with this job\.
+ `Tags` – A map array of key\-value pairs, not more than 50 pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 128 bytes long\.

  Each value is a UTF\-8 string, not more than 256 bytes long\.

  The tags to use with this job\. You may use tags to limit access to the job\. For more information about tags in AWS Glue, see [AWS Tags in AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/monitor-tags.html) in the developer guide\.
+ `NotificationProperty` – A [NotificationProperty](#aws-glue-api-jobs-job-NotificationProperty) object\.

  Specifies configuration properties of a job notification\.
+ `GlueVersion` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #13](aws-glue-api-common.md#regex_13)\.

  Glue version determines the versions of Apache Spark and Python that AWS Glue supports\. The Python version indicates the version supported for jobs of type Spark\. 

  For more information about the available AWS Glue versions and corresponding Spark and Python versions, see [Glue version](https://docs.aws.amazon.com/glue/latest/dg/add-job.html) in the developer guide\.

  Jobs that are created without specifying a Glue version default to Glue 0\.9\.
+ `NumberOfWorkers` – Number \(integer\)\.

  The number of workers of a defined `workerType` that are allocated when a job runs\.

  The maximum number of workers you can define are 299 for `G.1X`, and 149 for `G.2X`\. 
+ `WorkerType` – UTF\-8 string \(valid values: `Standard=""` \| `G.1X=""` \| `G.2X=""`\)\.

  The type of predefined worker that is allocated when a job runs\. Accepts a value of Standard, G\.1X, or G\.2X\.
  + For the `Standard` worker type, each worker provides 4 vCPU, 16 GB of memory and a 50GB disk, and 2 executors per worker\.
  + For the `G.1X` worker type, each worker maps to 1 DPU \(4 vCPU, 16 GB of memory, 64 GB disk\), and provides 1 executor per worker\. We recommend this worker type for memory\-intensive jobs\.
  + For the `G.2X` worker type, each worker maps to 2 DPU \(8 vCPU, 32 GB of memory, 128 GB disk\), and provides 1 executor per worker\. We recommend this worker type for memory\-intensive jobs\.

**Response**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The unique name that was provided for this job definition\.

**Errors**
+ `InvalidInputException`
+ `IdempotentParameterMismatchException`
+ `AlreadyExistsException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `ResourceNumberLimitExceededException`
+ `ConcurrentModificationException`

## UpdateJob Action \(Python: update\_job\)<a name="aws-glue-api-jobs-job-UpdateJob"></a>

Updates an existing job definition\.

**Request**
+ `JobName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job definition to update\.
+ `JobUpdate` – *Required:* A [JobUpdate](#aws-glue-api-jobs-job-JobUpdate) object\.

  Specifies the values with which to update the job definition\.

**Response**
+ `JobName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Returns the name of the updated job definition\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `ConcurrentModificationException`

## GetJob Action \(Python: get\_job\)<a name="aws-glue-api-jobs-job-GetJob"></a>

Retrieves an existing job definition\.

**Request**
+ `JobName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job definition to retrieve\.

**Response**
+ `Job` – A [Job](#aws-glue-api-jobs-job-Job) object\.

  The requested job definition\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## GetJobs Action \(Python: get\_jobs\)<a name="aws-glue-api-jobs-job-GetJobs"></a>

Retrieves all current job definitions\.

**Request**
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation call\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum size of the response\.

**Response**
+ `Jobs` – An array of [Job](#aws-glue-api-jobs-job-Job) objects\.

  A list of job definitions\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if not all job definitions have yet been returned\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## DeleteJob Action \(Python: delete\_job\)<a name="aws-glue-api-jobs-job-DeleteJob"></a>

Deletes a specified job definition\. If the job definition is not found, no exception is thrown\.

**Request**
+ `JobName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job definition to delete\.

**Response**
+ `JobName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job definition that was deleted\.

**Errors**
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## ListJobs Action \(Python: list\_jobs\)<a name="aws-glue-api-jobs-job-ListJobs"></a>

Retrieves the names of all job resources in this AWS account, or the resources with the specified tag\. This operation allows you to see which resources are available in your account, and their names\.

This operation takes the optional `Tags` field, which you can use as a filter on the response so that tagged resources can be retrieved as a group\. If you choose to use tags filtering, only resources with the tag are retrieved\.

**Request**
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation request\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum size of a list to return\.
+ `Tags` – A map array of key\-value pairs, not more than 50 pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 128 bytes long\.

  Each value is a UTF\-8 string, not more than 256 bytes long\.

  Specifies to return only these tagged resources\.

**Response**
+ `JobNames` – An array of UTF\-8 strings\.

  The names of all jobs in the account, or the jobs with the specified tags\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if the returned list does not contain the last metric available\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## BatchGetJobs Action \(Python: batch\_get\_jobs\)<a name="aws-glue-api-jobs-job-BatchGetJobs"></a>

Returns a list of resource metadata for a given list of job names\. After calling the `ListJobs` operation, you can call this operation to access the data to which you have been granted permissions\. This operation supports all IAM permissions, including permission conditions that uses tags\. 

**Request**
+ `JobNames` – *Required:* An array of UTF\-8 strings\.

  A list of job names, which might be the names returned from the `ListJobs` operation\.

**Response**
+ `Jobs` – An array of [Job](#aws-glue-api-jobs-job-Job) objects\.

  A list of job definitions\.
+ `JobsNotFound` – An array of UTF\-8 strings\.

  A list of names of jobs not found\.

**Errors**
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`