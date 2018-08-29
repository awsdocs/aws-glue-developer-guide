# Job Monitoring and Debugging<a name="monitor-profile-glue-job-cloudwatch-metrics"></a>

You can collect metrics about AWS Glue jobs and visualize them on the AWS Glue and Amazon CloudWatch consoles to identify and fix issues\. Profiling your AWS Glue jobs requires the following steps:

1. Enable the **Job metrics** option in the job definition\. You can enable profiling in the AWS Glue console or as a parameter to the job\. For more information see [Defining Job Properties](add-job.md#create-job) or [Special Parameters Used by AWS Glue](aws-glue-programming-etl-glue-arguments.md)\. 

1. Confirm that the job script initializes a `GlueContext`\. For example, the following script snippet initializes a `GlueContext` and shows where profiled code is placed in the script\. This general format is used in the debugging scenarios that follow\.

   ```
   import sys
   from awsglue.transforms import *
   from awsglue.utils import getResolvedOptions
   from pyspark.context import SparkContext
   from awsglue.context import GlueContext
   from awsglue.job import Job
   import time
   
   ## @params: [JOB_NAME]
   args = getResolvedOptions(sys.argv, ['JOB_NAME'])
   
   sc = SparkContext()
   glueContext = GlueContext(sc)
   spark = glueContext.spark_session
   job = Job(glueContext)
   job.init(args['JOB_NAME'], args)
   
   ...
   ...
   code-to-profile
   ...
   ...
   
   
   job.commit()
   ```

1. Run the job\.

1. Visualize the metrics on the AWS Glue console and identify abnormal metrics for the `driver` or an `executor`\.

1. Narrow down the root cause using the identified metric\.

1. Optionally, confirm the root cause using the log stream of the identified driver or job executor\.

**Topics**
+ [Debugging OOM Exceptions and Job Abnormalities](monitor-profile-debug-oom-abnormalities.md)
+ [Debugging Demanding Stages and Straggler Tasks](monitor-profile-debug-straggler.md)
+ [Monitoring the Progress of Multiple Jobs](monitor-debug-multiple.md)
+ [Monitoring for DPU Capacity Planning](monitor-debug-capacity.md)