# Development Endpoints API<a name="aws-glue-api-dev-endpoint"></a>

The Development Endpoints API describes the AWS Glue API related to testing using a custom DevEndpoint\.

## Data Types<a name="aws-glue-api-dev-endpoint-objects"></a>
+ [DevEndpoint Structure](#aws-glue-api-dev-endpoint-DevEndpoint)
+ [DevEndpointCustomLibraries Structure](#aws-glue-api-dev-endpoint-DevEndpointCustomLibraries)

## DevEndpoint Structure<a name="aws-glue-api-dev-endpoint-DevEndpoint"></a>

A development endpoint where a developer can remotely debug extract, transform, and load \(ETL\) scripts\.

**Fields**
+ `EndpointName` – UTF\-8 string\.

  The name of the `DevEndpoint`\.
+ `RoleArn` – UTF\-8 string, matching the [AWS IAM ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-iam-arn-id)\.

  The Amazon Resource Name \(ARN\) of the IAM role used in this `DevEndpoint`\.
+ `SecurityGroupIds` – An array of UTF\-8 strings\.

  A list of security group identifiers used in this `DevEndpoint`\.
+ `SubnetId` – UTF\-8 string\.

  The subnet ID for this `DevEndpoint`\.
+ `YarnEndpointAddress` – UTF\-8 string\.

  The YARN endpoint address used by this `DevEndpoint`\.
+ `PrivateAddress` – UTF\-8 string\.

  A private IP address to access the `DevEndpoint` within a VPC if the `DevEndpoint` is created within one\. The `PrivateAddress` field is present only when you create the `DevEndpoint` within your VPC\.
+ `ZeppelinRemoteSparkInterpreterPort` – Number \(integer\)\.

  The Apache Zeppelin port for the remote Apache Spark interpreter\.
+ `PublicAddress` – UTF\-8 string\.

  The public IP address used by this `DevEndpoint`\. The `PublicAddress` field is present only when you create a non\-virtual private cloud \(VPC\) `DevEndpoint`\.
+ `Status` – UTF\-8 string\.

  The current status of this `DevEndpoint`\.
+ `WorkerType` – UTF\-8 string \(valid values: `Standard=""` \| `G.1X=""` \| `G.2X=""`\)\.

  The type of predefined worker that is allocated to the development endpoint\. Accepts a value of Standard, G\.1X, or G\.2X\.
  + For the `Standard` worker type, each worker provides 4 vCPU, 16 GB of memory and a 50GB disk, and 2 executors per worker\.
  + For the `G.1X` worker type, each worker maps to 1 DPU \(4 vCPU, 16 GB of memory, 64 GB disk\), and provides 1 executor per worker\. We recommend this worker type for memory\-intensive jobs\.
  + For the `G.2X` worker type, each worker maps to 2 DPU \(8 vCPU, 32 GB of memory, 128 GB disk\), and provides 1 executor per worker\. We recommend this worker type for memory\-intensive jobs\.

  Known issue: when a development endpoint is created with the `G.2X` `WorkerType` configuration, the Spark drivers for the development endpoint will run on 4 vCPU, 16 GB of memory, and a 64 GB disk\. 
+ `GlueVersion` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #13](aws-glue-api-common.md#regex_13)\.

  Glue version determines the versions of Apache Spark and Python that AWS Glue supports\. The Python version indicates the version supported for running your ETL scripts on development endpoints\. 

  For more information about the available AWS Glue versions and corresponding Spark and Python versions, see [Glue version](https://docs.aws.amazon.com/glue/latest/dg/add-job.html) in the developer guide\.

  Development endpoints that are created without specifying a Glue version default to Glue 0\.9\.

  You can specify a version of Python support for development endpoints by using the `Arguments` parameter in the `CreateDevEndpoint` or `UpdateDevEndpoint` APIs\. If no arguments are provided, the version defaults to Python 2\.
+ `NumberOfWorkers` – Number \(integer\)\.

  The number of workers of a defined `workerType` that are allocated to the development endpoint\.

  The maximum number of workers you can define are 299 for `G.1X`, and 149 for `G.2X`\. 
+ `NumberOfNodes` – Number \(integer\)\.

  The number of AWS Glue Data Processing Units \(DPUs\) allocated to this `DevEndpoint`\.
+ `AvailabilityZone` – UTF\-8 string\.

  The AWS Availability Zone where this `DevEndpoint` is located\.
+ `VpcId` – UTF\-8 string\.

  The ID of the virtual private cloud \(VPC\) used by this `DevEndpoint`\.
+ `ExtraPythonLibsS3Path` – UTF\-8 string\.

  The paths to one or more Python libraries in an Amazon S3 bucket that should be loaded in your `DevEndpoint`\. Multiple values must be complete paths separated by a comma\.
**Note**  
You can only use pure Python libraries with a `DevEndpoint`\. Libraries that rely on C extensions, such as the [pandas](http://pandas.pydata.org/) Python data analysis library, are not currently supported\.
+ `ExtraJarsS3Path` – UTF\-8 string\.

  The path to one or more Java `.jar` files in an S3 bucket that should be loaded in your `DevEndpoint`\.
**Note**  
You can only use pure Java/Scala libraries with a `DevEndpoint`\.
+ `FailureReason` – UTF\-8 string\.

  The reason for a current failure in this `DevEndpoint`\.
+ `LastUpdateStatus` – UTF\-8 string\.

  The status of the last update\.
+ `CreatedTimestamp` – Timestamp\.

  The point in time at which this DevEndpoint was created\.
+ `LastModifiedTimestamp` – Timestamp\.

  The point in time at which this `DevEndpoint` was last modified\.
+ `PublicKey` – UTF\-8 string\.

  The public key to be used by this `DevEndpoint` for authentication\. This attribute is provided for backward compatibility because the recommended attribute to use is public keys\.
+ `PublicKeys` – An array of UTF\-8 strings, not more than 5 strings\.

  A list of public keys to be used by the `DevEndpoints` for authentication\. Using this attribute is preferred over a single public key because the public keys allow you to have a different private key per client\.
**Note**  
If you previously created an endpoint with a public key, you must remove that key to be able to set a list of public keys\. Call the `UpdateDevEndpoint` API operation with the public key content in the `deletePublicKeys` attribute, and the list of new keys in the `addPublicKeys` attribute\.
+ `SecurityConfiguration` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the `SecurityConfiguration` structure to be used with this `DevEndpoint`\.
+ `Arguments` – A map array of key\-value pairs, not more than 100 pairs\.

  Each key is a UTF\-8 string\.

  Each value is a UTF\-8 string\.

  A map of arguments used to configure the `DevEndpoint`\.

  Valid arguments are:
  + `"--enable-glue-datacatalog": ""`

  You can specify a version of Python support for development endpoints by using the `Arguments` parameter in the `CreateDevEndpoint` or `UpdateDevEndpoint` APIs\. If no arguments are provided, the version defaults to Python 2\.

## DevEndpointCustomLibraries Structure<a name="aws-glue-api-dev-endpoint-DevEndpointCustomLibraries"></a>

Custom libraries to be loaded into a development endpoint\.

**Fields**
+ `ExtraPythonLibsS3Path` – UTF\-8 string\.

  The paths to one or more Python libraries in an Amazon Simple Storage Service \(Amazon S3\) bucket that should be loaded in your `DevEndpoint`\. Multiple values must be complete paths separated by a comma\.
**Note**  
You can only use pure Python libraries with a `DevEndpoint`\. Libraries that rely on C extensions, such as the [pandas](http://pandas.pydata.org/) Python data analysis library, are not currently supported\.
+ `ExtraJarsS3Path` – UTF\-8 string\.

  The path to one or more Java `.jar` files in an S3 bucket that should be loaded in your `DevEndpoint`\.
**Note**  
You can only use pure Java/Scala libraries with a `DevEndpoint`\.

## Operations<a name="aws-glue-api-dev-endpoint-actions"></a>
+ [CreateDevEndpoint Action \(Python: create\_dev\_endpoint\)](#aws-glue-api-dev-endpoint-CreateDevEndpoint)
+ [UpdateDevEndpoint Action \(Python: update\_dev\_endpoint\)](#aws-glue-api-dev-endpoint-UpdateDevEndpoint)
+ [DeleteDevEndpoint Action \(Python: delete\_dev\_endpoint\)](#aws-glue-api-dev-endpoint-DeleteDevEndpoint)
+ [GetDevEndpoint Action \(Python: get\_dev\_endpoint\)](#aws-glue-api-dev-endpoint-GetDevEndpoint)
+ [GetDevEndpoints Action \(Python: get\_dev\_endpoints\)](#aws-glue-api-dev-endpoint-GetDevEndpoints)
+ [BatchGetDevEndpoints Action \(Python: batch\_get\_dev\_endpoints\)](#aws-glue-api-dev-endpoint-BatchGetDevEndpoints)
+ [ListDevEndpoints Action \(Python: list\_dev\_endpoints\)](#aws-glue-api-dev-endpoint-ListDevEndpoints)

## CreateDevEndpoint Action \(Python: create\_dev\_endpoint\)<a name="aws-glue-api-dev-endpoint-CreateDevEndpoint"></a>

Creates a new development endpoint\.

**Request**
+ `EndpointName` – *Required:* UTF\-8 string\.

  The name to be assigned to the new `DevEndpoint`\.
+ `RoleArn` – *Required:* UTF\-8 string, matching the [AWS IAM ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-iam-arn-id)\.

  The IAM role for the `DevEndpoint`\.
+ `SecurityGroupIds` – An array of UTF\-8 strings\.

  Security group IDs for the security groups to be used by the new `DevEndpoint`\.
+ `SubnetId` – UTF\-8 string\.

  The subnet ID for the new `DevEndpoint` to use\.
+ `PublicKey` – UTF\-8 string\.

  The public key to be used by this `DevEndpoint` for authentication\. This attribute is provided for backward compatibility because the recommended attribute to use is public keys\.
+ `PublicKeys` – An array of UTF\-8 strings, not more than 5 strings\.

  A list of public keys to be used by the development endpoints for authentication\. The use of this attribute is preferred over a single public key because the public keys allow you to have a different private key per client\.
**Note**  
If you previously created an endpoint with a public key, you must remove that key to be able to set a list of public keys\. Call the `UpdateDevEndpoint` API with the public key content in the `deletePublicKeys` attribute, and the list of new keys in the `addPublicKeys` attribute\.
+ `NumberOfNodes` – Number \(integer\)\.

  The number of AWS Glue Data Processing Units \(DPUs\) to allocate to this `DevEndpoint`\.
+ `WorkerType` – UTF\-8 string \(valid values: `Standard=""` \| `G.1X=""` \| `G.2X=""`\)\.

  The type of predefined worker that is allocated to the development endpoint\. Accepts a value of Standard, G\.1X, or G\.2X\.
  + For the `Standard` worker type, each worker provides 4 vCPU, 16 GB of memory and a 50GB disk, and 2 executors per worker\.
  + For the `G.1X` worker type, each worker maps to 1 DPU \(4 vCPU, 16 GB of memory, 64 GB disk\), and provides 1 executor per worker\. We recommend this worker type for memory\-intensive jobs\.
  + For the `G.2X` worker type, each worker maps to 2 DPU \(8 vCPU, 32 GB of memory, 128 GB disk\), and provides 1 executor per worker\. We recommend this worker type for memory\-intensive jobs\.

  Known issue: when a development endpoint is created with the `G.2X` `WorkerType` configuration, the Spark drivers for the development endpoint will run on 4 vCPU, 16 GB of memory, and a 64 GB disk\. 
+ `GlueVersion` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #13](aws-glue-api-common.md#regex_13)\.

  Glue version determines the versions of Apache Spark and Python that AWS Glue supports\. The Python version indicates the version supported for running your ETL scripts on development endpoints\. 

  For more information about the available AWS Glue versions and corresponding Spark and Python versions, see [Glue version](https://docs.aws.amazon.com/glue/latest/dg/add-job.html) in the developer guide\.

  Development endpoints that are created without specifying a Glue version default to Glue 0\.9\.

  You can specify a version of Python support for development endpoints by using the `Arguments` parameter in the `CreateDevEndpoint` or `UpdateDevEndpoint` APIs\. If no arguments are provided, the version defaults to Python 2\.
+ `NumberOfWorkers` – Number \(integer\)\.

  The number of workers of a defined `workerType` that are allocated to the development endpoint\.

  The maximum number of workers you can define are 299 for `G.1X`, and 149 for `G.2X`\. 
+ `ExtraPythonLibsS3Path` – UTF\-8 string\.

  The paths to one or more Python libraries in an Amazon S3 bucket that should be loaded in your `DevEndpoint`\. Multiple values must be complete paths separated by a comma\.
**Note**  
You can only use pure Python libraries with a `DevEndpoint`\. Libraries that rely on C extensions, such as the [pandas](http://pandas.pydata.org/) Python data analysis library, are not yet supported\.
+ `ExtraJarsS3Path` – UTF\-8 string\.

  The path to one or more Java `.jar` files in an S3 bucket that should be loaded in your `DevEndpoint`\.
+ `SecurityConfiguration` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the `SecurityConfiguration` structure to be used with this `DevEndpoint`\.
+ `Tags` – A map array of key\-value pairs, not more than 50 pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 128 bytes long\.

  Each value is a UTF\-8 string, not more than 256 bytes long\.

  The tags to use with this DevEndpoint\. You may use tags to limit access to the DevEndpoint\. For more information about tags in AWS Glue, see [AWS Tags in AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/monitor-tags.html) in the developer guide\.
+ `Arguments` – A map array of key\-value pairs, not more than 100 pairs\.

  Each key is a UTF\-8 string\.

  Each value is a UTF\-8 string\.

  A map of arguments used to configure the `DevEndpoint`\.

**Response**
+ `EndpointName` – UTF\-8 string\.

  The name assigned to the new `DevEndpoint`\.
+ `Status` – UTF\-8 string\.

  The current status of the new `DevEndpoint`\.
+ `SecurityGroupIds` – An array of UTF\-8 strings\.

  The security groups assigned to the new `DevEndpoint`\.
+ `SubnetId` – UTF\-8 string\.

  The subnet ID assigned to the new `DevEndpoint`\.
+ `RoleArn` – UTF\-8 string, matching the [AWS IAM ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-iam-arn-id)\.

  The Amazon Resource Name \(ARN\) of the role assigned to the new `DevEndpoint`\.
+ `YarnEndpointAddress` – UTF\-8 string\.

  The address of the YARN endpoint used by this `DevEndpoint`\.
+ `ZeppelinRemoteSparkInterpreterPort` – Number \(integer\)\.

  The Apache Zeppelin port for the remote Apache Spark interpreter\.
+ `NumberOfNodes` – Number \(integer\)\.

  The number of AWS Glue Data Processing Units \(DPUs\) allocated to this DevEndpoint\.
+ `WorkerType` – UTF\-8 string \(valid values: `Standard=""` \| `G.1X=""` \| `G.2X=""`\)\.

  The type of predefined worker that is allocated to the development endpoint\. May be a value of Standard, G\.1X, or G\.2X\.
+ `GlueVersion` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Custom string pattern #13](aws-glue-api-common.md#regex_13)\.

  Glue version determines the versions of Apache Spark and Python that AWS Glue supports\. The Python version indicates the version supported for running your ETL scripts on development endpoints\. 

  For more information about the available AWS Glue versions and corresponding Spark and Python versions, see [Glue version](https://docs.aws.amazon.com/glue/latest/dg/add-job.html) in the developer guide\.
+ `NumberOfWorkers` – Number \(integer\)\.

  The number of workers of a defined `workerType` that are allocated to the development endpoint\.
+ `AvailabilityZone` – UTF\-8 string\.

  The AWS Availability Zone where this `DevEndpoint` is located\.
+ `VpcId` – UTF\-8 string\.

  The ID of the virtual private cloud \(VPC\) used by this `DevEndpoint`\.
+ `ExtraPythonLibsS3Path` – UTF\-8 string\.

  The paths to one or more Python libraries in an S3 bucket that will be loaded in your `DevEndpoint`\.
+ `ExtraJarsS3Path` – UTF\-8 string\.

  Path to one or more Java `.jar` files in an S3 bucket that will be loaded in your `DevEndpoint`\.
+ `FailureReason` – UTF\-8 string\.

  The reason for a current failure in this `DevEndpoint`\.
+ `SecurityConfiguration` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the `SecurityConfiguration` structure being used with this `DevEndpoint`\.
+ `CreatedTimestamp` – Timestamp\.

  The point in time at which this `DevEndpoint` was created\.
+ `Arguments` – A map array of key\-value pairs, not more than 100 pairs\.

  Each key is a UTF\-8 string\.

  Each value is a UTF\-8 string\.

  The map of arguments used to configure this `DevEndpoint`\.

  Valid arguments are:
  + `"--enable-glue-datacatalog": ""`

  You can specify a version of Python support for development endpoints by using the `Arguments` parameter in the `CreateDevEndpoint` or `UpdateDevEndpoint` APIs\. If no arguments are provided, the version defaults to Python 2\.

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

Updates a specified development endpoint\.

**Request**
+ `EndpointName` – *Required:* UTF\-8 string\.

  The name of the `DevEndpoint` to be updated\.
+ `PublicKey` – UTF\-8 string\.

  The public key for the `DevEndpoint` to use\.
+ `AddPublicKeys` – An array of UTF\-8 strings, not more than 5 strings\.

  The list of public keys for the `DevEndpoint` to use\.
+ `DeletePublicKeys` – An array of UTF\-8 strings, not more than 5 strings\.

  The list of public keys to be deleted from the `DevEndpoint`\.
+ `CustomLibraries` – A [DevEndpointCustomLibraries](#aws-glue-api-dev-endpoint-DevEndpointCustomLibraries) object\.

  Custom Python or Java libraries to be loaded in the `DevEndpoint`\.
+ `UpdateEtlLibraries` – Boolean\.

  `True` if the list of custom libraries to be loaded in the development endpoint needs to be updated, or `False` if otherwise\.
+ `DeleteArguments` – An array of UTF\-8 strings\.

  The list of argument keys to be deleted from the map of arguments used to configure the `DevEndpoint`\.
+ `AddArguments` – A map array of key\-value pairs, not more than 100 pairs\.

  Each key is a UTF\-8 string\.

  Each value is a UTF\-8 string\.

  The map of arguments to add the map of arguments used to configure the `DevEndpoint`\.

  Valid arguments are:
  + `"--enable-glue-datacatalog": ""`

  You can specify a version of Python support for development endpoints by using the `Arguments` parameter in the `CreateDevEndpoint` or `UpdateDevEndpoint` APIs\. If no arguments are provided, the version defaults to Python 2\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`
+ `ValidationException`

## DeleteDevEndpoint Action \(Python: delete\_dev\_endpoint\)<a name="aws-glue-api-dev-endpoint-DeleteDevEndpoint"></a>

Deletes a specified development endpoint\.

**Request**
+ `EndpointName` – *Required:* UTF\-8 string\.

  The name of the `DevEndpoint`\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`

## GetDevEndpoint Action \(Python: get\_dev\_endpoint\)<a name="aws-glue-api-dev-endpoint-GetDevEndpoint"></a>

Retrieves information about a specified development endpoint\.

**Note**  
When you create a development endpoint in a virtual private cloud \(VPC\), AWS Glue returns only a private IP address, and the public IP address field is not populated\. When you create a non\-VPC development endpoint, AWS Glue returns only a public IP address\.

**Request**
+ `EndpointName` – *Required:* UTF\-8 string\.

  Name of the `DevEndpoint` to retrieve information for\.

**Response**
+ `DevEndpoint` – A [DevEndpoint](#aws-glue-api-dev-endpoint-DevEndpoint) object\.

  A `DevEndpoint` definition\.

**Errors**
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`

## GetDevEndpoints Action \(Python: get\_dev\_endpoints\)<a name="aws-glue-api-dev-endpoint-GetDevEndpoints"></a>

Retrieves all the development endpoints in this AWS account\.

**Note**  
When you create a development endpoint in a virtual private cloud \(VPC\), AWS Glue returns only a private IP address and the public IP address field is not populated\. When you create a non\-VPC development endpoint, AWS Glue returns only a public IP address\.

**Request**
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum size of information to return\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation call\.

**Response**
+ `DevEndpoints` – An array of [DevEndpoint](#aws-glue-api-dev-endpoint-DevEndpoint) objects\.

  A list of `DevEndpoint` definitions\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if not all `DevEndpoint` definitions have yet been returned\.

**Errors**
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`

## BatchGetDevEndpoints Action \(Python: batch\_get\_dev\_endpoints\)<a name="aws-glue-api-dev-endpoint-BatchGetDevEndpoints"></a>

Returns a list of resource metadata for a given list of development endpoint names\. After calling the `ListDevEndpoints` operation, you can call this operation to access the data to which you have been granted permissions\. This operation supports all IAM permissions, including permission conditions that uses tags\.

**Request**
+ `customerAccountId` – UTF\-8 string\.

  The AWS account ID\.
+ `DevEndpointNames` – *Required:* An array of UTF\-8 strings, not less than 1 or more than 25 strings\.

  The list of `DevEndpoint` names, which might be the names returned from the `ListDevEndpoint` operation\.

**Response**
+ `DevEndpoints` – An array of [DevEndpoint](#aws-glue-api-dev-endpoint-DevEndpoint) objects\.

  A list of `DevEndpoint` definitions\.
+ `DevEndpointsNotFound` – An array of UTF\-8 strings, not less than 1 or more than 25 strings\.

  A list of `DevEndpoints` not found\.

**Errors**
+ `AccessDeniedException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `InvalidInputException`

## ListDevEndpoints Action \(Python: list\_dev\_endpoints\)<a name="aws-glue-api-dev-endpoint-ListDevEndpoints"></a>

Retrieves the names of all `DevEndpoint` resources in this AWS account, or the resources with the specified tag\. This operation allows you to see which resources are available in your account, and their names\.

This operation takes the optional `Tags` field, which you can use as a filter on the response so that tagged resources can be retrieved as a group\. If you choose to use tags filtering, only resources with the tag are retrieved\.

**Request**
+ `NextToken` – UTF\-8 string\.

  A continuation token, if this is a continuation request\.
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  The maximum size of a list to return\.
+ `Tags` – A map array of key\-value pairs, not more than 50 pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 128 bytes long\.

  Each value is a UTF\-8 string, not more than 256 bytes long\.

  Specifies to return only these tagged resources\.

**Response**
+ `DevEndpointNames` – An array of UTF\-8 strings\.

  The names of all the `DevEndpoint`s in the account, or the `DevEndpoint`s with the specified tags\.
+ `NextToken` – UTF\-8 string\.

  A continuation token, if the returned list does not contain the last metric available\.

**Errors**
+ `InvalidInputException`
+ `EntityNotFoundException`
+ `InternalServiceException`
+ `OperationTimeoutException`