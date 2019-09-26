# Partition API<a name="aws-glue-api-catalog-partitions"></a>

The Partition API describes data types and operations used to work with partitions\.

## Data Types<a name="aws-glue-api-catalog-partitions-objects"></a>
+ [Partition Structure](#aws-glue-api-catalog-partitions-Partition)
+ [PartitionInput Structure](#aws-glue-api-catalog-partitions-PartitionInput)
+ [PartitionSpecWithSharedStorageDescriptor Structure](#aws-glue-api-catalog-partitions-PartitionSpecWithSharedStorageDescriptor)
+ [PartitionListComposingSpec Structure](#aws-glue-api-catalog-partitions-PartitionListComposingSpec)
+ [PartitionSpecProxy Structure](#aws-glue-api-catalog-partitions-PartitionSpecProxy)
+ [PartitionValueList Structure](#aws-glue-api-catalog-partitions-PartitionValueList)
+ [Segment Structure](#aws-glue-api-catalog-partitions-Segment)
+ [PartitionError Structure](#aws-glue-api-catalog-partitions-PartitionError)

## Partition Structure<a name="aws-glue-api-catalog-partitions-Partition"></a>

Represents a slice of table data\.

**Fields**
+ `Values` – An array of UTF\-8 strings\.

  The values of the partition\.
+ `DatabaseName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database in which to create the partition\.
+ `TableName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the database table in which to create the partition\.
+ `CreationTime` – Timestamp\.

  The time at which the partition was created\.
+ `LastAccessTime` – Timestamp\.

  The last time at which the partition was accessed\.
+ `StorageDescriptor` – A [StorageDescriptor](aws-glue-api-catalog-tables.md#aws-glue-api-catalog-tables-StorageDescriptor) object\.

  Provides information about the physical location where the partition is stored\.
+ `Parameters` – A map array of key\-value pairs\.

  Each key is a Key string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Each value is a UTF\-8 string, not more than 512000 bytes long\.

  These key\-value pairs define partition parameters\.
+ `LastAnalyzedTime` – Timestamp\.

  The last time at which column statistics were computed for this partition\.

## PartitionInput Structure<a name="aws-glue-api-catalog-partitions-PartitionInput"></a>

The structure used to create and update a partition\.

**Fields**
+ `Values` – An array of UTF\-8 strings\.

  The values of the partition\. Although this parameter is not required by the SDK, you must specify this parameter for a valid input\.

  The values for the keys for the new partition must be passed as an array of String objects that must be ordered in the same order as the partition keys appearing in the Amazon S3 prefix\. Otherwise AWS Glue will add the values to the wrong keys\.
+ `LastAccessTime` – Timestamp\.

  The last time at which the partition was accessed\.
+ `StorageDescriptor` – A [StorageDescriptor](aws-glue-api-catalog-tables.md#aws-glue-api-catalog-tables-StorageDescriptor) object\.

  Provides information about the physical location where the partition is stored\.
+ `Parameters` – A map array of key\-value pairs\.

  Each key is a Key string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Each value is a UTF\-8 string, not more than 512000 bytes long\.

  These key\-value pairs define partition parameters\.
+ `LastAnalyzedTime` – Timestamp\.

  The last time at which column statistics were computed for this partition\.

## PartitionSpecWithSharedStorageDescriptor Structure<a name="aws-glue-api-catalog-partitions-PartitionSpecWithSharedStorageDescriptor"></a>

A partition specification for partitions that share a physical location\.

**Fields**
+ `StorageDescriptor` – A [StorageDescriptor](aws-glue-api-catalog-tables.md#aws-glue-api-catalog-tables-StorageDescriptor) object\.

  The shared physical storage information\.
+ `Partitions` – An array of [Partition](#aws-glue-api-catalog-partitions-Partition) objects\.

  A list of the partitions that share this physical location\.

## PartitionListComposingSpec Structure<a name="aws-glue-api-catalog-partitions-PartitionListComposingSpec"></a>

Lists the related partitions\.

**Fields**
+ `Partitions` – An array of [Partition](#aws-glue-api-catalog-partitions-Partition) objects\.

  A list of the partitions in the composing specification\.

## PartitionSpecProxy Structure<a name="aws-glue-api-catalog-partitions-PartitionSpecProxy"></a>

Provides a root path to specified partitions\.

**Fields**
+ `DatabaseName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The catalog database in which the partitions reside\.
+ `TableName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the table that contains the partitions\.
+ `RootPath` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The root path of the proxy for addressing the partitions\.
+ `PartitionSpecWithSharedSD` – A [PartitionSpecWithSharedStorageDescriptor](#aws-glue-api-catalog-partitions-PartitionSpecWithSharedStorageDescriptor) object\.

  A specification of partitions that share the same physical storage location\.
+ `PartitionListComposingSpec` – A [PartitionListComposingSpec](#aws-glue-api-catalog-partitions-PartitionListComposingSpec) object\.

  Specifies a list of partitions\.

## PartitionValueList Structure<a name="aws-glue-api-catalog-partitions-PartitionValueList"></a>

Contains a list of values defining partitions\.

**Fields**
+ `Values` – *Required:* An array of UTF\-8 strings\.

  The list of values\.

## Segment Structure<a name="aws-glue-api-catalog-partitions-Segment"></a>

Defines a non\-overlapping region of a table's partitions, allowing multiple requests to be executed in parallel\.

**Fields**
+ `SegmentNumber` – *Required:* Number \(integer\), not more than None\.

  The zero\-based index number of the segment\. For example, if the total number of segments is 4, `SegmentNumber` values range from 0 through 3\.
+ `TotalSegments` – *Required:* Number \(integer\), not less than 1 or more than 10\.

  The total number of segments\.

## PartitionError Structure<a name="aws-glue-api-catalog-partitions-PartitionError"></a>

Contains information about a partition error\.

**Fields**
+ `PartitionValues` – An array of UTF\-8 strings\.

  The values that define the partition\.
+ `ErrorDetail` – An [ErrorDetail](aws-glue-api-common.md#aws-glue-api-common-ErrorDetail) object\.

  The details about the partition error\.

## Operations<a name="aws-glue-api-catalog-partitions-actions"></a>
+ [CreatePartition Action \(Python: create\_partition\)](#aws-glue-api-catalog-partitions-CreatePartition)
+ [BatchCreatePartition Action \(Python: batch\_create\_partition\)](#aws-glue-api-catalog-partitions-BatchCreatePartition)
+ [UpdatePartition Action \(Python: update\_partition\)](#aws-glue-api-catalog-partitions-UpdatePartition)
+ [DeletePartition Action \(Python: delete\_partition\)](#aws-glue-api-catalog-partitions-DeletePartition)
+ [BatchDeletePartition Action \(Python: batch\_delete\_partition\)](#aws-glue-api-catalog-partitions-BatchDeletePartition)
+ [GetPartition Action \(Python: get\_partition\)](#aws-glue-api-catalog-partitions-GetPartition)
+ [GetPartitions Action \(Python: get\_partitions\)](#aws-glue-api-catalog-partitions-GetPartitions)
+ [BatchGetPartition Action \(Python: batch\_get\_partition\)](#aws-glue-api-catalog-partitions-BatchGetPartition)

## CreatePartition Action \(Python: create\_partition\)<a name="aws-glue-api-catalog-partitions-CreatePartition"></a>

Creates a new partition\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The AWS account ID of the catalog in which the partition is to be created\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the metadata database in which the partition is to be created\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the metadata table in which the partition is to be created\.
+ `PartitionInput` – *Required:* A [PartitionInput](#aws-glue-api-catalog-partitions-PartitionInput) object\.

  A `PartitionInput` structure defining the partition to be created\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `InvalidInputException`
+ `AlreadyExistsException`
+ `ResourceNumberLimitExceededException`
+ `InternalServiceException`
+ `EntityNotFoundException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`

## BatchCreatePartition Action \(Python: batch\_create\_partition\)<a name="aws-glue-api-catalog-partitions-BatchCreatePartition"></a>

Creates one or more partitions in a batch operation\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the catalog in which the partition is to be created\. Currently, this should be the AWS account ID\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the metadata database in which the partition is to be created\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the metadata table in which the partition is to be created\.
+ `PartitionInputList` – *Required:* An array of [PartitionInput](#aws-glue-api-catalog-partitions-PartitionInput) objects, not more than 100 structures\.

  A list of `PartitionInput` structures that define the partitions to be created\.

**Response**
+ `Errors` – An array of [PartitionError](#aws-glue-api-catalog-partitions-PartitionError) objects\.

  The errors encountered when trying to create the requested partitions\.

**Errors**
+ `InvalidInputException`
+ `AlreadyExistsException`
+ `ResourceNumberLimitExceededException`
+ `InternalServiceException`
+ `EntityNotFoundException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`

## UpdatePartition Action \(Python: update\_partition\)<a name="aws-glue-api-catalog-partitions-UpdatePartition"></a>

Updates a partition\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the partition to be updated resides\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database in which the table in question resides\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the table in which the partition to be updated is located\.
+ `PartitionValueList` – *Required:* An array of UTF\-8 strings, not more than 100 strings\.

  A list of the values defining the partition\.
+ `PartitionInput` – *Required:* A [PartitionInput](#aws-glue-api-catalog-partitions-PartitionInput) object\.

  The new partition object to update the partition to\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`

## DeletePartition Action \(Python: delete\_partition\)<a name="aws-glue-api-catalog-partitions-DeletePartition"></a>

Deletes a specified partition\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the partition to be deleted resides\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database in which the table in question resides\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the table that contains the partition to be deleted\.
+ `PartitionValues` – *Required:* An array of UTF\-8 strings\.

  The values that define the partition\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## BatchDeletePartition Action \(Python: batch\_delete\_partition\)<a name="aws-glue-api-catalog-partitions-BatchDeletePartition"></a>

Deletes one or more partitions in a batch operation\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the partition to be deleted resides\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database in which the table in question resides\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the table that contains the partitions to be deleted\.
+ `PartitionsToDelete` – *Required:* An array of [PartitionValueList](#aws-glue-api-catalog-partitions-PartitionValueList) objects, not more than 25 structures\.

  A list of `PartitionInput` structures that define the partitions to be deleted\.

**Response**
+ `Errors` – An array of [PartitionError](#aws-glue-api-catalog-partitions-PartitionError) objects\.

  The errors encountered when trying to delete the requested partitions\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## GetPartition Action \(Python: get\_partition\)<a name="aws-glue-api-catalog-partitions-GetPartition"></a>

Retrieves information about a specified partition\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the partition in question resides\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database where the partition resides\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the partition's table\.
+ `PartitionValues` – *Required:* An array of UTF\-8 strings\.

  The values that define the partition\.

**Response**
+ `Partition` – A [Partition](#aws-glue-api-catalog-partitions-Partition) object\.

  The requested information, in the form of a `Partition` object\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`

## GetPartitions Action \(Python: get\_partitions\)<a name="aws-glue-api-catalog-partitions-GetPartitions"></a>

Retrieves information about the partitions in a table\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the partitions in question reside\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database where the partitions reside\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the partitions' table\.
+ `Expression` – Predicate string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  An expression that filters the partitions to be returned\.

  The expression uses SQL syntax similar to the SQL `WHERE` filter clause\. The SQL statement parser [JSQLParser](http://jsqlparser.sourceforge.net/home.php) parses the expression\. 

  *Operators*: The following are the operators that you can use in the `Expression` API call:  
=  
Checks whether the values of the two operands are equal; if yes, then the condition becomes true\.  
Example: Assume 'variable a' holds 10 and 'variable b' holds 20\.   
\(a = b\) is not true\.  
< >  
Checks whether the values of two operands are equal; if the values are not equal, then the condition becomes true\.  
Example: \(a < > b\) is true\.  
>  
Checks whether the value of the left operand is greater than the value of the right operand; if yes, then the condition becomes true\.  
Example: \(a > b\) is not true\.  
<  
Checks whether the value of the left operand is less than the value of the right operand; if yes, then the condition becomes true\.  
Example: \(a < b\) is true\.  
>=  
Checks whether the value of the left operand is greater than or equal to the value of the right operand; if yes, then the condition becomes true\.  
Example: \(a >= b\) is not true\.  
<=  
Checks whether the value of the left operand is less than or equal to the value of the right operand; if yes, then the condition becomes true\.  
Example: \(a <= b\) is true\.  
AND, OR, IN, BETWEEN, LIKE, NOT, IS NULL  
Logical operators\.

  *Supported Partition Key Types*: The following are the supported partition keys\.
  + `string`
  + `date`
  + `timestamp`
  + `int`
  + `bigint`
  + `long`
  + `tinyint`
  + `smallint`
  + `decimal`

  If an invalid type is encountered, an exception is thrown\. 

  The following list shows the valid operators on each type\. When you define a crawler, the `partitionKey` type is created as a `STRING`, to be compatible with the catalog partitions\. 

  *Sample API Call*:   
**Example**  

  The table `twitter_partition` has three partitions:

  ```
  year = 2015
          year = 2016
          year = 2017
  ```  
**Example**  

  Get partition `year` equal to 2015

  ```
  aws glue get-partitions --database-name dbname --table-name twitter_partition 
          --expression "year*=*'2015'"
  ```  
**Example**  

  Get partition `year` between 2016 and 2018 \(exclusive\)

  ```
  aws glue get-partitions --database-name dbname --table-name twitter_partition 
          --expression "year>'2016' AND year<'2018'"
  ```  
**Example**  

  Get partition `year` between 2015 and 2018 \(inclusive\)\. The following API calls are equivalent to each other:

  ```
  aws glue get-partitions --database-name dbname --table-name twitter_partition 
          --expression "year>='2015' AND year<='2018'"
          
          aws glue get-partitions --database-name dbname --table-name twitter_partition 
          --expression "year BETWEEN 2015 AND 2018"
          
          aws glue get-partitions --database-name dbname --table-name twitter_partition 
          --expression "year IN (2015,2016,2017,2018)"
  ```  
**Example**  

  A wildcard partition filter, where the following call output is partition year=2017\. A regular expression is not supported in `LIKE`\.

  ```
  aws glue get-partitions --database-name dbname --table-name twitter_partition 
          --expression "year LIKE '%7'"
  ```
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is not the first call to retrieve these partitions\.
+ `Segment` – A [Segment](#aws-glue-api-catalog-partitions-Segment) object\.

  The segment of the table's partitions to scan in this request\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum number of partitions to return in a single response\.

**Response**
+ `Partitions` – An array of [Partition](#aws-glue-api-catalog-partitions-Partition) objects\.

  A list of requested partitions\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if the returned list of partitions does not include the last one\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `OperationTimeoutException`
+ `InternalServiceException`
+ `GlueEncryptionException`

## BatchGetPartition Action \(Python: batch\_get\_partition\)<a name="aws-glue-api-catalog-partitions-BatchGetPartition"></a>

Retrieves partitions in a batch request\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the partitions in question reside\. If none is supplied, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database where the partitions reside\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the partitions' table\.
+ `PartitionsToGet` – *Required:* An array of [PartitionValueList](#aws-glue-api-catalog-partitions-PartitionValueList) objects, not more than 1000 structures\.

  A list of partition values identifying the partitions to retrieve\.

**Response**
+ `Partitions` – An array of [Partition](#aws-glue-api-catalog-partitions-Partition) objects\.

  A list of the requested partitions\.
+ `UnprocessedKeys` – An array of [PartitionValueList](#aws-glue-api-catalog-partitions-PartitionValueList) objects, not more than 1000 structures\.

  A list of the partition values in the request for which partitions were not returned\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `OperationTimeoutException`
+ `InternalServiceException`
+ `GlueEncryptionException`