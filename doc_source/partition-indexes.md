# Working with Partition Indexes<a name="partition-indexes"></a>

Over time, hundreds of thousands of partitions get added to a table\. The [GetPartitions API](https://docs.aws.amazon.com/glue/latest/webapi/API_GetPartitions.html) is used to fetch the partitions in the table\. The API returns partitions which match the expression provided in the request\.

Lets take a *sales\_data* table as an example which is partitioned by the keys *Country*, *Category*, *Year*, and *Month*\. If you want to obtain sales data for all the items sold for the *Books* category in the year *2020*, you have to make a `GetPartitions` request with the expression "Category = Books and year = 2020" to the Data Catalog\.

If no partition indexes are present on the table, AWS Glue loads all the partitions of the table, and then filters the loaded partitions using the query expression provided by the user in the `GetPartitions` request\. The query takes more time to run as the number of partitions increase on a table with no indexes\. With an index, the `GetPartitions` query will try to fetch a subset of the partitions instead of loading all the partitions in the table\.

**Topics**
+ [About Partition Indexes](#partition-index-1)
+ [Creating a Table with Partition Indexes](#partition-index-creating-table)
+ [Adding a Partition Index to an Existing Table](#partition-index-existing-table)
+ [Describing Partition Indexes on a Table](#partition-index-describing)
+ [Limitations on Using Partition Indexes](#partition-index-limitations)
+ [Using Indexes for an Optimized GetPartitions Call](#partition-index-getpartitions)

## About Partition Indexes<a name="partition-index-1"></a>

When you create a partition index, you specify a list of partition keys that already exist on a given table\. Partition index is sub list of partition keys defined in the table\. A partition index can be created on any permutation of partition keys defined on the table\. For the above *sales\_data* table, the possible indexes are \(country, category, year, month\), \(country, category, year\), \(country, category\), \(country\), \(category, country, year, month\), and so on\.

The Data Catalog will concatenate the partition values in the order provided at the time of index creation\. The index is built consistently as partitions are added to the table\. Indexes can be created for String and Numeric \(int, bigint, long, tinyint, and smallint\) column types\. For example, for a table with the partition keys `country` \(String\), `item` \(String\), `creationDate` \(date\), an index cannot be created on the partition key `creationDate`\.

Indexes on Numeric data types support =, >, >=, <, <= and between operators\. The String data type only supports the equal \(=\) operator\. The indexing solution currently only supports the `AND` logical operator\. Sub\-expressions with the operators "LIKE", "IN", "OR", and "NOT" are ignored in the expression for filtering using an index\. Filtering for the ignored sub\-expression is done on the partitions fetched after applying index filtering\.

For each partition added to a table, there is a corresponding index item created\. For a table with ‘n’ partitions, 1 partition index will result in 'n' partition index items\. 'm' partition index on same table will result into 'm\*n' partition index items\. Each partition index item will be charged according to the current AWS Glue pricing policy for data catalog storage\. For details on storage object pricing, see [AWS Glue pricing](https://aws.amazon.com/glue/pricing/)\.

## Creating a Table with Partition Indexes<a name="partition-index-creating-table"></a>

You can create a partition index during table creation\. The `CreateTable` request takes a list of [`PartitionIndex` objects](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-api-catalog-tables.html#aws-glue-api-catalog-tables-PartitionIndex) as an input\. A maximum of 3 partition indexes can be created on a given table\. Each partition index requires a name and a list of `partitionKeys` defined for the table\. Created indexes on a table can be fetched using the [`GetPartitionIndexes` API](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-api-catalog-tables.html#aws-glue-api-catalog-tables-GetPartitionIndexes)

## Adding a Partition Index to an Existing Table<a name="partition-index-existing-table"></a>

To add a partition index to an existing table, use the `CreatePartitionIndex` operation\. You can create one `PartitionIndex` per `CreatePartitionIndex` operation\. Adding an index does not affect the availability of a table, as the table continues to be available while indexes are being created\.

The index status for an added partition is set to CREATING and the creation of the index data is started\. If the process for creating the indexes is successful, the indexStatus is updated to ACTIVE and for an unsuccessful process, the index status is updated to FAILED\. Index creation can fail for multiple reasons, and you can use the `GetPartitionIndexes` operation to retrieve the failure details\. The possible failures are:
+ ENCRYPTED\_PARTITION\_ERROR — Index creation on a table with encrypted partitions is not supported\.
+ INVALID\_PARTITION\_TYPE\_DATA\_ERROR — Observed when the `partitionKey` value is not a valid value for the corresponding `partitionKey` data type\. For example: a `partitionKey` with the 'int' datatype has a value 'foo'\.
+ MISSING\_PARTITION\_VALUE\_ERROR — Observed when the `partitionValue` for an `indexedKey` is not present\. This can happen when a table is not partitioned consistently\.
+ UNSUPPORTED\_PARTITION\_CHARACTER\_ERROR — Observed when the value for an indexed partition key contains the characters \\u0000, \\u0001 or \\u0002
+ INTERNAL\_ERROR — An internal error occurred while indexes were being created\. 

## Describing Partition Indexes on a Table<a name="partition-index-describing"></a>

To fetch the partition indexes created on a table, use the `GetPartitionIndexes` operation\. The response returns all the indexes on the table, along with the current status of each index \(the `IndexStatus`\)\.

The `IndexStatus` for a partition index will be one of the following:
+ `CREATING` — The index is currently being created, and is not yet available for use\.
+ `ACTIVE` — The index is ready for use\. Requests can use the index to perform an optimized query\.
+ `DELETING` — The index is currently being deleted, and can no longer be used\. An index in the active state can be deleted using the `DeletePartitionIndex` request, which moves the status from ACTIVE to DELETING\.
+ `FAILED` — The index creation on an existing table failed\. Each table stores the last 10 failed indexes\.

The possible state transitions for indexes created on an existing table are:
+ CREATING → ACTIVE → DELETING
+ CREATING → FAILED

## Limitations on Using Partition Indexes<a name="partition-index-limitations"></a>

Once you have created a partition index, note these changes to table and partition functionality:

**New Partition Creation \(after Index Addition\)**  
After a partition index is created on a table, all new partitions added to the table will be validated for the data type checks for indexed keys\. The partition value of the indexed keys will be validated for data type format\. If the data type check fails, the create partition operation will fail\. For the *sales\_data* table, if an index is created for keys \(category, year\) where the category is of type `string` and year of type `int`, the creation of the new partition with a value of YEAR as "foo" will fail\.

After indexes are enabled, the addition of partitions with indexed key values having the characters U\+0000, U\+00001, and U\+0002 will start to fail\.

**Table Updates**  
Once a partition index is created on a table, you cannot modify the partition key names for existing partition keys, and you cannot change the type, or order, of keys which are registered with the index\.

**Create Table As Select \(CTAS\) Statements in Athena**  
The [INSERT INTO option](https://docs.aws.amazon.com/athena/latest/ug/ctas-insert-into-etl.html) is not supported on tables with a partition index\. Tables with a partition index cannot use the INSERT INTO command on Athena\. If an index is created on a table, INSERT INTO queries will fail with the message "HIVE\_METASTORE\_ERROR: Partition creation cannot be completed since partition indexes are enabled on table"\. 

**Limited Support**  
Calls from Athena, Redshift Spectrum and AWS Glue ETL do not currently utilize indexes for fetching partitions\. Currently, catalog requests from EMR are able to utilize indexes for fetching partitions\.

## Using Indexes for an Optimized GetPartitions Call<a name="partition-index-getpartitions"></a>

When you call `GetPartitions` on a table with an index, you can include an expression, and if applicable the Data Catalog will use an index if possible\. The first key of the index should be passed in the expression for the indexes to be used in filtering\. Index optimization in filtering is applied as a best effort\. The Data Catalog tries to use index optimization as much as possible, but in case of a missing index, or unsupported operator, it falls back to the existing implementation of loading all partitions\. 

For the *sales\_data* table above, lets add the index \[Country, Category, Year\]\. If "Country" is not passed in the expression, the registered index will not be able to filter partitions using indexes\. You can add up to 3 indexes to support various query patterns\.

Lets take some example expressions and see how indexes work on them:


| Expressions | How Index Will Be Used | 
| --- | --- | 
|  Country = 'US'  |  Index will be used to filter partitions\.  | 
|  Country = 'US' and Category = 'Shoes'  |  Index will be used to filter partitions\.  | 
|  Category = 'Shoes'  |  Indexes will not be used as "country" is not provided in the expression\. All partitions will be loaded to return a response\.  | 
|  Country = 'US' and Category = 'Shoes' and Year > '2018'  |  Index will be used to filter partitions\.  | 
|  Country = 'US' and Category = 'Shoes' and Year > '2018' and month = 2  |  Index will be used to fetch all partitions with country = "US" and category = "shoes" and year > 2018\. Then, filtering on the month expression will be performed\.  | 
|  Country = 'US' AND Category = 'Shoes' OR Year > '2018'  |  Indexes will not be used as an `OR` operator is present in the expression\.  | 
|  Country = 'US' AND Category = 'Shoes' AND \(Year = 2017 OR Year = '2018'\)  |  Index will be used to fetch all partitions with country = "US" and category = "shoes", and then filtering on the year expression will be performed\.  | 
|  Country in \('US', 'UK'\) AND Category = 'Shoes'  |  Indexes will not be used for filtering as the `IN` operator is not supported currently\.  | 