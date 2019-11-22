# Enabling Continuous Logging for AWS Glue Jobs<a name="monitor-continuous-logging-enable"></a>

You can enable continuous logging using the AWS Glue console or through the AWS Command Line Interface \(AWS CLI\)\. 

You can enable continuous logging with either a standard filter or no filter when you create a new job, edit an existing job, or enable it through the AWS CLI\. Choosing the **Standard filter** prunes out non\-useful Apache Spark driver/executor and Apache Hadoop YARN heartbeat log messages\. Choosing **No filter** gives you all the log messages\.

You can also specify custom configuration options such as the AWS CloudWatch log group name, CloudWatch log stream prefix before the AWS Glue job run ID driver/executor ID, and log conversion pattern for log messages\. These configurations help you to set aggregate logs in custom CloudWatch log groups with different expiration policies, and analyze them further with custom log stream prefixes and conversions patterns\. 

**Topics**
+ [Using the AWS Management Console](#monitor-continuous-logging-enable-console)
+ [Logging Application\-Specific Messages Using the Custom Script Logger](#monitor-continuous-logging-script)
+ [Enabling the Progress Bar to Show Job Progress](#monitor-continuous-logging-progress)

## Using the AWS Management Console<a name="monitor-continuous-logging-enable-console"></a>

Follow these steps to use the console to enable continuous logging when creating or editing an AWS Glue job\.

**To create a new AWS Glue job with continuous logging**

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, choose **Jobs**\.

1. Choose **Add job**\.

1. In **Configure the job properties**, choose **Monitoring options**\.

1. In the **Continuous logging** tab, choose **Enable**\.

1. Choose **Standard filter** or **No filter**\.

**To enable continuous logging for an existing AWS Glue job**

1. Open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, choose **Jobs**\.

1. Choose an existing job from the **Jobs** list\.

1. Choose **Action**, **Edit job**\.

1. Choose **Monitoring options**\.

1. In the **Continuous logging** tab, choose **Enable**\.

1. Choose **Standard filter** or **No filter**\.

**To enable continuous logging for all newly created AWS Glue jobs**

1. Open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, choose **Jobs**\.

1. In the upper\-right corner, choose **User preferences**\.

1. Choose **Monitoring options**\.

1. In the **Continuous logging** tab, choose **Enable**\.

1. Choose **Standard filter** or **No filter**\.

These user preferences are applied to all new jobs unless you override them explicitly when creating an AWS Glue job or by editing an existing job as described previously\.

### Using the AWS CLI<a name="monitor-continuous-logging-cli"></a>

To enable continuous logging, you pass in job parameters to an AWS Glue job\. When you want to use the standard filter, pass the following special job parameters similar to other AWS Glue job parameters\. For more information, see [Special Parameters Used by AWS Glue](aws-glue-programming-etl-glue-arguments.md)\.

```
'--enable-continuous-cloudwatch-log': 'true'
```

When you want no filter, use the following\.

```
'--enable-continuous-cloudwatch-log': 'true',
'--enable-continuous-log-filter': 'false'
```

You can specify a custom AWS CloudWatch log group name\. If not specified, the default log group name is `/aws-glue/jobs/logs-v2/`\.

```
'--continuous-log-logGroup': 'custom_log_group_name'
```

You can specify a custom AWS CloudWatch log stream prefix\. If not specified, the default log stream prefix is the job run ID\.

```
'--continuous-log-logStreamPrefix': 'custom_log_stream_prefix'
```

You can specify a custom continuous logging conversion pattern\. If not specified, the default conversion pattern is `%d{yy/MM/dd HH:mm:ss} %p %c{1}: %m%n`\. Note that the conversion pattern only applies to driver logs and executor logs\. It does not affect the Glue progress bar\.

```
'--continuous-log-conversionPattern': 'custom_log_conversion_pattern'
```

## Logging Application\-Specific Messages Using the Custom Script Logger<a name="monitor-continuous-logging-script"></a>

You can use the AWS Glue logger to log any application\-specific messages in the script that are sent in real time to the driver log stream\.

The following example shows a Python script\.

```
from awsglue.context import GlueContext
from pyspark.context import SparkContext

sc = SparkContext()
glueContext = GlueContext(sc)
logger = glueContext.get_logger()
logger.info("info message")
logger.warn("warn message")
logger.error("error message")
```

The following example shows a Scala script\.

```
import com.amazonaws.services.glue.log.GlueLogger

object GlueApp {
  def main(sysArgs: Array[String]) {
    val logger = new GlueLogger
    logger.info("info message")
    logger.warn("warn message")
    logger.error("error message")
  }
}
```

## Enabling the Progress Bar to Show Job Progress<a name="monitor-continuous-logging-progress"></a>

AWS Glue provides a real\-time progress bar under the `JOB_RUN_ID-progress-bar` log stream to check AWS Glue job run status\. Currently it supports only jobs that initialize `glueContext`\. If you run a pure Spark job without initializing `glueContext`, the AWS Glue progress bar does not appear\.

The progress bar shows the following progress update every 5 seconds\.

```
Stage Number (Stage Name): > (numCompletedTasks + numActiveTasks) / totalNumOfTasksInThisStage]
```