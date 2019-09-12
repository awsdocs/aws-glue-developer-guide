# Continuous Logging for AWS Glue Jobs<a name="monitor-continuous-logging"></a>

AWS Glue provides real\-time, continuous logging for AWS Glue jobs\. You can view real\-time Apache Spark job logs in Amazon CloudWatch, including driver logs, executor logs, and an Apache Spark job progress bar\. Viewing real\-time logs provides you with a better perspective on the running job\.

When you start an AWS Glue job, it sends the real\-time logging information to CloudWatch \(every 5 seconds and before each executor termination\) after the Spark application starts running\. You can view the logs on the AWS Glue console or the CloudWatch console dashboard\.

The continuous logging feature includes the following capabilities: 
+ Continuous logging with a default filter to reduce high verbosity in the logs
+ Continuous logging with no filter
+ A custom script logger to log application\-specific messages
+ A console progress bar to track the running status of the current AWS Glue job

**Topics**
+ [Enabling Continuous Logging for AWS Glue Jobs](monitor-continuous-logging-enable.md)
+ [Viewing Continuous Logging for AWS Glue Jobs](monitor-continuous-logging-view.md)