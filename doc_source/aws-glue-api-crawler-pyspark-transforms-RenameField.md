# RenameField Class<a name="aws-glue-api-crawler-pyspark-transforms-RenameField"></a>

Renames a node within a `DynamicFrame`\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-RenameField-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-RenameField-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-RenameField-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-RenameField-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-RenameField-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-RenameField-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-RenameField-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-RenameField-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-RenameField-describe)

## \_\_call\_\_\(frame, old\_name, new\_name, transformation\_ctx = "", info = "", stageThreshold = 0, totalThreshold = 0\)<a name="aws-glue-api-crawler-pyspark-transforms-RenameField-__call__"></a>

Renames a node within a `DynamicFrame`\.
+ `frame` – The `DynamicFrame` in which to rename a node \(required\)\.
+ `old_name` – Full path to the node to rename \(required\)\.

  If the old name has dots in it, RenameField will not work unless you place back\-ticks around it \(````\)\. For example, to replace `this.old.name` with `thisNewName`, you would call RenameField as follows:

  ```
  newDyF = RenameField(oldDyF, "`this.old.name`", "thisNewName")
  ```
+ `new_name` – New name, including full path \(required\)\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string associated with errors in the transformation \(optional\)\.
+ `stageThreshold` – The maximum number of errors that can occur in the transformation before it errors out \(optional; the default is zero\)\.
+ `totalThreshold` – The maximum number of errors that can occur overall before processing errors out \(optional; the default is zero\)\.

Returns a `DynamicFrame` with the specified field renamed\.

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-RenameField-apply"></a>

Inherited from `GlueTransform` [apply](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)\.

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-RenameField-name"></a>

Inherited from `GlueTransform` [name](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)\.

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-RenameField-describeArgs"></a>

Inherited from `GlueTransform` [describeArgs](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)\.

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-RenameField-describeReturn"></a>

Inherited from `GlueTransform` [describeReturn](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)\.

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-RenameField-describeTransform"></a>

Inherited from `GlueTransform` [describeTransform](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)\.

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-RenameField-describeErrors"></a>

Inherited from `GlueTransform` [describeErrors](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)\.

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-RenameField-describe"></a>

Inherited from `GlueTransform` [describe](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)\.