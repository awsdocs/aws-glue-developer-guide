# Connection API<a name="aws-glue-api-catalog-connections"></a>

## Data Types<a name="aws-glue-api-catalog-connections-objects"></a>
+ [Connection Structure](#aws-glue-api-catalog-connections-Connection)
+ [ConnectionInput Structure](#aws-glue-api-catalog-connections-ConnectionInput)
+ [PhysicalConnectionRequirements Structure](#aws-glue-api-catalog-connections-PhysicalConnectionRequirements)
+ [GetConnectionsFilter Structure](#aws-glue-api-catalog-connections-GetConnectionsFilter)

## Connection Structure<a name="aws-glue-api-catalog-connections-Connection"></a>

Defines a connection to a data source\.

**Fields**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the connection definition\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Description of the connection\.
+ `ConnectionType` – UTF\-8 string \(valid values: `JDBC` \| `SFTP`\)\.

  The type of the connection\. Currently, only JDBC is supported; SFTP is not supported\.
+ `MatchCriteria` – An array of UTF\-8 strings, not more than 10 strings\.

  A list of criteria that can be used in selecting this connection\.
+ `ConnectionProperties` – A map array of key\-value pairs, not more than 100pairs\.

  Each key is a UTF\-8 string \(valid values: `HOST` \| `PORT` \| `USERNAME="USER_NAME"` \| `PASSWORD` \| `ENCRYPTED_PASSWORD` \| `JDBC_DRIVER_JAR_URI` \| `JDBC_DRIVER_CLASS_NAME` \| `JDBC_ENGINE` \| `JDBC_ENGINE_VERSION` \| `CONFIG_FILES` \| `INSTANCE_ID` \| `JDBC_CONNECTION_URL` \| `JDBC_ENFORCE_SSL`\)\.

  Each value is a Value string\.

  These key\-value pairs define parameters for the connection:
  + `HOST` \- The host URI: either the fully qualified domain name \(FQDN\) or the IPv4 address of the database host\.
  + `PORT` \- The port number, between 1024 and 65535, of the port on which the database host is listening for database connections\.
  + `USER_NAME` \- The name under which to log in to the database\.
  + `PASSWORD` \- A password, if one is used, for the user name\.
  + `ENCRYPTED_PASSWORD` \- \(Reserved for future use\)\.
  + `JDBC_DRIVER_JAR_URI` \- The S3 path of the a jar file that contains the JDBC driver to use\.
  + `JDBC_DRIVER_CLASS_NAME` \- The class name of the JDBC driver to use\.
  + `JDBC_ENGINE` \- The name of the JDBC engine to use\.
  + `JDBC_ENGINE_VERSION` \- The version of the JDBC engine to use\.
  + `CONFIG_FILES` \- \(Reserved for future use\)\.
  + `INSTANCE_ID` \- The instance ID to use\.
  + `JDBC_CONNECTION_URL` \- The URL for the JDBC connection\.
  + `JDBC_ENFORCE_SSL` \- A Boolean string \(true, false\) specifying whether SSL with hostname matching will be enforced for the JDBC connection on the client\. The default is false\.
+ `PhysicalConnectionRequirements` – A [PhysicalConnectionRequirements](#aws-glue-api-catalog-connections-PhysicalConnectionRequirements) object\.

  A map of physical connection requirements, such as VPC and SecurityGroup, needed for making this connection successfully\.
+ `CreationTime` – Timestamp\.

  The time this connection definition was created\.
+ `LastUpdatedTime` – Timestamp\.

  The last time this connection definition was updated\.
+ `LastUpdatedBy` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The user, group or role that last updated this connection definition\.

## ConnectionInput Structure<a name="aws-glue-api-catalog-connections-ConnectionInput"></a>

A structure used to specify a connection to create or update\.

**Fields**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the connection\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Description of the connection\.
+ `ConnectionType` – *Required:* UTF\-8 string \(valid values: `JDBC` \| `SFTP`\)\.

  The type of the connection\. Currently, only JDBC is supported; SFTP is not supported\.
+ `MatchCriteria` – An array of UTF\-8 strings, not more than 10 strings\.

  A list of criteria that can be used in selecting this connection\.
+ `ConnectionProperties` – *Required:* A map array of key\-value pairs, not more than 100pairs\.

  Each key is a UTF\-8 string \(valid values: `HOST` \| `PORT` \| `USERNAME="USER_NAME"` \| `PASSWORD` \| `ENCRYPTED_PASSWORD` \| `JDBC_DRIVER_JAR_URI` \| `JDBC_DRIVER_CLASS_NAME` \| `JDBC_ENGINE` \| `JDBC_ENGINE_VERSION` \| `CONFIG_FILES` \| `INSTANCE_ID` \| `JDBC_CONNECTION_URL` \| `JDBC_ENFORCE_SSL`\)\.

  Each value is a Value string\.

  These key\-value pairs define parameters for the connection\.
+ `PhysicalConnectionRequirements` – A [PhysicalConnectionRequirements](#aws-glue-api-catalog-connections-PhysicalConnectionRequirements) object\.

  A map of physical connection requirements, such as VPC and SecurityGroup, needed for making this connection successfully\.

## PhysicalConnectionRequirements Structure<a name="aws-glue-api-catalog-connections-PhysicalConnectionRequirements"></a>

Specifies the physical requirements for a connection\.

**Fields**
+ `SubnetId` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The subnet ID used by the connection\.
+ `SecurityGroupIdList` – An array of UTF\-8 strings, not more than 50 strings\.

  The security group ID list used by the connection\.
+ `AvailabilityZone` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The connection's availability zone\. This field is redundant, since the specified subnet implies the availability zone to be used\. The field must be populated now, but will be deprecated in the future\.

## GetConnectionsFilter Structure<a name="aws-glue-api-catalog-connections-GetConnectionsFilter"></a>

Filters the connection definitions returned by the `GetConnections` API\.

**Fields**
+ `MatchCriteria` – An array of UTF\-8 strings, not more than 10 strings\.

  A criteria string that must match the criteria recorded in the connection definition for that connection definition to be returned\.
+ `ConnectionType` – UTF\-8 string \(valid values: `JDBC` \| `SFTP`\)\.

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
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which to create the connection\. If none is supplied, the AWS account ID is used by default\.
+ `ConnectionInput` – *Required:* A [ConnectionInput](#aws-glue-api-catalog-connections-ConnectionInput) object\.

  A `ConnectionInput` object defining the connection to create\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `AlreadyExistsException`
+ `InvalidInputException`
+ `OperationTimeoutException`
+ `ResourceNumberLimitExceededException`

## DeleteConnection Action \(Python: delete\_connection\)<a name="aws-glue-api-catalog-connections-DeleteConnection"></a>

Deletes a connection from the Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which the connection resides\. If none is supplied, the AWS account ID is used by default\.
+ `ConnectionName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

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
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which the connection resides\. If none is supplied, the AWS account ID is used by default\.
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the connection definition to retrieve\.

**Response**
+ `Connection` – A [Connection](#aws-glue-api-catalog-connections-Connection) object\.

  The requested connection definition\.

**Errors**
+ `EntityNotFoundException`
+ `OperationTimeoutException`
+ `InvalidInputException`

## GetConnections Action \(Python: get\_connections\)<a name="aws-glue-api-catalog-connections-GetConnections"></a>

Retrieves a list of connection definitions from the Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which the connections reside\. If none is supplied, the AWS account ID is used by default\.
+ `Filter` – A [GetConnectionsFilter](#aws-glue-api-catalog-connections-GetConnectionsFilter) object\.

  A filter that controls which connections will be returned\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation call\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum number of connections to return in one response\.

**Response**
+ `ConnectionList` – An array of [Connection](#aws-glue-api-catalog-connections-Connection) objects\.

  A list of requested connection definitions\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if the list of connections returned does not include the last of the filtered connections\.

**Errors**
+ `EntityNotFoundException`
+ `OperationTimeoutException`
+ `InvalidInputException`

## UpdateConnection Action \(Python: update\_connection\)<a name="aws-glue-api-catalog-connections-UpdateConnection"></a>

Updates a connection definition in the Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which the connection resides\. If none is supplied, the AWS account ID is used by default\.
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the connection definition to update\.
+ `ConnectionInput` – *Required:* A [ConnectionInput](#aws-glue-api-catalog-connections-ConnectionInput) object\.

  A `ConnectionInput` object that redefines the connection in question\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `OperationTimeoutException`
+ `InvalidInputException`

## BatchDeleteConnection Action \(Python: batch\_delete\_connection\)<a name="aws-glue-api-catalog-connections-BatchDeleteConnection"></a>

Deletes a list of connection definitions from the Data Catalog\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog in which the connections reside\. If none is supplied, the AWS account ID is used by default\.
+ `ConnectionNameList` – *Required:* An array of UTF\-8 strings, not more than 25 strings\.

  A list of names of the connections to delete\.

**Response**
+ `Succeeded` – An array of UTF\-8 strings\.

  A list of names of the connection definitions that were successfully deleted\.
+ `Errors` – A map array of key\-value pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Each value is a An [ErrorDetail](aws-glue-api-common.md#aws-glue-api-common-ErrorDetail) object\.

  A map of the names of connections that were not successfully deleted to error details\.

**Errors**
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`