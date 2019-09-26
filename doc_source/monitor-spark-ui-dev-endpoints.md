# Enabling the Apache Spark Web UI for Development Endpoints<a name="monitor-spark-ui-dev-endpoints"></a>

Use the Apache Spark web UI to monitor and debug Spark applications running on AWS Glue development endpoints\. You can configure the development endpoints with the Spark Web UI using the AWS Glue console or the AWS Command Line Interface \(AWS CLI\)\.

**Topics**
+ [Enabling the Spark UI \(Console\)](#monitor-spark-ui-dev-endpoints-console)
+ [Enabling the Spark UI \(AWS CLI\)](#monitor-spark-ui-jobs-cli)

## Enabling the Spark UI \(Console\)<a name="monitor-spark-ui-dev-endpoints-console"></a>

Follow these steps to configure the Spark UI using the AWS Management Console\.

**To create a development endpoint with the Spark UI enabled**

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, choose **Dev endpoints**\.

1. Choose **Add endpoint**\.

1. In **Configuration**, open the **Spark UI options**\.

1. In the **Spark UI** tab, choose **Enable**\.

1. Specify an Amazon S3 path for storing the Spark event logs\.

## Enabling the Spark UI \(AWS CLI\)<a name="monitor-spark-ui-jobs-cli"></a>

To create a development endpoint with the Spark UI enabled using the AWS CLI, pass in the following arguments in JSON syntax\.

```
{
    "EndpointName": "Name",
    "RoleArn": "role_ARN",
    "PublicKey": "public_key_contents",
    "NumberOfNodes": 2,
    "Arguments": {
        "--enable-spark-ui": "true",
        "--spark-event-logs-path": "s3://s3-event-log-path"
    }
}
```