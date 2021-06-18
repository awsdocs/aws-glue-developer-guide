# Crawler Scheduler API<a name="aws-glue-api-crawler-scheduler"></a>

The Crawler Scheduler API describes AWS Glue crawler data types, along with the API for creating, deleting, updating, and listing crawlers\.

## Data Types<a name="aws-glue-api-crawler-scheduler-objects"></a>
+ [Schedule Structure](#aws-glue-api-crawler-scheduler-Schedule)

## Schedule Structure<a name="aws-glue-api-crawler-scheduler-Schedule"></a>

A scheduling object using a `cron` statement to schedule an event\.

**Fields**
+ `ScheduleExpression` – UTF\-8 string\.

  A `cron` expression used to specify the schedule \(see [Time\-Based Schedules for Jobs and Crawlers](https://docs.aws.amazon.com/glue/latest/dg/monitor-data-warehouse-schedule.html)\. For example, to run something every day at 12:15 UTC, you would specify: `cron(15 12 * * ? *)`\.
+ `State` – UTF\-8 string \(valid values: `SCHEDULED` \| `NOT_SCHEDULED` \| `TRANSITIONING`\)\.

  The state of the schedule\.

## Operations<a name="aws-glue-api-crawler-scheduler-actions"></a>
+ [UpdateCrawlerSchedule Action \(Python: update\_crawler\_schedule\)](#aws-glue-api-crawler-scheduler-UpdateCrawlerSchedule)
+ [StartCrawlerSchedule Action \(Python: start\_crawler\_schedule\)](#aws-glue-api-crawler-scheduler-StartCrawlerSchedule)
+ [StopCrawlerSchedule Action \(Python: stop\_crawler\_schedule\)](#aws-glue-api-crawler-scheduler-StopCrawlerSchedule)

## UpdateCrawlerSchedule Action \(Python: update\_crawler\_schedule\)<a name="aws-glue-api-crawler-scheduler-UpdateCrawlerSchedule"></a>

Updates the schedule of a crawler using a `cron` expression\. 

**Request**
+ `CrawlerName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the crawler whose schedule to update\.
+ `Schedule` – UTF\-8 string\.

  The updated `cron` expression used to specify the schedule \(see [Time\-Based Schedules for Jobs and Crawlers](https://docs.aws.amazon.com/glue/latest/dg/monitor-data-warehouse-schedule.html)\. For example, to run something every day at 12:15 UTC, you would specify: `cron(15 12 * * ? *)`\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `VersionMismatchException`
+ `SchedulerTransitioningException`
+ `OperationTimeoutException`

## StartCrawlerSchedule Action \(Python: start\_crawler\_schedule\)<a name="aws-glue-api-crawler-scheduler-StartCrawlerSchedule"></a>

Changes the schedule state of the specified crawler to `SCHEDULED`, unless the crawler is already running or the schedule state is already `SCHEDULED`\.

**Request**
+ `CrawlerName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the crawler to schedule\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `SchedulerRunningException`
+ `SchedulerTransitioningException`
+ `NoScheduleException`
+ `OperationTimeoutException`

## StopCrawlerSchedule Action \(Python: stop\_crawler\_schedule\)<a name="aws-glue-api-crawler-scheduler-StopCrawlerSchedule"></a>

Sets the schedule state of the specified crawler to `NOT_SCHEDULED`, but does not stop the crawler if it is already running\.

**Request**
+ `CrawlerName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the crawler whose schedule state to set\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `SchedulerNotRunningException`
+ `SchedulerTransitioningException`
+ `OperationTimeoutException`