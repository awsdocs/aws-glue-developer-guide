# Triggers<a name="aws-glue-api-jobs-trigger"></a>

The Triggers API describes the data types and API related to creating, updating, or deleting, and starting and stopping job triggers in AWS Glue\.

## Data Types<a name="aws-glue-api-jobs-trigger-objects"></a>
+ [Trigger Structure](#aws-glue-api-jobs-trigger-Trigger)
+ [TriggerUpdate Structure](#aws-glue-api-jobs-trigger-TriggerUpdate)
+ [Predicate Structure](#aws-glue-api-jobs-trigger-Predicate)
+ [Condition Structure](#aws-glue-api-jobs-trigger-Condition)
+ [Action Structure](#aws-glue-api-jobs-trigger-Action)

## Trigger Structure<a name="aws-glue-api-jobs-trigger-Trigger"></a>

Information about a specific trigger\.

**Fields**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the trigger\.
+ `WorkflowName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the workflow associated with the trigger\.
+ `Id` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Reserved for future use\.
+ `Type` – UTF\-8 string \(valid values: `SCHEDULED` \| `CONDITIONAL` \| `ON_DEMAND`\)\.

  The type of trigger that this is\.
+ `State` – UTF\-8 string \(valid values: `CREATING` \| `CREATED` \| `ACTIVATING` \| `ACTIVATED` \| `DEACTIVATING` \| `DEACTIVATED` \| `DELETING` \| `UPDATING`\)\.

  The current state of the trigger\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of this trigger\.
+ `Schedule` – UTF\-8 string\.

  A `cron` expression used to specify the schedule \(see [Time\-Based Schedules for Jobs and Crawlers](https://docs.aws.amazon.com/glue/latest/dg/monitor-data-warehouse-schedule.html)\. For example, to run something every day at 12:15 UTC, you would specify: `cron(15 12 * * ? *)`\.
+ `Actions` – An array of [Action](#aws-glue-api-jobs-trigger-Action) objects\.

  The actions initiated by this trigger\.
+ `Predicate` – A [Predicate](#aws-glue-api-jobs-trigger-Predicate) object\.

  The predicate of this trigger, which defines when it will fire\.

## TriggerUpdate Structure<a name="aws-glue-api-jobs-trigger-TriggerUpdate"></a>

A structure used to provide information used to update a trigger\. This object updates the previous trigger definition by overwriting it completely\.

**Fields**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Reserved for future use\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of this trigger\.
+ `Schedule` – UTF\-8 string\.

  A `cron` expression used to specify the schedule \(see [Time\-Based Schedules for Jobs and Crawlers](https://docs.aws.amazon.com/glue/latest/dg/monitor-data-warehouse-schedule.html)\. For example, to run something every day at 12:15 UTC, you would specify: `cron(15 12 * * ? *)`\.
+ `Actions` – An array of [Action](#aws-glue-api-jobs-trigger-Action) objects\.

  The actions initiated by this trigger\.
+ `Predicate` – A [Predicate](#aws-glue-api-jobs-trigger-Predicate) object\.

  The predicate of this trigger, which defines when it will fire\.

## Predicate Structure<a name="aws-glue-api-jobs-trigger-Predicate"></a>

Defines the predicate of the trigger, which determines when it fires\.

**Fields**
+ `Logical` – UTF\-8 string \(valid values: `AND` \| `ANY`\)\.

  An optional field if only one condition is listed\. If multiple conditions are listed, then this field is required\.
+ `Conditions` – An array of [Condition](#aws-glue-api-jobs-trigger-Condition) objects\.

  A list of the conditions that determine when the trigger will fire\.

## Condition Structure<a name="aws-glue-api-jobs-trigger-Condition"></a>

Defines a condition under which a trigger fires\.

**Fields**
+ `LogicalOperator` – UTF\-8 string \(valid values: `EQUALS`\)\.

  A logical operator\.
+ `JobName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job whose `JobRuns` this condition applies to, and on which this trigger waits\.
+ `State` – UTF\-8 string \(valid values: `STARTING` \| `RUNNING` \| `STOPPING` \| `STOPPED` \| `SUCCEEDED` \| `FAILED` \| `TIMEOUT`\)\.

  The condition state\. Currently, the values supported are `SUCCEEDED`, `STOPPED`, `TIMEOUT`, and `FAILED`\.
+ `CrawlerName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the crawler to which this condition applies\.
+ `CrawlState` – UTF\-8 string \(valid values: `RUNNING` \| `SUCCEEDED` \| `CANCELLED` \| `FAILED`\)\.

  The state of the crawler to which this condition applies\.

## Action Structure<a name="aws-glue-api-jobs-trigger-Action"></a>

Defines an action to be initiated by a trigger\.

**Fields**
+ `JobName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of a job to be executed\.
+ `Arguments` – A map array of key\-value pairs\.

  Each key is a UTF\-8 string\.

  Each value is a UTF\-8 string\.

  The job arguments used when this trigger fires\. For this job run, they replace the default arguments set in the job definition itself\.

  You can specify arguments here that your own job\-execution script consumes, as well as arguments that AWS Glue itself consumes\.

  For information about how to specify and consume your own Job arguments, see the [Calling AWS Glue APIs in Python](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-calling.html) topic in the developer guide\.

  For information about the key\-value pairs that AWS Glue consumes to set up your job, see the [Special Parameters Used by AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-glue-arguments.html) topic in the developer guide\.
+ `Timeout` – Number \(integer\), at least 1\.

  The `JobRun` timeout in minutes\. This is the maximum time that a job run can consume resources before it is terminated and enters `TIMEOUT` status\. The default is 2,880 minutes \(48 hours\)\. This overrides the timeout value set in the parent job\.
+ `SecurityConfiguration` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the `SecurityConfiguration` structure to be used with this action\.
+ `NotificationProperty` – A [NotificationProperty](aws-glue-api-jobs-job.md#aws-glue-api-jobs-job-NotificationProperty) object\.

  Specifies configuration properties of a job run notification\.
+ `CrawlerName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the crawler to be used with this action\.

## Operations<a name="aws-glue-api-jobs-trigger-actions"></a>
+ [CreateTrigger Action \(Python: create\_trigger\)](#aws-glue-api-jobs-trigger-CreateTrigger)
+ [StartTrigger Action \(Python: start\_trigger\)](#aws-glue-api-jobs-trigger-StartTrigger)
+ [GetTrigger Action \(Python: get\_trigger\)](#aws-glue-api-jobs-trigger-GetTrigger)
+ [GetTriggers Action \(Python: get\_triggers\)](#aws-glue-api-jobs-trigger-GetTriggers)
+ [UpdateTrigger Action \(Python: update\_trigger\)](#aws-glue-api-jobs-trigger-UpdateTrigger)
+ [StopTrigger Action \(Python: stop\_trigger\)](#aws-glue-api-jobs-trigger-StopTrigger)
+ [DeleteTrigger Action \(Python: delete\_trigger\)](#aws-glue-api-jobs-trigger-DeleteTrigger)
+ [ListTriggers Action \(Python: list\_triggers\)](#aws-glue-api-jobs-trigger-ListTriggers)
+ [BatchGetTriggers Action \(Python: batch\_get\_triggers\)](#aws-glue-api-jobs-trigger-BatchGetTriggers)

## CreateTrigger Action \(Python: create\_trigger\)<a name="aws-glue-api-jobs-trigger-CreateTrigger"></a>

Creates a new trigger\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the trigger\.
+ `WorkflowName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the workflow associated with the trigger\.
+ `Type` – *Required:* UTF\-8 string \(valid values: `SCHEDULED` \| `CONDITIONAL` \| `ON_DEMAND`\)\.

  The type of the new trigger\.
+ `Schedule` – UTF\-8 string\.

  A `cron` expression used to specify the schedule \(see [Time\-Based Schedules for Jobs and Crawlers](https://docs.aws.amazon.com/glue/latest/dg/monitor-data-warehouse-schedule.html)\. For example, to run something every day at 12:15 UTC, you would specify: `cron(15 12 * * ? *)`\.

  This field is required when the trigger type is SCHEDULED\.
+ `Predicate` – A [Predicate](#aws-glue-api-jobs-trigger-Predicate) object\.

  A predicate to specify when the new trigger should fire\.

  This field is required when the trigger type is `CONDITIONAL`\.
+ `Actions` – *Required:* An array of [Action](#aws-glue-api-jobs-trigger-Action) objects\.

  The actions initiated by this trigger when it fires\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of the new trigger\.
+ `StartOnCreation` – Boolean\.

  Set to `true` to start `SCHEDULED` and `CONDITIONAL` triggers when created\. True is not supported for `ON_DEMAND` triggers\.
+ `Tags` – A map array of key\-value pairs, not more than 50 pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 128 bytes long\.

  Each value is a UTF\-8 string, not more than 256 bytes long\.

  The tags to use with this trigger\. You may use tags to limit access to the trigger\. For more information about tags in AWS Glue, see [AWS Tags in AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/monitor-tags.html) in the developer guide\. 

**Response**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the trigger\.

**Errors**
+ `AlreadyExistsException`
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `IdempotentParameterMismatchException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `ResourceNumberLimitExceededException`
+ `ConcurrentModificationException`

## StartTrigger Action \(Python: start\_trigger\)<a name="aws-glue-api-jobs-trigger-StartTrigger"></a>

Starts an existing trigger\. See [Triggering Jobs](https://docs.aws.amazon.com/glue/latest/dg/trigger-job.html) for information about how different types of trigger are started\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the trigger to start\.

**Response**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

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
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the trigger to retrieve\.

**Response**
+ `Trigger` – A [Trigger](#aws-glue-api-jobs-trigger-Trigger) object\.

  The requested trigger definition\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## GetTriggers Action \(Python: get\_triggers\)<a name="aws-glue-api-jobs-trigger-GetTriggers"></a>

Gets all the triggers associated with a job\.

**Request**
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation call\.
+ `DependentJobName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the job to retrieve triggers for\. The trigger that can start this job is returned, and if there is no such trigger, all triggers are returned\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum size of the response\.

**Response**
+ `Triggers` – An array of [Trigger](#aws-glue-api-jobs-trigger-Trigger) objects\.

  A list of triggers for the specified job\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if not all the requested triggers have yet been returned\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## UpdateTrigger Action \(Python: update\_trigger\)<a name="aws-glue-api-jobs-trigger-UpdateTrigger"></a>

Updates a trigger definition\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the trigger to update\.
+ `TriggerUpdate` – *Required:* A [TriggerUpdate](#aws-glue-api-jobs-trigger-TriggerUpdate) object\.

  The new values with which to update the trigger\.

**Response**
+ `Trigger` – A [Trigger](#aws-glue-api-jobs-trigger-Trigger) object\.

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
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the trigger to stop\.

**Response**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

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
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the trigger to delete\.

**Response**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the trigger that was deleted\.

**Errors**
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `ConcurrentModificationException`

## ListTriggers Action \(Python: list\_triggers\)<a name="aws-glue-api-jobs-trigger-ListTriggers"></a>

Retrieves the names of all trigger resources in this AWS account, or the resources with the specified tag\. This operation allows you to see which resources are available in your account, and their names\.

This operation takes the optional `Tags` field, which you can use as a filter on the response so that tagged resources can be retrieved as a group\. If you choose to use tags filtering, only resources with the tag are retrieved\.

**Request**
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation request\.
+ `DependentJobName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

   The name of the job for which to retrieve triggers\. The trigger that can start this job is returned\. If there is no such trigger, all triggers are returned\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum size of a list to return\.
+ `Tags` – A map array of key\-value pairs, not more than 50 pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 128 bytes long\.

  Each value is a UTF\-8 string, not more than 256 bytes long\.

  Specifies to return only these tagged resources\.

**Response**
+ `TriggerNames` – An array of UTF\-8 strings\.

  The names of all triggers in the account, or the triggers with the specified tags\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if the returned list does not contain the last metric available\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## BatchGetTriggers Action \(Python: batch\_get\_triggers\)<a name="aws-glue-api-jobs-trigger-BatchGetTriggers"></a>

Returns a list of resource metadata for a given list of trigger names\. After calling the `ListTriggers` operation, you can call this operation to access the data to which you have been granted permissions\. This operation supports all IAM permissions, including permission conditions that uses tags\.

**Request**
+ `TriggerNames` – *Required:* An array of UTF\-8 strings\.

  A list of trigger names, which may be the names returned from the `ListTriggers` operation\.

**Response**
+ `Triggers` – An array of [Trigger](#aws-glue-api-jobs-trigger-Trigger) objects\.

  A list of trigger definitions\.
+ `TriggersNotFound` – An array of UTF\-8 strings\.

  A list of names of triggers not found\.

**Errors**
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`