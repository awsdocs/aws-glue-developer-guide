# Join Class<a name="aws-glue-api-crawler-pyspark-transforms-join"></a>

Performs an equality join on two `DynamicFrames`\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-join-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-join-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-join-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-join-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-join-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-join-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-join-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-join-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-join-describe)

## \_\_call\_\_\(frame1, frame2, keys1, keys2, transformation\_ctx = ""\)<a name="aws-glue-api-crawler-pyspark-transforms-join-__call__"></a>

Performs an equality join on two `DynamicFrames`\.
+ `frame1` – The first `DynamicFrame` to join \(required\)\.
+ `frame2` – The second `DynamicFrame` to join \(required\)\.
+ `keys1` – The keys to join on for the first frame \(required\)\.
+ `keys2` – The keys to join on for the second frame \(required\)\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.

Returns a new `DynamicFrame` obtained by joining the two `DynamicFrames`\.

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-join-apply"></a>

Inherited from `GlueTransform` [apply](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-join-name"></a>

Inherited from `GlueTransform` [name](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-join-describeArgs"></a>

Inherited from `GlueTransform` [describeArgs](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-join-describeReturn"></a>

Inherited from `GlueTransform` [describeReturn](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-join-describeTransform"></a>

Inherited from `GlueTransform` [describeTransform](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-join-describeErrors"></a>

Inherited from `GlueTransform` [describeErrors](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-join-describe"></a>

Inherited from `GlueTransform` [describe](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)