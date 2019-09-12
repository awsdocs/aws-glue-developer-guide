# Logging and Monitoring in AWS Glue<a name="logging-and-monitoring"></a>

You can automate the running of your ETL \(extract, transform, and load\) jobs\. AWS Glue provides metrics for crawlers and jobs that you can monitor\. After you set up the AWS Glue Data Catalog with the required metadata, AWS Glue provides statistics about the health of your environment\. You can automate the invocation of crawlers and jobs with a time\-based schedule based on cron\. You can also trigger jobs when an event\-based trigger fires\. 

AWS Glue is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or AWS service in AWS Glue\. If you create a trail, you can enable continuous delivery of CloudTrail events to an Amazon Simple Storage Service \(Amazon S3\) bucket, Amazon CloudWatch Logs, and Amazon CloudWatch Events\. Every event or log entry contains information about who generated the request\. 

Use Amazon CloudWatch Events to automate your AWS services and respond automatically to system events such as application availability issues or resource changes\. Events from AWS services are delivered to CloudWatch Events in near\-real time\. You can write simple rules to indicate which events are of interest and what automated actions to take when an event matches a rule\. 

For more information, see [Automating AWS Glue with CloudWatch Events](automating-awsglue-with-cloudwatch-events.md)\.