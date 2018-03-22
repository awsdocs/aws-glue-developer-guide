# SelectFields Class<a name="aws-glue-api-crawler-pyspark-transforms-SelectFields"></a>

Gets fields in a `DynamicFrame`\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-SelectFields-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-SelectFields-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-SelectFields-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-SelectFields-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-SelectFields-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-SelectFields-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-SelectFields-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-SelectFields-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-SelectFields-describe)

## \_\_call\_\_\(frame, paths, transformation\_ctx = "", info = "", stageThreshold = 0, totalThreshold = 0\)<a name="aws-glue-api-crawler-pyspark-transforms-SelectFields-__call__"></a>

Gets fields \(nodes\) in a `DynamicFrame`\.
+ `frame` – The `DynamicFrame` in which to select fields \(required\)\.
+ `paths` – A list of full paths to the fields to select \(required\)\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string associated with errors in the transformation \(optional\)\.
+ `stageThreshold` – The maximum number of errors that can occur in the transformation before it errors out \(optional; the default is zero\)\.
+ `totalThreshold` – The maximum number of errors that can occur overall before processing errors out \(optional; the default is zero\)\.

Returns a new `DynamicFrame` containing only the specified fields\.

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-SelectFields-apply"></a>

Inherited from `GlueTransform` [apply](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)\.

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SelectFields-name"></a>

Inherited from `GlueTransform` [name](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)\.

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SelectFields-describeArgs"></a>

Inherited from `GlueTransform` [describeArgs](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)\.

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SelectFields-describeReturn"></a>

Inherited from `GlueTransform` [describeReturn](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)\.

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SelectFields-describeTransform"></a>

Inherited from `GlueTransform` [describeTransform](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)\.

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SelectFields-describeErrors"></a>

Inherited from `GlueTransform` [describeErrors](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)\.

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SelectFields-describe"></a>

Inherited from `GlueTransform` [describe](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)\.