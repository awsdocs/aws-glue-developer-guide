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
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/console-jobs.html)

**Note**  
The job assumes the permissions of the **IAM role** that you specify when you create it\. This IAM role must have permission to extract data from your data store and write to your target\. The AWS Glue console only lists IAM roles that have attached a trust policy for the AWS Glue principal service\. For more information about providing roles for AWS Glue, see [Using Identity\-Based Policies \(IAM Policies\)](using-identity-based-policies.md)\.

**Important**  
Check [Troubleshooting Errors in AWS Glue](glue-troubleshooting-errors.md) for known problems when a job runs\.

To learn about the properties that are required for each job, see [Defining Job Properties](add-job.md#create-job)\.

To get step\-by\-step guidance for adding a job with a generated script, see the **Add job** tutorial in the AWS Glue console\.

## Viewing Job Details<a name="console-jobs-details"></a>

To see details of a job, select the job in the **Jobs** list and review the information on the following tabs:
+ History
+ Details
+ Script

### History<a name="console-jobs-details-history"></a>

The **History** tab shows your job run history and how successful a job has been in the past\. For each job, the run metrics include the following:
+ **Run ID** is an identifier created by AWS Glue for each run of this job\.
+ **Retry attempt** shows the number of attempts for jobs that required AWS Glue to automatically retry\.
+ **Run status** shows the success of each run listed with the most recent run at the top\. If a job is `Running` or `Starting`, you can choose the action icon in this column to stop it\.
+ **Error** shows the details of an error meesage if the run was not successful\.
+ **Logs** links to the logs written to `stdout` for this job run\.

  The **Logs** link takes you to the CloudWatch Logs, where you can see all the details about the tables that were created in the AWS Glue Data Catalog and any errors that were encountered\. You can manage your log retention period in the CloudWatch console\. The default log retention is `Never Expire`\. For more information about how to change the retention period, see [Change Log Data Retention in CloudWatch Logs](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/SettingLogRetention.html)\.
+ **Error logs** links to the logs written to `stderr` for this job run\. 

  This link takes you to the CloudWatch Logs, where you can see details about any errors that were encountered\. You can manage your log retention period in the CloudWatch console\. The default log retention is `Never Expire`\. For more information about how to change the retention period, see [Change Log Data Retention in CloudWatch Logs](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/SettingLogRetention.html)\.
+ **Execution time** shows the length of time during which the job run consumed resources\. The amount is calculated from when the job run starts consuming resources until it finishes\.
+ **Timeout** shows the maximum execution time during which this job run can consume resources before it stops and goes into timeout status\.
+ **Triggered by** shows the trigger that fired to start this job run\.
+ **Start time** shows the date and time \(local time\) that the job started\.
+ **End time** shows the date and time \(local time\) that the job ended\.

### Details<a name="console-jobs-details-details"></a>

The **Details** tab includes attributes of your job\. It shows you the details about the job definition and also lists the triggers that can start this job\. Each time one of the triggers in the list fires, the job is started\. For the list of triggers, the details include the following:
+ **Trigger name** shows the names of triggers that start this job when fired\.
+ **Trigger type** lists the type of trigger that starts this job\.
+ **Trigger status** displays whether the trigger is created, activated, or deactivated\.
+ **Trigger parameters** shows parameters that define when the trigger fires\.
+ **Jobs to trigger** shows the list of jobs that start when this trigger fires\.

### Script<a name="console-jobs-details-script"></a>

The **Script** tab shows the script that runs when your job is started\. You can invoke an **Edit script** view from this tab\. For more information about the script editor in the AWS Glue console, see [Working with Scripts on the AWS Glue Console](console-edit-script.md)\. For information about the functions that are called in your script, see [Program AWS Glue ETL Scripts in Python](aws-glue-programming-python.md)\.