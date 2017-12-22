# Partition API<a name="aws-glue-api-catalog-partitions"></a>

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

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database where the table in question is located\.

+ `TableName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the table in question\.

+ `CreationTime` – Timestamp\.

  The time at which the partition was created\.

+ `LastAccessTime` – Timestamp\.

  The last time at which the partition was accessed\.

+ `StorageDescriptor` – A StorageDescriptor object\.

  Provides information about the physical location where the partition is stored\.

+ `Parameters` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\.

  Partition parameters, in the form of a list of key\-value pairs\.

+ `LastAnalyzedTime` – Timestamp\.

  The last time at which column statistics were computed for this partition\.

## PartitionInput Structure<a name="aws-glue-api-catalog-partitions-PartitionInput"></a>

The structure used to create and update a partion\.

**Fields**

+ `Values` – An array of UTF\-8 strings\.

  The values of the partition\.

+ `LastAccessTime` – Timestamp\.

  The last time at which the partition was accessed\.

+ `StorageDescriptor` – A StorageDescriptor object\.

  Provides information about the physical location where the partition is stored\.

+ `Parameters` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\.

  Partition parameters, in the form of a list of key\-value pairs\.

+ `LastAnalyzedTime` – Timestamp\.

  The last time at which column statistics were computed for this partition\.

## PartitionSpecWithSharedStorageDescriptor Structure<a name="aws-glue-api-catalog-partitions-PartitionSpecWithSharedStorageDescriptor"></a>

A partition specification for partitions that share a physical location\.

**Fields**

+ `StorageDescriptor` – A StorageDescriptor object\.

  The shared physical storage information\.

+ `Partitions` – An array of [Partition](#aws-glue-api-catalog-partitions-Partition)s\.

  A list of the partitions that share this physical location\.

## PartitionListComposingSpec Structure<a name="aws-glue-api-catalog-partitions-PartitionListComposingSpec"></a>

Lists related partitions\.

**Fields**

+ `Partitions` – An array of [Partition](#aws-glue-api-catalog-partitions-Partition)s\.

  A list of the partitions in the composing specification\.

## PartitionSpecProxy Structure<a name="aws-glue-api-catalog-partitions-PartitionSpecProxy"></a>

Provides a root path to specified partitions\.

**Fields**

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The catalog database in which the partions reside\.

+ `TableName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the table containing the partitions\.

+ `RootPath` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The root path of the proxy for addressing the partitions\.

+ `PartitionSpecWithSharedSD` – A PartitionSpecWithSharedStorageDescriptor object\.

  A specification of partitions that share the same physical storage location\.

+ `PartitionListComposingSpec` – A PartitionListComposingSpec object\.

  Specifies a list of partitions\.

## PartitionValueList Structure<a name="aws-glue-api-catalog-partitions-PartitionValueList"></a>

Contains a list of values defining partitions\.

**Fields**

+ `Values` – An array of UTF\-8 strings\. Required\.

  The list of values\.

## Segment Structure<a name="aws-glue-api-catalog-partitions-Segment"></a>

Defines a non\-overlapping region of a table's partitions, allowing multiple requests to be executed in parallel\.

**Fields**

+ `SegmentNumber` – Number \(integer\)\. Required\.

  The zero\-based index number of the this segment\. For example, if the total number of segments is 4, SegmentNumber values will range from zero through three\.

+ `TotalSegments` – Number \(integer\)\. Required\.

  The total numer of segments\.

## PartitionError Structure<a name="aws-glue-api-catalog-partitions-PartitionError"></a>

Contains information about a partition error\.

**Fields**

+ `PartitionValues` – An array of UTF\-8 strings\.

  The values that define the partition\.

+ `ErrorDetail` – An ErrorDetail object\.

  Details about the partition error\.

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

+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the catalog in which the partion is to be created\. Currently, this should be the AWS account ID\.

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the metadata database in which the partition is to be created\.

+ `TableName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the metadata table in which the partition is to be created\.

+ `PartitionInput` – A PartitionInput object\. Required\.

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

## BatchCreatePartition Action \(Python: batch\_create\_partition\)<a name="aws-glue-api-catalog-partitions-BatchCreatePartition"></a>

Creates one or more partitions in a batch operation\.

**Request**

+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the catalog in which the partion is to be created\. Currently, this should be the AWS account ID\.

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the metadata database in which the partition is to be created\.

+ `TableName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the metadata table in which the partition is to be created\.

+ `PartitionInputList` – An array of [PartitionInput](#aws-glue-api-catalog-partitions-PartitionInput)s\. Required\.

  A list of `PartitionInput` structures that define the partitions to be created\.

**Response**

+ `Errors` – An array of [PartitionError](#aws-glue-api-catalog-partitions-PartitionError)s\.

  Errors encountered when trying to create the requested partitions\.

**Errors**

+ `InvalidInputException`

+ `AlreadyExistsException`

+ `ResourceNumberLimitExceededException`

+ `InternalServiceException`

+ `EntityNotFoundException`

+ `OperationTimeoutException`

## UpdatePartition Action \(Python: update\_partition\)<a name="aws-glue-api-catalog-partitions-UpdatePartition"></a>

Updates a partition\.

**Request**

+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the partition to be updated resides\. If none is supplied, the AWS account ID is used by default\.

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the catalog database in which the table in question resides\.

+ `TableName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the table where the partition to be updated is located\.

+ `PartitionValueList` – An array of UTF\-8 strings\. Required\.

  A list of the values defining the partition\.

+ `PartitionInput` – A PartitionInput object\. Required\.

  The new partition object to which to update the partition\.

**Response**

+ *No Response parameters\.*

**Errors**

+ `EntityNotFoundException`

+ `InvalidInputException`

+ `InternalServiceException`

+ `OperationTimeoutException`

## DeletePartition Action \(Python: delete\_partition\)<a name="aws-glue-api-catalog-partitions-DeletePartition"></a>

Deletes a specified partition\.

**Request**

+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the partition to be deleted resides\. If none is supplied, the AWS account ID is used by default\.

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the catalog database in which the table in question resides\.

+ `TableName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the table where the partition to be deleted is located\.

+ `PartitionValues` – An array of UTF\-8 strings\. Required\.

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

+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the partition to be deleted resides\. If none is supplied, the AWS account ID is used by default\.

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the catalog database in which the table in question resides\.

+ `TableName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the table where the partitions to be deleted is located\.

+ `PartitionsToDelete` – An array of [PartitionValueList](#aws-glue-api-catalog-partitions-PartitionValueList)s\. Required\.

  A list of `PartitionInput` structures that define the partitions to be deleted\.

**Response**

+ `Errors` – An array of [PartitionError](#aws-glue-api-catalog-partitions-PartitionError)s\.

  Errors encountered when trying to delete the requested partitions\.

**Errors**

+ `InvalidInputException`

+ `EntityNotFoundException`

+ `InternalServiceException`

+ `OperationTimeoutException`

## GetPartition Action \(Python: get\_partition\)<a name="aws-glue-api-catalog-partitions-GetPartition"></a>

Retrieves information about a specified partition\.

**Request**

+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the partition in question resides\. If none is supplied, the AWS account ID is used by default\.

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the catalog database where the partition resides\.

+ `TableName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the partition's table\.

+ `PartitionValues` – An array of UTF\-8 strings\. Required\.

  The values that define the partition\.

**Response**

+ `Partition` – A Partition object\.

  The requested information, in the form of a `Partition` object\.

**Errors**

+ `EntityNotFoundException`

+ `InvalidInputException`

+ `InternalServiceException`

+ `OperationTimeoutException`

## GetPartitions Action \(Python: get\_partitions\)<a name="aws-glue-api-catalog-partitions-GetPartitions"></a>

Retrieves information about the partitions in a table\.

**Request**

+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the partitions in question reside\. If none is supplied, the AWS account ID is used by default\.

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the catalog database where the partitions reside\.

+ `TableName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the partitions' table\.

+ `Expression` – Predicate string, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  An expression filtering the partitions to be returned\.

+ `NextToken` – String\.

  A continuation token, if this is not the first call to retrieve these partitions\.

+ `Segment` – A Segment object\.

  The segment of the table's partitions to scan in this request\.

+ `MaxResults` – Number \(integer\)\.

  The maximum number of partitions to return in a single response\.

**Response**

+ `Partitions` – An array of [Partition](#aws-glue-api-catalog-partitions-Partition)s\.

  A list of requested partitions\.

+ `NextToken` – String\.

  A continuation token, if the returned list of partitions does not does not include the last one\.

**Errors**

+ `EntityNotFoundException`

+ `InvalidInputException`

+ `OperationTimeoutException`

+ `InternalServiceException`

## BatchGetPartition Action \(Python: batch\_get\_partition\)<a name="aws-glue-api-catalog-partitions-BatchGetPartition"></a>

Retrieves partitions in a batch request\.

**Request**

+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the partitions in question reside\. If none is supplied, the AWS account ID is used by default\.

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the catalog database where the partitions reside\.

+ `TableName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the partitions' table\.

+ `PartitionsToGet` – An array of [PartitionValueList](#aws-glue-api-catalog-partitions-PartitionValueList)s\. Required\.

  A list of partition values identifying the partitions to retrieve\.

**Response**

+ `Partitions` – An array of [Partition](#aws-glue-api-catalog-partitions-Partition)s\.

  A list of the requested partitions\.

+ `UnprocessedKeys` – An array of [PartitionValueList](#aws-glue-api-catalog-partitions-PartitionValueList)s\.

  A list of the partition values in the request for which partions were not returned\.

**Errors**

+ `InvalidInputException`

+ `EntityNotFoundException`

+ `OperationTimeoutException`

+ `InternalServiceException`