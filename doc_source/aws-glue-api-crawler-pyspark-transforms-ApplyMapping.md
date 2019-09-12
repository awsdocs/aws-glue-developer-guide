# ApplyMapping Class<a name="aws-glue-api-crawler-pyspark-transforms-ApplyMapping"></a>

Applies a mapping in a `DynamicFrame`\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-ApplyMapping-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-ApplyMapping-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-ApplyMapping-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-ApplyMapping-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-ApplyMapping-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-ApplyMapping-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-ApplyMapping-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-ApplyMapping-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-ApplyMapping-describe)

## \_\_call\_\_\(frame, mappings, transformation\_ctx = "", info = "", stageThreshold = 0, totalThreshold = 0\)<a name="aws-glue-api-crawler-pyspark-transforms-ApplyMapping-__call__"></a>

Applies a declarative mapping to a specified `DynamicFrame`\.
+ `frame` – The `DynamicFrame` in which to apply the mapping \(required\)\.
+ `mappings` – A list of mapping tuples, each consisting of: \(source column, source type, target column, target type\)\. Required\.

  If the source column has dots in it, the mapping will not work unless you place back\-ticks around it \(````\)\. For example, to map `this.old.name` \(string\) to `thisNewName` \(string\), you would use the following tuple:

  ```
  ("`this.old.name`", "string", "thisNewName", "string")
  ```
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string associated with errors in the transformation \(optional\)\.
+ `stageThreshold` – The maximum number of errors that can occur in the transformation before it errors out \(optional; the default is zero\)\.
+ `totalThreshold` – The maximum number of errors that can occur overall before processing errors out \(optional; the default is zero\)\.

Returns only the fields of the `DynamicFrame` specified in the "mapping" tuples\.

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-ApplyMapping-apply"></a>

Inherited from `GlueTransform` [apply](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)\.

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-ApplyMapping-name"></a>

Inherited from `GlueTransform` [name](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)\.

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-ApplyMapping-describeArgs"></a>

Inherited from `GlueTransform` [describeArgs](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)\.

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-ApplyMapping-describeReturn"></a>

Inherited from `GlueTransform` [describeReturn](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)\.

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-ApplyMapping-describeTransform"></a>

Inherited from `GlueTransform` [describeTransform](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)\.

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-ApplyMapping-describeErrors"></a>

Inherited from `GlueTransform` [describeErrors](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)\.

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-ApplyMapping-describe"></a>

Inherited from `GlueTransform` [describe](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)\.