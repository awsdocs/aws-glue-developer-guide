# Incremental Crawls in AWS Glue<a name="incremental-crawls"></a>

For an Amazon Simple Storage Service \(Amazon S3\) data source, incremental crawls only crawl folders that were added since the last crawler run\. Without this option, the crawler crawls the entire dataset\. Incremental crawls can save significant time and cost\. To perform an incremental crawl, you can set the **Crawl new folders only** option in the AWS Glue console or set the `RecrawlPolicy` property in the `CreateCrawler` request in the API\.

Incremental crawls are best suited to incremental datasets with a stable table schema\. The typical use case is for scheduled crawlers, where during each crawl, new partitions are added\. Continuing with the example in [How Does a Crawler Determine When to Create Partitions?](crawler-s3-folder-table-partition.md), the following diagram shows that files for the month of March have been added\.

![\[The folder (rectangle) hierarchy is the same as in the previous image, except that a rectangle for the month of March is added, with a single subfolder, day=1. That subfolder has four files.\]](http://docs.aws.amazon.com/glue/latest/dg/images/crawlers-s3-folders-new.png)

If you set the **Crawl new folders only** option, only the new folder, `month=Mar` is crawled\.

**Notes and Restrictions for Incremental Crawls**  
Keep in mind the following additional information about incremental crawls:
+ The best practice for incremental crawls is to first run a complete crawl on the target dataset to enable the crawler to record the initial schema and partition structure\.
+ When this option is turned on, you can't change the Amazon S3 target data stores when editing the crawler\.
+ This option affects certain crawler configuration settings\. When turned on, it forces the update behavior and delete behavior of the crawler to `LOG`\. This means that:
  + If an incremental crawl discovers objects with schemas that are different enough from the schema recorded in the Data Catalog such that the crawler cannot create new partitions, the crawler ignores the objects and records the event in CloudWatch Logs\.
  + If an incremental crawl discovers deleted objects, it ignores them and doesn't update the Data Catalog\.

  For more information, see [Setting Crawler Configuration Options](crawler-configuration.md)\.
+ If an incremental crawl discovers multiple new partitions or folders added, the majority of them have to match the schema recorded in the Data Catalog to enable the crawler to add them successfully\. Otherwise, the crawler might fail to add the partitions because there are too many schema varieties\.