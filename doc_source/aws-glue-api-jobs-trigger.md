# Triggers<a name="aws-glue-api-jobs-trigger"></a>

## Data Types<a name="aws-glue-api-jobs-trigger-objects"></a>
+ [Trigger Structure](#aws-glue-api-jobs-trigger-Trigger)
+ [TriggerUpdate Structure](#aws-glue-api-jobs-trigger-TriggerUpdate)
+ [Predicate Structure](#aws-glue-api-jobs-trigger-Predicate)
+ [Condition Structure](#aws-glue-api-jobs-trigger-Condition)
+ [Action Structure](#aws-glue-api-jobs-trigger-Action)

## Trigger Structure<a name="aws-glue-api-jobs-trigger-Trigger"></a>

Information about a specific trigger\.

**Fields**
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the trigger\.
+ `Id` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Reserved for future use\.
+ `Type` – String \(valid values: `SCHEDULED` \| `CONDITIONAL` \| `ON_DEMAND`\)\.

  The type of trigger that this is\.
+ `State` – String \(valid values: `CREATING` \| `CREATED` \| `ACTIVATING` \| `ACTIVATED` \| `DEACTIVATING` \| `DEACTIVATED` \| `DELETING` \| `UPDATING`\)\.

  The current state of the trigger\.
+ `Description` – Description string, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of this trigger\.
+ `Schedule` – String\.

  A `cron` expression used to specify the schedule \(see [Time\-Based Schedules for Jobs and Crawlers](http://docs.aws.amazon.com/glue/latest/dg/monitor-data-warehouse-schedule.html)\. For example, to run something every day at 12:15 UTC, you would specify: `cron(15 12 * * ? *)`\.
+ `Actions` – An array of [Action](#aws-glue-api-jobs-trigger-Action)s\.

  The actions initiated by this trigger\.
+ `Predicate` – A Predicate object\.

  The predicate of this trigger, which defines when it will fire\.

## TriggerUpdate Structure<a name="aws-glue-api-jobs-trigger-TriggerUpdate"></a>

A structure used to provide information used to update a trigger\. This object will update the the previous trigger definition by overwriting it completely\.

**Fields**
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Reserved for future use\.
+ `Description` – Description string, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of this trigger\.
+ `Schedule` – String\.

  A `cron` expression used to specify the schedule \(see [Time\-Based Schedules for Jobs and Crawlers](http://docs.aws.amazon.com/glue/latest/dg/monitor-data-warehouse-schedule.html)\. For example, to run something every day at 12:15 UTC, you would specify: `cron(15 12 * * ? *)`\.
+ `Actions` – An array of [Action](#aws-glue-api-jobs-trigger-Action)s\.

  The actions initiated by this trigger\.
+ `Predicate` – A Predicate object\.

  The predicate of this trigger, which defines when it will fire\.

## Predicate Structure<a name="aws-glue-api-jobs-trigger-Predicate"></a>

Defines the predicate of the trigger, which determines when it fires\.

**Fields**
+ `Logical` – String \(valid values: `AND` \| `ANY`\)\.

  Optional field if only one condition is listed\. If multiple conditions are listed, then this field is required\.
+ `Conditions` – An array of [Condition](#aws-glue-api-jobs-trigger-Condition)s\.

  A list of the conditions that determine when the trigger will fire\.

## Condition Structure<a name="aws-glue-api-jobs-trigger-Condition"></a>

Defines a condition under which a trigger fires\.

**Fields**
+ `LogicalOperator` – String \(valid values: `EQUALS`\)\.

  A logical operator\.
+ `JobName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the Job to whose JobRuns this condition applies and on which this trigger waits\.
+ `State` – String \(valid values: `STARTING` \| `RUNNING` \| `STOPPING` \| `STOPPED` \| `SUCCEEDED` \| `FAILED` \| `TIMEOUT`\)\.

  The condition state\. Currently, the values supported are SUCCEEDED, STOPPED, TIMEOUT and FAILED\.

## Action Structure<a name="aws-glue-api-jobs-trigger-Action"></a>

Defines an action to be initiated by a trigger\.

**Fields**
+ `JobName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of a job to be executed\.
+ `Arguments` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\.

  Arguments to be passed to the job\.

  You can specify arguments here that your own job\-execution script consumes, as well as arguments that AWS Glue itself consumes\.

  For information about how to specify and consume your own Job arguments, see the [Calling AWS Glue APIs in Python](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-calling.html) topic in the developer guide\.

  For information about the key\-value pairs that AWS Glue consumes to set up your job, see the [Special Parameters Used by AWS Glue](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-glue-arguments.html) topic in the developer guide\.
+ `Timeout` – Number \(integer\)\.

  The JobRun timeout in minutes\. This is the maximum time that a job run can consume resources before it is terminated and enters `TIMEOUT` status\. The default is 2,880 minutes \(48 hours\)\. This overrides the timeout value set in the parent job\.

## Operations<a name="aws-glue-api-jobs-trigger-actions"></a>
+ [CreateTrigger Action \(Python: create\_trigger\)](#aws-glue-api-jobs-trigger-CreateTrigger)
+ [StartTrigger Action \(Python: start\_trigger\)](#aws-glue-api-jobs-trigger-StartTrigger)
+ [GetTrigger Action \(Python: get\_trigger\)](#aws-glue-api-jobs-trigger-GetTrigger)
+ [GetTriggers Action \(Python: get\_triggers\)](#aws-glue-api-jobs-trigger-GetTriggers)
+ [UpdateTrigger Action \(Python: update\_trigger\)](#aws-glue-api-jobs-trigger-UpdateTrigger)
+ [StopTrigger Action \(Python: stop\_trigger\)](#aws-glue-api-jobs-trigger-StopTrigger)
+ [DeleteTrigger Action \(Python: delete\_trigger\)](#aws-glue-api-jobs-trigger-DeleteTrigger)

## CreateTrigger Action \(Python: create\_trigger\)<a name="aws-glue-api-jobs-trigger-CreateTrigger"></a>

Creates a new trigger\.

**Request**
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the trigger\.
+ `Type` – String \(valid values: `SCHEDULED` \| `CONDITIONAL` \| `ON_DEMAND`\)\. Required\.

  The type of the new trigger\.
+ `Schedule` – String\.

  A `cron` expression used to specify the schedule \(see [Time\-Based Schedules for Jobs and Crawlers](http://docs.aws.amazon.com/glue/latest/dg/monitor-data-warehouse-schedule.html)\. For example, to run something every day at 12:15 UTC, you would specify: `cron(15 12 * * ? *)`\.

  This field is required when the trigger type is SCHEDULED\.
+ `Predicate` – A Predicate object\.

  A predicate to specify when the new trigger should fire\.

  This field is required when the trigger type is CONDITIONAL\.
+ `Actions` – An array of [Action](#aws-glue-api-jobs-trigger-Action)s\. Required\.

  The actions initiated by this trigger when it fires\.
+ `Description` – Description string, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of the new trigger\.
+ `StartOnCreation` – Boolean\.

  Set to true to start SCHEDULED and CONDITIONAL triggers when created\. True not supported for ON\_DEMAND triggers\.

**Response**
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the trigger\.

**Errors**
+ `AlreadyExistsException`
+ `InvalidInputException`
+ `IdempotentParameterMismatchException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `ResourceNumberLimitExceededException`
+ `ConcurrentModificationException`

## StartTrigger Action \(Python: start\_trigger\)<a name="aws-glue-api-jobs-trigger-StartTrigger"></a>

Starts an existing trigger\. See [Triggering Jobs](http://docs.aws.amazon.com/glue/latest/dg/trigger-job.html) for information about how different types of trigger are started\.

**Request**
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the trigger to start\.

**Response**
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the trigger that was started\.

**Errors**
+ `InvalidInputException`
+ `InternalServiceException`
+ `EntityNotFoundException`
+ `OperationTimeoutException`
+ `ResourceNumberLimitExceededException`
+ `ConcurrentRunsExceededException`

## GetTrigger Action \(Python: get\_trigger\)<a name="aws-glue-api-jobs-trigger-GetTrigger"></a>

Retrieves the definition of a trigger\.

**Request**
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the trigger to retrieve\.

**Response**
+ `Trigger` – A Trigger object\.

  The requested trigger definition\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## GetTriggers Action \(Python: get\_triggers\)<a name="aws-glue-api-jobs-trigger-GetTriggers"></a>

Gets all the triggers associated with a job\.

**Request**
+ `NextToken` – String\.

  A continuation token, if this is a continuation call\.
+ `DependentJobName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job for which to retrieve triggers\. The trigger that can start this job will be returned, and if there is no such trigger, all triggers will be returned\.
+ `MaxResults` – Number \(integer\)\.

  The maximum size of the response\.

**Response**
+ `Triggers` – An array of [Trigger](#aws-glue-api-jobs-trigger-Trigger)s\.

  A list of triggers for the specified job\.
+ `NextToken` – String\.

  A continuation token, if not all the requested triggers have yet been returned\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## UpdateTrigger Action \(Python: update\_trigger\)<a name="aws-glue-api-jobs-trigger-UpdateTrigger"></a>

Updates a trigger definition\.

**Request**
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the trigger to update\.
+ `TriggerUpdate` – A TriggerUpdate object\. Required\.

  The new values with which to update the trigger\.

**Response**
+ `Trigger` – A Trigger object\.

  The resulting trigger definition\.

**Errors**
+ `InvalidInputException`
+ `InternalServiceException`
+ `EntityNotFoundException`
+ `OperationTimeoutException`
+ `ConcurrentModificationException`

## StopTrigger Action \(Python: stop\_trigger\)<a name="aws-glue-api-jobs-trigger-StopTrigger"></a>

Stops a specified trigger\.

**Request**
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the trigger to stop\.

**Response**
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the trigger that was stopped\.

**Errors**
+ `InvalidInputException`
+ `InternalServiceException`
+ `EntityNotFoundException`
+ `OperationTimeoutException`
+ `ConcurrentModificationException`

## DeleteTrigger Action \(Python: delete\_trigger\)<a name="aws-glue-api-jobs-trigger-DeleteTrigger"></a>

Deletes a specified trigger\. If the trigger is not found, no exception is thrown\.

**Request**
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the trigger to delete\.

**Response**
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the trigger that was deleted\.

**Errors**
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `ConcurrentModificationException`