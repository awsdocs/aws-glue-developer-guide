# DynamicFrame Class<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame"></a>

One of the major abstractions in Apache Spark is the SparkSQL `DataFrame`, which is similar to the `DataFrame` construct found in R and Pandas\. A `DataFrame` is similar to a table and supports functional\-style \(map/reduce/filter/etc\.\) operations and SQL operations \(select, project, aggregate\)\.

`DataFrames` are powerful and widely used, but they have limitations with respect to extract, transform, and load \(ETL\) operations\. Most significantly, they require a schema to be specified before any data is loaded\. SparkSQL addresses this by making two passes over the data—the first to infer the schema, and the second to load the data\. However, this inference is limited and doesn't address the realities of messy data\. For example, the same field might be of a different type in different records\. Apache Spark often gives up and reports the type as `string` using the original field text\. This might not be correct, and you might want finer control over how schema discrepancies are resolved\. And for large datasets, an additional pass over the source data might be prohibitively expensive\.

To address these limitations, AWS Glue introduces the `DynamicFrame`\. A `DynamicFrame` is similar to a `DataFrame`, except that each record is self\-describing, so no schema is required initially\. Instead, AWS Glue computes a schema on\-the\-fly when required, and explicitly encodes schema inconsistencies using a choice \(or union\) type\. You can resolve these inconsistencies to make your datasets compatible with data stores that require a fixed schema\.

Similarly, a `DynamicRecord` represents a logical record within a `DynamicFrame`\. It is like a row in a Spark `DataFrame`, except that it is self\-describing and can be used for data that does not conform to a fixed schema\.

You can convert `DynamicFrames` to and from `DataFrames` once you resolve any schema inconsistencies\. 

##  — Construction —<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-_constructing"></a>
+ [\_\_init\_\_](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-__init__)
+ [fromDF](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-fromDF)
+ [toDF](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-toDF)

## \_\_init\_\_<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-__init__"></a>

**`__init__(jdf, glue_ctx, name)`**
+ `jdf` – A reference to the data frame in the Java Virtual Machine \(JVM\)\.
+ `glue_ctx` – A [GlueContext Class](aws-glue-api-crawler-pyspark-extensions-glue-context.md) object\.
+ `name` – An optional name string, empty by default\.

## fromDF<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-fromDF"></a>

**`fromDF(dataframe, glue_ctx, name)`**

Converts a `DataFrame` to a `DynamicFrame` by converting `DataFrame` fields to `DynamicRecord` fields\. Returns the new `DynamicFrame`\.

A `DynamicRecord` represents a logical record in a `DynamicFrame`\. It is similar to a row in a Spark `DataFrame`, except that it is self\-describing and can be used for data that does not conform to a fixed schema\.
+ `dataframe` – The Apache Spark SQL `DataFrame` to convert \(required\)\.
+ `glue_ctx` – The [GlueContext Class](aws-glue-api-crawler-pyspark-extensions-glue-context.md) object that specifies the context for this transform \(required\)\.
+ `name` – The name of the resulting `DynamicFrame` \(required\)\.

## toDF<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-toDF"></a>

**`toDF(options)`**

Converts a `DynamicFrame` to an Apache Spark `DataFrame` by converting `DynamicRecords` into `DataFrame` fields\. Returns the new `DataFrame`\.

A `DynamicRecord` represents a logical record in a `DynamicFrame`\. It is similar to a row in a Spark `DataFrame`, except that it is self\-describing and can be used for data that does not conform to a fixed schema\.
+ `options` – A list of options\. Specify the target type if you choose the `Project` and `Cast` action type\. Examples include the following:

  ```
  >>>toDF([ResolveOption("a.b.c", "KeepAsStruct")])
  >>>toDF([ResolveOption("a.b.c", "Project", DoubleType())])
  ```

##  — Information —<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-_informational"></a>
+ [count](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-count)
+ [schema](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-schema)
+ [printSchema](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-printSchema)
+ [show](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-show)
+ [repartition](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-repartition)
+ [coalesce](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-coalesce)

## count<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-count"></a>

`count( )` – Returns the number of rows in the underlying `DataFrame`\.

## schema<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-schema"></a>

`schema( )` – Returns the schema of this `DynamicFrame`, or if that is not available, the schema of the underlying `DataFrame`\.

## printSchema<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-printSchema"></a>

`printSchema( )` – Prints the schema of the underlying `DataFrame`\.

## show<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-show"></a>

`show(num_rows)` – Prints a specified number of rows from the underlying `DataFrame`\.

## repartition<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-repartition"></a>

`repartition(numPartitions)` – Returns a new DynamicFrame with `numPartitions` partitions\.

## coalesce<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-coalesce"></a>

`coalesce(numPartitions)` – Returns a new DynamicFrame with `numPartitions` partitions\.

##  — Transforms —<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-_transforms"></a>
+ [apply\_mapping](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-apply_mapping)
+ [drop\_fields](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-drop_fields)
+ [filter](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-filter)
+ [join](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-join)
+ [map](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-map)
+ [relationalize](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-relationalize)
+ [rename\_field](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-rename_field)
+ [resolveChoice](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-resolveChoice)
+ [select\_fields](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-select_fields)
+ [spigot](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-spigot)
+ [split\_fields](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-split_fields)
+ [split\_rows](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-split_rows)
+ [unbox](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-unbox)
+ [unnest](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-unnest)
+ [write](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-write)

## apply\_mapping<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-apply_mapping"></a>

**`apply_mapping(mappings, transformation_ctx="", info="", stageThreshold=0, totalThreshold=0)`**

Applies a declarative mapping to this `DynamicFrame` and returns a new `DynamicFrame` with those mappings applied\.
+ `mappings` – A list of mapping tuples, each consisting of: \(source column, source type, target column, target type\)\. Required\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string to be associated with error reporting for this transformation \(optional\)\.
+ `stageThreshold` – The number of errors encountered during this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.
+ `totalThreshold` – The number of errors encountered up to and including this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.

## drop\_fields<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-drop_fields"></a>

**`drop_fields(paths, transformation_ctx="", info="", stageThreshold=0, totalThreshold=0)`**

Calls the [FlatMap Class](aws-glue-api-crawler-pyspark-transforms-flat-map.md) transform to remove fields from a `DynamicFrame`\. Returns a new `DynamicFrame` with the specified fields dropped\.
+ `paths` – A list of strings, each containing the full path to a field node you want to drop\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string to be associated with error reporting for this transformation \(optional\)\.
+ `stageThreshold` – The number of errors encountered during this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.
+ `totalThreshold` – The number of errors encountered up to and including this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.

## filter<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-filter"></a>

**`filter(f, transformation_ctx="", info="", stageThreshold=0, totalThreshold=0)`**

Returns a new `DynamicFrame` built by selecting all `DynamicRecords` within the input `DynamicFrame` that satisfy the specified predicate function `f`\.
+ `f` – The predicate function to apply to the `DynamicFrame`\. The function must take a `DynamicRecord` as an argument and return True if the `DynamicRecord` meets the filter requirements, or False if not \(required\)\.

  A `DynamicRecord` represents a logical record in a `DynamicFrame`\. It is similar to a row in a Spark `DataFrame`, except that it is self\-describing and can be used for data that does not conform to a fixed schema\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string to be associated with error reporting for this transformation \(optional\)\.
+ `stageThreshold` – The number of errors encountered during this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.
+ `totalThreshold` – The number of errors encountered up to and including this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.

For an example of how to use the `filter` transform, see [Filter Class](aws-glue-api-crawler-pyspark-transforms-filter.md)\.

## join<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-join"></a>

**`join(paths1, paths2, frame2, transformation_ctx="", info="", stageThreshold=0, totalThreshold=0)`**

Performs an equality join with another `DynamicFrame` and returns the resulting `DynamicFrame`\.
+ `paths1` – A list of the keys in this frame to join\.
+ `paths2` – A list of the keys in the other frame to join\.
+ `frame2` – The other `DynamicFrame` to join\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string to be associated with error reporting for this transformation \(optional\)\.
+ `stageThreshold` – The number of errors encountered during this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.
+ `totalThreshold` – The number of errors encountered up to and including this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.

## map<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-map"></a>

**`map(f, transformation_ctx="", info="", stageThreshold=0, totalThreshold=0)`**

Returns a new `DynamicFrame` that results from applying the specified mapping function to all records in the original `DynamicFrame`\.
+ `f` – The mapping function to apply to all records in the `DynamicFrame`\. The function must take a `DynamicRecord` as an argument and return a new `DynamicRecord` \(required\)\.

  A `DynamicRecord` represents a logical record in a `DynamicFrame`\. It is similar to a row in an Apache Spark `DataFrame`, except that it is self\-describing and can be used for data that does not conform to a fixed schema\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string associated with errors in the transformation \(optional\)\.
+ `stageThreshold` – The maximum number of errors that can occur in the transformation before it errors out \(optional; the default is zero\)\.
+ `totalThreshold` – The maximum number of errors that can occur overall before processing errors out \(optional; the default is zero\)\.

For an example of how to use the `map` transform, see [Map Class](aws-glue-api-crawler-pyspark-transforms-map.md)\.

## relationalize<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-relationalize"></a>

**`relationalize(root_table_name, staging_path, options, transformation_ctx="", info="", stageThreshold=0, totalThreshold=0)`**

Relationalizes a `DynamicFrame` by producing a list of frames that are generated by unnesting nested columns and pivoting array columns\. The pivoted array column can be joined to the root table using the joinkey generated during the unnest phase\.
+ `root_table_name` – The name for the root table\.
+ `staging_path` – The path at which to store partitions of pivoted tables in CSV format \(optional\)\. Pivoted tables are read back from this path\.
+ `options` – A dictionary of optional parameters\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string to be associated with error reporting for this transformation \(optional\)\.
+ `stageThreshold` – The number of errors encountered during this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.
+ `totalThreshold` – The number of errors encountered up to and including this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.

## rename\_field<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-rename_field"></a>

**`rename_field(oldName, newName, transformation_ctx="", info="", stageThreshold=0, totalThreshold=0)`**

Renames a field in this `DynamicFrame` and returns a new `DynamicFrame` with the field renamed\.
+ `oldName` – The full path to the node you want to rename\.

  If the old name has dots in it, RenameField will not work unless you place back\-ticks around it \(```\)\. For example, to replace `this.old.name` with `thisNewName`, you would call rename\_field as follows:

  ```
  newDyF = oldDyF.rename_field("`this.old.name`", "thisNewName")
  ```
+ `newName` – The new name, as a full path\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string to be associated with error reporting for this transformation \(optional\)\.
+ `stageThreshold` – The number of errors encountered during this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.
+ `totalThreshold` – The number of errors encountered up to and including this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.

## resolveChoice<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-resolveChoice"></a>

**`resolveChoice(specs = None, option="", transformation_ctx="", info="", stageThreshold=0, totalThreshold=0)`**

Resolves a choice type within this `DynamicFrame` and returns the new `DynamicFrame`\.
+ `specs` – A list of specific ambiguities to resolve, each in the form of a tuple: `(path, action)`\. The `path` value identifies a specific ambiguous element, and the `action` value identifies the corresponding resolution\. Only one of the `specs` and `option` parameters can be used\. If the `spec` parameter is not `None`, then the `option` parameter must be an empty string\. Conversely if the `option` is not an empty string, then the `spec` parameter must be `None`\. If neither parameter is provided, AWS Glue tries to parse the schema and use it to resolve ambiguities\. 

  The `action` portion of a `specs` tuple can specify one of four resolution strategies:
  + `cast`:   Allows you to specify a type to cast to \(for example, `cast:int`\)\.
  + `make_cols`:   Resolves a potential ambiguity by flattening the data\. For example, if `columnA` could be an `int` or a `string`, the resolution would be to produce two columns named `columnA_int` and `columnA_string` in the resulting `DynamicFrame`\.
  + `make_struct`:   Resolves a potential ambiguity by using a struct to represent the data\. For example, if data in a column could be an `int` or a `string`, using the `make_struct` action produces a column of structures in the resulting `DynamicFrame` that each contains both an `int` and a `string`\.
  + `project`:   Resolves a potential ambiguity by projecting all the data to one of the possible data types\. For example, if data in a column could be an `int` or a `string`, using a `project:string` action produces a column in the resulting `DynamicFrame` where all the `int` values have been converted to strings\.

  If the `path` identifies an array, place empty square brackets after the name of the array to avoid ambiguity\. For example, suppose you are working with data structured as follows:

  ```
  "myList": [
    { "price": 100.00 },
    { "price": "$100.00" }
  ]
  ```

  You can select the numeric rather than the string version of the price by setting the `path` to `"myList[].price"`, and the `action` to `"cast:double"`\.
+ `option` – The default resolution action if the `specs` parameter is `None`\. If the `specs` parameter is not `None`, then this must not be set to anything but an empty string\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string to be associated with error reporting for this transformation \(optional\)\.
+ `stageThreshold` – The number of errors encountered during this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.
+ `totalThreshold` – The number of errors encountered up to and including this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.

**Example**  

```
df1 = df.resolveChoice(option = "make_cols")
df2 = df.resolveChoice(specs = [("a.b", "make_struct"), ("c.d", "cast:double")])
```

## select\_fields<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-select_fields"></a>

**`select_fields(paths, transformation_ctx="", info="", stageThreshold=0, totalThreshold=0)`**

Returns a new `DynamicFrame` containing the selected fields\.
+ `paths` – A list of strings, each of which is a path to a top\-level node that you want to select\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string to be associated with error reporting for this transformation \(optional\)\.
+ `stageThreshold` – The number of errors encountered during this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.
+ `totalThreshold` – The number of errors encountered up to and including this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.

## spigot<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-spigot"></a>

**`spigot(path, options={})`**

Writes sample records to a specified destination during a transformation, and returns the input `DynamicFrame` with an additional write step\.
+ `path` – The path to the destination to which to write \(required\)\.
+ `options` – Key\-value pairs specifying options \(optional\)\. The `"topk"` option specifies that the first `k` records should be written\. The `"prob"` option specifies the probability \(as a decimal\) of picking any given record, to be used in selecting records to write\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.

## split\_fields<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-split_fields"></a>

**`split_fields(paths, name1, name2, transformation_ctx="", info="", stageThreshold=0, totalThreshold=0)`**

Returns a new `DynamicFrameCollection` that contains two `DynamicFrames`: the first containing all the nodes that have been split off, and the second containing the nodes that remain\.
+ `paths` – A list of strings, each of which is a full path to a node that you want to split into a new `DynamicFrame`\.
+ `name1` – A name string for the `DynamicFrame` that is split off\.
+ `name2` – A name string for the `DynamicFrame` that remains after the specified nodes have been split off\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string to be associated with error reporting for this transformation \(optional\)\.
+ `stageThreshold` – The number of errors encountered during this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.
+ `totalThreshold` – The number of errors encountered up to and including this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.

## split\_rows<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-split_rows"></a>

Splits one or more rows in a `DynamicFrame` off into a new `DynamicFrame`\.

**`split_rows(comparison_dict, name1, name2, transformation_ctx="", info="", stageThreshold=0, totalThreshold=0)`**

Returns a new `DynamicFrameCollection` containing two `DynamicFrames`: the first containing all the rows that have been split off and the second containing the rows that remain\.
+ `comparison_dict` – A dictionary in which the key is a path to a column and the value is another dictionary for mapping comparators to values to which the column value are compared\. For example, `{"age": {">": 10, "<": 20}}` splits off all rows whose value in the age column is greater than 10 and less than 20\.
+ `name1` – A name string for the `DynamicFrame` that is split off\.
+ `name2` – A name string for the `DynamicFrame` that remains after the specified nodes have been split off\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string to be associated with error reporting for this transformation \(optional\)\.
+ `stageThreshold` – The number of errors encountered during this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.
+ `totalThreshold` – The number of errors encountered up to and including this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.

## unbox<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-unbox"></a>

**`unbox(path, format, transformation_ctx="", info="", stageThreshold=0, totalThreshold=0, **options)`**

Unboxes a string field in a `DynamicFrame` and returns a new `DynamicFrame` containing the unboxed `DynamicRecords`\.

A `DynamicRecord` represents a logical record in a `DynamicFrame`\. It is similar to a row in an Apache Spark `DataFrame`, except that it is self\-describing and can be used for data that does not conform to a fixed schema\.
+ `path` – A full path to the string node you want to unbox\.
+ `format` – A format specification \(optional\)\. This is used for an Amazon Simple Storage Service \(Amazon S3\) or an AWS Glue connection that supports multiple formats\. See [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md) for the formats that are supported\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string to be associated with error reporting for this transformation \(optional\)\.
+ `stageThreshold` – The number of errors encountered during this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.
+ `totalThreshold` – The number of errors encountered up to and including this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.
+ `options` – One or more of the following:
  + `separator` – A string containing the separator character\.
  + `escaper` – A string containing the escape character\.
  + `skipFirst` – A Boolean value indicating whether to skip the first instance\.
  + `withSchema` – A string containing the schema; must be called using `StructType.json( )`\.
  + `withHeader` – A Boolean value indicating whether a header is included\.

For example: `unbox("a.b.c", "csv", separator="|")`

## unnest<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-unnest"></a>

Unnests nested objects in a `DynamicFrame`, making them top\-level objects, and returns a new unnested `DynamicFrame`\.

**`unnest(transformation_ctx="", info="", stageThreshold=0, totalThreshold=0)`**

Unnests nested objects in a `DynamicFrame`, making them top\-level objects, and returns a new unnested `DynamicFrame`\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string to be associated with error reporting for this transformation \(optional\)\.
+ `stageThreshold` – The number of errors encountered during this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.
+ `totalThreshold` – The number of errors encountered up to and including this transformation at which the process should error out \(optional: zero by default, indicating that the process should not error out\)\.

For example: `unnest( )`

## write<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-write"></a>

**`write(connection_type, connection_options, format, format_options, accumulator_size)`**

Gets a [DataSink\(object\)](aws-glue-api-crawler-pyspark-extensions-types.md#aws-glue-api-crawler-pyspark-extensions-types-awsglue-data-sink) of the specified connection type from the [GlueContext Class](aws-glue-api-crawler-pyspark-extensions-glue-context.md) of this `DynamicFrame`, and uses it to format and write the contents of this `DynamicFrame`\. Returns the new `DynamicFrame` formatted and written as specified\.
+ `connection_type` – The connection type to use\. Valid values include `s3`, `mysql`, `postgresql`, `redshift`, `sqlserver`, and `oracle`\.
+ `connection_options` – The connection option to use \(optional\)\. For a `connection_type` of `s3`, an Amazon S3 path is defined\.

  ```
  connection_options = {"path": "s3://aws-glue-target/temp"}
  ```

  For JDBC connections, several properties must be defined\. Note that the database name must be part of the URL\. It can optionally be included in the connection options\.

  ```
  connection_options = {"url": "jdbc-url/database", "user": "username", "password": "password","dbtable": "table-name", "redshiftTmpDir": "s3-tempdir-path"} 
  ```
+ `format` – A format specification \(optional\)\. This is used for an Amazon Simple Storage Service \(Amazon S3\) or an AWS Glue connection that supports multiple formats\. See [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md) for the formats that are supported\.
+ `format_options` – Format options for the specified format\. See [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md) for the formats that are supported\.
+ `accumulator_size` – The accumulable size to use \(optional\)\.

##  — Errors —<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-_errors"></a>
+ [assertErrorThreshold](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-assertErrorThreshold)
+ [errorsAsDynamicFrame](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-errorsAsDynamicFrame)
+ [errorsCount](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-errorsCount)
+ [stageErrorsCount](#aws-glue-api-crawler-pyspark-extensions-dynamic-frame-stageErrorsCount)

## assertErrorThreshold<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-assertErrorThreshold"></a>

`assertErrorThreshold( )` – An assert for errors in the transformations that created this `DynamicFrame`\. Returns an `Exception` from the underlying `DataFrame`\.

## errorsAsDynamicFrame<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-errorsAsDynamicFrame"></a>

`errorsAsDynamicFrame( )` – Returns a `DynamicFrame` that has error records nested inside\.

## errorsCount<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-errorsCount"></a>

`errorsCount( )` – Returns the total number of errors in a `DynamicFrame`\.

## stageErrorsCount<a name="aws-glue-api-crawler-pyspark-extensions-dynamic-frame-stageErrorsCount"></a>

`stageErrorsCount` – Returns the number of errors that occurred in the process of generating this `DynamicFrame`\.