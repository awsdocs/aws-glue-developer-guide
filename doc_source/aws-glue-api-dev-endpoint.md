# AWS Glue Development Endpoints API<a name="aws-glue-api-dev-endpoint"></a>

## Data Types<a name="aws-glue-api-dev-endpoint-objects"></a>

+ [DevEndpoint Structure](#aws-glue-api-dev-endpoint-DevEndpoint)

+ [DevEndpointCustomLibraries Structure](#aws-glue-api-dev-endpoint-DevEndpointCustomLibraries)

## DevEndpoint Structure<a name="aws-glue-api-dev-endpoint-DevEndpoint"></a>

A development endpoint where a developer can remotely debug ETL scripts\.

**Fields**

+ `EndpointName` – String\.

  The name of the DevEndpoint\.

+ `RoleArn` – String, matching the [AWS ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-arn-id)\.

  The AWS ARN of the IAM role used in this DevEndpoint\.

+ `SecurityGroupIds` – An array of UTF\-8 strings\.

  A list of security group identifiers used in this DevEndpoint\.

+ `SubnetId` – String\.

  The subnet ID for this DevEndpoint\.

+ `YarnEndpointAddress` – String\.

  The YARN endpoint address used by this DevEndpoint\.

+ `ZeppelinRemoteSparkInterpreterPort` – Number \(integer\)\.

  The Apache Zeppelin port for the remote Apache Spark interpreter\.

+ `PublicAddress` – String\.

  The public VPC address used by this DevEndpoint\.

+ `Status` – String\.

  The current status of this DevEndpoint\.

+ `NumberOfNodes` – Number \(integer\)\.

  The number of AWS Glue Data Processing Units \(DPUs\) allocated to this DevEndpoint\.

+ `AvailabilityZone` – String\.

  The AWS availability zone where this DevEndpoint is located\.

+ `VpcId` – String\.

  The ID of the virtual private cloud \(VPC\) used by this DevEndpoint\.

+ `ExtraPythonLibsS3Path` – String\.

  Path\(s\) to one or more Python libraries in an S3 bucket that should be loaded in your DevEndpoint\. Multiple values must be complete paths separated by a comma\.

  Please note that only pure Python libraries can currently be used on a DevEndpoint\. Libraries that rely on C extensions, such as the [pandas](http://pandas.pydata.org/) Python data analysis library, are not yet supported\.

+ `ExtraJarsS3Path` – String\.

  Path to one or more Java Jars in an S3 bucket that should be loaded in your DevEndpoint\.

  Please note that only pure Java/Scala libraries can currently be used on a DevEndpoint\.

+ `FailureReason` – String\.

  The reason for a current failure in this DevEndpoint\.

+ `LastUpdateStatus` – String\.

  The status of the last update\.

+ `CreatedTimestamp` – Timestamp\.

  The point in time at which this DevEndpoint was created\.

+ `LastModifiedTimestamp` – Timestamp\.

  The point in time at which this DevEndpoint was last modified\.

+ `PublicKey` – String\.

  The public key to be used by this DevEndpoint for authentication\.

## DevEndpointCustomLibraries Structure<a name="aws-glue-api-dev-endpoint-DevEndpointCustomLibraries"></a>

Custom libraries to be loaded into a DevEndpoint\.

**Fields**

+ `ExtraPythonLibsS3Path` – String\.

  Path\(s\) to one or more Python libraries in an S3 bucket that should be loaded in your DevEndpoint\. Multiple values must be complete paths separated by a comma\.

  Please note that only pure Python libraries can currently be used on a DevEndpoint\. Libraries that rely on C extensions, such as the [pandas](http://pandas.pydata.org/) Python data analysis library, are not yet supported\.

+ `ExtraJarsS3Path` – String\.

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

+ `EndpointName` – String\. Required\.

  The name to be assigned to the new DevEndpoint\.

+ `RoleArn` – String, matching the [AWS ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-arn-id)\. Required\.

  The IAM role for the DevEndpoint\.

+ `SecurityGroupIds` – An array of UTF\-8 strings\.

  Security group IDs for the security groups to be used by the new DevEndpoint\.

+ `SubnetId` – String\.

  The subnet ID for the new DevEndpoint to use\.

+ `PublicKey` – String\.

  The public key to use for authentication\.

+ `NumberOfNodes` – Number \(integer\)\.

  The number of AWS Glue Data Processing Units \(DPUs\) to allocate to this DevEndpoint\.

+ `ExtraPythonLibsS3Path` – String\.

  Path\(s\) to one or more Python libraries in an S3 bucket that should be loaded in your DevEndpoint\. Multiple values must be complete paths separated by a comma\.

  Please note that only pure Python libraries can currently be used on a DevEndpoint\. Libraries that rely on C extensions, such as the [pandas](http://pandas.pydata.org/) Python data analysis library, are not yet supported\.

+ `ExtraJarsS3Path` – String\.

  Path to one or more Java Jars in an S3 bucket that should be loaded in your DevEndpoint\.

**Response**

+ `EndpointName` – String\.

  The name assigned to the new DevEndpoint\.

+ `Status` – String\.

  The current status of the new DevEndpoint\.

+ `SecurityGroupIds` – An array of UTF\-8 strings\.

  The security groups assigned to the new DevEndpoint\.

+ `SubnetId` – String\.

  The subnet ID assigned to the new DevEndpoint\.

+ `RoleArn` – String, matching the [AWS ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-arn-id)\.

  The AWS ARN of the role assigned to the new DevEndpoint\.

+ `YarnEndpointAddress` – String\.

  The address of the YARN endpoint used by this DevEndpoint\.

+ `ZeppelinRemoteSparkInterpreterPort` – Number \(integer\)\.

  The Apache Zeppelin port for the remote Apache Spark interpreter\.

+ `NumberOfNodes` – Number \(integer\)\.

  The number of AWS Glue Data Processing Units \(DPUs\) allocated to this DevEndpoint\.

+ `AvailabilityZone` – String\.

  The AWS availability zone where this DevEndpoint is located\.

+ `VpcId` – String\.

  The ID of the VPC used by this DevEndpoint\.

+ `ExtraPythonLibsS3Path` – String\.

  Path\(s\) to one or more Python libraries in an S3 bucket that will be loaded in your DevEndpoint\.

+ `ExtraJarsS3Path` – String\.

  Path to one or more Java Jars in an S3 bucket that will be loaded in your DevEndpoint\.

+ `FailureReason` – String\.

  The reason for a current failure in this DevEndpoint\.

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

+ `EndpointName` – String\. Required\.

  The name of the DevEndpoint to be updated\.

+ `PublicKey` – String\.

  The public key for the DevEndpoint to use\.

+ `CustomLibraries` – A DevEndpointCustomLibraries object\.

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

+ `EndpointName` – String\. Required\.

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

**Request**

+ `EndpointName` – String\. Required\.

  Name of the DevEndpoint for which to retrieve information\.

**Response**

+ `DevEndpoint` – A DevEndpoint object\.

  A DevEndpoint definition\.

**Errors**

+ `EntityNotFoundException`

+ `InternalServiceException`

+ `OperationTimeoutException`

+ `InvalidInputException`

## GetDevEndpoints Action \(Python: get\_dev\_endpoints\)<a name="aws-glue-api-dev-endpoint-GetDevEndpoints"></a>

Retrieves all the DevEndpoints in this AWS account\.

**Request**

+ `MaxResults` – Number \(integer\)\.

  The maximum size of information to return\.

+ `NextToken` – String\.

  A continuation token, if this is a continuation call\.

**Response**

+ `DevEndpoints` – An array of [DevEndpoint](#aws-glue-api-dev-endpoint-DevEndpoint)s\.

  A list of DevEndpoint definitions\.

+ `NextToken` – String\.

  A continuation token, if not all DevEndpoint definitions have yet been returned\.

**Errors**

+ `EntityNotFoundException`

+ `InternalServiceException`

+ `OperationTimeoutException`

+ `InvalidInputException`