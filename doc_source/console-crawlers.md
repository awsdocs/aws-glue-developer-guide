# Working with Crawlers on the AWS Glue Console<a name="console-crawlers"></a>

A crawler accesses your data store, extracts metadata, and creates table definitions in the AWS Glue Data Catalog\. The **Crawlers** pane in the AWS Glue console lists all the crawlers that you create\. The list displays status and metrics from the last run of your crawler\.

**To add a crawler using the console**

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue\)\. Choose **Crawlers** in the navigation pane\.

1. Choose **Add crawler**, and follow the instructions in the **Add crawler** wizard\.
**Note**  
To get step\-by\-step guidance for adding a crawler, choose **Add crawler** under **Tutorials** in the navigation pane\. You can also use the **Add crawler** wizard to create and modify an IAM role that attaches a policy that includes permissions for your Amazon Simple Storage Service \(Amazon S3\) data stores\.

   Optionally, you can tag your crawler with a **Tag key** and optional **Tag value**\. Once created, tag keys are read\-only\. Use tags on some resources to help you organize and identify them\. For more information, see [AWS Tags in AWS Glue](monitor-tags.md)\. 

   Optionally, you can add a security configuration to a crawler to specify at\-rest encryption options\. 

When a crawler runs, the provided IAM role must have permission to access the data store that is crawled\.

When you crawl a JDBC data store, a connection is required\. For more information, see [Adding an AWS Glue Connection](console-connections.md)\. An exclude path is relative to the include path\. For example, to exclude a table in your JDBC data store, type the table name in the exclude path\.

When you crawl DynamoDB tables, you can choose one table name from the list of DynamoDB tables in your account\.

**Tip**  
For more information about configuring crawlers, see [Crawler Properties](define-crawler.md)\.

## Viewing Crawler Results and Details<a name="console-crawlers-details"></a>

After the crawler runs successfully, it creates table definitions in the Data Catalog\. Choose **Tables** in the navigation pane to see the tables that were created by your crawler in the database that you specified\.

You can view information related to the crawler itself as follows:
+ The **Crawlers** page on the AWS Glue console displays the following properties for a crawler:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/console-crawlers.html)
+ To view the actions and log messages for a crawler, choose **Crawlers** in the navigation pane to see the crawlers you created\. Find the crawler name in the list and choose the **Logs** link\. This link takes you to the CloudWatch Logs, where you can see details about which tables were created in the AWS Glue Data Catalog and any errors that were encountered\.

  You can manage your log retention period in the CloudWatch console\. The default log retention is `Never Expire`\. For more information about how to change the retention period, see [Change Log Data Retention in CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/SettingLogRetention.html)\.

  For more information about viewing the log information, see [Automated Monitoring Tools](monitor-glue.md#monitoring-automated_tools) in this guide and [Querying AWS CloudTrail Logs](https://docs.aws.amazon.com/athena/latest/ug/cloudtrail-logs.html) in the *Amazon Athena User Guide*\. Also, see the blog [Easily query AWS service logs using Athena](http://aws.amazon.com/blogs/big-data/easily-query-aws-service-logs-using-amazon-athena/) for information about how to use the Athena Glue Service Logs \(AGSlogger\) Python library in conjunction with AWS Glue ETL jobs to allow a common framework for processing log data\.
+ To see detailed information for a crawler, choose the crawler name in the list\. Crawler details include the information you defined when you created the crawler with the **Add crawler** wizard\.