# Running and Monitoring AWS Glue<a name="monitor-glue"></a>

You can automate the running of your ETL \(extract, transform, and load\) jobs\. AWS Glue also provides metrics for crawlers and jobs that you can monitor\. After you set up the AWS Glue Data Catalog with the required metadata, AWS Glue provides statistics about the health of your environment\. You can automate the invocation of crawlers and jobs with a time\-based schedule based on cron\. You can also trigger jobs when an event\-based trigger fires\.

The main objective of AWS Glue is to provide an easier way to extract and transform your data from source to target\. To accomplish this objective, an ETL job follows these typical steps \(as shown in the diagram that follows\):

1. A trigger fires to initiate a job run\. This event can be set up on a recurring schedule or to satisfy a dependency\.

1. The job extracts data from your source\. If required, connection properties are used to access your source\.

1. The job transforms your data using a script that you created and the values of any arguments\. The script contains the Scala or PySpark Python code that transforms your data\.

1. The transformed data is loaded to your data targets\. If required, connection properties are used to access the target\.

1. Statistics are collected about the job run and are written to your Data Catalog\.

The following diagram shows the ETL workflow containing these five steps\.

![\[Dataflow showing extract, transform, and load in AWS Glue in 5 basic steps.\]](http://docs.aws.amazon.com/glue/latest/dg/images/MonitorJobRun-overview.png)

**Topics**
+ [Automated Monitoring Tools](#monitoring-automated_tools)
+ [Time\-Based Schedules for Jobs and Crawlers](monitor-data-warehouse-schedule.md)
+ [Tracking Processed Data Using Job Bookmarks](monitor-continuations.md)
+ [AWS Tags in AWS Glue](monitor-tags.md)
+ [Automating AWS Glue with CloudWatch Events](automating-awsglue-with-cloudwatch-events.md)
+ [Monitoring Jobs Using the Apache Spark Web UI](monitor-spark-ui.md)
+ [Monitoring with Amazon CloudWatch](monitor-cloudwatch.md)
+ [Job Monitoring and Debugging](monitor-profile-glue-job-cloudwatch-metrics.md)
+ [Logging AWS Glue API Calls with AWS CloudTrail](monitor-cloudtrail.md)

## Automated Monitoring Tools<a name="monitoring-automated_tools"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of AWS Glue and your other AWS solutions\. AWS provides monitoring tools that you can use to watch AWS Glue, report when something is wrong, and take action automatically when appropriate:

You can use the following automated monitoring tools to watch AWS Glue and report when something is wrong:
+ **Amazon CloudWatch Events** delivers a near real\-time stream of system events that describe changes in AWS resources\. CloudWatch Events enables automated event\-driven computing\. You can write rules that watch for certain events and trigger automated actions in other AWS services when these events occur\. For more information, see the [Amazon CloudWatch Events User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/)\.
+ **Amazon CloudWatch Logs** enables you to monitor, store, and access your log files from Amazon EC2 instances, AWS CloudTrail, and other sources\. CloudWatch Logs can monitor information in the log files and notify you when certain thresholds are met\. You can also archive your log data in highly durable storage\. For more information, see the [Amazon CloudWatch Logs User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)\.
+ **AWS CloudTrail** captures API calls and related events made by or on behalf of your AWS account and delivers the log files to an Amazon S3 bucket that you specify\. You can identify which users and accounts call AWS, the source IP address from which the calls are made, and when the calls occur\. For more information, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.