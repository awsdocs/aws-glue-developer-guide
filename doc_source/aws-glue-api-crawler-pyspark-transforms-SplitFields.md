# SplitFields Class<a name="aws-glue-api-crawler-pyspark-transforms-SplitFields"></a>

Splits a `DynamicFrame` into two new ones, by specified fields\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-SplitFields-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-SplitFields-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-SplitFields-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-SplitFields-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-SplitFields-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-SplitFields-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-SplitFields-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-SplitFields-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-SplitFields-describe)

## \_\_call\_\_\(frame, paths, name1 = None, name2 = None, transformation\_ctx = "", info = "", stageThreshold = 0, totalThreshold = 0\)<a name="aws-glue-api-crawler-pyspark-transforms-SplitFields-__call__"></a>

Splits one or more fields in a `DynamicFrame` off into a new `DynamicFrame` and creates another new `DynamicFrame` containing the fields that remain\.
+ `frame` – The source `DynamicFrame` to split into two new ones \(required\)\.
+ `paths` – A list of full paths to the fields to be split \(required\)\.
+ `name1` – The name to assign to the `DynamicFrame` that will contain the fields to be split off \(optional\)\. If no name is supplied, the name of the source frame is used with "1" appended\.
+ `name2` – The name to assign to the `DynamicFrame` that will contain the fields that remain after the specified fields are split off \(optional\)\. If no name is provided, the name of the source frame is used with "2" appended\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string associated with errors in the transformation \(optional\)\.
+ `stageThreshold` – The maximum number of errors that can occur in the transformation before it errors out \(optional; the default is zero\)\.
+ `totalThreshold` – The maximum number of errors that can occur overall before processing errors out \(optional; the default is zero\)\.

Returns a `DynamicFrameCollection` containing two `DynamicFrames`: one contains only the specified fields to split off, and the other contains the remaining fields\.

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-SplitFields-apply"></a>

Inherited from `GlueTransform` [apply](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)\.

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SplitFields-name"></a>

Inherited from `GlueTransform` [name](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)\.

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SplitFields-describeArgs"></a>

Inherited from `GlueTransform` [describeArgs](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)\.

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SplitFields-describeReturn"></a>

Inherited from `GlueTransform` [describeReturn](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)\.

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SplitFields-describeTransform"></a>

Inherited from `GlueTransform` [describeTransform](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)\.

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SplitFields-describeErrors"></a>

Inherited from `GlueTransform` [describeErrors](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)\.

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SplitFields-describe"></a>

Inherited from `GlueTransform` [describe](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)\.