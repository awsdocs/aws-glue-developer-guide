# Workflows<a name="aws-glue-api-workflow"></a>

The Workflows API describes the data types and API related to creating, updating, or viewing workflows in AWS Glue\.

## Data Types<a name="aws-glue-api-workflow-objects"></a>
+ [JobNodeDetails Structure](#aws-glue-api-workflow-JobNodeDetails)
+ [CrawlerNodeDetails Structure](#aws-glue-api-workflow-CrawlerNodeDetails)
+ [TriggerNodeDetails Structure](#aws-glue-api-workflow-TriggerNodeDetails)
+ [Crawl Structure](#aws-glue-api-workflow-Crawl)
+ [Node Structure](#aws-glue-api-workflow-Node)
+ [Edge Structure](#aws-glue-api-workflow-Edge)
+ [WorkflowGraph Structure](#aws-glue-api-workflow-WorkflowGraph)
+ [WorkflowRun Structure](#aws-glue-api-workflow-WorkflowRun)
+ [WorkflowRunStatistics Structure](#aws-glue-api-workflow-WorkflowRunStatistics)
+ [Workflow Structure](#aws-glue-api-workflow-Workflow)

## JobNodeDetails Structure<a name="aws-glue-api-workflow-JobNodeDetails"></a>

The details of a Job node present in the workflow\.

**Fields**
+ `JobRuns` – An array of [JobRun](aws-glue-api-jobs-runs.md#aws-glue-api-jobs-runs-JobRun) objects\.

  The information for the job runs represented by the job node\.

## CrawlerNodeDetails Structure<a name="aws-glue-api-workflow-CrawlerNodeDetails"></a>

The details of a Crawler node present in the workflow\.

**Fields**
+ `Crawls` – An array of [Crawl](#aws-glue-api-workflow-Crawl) objects\.

  A list of crawls represented by the crawl node\.

## TriggerNodeDetails Structure<a name="aws-glue-api-workflow-TriggerNodeDetails"></a>

The details of a Trigger node present in the workflow\.

**Fields**
+ `Trigger` – A [Trigger](aws-glue-api-jobs-trigger.md#aws-glue-api-jobs-trigger-Trigger) object\.

  The information of the trigger represented by the trigger node\.

## Crawl Structure<a name="aws-glue-api-workflow-Crawl"></a>

The details of a crawl in the workflow\.

**Fields**
+ `State` – UTF\-8 string \(valid values: `RUNNING` \| `SUCCEEDED` \| `CANCELLED` \| `FAILED`\)\.

  The state of the crawler\.
+ `StartedOn` – Timestamp\.

  The date and time on which the crawl started\.
+ `CompletedOn` – Timestamp\.

  The date and time on which the crawl completed\.
+ `ErrorMessage` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  The error message associated with the crawl\.
+ `LogGroup` – UTF\-8 string, not less than 1 or more than 512 bytes long, matching the [Log group string pattern](aws-glue-api-common.md#aws-glue-api-regex-logGroup-id)\.

  The log group associated with the crawl\.
+ `LogStream` – UTF\-8 string, not less than 1 or more than 512 bytes long, matching the [Log-stream string pattern](aws-glue-api-common.md#aws-glue-api-regex-logStream-id)\.

  The log stream associated with the crawl\.

## Node Structure<a name="aws-glue-api-workflow-Node"></a>

A node represents an AWS Glue component like Trigger, Job etc\. which is part of a workflow\.

**Fields**
+ `Type` – UTF\-8 string \(valid values: `CRAWLER` \| `JOB` \| `TRIGGER`\)\.

  The type of AWS Glue component represented by the node\.
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the AWS Glue component represented by the node\.
+ `UniqueId` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The unique Id assigned to the node within the workflow\.
+ `TriggerDetails` – A [TriggerNodeDetails](#aws-glue-api-workflow-TriggerNodeDetails) object\.

  Details of the Trigger when the node represents a Trigger\.
+ `JobDetails` – A [JobNodeDetails](#aws-glue-api-workflow-JobNodeDetails) object\.

  Details of the Job when the node represents a Job\.
+ `CrawlerDetails` – A [CrawlerNodeDetails](#aws-glue-api-workflow-CrawlerNodeDetails) object\.

  Details of the crawler when the node represents a crawler\.

## Edge Structure<a name="aws-glue-api-workflow-Edge"></a>

An edge represents a directed connection between two AWS Glue components which are part of the workflow the edge belongs to\.

**Fields**
+ `SourceId` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The unique of the node within the workflow where the edge starts\.
+ `DestinationId` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The unique of the node within the workflow where the edge ends\.

## WorkflowGraph Structure<a name="aws-glue-api-workflow-WorkflowGraph"></a>

A workflow graph represents the complete workflow containing all the AWS Glue components present in the workflow and all the directed connections between them\.

**Fields**
+ `Nodes` – An array of [Node](#aws-glue-api-workflow-Node) objects\.

  A list of the the AWS Glue components belong to the workflow represented as nodes\.
+ `Edges` – An array of [Edge](#aws-glue-api-workflow-Edge) objects\.

  A list of all the directed connections between the nodes belonging to the workflow\.

## WorkflowRun Structure<a name="aws-glue-api-workflow-WorkflowRun"></a>

A workflow run is an execution of a workflow providing all the runtime information\.

**Fields**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the workflow which was executed\.
+ `WorkflowRunId` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of this workflow run\.
+ `WorkflowRunProperties` – A map array of key\-value pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Each value is a UTF\-8 string\.

  The workflow run properties which were set during the run\.
+ `StartedOn` – Timestamp\.

  The date and time when the workflow run was started\.
+ `CompletedOn` – Timestamp\.

  The date and time when the workflow run completed\.
+ `Status` – UTF\-8 string \(valid values: `RUNNING` \| `COMPLETED`\)\.

  The status of the workflow run\.
+ `Statistics` – A [WorkflowRunStatistics](#aws-glue-api-workflow-WorkflowRunStatistics) object\.

  The statistics of the run\.
+ `Graph` – A [WorkflowGraph](#aws-glue-api-workflow-WorkflowGraph) object\.

  The graph representing all the AWS Glue components that belong to the workflow as nodes and directed connections between them as edges\.

## WorkflowRunStatistics Structure<a name="aws-glue-api-workflow-WorkflowRunStatistics"></a>

Workflow run statistics provides statistics about the workflow run\.

**Fields**
+ `TotalActions` – Number \(integer\)\.

  Total number of Actions in the workflow run\.
+ `TimeoutActions` – Number \(integer\)\.

  Total number of Actions which timed out\.
+ `FailedActions` – Number \(integer\)\.

  Total number of Actions which have failed\.
+ `StoppedActions` – Number \(integer\)\.

  Total number of Actions which have stopped\.
+ `SucceededActions` – Number \(integer\)\.

  Total number of Actions which have succeeded\.
+ `RunningActions` – Number \(integer\)\.

  Total number Actions in running state\.

## Workflow Structure<a name="aws-glue-api-workflow-Workflow"></a>

A workflow represents a flow in which AWS Glue components should be executed to complete a logical task\.

**Fields**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the workflow representing the flow\.
+ `Description` – UTF\-8 string\.

  A description of the workflow\.
+ `DefaultRunProperties` – A map array of key\-value pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Each value is a UTF\-8 string\.

  A collection of properties to be used as part of each execution of the workflow\.
+ `CreatedOn` – Timestamp\.

  The date and time when the workflow was created\.
+ `LastModifiedOn` – Timestamp\.

  The date and time when the workflow was last modified\.
+ `LastRun` – A [WorkflowRun](#aws-glue-api-workflow-WorkflowRun) object\.

  The information about the last execution of the workflow\.
+ `Graph` – A [WorkflowGraph](#aws-glue-api-workflow-WorkflowGraph) object\.

  The graph representing all the AWS Glue components that belong to the workflow as nodes and directed connections between them as edges\.
+ `CreationStatus` – UTF\-8 string \(valid values: `CREATING` \| `CREATED` \| `CREATION_FAILED`\)\.

  The creation status of the workflow\.

## Operations<a name="aws-glue-api-workflow-actions"></a>
+ [CreateWorkflow Action \(Python: create\_workflow\)](#aws-glue-api-workflow-CreateWorkflow)
+ [UpdateWorkflow Action \(Python: update\_workflow\)](#aws-glue-api-workflow-UpdateWorkflow)
+ [DeleteWorkflow Action \(Python: delete\_workflow\)](#aws-glue-api-workflow-DeleteWorkflow)
+ [ListWorkflows Action \(Python: list\_workflows\)](#aws-glue-api-workflow-ListWorkflows)
+ [BatchGetWorkflows Action \(Python: batch\_get\_workflows\)](#aws-glue-api-workflow-BatchGetWorkflows)
+ [GetWorkflowRun Action \(Python: get\_workflow\_run\)](#aws-glue-api-workflow-GetWorkflowRun)
+ [GetWorkflowRuns Action \(Python: get\_workflow\_runs\)](#aws-glue-api-workflow-GetWorkflowRuns)
+ [GetWorkflowRunProperties Action \(Python: get\_workflow\_run\_properties\)](#aws-glue-api-workflow-GetWorkflowRunProperties)
+ [PutWorkflowRunProperties Action \(Python: put\_workflow\_run\_properties\)](#aws-glue-api-workflow-PutWorkflowRunProperties)

## CreateWorkflow Action \(Python: create\_workflow\)<a name="aws-glue-api-workflow-CreateWorkflow"></a>

Creates a new workflow\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name to be assigned to the workflow\. It should be unique within your account\.
+ `Description` – UTF\-8 string\.

  A description of the workflow\.
+ `DefaultRunProperties` – A map array of key\-value pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Each value is a UTF\-8 string\.

  A collection of properties to be used as part of each execution of the workflow\.
+ `Tags` – A map array of key\-value pairs, not more than 50 pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 128 bytes long\.

  Each value is a UTF\-8 string, not more than 256 bytes long\.

  The tags to be used with this workflow\.

**Response**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the workflow which was provided as part of the request\.

**Errors**
+ `AlreadyExistsException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `ResourceNumberLimitExceededException`
+ `ConcurrentModificationException`

## UpdateWorkflow Action \(Python: update\_workflow\)<a name="aws-glue-api-workflow-UpdateWorkflow"></a>

Updates an existing workflow\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the workflow to be updated\.
+ `Description` – UTF\-8 string\.

  The description of the workflow\.
+ `DefaultRunProperties` – A map array of key\-value pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Each value is a UTF\-8 string\.

  A collection of properties to be used as part of each execution of the workflow\.

**Response**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the workflow which was specified in input\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `ConcurrentModificationException`

## DeleteWorkflow Action \(Python: delete\_workflow\)<a name="aws-glue-api-workflow-DeleteWorkflow"></a>

Deletes a workflow\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the workflow to be deleted\.

**Response**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the workflow specified in input\.

**Errors**
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `ConcurrentModificationException`

## ListWorkflows Action \(Python: list\_workflows\)<a name="aws-glue-api-workflow-ListWorkflows"></a>

Lists names of workflows created in the account\.

**Request**
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation request\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum size of a list to return\.

**Response**
+ `Workflows` – An array of UTF\-8 strings, not less than 1 or more than 25 strings\.

  List of names of workflows in the account\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if not all workflow names have been returned\.

**Errors**
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## BatchGetWorkflows Action \(Python: batch\_get\_workflows\)<a name="aws-glue-api-workflow-BatchGetWorkflows"></a>

Returns a list of resource metadata for a given list of workflow names\. After calling the `ListWorkflows` operation, you can call this operation to access the data to which you have been granted permissions\. This operation supports all IAM permissions, including permission conditions that uses tags\.

**Request**
+ `Names` – *Required:* An array of UTF\-8 strings, not less than 1 or more than 25 strings\.

  A list of workflow names, which may be the names returned from the `ListWorkflows` operation\.
+ `IncludeGraph` – Boolean\.

  Specifies whether to include a graph when returning the workflow resource metadata\.

**Response**
+ `Workflows` – An array of [Workflow](#aws-glue-api-workflow-Workflow) objects, not less than 1 or more than 25 structures\.

  A list of workflow resource metadata\.
+ `MissingWorkflows` – An array of UTF\-8 strings, not less than 1 or more than 25 strings\.

  A list of names of workflows not found\.

**Errors**
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`

## GetWorkflowRun Action \(Python: get\_workflow\_run\)<a name="aws-glue-api-workflow-GetWorkflowRun"></a>

Retrieves the metadata for a given workflow run\. 

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the workflow being run\.
+ `RunId` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the workflow run\.
+ `IncludeGraph` – Boolean\.

  Specifies whether to include the workflow graph in response or not\.

**Response**
+ `Run` – A [WorkflowRun](#aws-glue-api-workflow-WorkflowRun) object\.

  The requested workflow run metadata\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## GetWorkflowRuns Action \(Python: get\_workflow\_runs\)<a name="aws-glue-api-workflow-GetWorkflowRuns"></a>

Retrieves metadata for all runs of a given workflow\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the workflow whose metadata of runs should be returned\.
+ `IncludeGraph` – Boolean\.

  Specifies whether to include the workflow graph in response or not\.
+ `NextToken` – UTF\-8 string\.

  The maximum size of the response\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum number of workflow runs to be included in the response\.

**Response**
+ `Runs` – An array of [WorkflowRun](#aws-glue-api-workflow-WorkflowRun) objects, not less than 1 or more than 1000 structures\.

  A list of workflow run metadata objects\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if not all requested workflow runs have been returned\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## GetWorkflowRunProperties Action \(Python: get\_workflow\_run\_properties\)<a name="aws-glue-api-workflow-GetWorkflowRunProperties"></a>

Retrieves the workflow run properties which were set during the run\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the workflow which was run\.
+ `RunId` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the workflow run whose run properties should be returned\.

**Response**
+ `RunProperties` – A map array of key\-value pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Each value is a UTF\-8 string\.

  The workflow run properties which were set during the specified run\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## PutWorkflowRunProperties Action \(Python: put\_workflow\_run\_properties\)<a name="aws-glue-api-workflow-PutWorkflowRunProperties"></a>

Puts the specified workflow run properties for the given workflow run\. If a property already exists for the specified run, then it overrides the value otherwise adds the property to existing properties\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the workflow which was run\.
+ `RunId` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the workflow run for which the run properties should be updated\.
+ `RunProperties` – *Required:* A map array of key\-value pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Each value is a UTF\-8 string\.

  The properties to put for the specified run\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `AlreadyExistsException`
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `ResourceNumberLimitExceededException`
+ `ConcurrentModificationException`