# Common Data Types<a name="aws-glue-api-common"></a>

## Tag Structure<a name="aws-glue-api-common-Tag"></a>

An AWS Tag\.

**Fields**
+ `key` – String\.

  The tag key\.
+ `value` – String\.

  The tag value\.

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
+ `ErrorCode` – String, matching the [Single-line string pattern](#aws-glue-api-regex-oneLine)\.

  The code associated with this error\.
+ `ErrorMessage` – Description string, matching the [URI address multi-line string pattern](#aws-glue-api-regex-uri)\.

  A message describing the error\.

## PropertyPredicate Structure<a name="aws-glue-api-common-PropertyPredicate"></a>

Defines a property predicate\.

**Fields**
+ `Key` – Value string\.

  The key of the property\.
+ `Value` – Value string\.

  The value of the property\.
+ `Comparator` – String \(valid values: `EQUALS` \| `GREATER_THAN` \| `LESS_THAN` \| `GREATER_THAN_EQUALS` \| `LESS_THAN_EQUALS`\)\.

  The comparator used to compare this property to others\.

## ResourceUri Structure<a name="aws-glue-api-common-ResourceUri"></a>

URIs for function resources\.

**Fields**
+ `ResourceType` – String \(valid values: `JAR` \| `FILE` \| `ARCHIVE`\)\.

  The type of the resource\.
+ `Uri` – Uniform resource identifier \(uri\), matching the [URI address multi-line string pattern](#aws-glue-api-regex-uri)\.

  The URI for accessing the resource\.

## String Patterns<a name="aws-glue-api-common-_string-patterns"></a>

The API uses the following regular expressions to define what is valid content for various string parameters and members:
+ Single\-line string pattern – "`[\u0020-\uD7FF\uE000-\uFFFD\uD800\uDC00-\uDBFF\uDFFF\t]*`"
+ URI address multi\-line string pattern – "`[\u0020-\uD7FF\uE000-\uFFFD\uD800\uDC00-\uDBFF\uDFFF\r\n\t]*`"
+ A Logstash Grok string pattern – "`[\u0020-\uD7FF\uE000-\uFFFD\uD800\uDC00-\uDBFF\uDFFF\r\t]*`"
+ Identifier string pattern – "`[A-Za-z_][A-Za-z0-9_]*`"
+ AWS ARN string pattern – "`arn:aws:iam::\d{12}:role/.*`"
+ Version string pattern – "`^[a-zA-Z0-9-_]+$`"
+ Log group string pattern – "`[\.\-_/#A-Za-z0-9]+`"
+ Log\-stream string pattern – "`[^:*]*`"
+ Custom string pattern \#8 – "`^$|arn:aws:kms:.*`"