# Enabling the Apache Spark Web UI for AWS Glue Jobs<a name="monitor-spark-ui-jobs"></a>

You can use the Apache Spark web UI to monitor and debug AWS Glue ETL jobs running on the AWS Glue job system\. You can configure the Spark UI using the AWS Glue console or the AWS Command Line Interface \(AWS CLI\)\.

**Topics**
+ [Configuring the Spark UI \(Console\)](#monitor-spark-ui-jobs-console)
+ [Configuring the Spark UI \(AWS CLI\)](#monitor-spark-ui-jobs-cli)

## Configuring the Spark UI \(Console\)<a name="monitor-spark-ui-jobs-console"></a>

Follow these steps to configure the Spark UI using the AWS Management Console\.

**To create a job with the Spark UI enabled**

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, choose **Jobs**\.

1. Choose **Add job**\.

1. In **Configure the job properties**, open the **Monitoring options**\.

1. In the **Spark UI** tab, choose **Enable**\.

1. Specify an Amazon S3 path for storing the Spark event logs for the job\.

**To edit an existing job to enable the Spark UI**

1. Open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, choose **Jobs**\.

1. Choose an existing job in the job list\.

1. Choose **Action**, and then choose **Edit job**\.

1. Open the **Monitoring options**\.

1. In the **Spark UI** tab, choose **Enable**\.

1. Enter an Amazon S3 path for storing the Spark event logs for the job\.

**To set up user preferences for new jobs to enable the Spark UI**

1. Open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the upper\-right corner, choose **User preferences**\.

1. Open the **Monitoring options**\.

1. In the **Spark UI** tab, choose **Enable**\.

1. Specify an Amazon S3 path for storing the Spark event logs for the job\.

**To set up the job run options to enable the Spark UI**

1. Open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, choose **Jobs**\.

1. Choose an existing job in the job lists\.

1. Choose **Scripts** and **Edit Job**\. You navigate to the code pane\.

1. Choose **Run job**\.

1. Open the **Monitoring options**\.

1. In the **Spark UI** tab, choose **Enable**\.

1. Specify an Amazon S3 path for storing the Spark event logs for the job\.

## Configuring the Spark UI \(AWS CLI\)<a name="monitor-spark-ui-jobs-cli"></a>

To enable the Spark UI feature using the AWS CLI, pass in the following job parameters to AWS Glue jobs\. For more information, see [ Special Parameters Used by AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-glue-arguments.html)\.

```
'--enable-spark-ui': 'true',
'--spark-event-logs-path': 's3://s3-event-log-path'
```

Every 30 seconds, AWS Glue flushes the Spark event logs to the Amazon S3 path that you specify\.