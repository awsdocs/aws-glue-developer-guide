# DropFields Class<a name="aws-glue-api-crawler-pyspark-transforms-DropFields"></a>

Drops fields within a `DynamicFrame`\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-DropFields-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-DropFields-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-DropFields-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-DropFields-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-DropFields-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-DropFields-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-DropFields-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-DropFields-describe)

## \_\_call\_\_\(frame, paths, transformation\_ctx = "", info = "", stageThreshold = 0, totalThreshold = 0\)<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-__call__"></a>

Drops nodes within a `DynamicFrame`\.
+ `frame` – The `DynamicFrame` in which to drop the nodes \(required\)\.
+ `paths` – A list of full paths to the nodes to drop \(required\)\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string associated with errors in the transformation \(optional\)\.
+ `stageThreshold` – The maximum number of errors that can occur in the transformation before it errors out \(optional; the default is zero\)\.
+ `totalThreshold` – The maximum number of errors that can occur overall before processing errors out \(optional; the default is zero\)\.

Returns a new `DynamicFrame` without the specified fields\.

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-apply"></a>

Inherited from `GlueTransform` [apply](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)\.

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-name"></a>

Inherited from `GlueTransform` [name](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)\.

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-describeArgs"></a>

Inherited from `GlueTransform` [describeArgs](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)\.

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-describeReturn"></a>

Inherited from `GlueTransform` [describeReturn](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)\.

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-describeTransform"></a>

Inherited from `GlueTransform` [describeTransform](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)\.

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-describeErrors"></a>

Inherited from `GlueTransform` [describeErrors](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)\.

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-DropFields-describe"></a>

Inherited from `GlueTransform` [describe](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)\.

## Examples<a name="aws-glue-api-crawler-pyspark-transforms-DropFields_examples"></a>
+ [Example dataset](#aws-glue-api-crawler-pyspark-transforms-DropFields_dataset)
+ [Drop a top\-level field](#aws-glue-api-crawler-pyspark-transforms-DropFields_example1)
+ [Drop a nested field](#aws-glue-api-crawler-pyspark-transforms-DropFields_example2)
+ [Drop a nested field in an array](#aws-glue-api-crawler-pyspark-transforms-DropFields_example3)

### Dataset used for DropFields examples<a name="aws-glue-api-crawler-pyspark-transforms-DropFields_dataset"></a>

The following dataset is used for the DropFields examples:

```
{name: Sally, age: 23, location: {state: WY, county: Fremont}, friends: []}
  {name: Varun, age: 34, location: {state: NE, county: Douglas}, friends: [{name: Arjun, age: 3}]}
  {name: George, age: 52, location: {state: NY}, friends: [{name: Fred}, {name: Amy, age: 15}]}
  {name: Haruki, age: 21, location: {state: AK, county: Denali}}
  {name: Sheila, age: 63, friends: [{name: Nancy, age: 22}]}
```

This dataset has the following schema:

```
root
  |-- name: string
  |-- age: int
  |-- location: struct
  |    |-- state: string
  |    |-- county: string
  |-- friends: array
  |    |-- element: struct
  |    |    |-- name: string
  |    |    |-- age: int
```

### Example: Drop a top\-level field<a name="aws-glue-api-crawler-pyspark-transforms-DropFields_example1"></a>

Use code similar to the following to drop the `age` field:

```
df_no_age = DropFields.apply(df, paths=['age'])
```

Resulting dataset:

```
  {name: Sally, location: {state: WY, county: Fremont}, friends: []}
  {name: Varun, location: {state: NE, county: Douglas}, friends: [{name: Arjun, age: 3}]}
  {name: George, location: {state: NY}, friends: [{name: Fred}, {name: Amy, age: 15}]}
  {name: Haruki, location: {state: AK, county: Denali}}
  {name: Sheila, friends: [{name: Nancy, age: 22}]}
```

Resulting schema:

```
  root
  |-- name: string
  |-- location: struct
  |    |-- state: string
  |    |-- county: string
  |-- friends: array
  |    |-- element: struct
  |    |    |-- name: string
  |    |    |-- age: int
```

### Example: Drop a nested field<a name="aws-glue-api-crawler-pyspark-transforms-DropFields_example2"></a>

To drop a nested field, you can qualify the field with a `'.'`\.

```
df_no_county = DropFields.apply(df, paths=['location.county'])
```

Resulting dataset:

```
{name: Sally, age: 23, location: {state: WY}, friends: []}
  {name: Varun, age: 34, location: {state: NE}, friends: [{name: Arjun, age: 3}]}
  {name: George, age: 52, location: {state: NY}, friends: [{name: Fred}, {name: Amy, age: 15}]}
  {name: Haruki, age: 21, location: {state: AK}}
  {name: Sheila, age: 63, friends: [{name: Nancy, age: 22}]}
```

If you drop the last element of a `struct` type, the transform removes the entire `struct`\. 

```
df_no_county = DropFields.apply(df, paths=['location.state])
```

Resulting schema:

```
root
  |-- name: string
  |-- age: int
  |-- friends: array
  |    |-- element: struct
  |    |    |-- name: string
  |    |    |-- age: int
```

### Example: Drop a nested field from an array<a name="aws-glue-api-crawler-pyspark-transforms-DropFields_example3"></a>

No special syntax is needed to drop a field from inside a `struct` nested inside an `array`\. For example, we can drop the **`age`** field from the **`friends`** array with the following:

```
df_no_friend_age = DropFields.apply(df, paths=['friends.age'])
```

Resulting dataset:

```
{name: Sally, age: 23, location: {state: WY, county: Fremont}}
  {name: Varun, age: 34, location: {state: NE, county: Douglas}, friends: [{name: Arjun}]}
  {name: George, age: 52, location: {state: NY}, friends: [{name: Fred}, {name: Amy}]}
  {name: Haruki, age: 21, location: {state: AK, county: Denali}}
  {name: Sheila, age: 63, friends: [{name: Nancy}]}
```

Resulting schema:

```
 root
  |-- name: string
  |-- age: int
  |-- location: struct
  |    |-- state: string
  |    |-- county: string
  |-- friends: array
  |    |-- element: struct
  |    |    |-- name: string
```

## Example for DropFields<a name="pyspark-DropFields-example"></a>

The backticks \(`\) around `.zip` in the following example are needed because the column name contains a period \(\.\)\.

```
dyf_dropfields = DropFields.apply(frame = dyf_join, paths = "`.zip`")
```