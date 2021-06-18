# How Does a Crawler Determine When to Create Partitions?<a name="crawler-s3-folder-table-partition"></a>

When an AWS Glue crawler scans Amazon S3 and detects multiple folders in a bucket, it determines the root of a table in the folder structure and which folders are partitions of a table\. The name of the table is based on the Amazon S3 prefix or folder name\. You provide an **Include path** that points to the folder level to crawl\. When the majority of schemas at a folder level are similar, the crawler creates partitions of a table instead of separate tables\. To influence the crawler to create separate tables, add each table's root folder as a separate data store when you define the crawler\.

For example, consider the following Amazon S3 folder structure\.

![\[Rectangles at multiple levels represent a folder hierarchy in Amazon S3. The top rectangle is labeled Sales. Rectangle below that is labeled year=2019. Two rectangles below that are labeled month=Jan and month=Feb. Each of those rectangles has two rectangles below them, labeled day=1 and day=2. All four "day" (bottom) rectangles have either two or four files under them. All rectangles and files are connected with lines.\]](http://docs.aws.amazon.com/glue/latest/dg/images/crawlers-s3-folders.png)

The paths to the four lowest level folders are the following:

```
S3://sales/year=2019/month=Jan/day=1
S3://sales/year=2019/month=Jan/day=2
S3://sales/year=2019/month=Feb/day=1
S3://sales/year=2019/month=Feb/day=2
```

Assume that the crawler target is set at `Sales`, and that all files in the `day=n` folders have the same format \(for example, JSON, not encrypted\), and have the same or very similar schemas\. The crawler will create a single table with four partitions, with partition keys `year`, `month`, and `day`\.

In the next example, consider the following Amazon S3 structure:

```
s3://bucket01/folder1/table1/partition1/file.txt
s3://bucket01/folder1/table1/partition2/file.txt
s3://bucket01/folder1/table1/partition3/file.txt
s3://bucket01/folder1/table2/partition4/file.txt
s3://bucket01/folder1/table2/partition5/file.txt
```

If the schemas for files under `table1` and `table2` are similar, and a single data store is defined in the crawler with **Include path** `s3://bucket01/folder1/`, the crawler creates a single table with two partition key columns\. The first partition key column contains `table1` and `table2`, and the second partition key column contains `partition1` through `partition3` for the `table1` partition and `partition4` and `partition5` for the `table2` partition\. To create two separate tables, define the crawler with two data stores\. In this example, define the first **Include path** as `s3://bucket01/folder1/table1/` and the second as `s3://bucket01/folder1/table2`\.

**Note**  
In Amazon Athena, each table corresponds to an Amazon S3 prefix with all the objects in it\. If objects have different schemas, Athena does not recognize different objects within the same prefix as separate tables\. This can happen if a crawler creates multiple tables from the same Amazon S3 prefix\. This might lead to queries in Athena that return zero results\. For Athena to properly recognize and query tables, create the crawler with a separate **Include path** for each different table schema in the Amazon S3 folder structure\. For more information, see [Best Practices When Using Athena with AWS Glue](https://docs.aws.amazon.com/athena/latest/ug/glue-best-practices.html) and this [AWS Knowledge Center article](https://aws.amazon.com/premiumsupport/knowledge-center/athena-empty-results/)\.