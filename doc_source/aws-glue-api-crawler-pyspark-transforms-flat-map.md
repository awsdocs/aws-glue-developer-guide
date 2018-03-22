# FlatMap Class<a name="aws-glue-api-crawler-pyspark-transforms-flat-map"></a>

Applies a transform to each `DynamicFrame` in a collection and flattens the results\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-flat-map-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-flat-map-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-flat-map-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-flat-map-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-flat-map-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-flat-map-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-flat-map-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-flat-map-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-flat-map-describe)

## \_\_call\_\_\(dfc, BaseTransform, frame\_name, transformation\_ctx = "", \*\*base\_kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-flat-map-__call__"></a>

Applies a transform to each `DynamicFrame` in a collection and flattens the results\.
+ `dfc` – The `DynamicFrameCollection` over which to flatmap \(required\)\.
+ `BaseTransform` – A transform derived from `GlueTransform` to apply to each member of the collection \(required\)\.
+ `frame_name` – The argument name to pass the elements of the collection to \(required\)\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `base_kwargs` – Arguments to pass to the base transform \(required\)\.

Returns a new `DynamicFrameCollection` created by applying the transform to each `DynamicFrame` in the source `DynamicFrameCollection`\.

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-flat-map-apply"></a>

Inherited from `GlueTransform` [apply](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)\.

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-flat-map-name"></a>

Inherited from `GlueTransform` [name](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)\.

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-flat-map-describeArgs"></a>

Inherited from `GlueTransform` [describeArgs](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)\.

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-flat-map-describeReturn"></a>

Inherited from `GlueTransform` [describeReturn](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)\.

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-flat-map-describeTransform"></a>

Inherited from `GlueTransform` [describeTransform](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)\.

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-flat-map-describeErrors"></a>

Inherited from `GlueTransform` [describeErrors](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)\.

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-flat-map-describe"></a>

Inherited from `GlueTransform` [describe](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)\.