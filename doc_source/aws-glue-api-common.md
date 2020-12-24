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
+ `UnscaledValue` – *Required:* Blob\.

  The unscaled numeric value\.
+ `Scale` – *Required:* Number \(integer\)\.

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

## ColumnStatistics Structure<a name="aws-glue-api-common-ColumnStatistics"></a>

Represents the generated column\-level statistics for a table or partition\.

**Fields**
+ `ColumnName` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](#aws-glue-api-regex-oneLine)\.

  Name of column which statistics belong to\.
+ `ColumnType` – *Required:* Type name, not more than 20000 bytes long, matching the [Single-line string pattern](#aws-glue-api-regex-oneLine)\.

  The data type of the column\.
+ `AnalyzedTime` – *Required:* Timestamp\.

  The timestamp of when column statistics were generated\.
+ `StatisticsData` – *Required:* A [ColumnStatisticsData](#aws-glue-api-common-ColumnStatisticsData) object\.

  A `ColumnStatisticData` object that contains the statistics data values\.

## ColumnStatisticsError Structure<a name="aws-glue-api-common-ColumnStatisticsError"></a>

Encapsulates a `ColumnStatistics` object that failed and the reason for failure\.

**Fields**
+ `ColumnStatistics` – A [ColumnStatistics](#aws-glue-api-common-ColumnStatistics) object\.

  The `ColumnStatistics` of the column\.
+ `Error` – An [ErrorDetail](#aws-glue-api-common-ErrorDetail) object\.

  An error message with the reason for the failure of an operation\.

## ColumnError Structure<a name="aws-glue-api-common-ColumnError"></a>

Encapsulates a column name that failed and the reason for failure\.

**Fields**
+ `ColumnName` – UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](#aws-glue-api-regex-oneLine)\.

  The name of the column that failed\.
+ `Error` – An [ErrorDetail](#aws-glue-api-common-ErrorDetail) object\.

  An error message with the reason for the failure of an operation\.

## ColumnStatisticsData Structure<a name="aws-glue-api-common-ColumnStatisticsData"></a>

Contains the individual types of column statistics data\. Only one data object should be set and indicated by the `Type` attribute\.

**Fields**
+ `Type` – *Required:* UTF\-8 string \(valid values: `BOOLEAN` \| `DATE` \| `DECIMAL` \| `DOUBLE` \| `LONG` \| `STRING` \| `BINARY`\)\.

  The type of column statistics data\.
+ `BooleanColumnStatisticsData` – A [BooleanColumnStatisticsData](#aws-glue-api-common-BooleanColumnStatisticsData) object\.

  Boolean column statistics data\.
+ `DateColumnStatisticsData` – A [DateColumnStatisticsData](#aws-glue-api-common-DateColumnStatisticsData) object\.

  Date column statistics data\.
+ `DecimalColumnStatisticsData` – A [DecimalColumnStatisticsData](#aws-glue-api-common-DecimalColumnStatisticsData) object\.

  Decimal column statistics data\.
+ `DoubleColumnStatisticsData` – A [DoubleColumnStatisticsData](#aws-glue-api-common-DoubleColumnStatisticsData) object\.

  Double column statistics data\.
+ `LongColumnStatisticsData` – A [LongColumnStatisticsData](#aws-glue-api-common-LongColumnStatisticsData) object\.

  Long column statistics data\.
+ `StringColumnStatisticsData` – A [StringColumnStatisticsData](#aws-glue-api-common-StringColumnStatisticsData) object\.

  String column statistics data\.
+ `BinaryColumnStatisticsData` – A [BinaryColumnStatisticsData](#aws-glue-api-common-BinaryColumnStatisticsData) object\.

  Binary column statistics data\.

## BooleanColumnStatisticsData Structure<a name="aws-glue-api-common-BooleanColumnStatisticsData"></a>

Defines column statistics supported for Boolean data columns\.

**Fields**
+ `NumberOfTrues` – *Required:* Number \(long\), not more than None\.

  The number of true values in the column\.
+ `NumberOfFalses` – *Required:* Number \(long\), not more than None\.

  The number of false values in the column\.
+ `NumberOfNulls` – *Required:* Number \(long\), not more than None\.

  The number of null values in the column\.

## DateColumnStatisticsData Structure<a name="aws-glue-api-common-DateColumnStatisticsData"></a>

Defines column statistics supported for timestamp data columns\.

**Fields**
+ `MinimumValue` – Timestamp\.

  The lowest value in the column\.
+ `MaximumValue` – Timestamp\.

  The highest value in the column\.
+ `NumberOfNulls` – *Required:* Number \(long\), not more than None\.

  The number of null values in the column\.
+ `NumberOfDistinctValues` – *Required:* Number \(long\), not more than None\.

  The number of distinct values in a column\.

## DecimalColumnStatisticsData Structure<a name="aws-glue-api-common-DecimalColumnStatisticsData"></a>

Defines column statistics supported for fixed\-point number data columns\.

**Fields**
+ `MinimumValue` – A [DecimalNumber](#aws-glue-api-common-DecimalNumber) object\.

  The lowest value in the column\.
+ `MaximumValue` – A [DecimalNumber](#aws-glue-api-common-DecimalNumber) object\.

  The highest value in the column\.
+ `NumberOfNulls` – *Required:* Number \(long\), not more than None\.

  The number of null values in the column\.
+ `NumberOfDistinctValues` – *Required:* Number \(long\), not more than None\.

  The number of distinct values in a column\.

## DoubleColumnStatisticsData Structure<a name="aws-glue-api-common-DoubleColumnStatisticsData"></a>

Defines column statistics supported for floating\-point number data columns\.

**Fields**
+ `MinimumValue` – Number \(double\)\.

  The lowest value in the column\.
+ `MaximumValue` – Number \(double\)\.

  The highest value in the column\.
+ `NumberOfNulls` – *Required:* Number \(long\), not more than None\.

  The number of null values in the column\.
+ `NumberOfDistinctValues` – *Required:* Number \(long\), not more than None\.

  The number of distinct values in a column\.

## LongColumnStatisticsData Structure<a name="aws-glue-api-common-LongColumnStatisticsData"></a>

Defines column statistics supported for integer data columns\.

**Fields**
+ `MinimumValue` – Number \(long\)\.

  The lowest value in the column\.
+ `MaximumValue` – Number \(long\)\.

  The highest value in the column\.
+ `NumberOfNulls` – *Required:* Number \(long\), not more than None\.

  The number of null values in the column\.
+ `NumberOfDistinctValues` – *Required:* Number \(long\), not more than None\.

  The number of distinct values in a column\.

## StringColumnStatisticsData Structure<a name="aws-glue-api-common-StringColumnStatisticsData"></a>

Defines column statistics supported for character sequence data values\.

**Fields**
+ `MaximumLength` – *Required:* Number \(long\), not more than None\.

  The size of the longest string in the column\.
+ `AverageLength` – *Required:* Number \(double\), not more than None\.

  The average string length in the column\.
+ `NumberOfNulls` – *Required:* Number \(long\), not more than None\.

  The number of null values in the column\.
+ `NumberOfDistinctValues` – *Required:* Number \(long\), not more than None\.

  The number of distinct values in a column\.

## BinaryColumnStatisticsData Structure<a name="aws-glue-api-common-BinaryColumnStatisticsData"></a>

Defines column statistics supported for bit sequence data values\.

**Fields**
+ `MaximumLength` – *Required:* Number \(long\), not more than None\.

  The size of the longest bit sequence in the column\.
+ `AverageLength` – *Required:* Number \(double\), not more than None\.

  The average bit sequence length in the column\.
+ `NumberOfNulls` – *Required:* Number \(long\), not more than None\.

  The number of null values in the column\.

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
+ Custom string pattern \#11 – "`[a-f0-9]{8}-[a-f0-9]{4}-[a-f0-9]{4}-[a-f0-9]{4}-[a-f0-9]{12}`"
+ Custom string pattern \#12 – "`[a-zA-Z0-9-_$#]+`"
+ Custom string pattern \#13 – "`^[2-3]$`"
+ Custom string pattern \#14 – "`^\w+\.\w+\.\w+$`"
+ Custom string pattern \#15 – "`^\w+\.\w+$`"
+ Custom string pattern \#16 – "`arn:aws:kms:.*`"
+ Custom string pattern \#17 – "`.*\S.*`"
+ Custom string pattern \#18 – "`[a-zA-Z0-9+-=._./@]+`"
+ Custom string pattern \#19 – "`[1-9][0-9]*|[1-9][0-9]*-[1-9][0-9]*`"