# DropFields Class<a name="aws-glue-api-crawler-pyspark-transforms-DropFields"></a>

Drops fields within a `DynamicFrame`\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-DropFields-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-DropFields-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-DropFields-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-DropFields-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-DropFields-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-DropFields-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-DropFields-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-DropFields-describe)

## \_\_call\_\_\(frame, paths, transformation\_ctx = "", info = "", stageThreshold = 0, totalThreshold = 0\)<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-__call__"></a>

Drops nodes within a `DynamicFrame`\.
+ `frame` – The `DynamicFrame` in which to drop the nodes \(required\)\.
+ `paths` – A list of full paths to the nodes to drop \(required\)\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string associated with errors in the transformation \(optional\)\.
+ `stageThreshold` – The maximum number of errors that can occur in the transformation before it errors out \(optional; the default is zero\)\.
+ `totalThreshold` – The maximum number of errors that can occur overall before processing errors out \(optional; the default is zero\)\.

Returns a new `DynamicFrame` without the specified fields\.

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-apply"></a>

Inherited from `GlueTransform` [apply](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)\.

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-name"></a>

Inherited from `GlueTransform` [name](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)\.

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-describeArgs"></a>

Inherited from `GlueTransform` [describeArgs](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)\.

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-describeReturn"></a>

Inherited from `GlueTransform` [describeReturn](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)\.

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-describeTransform"></a>

Inherited from `GlueTransform` [describeTransform](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)\.

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-describeErrors"></a>

Inherited from `GlueTransform` [describeErrors](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)\.

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-describe"></a>

Inherited from `GlueTransform` [describe](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)\.