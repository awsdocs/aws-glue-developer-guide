# MapToCollection Class<a name="aws-glue-api-crawler-pyspark-transforms-MapToCollection"></a>

Applies a transform to each `DynamicFrame` in the specified `DynamicFrameCollection`\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-MapToCollection-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-MapToCollection-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-MapToCollection-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-MapToCollection-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-MapToCollection-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-MapToCollection-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-MapToCollection-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-MapToCollection-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-MapToCollection-describe)

## \_\_call\_\_\(dfc, BaseTransform, frame\_name, transformation\_ctx = "", \*\*base\_kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-MapToCollection-__call__"></a>

Applies a transform function to each `DynamicFrame` in the specified `DynamicFrameCollection`\.
+ `dfc` – The `DynamicFrameCollection` over which to apply the transform function \(required\)\.
+ `callable` – A callable transform function to apply to each member of the collection \(required\)\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.

Returns a new `DynamicFrameCollection` created by applying the transform to each `DynamicFrame` in the source `DynamicFrameCollection`\.

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-MapToCollection-apply"></a>

Inherited from `GlueTransform` [apply](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-MapToCollection-name"></a>

Inherited from `GlueTransform` [name](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)\.

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-MapToCollection-describeArgs"></a>

Inherited from `GlueTransform` [describeArgs](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)\.

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-MapToCollection-describeReturn"></a>

Inherited from `GlueTransform` [describeReturn](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)\.

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-MapToCollection-describeTransform"></a>

Inherited from `GlueTransform` [describeTransform](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)\.

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-MapToCollection-describeErrors"></a>

Inherited from `GlueTransform` [describeErrors](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)\.

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-MapToCollection-describe"></a>

Inherited from `GlueTransform` [describe](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)\.