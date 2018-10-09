# ResolveChoice Class<a name="aws-glue-api-crawler-pyspark-transforms-ResolveChoice"></a>

Resolves a choice type within a `DynamicFrame`\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-ResolveChoice-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-ResolveChoice-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-ResolveChoice-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-ResolveChoice-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-ResolveChoice-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-ResolveChoice-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-ResolveChoice-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-ResolveChoice-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-ResolveChoice-describe)

## \_\_call\_\_\(frame, specs = None, choice = "", transformation\_ctx = "", info = "", stageThreshold = 0, totalThreshold = 0\)<a name="aws-glue-api-crawler-pyspark-transforms-ResolveChoice-__call__"></a>

Provides information for resolving ambiguous types within a `DynamicFrame`\. Returns the resulting `DynamicFrame`\.
+ `frame` – The `DynamicFrame` in which to resolve the choice type \(required\)\.
+ `specs` – A list of specific ambiguities to resolve, each in the form of a tuple:`(path, action)`\. The `path` value identifies a specific ambiguous element, and the `action` value identifies the corresponding resolution\. Only one of the `spec` and `choice` parameters can be used\. If the `spec` parameter is not `None`, then the `choice` parameter must be an empty string\. Conversely if the `choice` is not an empty string, then the `spec` parameter must be `None`\. If neither parameter is provided, AWS Glue tries to parse the schema and use it to resolve ambiguities\. 

  The `action` portion of a `specs` tuple can specify one of four resolution strategies:
  + `cast`:  Allows you to specify a type to cast to \(for example, `cast:int`\)\.
  + `make_cols`:  Resolves a potential ambiguity by flattening the data\. For example, if `columnA` could be an `int` or a `string`, the resolution is to produce two columns named `columnA_int` and `columnA_string` in the resulting `DynamicFrame`\.
  + `make_struct`:  Resolves a potential ambiguity by using a struct to represent the data\. For example, if data in a column could be an `int` or a `string`, using the `make_struct` action produces a column of structures in the resulting `DynamicFrame` with each containing both an `int` and a `string`\.
  + `project`:  Resolves a potential ambiguity by retaining only values of a specified type in the resulting `DynamicFrame`\. For example, if data in a `ChoiceType` column could be an `int` or a `string`, specifying a `project:string` action drops columns from the resulting `DynamicFrame` which are not type `string`\. 

  If the `path` identifies an array, place empty square brackets after the name of the array to avoid ambiguity\. For example, suppose you are working with data structured as follows:

  ```
  "myList": [
    { "price": 100.00 },
    { "price": "$100.00" }
  ]
  ```

  You can select the numeric rather than the string version of the price by setting the `path` to `"myList[].price"`, and the `action` to `"cast:double"`\.
+ `choice` – The default resolution action if the `specs` parameter is `None`\. If the `specs` parameter is not `None`, then this must not be set to anything but an empty string\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string associated with errors in the transformation \(optional\)\.
+ `stageThreshold` – The maximum number of errors that can occur in the transformation before it errors out \(optional; the default is zero\)\.
+ `totalThreshold` – The maximum number of errors that can occur overall before processing errors out \(optional; the default is zero\)\.

Returns a `DynamicFrame` with the resolved choice\.

**Example**  

```
df1 = ResolveChoice.apply(df, choice = "make_cols")
df2 = ResolveChoice.apply(df, specs = [("a.b", "make_struct"), ("c.d", "cast:double")])
```

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-ResolveChoice-apply"></a>

Inherited from `GlueTransform` [apply](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)\.

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-ResolveChoice-name"></a>

Inherited from `GlueTransform` [name](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)\.

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-ResolveChoice-describeArgs"></a>

Inherited from `GlueTransform` [describeArgs](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)\.

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-ResolveChoice-describeReturn"></a>

Inherited from `GlueTransform` [describeReturn](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)\.

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-ResolveChoice-describeTransform"></a>

Inherited from `GlueTransform` [describeTransform](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)\.

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-ResolveChoice-describeErrors"></a>

Inherited from `GlueTransform` [describeErrors](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)\.

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-ResolveChoice-describe"></a>

Inherited from `GlueTransform` [describe](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)\.