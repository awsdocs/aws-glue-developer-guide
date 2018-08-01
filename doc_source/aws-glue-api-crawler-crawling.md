# Crawler API<a name="aws-glue-api-crawler-crawling"></a>

## Data Types<a name="aws-glue-api-crawler-crawling-objects"></a>
+ [Crawler Structure](#aws-glue-api-crawler-crawling-Crawler)
+ [Schedule Structure](#aws-glue-api-crawler-crawling-Schedule)
+ [CrawlerTargets Structure](#aws-glue-api-crawler-crawling-CrawlerTargets)
+ [S3Target Structure](#aws-glue-api-crawler-crawling-S3Target)
+ [JdbcTarget Structure](#aws-glue-api-crawler-crawling-JdbcTarget)
+ [DynamoDBTarget Structure](#aws-glue-api-crawler-crawling-DynamoDBTarget)
+ [CrawlerMetrics Structure](#aws-glue-api-crawler-crawling-CrawlerMetrics)
+ [SchemaChangePolicy Structure](#aws-glue-api-crawler-crawling-SchemaChangePolicy)
+ [LastCrawlInfo Structure](#aws-glue-api-crawler-crawling-LastCrawlInfo)

## Crawler Structure<a name="aws-glue-api-crawler-crawling-Crawler"></a>

Specifies a crawler program that examines a data source and uses classifiers to try to determine its schema\. If successful, the crawler records metadata concerning the data source in the AWS Glue Data Catalog\.

**Fields**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The crawler name\.
+ `Role` – UTF\-8 string\.

  The IAM role \(or ARN of an IAM role\) used to access customer resources, such as data in Amazon S3\.
+ `Targets` – A [CrawlerTargets](#aws-glue-api-crawler-crawling-CrawlerTargets) object\.

  A collection of targets to crawl\.
+ `DatabaseName` – UTF\-8 string\.

  The database where metadata is written by this crawler\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of the crawler\.
+ `Classifiers` – An array of UTF\-8 strings\.

  A list of custom classifiers associated with the crawler\.
+ `SchemaChangePolicy` – A [SchemaChangePolicy](#aws-glue-api-crawler-crawling-SchemaChangePolicy) object\.

  Sets the behavior when the crawler finds a changed or deleted object\.
+ `State` – UTF\-8 string \(valid values: `READY` \| `RUNNING` \| `STOPPING`\)\.

  Indicates whether the crawler is running, or whether a run is pending\.
+ `TablePrefix` – UTF\-8 string, not more than 128 bytes long\.

  The prefix added to the names of tables that are created\.
+ `Schedule` – A [Schedule](aws-glue-api-crawler-scheduler.md#aws-glue-api-crawler-scheduler-Schedule) object\.

  For scheduled crawlers, the schedule when the crawler runs\.
+ `CrawlElapsedTime` – Number \(long\)\.

  If the crawler is running, contains the total time elapsed since the last crawl began\.
+ `CreationTime` – Timestamp\.

  The time when the crawler was created\.
+ `LastUpdated` – Timestamp\.

  The time the crawler was last updated\.
+ `LastCrawl` – A [LastCrawlInfo](#aws-glue-api-crawler-crawling-LastCrawlInfo) object\.

  The status of the last crawl, and potentially error information if an error occurred\.
+ `Version` – Number \(long\)\.

  The version of the crawler\.
+ `Configuration` – UTF\-8 string\.

  Crawler configuration information\. This versioned JSON string allows users to specify aspects of a crawler's behavior\. For more information, see [Configuring a Crawler](http://docs.aws.amazon.com/glue/latest/dg/crawler-configuration.html)\.

## Schedule Structure<a name="aws-glue-api-crawler-crawling-Schedule"></a>

A scheduling object using a `cron` statement to schedule an event\.

**Fields**
+ `ScheduleExpression` – UTF\-8 string\.

  A `cron` expression used to specify the schedule \(see [Time\-Based Schedules for Jobs and Crawlers](http://docs.aws.amazon.com/glue/latest/dg/monitor-data-warehouse-schedule.html)\. For example, to run something every day at 12:15 UTC, you would specify: `cron(15 12 * * ? *)`\.
+ `State` – UTF\-8 string \(valid values: `SCHEDULED` \| `NOT_SCHEDULED` \| `TRANSITIONING`\)\.

  The state of the schedule\.

## CrawlerTargets Structure<a name="aws-glue-api-crawler-crawling-CrawlerTargets"></a>

Specifies data stores to crawl\.

**Fields**
+ `S3Targets` – An array of [S3Target](#aws-glue-api-crawler-crawling-S3Target) objects\.

  Specifies Amazon S3 targets\.
+ `JdbcTargets` – An array of [JdbcTarget](#aws-glue-api-crawler-crawling-JdbcTarget) objects\.

  Specifies JDBC targets\.
+ `DynamoDBTargets` – An array of [DynamoDBTarget](#aws-glue-api-crawler-crawling-DynamoDBTarget) objects\.

  Specifies DynamoDB targets\.

## S3Target Structure<a name="aws-glue-api-crawler-crawling-S3Target"></a>

Specifies a data store in Amazon S3\.

**Fields**
+ `Path` – UTF\-8 string\.

  The path to the Amazon S3 target\.
+ `Exclusions` – An array of UTF\-8 strings\.

  A list of glob patterns used to exclude from the crawl\. For more information, see [Catalog Tables with a Crawler](http://docs.aws.amazon.com/glue/latest/dg/add-crawler.html)\.

## JdbcTarget Structure<a name="aws-glue-api-crawler-crawling-JdbcTarget"></a>

Specifies a JDBC data store to crawl\.

**Fields**
+ `ConnectionName` – UTF\-8 string\.

  The name of the connection to use to connect to the JDBC target\.
+ `Path` – UTF\-8 string\.

  The path of the JDBC target\.
+ `Exclusions` – An array of UTF\-8 strings\.

  A list of glob patterns used to exclude from the crawl\. For more information, see [Catalog Tables with a Crawler](http://docs.aws.amazon.com/glue/latest/dg/add-crawler.html)\.

## DynamoDBTarget Structure<a name="aws-glue-api-crawler-crawling-DynamoDBTarget"></a>

Specifies a DynamoDB table to crawl\.

**Fields**
+ `Path` – UTF\-8 string\.

  The name of the DynamoDB table to crawl\.

## CrawlerMetrics Structure<a name="aws-glue-api-crawler-crawling-CrawlerMetrics"></a>

Metrics for a specified crawler\.

**Fields**
+ `CrawlerName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the crawler\.
+ `TimeLeftSeconds` – Number \(double\), not more than None\.

  The estimated time left to complete a running crawl\.
+ `StillEstimating` – Boolean\.

  True if the crawler is still estimating how long it will take to complete this run\.
+ `LastRuntimeSeconds` – Number \(double\), not more than None\.

  The duration of the crawler's most recent run, in seconds\.
+ `MedianRuntimeSeconds` – Number \(double\), not more than None\.

  The median duration of this crawler's runs, in seconds\.
+ `TablesCreated` – Number \(integer\), not more than None\.

  The number of tables created by this crawler\.
+ `TablesUpdated` – Number \(integer\), not more than None\.

  The number of tables updated by this crawler\.
+ `TablesDeleted` – Number \(integer\), not more than None\.

  The number of tables deleted by this crawler\.

## SchemaChangePolicy Structure<a name="aws-glue-api-crawler-crawling-SchemaChangePolicy"></a>

Crawler policy for update and deletion behavior\.

**Fields**
+ `UpdateBehavior` – UTF\-8 string \(valid values: `LOG` \| `UPDATE_IN_DATABASE`\)\.

  The update behavior when the crawler finds a changed schema\.
+ `DeleteBehavior` – UTF\-8 string \(valid values: `LOG` \| `DELETE_FROM_DATABASE` \| `DEPRECATE_IN_DATABASE`\)\.

  The deletion behavior when the crawler finds a deleted object\.

## LastCrawlInfo Structure<a name="aws-glue-api-crawler-crawling-LastCrawlInfo"></a>

Status and error information about the most recent crawl\.

**Fields**
+ `Status` – UTF\-8 string \(valid values: `SUCCEEDED` \| `CANCELLED` \| `FAILED`\)\.

  Status of the last crawl\.
+ `ErrorMessage` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  If an error occurred, the error information about the last crawl\.
+ `LogGroup` – UTF\-8 string, not less than 1 or more than 512 bytes long, matching the [Log group string pattern](aws-glue-api-common.md#aws-glue-api-regex-logGroup-id)\.

  The log group for the last crawl\.
+ `LogStream` – UTF\-8 string, not less than 1 or more than 512 bytes long, matching the [Log-stream string pattern](aws-glue-api-common.md#aws-glue-api-regex-logStream-id)\.

  The log stream for the last crawl\.
+ `MessagePrefix` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The prefix for a message about this crawl\.
+ `StartTime` – Timestamp\.

  The time at which the crawl started\.

## Operations<a name="aws-glue-api-crawler-crawling-actions"></a>
+ [CreateCrawler Action \(Python: create\_crawler\)](#aws-glue-api-crawler-crawling-CreateCrawler)
+ [DeleteCrawler Action \(Python: delete\_crawler\)](#aws-glue-api-crawler-crawling-DeleteCrawler)
+ [GetCrawler Action \(Python: get\_crawler\)](#aws-glue-api-crawler-crawling-GetCrawler)
+ [GetCrawlers Action \(Python: get\_crawlers\)](#aws-glue-api-crawler-crawling-GetCrawlers)
+ [GetCrawlerMetrics Action \(Python: get\_crawler\_metrics\)](#aws-glue-api-crawler-crawling-GetCrawlerMetrics)
+ [UpdateCrawler Action \(Python: update\_crawler\)](#aws-glue-api-crawler-crawling-UpdateCrawler)
+ [StartCrawler Action \(Python: start\_crawler\)](#aws-glue-api-crawler-crawling-StartCrawler)
+ [StopCrawler Action \(Python: stop\_crawler\)](#aws-glue-api-crawler-crawling-StopCrawler)

## CreateCrawler Action \(Python: create\_crawler\)<a name="aws-glue-api-crawler-crawling-CreateCrawler"></a>

Creates a new crawler with specified targets, role, configuration, and optional schedule\. At least one crawl target must be specified, in the *s3Targets* field, the *jdbcTargets* field, or the *DynamoDBTargets* field\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the new crawler\.
+ `Role` – *Required:* UTF\-8 string\.

  The IAM role \(or ARN of an IAM role\) used by the new crawler to access customer resources\.
+ `DatabaseName` – *Required:* UTF\-8 string\.

  The AWS Glue database where results are written, such as: `arn:aws:daylight:us-east-1::database/sometable/*`\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of the new crawler\.
+ `Targets` – *Required:* A [CrawlerTargets](#aws-glue-api-crawler-crawling-CrawlerTargets) object\.

  A list of collection of targets to crawl\.
+ `Schedule` – UTF\-8 string\.

  A `cron` expression used to specify the schedule \(see [Time\-Based Schedules for Jobs and Crawlers](http://docs.aws.amazon.com/glue/latest/dg/monitor-data-warehouse-schedule.html)\. For example, to run something every day at 12:15 UTC, you would specify: `cron(15 12 * * ? *)`\.
+ `Classifiers` – An array of UTF\-8 strings\.

  A list of custom classifiers that the user has registered\. By default, all built\-in classifiers are included in a crawl, but these custom classifiers always override the default classifiers for a given classification\.
+ `TablePrefix` – UTF\-8 string, not more than 128 bytes long\.

  The table prefix used for catalog tables that are created\.
+ `SchemaChangePolicy` – A [SchemaChangePolicy](#aws-glue-api-crawler-crawling-SchemaChangePolicy) object\.

  Policy for the crawler's update and deletion behavior\.
+ `Configuration` – UTF\-8 string\.

  Crawler configuration information\. This versioned JSON string allows users to specify aspects of a crawler's behavior\. For more information, see [Configuring a Crawler](http://docs.aws.amazon.com/glue/latest/dg/crawler-configuration.html)\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `InvalidInputException`
+ `AlreadyExistsException`
+ `OperationTimeoutException`
+ `ResourceNumberLimitExceededException`

## DeleteCrawler Action \(Python: delete\_crawler\)<a name="aws-glue-api-crawler-crawling-DeleteCrawler"></a>

Removes a specified crawler from the Data Catalog, unless the crawler state is `RUNNING`\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the crawler to remove\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `CrawlerRunningException`
+ `SchedulerTransitioningException`
+ `OperationTimeoutException`

## GetCrawler Action \(Python: get\_crawler\)<a name="aws-glue-api-crawler-crawling-GetCrawler"></a>

Retrieves metadata for a specified crawler\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the crawler to retrieve metadata for\.

**Response**
+ `Crawler` – A [Crawler](#aws-glue-api-crawler-crawling-Crawler) object\.

  The metadata for the specified crawler\.

**Errors**
+ `EntityNotFoundException`
+ `OperationTimeoutException`

## GetCrawlers Action \(Python: get\_crawlers\)<a name="aws-glue-api-crawler-crawling-GetCrawlers"></a>

Retrieves metadata for all crawlers defined in the customer account\.

**Request**
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The number of crawlers to return on each call\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation request\.

**Response**
+ `Crawlers` – An array of [Crawler](#aws-glue-api-crawler-crawling-Crawler) objects\.

  A list of crawler metadata\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if the returned list has not reached the end of those defined in this customer account\.

**Errors**
+ `OperationTimeoutException`

## GetCrawlerMetrics Action \(Python: get\_crawler\_metrics\)<a name="aws-glue-api-crawler-crawling-GetCrawlerMetrics"></a>

Retrieves metrics about specified crawlers\.

**Request**
+ `CrawlerNameList` – An array of UTF\-8 strings, not more than 100 strings\.

  A list of the names of crawlers about which to retrieve metrics\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum size of a list to return\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation call\.

**Response**
+ `CrawlerMetricsList` – An array of [CrawlerMetrics](#aws-glue-api-crawler-crawling-CrawlerMetrics) objects\.

  A list of metrics for the specified crawler\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if the returned list does not contain the last metric available\.

**Errors**
+ `OperationTimeoutException`

## UpdateCrawler Action \(Python: update\_crawler\)<a name="aws-glue-api-crawler-crawling-UpdateCrawler"></a>

Updates a crawler\. If a crawler is running, you must stop it using `StopCrawler` before updating it\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the new crawler\.
+ `Role` – UTF\-8 string\.

  The IAM role \(or ARN of an IAM role\) used by the new crawler to access customer resources\.
+ `DatabaseName` – UTF\-8 string\.

  The AWS Glue database where results are stored, such as: `arn:aws:daylight:us-east-1::database/sometable/*`\.
+ `Description` – UTF\-8 string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of the new crawler\.
+ `Targets` – A [CrawlerTargets](#aws-glue-api-crawler-crawling-CrawlerTargets) object\.

  A list of targets to crawl\.
+ `Schedule` – UTF\-8 string\.

  A `cron` expression used to specify the schedule \(see [Time\-Based Schedules for Jobs and Crawlers](http://docs.aws.amazon.com/glue/latest/dg/monitor-data-warehouse-schedule.html)\. For example, to run something every day at 12:15 UTC, you would specify: `cron(15 12 * * ? *)`\.
+ `Classifiers` – An array of UTF\-8 strings\.

  A list of custom classifiers that the user has registered\. By default, all built\-in classifiers are included in a crawl, but these custom classifiers always override the default classifiers for a given classification\.
+ `TablePrefix` – UTF\-8 string, not more than 128 bytes long\.

  The table prefix used for catalog tables that are created\.
+ `SchemaChangePolicy` – A [SchemaChangePolicy](#aws-glue-api-crawler-crawling-SchemaChangePolicy) object\.

  Policy for the crawler's update and deletion behavior\.
+ `Configuration` – UTF\-8 string\.

  Crawler configuration information\. This versioned JSON string allows users to specify aspects of a crawler's behavior\. For more information, see [Configuring a Crawler](http://docs.aws.amazon.com/glue/latest/dg/crawler-configuration.html)\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `InvalidInputException`
+ `VersionMismatchException`
+ `EntityNotFoundException`
+ `CrawlerRunningException`
+ `OperationTimeoutException`

## StartCrawler Action \(Python: start\_crawler\)<a name="aws-glue-api-crawler-crawling-StartCrawler"></a>

Starts a crawl using the specified crawler, regardless of what is scheduled\. If the crawler is already running, returns a [CrawlerRunningException](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-api-exceptions.html#aws-glue-api-exceptions-CrawlerRunningException)\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the crawler to start\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `CrawlerRunningException`
+ `OperationTimeoutException`

## StopCrawler Action \(Python: stop\_crawler\)<a name="aws-glue-api-crawler-crawling-StopCrawler"></a>

If the specified crawler is running, stops the crawl\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the crawler to stop\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `CrawlerNotRunningException`
+ `CrawlerStoppingException`
+ `OperationTimeoutException`