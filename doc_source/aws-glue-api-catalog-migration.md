# Importing an Athena Catalog to AWS Glue<a name="aws-glue-api-catalog-migration"></a>

The Migration API describes AWS Glue data types and operations having to do with migrating an Athena Data Catalog to AWS Glue\.

## Data Types<a name="aws-glue-api-catalog-migration-objects"></a>
+ [CatalogImportStatus Structure](#aws-glue-api-catalog-migration-CatalogImportStatus)

## CatalogImportStatus Structure<a name="aws-glue-api-catalog-migration-CatalogImportStatus"></a>

A structure containing migration status information\.

**Fields**
+ `ImportCompleted` – Boolean\.

  `True` if the migration has completed, or `False` otherwise\.
+ `ImportTime` – Timestamp\.

  The time that the migration was started\.
+ `ImportedBy` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the person who initiated the migration\.

## Operations<a name="aws-glue-api-catalog-migration-actions"></a>
+ [ImportCatalogToGlue Action \(Python: import\_catalog\_to\_glue\)](#aws-glue-api-catalog-migration-ImportCatalogToGlue)
+ [GetCatalogImportStatus Action \(Python: get\_catalog\_import\_status\)](#aws-glue-api-catalog-migration-GetCatalogImportStatus)

## ImportCatalogToGlue Action \(Python: import\_catalog\_to\_glue\)<a name="aws-glue-api-catalog-migration-ImportCatalogToGlue"></a>

Imports an existing Amazon Athena Data Catalog to AWS Glue

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the catalog to import\. Currently, this should be the AWS account ID\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `InternalServiceException`
+ `OperationTimeoutException`

## GetCatalogImportStatus Action \(Python: get\_catalog\_import\_status\)<a name="aws-glue-api-catalog-migration-GetCatalogImportStatus"></a>

Retrieves the status of a migration operation\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the catalog to migrate\. Currently, this should be the AWS account ID\.

**Response**
+ `ImportStatus` – A [CatalogImportStatus](#aws-glue-api-catalog-migration-CatalogImportStatus) object\.

  The status of the specified catalog migration\.

**Errors**
+ `InternalServiceException`
+ `OperationTimeoutException`