# Reading from JDBC Tables in Parallel<a name="run-jdbc-parallel-read-job"></a>

You can set properties of your JDBC table to enable AWS Glue to read data in parallel\. When you set certain properties, you instruct AWS Glue to run parallel SQL queries against logical partitions of your data\. You can control partitioning by setting a hash field or a hash expression\. You can also control the number of parallel reads that are used to access your data\. 

To enable parallel reads, you can set key\-value pairs in the parameters field of your table structure\. Use JSON notation to set a value for the parameter field of your table\. For more information about editing the properties of a table, see [Viewing and Editing Table Details](console-tables.md#console-tables-details)\. You can also enable parallel reads when you call the ETL \(extract, transform, and load\) methods `create_dynamic_frame_from_options` and `create_dynamic_frame_from_catalog`\. For more information about specifying options in these methods, see [from\_options](aws-glue-api-crawler-pyspark-extensions-dynamic-frame-reader.md#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-reader-from_options) and [from\_catalog](aws-glue-api-crawler-pyspark-extensions-dynamic-frame-reader.md#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-reader-from_catalog) \. 

You can use this method for JDBC tables, that is, most tables whose base data is a JDBC data store\. These properties are ignored when reading Amazon Redshift and Amazon S3 tables\.

**hashfield**  
Set `hashfield` to the name of a column in the JDBC table to be used to divide the data into partitions\. For best results, this column should have an even distribution of values to spread the data between partitions\. This column can be of any data type\. AWS Glue generates non\-overlapping queries that run in parallel to read the data partitioned by this column\. For example, if your data is evenly distributed by month, you can use the `month` column to read each month of data in parallel:  

```
  'hashfield': 'month'
```
AWS Glue creates a query to hash the field value to a partition number and runs the query for all partitions in parallel\. To use your own query to partition a table read, provide a `hashexpression` instead of a `hashfield`\.

**hashexpression**  
Set `hashexpression` to an SQL expression \(conforming to the JDBC database engine grammar\) that returns a whole number\. A simple expression is the name of any numeric column in the table\. AWS Glue generates SQL queries to read the JDBC data in parallel using the `hashexpression` in the `WHERE` clause to partition data\.  
For example, use the numeric column `customerID` to read data partitioned by a customer number:  

```
  'hashexpression': 'customerID'
```
To have AWS Glue control the partitioning, provide a `hashfield` instead of a `hashexpression`\.

**hashpartitions**  
Set `hashpartitions` to the number of parallel reads of the JDBC table\. If this property is not set, the default value is 7\.  
For example, set the number of parallel reads to `5` so that AWS Glue reads your data with five queries \(or fewer\):  

```
  'hashpartitions': '5'
```