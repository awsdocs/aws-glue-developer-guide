# Database API<a name="aws-glue-api-catalog-databases"></a>

## Data Types<a name="aws-glue-api-catalog-databases-objects"></a>
+ [Database Structure](#aws-glue-api-catalog-databases-Database)
+ [DatabaseInput Structure](#aws-glue-api-catalog-databases-DatabaseInput)

## Database Structure<a name="aws-glue-api-catalog-databases-Database"></a>

The `Database` object represents a logical grouping of tables that may reside in a Hive metastore or an RDBMS\.

**Fields**
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  Name of the database\. For Hive compatibility, this is folded to lowercase when it is stored\.
+ `Description` – Description string, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Description of the database\.
+ `LocationUri` – Uniform resource identifier \(uri\), matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  The location of the database \(for example, an HDFS path\)\.
+ `Parameters` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\.

  A list of key\-value pairs that define parameters and properties of the database\.
+ `CreateTime` – Timestamp\.

  The time at which the metadata database was created in the catalog\.

## DatabaseInput Structure<a name="aws-glue-api-catalog-databases-DatabaseInput"></a>

The structure used to create or update a database\.

**Fields**
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  Name of the database\. For Hive compatibility, this is folded to lowercase when it is stored\.
+ `Description` – Description string, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Description of the database
+ `LocationUri` – Uniform resource identifier \(uri\), matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  The location of the database \(for example, an HDFS path\)\.
+ `Parameters` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\.

  A list of key\-value pairs that define parameters and properties of the database\.

## Operations<a name="aws-glue-api-catalog-databases-actions"></a>
+ [CreateDatabase Action \(Python: create\_database\)](#aws-glue-api-catalog-databases-CreateDatabase)
+ [UpdateDatabase Action \(Python: update\_database\)](#aws-glue-api-catalog-databases-UpdateDatabase)
+ [DeleteDatabase Action \(Python: delete\_database\)](#aws-glue-api-catalog-databases-DeleteDatabase)
+ [GetDatabase Action \(Python: get\_database\)](#aws-glue-api-catalog-databases-GetDatabase)
+ [GetDatabases Action \(Python: get\_databases\)](#aws-glue-api-catalog-databases-GetDatabases)

## CreateDatabase Action \(Python: create\_database\)<a name="aws-glue-api-catalog-databases-CreateDatabase"></a>

Creates a new database in a Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which to create the database\. If none is supplied, the AWS account ID is used by default\.
+ `DatabaseInput` – A DatabaseInput object\. Required\.

  A `DatabaseInput` object defining the metadata database to create in the catalog\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `InvalidInputException`
+ `AlreadyExistsException`
+ `ResourceNumberLimitExceededException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`

## UpdateDatabase Action \(Python: update\_database\)<a name="aws-glue-api-catalog-databases-UpdateDatabase"></a>

Updates an existing database definition in a Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which the metadata database resides\. If none is supplied, the AWS account ID is used by default\.
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the database to update in the catalog\. For Hive compatibility, this is folded to lowercase\.
+ `DatabaseInput` – A DatabaseInput object\. Required\.

  A `DatabaseInput` object specifying the new definition of the metadata database in the catalog\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`

## DeleteDatabase Action \(Python: delete\_database\)<a name="aws-glue-api-catalog-databases-DeleteDatabase"></a>

Removes a specified Database from a Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which the database resides\. If none is supplied, the AWS account ID is used by default\.
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the Database to delete\. For Hive compatibility, this must be all lowercase\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## GetDatabase Action \(Python: get\_database\)<a name="aws-glue-api-catalog-databases-GetDatabase"></a>

Retrieves the definition of a specified database\.

**Request**
+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which the database resides\. If none is supplied, the AWS account ID is used by default\.
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the database to retrieve\. For Hive compatibility, this should be all lowercase\.

**Response**
+ `Database` – A Database object\.

  The definition of the specified database in the catalog\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`

## GetDatabases Action \(Python: get\_databases\)<a name="aws-glue-api-catalog-databases-GetDatabases"></a>

Retrieves all Databases defined in a given Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog from which to retrieve `Databases`\. If none is supplied, the AWS account ID is used by default\.
+ `NextToken` – String\.

  A continuation token, if this is a continuation call\.
+ `MaxResults` – Number \(integer\)\.

  The maximum number of databases to return in one response\.

**Response**
+ `DatabaseList` – An array of [Database](#aws-glue-api-catalog-databases-Database)s\. Required\.

  A list of `Database` objects from the specified catalog\.
+ `NextToken` – String\.

  A continuation token for paginating the returned list of tokens, returned if the current segment of the list is not the last\.

**Errors**
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`