# Schema Registry<a name="aws-glue-api-schema-registry-api"></a>

The Schema Registry API describes the data types and API related to working with schemas in AWS Glue\.

## Data Types<a name="aws-glue-api-schema-registry-api-objects"></a>
+ [RegistryId Structure](#aws-glue-api-schema-registry-api-RegistryId)
+ [RegistryListItem Structure](#aws-glue-api-schema-registry-api-RegistryListItem)
+ [MetadataInfo Structure](#aws-glue-api-schema-registry-api-MetadataInfo)
+ [SchemaListItem Structure](#aws-glue-api-schema-registry-api-SchemaListItem)
+ [SchemaVersionListItem Structure](#aws-glue-api-schema-registry-api-SchemaVersionListItem)
+ [MetadataKeyValuePair Structure](#aws-glue-api-schema-registry-api-MetadataKeyValuePair)
+ [SchemaVersionErrorItem Structure](#aws-glue-api-schema-registry-api-SchemaVersionErrorItem)
+ [ErrorDetails Structure](#aws-glue-api-schema-registry-api-ErrorDetails)
+ [SchemaVersionNumber Structure](#aws-glue-api-schema-registry-api-SchemaVersionNumber)
+ [SchemaId Structure](#aws-glue-api-schema-registry-api-SchemaId)

## RegistryId Structure<a name="aws-glue-api-schema-registry-api-RegistryId"></a>

A wrapper structure that may contain the registry name and Amazon Resource Name \(ARN\)\.

**Fields**
+ `RegistryName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  Name of the registry\. Used only for lookup\. One of `RegistryArn` or `RegistryName` has to be provided\. 
+ `RegistryArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  Arn of the registry to be updated\. One of `RegistryArn` or `RegistryName` has to be provided\.

## RegistryListItem Structure<a name="aws-glue-api-schema-registry-api-RegistryListItem"></a>

A structure containing the details for a registry\.

**Fields**
+ `RegistryName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name of the registry\.
+ `RegistryArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) of the registry\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of the registry\.
+ `Status` – UTF\-8 string \(valid values: `AVAILABLE` \| `DELETING`\)\.

  The status of the registry\.
+ `CreatedTime` – UTF\-8 string\.

  The data the registry was created\.
+ `UpdatedTime` – UTF\-8 string\.

  The date the registry was updated\.

## MetadataInfo Structure<a name="aws-glue-api-schema-registry-api-MetadataInfo"></a>

A structure containing metadata information for a schema version\.

**Fields**
+ `MetadataValue` – UTF\-8 string, not less than 1 or more than 256 bytes long, matching the [Custom string pattern #18](aws-glue-api-common.md#regex_18)\.

  The metadata key's corresponding value\.
+ `CreatedTime` – UTF\-8 string\.

  The time at which the entry was created\.

## SchemaListItem Structure<a name="aws-glue-api-schema-registry-api-SchemaListItem"></a>

An object that contains minimal details for a schema\.

**Fields**
+ `RegistryName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  the name of the registry where the schema resides\.
+ `SchemaName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name of the schema\.
+ `SchemaArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) for the schema\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description for the schema\.
+ `SchemaStatus` – UTF\-8 string \(valid values: `AVAILABLE` \| `PENDING` \| `DELETING`\)\.

  The status of the schema\.
+ `CreatedTime` – UTF\-8 string\.

  The date and time that a schema was created\.
+ `UpdatedTime` – UTF\-8 string\.

  The date and time that a schema was updated\.

## SchemaVersionListItem Structure<a name="aws-glue-api-schema-registry-api-SchemaVersionListItem"></a>

An object containing the details about a schema version\.

**Fields**
+ `SchemaArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) of the schema\.
+ `SchemaVersionId` – UTF\-8 string, not less than 36 or more than 36 bytes long, matching the [Custom string pattern #11](aws-glue-api-common.md#regex_11)\.

  The unique identifier of the schema version\.
+ `VersionNumber` – Number \(long\), not less than 1 or more than 100000\.

  The version number of the schema\.
+ `Status` – UTF\-8 string \(valid values: `AVAILABLE` \| `PENDING` \| `FAILURE` \| `DELETING`\)\.

  The status of the schema version\.
+ `CreatedTime` – UTF\-8 string\.

  The date and time the schema version was created\.

## MetadataKeyValuePair Structure<a name="aws-glue-api-schema-registry-api-MetadataKeyValuePair"></a>

A structure containing a key value pair for metadata\.

**Fields**
+ `MetadataKey` – UTF\-8 string, not less than 1 or more than 128 bytes long, matching the [Custom string pattern #18](aws-glue-api-common.md#regex_18)\.

  A metadata key\.
+ `MetadataValue` – UTF\-8 string, not less than 1 or more than 256 bytes long, matching the [Custom string pattern #18](aws-glue-api-common.md#regex_18)\.

  A metadata key's corresponding value\.

## SchemaVersionErrorItem Structure<a name="aws-glue-api-schema-registry-api-SchemaVersionErrorItem"></a>

An object that contains the error details for an operation on a schema version\.

**Fields**
+ `VersionNumber` – Number \(long\), not less than 1 or more than 100000\.

  The version number of the schema\.
+ `ErrorDetails` – An [ErrorDetails](#aws-glue-api-schema-registry-api-ErrorDetails) object\.

  The details of the error for the schema version\.

## ErrorDetails Structure<a name="aws-glue-api-schema-registry-api-ErrorDetails"></a>

An object containing error details\.

**Fields**
+ `ErrorCode` – UTF\-8 string\.

  The error code for an error\.
+ `ErrorMessage` – UTF\-8 string\.

  The error message for an error\.

## SchemaVersionNumber Structure<a name="aws-glue-api-schema-registry-api-SchemaVersionNumber"></a>

A structure containing the schema version information\.

**Fields**
+ `LatestVersion` – Boolean\.

  The latest version available for the schema\.
+ `VersionNumber` – Number \(long\), not less than 1 or more than 100000\.

  The version number of the schema\.

## SchemaId Structure<a name="aws-glue-api-schema-registry-api-SchemaId"></a>

The unique ID of the schema in the AWS Glue schema registry\.

**Fields**
+ `SchemaArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) of the schema\. One of `SchemaArn` or `SchemaName` has to be provided\.
+ `SchemaName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name of the schema\. One of `SchemaArn` or `SchemaName` has to be provided\.
+ `RegistryName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name of the schema registry that contains the schema\.

## Operations<a name="aws-glue-api-schema-registry-api-actions"></a>
+ [CreateRegistry Action \(Python: create\_registry\)](#aws-glue-api-schema-registry-api-CreateRegistry)
+ [CreateSchema Action](#aws-glue-api-schema-registry-api-CreateSchema)
+ [GetSchema Action](#aws-glue-api-schema-registry-api-GetSchema)
+ [ListSchemaVersions Action](#aws-glue-api-schema-registry-api-ListSchemaVersions)
+ [GetSchemaVersion Action](#aws-glue-api-schema-registry-api-GetSchemaVersion)
+ [GetSchemaVersionsDiff Action](#aws-glue-api-schema-registry-api-GetSchemaVersionsDiff)
+ [ListRegistries Action](#aws-glue-api-schema-registry-api-ListRegistries)
+ [ListSchemas Action](#aws-glue-api-schema-registry-api-ListSchemas)
+ [RegisterSchemaVersion Action](#aws-glue-api-schema-registry-api-RegisterSchemaVersion)
+ [UpdateSchema Action](#aws-glue-api-schema-registry-api-UpdateSchema)
+ [CheckSchemaVersionValidity Action](#aws-glue-api-schema-registry-api-CheckSchemaVersionValidity)
+ [UpdateRegistry Action](#aws-glue-api-schema-registry-api-UpdateRegistry)
+ [GetSchemaByDefinition Action](#aws-glue-api-schema-registry-api-GetSchemaByDefinition)
+ [GetRegistry Action](#aws-glue-api-schema-registry-api-GetRegistry)
+ [PutSchemaVersionMetadata Action](#aws-glue-api-schema-registry-api-PutSchemaVersionMetadata)
+ [QuerySchemaVersionMetadata Action](#aws-glue-api-schema-registry-api-QuerySchemaVersionMetadata)
+ [RemoveSchemaVersionMetadata Action](#aws-glue-api-schema-registry-api-RemoveSchemaVersionMetadata)
+ [DeleteRegistry Action](#aws-glue-api-schema-registry-api-DeleteRegistry)
+ [DeleteSchema Action](#aws-glue-api-schema-registry-api-DeleteSchema)
+ [DeleteSchemaVersions Action](#aws-glue-api-schema-registry-api-DeleteSchemaVersions)

## CreateRegistry Action \(Python: create\_registry\)<a name="aws-glue-api-schema-registry-api-CreateRegistry"></a>

Creates a new registry which may be used to hold a collection of schemas\.

**Request**
+ `RegistryName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  Name of the registry to be created of max length of 255, and may only contain letters, numbers, hyphen, underscore, dollar sign, or hash mark\. No whitespace\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of the registry\. If description is not provided, there will not be any default value for this\.
+ `Tags` – A map array of key\-value pairs, not more than 50 pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 128 bytes long\.

  Each value is a UTF\-8 string, not more than 256 bytes long\.

  AWS tags that contain a key value pair and may be searched by console, command line, or API\.

**Response**
+ `RegistryArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) of the newly created registry\.
+ `RegistryName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name of the registry\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of the registry\.
+ `Tags` – A map array of key\-value pairs, not more than 50 pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 128 bytes long\.

  Each value is a UTF\-8 string, not more than 256 bytes long\.

  The tags for the registry\.

**Errors**
+ `InvalidInputException`
+ `AccessDeniedException`
+ `AlreadyExistsException`
+ `ResourceNumberLimitExceededException`
+ `InternalServiceException`

## CreateSchema Action<a name="aws-glue-api-schema-registry-api-CreateSchema"></a>

Creates a new schema set and registers the schema definition\. Returns an error if the schema set already exists without actually registering the version\.

When the schema set is created, a version checkpoint will be set to the first version\. Compatibility mode "DISABLED" restricts any additional schema versions from being added after the first schema version\. For all other compatibility modes, validation of compatibility settings will be applied only from the second version onwards when the `RegisterSchemaVersion` API is used\.

When this API is called without a `RegistryId`, this will create an entry for a "default\-registry" in the registry database tables, if it is not already present\.

**Request**
+ `RegistryId` – A [RegistryId](#aws-glue-api-schema-registry-api-RegistryId) object\.

   This is a wrapper shape to contain the registry identity fields\. If this is not provided, the default registry will be used\. The ARN format for the same will be: `arn:aws:glue:us-east-2:<customer id>:registry/default-registry:random-5-letter-id`\.
+ `SchemaName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  Name of the schema to be created of max length of 255, and may only contain letters, numbers, hyphen, underscore, dollar sign, or hash mark\. No whitespace\.
+ `DataFormat` – *Required:* UTF\-8 string \(valid values: `AVRO`\)\.

  The data format of the schema definition\. Currently only `AVRO` is supported\.
+ `Compatibility` – UTF\-8 string \(valid values: `NONE` \| `DISABLED` \| `BACKWARD` \| `BACKWARD_ALL` \| `FORWARD` \| `FORWARD_ALL` \| `FULL` \| `FULL_ALL`\)\.

  The compatibility mode of the schema\. The possible values are:
  +  *NONE*: No compatibility mode applies\. You can use this choice in development scenarios or if you do not know the compatibility mode that you want to apply to schemas\. Any new version added will be accepted without undergoing a compatibility check\.
  +  *DISABLED*: This compatibility choice prevents versioning for a particular schema\. You can use this choice to prevent future versioning of a schema\.
  +  *BACKWARD*: This compatibility choice is recommended as it allows data receivers to read both the current and one previous schema version\. This means that for instance, a new schema version cannot drop data fields or change the type of these fields, so they can't be read by readers using the previous version\.
  +  *BACKWARD\_ALL*: This compatibility choice allows data receivers to read both the current and all previous schema versions\. You can use this choice when you need to delete fields or add optional fields, and check compatibility against all previous schema versions\. 
  +  *FORWARD*: This compatibility choice allows data receivers to read both the current and one next schema version, but not necessarily later versions\. You can use this choice when you need to add fields or delete optional fields, but only check compatibility against the last schema version\.
  +  *FORWARD\_ALL*: This compatibility choice allows data receivers to read written by producers of any new registered schema\. You can use this choice when you need to add fields or delete optional fields, and check compatibility against all previous schema versions\.
  +  *FULL*: This compatibility choice allows data receivers to read data written by producers using the previous or next version of the schema, but not necessarily earlier or later versions\. You can use this choice when you need to add or remove optional fields, but only check compatibility against the last schema version\.
  +  *FULL\_ALL*: This compatibility choice allows data receivers to read data written by producers using all previous schema versions\. You can use this choice when you need to add or remove optional fields, and check compatibility against all previous schema versions\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  An optional description of the schema\. If description is not provided, there will not be any automatic default value for this\.
+ `Tags` – A map array of key\-value pairs, not more than 50 pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 128 bytes long\.

  Each value is a UTF\-8 string, not more than 256 bytes long\.

  AWS tags that contain a key value pair and may be searched by console, command line, or API\. If specified, follows the AWS tags\-on\-create pattern\.
+ `SchemaDefinition` – UTF\-8 string, not less than 1 or more than 170000 bytes long, matching the [Custom string pattern #17](aws-glue-api-common.md#regex_17)\.

  The schema definition using the `DataFormat` setting for `SchemaName`\.

**Response**
+ `RegistryName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name of the registry\.
+ `RegistryArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) of the registry\.
+ `SchemaName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name of the schema\.
+ `SchemaArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) of the schema\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of the schema if specified when created\.
+ `DataFormat` – UTF\-8 string \(valid values: `AVRO`\)\.

  The data format of the schema definition\. Currently only `AVRO` is supported\.
+ `Compatibility` – UTF\-8 string \(valid values: `NONE` \| `DISABLED` \| `BACKWARD` \| `BACKWARD_ALL` \| `FORWARD` \| `FORWARD_ALL` \| `FULL` \| `FULL_ALL`\)\.

  The schema compatibility mode\.
+ `SchemaCheckpoint` – Number \(long\), not less than 1 or more than 100000\.

  The version number of the checkpoint \(the last time the compatibility mode was changed\)\.
+ `LatestSchemaVersion` – Number \(long\), not less than 1 or more than 100000\.

  The latest version of the schema associated with the returned schema definition\.
+ `NextSchemaVersion` – Number \(long\), not less than 1 or more than 100000\.

  The next version of the schema associated with the returned schema definition\.
+ `SchemaStatus` – UTF\-8 string \(valid values: `AVAILABLE` \| `PENDING` \| `DELETING`\)\.

  The status of the schema\. 
+ `Tags` – A map array of key\-value pairs, not more than 50 pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 128 bytes long\.

  Each value is a UTF\-8 string, not more than 256 bytes long\.

  The tags for the schema\.
+ `SchemaVersionId` – UTF\-8 string, not less than 36 or more than 36 bytes long, matching the [Custom string pattern #11](aws-glue-api-common.md#regex_11)\.

  The unique identifier of the first schema version\.
+ `SchemaVersionStatus` – UTF\-8 string \(valid values: `AVAILABLE` \| `PENDING` \| `FAILURE` \| `DELETING`\)\.

  The status of the first schema version created\.

**Errors**
+ `InvalidInputException`
+ `AccessDeniedException`
+ `EntityNotFoundException`
+ `AlreadyExistsException`
+ `ResourceNumberLimitExceededException`
+ `InternalServiceException`

## GetSchema Action<a name="aws-glue-api-schema-registry-api-GetSchema"></a>

Describes the specified schema in detail\.

**Request**
+ `SchemaId` – *Required:* A [SchemaId](#aws-glue-api-schema-registry-api-SchemaId) object\.

  This is a wrapper structure to contain schema identity fields\. The structure contains:
  + SchemaId$SchemaArn: The Amazon Resource Name \(ARN\) of the schema\. Either `SchemaArn` or `SchemaName` and `RegistryName` has to be provided\.
  + SchemaId$SchemaName: The name of the schema\. Either `SchemaArn` or `SchemaName` and `RegistryName` has to be provided\.

**Response**
+ `RegistryName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name of the registry\.
+ `RegistryArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) of the registry\.
+ `SchemaName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name of the schema\.
+ `SchemaArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) of the schema\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of schema if specified when created
+ `DataFormat` – UTF\-8 string \(valid values: `AVRO`\)\.

  The data format of the schema definition\. Currently only `AVRO` is supported\.
+ `Compatibility` – UTF\-8 string \(valid values: `NONE` \| `DISABLED` \| `BACKWARD` \| `BACKWARD_ALL` \| `FORWARD` \| `FORWARD_ALL` \| `FULL` \| `FULL_ALL`\)\.

  The compatibility mode of the schema\.
+ `SchemaCheckpoint` – Number \(long\), not less than 1 or more than 100000\.

  The version number of the checkpoint \(the last time the compatibility mode was changed\)\.
+ `LatestSchemaVersion` – Number \(long\), not less than 1 or more than 100000\.

  The latest version of the schema associated with the returned schema definition\.
+ `NextSchemaVersion` – Number \(long\), not less than 1 or more than 100000\.

  The next version of the schema associated with the returned schema definition\.
+ `SchemaStatus` – UTF\-8 string \(valid values: `AVAILABLE` \| `PENDING` \| `DELETING`\)\.

  The status of the schema\.
+ `CreatedTime` – UTF\-8 string\.

  The date and time the schema was created\.
+ `UpdatedTime` – UTF\-8 string\.

  The date and time the schema was updated\.

**Errors**
+ `InvalidInputException`
+ `AccessDeniedException`
+ `EntityNotFoundException`
+ `InternalServiceException`

## ListSchemaVersions Action<a name="aws-glue-api-schema-registry-api-ListSchemaVersions"></a>

Returns a list of schema versions that you have created, with minimal information\. Schema versions in Deleted status will not be included in the results\. Empty results will be returned if there are no schema versions available\.

**Request**
+ `SchemaId` – *Required:* A [SchemaId](#aws-glue-api-schema-registry-api-SchemaId) object\.

  This is a wrapper structure to contain schema identity fields\. The structure contains:
  + SchemaId$SchemaArn: The Amazon Resource Name \(ARN\) of the schema\. Either `SchemaArn` or `SchemaName` and `RegistryName` has to be provided\.
  + SchemaId$SchemaName: The name of the schema\. Either `SchemaArn` or `SchemaName` and `RegistryName` has to be provided\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 100\.

  Maximum number of results required per page\. If the value is not supplied, this will be defaulted to 25 per page\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation call\.

**Response**
+ `Schemas` – An array of [SchemaVersionListItem](#aws-glue-api-schema-registry-api-SchemaVersionListItem) objects\.

  An array of `SchemaVersionList` objects containing details of each schema version\.
+ `NextToken` – UTF\-8 string\.

  A continuation token for paginating the returned list of tokens, returned if the current segment of the list is not the last\.

**Errors**
+ `InvalidInputException`
+ `AccessDeniedException`
+ `EntityNotFoundException`
+ `InternalServiceException`

## GetSchemaVersion Action<a name="aws-glue-api-schema-registry-api-GetSchemaVersion"></a>

Get the specified schema by its unique ID assigned when a version of the schema is created or registered\. Schema versions in Deleted status will not be included in the results\.

**Request**
+ `SchemaId` – A [SchemaId](#aws-glue-api-schema-registry-api-SchemaId) object\.

  This is a wrapper structure to contain schema identity fields\. The structure contains:
  + SchemaId$SchemaArn: The Amazon Resource Name \(ARN\) of the schema\. Either `SchemaArn` or `SchemaName` and `RegistryName` has to be provided\.
  + SchemaId$SchemaName: The name of the schema\. Either `SchemaArn` or `SchemaName` and `RegistryName` has to be provided\.
+ `SchemaVersionId` – UTF\-8 string, not less than 36 or more than 36 bytes long, matching the [Custom string pattern #11](aws-glue-api-common.md#regex_11)\.

  The `SchemaVersionId` of the schema version\. This field is required for fetching by schema ID\. Either this or the `SchemaId` wrapper has to be provided\.
+ `SchemaVersionNumber` – A [SchemaVersionNumber](#aws-glue-api-schema-registry-api-SchemaVersionNumber) object\.

  The version number of the schema\.

**Response**
+ `SchemaVersionId` – UTF\-8 string, not less than 36 or more than 36 bytes long, matching the [Custom string pattern #11](aws-glue-api-common.md#regex_11)\.

  The `SchemaVersionId` of the schema version\.
+ `SchemaDefinition` – UTF\-8 string, not less than 1 or more than 170000 bytes long, matching the [Custom string pattern #17](aws-glue-api-common.md#regex_17)\.

  The schema definition for the schema ID\.
+ `DataFormat` – UTF\-8 string \(valid values: `AVRO`\)\.

  The data format of the schema definition\. Currently only `AVRO` is supported\.
+ `SchemaArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) of the schema\.
+ `VersionNumber` – Number \(long\), not less than 1 or more than 100000\.

  The version number of the schema\.
+ `Status` – UTF\-8 string \(valid values: `AVAILABLE` \| `PENDING` \| `FAILURE` \| `DELETING`\)\.

  The status of the schema version\. 
+ `CreatedTime` – UTF\-8 string\.

  The date and time the schema version was created\.

**Errors**
+ `InvalidInputException`
+ `AccessDeniedException`
+ `EntityNotFoundException`
+ `InternalServiceException`

## GetSchemaVersionsDiff Action<a name="aws-glue-api-schema-registry-api-GetSchemaVersionsDiff"></a>

Fetches the schema version difference in the specified difference type between two stored schema versions in the Schema Registry\.

This API allows you to compare two schema versions between two schema definitions under the same schema\.

**Request**
+ `SchemaId` – *Required:* A [SchemaId](#aws-glue-api-schema-registry-api-SchemaId) object\.

  This is a wrapper structure to contain schema identity fields\. The structure contains:
  + SchemaId$SchemaArn: The Amazon Resource Name \(ARN\) of the schema\. One of `SchemaArn` or `SchemaName` has to be provided\.
  + SchemaId$SchemaName: The name of the schema\. One of `SchemaArn` or `SchemaName` has to be provided\.
+ `FirstSchemaVersionNumber` – *Required:* A [SchemaVersionNumber](#aws-glue-api-schema-registry-api-SchemaVersionNumber) object\.

  The first of the two schema versions to be compared\.
+ `SecondSchemaVersionNumber` – *Required:* A [SchemaVersionNumber](#aws-glue-api-schema-registry-api-SchemaVersionNumber) object\.

  The second of the two schema versions to be compared\.
+ `SchemaDiffType` – *Required:* UTF\-8 string \(valid values: `SYNTAX_DIFF`\)\.

  Refers to `SYNTAX_DIFF`, which is the currently supported diff type\.

**Response**
+ `Diff` – UTF\-8 string, not less than 1 or more than 340000 bytes long, matching the [Custom string pattern #17](aws-glue-api-common.md#regex_17)\.

  The difference between schemas as a string in JsonPatch format\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `AccessDeniedException`
+ `InternalServiceException`

## ListRegistries Action<a name="aws-glue-api-schema-registry-api-ListRegistries"></a>

Returns a list of registries that you have created, with minimal registry information\. Registries in the `Deleting` status will not be included in the results\. Empty results will be returned if there are no registries available\.

**Request**
+ `MaxResults` – Number \(integer\), not less than 1 or more than 100\.

  Maximum number of results required per page\. If the value is not supplied, this will be defaulted to 25 per page\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation call\.

**Response**
+ `Registries` – An array of [RegistryListItem](#aws-glue-api-schema-registry-api-RegistryListItem) objects\.

  An array of `RegistryDetailedListItem` objects containing minimal details of each registry\.
+ `NextToken` – UTF\-8 string\.

  A continuation token for paginating the returned list of tokens, returned if the current segment of the list is not the last\.

**Errors**
+ `InvalidInputException`
+ `AccessDeniedException`
+ `InternalServiceException`

## ListSchemas Action<a name="aws-glue-api-schema-registry-api-ListSchemas"></a>

Returns a list of schemas with minimal details\. Schemas in Deleting status will not be included in the results\. Empty results will be returned if there are no schemas available\.

When the `RegistryId` is not provided, all the schemas across registries will be part of the API response\.

**Request**
+ `RegistryId` – A [RegistryId](#aws-glue-api-schema-registry-api-RegistryId) object\.

  A wrapper structure that may contain the registry name and Amazon Resource Name \(ARN\)\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 100\.

  Maximum number of results required per page\. If the value is not supplied, this will be defaulted to 25 per page\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation call\.

**Response**
+ `Schemas` – An array of [SchemaListItem](#aws-glue-api-schema-registry-api-SchemaListItem) objects\.

  An array of `SchemaListItem` objects containing details of each schema\.
+ `NextToken` – UTF\-8 string\.

  A continuation token for paginating the returned list of tokens, returned if the current segment of the list is not the last\.

**Errors**
+ `InvalidInputException`
+ `AccessDeniedException`
+ `EntityNotFoundException`
+ `InternalServiceException`

## RegisterSchemaVersion Action<a name="aws-glue-api-schema-registry-api-RegisterSchemaVersion"></a>

Adds a new version to the existing schema\. Returns an error if new version of schema does not meet the compatibility requirements of the schema set\. This API will not create a new schema set and will return a 404 error if the schema set is not already present in the Schema Registry\.

If this is the first schema definition to be registered in the Schema Registry, this API will store the schema version and return immediately\. Otherwise, this call has the potential to run longer than other operations due to compatibility modes\. You can call the `GetSchemaVersion` API with the `SchemaVersionId` to check compatibility modes\.

If the same schema definition is already stored in Schema Registry as a version, the schema ID of the existing schema is returned to the caller\.

**Request**
+ `SchemaId` – *Required:* A [SchemaId](#aws-glue-api-schema-registry-api-SchemaId) object\.

  This is a wrapper structure to contain schema identity fields\. The structure contains:
  + SchemaId$SchemaArn: The Amazon Resource Name \(ARN\) of the schema\. Either `SchemaArn` or `SchemaName` and `RegistryName` has to be provided\.
  + SchemaId$SchemaName: The name of the schema\. Either `SchemaArn` or `SchemaName` and `RegistryName` has to be provided\.
+ `SchemaDefinition` – *Required:* UTF\-8 string, not less than 1 or more than 170000 bytes long, matching the [Custom string pattern #17](aws-glue-api-common.md#regex_17)\.

  The schema definition using the `DataFormat` setting for the `SchemaName`\.

**Response**
+ `SchemaVersionId` – UTF\-8 string, not less than 36 or more than 36 bytes long, matching the [Custom string pattern #11](aws-glue-api-common.md#regex_11)\.

  The unique ID that represents the version of this schema\.
+ `VersionNumber` – Number \(long\), not less than 1 or more than 100000\.

  The version of this schema \(for sync flow only, in case this is the first version\)\.
+ `Status` – UTF\-8 string \(valid values: `AVAILABLE` \| `PENDING` \| `FAILURE` \| `DELETING`\)\.

  The status of the schema version\.

**Errors**
+ `InvalidInputException`
+ `AccessDeniedException`
+ `EntityNotFoundException`
+ `ResourceNumberLimitExceededException`
+ `ConcurrentModificationException`
+ `InternalServiceException`

## UpdateSchema Action<a name="aws-glue-api-schema-registry-api-UpdateSchema"></a>

Updates the description, compatibility setting, or version checkpoint for a schema set\.

For updating the compatibility setting, the call will not validate compatibility for the entire set of schema versions with the new compatibility setting\. If the value for `Compatibility` is provided, the `VersionNumber` \(a checkpoint\) is also required\. The API will validate the checkpoint version number for consistency\.

If the value for the `VersionNumber` \(checkpoint\) is provided, `Compatibility` is optional and this can be used to set/reset a checkpoint for the schema\.

This update will happen only if the schema is in the AVAILABLE state\.

**Request**
+ `SchemaId` – *Required:* A [SchemaId](#aws-glue-api-schema-registry-api-SchemaId) object\.

  This is a wrapper structure to contain schema identity fields\. The structure contains:
  + SchemaId$SchemaArn: The Amazon Resource Name \(ARN\) of the schema\. One of `SchemaArn` or `SchemaName` has to be provided\.
  + SchemaId$SchemaName: The name of the schema\. One of `SchemaArn` or `SchemaName` has to be provided\.
+ `SchemaVersionNumber` – A [SchemaVersionNumber](#aws-glue-api-schema-registry-api-SchemaVersionNumber) object\.

  Version number required for check pointing\. One of `VersionNumber` or `Compatibility` has to be provided\.
+ `Compatibility` – UTF\-8 string \(valid values: `NONE` \| `DISABLED` \| `BACKWARD` \| `BACKWARD_ALL` \| `FORWARD` \| `FORWARD_ALL` \| `FULL` \| `FULL_ALL`\)\.

  The new compatibility setting for the schema\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  The new description for the schema\.

**Response**
+ `SchemaArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) of the schema\.
+ `SchemaName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name of the schema\.
+ `RegistryName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name of the registry that contains the schema\.

**Errors**
+ `InvalidInputException`
+ `AccessDeniedException`
+ `EntityNotFoundException`
+ `ConcurrentModificationException`
+ `InternalServiceException`

## CheckSchemaVersionValidity Action<a name="aws-glue-api-schema-registry-api-CheckSchemaVersionValidity"></a>

Validates the supplied schema\. This call has no side effects, it simply validates using the supplied schema using `DataFormat` as the format\. Since it does not take a schema set name, no compatibility checks are performed\.

**Request**
+ `DataFormat` – *Required:* UTF\-8 string \(valid values: `AVRO`\)\.

  The data format of the schema definition\. Currently only `AVRO` is supported\.
+ `SchemaDefinition` – *Required:* UTF\-8 string, not less than 1 or more than 170000 bytes long, matching the [Custom string pattern #17](aws-glue-api-common.md#regex_17)\.

  The definition of the schema that has to be validated\.

**Response**
+ `Valid` – Boolean\.

  Return true, if the schema is valid and false otherwise\.
+ `Error` – UTF\-8 string, not less than 1 or more than 5000 bytes long\.

  A validation failure error message\.

**Errors**
+ `InvalidInputException`
+ `AccessDeniedException`
+ `InternalServiceException`

## UpdateRegistry Action<a name="aws-glue-api-schema-registry-api-UpdateRegistry"></a>

Updates an existing registry which is used to hold a collection of schemas\. The updated properties relate to the registry, and do not modify any of the schemas within the registry\. 

**Request**
+ `RegistryId` – *Required:* A [RegistryId](#aws-glue-api-schema-registry-api-RegistryId) object\.

  This is a wrapper structure that may contain the registry name and Amazon Resource Name \(ARN\)\.
+ `Description` – *Required:* Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of the registry\. If description is not provided, this field will not be updated\.

**Response**
+ `RegistryName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name of the updated registry\.
+ `RegistryArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource name \(ARN\) of the updated registry\.

**Errors**
+ `InvalidInputException`
+ `AccessDeniedException`
+ `EntityNotFoundException`
+ `ConcurrentModificationException`
+ `InternalServiceException`

## GetSchemaByDefinition Action<a name="aws-glue-api-schema-registry-api-GetSchemaByDefinition"></a>

Retrieves a schema by the `SchemaDefinition`\. The schema definition is sent to the Schema Registry, canonicalized, and hashed\. If the hash is matched within the scope of the `SchemaName` or ARN \(or the default registry, if none is supplied\), that schema's metadata is returned\. Otherwise, a 404 or NotFound error is returned\. Schema versions in `Deleted` statuses will not be included in the results\.

**Request**
+ `SchemaId` – *Required:* A [SchemaId](#aws-glue-api-schema-registry-api-SchemaId) object\.

  This is a wrapper structure to contain schema identity fields\. The structure contains:
  + SchemaId$SchemaArn: The Amazon Resource Name \(ARN\) of the schema\. One of `SchemaArn` or `SchemaName` has to be provided\.
  + SchemaId$SchemaName: The name of the schema\. One of `SchemaArn` or `SchemaName` has to be provided\.
+ `SchemaDefinition` – *Required:* UTF\-8 string, not less than 1 or more than 170000 bytes long, matching the [Custom string pattern #17](aws-glue-api-common.md#regex_17)\.

  The definition of the schema for which schema details are required\.

**Response**
+ `SchemaVersionId` – UTF\-8 string, not less than 36 or more than 36 bytes long, matching the [Custom string pattern #11](aws-glue-api-common.md#regex_11)\.

  The schema ID of the schema version\.
+ `SchemaArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) of the schema\.
+ `DataFormat` – UTF\-8 string \(valid values: `AVRO`\)\.

  The data format of the schema definition\. Currently only `AVRO` is supported\.
+ `Status` – UTF\-8 string \(valid values: `AVAILABLE` \| `PENDING` \| `FAILURE` \| `DELETING`\)\.

  The status of the schema version\.
+ `CreatedTime` – UTF\-8 string\.

  The date and time the schema was created\.

**Errors**
+ `InvalidInputException`
+ `AccessDeniedException`
+ `EntityNotFoundException`
+ `InternalServiceException`

## GetRegistry Action<a name="aws-glue-api-schema-registry-api-GetRegistry"></a>

Describes the specified registry in detail\.

**Request**
+ `RegistryId` – *Required:* A [RegistryId](#aws-glue-api-schema-registry-api-RegistryId) object\.

  This is a wrapper structure that may contain the registry name and Amazon Resource Name \(ARN\)\.

**Response**
+ `RegistryName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name of the registry\.
+ `RegistryArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) of the registry\.
+ `Description` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  A description of the registry\.
+ `Status` – UTF\-8 string \(valid values: `AVAILABLE` \| `DELETING`\)\.

  The status of the registry\.
+ `CreatedTime` – UTF\-8 string\.

  The date and time the registry was created\.
+ `UpdatedTime` – UTF\-8 string\.

  The date and time the registry was updated\.

**Errors**
+ `InvalidInputException`
+ `AccessDeniedException`
+ `EntityNotFoundException`
+ `InternalServiceException`

## PutSchemaVersionMetadata Action<a name="aws-glue-api-schema-registry-api-PutSchemaVersionMetadata"></a>

Puts the metadata key value pair for a specified schema version ID\. A maximum of 10 key value pairs will be allowed per schema version\. They can be added over one or more calls\.

**Request**
+ `SchemaId` – A [SchemaId](#aws-glue-api-schema-registry-api-SchemaId) object\.

  The unique ID for the schema\.
+ `SchemaVersionNumber` – A [SchemaVersionNumber](#aws-glue-api-schema-registry-api-SchemaVersionNumber) object\.

  The version number of the schema\.
+ `SchemaVersionId` – UTF\-8 string, not less than 36 or more than 36 bytes long, matching the [Custom string pattern #11](aws-glue-api-common.md#regex_11)\.

  The unique version ID of the schema version\.
+ `MetadataKeyValue` – *Required:* A [MetadataKeyValuePair](#aws-glue-api-schema-registry-api-MetadataKeyValuePair) object\.

  The metadata key's corresponding value\.

**Response**
+ `SchemaArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) for the schema\.
+ `SchemaName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name for the schema\.
+ `RegistryName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name for the registry\.
+ `LatestVersion` – Boolean\.

  The latest version of the schema\.
+ `VersionNumber` – Number \(long\), not less than 1 or more than 100000\.

  The version number of the schema\.
+ `SchemaVersionId` – UTF\-8 string, not less than 36 or more than 36 bytes long, matching the [Custom string pattern #11](aws-glue-api-common.md#regex_11)\.

  The unique version ID of the schema version\.
+ `MetadataKey` – UTF\-8 string, not less than 1 or more than 128 bytes long, matching the [Custom string pattern #18](aws-glue-api-common.md#regex_18)\.

  The metadata key\.
+ `MetadataValue` – UTF\-8 string, not less than 1 or more than 256 bytes long, matching the [Custom string pattern #18](aws-glue-api-common.md#regex_18)\.

  The value of the metadata key\.

**Errors**
+ `InvalidInputException`
+ `AccessDeniedException`
+ `AlreadyExistsException`
+ `EntityNotFoundException`
+ `ResourceNumberLimitExceededException`

## QuerySchemaVersionMetadata Action<a name="aws-glue-api-schema-registry-api-QuerySchemaVersionMetadata"></a>

Queries for the schema version metadata information\. 

**Request**
+ `SchemaId` – A [SchemaId](#aws-glue-api-schema-registry-api-SchemaId) object\.

  A wrapper structure that may contain the schema name and Amazon Resource Name \(ARN\)\.
+ `SchemaVersionNumber` – A [SchemaVersionNumber](#aws-glue-api-schema-registry-api-SchemaVersionNumber) object\.

  The version number of the schema\.
+ `SchemaVersionId` – UTF\-8 string, not less than 36 or more than 36 bytes long, matching the [Custom string pattern #11](aws-glue-api-common.md#regex_11)\.

  The unique version ID of the schema version\.
+ `MetadataList` – An array of [MetadataKeyValuePair](#aws-glue-api-schema-registry-api-MetadataKeyValuePair) objects\.

  Search key\-value pairs for metadata, if they are not provided all the metadata information will be fetched\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 50\.

  Maximum number of results required per page\. If the value is not supplied, this will be defaulted to 25 per page\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation call\.

**Response**
+ `MetadataInfoMap` – A map array of key\-value pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 128 bytes long, matching the [Custom string pattern #18](aws-glue-api-common.md#regex_18)\.

  Each value is a A [MetadataInfo](#aws-glue-api-schema-registry-api-MetadataInfo) object\.

  A map of a metadata key and associated values\.
+ `SchemaVersionId` – UTF\-8 string, not less than 36 or more than 36 bytes long, matching the [Custom string pattern #11](aws-glue-api-common.md#regex_11)\.

  The unique version ID of the schema version\.
+ `NextToken` – UTF\-8 string\.

  A continuation token for paginating the returned list of tokens, returned if the current segment of the list is not the last\.

**Errors**
+ `InvalidInputException`
+ `AccessDeniedException`
+ `EntityNotFoundException`

## RemoveSchemaVersionMetadata Action<a name="aws-glue-api-schema-registry-api-RemoveSchemaVersionMetadata"></a>

Removes a key value pair from the schema version metadata for the specified schema version ID\.

**Request**
+ `SchemaId` – A [SchemaId](#aws-glue-api-schema-registry-api-SchemaId) object\.

  A wrapper structure that may contain the schema name and Amazon Resource Name \(ARN\)\.
+ `SchemaVersionNumber` – A [SchemaVersionNumber](#aws-glue-api-schema-registry-api-SchemaVersionNumber) object\.

  The version number of the schema\.
+ `SchemaVersionId` – UTF\-8 string, not less than 36 or more than 36 bytes long, matching the [Custom string pattern #11](aws-glue-api-common.md#regex_11)\.

  The unique version ID of the schema version\.
+ `MetadataKeyValue` – *Required:* A [MetadataKeyValuePair](#aws-glue-api-schema-registry-api-MetadataKeyValuePair) object\.

  The value of the metadata key\.

**Response**
+ `SchemaArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) of the schema\.
+ `SchemaName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name of the schema\.
+ `RegistryName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name of the registry\.
+ `LatestVersion` – Boolean\.

  The latest version of the schema\.
+ `VersionNumber` – Number \(long\), not less than 1 or more than 100000\.

  The version number of the schema\.
+ `SchemaVersionId` – UTF\-8 string, not less than 36 or more than 36 bytes long, matching the [Custom string pattern #11](aws-glue-api-common.md#regex_11)\.

  The version ID for the schema version\.
+ `MetadataKey` – UTF\-8 string, not less than 1 or more than 128 bytes long, matching the [Custom string pattern #18](aws-glue-api-common.md#regex_18)\.

  The metadata key\.
+ `MetadataValue` – UTF\-8 string, not less than 1 or more than 256 bytes long, matching the [Custom string pattern #18](aws-glue-api-common.md#regex_18)\.

  The value of the metadata key\.

**Errors**
+ `InvalidInputException`
+ `AccessDeniedException`
+ `EntityNotFoundException`

## DeleteRegistry Action<a name="aws-glue-api-schema-registry-api-DeleteRegistry"></a>

Delete the entire registry including schema and all of its versions\. To get the status of the delete operation, you can call the `GetRegistry` API after the asynchronous call\. Deleting a registry will disable all online operations for the registry such as the `UpdateRegistry`, `CreateSchema`, `UpdateSchema`, and `RegisterSchemaVersion` APIs\. 

**Request**
+ `RegistryId` – *Required:* A [RegistryId](#aws-glue-api-schema-registry-api-RegistryId) object\.

  This is a wrapper structure that may contain the registry name and Amazon Resource Name \(ARN\)\.

**Response**
+ `RegistryName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name of the registry being deleted\.
+ `RegistryArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) of the registry being deleted\.
+ `Status` – UTF\-8 string \(valid values: `AVAILABLE` \| `DELETING`\)\.

  The status of the registry\. A successful operation will return the `Deleting` status\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `AccessDeniedException`
+ `ConcurrentModificationException`

## DeleteSchema Action<a name="aws-glue-api-schema-registry-api-DeleteSchema"></a>

Deletes the entire schema set, including the schema set and all of its versions\. To get the status of the delete operation, you can call `GetSchema` API after the asynchronous call\. Deleting a registry will disable all online operations for the schema, such as the `GetSchemaByDefinition`, and `RegisterSchemaVersion` APIs\.

**Request**
+ `SchemaId` – *Required:* A [SchemaId](#aws-glue-api-schema-registry-api-SchemaId) object\.

  This is a wrapper structure that may contain the schema name and Amazon Resource Name \(ARN\)\.

**Response**
+ `SchemaArn` – UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) of the schema being deleted\.
+ `SchemaName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #12](aws-glue-api-common.md#regex_12)\.

  The name of the schema being deleted\.
+ `Status` – UTF\-8 string \(valid values: `AVAILABLE` \| `PENDING` \| `DELETING`\)\.

  The status of the schema\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `AccessDeniedException`
+ `ConcurrentModificationException`

## DeleteSchemaVersions Action<a name="aws-glue-api-schema-registry-api-DeleteSchemaVersions"></a>

Remove versions from the specified schema\. A version number or range may be supplied\. If the compatibility mode forbids deleting of a version that is necessary, such as BACKWARDS\_FULL, an error is returned\. Calling the `GetSchemaVersions` API after this call will list the status of the deleted versions\.

When the range of version numbers contain check pointed version, the API will return a 409 conflict and will not proceed with the deletion\. You have to remove the checkpoint first using the `DeleteSchemaCheckpoint` API before using this API\.

You cannot use the `DeleteSchemaVersions` API to delete the first schema version in the schema set\. The first schema version can only be deleted by the `DeleteSchema` API\. This operation will also delete the attached `SchemaVersionMetadata` under the schema versions\. Hard deletes will be enforced on the database\.

If the compatibility mode forbids deleting of a version that is necessary, such as BACKWARDS\_FULL, an error is returned\.

**Request**
+ `SchemaId` – *Required:* A [SchemaId](#aws-glue-api-schema-registry-api-SchemaId) object\.

  This is a wrapper structure that may contain the schema name and Amazon Resource Name \(ARN\)\.
+ `Versions` – *Required:* UTF\-8 string, not less than 1 or more than 100000 bytes long, matching the [Custom string pattern #19](aws-glue-api-common.md#regex_19)\.

  A version range may be supplied which may be of the format:
  + a single version number, 5
  + a range, 5\-8 : deletes versions 5, 6, 7, 8

**Response**
+ `SchemaVersionErrors` – An array of [SchemaVersionErrorItem](#aws-glue-api-schema-registry-api-SchemaVersionErrorItem) objects\.

  A list of `SchemaVersionErrorItem` objects, each containing an error and schema version\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `AccessDeniedException`
+ `ConcurrentModificationException`