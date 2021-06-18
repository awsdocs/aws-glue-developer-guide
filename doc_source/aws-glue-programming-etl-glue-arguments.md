# Special Parameters Used by AWS Glue<a name="aws-glue-programming-etl-glue-arguments"></a>

AWS Glue recognizes several argument names that you can use to set up the script environment for your jobs and job runs:
+ `--job-language` — The script programming language\. This value must be either `scala` or `python`\. If this parameter is not present, the default is `python`\.
+ `--class` — The Scala class that serves as the entry point for your Scala script\. This applies only if your `--job-language` is set to `scala`\.
+ `--scriptLocation` — The Amazon Simple Storage Service \(Amazon S3\) location where your ETL script is located \(in the form `s3://path/to/my/script.py`\)\. This parameter overrides a script location set in the `JobCommand` object\.
+ `--extra-py-files` — The Amazon S3 paths to additional Python modules that AWS Glue adds to the Python path before executing your script\. Multiple values must be complete paths separated by a comma \(`,`\)\. Only individual files are supported, not a directory path\. Currently, only pure Python modules work\. Extension modules written in C or other languages are not supported\.
+ `--extra-jars` — The Amazon S3 paths to additional Java `.jar` files that AWS Glue adds to the Java classpath before executing your script\. Multiple values must be complete paths separated by a comma \(`,`\)\.
+ `--user-jars-first` — When setting this value to `true`, it prioritizes the customer's extra JAR files in the classpath\. This option is only available in AWS Glue version 2\.0\.
+ `--use-postgres-driver` — When setting this value to `true`, it prioritizes the Postgres JDBC driver in the class path to avoid a conflict with the Amazon Redshift JDBC driver\. This option is only available in AWS Glue version 2\.0\.
+ `--extra-files` — The Amazon S3 paths to additional files, such as configuration files that AWS Glue copies to the working directory of your script before executing it\. Multiple values must be complete paths separated by a comma \(`,`\)\. Only individual files are supported, not a directory path\.
+ `--job-bookmark-option` — Controls the behavior of a job bookmark\. The following option values can be set\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-glue-arguments.html)

  For example, to enable a job bookmark, pass the following argument\.

  ```
  '--job-bookmark-option': 'job-bookmark-enable'
  ```
+ `--TempDir` — Specifies an Amazon S3 path to a bucket that can be used as a temporary directory for the job\.

  For example, to set a temporary directory, pass the following argument\.

  ```
  '--TempDir': 's3-path-to-directory'
  ```
+ `--enable-s3-parquet-optimized-committer` — Enables the EMRFS S3\-optimized committer for writing Parquet data into Amazon S3\. You can supply the parameter/value pair via the AWS Glue console when creating or updating an AWS Glue job\. Setting the value to **true** enables the committer\. By default the flag is turned off\.

  For more information, see [Using the EMRFS S3\-optimized Committer](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-spark-s3-optimized-committer.html)\.
+ `--enable-rename-algorithm-v2` — Sets the EMRFS rename algorithm version to version 2\. When a Spark job uses dynamic partition overwrite mode, there is a possibility that a duplicate partition is created\. For instance, you can end up with a duplicate partition such as `s3://bucket/table/location/p1=1/p1=1`\. Here, P1 is the partition that is being overwritten\. Rename algorithm version 2 fixes this issue\.

  This option is only available on AWS Glue version 1\.0\.
+ `--enable-glue-datacatalog` — Enables you to use the AWS Glue Data Catalog as an Apache Spark Hive metastore\. To enable this feature, only specify the key; no value is needed\.
+ `--enable-metrics` — Enables the collection of metrics for job profiling for this job run\. These metrics are available on the AWS Glue console and the Amazon CloudWatch console\. To enable metrics, only specify the key; no value is needed\.
+ `--enable-continuous-cloudwatch-log` — Enables real\-time continuous logging for AWS Glue jobs\. You can view real\-time Apache Spark job logs in CloudWatch\.
+ `--enable-continuous-log-filter` — Specifies a standard filter \(`true`\) or no filter \(`false`\) when you create or edit a job enabled for continuous logging\. Choosing the standard filter prunes out non\-useful Apache Spark driver/executor and Apache Hadoop YARN heartbeat log messages\. Choosing no filter gives you all the log messages\.
+ `--continuous-log-logGroup` — Specifies a custom Amazon CloudWatch log group name for a job enabled for continuous logging\.
+ `--continuous-log-logStreamPrefix` — Specifies a custom CloudWatch log stream prefix for a job enabled for continuous logging\.
+ `--continuous-log-conversionPattern` — Specifies a custom conversion log pattern for a job enabled for continuous logging\. The conversion pattern applies only to driver logs and executor logs\. It does not affect the AWS Glue progress bar\.
+ `--enable-spark-ui` — When set to `true`, turns on the feature to use the Spark UI to monitor and debug AWS Glue ETL jobs\.
+ `--spark-event-logs-path` — Specifies an Amazon S3 path\. When using the Spark UI monitoring feature, AWS Glue flushes the Spark event logs to this Amazon S3 path every 30 seconds to a bucket that can be used as a temporary directory for storing Spark UI events\.

For example, the following is the syntax for running a job using `--arguments` to set a special parameter\.

```
$ aws glue start-job-run --job-name "CSV to CSV" --arguments='--scriptLocation="s3://my_glue/libraries/test_lib.py"'
```

AWS Glue uses the following arguments internally and you should never use them:
+ `--conf` — Internal to AWS Glue\. Do not set\.
+ `--debug` — Internal to AWS Glue\. Do not set\.
+ `--mode` — Internal to AWS Glue\. Do not set\.
+ `--JOB_NAME` — Internal to AWS Glue\. Do not set\.