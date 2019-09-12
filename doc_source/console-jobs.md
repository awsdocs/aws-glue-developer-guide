# Working with Jobs on the AWS Glue Console<a name="console-jobs"></a>

A job in AWS Glue consists of the business logic that performs extract, transform, and load \(ETL\) work\. You can create jobs in the **ETL** section of the AWS Glue console\. 

To view existing jobs, sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\. Then choose the **Jobs** tab in AWS Glue\. The **Jobs** list displays the location of the script that is associated with each job, when the job was last modified, and the current job bookmark option\. 

From the **Jobs** list, you can do the following:
+ To start an existing job, choose **Action**, and then choose **Run job**\.
+ To stop a `Running` or `Starting` job, choose **Action**, and then choose **Stop job run**\.
+ To add triggers that start a job, choose **Action**, **Choose job triggers**\.
+ To modify an existing job, choose **Action**, and then choose **Edit job** or **Delete**\.
+ To change a script that is associated with a job, choose **Action**, **Edit script**\.
+ To reset the state information that AWS Glue stores about your job, choose **Action**, **Reset job bookmark**\.
+ To create a development endpoint with the properties of this job, choose **Action**, **Create development endpoint**\.

**To add a new job using the console**

1. Open the AWS Glue console, and choose the **Jobs** tab\.

1. Choose **Add job**, and follow the instructions in the **Add job** wizard\.

   If you decide to have AWS Glue generate a script for your job, you must specify the job properties, data sources, and data targets, and verify the schema mapping of source columns to target columns\. The generated script is a starting point for you to add code to perform your ETL work\. Verify the code in the script and modify it to meet your business needs\.
**Note**  
To get step\-by\-step guidance for adding a job with a generated script, see the **Add job** tutorial in the console\.

   Optionally, you can add a security configuration to a job to specify at\-rest encryption options\.     
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/console-jobs.html)

**Note**  
The job assumes the permissions of the **IAM role** that you specify when you create it\. This IAM role must have permission to extract data from your data store and write to your target\. The AWS Glue console only lists IAM roles that have attached a trust policy for the AWS Glue principal service\. For more information about providing roles for AWS Glue, see [Identity\-Based Policies](using-identity-based-policies.md)\.  
If the job reads AWS KMS encrypted Amazon Simple Storage Service \(Amazon S3\) data, then the **IAM role** must have decrypt permission on the KMS key\. For more information, see [Step 2: Create an IAM Role for AWS Glue](create-an-iam-role.md)\.

**Important**  
Check [Troubleshooting Errors in AWS Glue](glue-troubleshooting-errors.md) for known problems when a job runs\.

To learn about the properties that are required for each job, see [Defining Job Properties](add-job.md#create-job)\.

To get step\-by\-step guidance for adding a job with a generated script, see the **Add job** tutorial in the AWS Glue console\.

## Viewing Job Details<a name="console-jobs-details"></a>

To see details of a job, select the job in the **Jobs** list and review the information on the following tabs:
+ History
+ Details
+ Script
+ Metrics

### History<a name="console-jobs-details-history"></a>

The **History** tab shows your job run history and how successful a job has been in the past\. For each job, the run metrics include the following:
+ **Run ID** is an identifier created by AWS Glue for each run of this job\.
+ **Retry attempt** shows the number of attempts for jobs that required AWS Glue to automatically retry\.
+ **Run status** shows the success of each run listed with the most recent run at the top\. If a job is `Running` or `Starting`, you can choose the action icon in this column to stop it\.
+ **Error** shows the details of an error message if the run was not successful\.
+ **Logs** links to the logs written to `stdout` for this job run\.

  The **Logs** link takes you to Amazon CloudWatch Logs, where you can see all the details about the tables that were created in the AWS Glue Data Catalog and any errors that were encountered\. You can manage your log retention period in the CloudWatch console\. The default log retention is `Never Expire`\. For more information about how to change the retention period, see [Change Log Data Retention in CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html#SettingLogRetention) in the *Amazon CloudWatch Logs User Guide*\.
+ **Error logs** links to the logs written to `stderr` for this job run\. 

  This link takes you to CloudWatch Logs, where you can see details about any errors that were encountered\. You can manage your log retention period on the CloudWatch console\. The default log retention is `Never Expire`\. For more information about how to change the retention period, see [Change Log Data Retention in CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html#SettingLogRetention) in the *Amazon CloudWatch Logs User Guide*\.
+ **Execution time** shows the length of time during which the job run consumed resources\. The amount is calculated from when the job run starts consuming resources until it finishes\.
+ **Timeout** shows the maximum execution time during which this job run can consume resources before it stops and goes into timeout status\.
+ **Delay** shows the threshold before sending a job delay notification\. When a job run execution time reaches this threshold, AWS Glue sends a notification \("Glue Job Run Status"\) to CloudWatch Events\.
+ **Triggered by** shows the trigger that fired to start this job run\.
+ **Start time** shows the date and time \(local time\) that the job started\.
+ **End time** shows the date and time \(local time\) that the job ended\.

For a specific job run, you can **View run metrics**, which displays graphs of metrics for the selected job run\. For more information about how to enable metrics and interpret the graphs, see [Job Monitoring and Debugging](monitor-profile-glue-job-cloudwatch-metrics.md)\.

### Details<a name="console-jobs-details-details"></a>

The **Details** tab includes attributes of your job\. It shows you the details about the job definition and also lists the triggers that can start this job\. Each time one of the triggers in the list fires, the job is started\. For the list of triggers, the details include the following:
+ **Trigger name** shows the names of triggers that start this job when fired\.
+ **Trigger type** lists the type of trigger that starts this job\.
+ **Trigger status** displays whether the trigger is created, activated, or deactivated\.
+ **Trigger parameters** shows parameters that define when the trigger fires\.
+ **Jobs to trigger** shows the list of jobs that start when this trigger fires\.

**Note**  
The **Details** tab does not include source and target information\. Review the script to see the source and target details\.

### Script<a name="console-jobs-details-script"></a>

The **Script** tab shows the script that runs when your job is started\. You can invoke an **Edit script** view from this tab\. For more information about the script editor in the AWS Glue console, see [Working with Scripts on the AWS Glue Console](console-edit-script.md)\. For information about the functions that are called in your script, see [Program AWS Glue ETL Scripts in Python](aws-glue-programming-python.md)\.

### Metrics<a name="console-jobs-details-metrics"></a>

The **Metrics** tab shows metrics collected when a job runs and profiling is enabled\. The following graphs are shown: 
+ ETL Data Movement
+ Memory Profile: Driver and Executors

Choose **View additional metrics** to show the following graphs:
+ ETL Data Movement
+ Memory Profile: Driver and Executors
+ Data Shuffle Across Executors
+ CPU Load: Driver and Executors
+ Job Execution: Active Executors, Completed Stages & Maximum Needed Executors

Data for these graphs is pushed to CloudWatch metrics if the job is enabled to collect metrics\. For more information about how to enable metrics and interpret the graphs, see [Job Monitoring and Debugging](monitor-profile-glue-job-cloudwatch-metrics.md)\. 

**Example of ETL Data Movement Graph**  
The ETL Data Movement graph shows the following metrics:  
+ The number of bytes read from Amazon S3 by all executors—[`glue.ALL.s3.filesystem.read_bytes`](monitoring-awsglue-with-cloudwatch-metrics.md#glue.ALL.s3.filesystem.read_bytes)
+ The number of bytes written to Amazon S3 by all executors—[`glue.ALL.s3.filesystem.write_bytes`](monitoring-awsglue-with-cloudwatch-metrics.md#glue.ALL.s3.filesystem.write_bytes)

![\[The graph for ETL Data Movement in the Metrics tab of the AWS Glue console.\]](http://docs.aws.amazon.com/glue/latest/dg/images/job_detailed_etl.png)

**Example of Memory Profile Graph**  
The Memory Profile graph shows the following metrics:  
+ The fraction of memory used by the JVM heap for this driver \(scale: 0–1\) by the driver, an executor identified by *executorId*, or all executors—
  + [`glue.driver.jvm.heap.usage`](monitoring-awsglue-with-cloudwatch-metrics.md#glue.driver.jvm.heap.usage)
  + [`glue.executorId.jvm.heap.usage`](monitoring-awsglue-with-cloudwatch-metrics.md#glue.executorId.jvm.heap.usage)
  + [`glue.ALL.jvm.heap.usage`](monitoring-awsglue-with-cloudwatch-metrics.md#glue.ALL.jvm.heap.usage)

![\[The graph for Memory Profile in the Metrics tab of the AWS Glue console.\]](http://docs.aws.amazon.com/glue/latest/dg/images/job_detailed_mem.png)

**Example of Data Shuffle Across Executors Graph**  
The Data Shuffle Across Executors graph shows the following metrics:  
+ The number of bytes read by all executors to shuffle data between them—[`glue.driver.aggregate.shuffleLocalBytesRead`](monitoring-awsglue-with-cloudwatch-metrics.md#glue.driver.aggregate.shuffleLocalBytesRead)
+ The number of bytes written by all executors to shuffle data between them—[`glue.driver.aggregate.shuffleBytesWritten`](monitoring-awsglue-with-cloudwatch-metrics.md#glue.driver.aggregate.shuffleBytesWritten)

![\[The graph for Data Shuffle Across Executors in the Metrics tab of the AWS Glue console.\]](http://docs.aws.amazon.com/glue/latest/dg/images/job_detailed_data.png)

**Example of CPU Load Graph**  
The CPU Load graph shows the following metrics:  
+ The fraction of CPU system load used \(scale: 0–1\) by the driver, an executor identified by *executorId*, or all executors—
  + [`glue.driver.system.cpuSystemLoad`](monitoring-awsglue-with-cloudwatch-metrics.md#glue.driver.system.cpuSystemLoad)
  + [`glue.executorId.system.cpuSystemLoad`](monitoring-awsglue-with-cloudwatch-metrics.md#glue.executorId.system.cpuSystemLoad)
  + [`glue.ALL.system.cpuSystemLoad`](monitoring-awsglue-with-cloudwatch-metrics.md#glue.ALL.system.cpuSystemLoad)

![\[The graph for CPU Load in the Metrics tab of the AWS Glue console.\]](http://docs.aws.amazon.com/glue/latest/dg/images/job_detailed_cpu.png)

**Example of Job Execution Graph**  
The Job Execution graph shows the following metrics:  
+ The number of actively running executors—[`glue.driver.ExecutorAllocationManager.executors.numberAllExecutors`](monitoring-awsglue-with-cloudwatch-metrics.md#glue.driver.ExecutorAllocationManager.executors.numberAllExecutors)
+ The number of completed stages—[`glue.aggregate.numCompletedStages`](monitoring-awsglue-with-cloudwatch-metrics.md#glue.driver.aggregate.numCompletedStages)
+ The number of maximum needed executors—[`glue.driver.ExecutorAllocationManager.executors.numberMaxNeededExecutors`](monitoring-awsglue-with-cloudwatch-metrics.md#glue.driver.ExecutorAllocationManager.executors.numberMaxNeededExecutors)

![\[The graph for Job Execution in the Metrics tab of the AWS Glue console.\]](http://docs.aws.amazon.com/glue/latest/dg/images/job_detailed_exec.png)