# Common Data Types<a name="aws-glue-api-common"></a>

The Common Data Types describes miscellaneous common data types in AWS Glue\.

## Tag Structure<a name="aws-glue-api-common-Tag"></a>

The `Tag` object represents a label that you can assign to an AWS resource\. Each tag consists of a key and an optional value, both of which you define\.

For more information about tags, and controlling access to resources in AWS Glue, see [AWS Tags in AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/monitor-tags.html) and [Specifying AWS Glue Resource ARNs](https://docs.aws.amazon.com/glue/latest/dg/glue-specifying-resource-arns.html) in the developer guide\.

**Fields**
+ `key` – UTF\-8 string, not less than 1 or more than 128 bytes long\.

  The tag key\. The key is required when you create a tag on an object\. The key is case\-sensitive, and must not contain the prefix aws\.
+ `value` – UTF\-8 string, not more than 256 bytes long\.

  The tag value\. The value is optional when you create a tag on an object\. The value is case\-sensitive, and must not contain the prefix aws\.

## DecimalNumber Structure<a name="aws-glue-api-common-DecimalNumber"></a>

Contains a numeric value in decimal format\.

**Fields**
+ `UnscaledValue` – Blob\.

  The unscaled numeric value\.
+ `Scale` – Number \(integer\)\.

  The scale that determines where the decimal point falls in the unscaled value\.

## ErrorDetail Structure<a name="aws-glue-api-common-ErrorDetail"></a>

Contains details about an error\.

**Fields**
+ `ErrorCode` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](#aws-glue-api-regex-oneLine)\.

  The code associated with this error\.
+ `ErrorMessage` – Description string, not more than 2048 bytes long, matching the [URI address multi-line string pattern](#aws-glue-api-regex-uri)\.

  A message describing the error\.

## PropertyPredicate Structure<a name="aws-glue-api-common-PropertyPredicate"></a>

Defines a property predicate\.

**Fields**
+ `Key` – Value string, not more than 1024 bytes long\.

  The key of the property\.
+ `Value` – Value string, not more than 1024 bytes long\.

  The value of the property\.
+ `Comparator` – UTF\-8 string \(valid values: `EQUALS` \| `GREATER_THAN` \| `LESS_THAN` \| `GREATER_THAN_EQUALS` \| `LESS_THAN_EQUALS`\)\.

  The comparator used to compare this property to others\.

## ResourceUri Structure<a name="aws-glue-api-common-ResourceUri"></a>

The URIs for function resources\.

**Fields**
+ `ResourceType` – UTF\-8 string \(valid values: `JAR` \| `FILE` \| `ARCHIVE`\)\.

  The type of the resource\.
+ `Uri` – Uniform resource identifier \(uri\), not less than 1 or more than 1024 bytes long, matching the [URI address multi-line string pattern](#aws-glue-api-regex-uri)\.

  The URI for accessing the resource\.

## String Patterns<a name="aws-glue-api-common-_string-patterns"></a>

The API uses the following regular expressions to define what is valid content for various string parameters and members:
+ Single\-line string pattern – "`[\u0020-\uD7FF\uE000-\uFFFD\uD800\uDC00-\uDBFF\uDFFF\t]*`"
+ URI address multi\-line string pattern – "`[\u0020-\uD7FF\uE000-\uFFFD\uD800\uDC00-\uDBFF\uDFFF\r\n\t]*`"
+ A Logstash Grok string pattern – "`[\u0020-\uD7FF\uE000-\uFFFD\uD800\uDC00-\uDBFF\uDFFF\r\t]*`"
+ Identifier string pattern – "`[A-Za-z_][A-Za-z0-9_]*`"
+ AWS Glue ARN string pattern – "`arn:aws:glue:.*`"
+ AWS IAM ARN string pattern – "`arn:aws:iam::\d{12}:role/.*`"
+ Version string pattern – "`^[a-zA-Z0-9-_]+$`"
+ Log group string pattern – "`[\.\-_/#A-Za-z0-9]+`"
+ Log\-stream string pattern – "`[^:*]*`"
+ Custom string pattern \#10 – "`[^\r\n]`"
+ Custom string pattern \#11 – "`^[2-3]$`"
+ Custom string pattern \#12 – "`^\w+\.\w+\.\w+$`"
+ Custom string pattern \#13 – "`^\w+\.\w+$`"
+ Custom string pattern \#14 – "`arn:aws:kms:.*`"