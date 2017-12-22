# AWS Glue API Permissions: Actions and Resources Reference<a name="api-permissions-reference"></a>

When you are setting up [Access Control](authentication-and-access-control.md#access-control) and writing a permissions policy that you can attach to an IAM identity \(identity\-based policies\), you can use the following table as a reference\. The table lists each AWS Glue API operation, the corresponding actions for which you can grant permissions to perform the action, and the AWS resource for which you can grant the permissions\. You specify the actions in the policy's `Action` field, and you specify the resource value in the policy's `Resource` field\. 

You can use AWS\-wide condition keys in your AWS Glue policies to express conditions\. For a complete list of AWS\-wide keys, see [Available Keys](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\. 

**Note**  
To specify an action, use the `glue:` prefix followed by the API operation name \(for example, `glue:GetTable`\)\.

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.


**AWS Glue API and Required Permissions for Actions**  

| AWS Glue API Operations | Required Permissions \(API Actions\) | Resources | 
| --- | --- | --- | 
| [BatchCreatePartition Action \(Python: batch\_create\_partition\)](aws-glue-api-catalog-partitions.md#aws-glue-api-catalog-partitions-BatchCreatePartition) | glue:BatchCreatePartition | \* | 
| [BatchDeleteConnection Action \(Python: batch\_delete\_connection\)](aws-glue-api-catalog-connections.md#aws-glue-api-catalog-connections-BatchDeleteConnection) | glue:BatchDeleteConnection | \* | 
| [BatchDeletePartition Action \(Python: batch\_delete\_partition\)](aws-glue-api-catalog-partitions.md#aws-glue-api-catalog-partitions-BatchDeletePartition) | glue:BatchDeletePartition | \* | 
| [BatchDeleteTable Action \(Python: batch\_delete\_table\)](aws-glue-api-catalog-tables.md#aws-glue-api-catalog-tables-BatchDeleteTable) | glue:BatchDeletetTable | \* | 
| [BatchGetPartition Action \(Python: batch\_get\_partition\)](aws-glue-api-catalog-partitions.md#aws-glue-api-catalog-partitions-BatchGetPartition) | glue:BatchGetPartition | \* | 
| [BatchStopJobRun Action \(Python: batch\_stop\_job\_run\)](aws-glue-api-jobs-runs.md#aws-glue-api-jobs-runs-BatchStopJobRun) | glue:BatchStopJobRun | \* | 
| [CreateClassifier Action \(Python: create\_classifier\)](aws-glue-api-crawler-classifiers.md#aws-glue-api-crawler-classifiers-CreateClassifier) | glue:CreateClassifier | \* | 
| [CreateConnection Action \(Python: create\_connection\)](aws-glue-api-catalog-connections.md#aws-glue-api-catalog-connections-CreateConnection) | glue:CreateConnection | \* | 
| [CreateCrawler Action \(Python: create\_crawler\)](aws-glue-api-crawler-crawling.md#aws-glue-api-crawler-crawling-CreateCrawler) | glue:CreateCrawler | \* | 
| [CreateDatabase Action \(Python: create\_database\)](aws-glue-api-catalog-databases.md#aws-glue-api-catalog-databases-CreateDatabase) | glue:CreateDatabase | \* | 
| [CreateDevEndpoint Action \(Python: create\_dev\_endpoint\)](aws-glue-api-dev-endpoint.md#aws-glue-api-dev-endpoint-CreateDevEndpoint) | glue:CreateDevEndpoint | \* | 
| [CreateJob Action \(Python: create\_job\)](aws-glue-api-jobs-job.md#aws-glue-api-jobs-job-CreateJob) | glue:CreateJob | \* | 
| [CreatePartition Action \(Python: create\_partition\)](aws-glue-api-catalog-partitions.md#aws-glue-api-catalog-partitions-CreatePartition) | glue:CreatePartition | \* | 
| [CreateScript Action \(Python: create\_script\)](aws-glue-api-etl-script-generation.md#aws-glue-api-etl-script-generation-CreateScript) | glue:CreateScript | \* | 
| [CreateTable Action \(Python: create\_table\)](aws-glue-api-catalog-tables.md#aws-glue-api-catalog-tables-CreateTable) | glue:CreateTable | \* | 
| [CreateTrigger Action \(Python: create\_trigger\)](aws-glue-api-jobs-trigger.md#aws-glue-api-jobs-trigger-CreateTrigger) | glue:CreateTrigger | \* | 
| [CreateUserDefinedFunction Action \(Python: create\_user\_defined\_function\)](aws-glue-api-catalog-functions.md#aws-glue-api-catalog-functions-CreateUserDefinedFunction) | glue:CreateUserDefinedFunction | \* | 
| [DeleteClassifier Action \(Python: delete\_classifier\)](aws-glue-api-crawler-classifiers.md#aws-glue-api-crawler-classifiers-DeleteClassifier) | glue:DeleteClassifier | \* | 
| [DeleteConnection Action \(Python: delete\_connection\)](aws-glue-api-catalog-connections.md#aws-glue-api-catalog-connections-DeleteConnection) | glue:DeleteConnection | \* | 
| [DeleteCrawler Action \(Python: delete\_crawler\)](aws-glue-api-crawler-crawling.md#aws-glue-api-crawler-crawling-DeleteCrawler) | glue:DeleteCrawler | \* | 
| [DeleteDatabase Action \(Python: delete\_database\)](aws-glue-api-catalog-databases.md#aws-glue-api-catalog-databases-DeleteDatabase) | glue:DeleteDatabase | \* | 
| [DeleteDevEndpoint Action \(Python: delete\_dev\_endpoint\)](aws-glue-api-dev-endpoint.md#aws-glue-api-dev-endpoint-DeleteDevEndpoint) | glue:DeleteDevEndpoint | \* | 
| [DeleteJob Action \(Python: delete\_job\)](aws-glue-api-jobs-job.md#aws-glue-api-jobs-job-DeleteJob) | glue:DeleteJob | \* | 
| [DeletePartition Action \(Python: delete\_partition\)](aws-glue-api-catalog-partitions.md#aws-glue-api-catalog-partitions-DeletePartition) | glue:DeletePartition | \* | 
| [DeleteTable Action \(Python: delete\_table\)](aws-glue-api-catalog-tables.md#aws-glue-api-catalog-tables-DeleteTable) | glue:DeleteTable | \* | 
| [DeleteTrigger Action \(Python: delete\_trigger\)](aws-glue-api-jobs-trigger.md#aws-glue-api-jobs-trigger-DeleteTrigger) | glue:DeleteTrigger | \* | 
| [DeleteUserDefinedFunction Action \(Python: delete\_user\_defined\_function\)](aws-glue-api-catalog-functions.md#aws-glue-api-catalog-functions-DeleteUserDefinedFunction) | glue:DeleteUserDefinedFunction | \* | 
| [GetCatalogImportStatus Action \(Python: get\_catalog\_import\_status\)](aws-glue-api-catalog-migration.md#aws-glue-api-catalog-migration-GetCatalogImportStatus) | glue:GetCatalogImportStatus | \* | 
| [GetClassifier Action \(Python: get\_classifier\)](aws-glue-api-crawler-classifiers.md#aws-glue-api-crawler-classifiers-GetClassifier) | glue:GetClassifier | \* | 
| [GetClassifiers Action \(Python: get\_classifiers\)](aws-glue-api-crawler-classifiers.md#aws-glue-api-crawler-classifiers-GetClassifiers) | glue:GetClassifiers | \* | 
| [GetConnection Action \(Python: get\_connection\)](aws-glue-api-catalog-connections.md#aws-glue-api-catalog-connections-GetConnection) | glue:GetConnection | \* | 
| [GetConnections Action \(Python: get\_connections\)](aws-glue-api-catalog-connections.md#aws-glue-api-catalog-connections-GetConnections) | glue:GetConnections | \* | 
| [GetCrawler Action \(Python: get\_crawler\)](aws-glue-api-crawler-crawling.md#aws-glue-api-crawler-crawling-GetCrawler) | glue:GetCrawler | \* | 
| [GetCrawlerMetrics Action \(Python: get\_crawler\_metrics\)](aws-glue-api-crawler-crawling.md#aws-glue-api-crawler-crawling-GetCrawlerMetrics) | glue:GetCrawlerMetrics | \* | 
| [GetCrawlers Action \(Python: get\_crawlers\)](aws-glue-api-crawler-crawling.md#aws-glue-api-crawler-crawling-GetCrawlers) | glue:GetCrawlers | \* | 
| [GetDatabase Action \(Python: get\_database\)](aws-glue-api-catalog-databases.md#aws-glue-api-catalog-databases-GetDatabase) | glue:GetDatabase | \* | 
| [GetDatabases Action \(Python: get\_databases\)](aws-glue-api-catalog-databases.md#aws-glue-api-catalog-databases-GetDatabases) | glue:GetDatabases | \* | 
| [GetDataflowGraph Action \(Python: get\_dataflow\_graph\)](aws-glue-api-etl-script-generation.md#aws-glue-api-etl-script-generation-GetDataflowGraph) | glue:GetDataflowGraph | \* | 
| [GetDevEndpoint Action \(Python: get\_dev\_endpoint\)](aws-glue-api-dev-endpoint.md#aws-glue-api-dev-endpoint-GetDevEndpoint) | glue:GetDevEndpoint | \* | 
| [GetDevEndpoints Action \(Python: get\_dev\_endpoints\)](aws-glue-api-dev-endpoint.md#aws-glue-api-dev-endpoint-GetDevEndpoints) | glue:GetDevEndpoints | \* | 
| [GetJob Action \(Python: get\_job\)](aws-glue-api-jobs-job.md#aws-glue-api-jobs-job-GetJob) | glue:GetJob | \* | 
| [GetJobRun Action \(Python: get\_job\_run\)](aws-glue-api-jobs-runs.md#aws-glue-api-jobs-runs-GetJobRun) | glue:GetJobRun | \* | 
| [GetJobRuns Action \(Python: get\_job\_runs\)](aws-glue-api-jobs-runs.md#aws-glue-api-jobs-runs-GetJobRuns) | glue:GetJobRuns | \* | 
| [GetJobs Action \(Python: get\_jobs\)](aws-glue-api-jobs-job.md#aws-glue-api-jobs-job-GetJobs) | glue:GetJobs | \* | 
| [GetMapping Action \(Python: get\_mapping\)](aws-glue-api-etl-script-generation.md#aws-glue-api-etl-script-generation-GetMapping) | glue:GetMapping | \* | 
| [GetPartition Action \(Python: get\_partition\)](aws-glue-api-catalog-partitions.md#aws-glue-api-catalog-partitions-GetPartition) | glue:GetPartition | \* | 
| [GetPartitions Action \(Python: get\_partitions\)](aws-glue-api-catalog-partitions.md#aws-glue-api-catalog-partitions-GetPartitions) | glue:GetPartitions | \* | 
| [GetTable Action \(Python: get\_table\)](aws-glue-api-catalog-tables.md#aws-glue-api-catalog-tables-GetTable) | glue:GetTable | \* | 
| [GetTables Action \(Python: get\_tables\)](aws-glue-api-catalog-tables.md#aws-glue-api-catalog-tables-GetTables) | glue:GetTables | \* | 
| [GetTableVersions Action \(Python: get\_table\_versions\)](aws-glue-api-catalog-tables.md#aws-glue-api-catalog-tables-GetTableVersions) | glue:GetTableVersions | \* | 
| [GetTrigger Action \(Python: get\_trigger\)](aws-glue-api-jobs-trigger.md#aws-glue-api-jobs-trigger-GetTrigger) | glue:GetTrigger | \* | 
| [GetTriggers Action \(Python: get\_triggers\)](aws-glue-api-jobs-trigger.md#aws-glue-api-jobs-trigger-GetTriggers) | glue:GetTriggers | \* | 
| [GetUserDefinedFunction Action \(Python: get\_user\_defined\_function\)](aws-glue-api-catalog-functions.md#aws-glue-api-catalog-functions-GetUserDefinedFunction) | glue:GetUserDefinedFunction | \* | 
| [GetUserDefinedFunctions Action \(Python: get\_user\_defined\_functions\)](aws-glue-api-catalog-functions.md#aws-glue-api-catalog-functions-GetUserDefinedFunctions) | glue:GetUserDefinedFunctions | \* | 
| [ImportCatalogToGlue Action \(Python: import\_catalog\_to\_glue\)](aws-glue-api-catalog-migration.md#aws-glue-api-catalog-migration-ImportCatalogToGlue) | glue:ImportCatalogToGlue | \* | 
| [ResetJobBookmark Action \(Python: reset\_job\_bookmark\)](aws-glue-api-jobs-runs.md#aws-glue-api-jobs-runs-ResetJobBookmark) | glue:ResetJobBookmark | \* | 
| [StartCrawler Action \(Python: start\_crawler\)](aws-glue-api-crawler-crawling.md#aws-glue-api-crawler-crawling-StartCrawler) | glue:StartCrawler | \* | 
| [StartCrawlerSchedule Action \(Python: start\_crawler\_schedule\)](aws-glue-api-crawler-scheduler.md#aws-glue-api-crawler-scheduler-StartCrawlerSchedule) | glue:StartCrawlerSchedule | \* | 
| [StartJobRun Action \(Python: start\_job\_run\)](aws-glue-api-jobs-runs.md#aws-glue-api-jobs-runs-StartJobRun) | glue:StartJobRun | \* | 
| [StartTrigger Action \(Python: start\_trigger\)](aws-glue-api-jobs-trigger.md#aws-glue-api-jobs-trigger-StartTrigger) | glue:StartTrigger | \* | 
| [StopCrawler Action \(Python: stop\_crawler\)](aws-glue-api-crawler-crawling.md#aws-glue-api-crawler-crawling-StopCrawler) | glue:StopCrawler | \* | 
| [StopCrawlerSchedule Action \(Python: stop\_crawler\_schedule\)](aws-glue-api-crawler-scheduler.md#aws-glue-api-crawler-scheduler-StopCrawlerSchedule) | glue:StopCrawlerSchedule | \* | 
| [StopTrigger Action \(Python: stop\_trigger\)](aws-glue-api-jobs-trigger.md#aws-glue-api-jobs-trigger-StopTrigger) | glue:StopTrigger | \* | 
| [UpdateClassifier Action \(Python: update\_classifier\)](aws-glue-api-crawler-classifiers.md#aws-glue-api-crawler-classifiers-UpdateClassifier) | glue:UpdateClassifier | \* | 
| [UpdateConnection Action \(Python: update\_connection\)](aws-glue-api-catalog-connections.md#aws-glue-api-catalog-connections-UpdateConnection) | glue:UpdateConnection | \* | 
| [UpdateCrawler Action \(Python: update\_crawler\)](aws-glue-api-crawler-crawling.md#aws-glue-api-crawler-crawling-UpdateCrawler) | glue:UpdateCrawler | \* | 
| [UpdateCrawlerSchedule Action \(Python: update\_crawler\_schedule\)](aws-glue-api-crawler-scheduler.md#aws-glue-api-crawler-scheduler-UpdateCrawlerSchedule) | glue:UpdateCrawlerSchedule | \* | 
| [UpdateDatabase Action \(Python: update\_database\)](aws-glue-api-catalog-databases.md#aws-glue-api-catalog-databases-UpdateDatabase) | glue:UpdateDatabase | \* | 
| [UpdateDevEndpoint Action \(Python: update\_dev\_endpoint\)](aws-glue-api-dev-endpoint.md#aws-glue-api-dev-endpoint-UpdateDevEndpoint) | glue:UpdateDevEndpoint | \* | 
| [UpdateJob Action \(Python: update\_job\)](aws-glue-api-jobs-job.md#aws-glue-api-jobs-job-UpdateJob) | glue:UpdateJob | \* | 
| [UpdatePartition Action \(Python: update\_partition\)](aws-glue-api-catalog-partitions.md#aws-glue-api-catalog-partitions-UpdatePartition) | glue:UpdatePartition | \* | 
| [UpdateTable Action \(Python: update\_table\)](aws-glue-api-catalog-tables.md#aws-glue-api-catalog-tables-UpdateTable) | glue:UpdateTable | \* | 
| [UpdateTrigger Action \(Python: update\_trigger\)](aws-glue-api-jobs-trigger.md#aws-glue-api-jobs-trigger-UpdateTrigger) | glue:UpdateTrigger | \* | 
| [UpdateUserDefinedFunction Action \(Python: update\_user\_defined\_function\)](aws-glue-api-catalog-functions.md#aws-glue-api-catalog-functions-UpdateUserDefinedFunction) | glue:UpdatateUserDefinedFunction | \* | 

## Related Topics<a name="w3ab1c15c17c23"></a>

+ [Access Control](authentication-and-access-control.md#access-control)