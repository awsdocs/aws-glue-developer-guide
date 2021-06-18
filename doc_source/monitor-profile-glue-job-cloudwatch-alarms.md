# Setting Up Amazon CloudWatch Alarms on AWS Glue Job Profiles<a name="monitor-profile-glue-job-cloudwatch-alarms"></a>

AWS Glue metrics are also available in Amazon CloudWatch\. You can set up alarms on any AWS Glue metric for scheduled jobs\. 

A few common scenarios for setting up alarms are as follows:
+ Jobs running out of memory \(OOM\): Set an alarm when the memory usage exceeds the normal average for either the driver or an executor for an AWS Glue job\.
+ Straggling executors: Set an alarm when the number of executors falls below a certain threshold for a large duration of time in an AWS Glue job\.
+ Data backlog or reprocessing: Compare the metrics from individual jobs in a workflow using a CloudWatch math expression\. You can then trigger an alarm on the resulting expression value \(such as the ratio of bytes written by a job and bytes read by a following job\)\.

For detailed instructions on setting alarms, see [Create or Edit a CloudWatch Alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ConsoleAlarms.html) in the *[Amazon CloudWatch Events User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/)*\. 

For monitoring and debugging scenarios using CloudWatch, see [Job Monitoring and Debugging](monitor-profile-glue-job-cloudwatch-metrics.md)\.