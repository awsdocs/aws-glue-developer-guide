# AWS Glue Job Run Statuses<a name="job-run-statuses"></a>

You can view the status of an AWS Glue extract, transform, and load \(ETL\) job while it is running or after it has stopped\. You can view the status using the AWS Glue console, the AWS Command Line Interface \(AWS CLI\), or the [GetJobRun action](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-api-jobs-runs.html#aws-glue-api-jobs-runs-GetJobRun) in the AWS Glue API\.

Possible job run statuses are `STARTING`, `RUNNING`, `STOPPING`, `STOPPED`, `SUCCEEDED`, `FAILED`, `ERROR`, and `TIMEOUT`\. The following table lists the statuses that indicate abnormal job termination\.


| Job run status | Description | 
| --- | --- | 
| FAILED | The job exceeded its maximum allowed concurrent runs, or terminated with an unknown exit code\. | 
| ERROR | A workflow, schedule trigger, or event trigger attempted to run a deleted job\.  | 
| TIMEOUT | The job run time exceeded its specified timeout value\. | 