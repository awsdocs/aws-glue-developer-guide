# Monitoring with Amazon CloudWatch<a name="monitor-cloudwatch"></a>

You can monitor AWS Glue using Amazon CloudWatch, which collects and processes raw data from AWS Glue into readable, near\-real\-time metrics\. These statistics are recorded for a period of two weeks so that you can access historical information for a better perspective on how your web application or service is performing\. By default, AWS Glue metrics data is sent to CloudWatch automatically\. For more information, see [What Is Amazon CloudWatch?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatch.html) in the *Amazon CloudWatch User Guide*, and [AWS Glue Metrics](monitoring-awsglue-with-cloudwatch-metrics.md#awsglue-metrics)\.

AWS Glue also supports real\-time continuous logging for AWS Glue jobs\. When continuous logging is enabled for a job, you can view the real\-time logs on the AWS Glue console or the CloudWatch console dashboard\. For more information, see [Continuous Logging for AWS Glue Jobs](monitor-continuous-logging.md)\.

**Topics**
+ [Monitoring AWS Glue Using Amazon CloudWatch Metrics](monitoring-awsglue-with-cloudwatch-metrics.md)
+ [Setting Up Amazon CloudWatch Alarms on AWS Glue Job Profiles](monitor-profile-glue-job-cloudwatch-alarms.md)
+ [Continuous Logging for AWS Glue Jobs](monitor-continuous-logging.md)