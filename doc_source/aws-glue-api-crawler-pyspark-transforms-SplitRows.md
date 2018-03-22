# SplitRows Class<a name="aws-glue-api-crawler-pyspark-transforms-SplitRows"></a>

Splits a `DynamicFrame` in two by specified rows\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-SplitRows-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-SplitRows-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-SplitRows-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-SplitRows-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-SplitRows-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-SplitRows-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-SplitRows-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-SplitRows-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-SplitRows-describe)

## \_\_call\_\_\(frame, comparison\_dict, name1="frame1", name2="frame2", transformation\_ctx = "", info = None, stageThreshold = 0, totalThreshold = 0\)<a name="aws-glue-api-crawler-pyspark-transforms-SplitRows-__call__"></a>

Splits one or more rows in a `DynamicFrame` off into a new `DynamicFrame`\.
+ `frame` – The source `DynamicFrame` to split into two new ones \(required\)\.
+ `comparison_dict` – A dictionary where the key is the full path to a column, and the value is another dictionary mapping comparators to the value to which the column values are compared\. For example, `{"age": {">": 10, "<": 20}}` splits rows where the value of "age" is between 10 and 20, exclusive, from rows where "age" is outside that range \(required\)\.
+ `name1` – The name to assign to the `DynamicFrame` that will contain the rows to be split off \(optional\)\.
+ `name2` – The name to assign to the `DynamicFrame` that will contain the rows that remain after the specified rows are split off \(optional\)\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string associated with errors in the transformation \(optional\)\.
+ `stageThreshold` – The maximum number of errors that can occur in the transformation before it errors out \(optional; the default is zero\)\.
+ `totalThreshold` – The maximum number of errors that can occur overall before processing errors out \(optional; the default is zero\)\.

Returns a `DynamicFrameCollection` that contains two `DynamicFrames`: one contains only the specified rows to be split, and the other contains all remaining rows\.

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-SplitRows-apply"></a>

Inherited from `GlueTransform` [apply](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)\.

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SplitRows-name"></a>

Inherited from `GlueTransform` [name](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)\.

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SplitRows-describeArgs"></a>

Inherited from `GlueTransform` [describeArgs](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)\.

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SplitRows-describeReturn"></a>

Inherited from `GlueTransform` [describeReturn](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)\.

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SplitRows-describeTransform"></a>

Inherited from `GlueTransform` [describeTransform](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)\.

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SplitRows-describeErrors"></a>

Inherited from `GlueTransform` [describeErrors](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)\.

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SplitRows-describe"></a>

Inherited from `GlueTransform` [describe](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)\.