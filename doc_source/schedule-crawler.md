# Scheduling an AWS Glue Crawler<a name="schedule-crawler"></a>

You can run an AWS Glue crawler on demand or on a regular schedule\. Crawler schedules can be expressed in *cron* format\. For more information, see [cron](http://en.wikipedia.org/wiki/Cron) in Wikipedia\.

When you create a crawler based on a schedule, you can specify certain constraints, such as the frequency the crawler runs, which days of the week it runs, and at what time\. These constraints are based on cron\. When setting up a crawler schedule, you should consider the features and limitations of cron\. For example, if you choose to run your crawler on day 31 each month, keep in mind that some months don't have 31 days\.

For more information about using cron to schedule jobs and crawlers, see [Time\-Based Schedules for Jobs and Crawlers](monitor-data-warehouse-schedule.md)\.  