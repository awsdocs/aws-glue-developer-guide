# DropNullFields Class<a name="aws-glue-api-crawler-pyspark-transforms-DropNullFields"></a>

Drops all null fields in a `DynamicFrame` whose type is `NullType`\. These are fields with missing or null values in every record in the `DynamicFrame` data set\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-DropNullFields-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-DropNullFields-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-DropNullFields-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-DropNullFields-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-DropNullFields-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-DropNullFields-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-DropNullFields-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-DropNullFields-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-DropNullFields-describe)

## \_\_call\_\_\(frame, transformation\_ctx = "", info = "", stageThreshold = 0, totalThreshold = 0\)<a name="aws-glue-api-crawler-pyspark-transforms-DropNullFields-__call__"></a>

Drops all null fields in a `DynamicFrame` whose type is `NullType`\. These are fields with missing or null values in every record in the `DynamicFrame` data set\.
+ `frame` – The `DynamicFrame` in which to drop null fields \(required\)\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string associated with errors in the transformation \(optional\)\.
+ `stageThreshold` – The maximum number of errors that can occur in the transformation before it errors out \(optional; the default is zero\)\.
+ `totalThreshold` – The maximum number of errors that can occur overall before processing errors out \(optional; the default is zero\)\.

Returns a new `DynamicFrame` with no null fields\.

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-DropNullFields-apply"></a>
+ `cls` – cls

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropNullFields-name"></a>
+ `cls` – cls

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropNullFields-describeArgs"></a>
+ `cls` – cls

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropNullFields-describeReturn"></a>
+ `cls` – cls

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropNullFields-describeTransform"></a>
+ `cls` – cls

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropNullFields-describeErrors"></a>
+ `cls` – cls

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropNullFields-describe"></a>
+ `cls` – cls