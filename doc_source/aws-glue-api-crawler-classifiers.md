# Classifier API<a name="aws-glue-api-crawler-classifiers"></a>

## Data Types<a name="aws-glue-api-crawler-classifiers-objects"></a>
+ [Classifier Structure](#aws-glue-api-crawler-classifiers-Classifier)
+ [GrokClassifier Structure](#aws-glue-api-crawler-classifiers-GrokClassifier)
+ [XMLClassifier Structure](#aws-glue-api-crawler-classifiers-XMLClassifier)
+ [JsonClassifier Structure](#aws-glue-api-crawler-classifiers-JsonClassifier)
+ [CreateGrokClassifierRequest Structure](#aws-glue-api-crawler-classifiers-CreateGrokClassifierRequest)
+ [UpdateGrokClassifierRequest Structure](#aws-glue-api-crawler-classifiers-UpdateGrokClassifierRequest)
+ [CreateXMLClassifierRequest Structure](#aws-glue-api-crawler-classifiers-CreateXMLClassifierRequest)
+ [UpdateXMLClassifierRequest Structure](#aws-glue-api-crawler-classifiers-UpdateXMLClassifierRequest)
+ [CreateJsonClassifierRequest Structure](#aws-glue-api-crawler-classifiers-CreateJsonClassifierRequest)
+ [UpdateJsonClassifierRequest Structure](#aws-glue-api-crawler-classifiers-UpdateJsonClassifierRequest)

## Classifier Structure<a name="aws-glue-api-crawler-classifiers-Classifier"></a>

Classifiers are triggered during a crawl task\. A classifier checks whether a given file is in a format it can handle, and if it is, the classifier creates a schema in the form of a `StructType` object that matches that data format\.

You can use the standard classifiers that AWS Glue supplies, or you can write your own classifiers to best categorize your data sources and specify the appropriate schemas to use for them\. A classifier can be a `grok` classifier, an `XML` classifier, or a `JSON` classifier, as specified in one of the fields in the `Classifier` object\.

**Fields**
+ `GrokClassifier` – A [GrokClassifier](#aws-glue-api-crawler-classifiers-GrokClassifier) object\.

  A `GrokClassifier` object\.
+ `XMLClassifier` – A [XMLClassifier](#aws-glue-api-crawler-classifiers-XMLClassifier) object\.

  An `XMLClassifier` object\.
+ `JsonClassifier` – A [JsonClassifier](#aws-glue-api-crawler-classifiers-JsonClassifier) object\.

  A `JsonClassifier` object\.

## GrokClassifier Structure<a name="aws-glue-api-crawler-classifiers-GrokClassifier"></a>

A classifier that uses `grok` patterns\.

**Fields**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the classifier\.
+ `Classification` – *Required:* UTF\-8 string\.

  An identifier of the data format that the classifier matches, such as Twitter, JSON, Omniture logs, and so on\.
+ `CreationTime` – Timestamp\.

  The time this classifier was registered\.
+ `LastUpdated` – Timestamp\.

  The time this classifier was last updated\.
+ `Version` – Number \(long\)\.

  The version of this classifier\.
+ `GrokPattern` – *Required:* UTF\-8 string, not less than 1 or more than 2048 bytes long, matching the [A Logstash Grok string pattern](aws-glue-api-common.md#aws-glue-api-grok-pattern)\.

  The grok pattern applied to a data store by this classifier\. For more information, see built\-in patterns in [Writing Custom Classifers](http://docs.aws.amazon.com/glue/latest/dg/custom-classifier.html)\.
+ `CustomPatterns` – UTF\-8 string, not more than 16000 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Optional custom grok patterns defined by this classifier\. For more information, see custom patterns in [Writing Custom Classifers](http://docs.aws.amazon.com/glue/latest/dg/custom-classifier.html)\.

## XMLClassifier Structure<a name="aws-glue-api-crawler-classifiers-XMLClassifier"></a>

A classifier for `XML` content\.

**Fields**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the classifier\.
+ `Classification` – *Required:* UTF\-8 string\.

  An identifier of the data format that the classifier matches\.
+ `CreationTime` – Timestamp\.

  The time this classifier was registered\.
+ `LastUpdated` – Timestamp\.

  The time this classifier was last updated\.
+ `Version` – Number \(long\)\.

  The version of this classifier\.
+ `RowTag` – UTF\-8 string\.

  The XML tag designating the element that contains each record in an XML document being parsed\. Note that this cannot identify a self\-closing element \(closed by `/>`\)\. An empty row element that contains only attributes can be parsed as long as it ends with a closing tag \(for example, `<row item_a="A" item_b="B"></row>` is okay, but `<row item_a="A" item_b="B" />` is not\)\.

## JsonClassifier Structure<a name="aws-glue-api-crawler-classifiers-JsonClassifier"></a>

A classifier for `JSON` content\.

**Fields**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the classifier\.
+ `CreationTime` – Timestamp\.

  The time this classifier was registered\.
+ `LastUpdated` – Timestamp\.

  The time this classifier was last updated\.
+ `Version` – Number \(long\)\.

  The version of this classifier\.
+ `JsonPath` – *Required:* UTF\-8 string\.

  A `JsonPath` string defining the JSON data for the classifier to classify\. AWS Glue supports a subset of JsonPath, as described in [Writing JsonPath Custom Classifiers](https://docs.aws.amazon.com/glue/latest/dg/custom-classifier.html#custom-classifier-json)\.

## CreateGrokClassifierRequest Structure<a name="aws-glue-api-crawler-classifiers-CreateGrokClassifierRequest"></a>

Specifies a `grok` classifier for `CreateClassifier` to create\.

**Fields**
+ `Classification` – *Required:* UTF\-8 string\.

  An identifier of the data format that the classifier matches, such as Twitter, JSON, Omniture logs, Amazon CloudWatch Logs, and so on\.
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the new classifier\.
+ `GrokPattern` – *Required:* UTF\-8 string, not less than 1 or more than 2048 bytes long, matching the [A Logstash Grok string pattern](aws-glue-api-common.md#aws-glue-api-grok-pattern)\.

  The grok pattern used by this classifier\.
+ `CustomPatterns` – UTF\-8 string, not more than 16000 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Optional custom grok patterns used by this classifier\.

## UpdateGrokClassifierRequest Structure<a name="aws-glue-api-crawler-classifiers-UpdateGrokClassifierRequest"></a>

Specifies a grok classifier to update when passed to `UpdateClassifier`\.

**Fields**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the `GrokClassifier`\.
+ `Classification` – UTF\-8 string\.

  An identifier of the data format that the classifier matches, such as Twitter, JSON, Omniture logs, Amazon CloudWatch Logs, and so on\.
+ `GrokPattern` – UTF\-8 string, not less than 1 or more than 2048 bytes long, matching the [A Logstash Grok string pattern](aws-glue-api-common.md#aws-glue-api-grok-pattern)\.

  The grok pattern used by this classifier\.
+ `CustomPatterns` – UTF\-8 string, not more than 16000 bytes long, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Optional custom grok patterns used by this classifier\.

## CreateXMLClassifierRequest Structure<a name="aws-glue-api-crawler-classifiers-CreateXMLClassifierRequest"></a>

Specifies an XML classifier for `CreateClassifier` to create\.

**Fields**
+ `Classification` – *Required:* UTF\-8 string\.

  An identifier of the data format that the classifier matches\.
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the classifier\.
+ `RowTag` – UTF\-8 string\.

  The XML tag designating the element that contains each record in an XML document being parsed\. Note that this cannot identify a self\-closing element \(closed by `/>`\)\. An empty row element that contains only attributes can be parsed as long as it ends with a closing tag \(for example, `<row item_a="A" item_b="B"></row>` is okay, but `<row item_a="A" item_b="B" />` is not\)\.

## UpdateXMLClassifierRequest Structure<a name="aws-glue-api-crawler-classifiers-UpdateXMLClassifierRequest"></a>

Specifies an XML classifier to be updated\.

**Fields**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the classifier\.
+ `Classification` – UTF\-8 string\.

  An identifier of the data format that the classifier matches\.
+ `RowTag` – UTF\-8 string\.

  The XML tag designating the element that contains each record in an XML document being parsed\. Note that this cannot identify a self\-closing element \(closed by `/>`\)\. An empty row element that contains only attributes can be parsed as long as it ends with a closing tag \(for example, `<row item_a="A" item_b="B"></row>` is okay, but `<row item_a="A" item_b="B" />` is not\)\.

## CreateJsonClassifierRequest Structure<a name="aws-glue-api-crawler-classifiers-CreateJsonClassifierRequest"></a>

Specifies a JSON classifier for `CreateClassifier` to create\.

**Fields**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the classifier\.
+ `JsonPath` – *Required:* UTF\-8 string\.

  A `JsonPath` string defining the JSON data for the classifier to classify\. AWS Glue supports a subset of JsonPath, as described in [Writing JsonPath Custom Classifiers](https://docs.aws.amazon.com/glue/latest/dg/custom-classifier.html#custom-classifier-json)\.

## UpdateJsonClassifierRequest Structure<a name="aws-glue-api-crawler-classifiers-UpdateJsonClassifierRequest"></a>

Specifies a JSON classifier to be updated\.

**Fields**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  The name of the classifier\.
+ `JsonPath` – UTF\-8 string\.

  A `JsonPath` string defining the JSON data for the classifier to classify\. AWS Glue supports a subset of JsonPath, as described in [Writing JsonPath Custom Classifiers](https://docs.aws.amazon.com/glue/latest/dg/custom-classifier.html#custom-classifier-json)\.

## Operations<a name="aws-glue-api-crawler-classifiers-actions"></a>
+ [CreateClassifier Action \(Python: create\_classifier\)](#aws-glue-api-crawler-classifiers-CreateClassifier)
+ [DeleteClassifier Action \(Python: delete\_classifier\)](#aws-glue-api-crawler-classifiers-DeleteClassifier)
+ [GetClassifier Action \(Python: get\_classifier\)](#aws-glue-api-crawler-classifiers-GetClassifier)
+ [GetClassifiers Action \(Python: get\_classifiers\)](#aws-glue-api-crawler-classifiers-GetClassifiers)
+ [UpdateClassifier Action \(Python: update\_classifier\)](#aws-glue-api-crawler-classifiers-UpdateClassifier)

## CreateClassifier Action \(Python: create\_classifier\)<a name="aws-glue-api-crawler-classifiers-CreateClassifier"></a>

Creates a classifier in the user's account\. This may be a `GrokClassifier`, an `XMLClassifier`, or abbrev `JsonClassifier`, depending on which field of the request is present\.

**Request**
+ `GrokClassifier` – A [CreateGrokClassifierRequest](#aws-glue-api-crawler-classifiers-CreateGrokClassifierRequest) object\.

  A `GrokClassifier` object specifying the classifier to create\.
+ `XMLClassifier` – A [CreateXMLClassifierRequest](#aws-glue-api-crawler-classifiers-CreateXMLClassifierRequest) object\.

  An `XMLClassifier` object specifying the classifier to create\.
+ `JsonClassifier` – A [CreateJsonClassifierRequest](#aws-glue-api-crawler-classifiers-CreateJsonClassifierRequest) object\.

  A `JsonClassifier` object specifying the classifier to create\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `AlreadyExistsException`
+ `InvalidInputException`
+ `OperationTimeoutException`

## DeleteClassifier Action \(Python: delete\_classifier\)<a name="aws-glue-api-crawler-classifiers-DeleteClassifier"></a>

Removes a classifier from the Data Catalog\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the classifier to remove\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `EntityNotFoundException`
+ `OperationTimeoutException`

## GetClassifier Action \(Python: get\_classifier\)<a name="aws-glue-api-crawler-classifiers-GetClassifier"></a>

Retrieve a classifier by name\.

**Request**
+ `Name` – *Required:* UTF\-8 string, not less than 1 or more than 255 bytes long, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\.

  Name of the classifier to retrieve\.

**Response**
+ `Classifier` – A [Classifier](#aws-glue-api-crawler-classifiers-Classifier) object\.

  The requested classifier\.

**Errors**
+ `EntityNotFoundException`
+ `OperationTimeoutException`

## GetClassifiers Action \(Python: get\_classifiers\)<a name="aws-glue-api-crawler-classifiers-GetClassifiers"></a>

Lists all classifier objects in the Data Catalog\.

**Request**
+ `MaxResults` – Number \(integer\), not less than 1 or more than 1000\.

  Size of the list to return \(optional\)\.
+ `NextToken` – UTF\-8 string\.

  An optional continuation token\.

**Response**
+ `Classifiers` – An array of [Classifier](#aws-glue-api-crawler-classifiers-Classifier) objects\.

  The requested list of classifier objects\.
+ `NextToken` – UTF\-8 string\.

  A continuation token\.

**Errors**
+ `OperationTimeoutException`

## UpdateClassifier Action \(Python: update\_classifier\)<a name="aws-glue-api-crawler-classifiers-UpdateClassifier"></a>

Modifies an existing classifier \(a `GrokClassifier`, `XMLClassifier`, or `JsonClassifier`, depending on which field is present\)\.

**Request**
+ `GrokClassifier` – An [UpdateGrokClassifierRequest](#aws-glue-api-crawler-classifiers-UpdateGrokClassifierRequest) object\.

  A `GrokClassifier` object with updated fields\.
+ `XMLClassifier` – An [UpdateXMLClassifierRequest](#aws-glue-api-crawler-classifiers-UpdateXMLClassifierRequest) object\.

  An `XMLClassifier` object with updated fields\.
+ `JsonClassifier` – An [UpdateJsonClassifierRequest](#aws-glue-api-crawler-classifiers-UpdateJsonClassifierRequest) object\.

  A `JsonClassifier` object with updated fields\.

**Response**
+ *No Response parameters\.*

**Errors**
+ `InvalidInputException`
+ `VersionMismatchException`
+ `EntityNotFoundException`
+ `OperationTimeoutException`