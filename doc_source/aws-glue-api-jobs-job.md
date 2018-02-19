# Jobs<a name="aws-glue-api-jobs-job"></a>

## Data Types<a name="aws-glue-api-jobs-job-objects"></a>

+ [Job Structure](#aws-glue-api-jobs-job-Job)

+ [ExecutionProperty Structure](#aws-glue-api-jobs-job-ExecutionProperty)

+ [JobCommand Structure](#aws-glue-api-jobs-job-JobCommand)

+ [ConnectionsList Structure](#aws-glue-api-jobs-job-ConnectionsList)

+ [JobUpdate Structure](#aws-glue-api-jobs-job-JobUpdate)

## Job Structure<a name="aws-glue-api-jobs-job-Job"></a>

Specifies a job\.

**Fields**

+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name you assign to this job\.

+ `Description` – Description string, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Description of this job\.

+ `LogUri` – String\.

  This field is reserved for future use\.

+ `Role` – String\.

  The name or ARN of the IAM role associated with this job\.

+ `CreatedOn` – Timestamp\.

  The time and date that this job specification was created\.

+ `LastModifiedOn` – Timestamp\.

  The last point in time when this job specification was modified\.

+ `ExecutionProperty` – An ExecutionProperty object\.

  An ExecutionProperty specifying the maximum number of concurrent runs allowed for this job\.

+ `Command` – A JobCommand object\.

  The JobCommand that executes this job\.

+ `DefaultArguments` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\.

  The default arguments for this job, specified as name\-value pairs\.

  You can specify arguments here that your own job\-execution script consumes, as well as arguments that AWS Glue itself consumes\.

  For information about how to specify and consume your own Job arguments, see the [Calling AWS Glue APIs in Python](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-calling.html) topic in the developer guide\.

  For information about the key\-value pairs that AWS Glue consumes to set up your job, see the [Special Parameters Used by AWS Glue](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-glue-arguments.html) topic in the developer guide\.

+ `Connections` – A ConnectionsList object\.

  The connections used for this job\.

+ `MaxRetries` – Number \(integer\)\.

  The maximum number of times to retry this job if it fails\.

+ `AllocatedCapacity` – Number \(integer\)\.

  The number of AWS Glue data processing units \(DPUs\) allocated to this Job\. From 2 to 100 DPUs can be allocated; the default is 10\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://aws.amazon.com/glue/pricing/)\.

## ExecutionProperty Structure<a name="aws-glue-api-jobs-job-ExecutionProperty"></a>

An execution property of a job\.

**Fields**

+ `MaxConcurrentRuns` – Number \(integer\)\.

  The maximum number of concurrent runs allowed for a job\. The default is 1\. An error is returned when this threshold is reached\. The maximum value you can specify is controlled by a service limit\.

## JobCommand Structure<a name="aws-glue-api-jobs-job-JobCommand"></a>

Specifies code that executes a job\.

**Fields**

+ `Name` – String\.

  The name of the job command: this must be `glueetl`\.

+ `ScriptLocation` – String\.

  Specifies the S3 path to a script that executes a job \(required\)\.

## ConnectionsList Structure<a name="aws-glue-api-jobs-job-ConnectionsList"></a>

Specifies the connections used by a job\.

**Fields**

+ `Connections` – An array of UTF\-8 strings\.

  A list of connections used by the job\.

## JobUpdate Structure<a name="aws-glue-api-jobs-job-JobUpdate"></a>

Specifies information used to update an existing job\. Note that the previous job definition will be completely overwritten by this information\.

**Fields**

+ `Description` – Description string, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Description of the job\.

+ `LogUri` – String\.

  This field is reserved for future use\.

+ `Role` – String\.

  The name or ARN of the IAM role associated with this job \(required\)\.

+ `ExecutionProperty` – An ExecutionProperty object\.

  An ExecutionProperty specifying the maximum number of concurrent runs allowed for this job\.

+ `Command` – A JobCommand object\.

  The JobCommand that executes this job \(required\)\.

+ `DefaultArguments` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\.

  The default arguments for this job\.

  You can specify arguments here that your own job\-execution script consumes, as well as arguments that AWS Glue itself consumes\.

  For information about how to specify and consume your own Job arguments, see the [Calling AWS Glue APIs in Python](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-calling.html) topic in the developer guide\.

  For information about the key\-value pairs that AWS Glue consumes to set up your job, see the [Special Parameters Used by AWS Glue](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-glue-arguments.html) topic in the developer guide\.

+ `Connections` – A ConnectionsList object\.

  The connections used for this job\.

+ `MaxRetries` – Number \(integer\)\.

  The maximum number of times to retry this job if it fails\.

+ `AllocatedCapacity` – Number \(integer\)\.

  The number of AWS Glue data processing units \(DPUs\) to allocate to this Job\. From 2 to 100 DPUs can be allocated; the default is 10\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://aws.amazon.com/glue/pricing/)\.

## Operations<a name="aws-glue-api-jobs-job-actions"></a>

+ [CreateJob Action \(Python: create\_job\)](#aws-glue-api-jobs-job-CreateJob)

+ [UpdateJob Action \(Python: update\_job\)](#aws-glue-api-jobs-job-UpdateJob)

+ [GetJob Action \(Python: get\_job\)](#aws-glue-api-jobs-job-GetJob)

+ [GetJobs Action \(Python: get\_jobs\)](#aws-glue-api-jobs-job-GetJobs)

+ [DeleteJob Action \(Python: delete\_job\)](#aws-glue-api-jobs-job-DeleteJob)

## CreateJob Action \(Python: create\_job\)<a name="aws-glue-api-jobs-job-CreateJob"></a>

Creates a new job\.

**Request**

+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name you assign to this job\. It must be unique in your account\.

+ `Description` – Description string, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Description of the job\.

+ `LogUri` – String\.

  This field is reserved for future use\.

+ `Role` – String\. Required\.

  The name or ARN of the IAM role associated with this job\.

+ `ExecutionProperty` – An ExecutionProperty object\.

  An ExecutionProperty specifying the maximum number of concurrent runs allowed for this job\.

+ `Command` – A JobCommand object\. Required\.

  The JobCommand that executes this job\.

+ `DefaultArguments` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\.

  The default arguments for this job\.

  You can specify arguments here that your own job\-execution script consumes, as well as arguments that AWS Glue itself consumes\.

  For information about how to specify and consume your own Job arguments, see the [Calling AWS Glue APIs in Python](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-calling.html) topic in the developer guide\.

  For information about the key\-value pairs that AWS Glue consumes to set up your job, see the [Special Parameters Used by AWS Glue](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-glue-arguments.html) topic in the developer guide\.

+ `Connections` – A ConnectionsList object\.

  The connections used for this job\.

+ `MaxRetries` – Number \(integer\)\.

  The maximum number of times to retry this job if it fails\.

+ `AllocatedCapacity` – Number \(integer\)\.

  The number of AWS Glue data processing units \(DPUs\) to allocate to this Job\. From 2 to 100 DPUs can be allocated; the default is 10\. A DPU is a relative measure of processing power that consists of 4 vCPUs of compute capacity and 16 GB of memory\. For more information, see the [AWS Glue pricing page](https://aws.amazon.com/glue/pricing/)\.

+ `Tags` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\.

**Response**

+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The unique name that was provided\.

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

+ `JobName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  Name of the job definition to update\.

+ `JobUpdate` – A JobUpdate object\. Required\.

  Specifies the values with which to update the job\.

**Response**

+ `JobName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Returns the name of the updated job\.

**Errors**

+ `InvalidInputException`

+ `EntityNotFoundException`

+ `InternalServiceException`

+ `OperationTimeoutException`

+ `ConcurrentModificationException`

## GetJob Action \(Python: get\_job\)<a name="aws-glue-api-jobs-job-GetJob"></a>

Retrieves an existing job definition\.

**Request**

+ `JobName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the job to retrieve\.

**Response**

+ `Job` – A Job object\.

  The requested job definition\.

**Errors**

+ `InvalidInputException`

+ `EntityNotFoundException`

+ `InternalServiceException`

+ `OperationTimeoutException`

## GetJobs Action \(Python: get\_jobs\)<a name="aws-glue-api-jobs-job-GetJobs"></a>

Retrieves all current jobs\.

**Request**

+ `NextToken` – String\.

  A continuation token, if this is a continuation call\.

+ `MaxResults` – Number \(integer\)\.

  The maximum size of the response\.

**Response**

+ `Jobs` – An array of [Job](#aws-glue-api-jobs-job-Job)s\.

  A list of jobs\.

+ `NextToken` – String\.

  A continuation token, if not all jobs have yet been returned\.

**Errors**

+ `InvalidInputException`

+ `EntityNotFoundException`

+ `InternalServiceException`

+ `OperationTimeoutException`

## DeleteJob Action \(Python: delete\_job\)<a name="aws-glue-api-jobs-job-DeleteJob"></a>

Deletes a specified job\. If the job is not found, no exception is thrown\.

**Request**

+ `JobName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the job to delete\.

**Response**

+ `JobName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job that was deleted\.

**Errors**

+ `InvalidInputException`

+ `InternalServiceException`

+ `OperationTimeoutException`