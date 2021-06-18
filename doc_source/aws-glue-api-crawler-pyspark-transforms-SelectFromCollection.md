# SelectFromCollection Class<a name="aws-glue-api-crawler-pyspark-transforms-SelectFromCollection"></a>

Selects one `DynamicFrame` in a `DynamicFrameCollection`\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-SelectFromCollection-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-SelectFromCollection-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-SelectFromCollection-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-SelectFromCollection-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-SelectFromCollection-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-SelectFromCollection-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-SelectFromCollection-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-SelectFromCollection-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-SelectFromCollection-describe)

## \_\_call\_\_\(dfc, key, transformation\_ctx = ""\)<a name="aws-glue-api-crawler-pyspark-transforms-SelectFromCollection-__call__"></a>

Gets one `DynamicFrame` from a `DynamicFrameCollection`\.
+ `dfc` – The `DynamicFrameCollection` from which the `DynamicFrame` should be selected \(required\)\.
+ `key` – The key of the `DynamicFrame` to select \(required\)\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.

Returns the specified `DynamicFrame`\.

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-SelectFromCollection-apply"></a>

Inherited from `GlueTransform` [apply](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)\.

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SelectFromCollection-name"></a>

Inherited from `GlueTransform` [name](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)\.

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SelectFromCollection-describeArgs"></a>

Inherited from `GlueTransform` [describeArgs](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)\.

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SelectFromCollection-describeReturn"></a>

Inherited from `GlueTransform` [describeReturn](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)\.

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SelectFromCollection-describeTransform"></a>

Inherited from `GlueTransform` [describeTransform](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)\.

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SelectFromCollection-describeErrors"></a>

Inherited from `GlueTransform` [describeErrors](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)\.

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-SelectFromCollection-describe"></a>

Inherited from `GlueTransform` [describe](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)\.

## Example for SelectFromCollection<a name="pyspark-SelectFromCollection-example"></a>

The SelectFromCollection function retrieves a specific DynamicFrame from a collection of DynamicFrames\.

 This example uses the following DynamicFrame collection as input\.

```
dyf_relationize.keys() 
      
dict_keys(['root', 'root_customers'])
```

The following command retrieves the first DynamicFrame, with the key `root`\.

```
dyf_selectFromCollection = SelectFromCollection.apply(dyf_relationize, 'root')

dyf_selectFromCollection.toDF().show()
+---------+-----+
|customers| zip|
+---------+-----+
| 1|75091|
| 2|75023|
| 3|90093|
+---------+-----+
```

This next command retrieves the second DynamicFrame from the collection, with the key `root_customers`\.

```
dyf_selectFromCollection = SelectFromCollection.apply(dyf_relationize, 'root_customers') 

dyf_selectFromCollection.toDF().show() 
+---+-----+---------------------+----------------+
| id|index|customers.val.address|customers.val.id|
+---+-----+---------------------+----------------+
| 2| 0| 66 P Street, NY| 343|
| 3| 0| 708 Fed Ln, CA| 932|
| 3| 1| 807 Deccan Dr, CA| 102|
| 1| 0| 108 Park Street, TX| 623|
| 1| 1| 763 Marsh Ln, TX| 231|
+---+-----+---------------------+----------------+
```