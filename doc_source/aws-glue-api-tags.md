# Tagging APIs in AWS Glue<a name="aws-glue-api-tags"></a>

## Data Types<a name="aws-glue-api-tags-objects"></a>
+ [Tag Structure](#aws-glue-api-tags-Tag)

## Tag Structure<a name="aws-glue-api-tags-Tag"></a>

The `Tag` object represents a label that you can assign to an AWS resource\. Each tag consists of a key and an optional value, both of which you define\.

For more information about tags, and controlling access to resources in AWS Glue, see [AWS Tags in AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/monitor-tags.html) and [Specifying AWS Glue Resource ARNs](https://docs.aws.amazon.com/glue/latest/dg/glue-specifying-resource-arns.html) in the developer guide\.

**Fields**
+ `key` – UTF\-8 string, not less than 1 or more than 128 bytes long\.

  The tag key\. The key is required when you create a tag on an object\. The key is case\-sensitive, and must not contain the prefix aws\.
+ `value` – UTF\-8 string, not more than 256 bytes long\.

  The tag value\. The value is optional when you create a tag on an object\. The value is case\-sensitive, and must not contain the prefix aws\.

## Operations<a name="aws-glue-api-tags-actions"></a>
+ [TagResource Action \(Python: tag\_resource\)](#aws-glue-api-tags-TagResource)
+ [UntagResource Action \(Python: untag\_resource\)](#aws-glue-api-tags-UntagResource)
+ [GetTags Action \(Python: get\_tags\)](#aws-glue-api-tags-GetTags)

## TagResource Action \(Python: tag\_resource\)<a name="aws-glue-api-tags-TagResource"></a>

Adds tags to a resource\. A tag is a label you can assign to an AWS resource\. In AWS Glue, you can tag only certain resources\. For information about what resources you can tag, see [AWS Tags in AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/monitor-tags.html)\.

**Request**
+ `ResourceArn` – *Required:* UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The ARN of the AWS Glue resource to which to add the tags\. For more information about AWS Glue resource ARNs, see the [AWS Glue ARN string pattern](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-api-common.html#aws-glue-api-regex-aws-glue-arn-id)\.
+ `TagsToAdd` – *Required:* A map array of key\-value pairs, not more than 50 pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 128 bytes long\.

  Each value is a UTF\-8 string, not more than 256 bytes long\.

  Tags to add to this resource\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `EntityNotFoundException`

## UntagResource Action \(Python: untag\_resource\)<a name="aws-glue-api-tags-UntagResource"></a>

Removes tags from a resource\.

**Request**
+ `ResourceArn` – *Required:* UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) of the resource from which to remove the tags\.
+ `TagsToRemove` – *Required:* An array of UTF\-8 strings, not more than 50 strings\.

  Tags to remove from this resource\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `EntityNotFoundException`

## GetTags Action \(Python: get\_tags\)<a name="aws-glue-api-tags-GetTags"></a>

Retrieves a list of tags associated with a resource\.

**Request**
+ `ResourceArn` – *Required:* UTF\-8 string, not less than 1 or more than 10240 bytes long, matching the [AWS Glue ARN string pattern](aws-glue-api-common.md#aws-glue-api-regex-aws-glue-arn-id)\.

  The Amazon Resource Name \(ARN\) of the resource for which to retrieve tags\.

**Response**
+ `Tags` – A map array of key\-value pairs, not more than 50 pairs\.

  Each key is a UTF\-8 string, not less than 1 or more than 128 bytes long\.

  Each value is a UTF\-8 string, not more than 256 bytes long\.

  The requested tags\.

**Errors**
+ `InvalidInputException`
+ `InternalServiceException`
+ `OperationTimeoutException`
+ `EntityNotFoundException`