# Table API<a name="aws-glue-api-catalog-tables"></a>

The Table API describes data types and operations associated with tables\.

## Data Types<a name="aws-glue-api-catalog-tables-objects"></a>
+ [Table Structure](#aws-glue-api-catalog-tables-Table)
+ [TableInput Structure](#aws-glue-api-catalog-tables-TableInput)
+ [Column Structure](#aws-glue-api-catalog-tables-Column)
+ [StorageDescriptor Structure](#aws-glue-api-catalog-tables-StorageDescriptor)
+ [SchemaReference Structure](#aws-glue-api-catalog-tables-SchemaReference)
+ [SerDeInfo Structure](#aws-glue-api-catalog-tables-SerDeInfo)
+ [Order Structure](#aws-glue-api-catalog-tables-Order)
+ [SkewedInfo Structure](#aws-glue-api-catalog-tables-SkewedInfo)
+ [TableVersion Structure](#aws-glue-api-catalog-tables-TableVersion)
+ [TableError Structure](#aws-glue-api-catalog-tables-TableError)
+ [TableVersionError Structure](#aws-glue-api-catalog-tables-TableVersionError)
+ [SortCriterion Structure](#aws-glue-api-catalog-tables-SortCriterion)
+ [TableIdentifier Structure](#aws-glue-api-catalog-tables-TableIdentifier)
+ [KeySchemaElement Structure](#aws-glue-api-catalog-tables-KeySchemaElement)
+ [PartitionIndex Structure](#aws-glue-api-catalog-tables-PartitionIndex)
+ [PartitionIndexDescriptor Structure](#aws-glue-api-catalog-tables-PartitionIndexDescriptor)
+ [BackfillError Structure](#aws-glue-api-catalog-tables-BackfillError)

## Table Structure<a name="aws-glue-api-catalog-tables-Table"></a>

Represents a collection of related data organized in columns and rows\.

**Fields**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The table name\. For Hive compatibility, this must be entirely lowercase\.
+ `DatabaseName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the database where the table metadata resides\. For Hive compatibility, this must be all lowercase\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of the table\.
+ `Owner` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The owner of the table\.
+ `CreateTime` – Timestamp\.

  The time when the table definition was created in the Data Catalog\.
+ `UpdateTime` – Timestamp\.

  The last time that the table was updated\.
+ `LastAccessTime` – Timestamp\.

  The last time that the table was accessed\. This is usually taken from HDFS, and might not be reliable\.
+ `LastAnalyzedTime` – Timestamp\.

  The last time that column statistics were computed for this table\.
+ `Retention` – Number \(integer\), not more than None\.

  The retention time for this table\.
+ `StorageDescriptor` – A [StorageDescriptor](#aws-glue-api-catalog-tables-StorageDescriptor) object\.

  A storage descriptor containing information about the physical storage of this table\.
+ `PartitionKeys` – An array of [Column](#aws-glue-api-catalog-tables-Column) objects\.

  A list of columns by which the table is partitioned\. Only primitive types are supported as partition keys\.

  When you create a table used by Amazon Athena, and you do not specify any `partitionKeys`, you must at least set the value of `partitionKeys` to an empty list\. For example:

  `"PartitionKeys": []`
+ `ViewOriginalText` – UTF\-8 string, not more than 409600 bytes long\.

  If the table is a view, the original text of the view; otherwise `null`\.
+ `ViewExpandedText` – UTF\-8 string, not more than 409600 bytes long\.

  If the table is a view, the expanded text of the view; otherwise `null`\.
+ `TableType` – UTF\-8 string, not more than 255 bytes long\.

  The type of this table \(`EXTERNAL_TABLE`, `VIRTUAL_VIEW`, etc\.\)\.
+ `Parameters` – A map array of key\-value pairs\.

  Each key is a Key string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Each value is a UTF\-8 string, not more than 512000 bytes long\.

  These key\-value pairs define properties associated with the table\.
+ `CreatedBy` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The person or entity who created the table\.
+ `IsRegisteredWithLakeFormation` – Boolean\.

  Indicates whether the table has been registered with AWS Lake Formation\.
+ `TargetTable` – A [TableIdentifier](#aws-glue-api-catalog-tables-TableIdentifier) object\.

  A `TableIdentifier` structure that describes a target table for resource linking\.
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which the table resides\.

## TableInput Structure<a name="aws-glue-api-catalog-tables-TableInput"></a>

A structure used to define a table\.

**Fields**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The table name\. For Hive compatibility, this is folded to lowercase when it is stored\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of the table\.
+ `Owner` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The table owner\.
+ `LastAccessTime` – Timestamp\.

  The last time that the table was accessed\.
+ `LastAnalyzedTime` – Timestamp\.

  The last time that column statistics were computed for this table\.
+ `Retention` – Number \(integer\), not more than None\.

  The retention time for this table\.
+ `StorageDescriptor` – A [StorageDescriptor](#aws-glue-api-catalog-tables-StorageDescriptor) object\.

  A storage descriptor containing information about the physical storage of this table\.
+ `PartitionKeys` – An array of [Column](#aws-glue-api-catalog-tables-Column) objects\.

  A list of columns by which the table is partitioned\. Only primitive types are supported as partition keys\.

  When you create a table used by Amazon Athena, and you do not specify any `partitionKeys`, you must at least set the value of `partitionKeys` to an empty list\. For example:

  `"PartitionKeys": []`
+ `ViewOriginalText` – UTF\-8 string, not more than 409600 bytes long\.

  If the table is a view, the original text of the view; otherwise `null`\.
+ `ViewExpandedText` – UTF\-8 string, not more than 409600 bytes long\.

  If the table is a view, the expanded text of the view; otherwise `null`\.
+ `TableType` – UTF\-8 string, not more than 255 bytes long\.

  The type of this table \(`EXTERNAL_TABLE`, `VIRTUAL_VIEW`, etc\.\)\.
+ `Parameters` – A map array of key\-value pairs\.

  Each key is a Key string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Each value is a UTF\-8 string, not more than 512000 bytes long\.

  These key\-value pairs define properties associated with the table\.
+ `TargetTable` – A [TableIdentifier](#aws-glue-api-catalog-tables-TableIdentifier) object\.

  A `TableIdentifier` structure that describes a target table for resource linking\.

## Column Structure<a name="aws-glue-api-catalog-tables-Column"></a>

A column in a `Table`\.

**Fields**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the `Column`\.
+ `Type` – UTF\-8 string, not more than 131072 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The data type of the `Column`\.
+ `Comment` – Comment string, not more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  A free\-form text comment\.
+ `Parameters` – A map array of key\-value pairs\.

  Each key is a Key string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Each value is a UTF\-8 string, not more than 512000 bytes long\.

  These key\-value pairs define properties associated with the column\.

## StorageDescriptor Structure<a name="aws-glue-api-catalog-tables-StorageDescriptor"></a>

Describes the physical storage of table data\.

**Fields**
+ `Columns` – An array of [Column](#aws-glue-api-catalog-tables-Column) objects\.

  A list of the `Columns` in the table\.
+ `Location` – Location string, not more than 2056 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  The physical location of the table\. By default, this takes the form of the warehouse location, followed by the database location in the warehouse, followed by the table name\.
+ `InputFormat` – Format string, not more than 128 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The input format: `SequenceFileInputFormat` \(binary\), or `TextInputFormat`, or a custom format\.
+ `OutputFormat` – Format string, not more than 128 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The output format: `SequenceFileOutputFormat` \(binary\), or `IgnoreKeyTextOutputFormat`, or a custom format\.
+ `Compressed` – Boolean\.

  `True` if the data in the table is compressed, or `False` if not\.
+ `NumberOfBuckets` – Number \(integer\)\.

  Must be specified if the table contains any dimension columns\.
+ `SerdeInfo` – A [SerDeInfo](#aws-glue-api-catalog-tables-SerDeInfo) object\.

  The serialization/deserialization \(SerDe\) information\.
+ `BucketColumns` – An array of UTF\-8 strings\.

  A list of reducer grouping columns, clustering columns, and bucketing columns in the table\.
+ `SortColumns` – An array of [Order](#aws-glue-api-catalog-tables-Order) objects\.

  A list specifying the sort order of each bucket in the table\.
+ `Parameters` – A map array of key\-value pairs\.

  Each key is a Key string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Each value is a UTF\-8 string, not more than 512000 bytes long\.

  The user\-supplied properties in key\-value form\.
+ `SkewedInfo` – A [SkewedInfo](#aws-glue-api-catalog-tables-SkewedInfo) object\.

  The information about values that appear frequently in a column \(skewed values\)\.
+ `StoredAsSubDirectories` – Boolean\.

  `True` if the table data is stored in subdirectories, or `False` if not\.
+ `SchemaReference` – A [SchemaReference](#aws-glue-api-catalog-tables-SchemaReference) object\.

  An object that references a schema stored in the AWS Glue Schema Registry\.

  When creating a table, you can pass an empty list of columns for the schema, and instead use a schema reference\.

## SchemaReference Structure<a name="aws-glue-api-catalog-tables-SchemaReference"></a>

An object that references a schema stored in the AWS Glue Schema Registry\.

**Fields**
+ `SchemaId` – A [SchemaId](aws-glue-api-schema-registry-api.md#aws-glue-api-schema-registry-api-SchemaId) object\.

  A structure that contains schema identity fields\. Either this or the `SchemaVersionId` has to be provided\.
+ `SchemaVersionId` – UTF\-8 string, not less than 36 or more than 36 bytes long, matching the [Custom string pattern #11](aws-glue-api-common.md#regex_11)\.

  The unique ID assigned to a version of the schema\. Either this or the `SchemaId` has to be provided\.
+ `SchemaVersionNumber` – Number \(long\), not less than 1 or more than 100000\.

  The version number of the schema\.

## SerDeInfo Structure<a name="aws-glue-api-catalog-tables-SerDeInfo"></a>

Information about a serialization/deserialization program \(SerDe\) that serves as an extractor and loader\.

**Fields**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the SerDe\.
+ `SerializationLibrary` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Usually the class that implements the SerDe\. An example is `org.apache.hadoop.hive.serde2.columnar.ColumnarSerDe`\.
+ `Parameters` – A map array of key\-value pairs\.

  Each key is a Key string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Each value is a UTF\-8 string, not more than 512000 bytes long\.

  These key\-value pairs define initialization parameters for the SerDe\.

## Order Structure<a name="aws-glue-api-catalog-tables-Order"></a>

Specifies the sort order of a sorted column\.

**Fields**
+ `Column` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the column\.
+ `SortOrder` – *Required:* Number \(integer\), not more than 1\.

  Indicates that the column is sorted in ascending order \(`== 1`\), or in descending order \(`==0`\)\.

## SkewedInfo Structure<a name="aws-glue-api-catalog-tables-SkewedInfo"></a>

Specifies skewed values in a table\. Skewed values are those that occur with very high frequency\.

**Fields**
+ `SkewedColumnNames` – An array of UTF\-8 strings\.

  A list of names of columns that contain skewed values\.
+ `SkewedColumnValues` – An array of UTF\-8 strings\.

  A list of values that appear so frequently as to be considered skewed\.
+ `SkewedColumnValueLocationMaps` – A map array of key\-value pairs\.

  Each key is a UTF\-8 string\.

  Each value is a UTF\-8 string\.

  A mapping of skewed values to the columns that contain them\.

## TableVersion Structure<a name="aws-glue-api-catalog-tables-TableVersion"></a>

Specifies a version of a table\.

**Fields**
+ `Table` – A [Table](#aws-glue-api-catalog-tables-Table) object\.

  The table in question\.
+ `VersionId` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID value that identifies this table version\. A `VersionId` is a string representation of an integer\. Each version is incremented by 1\.

## TableError Structure<a name="aws-glue-api-catalog-tables-TableError"></a>

An error record for table operations\.

**Fields**
+ `TableName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the table\. For Hive compatibility, this must be entirely lowercase\.
+ `ErrorDetail` – An [ErrorDetail](aws-glue-api-common.md#aws-glue-api-common-ErrorDetail) object\.

  The details about the error\.

## TableVersionError Structure<a name="aws-glue-api-catalog-tables-TableVersionError"></a>

An error record for table\-version operations\.

**Fields**
+ `TableName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the table in question\.
+ `VersionId` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID value of the version in question\. A `VersionID` is a string representation of an integer\. Each version is incremented by 1\.
+ `ErrorDetail` – An [ErrorDetail](aws-glue-api-common.md#aws-glue-api-common-ErrorDetail) object\.

  The details about the error\.

## SortCriterion Structure<a name="aws-glue-api-catalog-tables-SortCriterion"></a>

Specifies a field to sort by and a sort order\.

**Fields**
+ `FieldName` – Value string, not more than 1024 bytes long\.

  The name of the field on which to sort\.
+ `Sort` – UTF\-8 string \(valid values: `ASC="ASCENDING"` \| `DESC="DESCENDING"`\)\.

  An ascending or descending sort\.

## TableIdentifier Structure<a name="aws-glue-api-catalog-tables-TableIdentifier"></a>

A structure that describes a target table for resource linking\.

**Fields**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which the table resides\.
+ `DatabaseName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database that contains the target table\.
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the target table\.

## KeySchemaElement Structure<a name="aws-glue-api-catalog-tables-KeySchemaElement"></a>

A partition key pair consisting of a name and a type\.

**Fields**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of a partition key\.
+ `Type` – *Required:* UTF\-8 string, not more than 131072 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The type of a partition key\.

## PartitionIndex Structure<a name="aws-glue-api-catalog-tables-PartitionIndex"></a>

A structure for a partition index\.

**Fields**
+ `Keys` – *Required:* An array of UTF\-8 strings, at least 1 string\.

  The keys for the partition index\.
+ `IndexName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the partition index\.

## PartitionIndexDescriptor Structure<a name="aws-glue-api-catalog-tables-PartitionIndexDescriptor"></a>

A descriptor for a partition index in a table\.

**Fields**
+ `IndexName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the partition index\.
+ `Keys` – *Required:* An array of [KeySchemaElement](#aws-glue-api-catalog-tables-KeySchemaElement) objects, at least 1 structure\.

  A list of one or more keys, as `KeySchemaElement` structures, for the partition index\.
+ `IndexStatus` – *Required:* UTF\-8 string \(valid values: `CREATING` \| `ACTIVE` \| `DELETING` \| `FAILED`\)\.

  The status of the partition index\. 

  The possible statuses are:
  + CREATING: The index is being created\. When an index is in a CREATING state, the index or its table cannot be deleted\.
  + ACTIVE: The index creation succeeds\.
  + FAILED: The index creation fails\. 
  + DELETING: The index is deleted from the list of indexes\.
+ `BackfillErrors` – An array of [BackfillError](#aws-glue-api-catalog-tables-BackfillError) objects\.

  A list of errors that can occur when registering partition indexes for an existing table\.

## BackfillError Structure<a name="aws-glue-api-catalog-tables-BackfillError"></a>

A list of errors that can occur when registering partition indexes for an existing table\.

These errors give the details about why an index registration failed and provide a limited number of partitions in the response, so that you can fix the partitions at fault and try registering the index again\. The most common set of errors that can occur are categorized as follows:
+ EncryptedPartitionError: The partitions are encrypted\.
+ InvalidPartitionTypeDataError: The partition value doesn't match the data type for that partition column\.
+ MissingPartitionValueError: The partitions are encrypted\.
+ UnsupportedPartitionCharacterError: Characters inside the partition value are not supported\. For example: U\+0000 , U\+0001, U\+0002\.
+ InternalError: Any error which does not belong to other error codes\.

**Fields**
+ `Code` – UTF\-8 string \(valid values: `ENCRYPTED_PARTITION_ERROR` \| `INTERNAL_ERROR` \| `INVALID_PARTITION_TYPE_DATA_ERROR` \| `MISSING_PARTITION_VALUE_ERROR` \| `UNSUPPORTED_PARTITION_CHARACTER_ERROR`\)\.

  The error code for an error that occurred when registering partition indexes for an existing table\.
+ `Partitions` – An array of [PartitionValueList](aws-glue-api-catalog-partitions.md#aws-glue-api-catalog-partitions-PartitionValueList) objects\.

  A list of a limited number of partitions in the response\.

## Operations<a name="aws-glue-api-catalog-tables-actions"></a>
+ [CreateTable Action \(Python: create\_table\)](#aws-glue-api-catalog-tables-CreateTable)
+ [UpdateTable Action \(Python: update\_table\)](#aws-glue-api-catalog-tables-UpdateTable)
+ [DeleteTable Action \(Python: delete\_table\)](#aws-glue-api-catalog-tables-DeleteTable)
+ [BatchDeleteTable Action \(Python: batch\_delete\_table\)](#aws-glue-api-catalog-tables-BatchDeleteTable)
+ [GetTable Action \(Python: get\_table\)](#aws-glue-api-catalog-tables-GetTable)
+ [GetTables Action \(Python: get\_tables\)](#aws-glue-api-catalog-tables-GetTables)
+ [GetTableVersion Action \(Python: get\_table\_version\)](#aws-glue-api-catalog-tables-GetTableVersion)
+ [GetTableVersions Action \(Python: get\_table\_versions\)](#aws-glue-api-catalog-tables-GetTableVersions)
+ [DeleteTableVersion Action \(Python: delete\_table\_version\)](#aws-glue-api-catalog-tables-DeleteTableVersion)
+ [BatchDeleteTableVersion Action \(Python: batch\_delete\_table\_version\)](#aws-glue-api-catalog-tables-BatchDeleteTableVersion)
+ [SearchTables Action \(Python: search\_tables\)](#aws-glue-api-catalog-tables-SearchTables)
+ [GetPartitionIndexes Action \(Python: get\_partition\_indexes\)](#aws-glue-api-catalog-tables-GetPartitionIndexes)
+ [CreatePartitionIndex Action \(Python: create\_partition\_index\)](#aws-glue-api-catalog-tables-CreatePartitionIndex)
+ [DeletePartitionIndex Action \(Python: delete\_partition\_index\)](#aws-glue-api-catalog-tables-DeletePartitionIndex)
+ [GetColumnStatisticsForTable Action \(Python: get\_column\_statistics\_for\_table\)](#aws-glue-api-catalog-tables-GetColumnStatisticsForTable)
+ [UpdateColumnStatisticsForTable Action \(Python: update\_column\_statistics\_for\_table\)](#aws-glue-api-catalog-tables-UpdateColumnStatisticsForTable)
+ [DeleteColumnStatisticsForTable Action \(Python: delete\_column\_statistics\_for\_table\)](#aws-glue-api-catalog-tables-DeleteColumnStatisticsForTable)

## CreateTable Action \(Python: create\_table\)<a name="aws-glue-api-catalog-tables-CreateTable"></a>

Creates a new table definition in the Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which to create the `Table`\. If none is supplied, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The catalog database in which to create the new table\. For Hive compatibility, this name is entirely lowercase\.
+ `TableInput` – *Required:* A [TableInput](#aws-glue-api-catalog-tables-TableInput) object\.

  The `TableInput` object that defines the metadata table to create in the catalog\.
+ `PartitionIndexes` – An array of [PartitionIndex](#aws-glue-api-catalog-tables-PartitionIndex) objects, not more than 3 structures\.

  A list of partition indexes, `PartitionIndex` structures, to create in the table\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `AlreadyExistsException`
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `ResourceNumberLimitExceededException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`

## UpdateTable Action \(Python: update\_table\)<a name="aws-glue-api-catalog-tables-UpdateTable"></a>

Updates a metadata table in the Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the table resides\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database in which the table resides\. For Hive compatibility, this name is entirely lowercase\.
+ `TableInput` – *Required:* A [TableInput](#aws-glue-api-catalog-tables-TableInput) object\.

  An updated `TableInput` object to define the metadata table in the catalog\.
+ `SkipArchive` – Boolean\.

  By default, `UpdateTable` always creates an archived version of the table before updating it\. However, if `skipArchive` is set to true, `UpdateTable` does not create the archived version\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `ConcurrentModificationException`
+ `ResourceNumberLimitExceededException`
+ `GlueEncryptionException`

## DeleteTable Action \(Python: delete\_table\)<a name="aws-glue-api-catalog-tables-DeleteTable"></a>

Removes a table definition from the Data Catalog\.

**Note**  
After completing this operation, you no longer have access to the table versions and partitions that belong to the deleted table\. AWS Glue deletes these "orphaned" resources asynchronously in a timely manner, at the discretion of the service\.  
To ensure the immediate deletion of all related resources, before calling `DeleteTable`, use `DeleteTableVersion` or `BatchDeleteTableVersion`, and `DeletePartition` or `BatchDeletePartition`, to delete any resources that belong to the table\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the table resides\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database in which the table resides\. For Hive compatibility, this name is entirely lowercase\.
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the table to be deleted\. For Hive compatibility, this name is entirely lowercase\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## BatchDeleteTable Action \(Python: batch\_delete\_table\)<a name="aws-glue-api-catalog-tables-BatchDeleteTable"></a>

Deletes multiple tables at once\.

**Note**  
After completing this operation, you no longer have access to the table versions and partitions that belong to the deleted table\. AWS Glue deletes these "orphaned" resources asynchronously in a timely manner, at the discretion of the service\.  
To ensure the immediate deletion of all related resources, before calling `BatchDeleteTable`, use `DeleteTableVersion` or `BatchDeleteTableVersion`, and `DeletePartition` or `BatchDeletePartition`, to delete any resources that belong to the table\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the table resides\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database in which the tables to delete reside\. For Hive compatibility, this name is entirely lowercase\.
+ `TablesToDelete` – *Required:* An array of UTF\-8 strings, not more than 100 strings\.

  A list of the table to delete\.

**Response**
+ `Errors` – An array of [TableError](#aws-glue-api-catalog-tables-TableError) objects\.

  A list of errors encountered in attempting to delete the specified tables\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## GetTable Action \(Python: get\_table\)<a name="aws-glue-api-catalog-tables-GetTable"></a>

Retrieves the `Table` definition in a Data Catalog for a specified table\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the table resides\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the database in the catalog in which the table resides\. For Hive compatibility, this name is entirely lowercase\.
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the table for which to retrieve the definition\. For Hive compatibility, this name is entirely lowercase\.

**Response**
+ `Table` – A [Table](#aws-glue-api-catalog-tables-Table) object\.

  The `Table` object that defines the specified table\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`

## GetTables Action \(Python: get\_tables\)<a name="aws-glue-api-catalog-tables-GetTables"></a>

Retrieves the definitions of some or all of the tables in a given `Database`\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the tables reside\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The database in the catalog whose tables to list\. For Hive compatibility, this name is entirely lowercase\.
+ `Expression` – UTF\-8 string, not more than 2048 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  A regular expression pattern\. If present, only those tables whose names match the pattern are returned\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, included if this is a continuation call\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum number of tables to return in a single response\.

**Response**
+ `TableList` – An array of [Table](#aws-glue-api-catalog-tables-Table) objects\.

  A list of the requested `Table` objects\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, present if the current list segment is not the last\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `OperationTimeoutException`
+ `InternalServiceException`
+ `GlueEncryptionException`

## GetTableVersion Action \(Python: get\_table\_version\)<a name="aws-glue-api-catalog-tables-GetTableVersion"></a>

Retrieves a specified version of a table\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the tables reside\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The database in the catalog in which the table resides\. For Hive compatibility, this name is entirely lowercase\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the table\. For Hive compatibility, this name is entirely lowercase\.
+ `VersionId` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID value of the table version to be retrieved\. A `VersionID` is a string representation of an integer\. Each version is incremented by 1\. 

**Response**
+ `TableVersion` – A [TableVersion](#aws-glue-api-catalog-tables-TableVersion) object\.

  The requested table version\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`

## GetTableVersions Action \(Python: get\_table\_versions\)<a name="aws-glue-api-catalog-tables-GetTableVersions"></a>

Retrieves a list of strings that identify available versions of a specified table\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the tables reside\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The database in the catalog in which the table resides\. For Hive compatibility, this name is entirely lowercase\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the table\. For Hive compatibility, this name is entirely lowercase\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is not the first call\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum number of table versions to return in one response\.

**Response**
+ `TableVersions` – An array of [TableVersion](#aws-glue-api-catalog-tables-TableVersion) objects\.

  A list of strings identifying available versions of the specified table\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if the list of available versions does not include the last one\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`

## DeleteTableVersion Action \(Python: delete\_table\_version\)<a name="aws-glue-api-catalog-tables-DeleteTableVersion"></a>

Deletes a specified version of a table\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the tables reside\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The database in the catalog in which the table resides\. For Hive compatibility, this name is entirely lowercase\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the table\. For Hive compatibility, this name is entirely lowercase\.
+ `VersionId` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the table version to be deleted\. A `VersionID` is a string representation of an integer\. Each version is incremented by 1\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## BatchDeleteTableVersion Action \(Python: batch\_delete\_table\_version\)<a name="aws-glue-api-catalog-tables-BatchDeleteTableVersion"></a>

Deletes a specified batch of versions of a table\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the tables reside\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The database in the catalog in which the table resides\. For Hive compatibility, this name is entirely lowercase\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the table\. For Hive compatibility, this name is entirely lowercase\.
+ `VersionIds` – *Required:* An array of UTF\-8 strings, not more than 100 strings\.

  A list of the IDs of versions to be deleted\. A `VersionId` is a string representation of an integer\. Each version is incremented by 1\.

**Response**
+ `Errors` – An array of [TableVersionError](#aws-glue-api-catalog-tables-TableVersionError) objects\.

  A list of errors encountered while trying to delete the specified table versions\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## SearchTables Action \(Python: search\_tables\)<a name="aws-glue-api-catalog-tables-SearchTables"></a>

Searches a set of tables based on properties in the table metadata as well as on the parent database\. You can search against text or filter conditions\. 

You can only get tables that you have access to based on the security policies defined in Lake Formation\. You need at least a read\-only access to the table for it to be returned\. If you do not have access to all the columns in the table, these columns will not be searched against when returning the list of tables back to you\. If you have access to the columns but not the data in the columns, those columns and the associated metadata for those columns will be included in the search\. 

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  A unique identifier, consisting of `account_id`\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, included if this is a continuation call\.
+ `Filters` – An array of [PropertyPredicate](aws-glue-api-common.md#aws-glue-api-common-PropertyPredicate) objects\.

  A list of key\-value pairs, and a comparator used to filter the search results\. Returns all entities matching the predicate\.

  The `Comparator` member of the `PropertyPredicate` struct is used only for time fields, and can be omitted for other field types\. Also, when comparing string values, such as when `Key=Name`, a fuzzy match algorithm is used\. The `Key` field \(for example, the value of the `Name` field\) is split on certain punctuation characters, for example, \-, :, \#, etc\. into tokens\. Then each token is exact\-match compared with the `Value` member of `PropertyPredicate`\. For example, if `Key=Name` and `Value=link`, tables named `customer-link` and `xx-link-yy` are returned, but `xxlinkyy` is not returned\.
+ `SearchText` – Value string, not more than 1024 bytes long\.

  A string used for a text search\.

  Specifying a value in quotes filters based on an exact match to the value\.
+ `SortCriteria` – An array of [SortCriterion](#aws-glue-api-catalog-tables-SortCriterion) objects, not more than 1 structures\.

  A list of criteria for sorting the results by a field name, in an ascending or descending order\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum number of tables to return in a single response\.
+ `ResourceShareType` – UTF\-8 string \(valid values: `FOREIGN` \| `ALL`\)\.

  Allows you to specify that you want to search the tables shared with your account\. The allowable values are `FOREIGN` or `ALL`\. 
  + If set to `FOREIGN`, will search the tables shared with your account\. 
  + If set to `ALL`, will search the tables shared with your account, as well as the tables in yor local account\. 

**Response**
+ `NextToken` – UTF\-8 string\.

  A continuation token, present if the current list segment is not the last\.
+ `TableList` – An array of [Table](#aws-glue-api-catalog-tables-Table) objects\.

  A list of the requested `Table` objects\. The `SearchTables` response returns only the tables that you have access to\.

**Errors**
+ `InternalServiceException`
+ `InvalidInputException`
+ `OperationTimeoutException`

## GetPartitionIndexes Action \(Python: get\_partition\_indexes\)<a name="aws-glue-api-catalog-tables-GetPartitionIndexes"></a>

Retrieves the partition indexes associated with a table\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The catalog ID where the table resides\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Specifies the name of a database from which you want to retrieve partition indexes\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Specifies the name of a table for which you want to retrieve the partition indexes\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, included if this is a continuation call\.

**Response**
+ `PartitionIndexDescriptorList` – An array of [PartitionIndexDescriptor](#aws-glue-api-catalog-tables-PartitionIndexDescriptor) objects\.

  A list of index descriptors\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, present if the current list segment is not the last\.

**Errors**
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `ConflictException`

## CreatePartitionIndex Action \(Python: create\_partition\_index\)<a name="aws-glue-api-catalog-tables-CreatePartitionIndex"></a>

Creates a specified partition index in an existing table\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The catalog ID where the table resides\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Specifies the name of a database in which you want to create a partition index\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Specifies the name of a table in which you want to create a partition index\.
+ `PartitionIndex` – *Required:* A [PartitionIndex](#aws-glue-api-catalog-tables-PartitionIndex) object\.

  Specifies a `PartitionIndex` structure to create a partition index in an existing table\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `AlreadyExistsException`
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `ResourceNumberLimitExceededException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`

## DeletePartitionIndex Action \(Python: delete\_partition\_index\)<a name="aws-glue-api-catalog-tables-DeletePartitionIndex"></a>

Deletes a specified partition index from an existing table\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The catalog ID where the table resides\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Specifies the name of a database from which you want to delete a partition index\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Specifies the name of a table from which you want to delete a partition index\.
+ `IndexName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the partition index to be deleted\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `ConflictException`
+ `GlueEncryptionException`

## GetColumnStatisticsForTable Action \(Python: get\_column\_statistics\_for\_table\)<a name="aws-glue-api-catalog-tables-GetColumnStatisticsForTable"></a>

Retrieves table statistics of columns\.

The Identity and Access Management \(IAM\) permission required for this operation is `GetTable`\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the partitions in question reside\. If none is supplied, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database where the partitions reside\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the partitions' table\.
+ `ColumnNames` – *Required:* An array of UTF\-8 strings, not more than 100 strings\.

  A list of the column names\.

**Response**
+ `ColumnStatisticsList` – An array of [ColumnStatistics](aws-glue-api-common.md#aws-glue-api-common-ColumnStatistics) objects\.

  List of ColumnStatistics that failed to be retrieved\.
+ `Errors` – An array of [ColumnError](aws-glue-api-common.md#aws-glue-api-common-ColumnError) objects\.

  List of ColumnStatistics that failed to be retrieved\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`

## UpdateColumnStatisticsForTable Action \(Python: update\_column\_statistics\_for\_table\)<a name="aws-glue-api-catalog-tables-UpdateColumnStatisticsForTable"></a>

Creates or updates table statistics of columns\.

The Identity and Access Management \(IAM\) permission required for this operation is `UpdateTable`\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the partitions in question reside\. If none is supplied, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database where the partitions reside\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the partitions' table\.
+ `ColumnStatisticsList` – *Required:* An array of [ColumnStatistics](aws-glue-api-common.md#aws-glue-api-common-ColumnStatistics) objects, not more than 25 structures\.

  A list of the column statistics\.

**Response**
+ `Errors` – An array of [ColumnStatisticsError](aws-glue-api-common.md#aws-glue-api-common-ColumnStatisticsError) objects\.

  List of ColumnStatisticsErrors\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`

## DeleteColumnStatisticsForTable Action \(Python: delete\_column\_statistics\_for\_table\)<a name="aws-glue-api-catalog-tables-DeleteColumnStatisticsForTable"></a>

Retrieves table statistics of columns\.

The Identity and Access Management \(IAM\) permission required for this operation is `DeleteTable`\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the partitions in question reside\. If none is supplied, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database where the partitions reside\.
+ `TableName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the partitions' table\.
+ `ColumnName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the column\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`