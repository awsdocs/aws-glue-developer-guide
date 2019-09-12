# Working with Tables on the AWS Glue Console<a name="console-tables"></a>

A table in the AWS Glue Data Catalog is the metadata definition that represents the data in a data store\. You create tables when you run a crawler, or you can create a table manually in the AWS Glue console\. The **Tables** list in the AWS Glue console displays values of your table's metadata\. You use table definitions to specify sources and targets when you create ETL \(extract, transform, and load\) jobs\. 

To get started, sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\. Choose the **Tables** tab, and use the **Add tables** button to create tables either with a crawler or by manually typing attributes\. 

## Adding Tables on the Console<a name="console-tables-add"></a>

To use a crawler to add tables, choose **Add tables**, **Add tables using a crawler**\. Then follow the instructions in the **Add crawler** wizard\. When the crawler runs, tables are added to the AWS Glue Data Catalog\. For more information, see [Defining Crawlers](add-crawler.md)\.

If you know the attributes that are required to create an Amazon Simple Storage Service \(Amazon S3\) table definition in your Data Catalog, you can create it with the table wizard\. Choose **Add tables**, **Add table manually**, and follow the instructions in the **Add table** wizard\.

When adding a table manually through the console, consider the following:
+ If you plan to access the table from Amazon Athena, then provide a name with only alphanumeric and underscore characters\. For more information, see [Athena names](https://docs.aws.amazon.com/athena/latest/ug/tables-databases-columns-names.html#ate-table-database-and-column-names-allow-only-underscore-special-characters)\.
+ The location of your source data must be an Amazon S3 path\.
+ The data format of the data must match one of the listed formats in the wizard\. The corresponding classification, SerDe, and other table properties are automatically populated based on the format chosen\. You can define tables with the following formats:   
**JSON**  
JavaScript Object Notation\.  
**CSV**  
Character separated values\. You also specify the delimiter of either comma, pipe, semicolon, tab, or Ctrl\-A\.  
**Parquet**  
Apache Parquet columnar storage\.  
**Avro**  
Apache Avro JSON binary format\.  
**XML**  
Extensible Markup Language format\. Specify the XML tag that defines a row in the data\. Columns are defined within row tags\.
+ You can define a partition key for the table\.
+ Currently, partitioned tables that you create with the console cannot be used in ETL jobs\.

## Table Attributes<a name="console-tables-attributes"></a>

The following are some important attributes of your table:

**Table name**  
The name is determined when the table is created, and you can't change it\. You refer to a table name in many AWS Glue operations\.

**Database**  
The container object where your table resides\. This object contains an organization of your tables that exists within the AWS Glue Data Catalog and might differ from an organization in your data store\. When you delete a database, all tables contained in the database are also deleted from the Data Catalog\.  

**Location**  
The pointer to the location of the data in a data store that this table definition represents\.

**Classification**  
A categorization value provided when the table was created\. Typically, this is written when a crawler runs and specifies the format of the source data\.

**Last updated**  
The time and date \(UTC\) that this table was updated in the Data Catalog\.

**Date added**  
The time and date \(UTC\) that this table was added to the Data Catalog\.

**Description**  
The description of the table\. You can write a description to help you understand the contents of the table\.

**Deprecated**  
If AWS Glue discovers that a table in the Data Catalog no longer exists in its original data store, it marks the table as deprecated in the data catalog\. If you run a job that references a deprecated table, the job might fail\. Edit jobs that reference deprecated tables to remove them as sources and targets\. We recommend that you delete deprecated tables when they are no longer needed\. 

**Connection**  
If AWS Glue requires a connection to your data store, the name of the connection is associated with the table\.

## Viewing and Editing Table Details<a name="console-tables-details"></a>

To see the details of an existing table, choose the table name in the list, and then choose **Action, View details**\.

The table details include properties of your table and its schema\.   This view displays the schema of the table, including column names in the order defined for the table, data types, and key columns for partitions\.  If a column is a complex type, you can choose **View properties** to display details of the structure of that field, as shown in the following example:

```
{
"StorageDescriptor": {
"cols": {
	"FieldSchema": [
	{
	"name": "primary-1",
	"type": "CHAR",
	"comment": ""
	},
	{
	"name": "second ",
	"type": "STRING",
	"comment": ""
	}
	]
},
"location": "s3://aws-logs-111122223333-us-east-1",
"inputFormat": "",
"outputFormat": "org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat",
"compressed": "false",
"numBuckets": "0",
"SerDeInfo": {
	"name": "",
	"serializationLib": "org.apache.hadoop.hive.serde2.OpenCSVSerde",
	"parameters": {
		"separatorChar": "|"
	}
},
"bucketCols": [],
"sortCols": [],
"parameters": {},
"SkewedInfo": {},
"storedAsSubDirectories": "false"
},
"parameters": {
"classification": "csv"
}
}
```

For more information about the properties of a table, such as `StorageDescriptor`, see [StorageDescriptor Structure](aws-glue-api-catalog-tables.md#aws-glue-api-catalog-tables-StorageDescriptor)\.

To change the schema of a table, choose **Edit schema** to add and remove columns, change column names, and change data types\.

To compare different versions of a table, including its schema, choose **Compare versions** to see a side\-by\-side comparison of two versions of the schema for a table\.

To display the files that make up an Amazon S3 partition, choose **View partition**\. For Amazon S3 tables, the **Key** column displays the partition keys that are used to partition the table in the source data store\. Partitioning is a way to divide a table into related parts based on the values of a key column, such as date, location, or department\. For more information about partitions, search the internet for information about "hive partitioning\."

**Note**  
To get step\-by\-step guidance for viewing the details of a table, see the **Explore table** tutorial in the console\.