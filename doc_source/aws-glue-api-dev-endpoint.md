# Development Endpoints API<a name="aws-glue-api-dev-endpoint"></a>

The Development Endpoints API describes the AWS Glue API related to testing using a custom DevEndpoint\.

## Data Types<a name="aws-glue-api-dev-endpoint-objects"></a>
+ [DevEndpoint Structure](#aws-glue-api-dev-endpoint-DevEndpoint)
+ [DevEndpointCustomLibraries Structure](#aws-glue-api-dev-endpoint-DevEndpointCustomLibraries)

## DevEndpoint Structure<a name="aws-glue-api-dev-endpoint-DevEndpoint"></a>

A development endpoint where a developer can remotely debug ETL scripts\.

**Fields**
+ `EndpointName` – UTF\-8 string\.

  The name of the DevEndpoint\.
+ `RoleArn` – UTF\-8 string, matching the [AWS IAM ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-iam-arn-id)\.

  The AWS ARN of the IAM role used in this DevEndpoint\.
+ `SecurityGroupIds` – An array of UTF\-8 strings\.

  A list of security group identifiers used in this DevEndpoint\.
+ `SubnetId` – UTF\-8 string\.

  The subnet ID for this DevEndpoint\.
+ `YarnEndpointAddress` – UTF\-8 string\.

  The YARN endpoint address used by this DevEndpoint\.
+ `PrivateAddress` – UTF\-8 string\.

  A private IP address to access the DevEndpoint within a VPC, if the DevEndpoint is created within one\. The PrivateAddress field is present only when you create the DevEndpoint within your virtual private cloud \(VPC\)\.
+ `ZeppelinRemoteSparkInterpreterPort` – Number \(integer\)\.

  The Apache Zeppelin port for the remote Apache Spark interpreter\.
+ `PublicAddress` – UTF\-8 string\.

  The public IP address used by this DevEndpoint\. The PublicAddress field is present only when you create a non\-VPC \(virtual private cloud\) DevEndpoint\.
+ `Status` – UTF\-8 string\.

  The current status of this DevEndpoint\.
+ `NumberOfNodes` – Number \(integer\)\.

  The number of AWS Glue Data Processing Units \(DPUs\) allocated to this DevEndpoint\.
+ `AvailabilityZone` – UTF\-8 string\.

  The AWS availability zone where this DevEndpoint is located\.
+ `VpcId` – UTF\-8 string\.

  The ID of the virtual private cloud \(VPC\) used by this DevEndpoint\.
+ `ExtraPythonLibsS3Path` – UTF\-8 string\.

  Path\(s\) to one or more Python libraries in an S3 bucket that should be loaded in your DevEndpoint\. Multiple values must be complete paths separated by a comma\.

  Please note that only pure Python libraries can currently be used on a DevEndpoint\. Libraries that rely on C extensions, such as the [pandas](http://pandas.pydata.org/) Python data analysis library, are not yet supported\.
+ `ExtraJarsS3Path` – UTF\-8 string\.

  Path to one or more Java Jars in an S3 bucket that should be loaded in your DevEndpoint\.

  Please note that only pure Java/Scala libraries can currently be used on a DevEndpoint\.
+ `FailureReason` – UTF\-8 string\.

  The reason for a current failure in this DevEndpoint\.
+ `LastUpdateStatus` – UTF\-8 string\.

  The status of the last update\.
+ `CreatedTimestamp` – Timestamp\.

  The point in time at which this DevEndpoint was created\.
+ `LastModifiedTimestamp` – Timestamp\.

  The point in time at which this DevEndpoint was last modified\.
+ `PublicKey` – UTF\-8 string\.

  The public key to be used by this DevEndpoint for authentication\. This attribute is provided for backward compatibility, as the recommended attribute to use is public keys\.
+ `PublicKeys` – An array of UTF\-8 strings, not more than 5 strings\.

  A list of public keys to be used by the DevEndpoints for authentication\. The use of this attribute is preferred over a single public key because the public keys allow you to have a different private key per client\.
**Note**  
If you previously created an endpoint with a public key, you must remove that key to be able to set a list of public keys: call the `UpdateDevEndpoint` API with the public key content in the `deletePublicKeys` attribute, and the list of new keys in the `addPublicKeys` attribute\.
+ `SecurityConfiguration` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the SecurityConfiguration structure to be used with this DevEndpoint\.

## DevEndpointCustomLibraries Structure<a name="aws-glue-api-dev-endpoint-DevEndpointCustomLibraries"></a>

Custom libraries to be loaded into a DevEndpoint\.

**Fields**
+ `ExtraPythonLibsS3Path` – UTF\-8 string\.

  Path\(s\) to one or more Python libraries in an S3 bucket that should be loaded in your DevEndpoint\. Multiple values must be complete paths separated by a comma\.

  Please note that only pure Python libraries can currently be used on a DevEndpoint\. Libraries that rely on C extensions, such as the [pandas](http://pandas.pydata.org/) Python data analysis library, are not yet supported\.
+ `ExtraJarsS3Path` – UTF\-8 string\.

  Path to one or more Java Jars in an S3 bucket that should be loaded in your DevEndpoint\.

  Please note that only pure Java/Scala libraries can currently be used on a DevEndpoint\.

## Operations<a name="aws-glue-api-dev-endpoint-actions"></a>
+ [CreateDevEndpoint Action \(Python: create\_dev\_endpoint\)](#aws-glue-api-dev-endpoint-CreateDevEndpoint)
+ [UpdateDevEndpoint Action \(Python: update\_dev\_endpoint\)](#aws-glue-api-dev-endpoint-UpdateDevEndpoint)
+ [DeleteDevEndpoint Action \(Python: delete\_dev\_endpoint\)](#aws-glue-api-dev-endpoint-DeleteDevEndpoint)
+ [GetDevEndpoint Action \(Python: get\_dev\_endpoint\)](#aws-glue-api-dev-endpoint-GetDevEndpoint)
+ [GetDevEndpoints Action \(Python: get\_dev\_endpoints\)](#aws-glue-api-dev-endpoint-GetDevEndpoints)

## CreateDevEndpoint Action \(Python: create\_dev\_endpoint\)<a name="aws-glue-api-dev-endpoint-CreateDevEndpoint"></a>

Creates a new DevEndpoint\.

**Request**
+ `EndpointName` – *Required:* UTF\-8 string\.

  The name to be assigned to the new DevEndpoint\.
+ `RoleArn` – *Required:* UTF\-8 string, matching the [AWS IAM ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-iam-arn-id)\.

  The IAM role for the DevEndpoint\.
+ `SecurityGroupIds` – An array of UTF\-8 strings\.

  Security group IDs for the security groups to be used by the new DevEndpoint\.
+ `SubnetId` – UTF\-8 string\.

  The subnet ID for the new DevEndpoint to use\.
+ `PublicKey` – UTF\-8 string\.

  The public key to be used by this DevEndpoint for authentication\. This attribute is provided for backward compatibility, as the recommended attribute to use is public keys\.
+ `PublicKeys` – An array of UTF\-8 strings, not more than 5 strings\.

  A list of public keys to be used by the DevEndpoints for authentication\. The use of this attribute is preferred over a single public key because the public keys allow you to have a different private key per client\.
**Note**  
If you previously created an endpoint with a public key, you must remove that key to be able to set a list of public keys: call the `UpdateDevEndpoint` API with the public key content in the `deletePublicKeys` attribute, and the list of new keys in the `addPublicKeys` attribute\.
+ `NumberOfNodes` – Number \(integer\)\.

  The number of AWS Glue Data Processing Units \(DPUs\) to allocate to this DevEndpoint\.
+ `ExtraPythonLibsS3Path` – UTF\-8 string\.

  Path\(s\) to one or more Python libraries in an S3 bucket that should be loaded in your DevEndpoint\. Multiple values must be complete paths separated by a comma\.

  Please note that only pure Python libraries can currently be used on a DevEndpoint\. Libraries that rely on C extensions, such as the [pandas](http://pandas.pydata.org/) Python data analysis library, are not yet supported\.
+ `ExtraJarsS3Path` – UTF\-8 string\.

  Path to one or more Java Jars in an S3 bucket that should be loaded in your DevEndpoint\.
+ `SecurityConfiguration` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the SecurityConfiguration structure to be used with this DevEndpoint\.

**Response**
+ `EndpointName` – UTF\-8 string\.

  The name assigned to the new DevEndpoint\.
+ `Status` – UTF\-8 string\.

  The current status of the new DevEndpoint\.
+ `SecurityGroupIds` – An array of UTF\-8 strings\.

  The security groups assigned to the new DevEndpoint\.
+ `SubnetId` – UTF\-8 string\.

  The subnet ID assigned to the new DevEndpoint\.
+ `RoleArn` – UTF\-8 string, matching the [AWS IAM ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-iam-arn-id)\.

  The AWS ARN of the role assigned to the new DevEndpoint\.
+ `YarnEndpointAddress` – UTF\-8 string\.

  The address of the YARN endpoint used by this DevEndpoint\.
+ `ZeppelinRemoteSparkInterpreterPort` – Number \(integer\)\.

  The Apache Zeppelin port for the remote Apache Spark interpreter\.
+ `NumberOfNodes` – Number \(integer\)\.

  The number of AWS Glue Data Processing Units \(DPUs\) allocated to this DevEndpoint\.
+ `AvailabilityZone` – UTF\-8 string\.

  The AWS availability zone where this DevEndpoint is located\.
+ `VpcId` – UTF\-8 string\.

  The ID of the VPC used by this DevEndpoint\.
+ `ExtraPythonLibsS3Path` – UTF\-8 string\.

  Path\(s\) to one or more Python libraries in an S3 bucket that will be loaded in your DevEndpoint\.
+ `ExtraJarsS3Path` – UTF\-8 string\.

  Path to one or more Java Jars in an S3 bucket that will be loaded in your DevEndpoint\.
+ `FailureReason` – UTF\-8 string\.

  The reason for a current failure in this DevEndpoint\.
+ `SecurityConfiguration` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the SecurityConfiguration structure being used with this DevEndpoint\.
+ `CreatedTimestamp` – Timestamp\.

  The point in time at which this DevEndpoint was created\.

**Errors**
+ `AccessDeniedException`
+ `AlreadyExistsException`
+ `IdempotentParameterMismatchException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`
+ `ValidationException`
+ `ResourceNumberLimitExceededException`

## UpdateDevEndpoint Action \(Python: update\_dev\_endpoint\)<a name="aws-glue-api-dev-endpoint-UpdateDevEndpoint"></a>

Updates a specified DevEndpoint\.

**Request**
+ `EndpointName` – *Required:* UTF\-8 string\.

  The name of the DevEndpoint to be updated\.
+ `PublicKey` – UTF\-8 string\.

  The public key for the DevEndpoint to use\.
+ `AddPublicKeys` – An array of UTF\-8 strings, not more than 5 strings\.

  The list of public keys for the DevEndpoint to use\.
+ `DeletePublicKeys` – An array of UTF\-8 strings, not more than 5 strings\.

  The list of public keys to be deleted from the DevEndpoint\.
+ `CustomLibraries` – A [DevEndpointCustomLibraries](#aws-glue-api-dev-endpoint-DevEndpointCustomLibraries) object\.

  Custom Python or Java libraries to be loaded in the DevEndpoint\.
+ `UpdateEtlLibraries` – Boolean\.

  True if the list of custom libraries to be loaded in the development endpoint needs to be updated, or False otherwise\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`
+ `ValidationException`

## DeleteDevEndpoint Action \(Python: delete\_dev\_endpoint\)<a name="aws-glue-api-dev-endpoint-DeleteDevEndpoint"></a>

Deletes a specified DevEndpoint\.

**Request**
+ `EndpointName` – *Required:* UTF\-8 string\.

  The name of the DevEndpoint\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`

## GetDevEndpoint Action \(Python: get\_dev\_endpoint\)<a name="aws-glue-api-dev-endpoint-GetDevEndpoint"></a>

Retrieves information about a specified DevEndpoint\.

**Note**  
When you create a development endpoint in a virtual private cloud \(VPC\), AWS Glue returns only a private IP address, and the public IP address field is not populated\. When you create a non\-VPC development endpoint, AWS Glue returns only a public IP address\.

**Request**
+ `EndpointName` – *Required:* UTF\-8 string\.

  Name of the DevEndpoint for which to retrieve information\.

**Response**
+ `DevEndpoint` – A [DevEndpoint](#aws-glue-api-dev-endpoint-DevEndpoint) object\.

  A DevEndpoint definition\.

**Errors**
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`

## GetDevEndpoints Action \(Python: get\_dev\_endpoints\)<a name="aws-glue-api-dev-endpoint-GetDevEndpoints"></a>

Retrieves all the DevEndpoints in this AWS account\.

**Note**  
When you create a development endpoint in a virtual private cloud \(VPC\), AWS Glue returns only a private IP address and the public IP address field is not populated\. When you create a non\-VPC development endpoint, AWS Glue returns only a public IP address\.

**Request**
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum size of information to return\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation call\.

**Response**
+ `DevEndpoints` – An array of [DevEndpoint](#aws-glue-api-dev-endpoint-DevEndpoint) objects\.

  A list of DevEndpoint definitions\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if not all DevEndpoint definitions have yet been returned\.

**Errors**
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`