# Working with Crawlers on the AWS Glue Console<a name="console-crawlers"></a>

A crawler accesses your data store, extracts metadata, and creates table definitions in the AWS Glue Data Catalog\. The **Crawlers** pane in the AWS Glue console lists all the crawlers that you create\. The list displays status and metrics from the last run of your crawler\.

**To add a crawler using the console**

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\. Choose **Crawlers** in the navigation pane\.

1. Choose **Add crawler**, and follow the instructions in the **Add crawler** wizard\.
**Note**  
To get step\-by\-step guidance for adding a crawler, choose **Add crawler** under **Tutorials** in the navigation pane\. You can also use the **Add crawler** wizard to create and modify an IAM role that attaches a policy that includes permissions for your Amazon Simple Storage Service \(Amazon S3\) data stores\.

   Optionally, you can tag your crawler with a **Tag key** and optional **Tag value**\. Once created, tag keys are read\-only\. Use tags on some resources to help you organize and identify them\. For more information, see [AWS Tags in AWS Glue](monitor-tags.md)\. 

   Optionally, you can add a security configuration to a crawler to specify at\-rest encryption options\. 

When a crawler runs, the provided IAM role must have permission to access the data store that is crawled\. For an Amazon S3 data store, you can use the AWS Glue console to create a policy or add a policy similar to the following: 

```
{
   "Version": "2012-10-17",
    "Statement": [
        {
          "Effect": "Allow",
          "Action": [
              "s3:GetObject",
              "s3:PutObject"
          ],
          "Resource": [
              "arn:aws:s3:::bucket/object*"
          ]
        }
    ]
}
```

If the crawler reads KMS encrypted Amazon S3 data, then the **IAM role** must have decrypt permission on the KMS key\. For more information, see [Step 2: Create an IAM Role for AWS Glue](create-an-iam-role.md)\.

For an Amazon DynamoDB data store, you can use the AWS Glue console to create a policy or add a policy similar to the following: 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:DescribeTable",
        "dynamodb:Scan"
      ],
      "Resource": [
        "arn:aws:dynamodb:region:account-id:table/table-name*"
      ]
    }
  ]
}
```

For Amazon S3 data stores, an exclude pattern is relative to the include path\. For more information about glob patterns, see [Which Data Stores Can I Crawl?](add-crawler.md#crawler-data-stores)\.

When you crawl a JDBC data store, a connection is required\. For more information, see [Working with Connections on the AWS Glue Console](console-connections.md)\. An exclude path is relative to the include path\. For example, to exclude a table in your JDBC data store, type the table name in the exclude path\.

When you crawl DynamoDB tables, you can choose one table name from the list of DynamoDB tables in your account\.

## Viewing Crawler Results<a name="console-crawlers-details"></a>

To view the results of a crawler, find the crawler name in the list and choose the **Logs** link\. This link takes you to the CloudWatch Logs, where you can see details about which tables were created in the AWS Glue Data Catalog and any errors that were encountered\. You can manage your log retention period in the CloudWatch console\. The default log retention is `Never Expire`\. For more information about how to change the retention period, see [Change Log Data Retention in CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/SettingLogRetention.html)\.

To see details of a crawler, choose the crawler name in the list\. Crawler details include the information you defined when you created the crawler with the **Add crawler** wizard\. When a crawler run completes, choose **Tables** in the navigation pane to see the tables that were created by your crawler in the database that you specified\.

**Note**  
The crawler assumes the permissions of the **IAM role** that you specify when you define it\. This IAM role must have permissions to extract data from your data store and write to the Data Catalog\. The AWS Glue console lists only IAM roles that have attached a trust policy for the AWS Glue principal service\. From the console, you can also create an IAM role with an IAM policy to access Amazon S3 data stores accessed by the crawler\. For more information about providing roles for AWS Glue, see [Identity\-Based Policies](using-identity-based-policies.md)\.

The following are some important properties and metrics about the last run of a crawler:

**Name**  
When you create a crawler, you must give it a unique name\.

**Schedule**  
You can choose to run your crawler on demand or choose a frequency with a schedule\. For more information about scheduling a crawler, see [Scheduling a Crawler](schedule-crawler.md)\.

**Status**  
A crawler can be ready, starting, stopping, scheduled, or schedule paused\. A running crawler progresses from starting to stopping\. You can resume or pause a schedule attached to a crawler\.

**Logs**  
Links to any available logs from the last run of the crawler\.

**Last runtime**  
The amount of time it took the crawler to run when it last ran\.

**Median runtime**  
The median amount of time it took the crawler to run since it was created\.

**Tables updated**  
The number of tables in the AWS Glue Data Catalog that were updated by the latest run of the crawler\.

**Tables added**  
The number of tables that were added into the AWS Glue Data Catalog by the latest run of the crawler\.