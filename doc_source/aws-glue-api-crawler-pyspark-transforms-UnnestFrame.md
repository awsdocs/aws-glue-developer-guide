# UnnestFrame Class<a name="aws-glue-api-crawler-pyspark-transforms-UnnestFrame"></a>

Unnests a `DynamicFrame`, flattens nested objects to top\-level elements, and generates joinkeys for array objects\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-UnnestFrame-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-UnnestFrame-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-UnnestFrame-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-UnnestFrame-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-UnnestFrame-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-UnnestFrame-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-UnnestFrame-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-UnnestFrame-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-UnnestFrame-describe)

## \_\_call\_\_\(frame, transformation\_ctx = "", info="", stageThreshold=0, totalThreshold=0\)<a name="aws-glue-api-crawler-pyspark-transforms-UnnestFrame-__call__"></a>

Unnests a `DynamicFrame`\. Flattens nested objects to top\-level elements, and generates joinkeys for array objects\.
+ `frame` – The `DynamicFrame` to unnest \(required\)\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string associated with errors in the transformation \(optional\)\.
+ `stageThreshold` – The maximum number of errors that can occur in the transformation before it errors out \(optional; the default is zero\)\.
+ `totalThreshold` – The maximum number of errors that can occur overall before processing errors out \(optional; the default is zero\)\.

Returns the unnested `DynamicFrame`\.

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-UnnestFrame-apply"></a>

Inherited from `GlueTransform` [apply](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)\.

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-UnnestFrame-name"></a>

Inherited from `GlueTransform` [name](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)\.

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-UnnestFrame-describeArgs"></a>

Inherited from `GlueTransform` [describeArgs](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)\.

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-UnnestFrame-describeReturn"></a>

Inherited from `GlueTransform` [describeReturn](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)\.

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-UnnestFrame-describeTransform"></a>

Inherited from `GlueTransform` [describeTransform](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)\.

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-UnnestFrame-describeErrors"></a>

Inherited from `GlueTransform` [describeErrors](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)\.

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-UnnestFrame-describe"></a>

Inherited from `GlueTransform` [describe](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)\.