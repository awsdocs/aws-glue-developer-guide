# Security APIs in AWS Glue<a name="aws-glue-api-jobs-security"></a>

The Security API describes the security data types, and the API related to security in AWS Glue\.

## Data Types<a name="aws-glue-api-jobs-security-objects"></a>
+ [DataCatalogEncryptionSettings Structure](#aws-glue-api-jobs-security-DataCatalogEncryptionSettings)
+ [EncryptionAtRest Structure](#aws-glue-api-jobs-security-EncryptionAtRest)
+ [ConnectionPasswordEncryption Structure](#aws-glue-api-jobs-security-ConnectionPasswordEncryption)
+ [EncryptionConfiguration Structure](#aws-glue-api-jobs-security-EncryptionConfiguration)
+ [S3Encryption Structure](#aws-glue-api-jobs-security-S3Encryption)
+ [CloudWatchEncryption Structure](#aws-glue-api-jobs-security-CloudWatchEncryption)
+ [JobBookmarksEncryption Structure](#aws-glue-api-jobs-security-JobBookmarksEncryption)
+ [SecurityConfiguration Structure](#aws-glue-api-jobs-security-SecurityConfiguration)

## DataCatalogEncryptionSettings Structure<a name="aws-glue-api-jobs-security-DataCatalogEncryptionSettings"></a>

Contains configuration information for maintaining Data Catalog security\.

**Fields**
+ `EncryptionAtRest` – An [EncryptionAtRest](#aws-glue-api-jobs-security-EncryptionAtRest) object\.

  Specifies the encryption\-at\-rest configuration for the Data Catalog\.
+ `ConnectionPasswordEncryption` – A [ConnectionPasswordEncryption](#aws-glue-api-jobs-security-ConnectionPasswordEncryption) object\.

  When connection password protection is enabled, the Data Catalog uses a customer\-provided key to encrypt the password as part of `CreateConnection` or `UpdateConnection` and store it in the `ENCRYPTED_PASSWORD` field in the connection properties\. You can enable catalog encryption or only password encryption\.

## EncryptionAtRest Structure<a name="aws-glue-api-jobs-security-EncryptionAtRest"></a>

Specifies the encryption\-at\-rest configuration for the Data Catalog\.

**Fields**
+ `CatalogEncryptionMode` – *Required:* UTF\-8 string \(valid values: `DISABLED` \| `SSE-KMS="SSEKMS"`\)\.

  The encryption\-at\-rest mode for encrypting Data Catalog data\.
+ `SseAwsKmsKeyId` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the AWS KMS key to use for encryption at rest\.

## ConnectionPasswordEncryption Structure<a name="aws-glue-api-jobs-security-ConnectionPasswordEncryption"></a>

The data structure used by the Data Catalog to encrypt the password as part of `CreateConnection` or `UpdateConnection` and store it in the `ENCRYPTED_PASSWORD` field in the connection properties\. You can enable catalog encryption or only password encryption\.

When a `CreationConnection` request arrives containing a password, the Data Catalog first encrypts the password using your AWS KMS key\. It then encrypts the whole connection object again if catalog encryption is also enabled\.

This encryption requires that you set AWS KMS key permissions to enable or restrict access on the password key according to your security requirements\. For example, you might want only administrators to have decrypt permission on the password key\.

**Fields**
+ `ReturnConnectionPasswordEncrypted` – *Required:* Boolean\.

  When the `ReturnConnectionPasswordEncrypted` flag is set to "true", passwords remain encrypted in the responses of `GetConnection` and `GetConnections`\. This encryption takes effect independently from catalog encryption\. 
+ `AwsKmsKeyId` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  An AWS KMS key that is used to encrypt the connection password\. 

  If connection password protection is enabled, the caller of `CreateConnection` and `UpdateConnection` needs at least `kms:Encrypt` permission on the specified AWS KMS key, to encrypt passwords before storing them in the Data Catalog\. 

  You can set the decrypt permission to enable or restrict access on the password key according to your security requirements\.

## EncryptionConfiguration Structure<a name="aws-glue-api-jobs-security-EncryptionConfiguration"></a>

Specifies an encryption configuration\.

**Fields**
+ `S3Encryption` – An array of [S3Encryption](#aws-glue-api-jobs-security-S3Encryption) objects\.

  The encryption configuration for Amazon Simple Storage Service \(Amazon S3\) data\.
+ `CloudWatchEncryption` – A [CloudWatchEncryption](#aws-glue-api-jobs-security-CloudWatchEncryption) object\.

  The encryption configuration for Amazon CloudWatch\.
+ `JobBookmarksEncryption` – A [JobBookmarksEncryption](#aws-glue-api-jobs-security-JobBookmarksEncryption) object\.

  The encryption configuration for job bookmarks\.

## S3Encryption Structure<a name="aws-glue-api-jobs-security-S3Encryption"></a>

Specifies how Amazon Simple Storage Service \(Amazon S3\) data should be encrypted\.

**Fields**
+ `S3EncryptionMode` – UTF\-8 string \(valid values: `DISABLED` \| `SSE-KMS="SSEKMS"` \| `SSE-S3="SSES3"`\)\.

  The encryption mode to use for Amazon S3 data\.
+ `KmsKeyArn` – UTF\-8 string, matching the [Custom string pattern #14](aws-glue-api-common.md#regex_14)\.

  The Amazon Resource Name \(ARN\) of the KMS key to be used to encrypt the data\.

## CloudWatchEncryption Structure<a name="aws-glue-api-jobs-security-CloudWatchEncryption"></a>

Specifies how Amazon CloudWatch data should be encrypted\.

**Fields**
+ `CloudWatchEncryptionMode` – UTF\-8 string \(valid values: `DISABLED` \| `SSE-KMS="SSEKMS"`\)\.

  The encryption mode to use for CloudWatch data\.
+ `KmsKeyArn` – UTF\-8 string, matching the [Custom string pattern #14](aws-glue-api-common.md#regex_14)\.

  The Amazon Resource Name \(ARN\) of the KMS key to be used to encrypt the data\.

## JobBookmarksEncryption Structure<a name="aws-glue-api-jobs-security-JobBookmarksEncryption"></a>

Specifies how job bookmark data should be encrypted\.

**Fields**
+ `JobBookmarksEncryptionMode` – UTF\-8 string \(valid values: `DISABLED` \| `CSE-KMS="CSEKMS"`\)\.

  The encryption mode to use for job bookmarks data\.
+ `KmsKeyArn` – UTF\-8 string, matching the [Custom string pattern #14](aws-glue-api-common.md#regex_14)\.

  The Amazon Resource Name \(ARN\) of the KMS key to be used to encrypt the data\.

## SecurityConfiguration Structure<a name="aws-glue-api-jobs-security-SecurityConfiguration"></a>

Specifies a security configuration\.

**Fields**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the security configuration\.
+ `CreatedTimeStamp` – Timestamp\.

  The time at which this security configuration was created\.
+ `EncryptionConfiguration` – An [EncryptionConfiguration](#aws-glue-api-jobs-security-EncryptionConfiguration) object\.

  The encryption configuration associated with this security configuration\.

## Operations<a name="aws-glue-api-jobs-security-actions"></a>
+ [GetDataCatalogEncryptionSettings Action \(Python: get\_data\_catalog\_encryption\_settings\)](#aws-glue-api-jobs-security-GetDataCatalogEncryptionSettings)
+ [PutDataCatalogEncryptionSettings Action \(Python: put\_data\_catalog\_encryption\_settings\)](#aws-glue-api-jobs-security-PutDataCatalogEncryptionSettings)
+ [PutResourcePolicy Action \(Python: put\_resource\_policy\)](#aws-glue-api-jobs-security-PutResourcePolicy)
+ [GetResourcePolicy Action \(Python: get\_resource\_policy\)](#aws-glue-api-jobs-security-GetResourcePolicy)
+ [DeleteResourcePolicy Action \(Python: delete\_resource\_policy\)](#aws-glue-api-jobs-security-DeleteResourcePolicy)
+ [CreateSecurityConfiguration Action \(Python: create\_security\_configuration\)](#aws-glue-api-jobs-security-CreateSecurityConfiguration)
+ [DeleteSecurityConfiguration Action \(Python: delete\_security\_configuration\)](#aws-glue-api-jobs-security-DeleteSecurityConfiguration)
+ [GetSecurityConfiguration Action \(Python: get\_security\_configuration\)](#aws-glue-api-jobs-security-GetSecurityConfiguration)
+ [GetSecurityConfigurations Action \(Python: get\_security\_configurations\)](#aws-glue-api-jobs-security-GetSecurityConfigurations)

## GetDataCatalogEncryptionSettings Action \(Python: get\_data\_catalog\_encryption\_settings\)<a name="aws-glue-api-jobs-security-GetDataCatalogEncryptionSettings"></a>

Retrieves the security configuration for a specified catalog\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog to retrieve the security configuration for\. If none is provided, the AWS account ID is used by default\.

**Response**
+ `DataCatalogEncryptionSettings` – A [DataCatalogEncryptionSettings](#aws-glue-api-jobs-security-DataCatalogEncryptionSettings) object\.

  The requested security configuration\.

**Errors**
+ `InternalServiceException`
+ `InvalidInputException`
+ `OperationTimeoutException`

## PutDataCatalogEncryptionSettings Action \(Python: put\_data\_catalog\_encryption\_settings\)<a name="aws-glue-api-jobs-security-PutDataCatalogEncryptionSettings"></a>

Sets the security configuration for a specified catalog\. After the configuration has been set, the specified encryption is applied to every catalog write thereafter\.

**Request**
+ `CatalogId` – Catalog id string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The ID of the Data Catalog to set the security configuration for\. If none is provided, the AWS account ID is used by default\.
+ `DataCatalogEncryptionSettings` – *Required:* A [DataCatalogEncryptionSettings](#aws-glue-api-jobs-security-DataCatalogEncryptionSettings) object\.

  The security configuration to set\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `InternalServiceException`
+ `InvalidInputException`
+ `OperationTimeoutException`

## PutResourcePolicy Action \(Python: put\_resource\_policy\)<a name="aws-glue-api-jobs-security-PutResourcePolicy"></a>

Sets the Data Catalog resource policy for access control\.

**Request**
+ `PolicyInJson` – *Required:* UTF\-8 string, not less than 2 or more than 10240 bytes long\.

  Contains the policy document to set, in JSON format\.
+ `PolicyHashCondition` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The hash value returned when the previous policy was set using `PutResourcePolicy`\. Its purpose is to prevent concurrent modifications of a policy\. Do not use this parameter if no previous policy has been set\.
+ `PolicyExistsCondition` – UTF\-8 string \(valid values: `MUST_EXIST` \| `NOT_EXIST` \| `NONE`\)\.

  A value of `MUST_EXIST` is used to update a policy\. A value of `NOT_EXIST` is used to create a new policy\. If a value of `NONE` or a null value is used, the call will not depend on the existence of a policy\.

**Response**
+ `PolicyHash` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  A hash of the policy that has just been set\. This must be included in a subsequent call that overwrites or updates this policy\.

**Errors**
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`
+ `ConditionCheckFailureException`

## GetResourcePolicy Action \(Python: get\_resource\_policy\)<a name="aws-glue-api-jobs-security-GetResourcePolicy"></a>

Retrieves a specified resource policy\.

**Request**
+ *No Request parameters\.*

**Response**
+ `PolicyInJson` – UTF\-8 string, not less than 2 or more than 10240 bytes long\.

  Contains the requested policy document, in JSON format\.
+ `PolicyHash` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Contains the hash value associated with this policy\.
+ `CreateTime` – Timestamp\.

  The date and time at which the policy was created\.
+ `UpdateTime` – Timestamp\.

  The date and time at which the policy was last updated\.

**Errors**
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`

## DeleteResourcePolicy Action \(Python: delete\_resource\_policy\)<a name="aws-glue-api-jobs-security-DeleteResourcePolicy"></a>

Deletes a specified policy\.

**Request**
+ `PolicyHashCondition` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The hash value returned when this policy was set\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`
+ `ConditionCheckFailureException`

## CreateSecurityConfiguration Action \(Python: create\_security\_configuration\)<a name="aws-glue-api-jobs-security-CreateSecurityConfiguration"></a>

Creates a new security configuration\. A security configuration is a set of security properties that can be used by AWS Glue\. You can use a security configuration to encrypt data at rest\. For information about using security configurations in AWS Glue, see [Encrypting Data Written by Crawlers, Jobs, and Development Endpoints](https://docs.aws.amazon.com/glue/latest/dg/encryption-security-configuration.html)\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name for the new security configuration\.
+ `EncryptionConfiguration` – *Required:* An [EncryptionConfiguration](#aws-glue-api-jobs-security-EncryptionConfiguration) object\.

  The encryption configuration for the new security configuration\.

**Response**
+ `Name` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name assigned to the new security configuration\.
+ `CreatedTimestamp` – Timestamp\.

  The time at which the new security configuration was created\.

**Errors**
+ `AlreadyExistsException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `ResourceNumberLimitExceededException`

## DeleteSecurityConfiguration Action \(Python: delete\_security\_configuration\)<a name="aws-glue-api-jobs-security-DeleteSecurityConfiguration"></a>

Deletes a specified security configuration\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the security configuration to delete\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## GetSecurityConfiguration Action \(Python: get\_security\_configuration\)<a name="aws-glue-api-jobs-security-GetSecurityConfiguration"></a>

Retrieves a specified security configuration\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the security configuration to retrieve\.

**Response**
+ `SecurityConfiguration` – A [SecurityConfiguration](#aws-glue-api-jobs-security-SecurityConfiguration) object\.

  The requested security configuration\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`

## GetSecurityConfigurations Action \(Python: get\_security\_configurations\)<a name="aws-glue-api-jobs-security-GetSecurityConfigurations"></a>

Retrieves a list of all security configurations\.

**Request**
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum number of results to return\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation call\.

**Response**
+ `SecurityConfigurations` – An array of [SecurityConfiguration](#aws-glue-api-jobs-security-SecurityConfiguration) objects\.

  A list of security configurations\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if there are more security configurations to return\.

**Errors**
+ `EntityNotFoundException`
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`