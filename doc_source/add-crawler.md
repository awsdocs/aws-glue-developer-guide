# Defining Crawlers<a name="add-crawler"></a>

You can use a crawler to populate the AWS Glue Data Catalog with tables\. This is the primary method used by most AWS Glue users\. A crawler can crawl multiple data stores in a single run\. Upon completion, the crawler creates or updates one or more tables in your Data Catalog\. Extract, transform, and load \(ETL\) jobs that you define in AWS Glue use these Data Catalog tables as sources and targets\. The ETL job reads from and writes to the data stores that are specified in the source and target Data Catalog tables\.

For more information about using the AWS Glue console to add a crawler, see [Working with Crawlers on the AWS Glue Console](console-crawlers.md)\.

**Topics**
+ [Which Data Stores Can I Crawl?](crawler-data-stores.md)
+ [How Crawlers Work](crawler-running.md)
+ [Crawler Prerequisites](crawler-prereqs.md)
+ [Crawler Properties](define-crawler.md)
+ [Setting Crawler Configuration Options](crawler-configuration.md)
+ [Scheduling an AWS Glue Crawler](schedule-crawler.md)
+ [Working with Crawlers on the AWS Glue Console](console-crawlers.md)