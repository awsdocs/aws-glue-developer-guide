# Connection API<a name="aws-glue-api-catalog-connections"></a>

## Data Types<a name="aws-glue-api-catalog-connections-objects"></a>
+ [Connection Structure](#aws-glue-api-catalog-connections-Connection)
+ [ConnectionInput Structure](#aws-glue-api-catalog-connections-ConnectionInput)
+ [PhysicalConnectionRequirements Structure](#aws-glue-api-catalog-connections-PhysicalConnectionRequirements)
+ [GetConnectionsFilter Structure](#aws-glue-api-catalog-connections-GetConnectionsFilter)

## Connection Structure<a name="aws-glue-api-catalog-connections-Connection"></a>

Defines a connection to a data source\.

**Fields**
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the connection definition\.
+ `Description` – Description string, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Description of the connection\.
+ `ConnectionType` – String \(valid values: `JDBC` \| `SFTP`\)\.

  The type of the connection\. Currently, only JDBC is supported; SFTP is not supported\.
+ `MatchCriteria` – An array of UTF\-8 strings\.

  A list of criteria that can be used in selecting this connection\.
+ `ConnectionProperties` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\.

  A list of key\-value pairs used as parameters for this connection\.
+ `PhysicalConnectionRequirements` – A PhysicalConnectionRequirements object\.

  A map of physical connection requirements, such as VPC and SecurityGroup, needed for making this connection successfully\.
+ `CreationTime` – Timestamp\.

  The time this connection definition was created\.
+ `LastUpdatedTime` – Timestamp\.

  The last time this connection definition was updated\.
+ `LastUpdatedBy` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The user, group or role that last updated this connection definition\.

## ConnectionInput Structure<a name="aws-glue-api-catalog-connections-ConnectionInput"></a>

A structure used to specify a connection to create or update\.

**Fields**
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the connection\.
+ `Description` – Description string, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Description of the connection\.
+ `ConnectionType` – String \(valid values: `JDBC` \| `SFTP`\)\. Required\.

  The type of the connection\. Currently, only JDBC is supported; SFTP is not supported\.
+ `MatchCriteria` – An array of UTF\-8 strings\.

  A list of criteria that can be used in selecting this connection\.
+ `ConnectionProperties` – An array of *UTF\-8 string*–to–*UTF\-8 string* mappings\. Required\.

  A list of key\-value pairs used as parameters for this connection\.
+ `PhysicalConnectionRequirements` – A PhysicalConnectionRequirements object\.

  A map of physical connection requirements, such as VPC and SecurityGroup, needed for making this connection successfully\.

## PhysicalConnectionRequirements Structure<a name="aws-glue-api-catalog-connections-PhysicalConnectionRequirements"></a>

Specifies the physical requirements for a connection\.

**Fields**
+ `SubnetId` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The subnet ID used by the connection\.
+ `SecurityGroupIdList` – An array of UTF\-8 strings\.

  The security group ID list used by the connection\.
+ `AvailabilityZone` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The connection's availability zone\. This field is deprecated and has no effect\.

## GetConnectionsFilter Structure<a name="aws-glue-api-catalog-connections-GetConnectionsFilter"></a>

Filters the connection definitions returned by the `GetConnections` API\.

**Fields**
+ `MatchCriteria` – An array of UTF\-8 strings\.

  A criteria string that must match the criteria recorded in the connection definition for that connection definition to be returned\.
+ `ConnectionType` – String \(valid values: `JDBC` \| `SFTP`\)\.

  The type of connections to return\. Currently, only JDBC is supported; SFTP is not supported\.

## Operations<a name="aws-glue-api-catalog-connections-actions"></a>
+ [CreateConnection Action \(Python: create\_connection\)](#aws-glue-api-catalog-connections-CreateConnection)
+ [DeleteConnection Action \(Python: delete\_connection\)](#aws-glue-api-catalog-connections-DeleteConnection)
+ [GetConnection Action \(Python: get\_connection\)](#aws-glue-api-catalog-connections-GetConnection)
+ [GetConnections Action \(Python: get\_connections\)](#aws-glue-api-catalog-connections-GetConnections)
+ [UpdateConnection Action \(Python: update\_connection\)](#aws-glue-api-catalog-connections-UpdateConnection)
+ [BatchDeleteConnection Action \(Python: batch\_delete\_connection\)](#aws-glue-api-catalog-connections-BatchDeleteConnection)

## CreateConnection Action \(Python: create\_connection\)<a name="aws-glue-api-catalog-connections-CreateConnection"></a>

Creates a connection definition in the Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which to create the connection\. If none is supplied, the AWS account ID is used by default\.
+ `ConnectionInput` – A ConnectionInput object\. Required\.

  A `ConnectionInput` object defining the connection to create\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `AlreadyExistsException`
+ `InvalidInputException`
+ `OperationTimeoutException`
+ `ResourceNumberLimitExceededException`
+ `GlueEncryptionException`

## DeleteConnection Action \(Python: delete\_connection\)<a name="aws-glue-api-catalog-connections-DeleteConnection"></a>

Deletes a connection from the Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which the connection resides\. If none is supplied, the AWS account ID is used by default\.
+ `ConnectionName` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the connection to delete\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `OperationTimeoutException`
+ `InvalidInputException`

## GetConnection Action \(Python: get\_connection\)<a name="aws-glue-api-catalog-connections-GetConnection"></a>

Retrieves a connection definition from the Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which the connection resides\. If none is supplied, the AWS account ID is used by default\.
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the connection definition to retrieve\.

**Response**
+ `Connection` – A Connection object\.

  The requested connection definition\.

**Errors**
+ `EntityNotFoundException`
+ `OperationTimeoutException`
+ `InvalidInputException`
+ `GlueEncryptionException`

## GetConnections Action \(Python: get\_connections\)<a name="aws-glue-api-catalog-connections-GetConnections"></a>

Retrieves a list of connection definitions from the Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which the connections reside\. If none is supplied, the AWS account ID is used by default\.
+ `Filter` – A GetConnectionsFilter object\.

  A filter that controls which connections will be returned\.
+ `NextToken` – String\.

  A continuation token, if this is a continuation call\.
+ `MaxResults` – Number \(integer\)\.

  The maximum number of connections to return in one response\.

**Response**
+ `ConnectionList` – An array of [Connection](#aws-glue-api-catalog-connections-Connection)s\.

  A list of requested connection definitions\.
+ `NextToken` – String\.

  A continuation token, if the list of connections returned does not include the last of the filtered connections\.

**Errors**
+ `EntityNotFoundException`
+ `OperationTimeoutException`
+ `InvalidInputException`
+ `GlueEncryptionException`

## UpdateConnection Action \(Python: update\_connection\)<a name="aws-glue-api-catalog-connections-UpdateConnection"></a>

Updates a connection definition in the Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which the connection resides\. If none is supplied, the AWS account ID is used by default\.
+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the connection definition to update\.
+ `ConnectionInput` – A ConnectionInput object\. Required\.

  A `ConnectionInput` object that redefines the connection in question\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `OperationTimeoutException`
+ `InvalidInputException`
+ `GlueEncryptionException`

## BatchDeleteConnection Action \(Python: batch\_delete\_connection\)<a name="aws-glue-api-catalog-connections-BatchDeleteConnection"></a>

Deletes a list of connection definitions from the Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which the connections reside\. If none is supplied, the AWS account ID is used by default\.
+ `ConnectionNameList` – An array of UTF\-8 strings\. Required\.

  A list of names of the connections to delete\.

**Response**
+ `Succeeded` – An array of UTF\-8 strings\.

  A list of names of the connection definitions that were successfully deleted\.
+ `Errors` – An array of *UTF\-8 string*–to–*[ErrorDetail](aws-glue-api-common.md#aws-glue-api-common-ErrorDetail)* mappings\.

  A map of the names of connections that were not successfully deleted to error details\.

**Errors**
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`