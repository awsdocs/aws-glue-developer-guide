# Spigot Class<a name="aws-glue-api-crawler-pyspark-transforms-spigot"></a>

Writes sample records to a specified destination during a transformation\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-spigot-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-spigot-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-spigot-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-spigot-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-spigot-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-spigot-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-spigot-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-spigot-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-spigot-describe)

## \_\_call\_\_\(frame, path, options, transformation\_ctx = ""\)<a name="aws-glue-api-crawler-pyspark-transforms-spigot-__call__"></a>

Writes sample records to a specified destination during a transformation\.
+ `frame` – The `DynamicFrame` to spigot \(required\)\.
+ `path` – The path to the destination to write to \(required\)\.
+ `options` – JSON key\-value pairs specifying options \(optional\)\. The `"topk"` option specifies that the first k records should be written\. The `"prob"` option specifies the probability \(as a decimal\) of picking any given record, to be used in selecting records to write\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.

Returns the input `DynamicFrame` with an additional write step\.

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-spigot-apply"></a>

Inherited from `GlueTransform` [apply](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-spigot-name"></a>

Inherited from `GlueTransform` [name](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-spigot-describeArgs"></a>

Inherited from `GlueTransform` [describeArgs](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-spigot-describeReturn"></a>

Inherited from `GlueTransform` [describeReturn](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-spigot-describeTransform"></a>

Inherited from `GlueTransform` [describeTransform](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-spigot-describeErrors"></a>

Inherited from `GlueTransform` [describeErrors](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-spigot-describe"></a>

Inherited from `GlueTransform` [describe](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)