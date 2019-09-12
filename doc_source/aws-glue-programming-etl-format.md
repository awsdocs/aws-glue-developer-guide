# Format Options for ETL Inputs and Outputs in AWS Glue<a name="aws-glue-programming-etl-format"></a>

Various AWS Glue PySpark and Scala methods and transforms specify their input and/or output format using a `format` parameter and a `format_options` parameter\. These parameters can take the following values:

## format="avro"<a name="aws-glue-programming-etl-format-avro"></a>

This value designates the [Apache Avro](https://avro.apache.org/) data format\.

There are no `format_options` values for `format="avro"`\.

## format="csv"<a name="aws-glue-programming-etl-format-csv"></a>

This value designates `comma-separated-values` as the data format \(for example, see [RFC 4180](https://tools.ietf.org/html/rfc4180) and [RFC 7111](https://tools.ietf.org/html/rfc7111)\)\.

You can use the following `format_options` values with `format="csv"`:
+ `separator`: Specifies the delimiter character\. The default is a comma: `','`, but any other character can be specified\.
+ `escaper`: Specifies a character to use for escaping\. The default value is `"none"`\. If enabled, the character which immediately follows is used as\-is, except for a small set of well\-known escapes \(`\n`, `\r`, `\t`, and `\0`\)\.
+ `quoteChar`: Specifies the character to use for quoting\. The default is a double quote: `'"'`\. Set this to `'-1'` to disable quoting entirely\.
+ `multiline`: A Boolean value that specifies whether a single record can span multiple lines\. This can occur when a field contains a quoted new\-line character\. You must set this option to "true" if any record spans multiple lines\. The default value is `"false"`, which allows for more aggressive file\-splitting during parsing\.
+ `withHeader`: A Boolean value that specifies whether to treat the first line as a header\. The default value is `"false"`\. This option can be used in the `DynamicFrameReader` class\.
+ `writeHeader`: A Boolean value that specifies whether to write the header to output\. The default value is `"true"`\. This option can be used in the `DynamicFrameWriter` class\.
+ `skipFirst`: A Boolean value that specifies whether to skip the first data line\. The default value is `"false"`\.

## format="ion"<a name="aws-glue-programming-etl-format-ion"></a>

This value designates [Amazon Ion](https://amzn.github.io/ion-docs/) as the data format\. \(For more information, see the [Amazon Ion Specification](https://amzn.github.io/ion-docs/spec.html)\.\)

**Currently, AWS Glue does not support `ion` for output\.**

There are no `format_options` values for `format="ion"`\.

## format="grokLog"<a name="aws-glue-programming-etl-format-grokLog"></a>

This value designates a log data format specified by one or more Logstash grok patterns \(for example, see [Logstash Reference \(6\.2\]: Grok filter plugin](https://www.elastic.co/guide/en/logstash/current/plugins-filters-grok.html)\)\.

**Currently, AWS Glue does not support `groklog` for output\.**

You can use the following `format_options` values with `format="grokLog"`:
+ `logFormat`: Specifies the grok pattern that matches the log's format\.
+ `customPatterns`: Specifies additional grok patterns used here\.
+ `MISSING`: Specifies the signal to use in identifying missing values\. The default is `'-'`\.
+ `LineCount`: Specifies the number of lines in each log record\. The default is `'1'`, and currently only single\-line records are supported\.
+ `StrictMode`: A Boolean value that specifies whether strict mode is enabled\. In strict mode, the reader doesn't do automatic type conversion or recovery\. The default value is `"false"`\.

## format="json"<a name="aws-glue-programming-etl-format-json"></a>

This value designates a [JSON](https://www.json.org/) \(JavaScript Object Notation\) data format\.

Currently, AWS Glue does not support `format_options` for `json` output\.

You can use the following `format_options` values with `format="json"`:
+ `jsonPath`: A [JsonPath](https://github.com/json-path/JsonPath) expression that identifies an object to be read into records\. This is particularly useful when a file contains records nested inside an outer array\. For example, the following JsonPath expression targets the `id` field of a JSON object:

  ```
  format="json", format_options={"jsonPath": "$.id"}
  ```
+ `multiline`: A Boolean value that specifies whether a single record can span multiple lines\. This can occur when a field contains a quoted new\-line character\. You must set this option to "true" if any record spans multiple lines\. The default value is `"false"`, which allows for more aggressive file\-splitting during parsing\.

## format="orc"<a name="aws-glue-programming-etl-format-orc"></a>

This value designates [Apache ORC](https://orc.apache.org/) as the data format\. \(For more information, see the [LanguageManual ORC](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC)\.\)

There are no `format_options` values for `format="orc"`\. However, any options that are accepted by the underlying SparkSQL code can be passed to it by way of the `connection_options` map parameter\.

## format="parquet"<a name="aws-glue-programming-etl-format-parquet"></a>

This value designates [Apache Parquet](https://parquet.apache.org/documentation/latest/) as the data format\.

There are no `format_options` values for `format="parquet"`\. However, any options that are accepted by the underlying SparkSQL code can be passed to it by way of the `connection_options` map parameter\.

## format="glueparquet"<a name="aws-glue-programming-etl-format-glue-parquet"></a>

This value designates a custom Parquet writer type that is optimized for Dynamic Frames as the data format\. A precomputed schema is not required before writing\. As data comes in, `glueparquet` computes and modifies the schema dynamically\. 

You can use the following `format_options` values with `format="glueparquet"`:
+ `compression`: Specifies the compression codec used when writing Parquet files\. The default value is `"snappy"`\.
+ `blockSize`: Specifies the size of a row group being buffered in memory\. The default value is `"128MB"`\.
+ `pageSize`: Specifies the size of the smallest unit that must be read fully to access a single record\. The default value is `"1MB"`\.

 Limitations:
+ `glueparquet` supports only a schema shrinkage or expansion, but not a type change\.
+ `glueparquet` is not able to store a schema\-only file\.

## format="xml"<a name="aws-glue-programming-etl-format-xml"></a>

This value designates XML as the data format, parsed through a fork of the [XML Data Source for Apache Spark](https://github.com/databricks/spark-xml) parser\.

**Currently, AWS Glue does not support "xml" for output\.**

You can use the following `format_options` values with `format="xml"`:
+ `rowTag`: Specifies the XML tag in the file to treat as a row\. Row tags cannot be self\-closing\.
+ `encoding`: Specifies the character encoding\. The default value is `"UTF-8"`\.
+ `excludeAttribute`: A Boolean value that specifies whether you want to exclude attributes in elements or not\. The default value is `"false"`\.
+ `treatEmptyValuesAsNulls`: A Boolean value that specifies whether to treat white space as a null value\. The default value is `"false"`\.
+ `attributePrefix`: A prefix for attributes to differentiate them from elements\. This prefix is used for field names\. The default value is `"_"`\.
+ `valueTag`: The tag used for a value when there are attributes in the element that have no child\. The default is `"_VALUE"`\.
+ `ignoreSurroundingSpaces`: A Boolean value that specifies whether the white space that surrounds values should be ignored\. The default value is `"false"`\.