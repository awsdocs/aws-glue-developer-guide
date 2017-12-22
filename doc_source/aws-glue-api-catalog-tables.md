# Table API<a name="aws-glue-api-catalog-tables"></a>

## Data Types<a name="aws-glue-api-catalog-tables-objects"></a>

+ [Table Structure](#aws-glue-api-catalog-tables-Table)

+ [TableInput Structure](#aws-glue-api-catalog-tables-TableInput)

+ [Column Structure](#aws-glue-api-catalog-tables-Column)

+ [StorageDescriptor Structure](#aws-glue-api-catalog-tables-StorageDescriptor)

+ [SerDeInfo Structure](#aws-glue-api-catalog-tables-SerDeInfo)

+ [Order Structure](#aws-glue-api-catalog-tables-Order)

+ [SkewedInfo Structure](#aws-glue-api-catalog-tables-SkewedInfo)

+ [TableVersion Structure](#aws-glue-api-catalog-tables-TableVersion)

+ [TableError Structure](#aws-glue-api-catalog-tables-TableError)

## Table Structure<a name="aws-glue-api-catalog-tables-Table"></a>

Represents a collection of related data organized in columns and rows\.

**Fields**

+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  Name of the table\. For Hive compatibility, this must be entirely lowercase\.

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the metadata database where the table metadata resides\. For Hive compatibility, this must be all lowercase\.

+ `Description` – Description string, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Description of the table\.

+ `Owner` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Owner of the table\.

+ `CreateTime` – Timestamp\.

  Time when the table definition was created in the Data Catalog\.

+ `UpdateTime` – Timestamp\.

  Last time the table was updated\.

+ `LastAccessTime` – Timestamp\.

  Last time the table was accessed\. This is usually taken from HDFS, and may not be reliable\.

+ `LastAnalyzedTime` – Timestamp\.

  Last time column statistics were computed for this table\.

+ `Retention` – Number \(integer\)\.

  Retention time for this table\.

+ `StorageDescriptor` – A StorageDescriptor object\.

  A storage descriptor containing information about the physical storage of this table\.

+ `PartitionKeys` – An array of [Column](#aws-glue-api-catalog-tables-Column)s\.

  A list of columns by which the table is partitioned\. Only primitive types are supported as partition keys\.

+ `ViewOriginalText` – String\.

  If the table is a view, the original text of the view; otherwise `null`\.

+ `ViewExpandedText` – String\.

  If the table is a view, the expanded text of the view; otherwise `null`\.

+ `TableType` – String\.

  The type of this table \(`EXTERNAL_TABLE`, `VIRTUAL_VIEW`, etc\.\)\.

+ `Parameters` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\.

  Properties associated with this table, as a list of key\-value pairs\.

+ `CreatedBy` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Person or entity who created the table\.

## TableInput Structure<a name="aws-glue-api-catalog-tables-TableInput"></a>

Structure used to create or update the table\.

**Fields**

+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  Name of the table\. For Hive compatibility, this is folded to lowercase when it is stored\.

+ `Description` – Description string, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Description of the table\.

+ `Owner` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Owner of the table\.

+ `LastAccessTime` – Timestamp\.

  Last time the table was accessed\.

+ `LastAnalyzedTime` – Timestamp\.

  Last time column statistics were computed for this table\.

+ `Retention` – Number \(integer\)\.

  Retention time for this table\.

+ `StorageDescriptor` – A StorageDescriptor object\.

  A storage descriptor containing information about the physical storage of this table\.

+ `PartitionKeys` – An array of [Column](#aws-glue-api-catalog-tables-Column)s\.

  A list of columns by which the table is partitioned\. Only primitive types are supported as partition keys\.

+ `ViewOriginalText` – String\.

  If the table is a view, the original text of the view; otherwise `null`\.

+ `ViewExpandedText` – String\.

  If the table is a view, the expanded text of the view; otherwise `null`\.

+ `TableType` – String\.

  The type of this table \(`EXTERNAL_TABLE`, `VIRTUAL_VIEW`, etc\.\)\.

+ `Parameters` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\.

  Properties associated with this table, as a list of key\-value pairs\.

## Column Structure<a name="aws-glue-api-catalog-tables-Column"></a>

A column in a `Table`\.

**Fields**

+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the `Column`\.

+ `Type` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The datatype of data in the `Column`\.

+ `Comment` – Comment string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Free\-form text comment\.

## StorageDescriptor Structure<a name="aws-glue-api-catalog-tables-StorageDescriptor"></a>

Describes the physical storage of table data\.

**Fields**

+ `Columns` – An array of [Column](#aws-glue-api-catalog-tables-Column)s\.

  A list of the `Columns` in the table\.

+ `Location` – Location string, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  The physical location of the table\. By default this takes the form of the warehouse location, followed by the database location in the warehouse, followed by the table name\.

+ `InputFormat` – Format string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The input format: `SequenceFileInputFormat` \(binary\), or `TextInputFormat`, or a custom format\.

+ `OutputFormat` – Format string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The output format: `SequenceFileOutputFormat` \(binary\), or `IgnoreKeyTextOutputFormat`, or a custom format\.

+ `Compressed` – Boolean\.

  True if the data in the table is compressed, or False if not\.

+ `NumberOfBuckets` – Number \(integer\)\.

  Must be specified if the table contains any dimension columns\.

+ `SerdeInfo` – A SerDeInfo object\.

  Serialization/deserialization \(SerDe\) information\.

+ `BucketColumns` – An array of UTF\-8 strings\.

  A list of reducer grouping columns, clustering columns, and bucketing columns in the table\.

+ `SortColumns` – An array of [Order](#aws-glue-api-catalog-tables-Order)s\.

  A list specifying the sort order of each bucket in the table\.

+ `Parameters` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\.

  User\-supplied properties in key\-value form\.

+ `SkewedInfo` – A SkewedInfo object\.

  Information about values that appear very frequently in a column \(skewed values\)\.

+ `StoredAsSubDirectories` – Boolean\.

  True if the table data is stored in subdirectories, or False if not\.

## SerDeInfo Structure<a name="aws-glue-api-catalog-tables-SerDeInfo"></a>

Information about a serialization/deserialization program \(SerDe\) which serves as an extractor and loader\.

**Fields**

+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the SerDe\.

+ `SerializationLibrary` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Usually the class that implements the SerDe\. An example is: `org.apache.hadoop.hive.serde2.columnar.ColumnarSerDe`\.

+ `Parameters` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\.

  A list of initialization parameters for the SerDe, in key\-value form\.

## Order Structure<a name="aws-glue-api-catalog-tables-Order"></a>

Specifies the sort order of a sorted column\.

**Fields**

+ `Column` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the column\.

+ `SortOrder` – Number \(integer\)\. Required\.

  Indicates that the column is sorted in ascending order \(`== 1`\), or in descending order \(`==0`\)\.

## SkewedInfo Structure<a name="aws-glue-api-catalog-tables-SkewedInfo"></a>

Specifies skewed values in a table\. Skewed are ones that occur with very high frequency\.

**Fields**

+ `SkewedColumnNames` – An array of UTF\-8 strings\.

  A list of names of columns that contain skewed values\.

+ `SkewedColumnValues` – An array of UTF\-8 strings\.

  A list of values that appear so frequently as to be considered skewed\.

+ `SkewedColumnValueLocationMaps` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\.

  A mapping of skewed values to the columns that contain them\.

## TableVersion Structure<a name="aws-glue-api-catalog-tables-TableVersion"></a>

Specifies a version of a table\.

**Fields**

+ `Table` – A Table object\.

  The table in question

+ `VersionId` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID value that identifies this table version\.

## TableError Structure<a name="aws-glue-api-catalog-tables-TableError"></a>

An error record for table operations\.

**Fields**

+ `TableName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the table\. For Hive compatibility, this must be entirely lowercase\.

+ `ErrorDetail` – An ErrorDetail object\.

  Detail about the error\.

## Operations<a name="aws-glue-api-catalog-tables-actions"></a>

+ [CreateTable Action \(Python: create\_table\)](#aws-glue-api-catalog-tables-CreateTable)

+ [UpdateTable Action \(Python: update\_table\)](#aws-glue-api-catalog-tables-UpdateTable)

+ [DeleteTable Action \(Python: delete\_table\)](#aws-glue-api-catalog-tables-DeleteTable)

+ [BatchDeleteTable Action \(Python: batch\_delete\_table\)](#aws-glue-api-catalog-tables-BatchDeleteTable)

+ [GetTable Action \(Python: get\_table\)](#aws-glue-api-catalog-tables-GetTable)

+ [GetTables Action \(Python: get\_tables\)](#aws-glue-api-catalog-tables-GetTables)

+ [GetTableVersions Action \(Python: get\_table\_versions\)](#aws-glue-api-catalog-tables-GetTableVersions)

## CreateTable Action \(Python: create\_table\)<a name="aws-glue-api-catalog-tables-CreateTable"></a>

Creates a new table definition in the Data Catalog\.

**Request**

+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which to create the `Table`\. If none is supplied, the AWS account ID is used by default\.

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The catalog database in which to create the new table\. For Hive compatibility, this name is entirely lowercase\.

+ `TableInput` – A TableInput object\. Required\.

  The `TableInput` object that defines the metadata table to create in the catalog\.

**Response**

+ *No Response parameters\.*

**Errors**

+ `AlreadyExistsException`

+ `InvalidInputException`

+ `EntityNotFoundException`

+ `ResourceNumberLimitExceededException`

+ `InternalServiceException`

+ `OperationTimeoutException`

Related Hive DDL:

```
CREATE [TEMPORARY] [EXTERNAL] TABLE [IF NOT EXISTS] [database_name.]table_name
      -- (Note: TEMPORARY available in Hive 0.14.0 and later)
      [(col_name data_type [COMMENT col_comment], ...)]
      [COMMENT table_comment]
      [PARTITIONED BY (col_name data_type [COMMENT col_comment], ...)]
      [CLUSTERED BY (col_name, col_name, ...)
      [SORTED BY (col_name [ASC|DESC], ...)] INTO num_buckets BUCKETS]
      [SKEWED BY (col_name, col_name, ...)
      -- (Note: Available in Hive 0.10.0 and later)]
      ON ((col_value, col_value, ...), (col_value, col_value, ...), ...)
      [STORED AS DIRECTORIES]
      [
      [ROW FORMAT row_format]
      [STORED AS file_format]
      | STORED BY 'storage.handler.class.name' [WITH SERDEPROPERTIES (...)]
      -- (Note: Available in Hive 0.6.0 and later)
      ]
      [LOCATION hdfs_path]
      [TBLPROPERTIES (property_name=property_value, ...)]
      -- (Note: Available in Hive 0.6.0 and later)
      [AS select_statement];
      -- (Note: Available in Hive 0.5.0 and later; not supported for external tables)
    
      CREATE [TEMPORARY] [EXTERNAL] TABLE [IF NOT EXISTS] [db_name.]table_name
      LIKE existing_table_or_view_name
      [LOCATION hdfs_path];
    
      row_format
      : DELIMITED [FIELDS TERMINATED BY char [ESCAPED BY char]]
      [COLLECTION ITEMS TERMINATED BY char]
      [MAP KEYS TERMINATED BY char] [LINES TERMINATED BY char]
      [NULL DEFINED AS char]   -- (Note: Available in Hive 0.13 and later)
      | SERDE serde_name
      [WITH SERDEPROPERTIES (property_name=property_value, property_name=property_value, ...)]
    
      file_format:
      : SEQUENCEFILE
      | TEXTFILE    -- (Default, depending on hive.default.fileformat configuration)
      | RCFILE      -- (Note: Available in Hive 0.6.0 and later)
      | ORC         -- (Note: Available in Hive 0.11.0 and later)
      | PARQUET     -- (Note: Available in Hive 0.13.0 and later)
      | AVRO        -- (Note: Available in Hive 0.14.0 and later)
      | INPUTFORMAT input_format_classname OUTPUTFORMAT output_format_classname
```

## UpdateTable Action \(Python: update\_table\)<a name="aws-glue-api-catalog-tables-UpdateTable"></a>

Updates a metadata table in the Data Catalog\.

**Request**

+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the table resides\. If none is supplied, the AWS account ID is used by default\.

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the catalog database in which the table resides\. For Hive compatibility, this name is entirely lowercase\.

+ `TableInput` – A TableInput object\. Required\.

  An updated `TableInput` object to define the metadata table in the catalog\.

**Response**

+ *No Response parameters\.*

**Errors**

+ `EntityNotFoundException`

+ `InvalidInputException`

+ `InternalServiceException`

+ `OperationTimeoutException`

+ `ConcurrentModificationException`

+ `ResourceNumberLimitExceededException`

## DeleteTable Action \(Python: delete\_table\)<a name="aws-glue-api-catalog-tables-DeleteTable"></a>

Removes a table definition from the Data Catalog\.

**Request**

+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the table resides\. If none is supplied, the AWS account ID is used by default\.

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the catalog database in which the table resides\. For Hive compatibility, this name is entirely lowercase\.

+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

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

**Request**

+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the table resides\. If none is supplied, the AWS account ID is used by default\.

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the catalog database where the tables to delete reside\. For Hive compatibility, this name is entirely lowercase\.

+ `TablesToDelete` – An array of UTF\-8 strings\. Required\.

  A list of the table to delete\.

**Response**

+ `Errors` – An array of [TableError](#aws-glue-api-catalog-tables-TableError)s\.

  A list of errors encountered in attempting to delete the specified tables\.

**Errors**

+ `InvalidInputException`

+ `EntityNotFoundException`

+ `InternalServiceException`

+ `OperationTimeoutException`

## GetTable Action \(Python: get\_table\)<a name="aws-glue-api-catalog-tables-GetTable"></a>

Retrieves the `Table` definition in a Data Catalog for a specified table\.

**Request**

+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the table resides\. If none is supplied, the AWS account ID is used by default\.

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the database in the catalog in which the table resides\. For Hive compatibility, this name is entirely lowercase\.

+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the table for which to retrieve the definition\. For Hive compatibility, this name is entirely lowercase\.

**Response**

+ `Table` – A Table object\.

  The `Table` object that defines the specified table\.

**Errors**

+ `EntityNotFoundException`

+ `InvalidInputException`

+ `InternalServiceException`

+ `OperationTimeoutException`

## GetTables Action \(Python: get\_tables\)<a name="aws-glue-api-catalog-tables-GetTables"></a>

Retrieves the definitions of some or all of the tables in a given `Database`\.

**Request**

+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the tables reside\. If none is supplied, the AWS account ID is used by default\.

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The database in the catalog whose tables to list\. For Hive compatibility, this name is entirely lowercase\.

+ `Expression` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  A regular expression pattern\. If present, only those tables whose names match the pattern are returned\.

+ `NextToken` – String\.

  A continuation token, included if this is a continuation call\.

+ `MaxResults` – Number \(integer\)\.

  The maximum number of tables to return in a single response\.

**Response**

+ `TableList` – An array of [Table](#aws-glue-api-catalog-tables-Table)s\.

  A list of the requested `Table` objects\.

+ `NextToken` – String\.

  A continuation token, present if the current list segment is not the last\.

**Errors**

+ `EntityNotFoundException`

+ `InvalidInputException`

+ `OperationTimeoutException`

+ `InternalServiceException`

## GetTableVersions Action \(Python: get\_table\_versions\)<a name="aws-glue-api-catalog-tables-GetTableVersions"></a>

Retrieves a list of strings that identify available versions of a specified table\.

**Request**

+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the tables reside\. If none is supplied, the AWS account ID is used by default\.

+ `DatabaseName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The database in the catalog in which the table resides\. For Hive compatibility, this name is entirely lowercase\.

+ `TableName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the table\. For Hive compatibility, this name is entirely lowercase\.

+ `NextToken` – String\.

  A continuation token, if this is not the first call\.

+ `MaxResults` – Number \(integer\)\.

  The maximum number of table versions to return in one response\.

**Response**

+ `TableVersions` – An array of [TableVersion](#aws-glue-api-catalog-tables-TableVersion)s\.

  A list of strings identifying available versions of the specified table\.

+ `NextToken` – String\.

  A continuation token, if the list of available versions does not include the last one\.

**Errors**

+ `EntityNotFoundException`

+ `InvalidInputException`

+ `InternalServiceException`

+ `OperationTimeoutException`