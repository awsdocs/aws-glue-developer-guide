# Classifier API<a name="aws-glue-api-crawler-classifiers"></a>

## Data Types<a name="aws-glue-api-crawler-classifiers-objects"></a>

+ [Classifier Structure](#aws-glue-api-crawler-classifiers-Classifier)

+ [GrokClassifier Structure](#aws-glue-api-crawler-classifiers-GrokClassifier)

+ [XMLClassifier Structure](#aws-glue-api-crawler-classifiers-XMLClassifier)

+ [CreateGrokClassifierRequest Structure](#aws-glue-api-crawler-classifiers-CreateGrokClassifierRequest)

+ [UpdateGrokClassifierRequest Structure](#aws-glue-api-crawler-classifiers-UpdateGrokClassifierRequest)

+ [CreateXMLClassifierRequest Structure](#aws-glue-api-crawler-classifiers-CreateXMLClassifierRequest)

+ [UpdateXMLClassifierRequest Structure](#aws-glue-api-crawler-classifiers-UpdateXMLClassifierRequest)

## Classifier Structure<a name="aws-glue-api-crawler-classifiers-Classifier"></a>

Classifiers are written in Python and triggered during a crawl task\. You can write your own classifiers to best categorize your data sources and specify the appropriate schemas to use for them\. A classifier checks whether a given file is in a format it can handle, and if it is, the classifier creates a schema in the form of a `StructType` object that matches that data format\.

A classifier can be either a `grok` classifier or an XML classifier, specified in one or the other field of the `Classifier` object\.

**Fields**

+ `GrokClassifier` – A GrokClassifier object\.

  A `GrokClassifier` object\.

+ `XMLClassifier` – A XMLClassifier object\.

  An `XMLClassifier` object\.

## GrokClassifier Structure<a name="aws-glue-api-crawler-classifiers-GrokClassifier"></a>

A classifier that uses `grok` patterns\.

**Fields**

+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the classifier\.

+ `Classification` – String\. Required\.

  An identifier of the data format that the classifier matches, such as Twitter, JSON, Omniture logs, and so on\.

+ `CreationTime` – Timestamp\.

  The time this classifier was registered\.

+ `LastUpdated` – Timestamp\.

  The time this classifier was last updated\.

+ `Version` – Number \(long\)\.

  The version of this classifier\.

+ `GrokPattern` – String, matching the [A Logstash Grok string pattern](aws-glue-api-common.md#aws-glue-api-grok-pattern)\. Required\.

  The grok pattern applied to a data store by this classifier\. For more information, see built\-in patterns in [Writing Custom Classifers](http://docs.aws.amazon.com/glue/latest/dg/custom-classifier.html)\.

+ `CustomPatterns` – String, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Optional custom grok patterns defined by this classifier\. For more information, see custom patterns in [Writing Custom Classifers](http://docs.aws.amazon.com/glue/latest/dg/custom-classifier.html)\.

## XMLClassifier Structure<a name="aws-glue-api-crawler-classifiers-XMLClassifier"></a>

A classifier for `XML` content\.

**Fields**

+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the classifier\.

+ `Classification` – String\. Required\.

  An identifier of the data format that the classifier matches\.

+ `CreationTime` – Timestamp\.

  The time this classifier was registered\.

+ `LastUpdated` – Timestamp\.

  The time this classifier was last updated\.

+ `Version` – Number \(long\)\.

  The version of this classifier\.

+ `RowTag` – String\.

  The XML tag designating the element that contains each record in an XML document being parsed\. Note that this cannot identify a self\-closing element \(closed by `/>`\)\. An empty row element that contains only attributes can be parsed as long as it ends with a closing tag \(for example, `<row item_a="A" item_b="B"></row>` is okay, but `<row item_a="A" item_b="B" />` is not\)\.

## CreateGrokClassifierRequest Structure<a name="aws-glue-api-crawler-classifiers-CreateGrokClassifierRequest"></a>

Specifies a `grok` classifier for `CreateClassifier` to create\.

**Fields**

+ `Classification` – String\. Required\.

  An identifier of the data format that the classifier matches, such as Twitter, JSON, Omniture logs, Amazon CloudWatch Logs, and so on\.

+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the new classifier\.

+ `GrokPattern` – String, matching the [A Logstash Grok string pattern](aws-glue-api-common.md#aws-glue-api-grok-pattern)\. Required\.

  The grok pattern used by this classifier\.

+ `CustomPatterns` – String, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Optional custom grok patterns used by this classifier\.

## UpdateGrokClassifierRequest Structure<a name="aws-glue-api-crawler-classifiers-UpdateGrokClassifierRequest"></a>

Specifies a grok classifier to update when passed to `UpdateClassifier`\.

**Fields**

+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the `GrokClassifier`\.

+ `Classification` – String\.

  An identifier of the data format that the classifier matches, such as Twitter, JSON, Omniture logs, Amazon CloudWatch Logs, and so on\.

+ `GrokPattern` – String, matching the [A Logstash Grok string pattern](aws-glue-api-common.md#aws-glue-api-grok-pattern)\.

  The grok pattern used by this classifier\.

+ `CustomPatterns` – String, matching the [URI address multi-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-uri)\.

  Optional custom grok patterns used by this classifier\.

## CreateXMLClassifierRequest Structure<a name="aws-glue-api-crawler-classifiers-CreateXMLClassifierRequest"></a>

Specifies an XML classifier for `CreateClassifier` to create\.

**Fields**

+ `Classification` – String\. Required\.

  An identifier of the data format that the classifier matches\.

+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the classifier\.

+ `RowTag` – String\.

  The XML tag designating the element that contains each record in an XML document being parsed\. Note that this cannot identify a self\-closing element \(closed by `/>`\)\. An empty row element that contains only attributes can be parsed as long as it ends with a closing tag \(for example, `<row item_a="A" item_b="B"></row>` is okay, but `<row item_a="A" item_b="B" />` is not\)\.

## UpdateXMLClassifierRequest Structure<a name="aws-glue-api-crawler-classifiers-UpdateXMLClassifierRequest"></a>

Specifies an XML classifier to be updated\.

**Fields**

+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  The name of the classifier\.

+ `Classification` – String\.

  An identifier of the data format that the classifier matches\.

+ `RowTag` – String\.

  The XML tag designating the element that contains each record in an XML document being parsed\. Note that this cannot identify a self\-closing element \(closed by `/>`\)\. An empty row element that contains only attributes can be parsed as long as it ends with a closing tag \(for example, `<row item_a="A" item_b="B"></row>` is okay, but `<row item_a="A" item_b="B" />` is not\)\.

## Operations<a name="aws-glue-api-crawler-classifiers-actions"></a>

+ [CreateClassifier Action \(Python: create\_classifier\)](#aws-glue-api-crawler-classifiers-CreateClassifier)

+ [DeleteClassifier Action \(Python: delete\_classifier\)](#aws-glue-api-crawler-classifiers-DeleteClassifier)

+ [GetClassifier Action \(Python: get\_classifier\)](#aws-glue-api-crawler-classifiers-GetClassifier)

+ [GetClassifiers Action \(Python: get\_classifiers\)](#aws-glue-api-crawler-classifiers-GetClassifiers)

+ [UpdateClassifier Action \(Python: update\_classifier\)](#aws-glue-api-crawler-classifiers-UpdateClassifier)

## CreateClassifier Action \(Python: create\_classifier\)<a name="aws-glue-api-crawler-classifiers-CreateClassifier"></a>

Creates a classifier in the user's account\. This may be either a `GrokClassifier` or an `XMLClassifier`\.

**Request**

+ `GrokClassifier` – A CreateGrokClassifierRequest object\.

  A `GrokClassifier` object specifying the classifier to create\.

+ `XMLClassifier` – A CreateXMLClassifierRequest object\.

  An `XMLClassifier` object specifying the classifier to create\.

**Response**

+ *No Response parameters\.*

**Errors**

+ `AlreadyExistsException`

+ `InvalidInputException`

+ `OperationTimeoutException`

## DeleteClassifier Action \(Python: delete\_classifier\)<a name="aws-glue-api-crawler-classifiers-DeleteClassifier"></a>

Removes a classifier from the Data Catalog\.

**Request**

+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  Name of the classifier to remove\.

**Response**

+ *No Response parameters\.*

**Errors**

+ `EntityNotFoundException`

+ `OperationTimeoutException`

## GetClassifier Action \(Python: get\_classifier\)<a name="aws-glue-api-crawler-classifiers-GetClassifier"></a>

Retrieve a classifier by name\.

**Request**

+ `Name` – String, matching the [Single-line string pattern](aws-glue-api-common.md#aws-glue-api-regex-oneLine)\. Required\.

  Name of the classifier to retrieve\.

**Response**

+ `Classifier` – A Classifier object\.

  The requested classifier\.

**Errors**

+ `EntityNotFoundException`

+ `OperationTimeoutException`

## GetClassifiers Action \(Python: get\_classifiers\)<a name="aws-glue-api-crawler-classifiers-GetClassifiers"></a>

Lists all classifier objects in the Data Catalog\.

**Request**

+ `MaxResults` – Number \(integer\)\.

  Size of the list to return \(optional\)\.

+ `NextToken` – String\.

  An optional continuation token\.

**Response**

+ `Classifiers` – An array of [Classifier](#aws-glue-api-crawler-classifiers-Classifier)s\.

  The requested list of classifier objects\.

+ `NextToken` – String\.

  A continuation token\.

**Errors**

+ `OperationTimeoutException`

## UpdateClassifier Action \(Python: update\_classifier\)<a name="aws-glue-api-crawler-classifiers-UpdateClassifier"></a>

Modifies an existing classifier \(either a `GrokClassifier` or an `XMLClassifier`\)\.

**Request**

+ `GrokClassifier` – An UpdateGrokClassifierRequest object\.

  A `GrokClassifier` object with updated fields\.

+ `XMLClassifier` – An UpdateXMLClassifierRequest object\.

  An `XMLClassifier` object with updated fields\.

**Response**

+ *No Response parameters\.*

**Errors**

+ `InvalidInputException`

+ `VersionMismatchException`

+ `EntityNotFoundException`

+ `OperationTimeoutException`