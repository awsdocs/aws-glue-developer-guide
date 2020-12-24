# Running Spark ETL Jobs with Reduced Startup Times<a name="reduced-start-times-spark-etl-jobs"></a>

AWS Glue version 2\.0 provides an upgraded infrastructure for running Apache Spark ETL \(extract, transform, and load\) jobs in AWS Glue with reduced startup times\. With the reduced wait times, data engineers can be more productive and increase their interactivity with AWS Glue\. The reduced variance in job start times can help you meet or exceed your SLAs of making data available for analytics\.

To use this feature with your AWS Glue ETL jobs, choose **2\.0** for the `Glue version` when creating your jobs\.

**Topics**
+ [New Features Supported](#reduced-start-times-new-features)
+ [Logging Behavior](#reduced-start-times-logging)
+ [Features Not Supported](#reduced-start-times-limitations)

## New Features Supported<a name="reduced-start-times-new-features"></a>

This section describes new features supported with AWS Glue version 2\.0\.

### Support for Specifying Additional Python Modules at the Job Level<a name="reduced-start-times-python-modules-specify"></a>

AWS Glue version 2\.0 also lets you provide additional Python modules or different versions at the job level\. You can use the `--additional-python-modules` option with a list of comma\-separated Python modules to add a new module or change the version of an existing module\.

For example to update or to add a new `scikit-learn` module use the following key/value: `"--additional-python-modules", "scikit-learn==0.21.3"`\.

Also, within the `--additional-python-modules` option you can specify an Amazon S3 path to a Python wheel module\. For example:

```
--additional-python-modules s3://aws-glue-native-spark/tests/j4.2/ephem-3.7.7.1-cp37-cp37m-linux_x86_64.whl,s3://aws-glue-native-spark/tests/j4.2/fbprophet-0.6-py3-none-any.whl,scikit-learn==0.21.3
```

AWS Glue uses the Python Package Installer \(pip3\) to install the additional modules\. You can pass additional options specified by the `python-modules-installer-option` to pip3 for installing the modules\. Any incompatibly or limitations from pip3 will apply\.

### Python Modules Already Provided in AWS Glue Version 2\.0<a name="reduced-start-times-python-modules"></a>

AWS Glue version 2\.0 supports the following python modules out of the box:
+ setuptools—45\.2\.0
+ setuptools—45\.2\.0 
+ subprocess32—3\.5\.4
+ ptvsd—4\.3\.2
+ pydevd—1\.9\.0
+ PyMySQL—0\.9\.3
+ docutils—0\.15\.2
+ jmespath—0\.9\.4
+ six—1\.14\.0
+ python\_dateutil—2\.8\.1
+ urllib3—1\.25\.8
+ botocore—1\.15\.4
+ s3transfer—0\.3\.3
+ boto3—1\.12\.4
+ certifi—2019\.11\.28
+ chardet—3\.0\.4
+ idna—2\.9
+ requests—2\.23\.0
+ pyparsing—2\.4\.6
+ enum34—1\.1\.9
+ pytz—2019\.3
+ numpy—1\.18\.1
+ cycler—0\.10\.0
+ kiwisolver—1\.1\.0
+ scipy—1\.4\.1
+ pandas—1\.0\.1
+ pyarrow—0\.16\.0
+ matplotlib—3\.1\.3
+ pyhocon—0\.3\.54
+ mpmath—1\.1\.0
+ sympy—1\.5\.1
+ patsy—0\.5\.1
+ statsmodels—0\.11\.1
+ fsspec—0\.6\.2
+ s3fs—0\.4\.0
+ Cython—0\.29\.15
+ joblib—0\.14\.1
+ pmdarima—1\.5\.3
+ scikit\-learn—0\.22\.1
+ tbats—1\.0\.9

## Logging Behavior<a name="reduced-start-times-logging"></a>

AWS Glue version 2\.0 supports different default logging behavior\. The differences include:
+ Logging occurs in realtime\.
+ There are separate streams for drivers and executors\.
+ For each driver and executor there are two streams, the output stream and the error stream\.

### Driver and Executor Streams<a name="reduced-start-times-logging-driver-executor"></a>

Driver streams are identified by the job run ID\. Executor streams are identified by the job <*run id*>\_<*executor task id*>\. For example: 
+ `"logStreamName": "jr_8255308b426fff1b4e09e00e0bd5612b1b4ec848d7884cebe61ed33a31789..._g-f65f617bd31d54bd94482af755b6cdf464542..."`

### Output and Errors Streams<a name="reduced-start-times-logging-output-error"></a>

The output stream has the standard output \(stdout\) from your code\. The error stream has logging messages from the your code/library\.
+ Log streams:
  + Driver log streams have <*jr*>, where <*jr*> is the job run ID\.
  + Executor log streams have <*jr*>\_<*g*>, where <*g*> is the task ID for the executor\. You can look up the executor task ID in the driver error log\.

The default log groups for AWS Glue version 2\.0 are as follows:
+ `/aws-glue/jobs/logs/output` for output
+ `/aws-glue/jobs/logs/error` for errors

When a security configuration is provided, the log group names change to:
+ `/aws-glue/jobs/<security configuration>-role/<Role Name>/output`
+ `/aws-glue/jobs/<security configuration>-role/<Role Name>/error`

On the console the **Logs** link points to the output log group and the **Error** link points to the error log group\. When continuous logging is enabled, the **Logs** links points to the continuous log group, and the **Output** link points to the output log group\.

### Logging Rules<a name="reduced-start-times-logging-rules"></a>

**Note**  
In AWS Glue version 2\.0, an issue with continuous logging requires that you create the log group before using the continuous logging feature\. To work around this issue, you must create the default logging groups or customer log groups specified by `--continuous-log-logGroup`, otherwise the continuous log streams will not show up\. AWS is working to address this issue\.  
The default log groupname for continuous logging is `/aws-glue/jobs/logs-v2`\.

In AWS Glue version 2\.0, continuous logging has the same behavior as in AWS Glue version 1\.0:
+ Default log group: `/aws-glue/jobs/logs-v2` 
+ Driver log stream: <*jr*>\-driver
+ Executor log stream: <*jr*>\-<*executor ID*>

  The log group name can be changed by setting `--continuous-log-logGroupName`

  The log streams name can be prefixed by setting `--continous-log-logStreamPrefix`

## Features Not Supported<a name="reduced-start-times-limitations"></a>

The following AWS Glue features are not supported:
+ Development endpoints
+ FindMatches machine learning transforms
+ AWS Glue version 2\.0 does not run on Apache YARN, so YARN settings do not apply
+ AWS Glue version 2\.0 does not have a Hadoop Distributed File System \(HDFS\)
+ AWS Glue version 2\.0 does not use dynamic allocation, hence the ExecutorAllocationManager metrics are not available
+ For AWS Glue version 2\.0 jobs, you specify the number of workers and worker type, but do not specify a `maxCapacity`\.