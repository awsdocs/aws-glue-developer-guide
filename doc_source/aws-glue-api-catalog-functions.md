# User\-Defined Function API<a name="aws-glue-api-catalog-functions"></a>

The User\-Defined Function API describes AWS Glue data types and operations used in working with functions\.

## Data Types<a name="aws-glue-api-catalog-functions-objects"></a>
+ [UserDefinedFunction Structure](#aws-glue-api-catalog-functions-UserDefinedFunction)
+ [UserDefinedFunctionInput Structure](#aws-glue-api-catalog-functions-UserDefinedFunctionInput)

## UserDefinedFunction Structure<a name="aws-glue-api-catalog-functions-UserDefinedFunction"></a>

Represents the equivalent of a Hive user\-defined function \(`UDF`\) definition\.

**Fields**
+ `FunctionName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the function\.
+ `ClassName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The Java class that contains the function code\.
+ `OwnerName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The owner of the function\.
+ `OwnerType` – UTF\-8 string \(valid values: `USER` \| `ROLE` \| `GROUP`\)\.

  The owner type\.
+ `CreateTime` – Timestamp\.

  The time at which the function was created\.
+ `ResourceUris` – An array of [ResourceUri](aws-glue-api-common.md#aws-glue-api-common-ResourceUri) objects, not more than 1000 structures\.

  The resource URIs for the function\.

## UserDefinedFunctionInput Structure<a name="aws-glue-api-catalog-functions-UserDefinedFunctionInput"></a>

A structure used to create or update a user\-defined function\.

**Fields**
+ `FunctionName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the function\.
+ `ClassName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The Java class that contains the function code\.
+ `OwnerName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The owner of the function\.
+ `OwnerType` – UTF\-8 string \(valid values: `USER` \| `ROLE` \| `GROUP`\)\.

  The owner type\.
+ `ResourceUris` – An array of [ResourceUri](aws-glue-api-common.md#aws-glue-api-common-ResourceUri) objects, not more than 1000 structures\.

  The resource URIs for the function\.

## Operations<a name="aws-glue-api-catalog-functions-actions"></a>
+ [CreateUserDefinedFunction Action \(Python: create\_user\_defined\_function\)](#aws-glue-api-catalog-functions-CreateUserDefinedFunction)
+ [UpdateUserDefinedFunction Action \(Python: update\_user\_defined\_function\)](#aws-glue-api-catalog-functions-UpdateUserDefinedFunction)
+ [DeleteUserDefinedFunction Action \(Python: delete\_user\_defined\_function\)](#aws-glue-api-catalog-functions-DeleteUserDefinedFunction)
+ [GetUserDefinedFunction Action \(Python: get\_user\_defined\_function\)](#aws-glue-api-catalog-functions-GetUserDefinedFunction)
+ [GetUserDefinedFunctions Action \(Python: get\_user\_defined\_functions\)](#aws-glue-api-catalog-functions-GetUserDefinedFunctions)

## CreateUserDefinedFunction Action \(Python: create\_user\_defined\_function\)<a name="aws-glue-api-catalog-functions-CreateUserDefinedFunction"></a>

Creates a new function definition in the Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which to create the function\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database in which to create the function\.
+ `FunctionInput` – *Required:* An [UserDefinedFunctionInput](#aws-glue-api-catalog-functions-UserDefinedFunctionInput) object\.

  A `FunctionInput` object that defines the function to create in the Data Catalog\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `AlreadyExistsException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `EntityNotFoundException`
+ `OperationTimeoutException`
+ `ResourceNumberLimitExceededException`
+ `GlueEncryptionException`

## UpdateUserDefinedFunction Action \(Python: update\_user\_defined\_function\)<a name="aws-glue-api-catalog-functions-UpdateUserDefinedFunction"></a>

Updates an existing function definition in the Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the function to be updated is located\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database where the function to be updated is located\.
+ `FunctionName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the function\.
+ `FunctionInput` – *Required:* An [UserDefinedFunctionInput](#aws-glue-api-catalog-functions-UserDefinedFunctionInput) object\.

  A `FunctionInput` object that redefines the function in the Data Catalog\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`

## DeleteUserDefinedFunction Action \(Python: delete\_user\_defined\_function\)<a name="aws-glue-api-catalog-functions-DeleteUserDefinedFunction"></a>

Deletes an existing function definition from the Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the function to be deleted is located\. If none is supplied, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database where the function is located\.
+ `FunctionName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the function definition to be deleted\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## GetUserDefinedFunction Action \(Python: get\_user\_defined\_function\)<a name="aws-glue-api-catalog-functions-GetUserDefinedFunction"></a>

Retrieves a specified function definition from the Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the function to be retrieved is located\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database where the function is located\.
+ `FunctionName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the function\.

**Response**
+ `UserDefinedFunction` – An [UserDefinedFunction](#aws-glue-api-catalog-functions-UserDefinedFunction) object\.

  The requested function definition\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `GlueEncryptionException`

## GetUserDefinedFunctions Action \(Python: get\_user\_defined\_functions\)<a name="aws-glue-api-catalog-functions-GetUserDefinedFunctions"></a>

Retrieves multiple function definitions from the Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog where the functions to be retrieved are located\. If none is provided, the AWS account ID is used by default\.
+ `DatabaseName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the catalog database where the functions are located\.
+ `Pattern` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  An optional function\-name pattern string that filters the function definitions returned\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation call\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum number of functions to return in one response\.

**Response**
+ `UserDefinedFunctions` – An array of [UserDefinedFunction](#aws-glue-api-catalog-functions-UserDefinedFunction) objects\.

  A list of requested function definitions\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if the list of functions returned does not include the last requested function\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `OperationTimeoutException`
+ `InternalServiceException`
+ `GlueEncryptionException`