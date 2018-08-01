# Jobs<a name="aws-glue-api-jobs-job"></a>

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

  Description of the job being defined\.
+ `LogUri` – UTF\-8 string\.

  This field is reserved for future use\.
+ `Role` – UTF\-8 string\.

  The name or ARN of the IAM role associated with this job\.
+ `CreatedOn` – Timestamp\.

  The time and date that this job definition was created\.
+ `LastModifiedOn` – Timestamp\.

  The last point in time when this job definition was modified\.
+ `ExecutionProperty` – An [ExecutionProperty](#aws-glue-api-jobs-job-ExecutionProperty) object\.

  An ExecutionProperty specifying the maximum number of concurrent runs allowed for this job\.
+ `Command` – A [JobCommand](#aws-glue-api-jobs-job-JobCommand) object\.

  The JobCommand that executes this job\.
+ `DefaultArguments` – A map array of key\-value pairs\.

  Each key is a UTF\-8 string\.

  Each value is a UTF\-8 string\.

  The default arguments for this job, specified as name\-value pairs\.

  You can specify arguments here that your own job\-execution script consumes, as well as arguments that AWS Glue itself consumes\.

  For information about how to specify and consume your own Job arguments, see the [Calling AWS Glue APIs in Python](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-calling.html) topic in the developer guide\.

  For information about the key\-value pairs that AWS Glue consumes to set up your job, see the [Special Parameters Used by AWS Glue](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-glue-arguments.html) topic in the developer guide\.
+ `Connections` – A [ConnectionsList](#aws-glue-api-jobs-job-ConnectionsList) object\.

  The connections used for this job\.
+ `MaxRetries` – Number \(integer\)\.

  The maximum number of times to retry this job after a JobRun fails\.
+ `AllocatedCapacity` – Number \(integer\)\.

  The number of AWS Glue data processing units \(DPUs\) allocated to runs of this job\. From 2 to 100 DPUs can be allocated; the default is 10\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://aws.amazon.com/glue/pricing/)\.
+ `Timeout` – Number \(integer\), at least 1\.

  The Job timeout in minutes\. This is the maximum time that a job run can consume resources before it is terminated and enters `TIMEOUT` status\. The default is 2,880 minutes \(48 hours\)\.
+ `NotificationProperty` – A [NotificationProperty](#aws-glue-api-jobs-job-NotificationProperty) object\.

  Specifies configuration properties of a job notification\.

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

  The name of the job command: this must be `glueetl`\.
+ `ScriptLocation` – UTF\-8 string\.

  Specifies the S3 path to a script that executes a job \(required\)\.

## ConnectionsList Structure<a name="aws-glue-api-jobs-job-ConnectionsList"></a>

Specifies the connections used by a job\.

**Fields**
+ `Connections` – An array of UTF\-8 strings\.

  A list of connections used by the job\.

## JobUpdate Structure<a name="aws-glue-api-jobs-job-JobUpdate"></a>

Specifies information used to update an existing job definition\. Note that the previous job definition will be completely overwritten by this information\.

**Fields**
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Description of the job being defined\.
+ `LogUri` – UTF\-8 string\.

  This field is reserved for future use\.
+ `Role` – UTF\-8 string\.

  The name or ARN of the IAM role associated with this job \(required\)\.
+ `ExecutionProperty` – An [ExecutionProperty](#aws-glue-api-jobs-job-ExecutionProperty) object\.

  An ExecutionProperty specifying the maximum number of concurrent runs allowed for this job\.
+ `Command` – A [JobCommand](#aws-glue-api-jobs-job-JobCommand) object\.

  The JobCommand that executes this job \(required\)\.
+ `DefaultArguments` – A map array of key\-value pairs\.

  Each key is a UTF\-8 string\.

  Each value is a UTF\-8 string\.

  The default arguments for this job\.

  You can specify arguments here that your own job\-execution script consumes, as well as arguments that AWS Glue itself consumes\.

  For information about how to specify and consume your own Job arguments, see the [Calling AWS Glue APIs in Python](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-calling.html) topic in the developer guide\.

  For information about the key\-value pairs that AWS Glue consumes to set up your job, see the [Special Parameters Used by AWS Glue](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-glue-arguments.html) topic in the developer guide\.
+ `Connections` – A [ConnectionsList](#aws-glue-api-jobs-job-ConnectionsList) object\.

  The connections used for this job\.
+ `MaxRetries` – Number \(integer\)\.

  The maximum number of times to retry this job if it fails\.
+ `AllocatedCapacity` – Number \(integer\)\.

  The number of AWS Glue data processing units \(DPUs\) to allocate to this Job\. From 2 to 100 DPUs can be allocated; the default is 10\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://aws.amazon.com/glue/pricing/)\.
+ `Timeout` – Number \(integer\), at least 1\.

  The Job timeout in minutes\. This is the maximum time that a job run can consume resources before it is terminated and enters `TIMEOUT` status\. The default is 2,880 minutes \(48 hours\)\.
+ `NotificationProperty` – A [NotificationProperty](#aws-glue-api-jobs-job-NotificationProperty) object\.

  Specifies configuration properties of a job notification\.

## Operations<a name="aws-glue-api-jobs-job-actions"></a>
+ [CreateJob Action \(Python: create\_job\)](#aws-glue-api-jobs-job-CreateJob)
+ [UpdateJob Action \(Python: update\_job\)](#aws-glue-api-jobs-job-UpdateJob)
+ [GetJob Action \(Python: get\_job\)](#aws-glue-api-jobs-job-GetJob)
+ [GetJobs Action \(Python: get\_jobs\)](#aws-glue-api-jobs-job-GetJobs)
+ [DeleteJob Action \(Python: delete\_job\)](#aws-glue-api-jobs-job-DeleteJob)

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

  The name or ARN of the IAM role associated with this job\.
+ `ExecutionProperty` – An [ExecutionProperty](#aws-glue-api-jobs-job-ExecutionProperty) object\.

  An ExecutionProperty specifying the maximum number of concurrent runs allowed for this job\.
+ `Command` – *Required:* A [JobCommand](#aws-glue-api-jobs-job-JobCommand) object\.

  The JobCommand that executes this job\.
+ `DefaultArguments` – A map array of key\-value pairs\.

  Each key is a UTF\-8 string\.

  Each value is a UTF\-8 string\.

  The default arguments for this job\.

  You can specify arguments here that your own job\-execution script consumes, as well as arguments that AWS Glue itself consumes\.

  For information about how to specify and consume your own Job arguments, see the [Calling AWS Glue APIs in Python](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-calling.html) topic in the developer guide\.

  For information about the key\-value pairs that AWS Glue consumes to set up your job, see the [Special Parameters Used by AWS Glue](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-glue-arguments.html) topic in the developer guide\.
+ `Connections` – A [ConnectionsList](#aws-glue-api-jobs-job-ConnectionsList) object\.

  The connections used for this job\.
+ `MaxRetries` – Number \(integer\)\.

  The maximum number of times to retry this job if it fails\.
+ `AllocatedCapacity` – Number \(integer\)\.

  The number of AWS Glue data processing units \(DPUs\) to allocate to this Job\. From 2 to 100 DPUs can be allocated; the default is 10\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://aws.amazon.com/glue/pricing/)\.
+ `Timeout` – Number \(integer\), at least 1\.

  The Job timeout in minutes\. This is the maximum time that a job run can consume resources before it is terminated and enters `TIMEOUT` status\. The default is 2,880 minutes \(48 hours\)\.
+ `NotificationProperty` – A [NotificationProperty](#aws-glue-api-jobs-job-NotificationProperty) object\.

  Specifies configuration properties of a job notification\.

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

  Name of the job definition to update\.
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