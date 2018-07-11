# AWS Glue Scala DynamicFrame Class<a name="glue-etl-scala-apis-glue-dynamicframe-class"></a>

**Package: com\.amazonaws\.services\.glue**

```
class DynamicFrame extends Serializable with Logging  (
           val glueContext : GlueContext,
           _records : RDD[DynamicRecord],
           val name : String = s"",
           val transformationContext : String = DynamicFrame.UNDEFINED,
           callSite : CallSite = CallSite("Not provided", ""),
           stageThreshold : Long = 0,
           totalThreshold : Long = 0,
           prevErrors : => Long = 0,
           errorExpr : => Unit = {} )
```

A `DynamicFrame` is a distributed collection of self\-describing [DynamicRecord](glue-etl-scala-apis-glue-dynamicrecord-class.md) objects\.

`DynamicFrame`s are designed to provide a flexible data model for ETL \(extract, transform, and load\) operations\. They don't require a schema to create, and you can use them to read and transform data that contains messy or inconsistent values and types\. A schema can be computed on demand for those operations that need one\.

`DynamicFrame`s provide a range of transformations for data cleaning and ETL\. They also support conversion to and from SparkSQL DataFrames to integrate with existing code and the many analytics operations that DataFrames provide\.

The following parameters are shared across many of the AWS Glue transformations that construct `DynamicFrame`s:
+ `transformationContext` — The identifier for this `DynamicFrame`\. The `transformationContext` is used as a key for job bookmark state that is persisted across runs\.
+ `callSite` — Provides context information for error reporting\. These values are automatically set when calling from Python\.
+ `stageThreshold` — The maximum number of error records that are allowed from the computation of this `DynamicFrame` before throwing an exception, excluding records that are present in the previous `DynamicFrame`\.
+ `totalThreshold` — The maximum number of total error records before an exception is thrown, including those from previous frames\.

## val errorsCount<a name="glue-etl-scala-apis-glue-dynamicframe-class-vals-errorsCount"></a>

```
val errorsCount
```

The number of error records in this `DynamicFrame`\. This includes errors from previous operations\.

## def applyMapping<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-applyMapping"></a>

```
def applyMapping( mappings : Seq[Product4[String, String, String, String]],
                  caseSensitive : Boolean = true,
                  transformationContext : String = "",
                  callSite : CallSite = CallSite("Not provided", ""),
                  stageThreshold : Long = 0,
                  totalThreshold : Long = 0
                ) : DynamicFrame
```
+ `mappings` — A sequence of mappings to construct a new `DynamicFrame`\.
+ `caseSensitive` — Whether to treat source columns as case sensitive\. Setting this to false might help when integrating with case\-insensitive stores like the AWS Glue Data Catalog\.

Selects, projects, and casts columns based on a sequence of mappings\.

Each mapping is made up of a source column and type and a target column and type\. Mappings can be specified as either a four\-tuple \(`source_path`, `source_type`,` target_path`, `target_type`\) or a [MappingSpec](glue-etl-scala-apis.md#glue-etl-scala-apis-glue-mappingspec) object containing the same information\.

In addition to using mappings for simple projections and casting, you can use them to nest or unnest fields by separating components of the path with '`.`' \(period\)\. 

For example, suppose that you have a `DynamicFrame` with the following schema:

```
 {{{
   root
   |-- name: string
   |-- age: int
   |-- address: struct
   |    |-- state: string
   |    |-- zip: int
 }}}
```

You can make the following call to unnest the `state` and `zip` fields:

```
 {{{
   df.applyMapping(
    Seq(("name", "string", "name", "string"),
        ("age", "int", "age", "int"),
        ("address.state", "string", "state", "string"),
        ("address.zip", "int", "zip", "int")))
 }}}
```

The resulting schema is as follows:

```
 {{{
   root
   |-- name: string
   |-- age: int
   |-- state: string
   |-- zip: int
 }}}
```

You can also use `applyMapping` to re\-nest columns\. For example, the following inverts the previous transformation and creates a struct named `address` in the target:

```
 {{{
   df.applyMapping(
    Seq(("name", "string", "name", "string"),
        ("age", "int", "age", "int"),
        ("state", "string", "address.state", "string"),
        ("zip", "int", "address.zip", "int")))
 }}}
```

Field names that contain '`.`' \(period\) characters can be quoted by using backticks \(`''`\)\.

**Note**  
Currently, you can't use the `applyMapping` method to map columns that are nested under arrays\.

## def assertErrorThreshold<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-assertErrorThreshold"></a>

```
def assertErrorThreshold : Unit
```

An action that forces computation and verifies that the number of error records falls below `stageThreshold` and `totalThreshold`\. Throws an exception if either condition fails\.

## def count<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-count"></a>

```
lazy
def count
```

Returns the number of elements in this `DynamicFrame`\.

## def dropField<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-dropField"></a>

```
def dropField( path : String,
               transformationContext : String = "",
               callSite : CallSite = CallSite("Not provided", ""),
               stageThreshold : Long = 0,
               totalThreshold : Long = 0
             ) : DynamicFrame
```

Returns a new `DynamicFrame` with the specified column removed\.

## def dropFields<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-dropFields"></a>

```
def dropFields( fieldNames : Seq[String],   // The column names to drop.
                transformationContext : String = "",
                callSite : CallSite = CallSite("Not provided", ""),
                stageThreshold : Long = 0,
                totalThreshold : Long = 0
              ) : DynamicFrame
```

Returns a new `DynamicFrame` with the specified columns removed\.

You can use this method to delete nested columns, including those inside of arrays, but not to drop specific array elements\.

## def dropNulls<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-dropNulls"></a>

```
def dropNulls( transformationContext : String = "",
               callSite : CallSite = CallSite("Not provided", ""),
               stageThreshold : Long = 0,
               totalThreshold : Long = 0 )
```

Returns a new `DynamicFrame` with all null columns removed\.

**Note**  
This only removes columns of type `NullType`\. Individual null values in other columns are not removed or modified\.

## def errorsAsDynamicFrame<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-errorsAsDynamicFrame"></a>

```
def errorsAsDynamicFrame
```

Returns a new `DynamicFrame` containing the error records from this `DynamicFrame`\.

## def filter<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-filter"></a>

```
def filter( f : DynamicRecord => Boolean,
            errorMsg : String = "",
            transformationContext : String = "",
            callSite : CallSite = CallSite("Not provided"),
            stageThreshold : Long = 0,
            totalThreshold : Long = 0
          ) : DynamicFrame
```

Constructs a new `DynamicFrame` containing only those records for which the function '`f`' returns `true`\. The filter function '`f`' should not mutate the input record\.

## def getName<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-getName"></a>

```
def getName : String 
```

Returns the name of this `DynamicFrame`\.

## def getNumPartitions<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-getNumPartitions"></a>

```
def getNumPartitions
```

Returns the number of partitions in this `DynamicFrame`\.

## def getSchemaIfComputed<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-getSchemaIfComputed"></a>

```
def getSchemaIfComputed : Option[Schema] 
```

Returns the schema if it has already been computed\. Does not scan the data if the schema has not already been computed\.

## def isSchemaComputed<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-isSchemaComputed"></a>

```
def isSchemaComputed : Boolean 
```

Returns `true` if the schema has been computed for this `DynamicFrame`, or `false` if not\. If this method returns false, then calling the `schema` method requires another pass over the records in this `DynamicFrame`\.

## def javaToPython<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-javaToPython"></a>

```
def javaToPython : JavaRDD[Array[Byte]] 
```

## def join<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-join"></a>

```
def join( keys1 : Seq[String],
          keys2 : Seq[String],
          frame2 : DynamicFrame,
          transformationContext : String = "",
          callSite : CallSite = CallSite("Not provided", ""),
          stageThreshold : Long = 0,
          totalThreshold : Long = 0
        ) : DynamicFrame
```
+ `keys1` — The columns in this `DynamicFrame` to use for the join\.
+ `keys2` — The columns in `frame2` to use for the join\. Must be the same length as `keys1`\.
+ `frame2` — The `DynamicFrame` to join against\.

Returns the result of performing an equijoin with `frame2` using the specified keys\.

## def map<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-map"></a>

```
def map( f : DynamicRecord => DynamicRecord,
         errorMsg : String = "",
         transformationContext : String = "",
         callSite : CallSite = CallSite("Not provided", ""),
         stageThreshold : Long = 0,
         totalThreshold : Long = 0
       ) : DynamicFrame
```

Returns a new `DynamicFrame` constructed by applying the specified function '`f`' to each record in this `DynamicFrame`\.

This method copies each record before applying the specified function, so it is safe to mutate the records\. If the mapping function throws an exception on a given record, that record is marked as an error, and the stack trace is saved as a column in the error record\.

## def printSchema<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-printSchema"></a>

```
def printSchema : Unit 
```

Prints the schema of this `DynamicFrame` to `stdout` in a human\-readable format\.

## def recomputeSchema<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-recomputeSchema"></a>

```
def recomputeSchema : Schema 
```

Forces a schema recomputation\. This requires a scan over the data, but it may "tighten" the schema if there are some fields in the current schema that are not present in the data\.

Returns the recomputed schema\.

## def relationalize<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-relationalize"></a>

```
def relationalize( rootTableName : String,
                   stagingPath : String,
                   options : JsonOptions = JsonOptions.empty,
                   transformationContext : String = "",
                   callSite : CallSite = CallSite("Not provided"),
                   stageThreshold : Long = 0,
                   totalThreshold : Long = 0
                 ) : Seq[DynamicFrame]
```
+ `rootTableName` — The name to use for the base `DynamicFrame` in the output\. `DynamicFrame`s that are created by pivoting arrays start with this as a prefix\.
+ `stagingPath` — The Amazon Simple Storage Service \(Amazon S3\) path for writing intermediate data\.
+ `options` — Relationalize options and configuration\. Currently unused\.

Flattens all nested structures and pivots arrays into separate tables\.

You can use this operation to prepare deeply nested data for ingestion into a relational database\. Nested structs are flattened in the same manner as the [unnest](#glue-etl-scala-apis-glue-dynamicframe-class-defs-unnest) transform\. Additionally, arrays are pivoted into separate tables with each array element becoming a row\. For example, suppose that you have a `DynamicFrame` with the following data:

```
 {"name": "Nancy", "age": 47, "friends": ["Fred", "Lakshmi"]}
 {"name": "Stephanie", "age": 28, "friends": ["Yao", "Phil", "Alvin"]}
 {"name": "Nathan", "age": 54, "friends": ["Nicolai", "Karen"]}
```

Execute the following code:

```
{{{
  df.relationalize("people", "s3:/my_bucket/my_path", JsonOptions.empty)
}}}
```

This produces two tables\. The first table is named "people" and contains the following:

```
{{{
  {"name": "Nancy", "age": 47, "friends": 1}
  {"name": "Stephanie", "age": 28, "friends": 2}
  {"name": "Nathan", "age": 54, "friends": 3)
}}}
```

Here, the friends array has been replaced with an auto\-generated join key\. A separate table named `people.friends` is created with the following content:

```
{{{
  {"id": 1, "index": 0, "val": "Fred"}
  {"id": 1, "index": 1, "val": "Lakshmi"}
  {"id": 2, "index": 0, "val": "Yao"}
  {"id": 2, "index": 1, "val": "Phil"}
  {"id": 2, "index": 2, "val": "Alvin"}
  {"id": 3, "index": 0, "val": "Nicolai"}
  {"id": 3, "index": 1, "val": "Karen"}
}}}
```

In this table, '`id`' is a join key that identifies which record the array element came from, '`index`' refers to the position in the original array, and '`val`' is the actual array entry\.

The `relationalize` method returns the sequence of `DynamicFrame`s created by applying this process recursively to all arrays\.

**Note**  
The AWS Glue library automatically generates join keys for new tables\. To ensure that join keys are unique across job runs, you must enable job bookmarks\.

## def renameField<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-renameField"></a>

```
def renameField( oldName : String,
                 newName : String,
                 transformationContext : String = "",
                 callSite : CallSite = CallSite("Not provided", ""),
                 stageThreshold : Long = 0,
                 totalThreshold : Long = 0
               ) : DynamicFrame
```
+ `oldName` — The original name of the column\.
+ `newName` — The new name of the column\.

Returns a new `DynamicFrame` with the specified field renamed\.

You can use this method to rename nested fields\. For example, the following code would rename `state` to `state_code` inside the address struct:

```
{{{
  df.renameField("address.state", "address.state_code")
}}}
```

## def repartition<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-repartition"></a>

```
def repartition( numPartitions : Int,
                 transformationContext : String = "",
                 callSite : CallSite = CallSite("Not provided", ""),
                 stageThreshold : Long = 0,
                 totalThreshold : Long = 0
               ) : DynamicFrame
```

Returns a new `DynamicFrame` with `numPartitions` partitions\.

## def resolveChoice<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-resolveChoice"></a>

```
def resolveChoice( specs : Seq[Product2[String, String]] = Seq.empty[ResolveSpec],
                   choiceOption : Option[ChoiceOption] = None,
                   database : Option[String] = None,
                   tableName : Option[String] = None,
                   transformationContext : String = "",
                   callSite : CallSite = CallSite("Not provided", ""),
                   stageThreshold : Long = 0,
                   totalThreshold : Long = 0
                 ) : DynamicFrame
```
+ `choiceOption` — An action to apply to all `ChoiceType` columns not listed in the specs sequence\.
+ `database` — The Data Catalog database to use with the `match_catalog` action\.
+ `tableName` — The Data Catalog table to use with the `match_catalog` action\.

Returns a new `DynamicFrame` by replacing one or more `ChoiceType`s with a more specific type\.

There are two ways to use `resolveChoice`\. The first is to specify a sequence of specific columns and how to resolve them\. These are specified as tuples made up of \(column, action\) pairs\.

The following are the possible actions:
+ `cast:type` — Attempts to cast all values to the specified type\.
+ `make_cols` — Converts each distinct type to a column with the name `columnName_type`\.
+ `make_struct` — Converts a column to a struct with keys for each distinct type\.
+ `project:type` — Retains only values of the specified type\.

The other mode for `resolveChoice` is to specify a single resolution for all `ChoiceType`s\. You can use this in cases where the complete list of `ChoiceType`s is unknown before execution\. In addition to the actions listed preceding, this mode also supports the following action:
+ `match_catalog` — Attempts to cast each `ChoiceType` to the corresponding type in the specified catalog table\.

**Examples:**

Resolve the `user.id` column by casting to an int, and make the `address` field retain only structs:

```
{{{
  df.resolveChoice(specs = Seq(("user.id", "cast:int"), ("address", "project:struct")))
}}}
```

Resolve all `ChoiceType`s by converting each choice to a separate column:

```
{{{
  df.resolveChoice(choiceOption = Some(ChoiceOption("make_cols")))
}}}
```

Resolve all `ChoiceType`s by casting to the types in the specified catalog table:

```
{{{
  df.resolveChoice(choiceOption = Some(ChoiceOption("match_catalog")),
                   database = Some("my_database"),
                   tableName = Some("my_table"))
}}}
```

## def schema<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-schema"></a>

```
def schema : Schema 
```

Returns the schema of this `DynamicFrame`\.

The returned schema is guaranteed to contain every field that is present in a record in this `DynamicFrame`\. But in a small number of cases, it might also contain additional fields\. You can use the [unnest](#glue-etl-scala-apis-glue-dynamicframe-class-defs-unnest) method to "tighten" the schema based on the records in this `DynamicFrame`\.

## def selectField<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-selectField"></a>

```
def selectField( fieldName : String,
                 transformationContext : String = "",
                 callSite : CallSite = CallSite("Not provided", ""),
                 stageThreshold : Long = 0,
                 totalThreshold : Long = 0
               ) : DynamicFrame
```

Returns a single field as a `DynamicFrame`\.

## def selectFields<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-selectFields"></a>

```
def selectFields( paths : Seq[String],
                  transformationContext : String = "",
                  callSite : CallSite = CallSite("Not provided", ""),
                  stageThreshold : Long = 0,
                  totalThreshold : Long = 0
                ) : DynamicFrame
```
+ `paths` — The sequence of column names to select\.

Returns a new `DynamicFrame` containing the specified columns\.

**Note**  
You can only use the `selectFields` method to select top\-level columns\. You can use the [applyMapping](#glue-etl-scala-apis-glue-dynamicframe-class-defs-applyMapping) method to select nested columns\.

## def show<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-show"></a>

```
def show( numRows : Int = 20 ) : Unit 
```
+ `numRows` — The number of rows to print\.

Prints rows from this `DynamicFrame` in JSON format\.

## def spigot<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-spigot"></a>

```
def spigot( path : String,
            options : JsonOptions = new JsonOptions("{}"),
            transformationContext : String = "",
            callSite : CallSite = CallSite("Not provided"),
            stageThreshold : Long = 0,
            totalThreshold : Long = 0
          ) : DynamicFrame
```

Passthrough transformation that returns the same records but writes out a subset of records as a side effect\.
+ `path` — The path in Amazon S3 to write output to, in the form `s3://bucket//path`\.
+ `options`  — An optional `JsonOptions` map describing the sampling behavior\.

Returns a `DynamicFrame` that contains the same records as this one\.

By default, writes 100 arbitrary records to the location specified by `path`\. You can customize this behavior by using the `options` map\. Valid keys include the following:
+ `topk` — Specifies the total number of records written out\. The default is 100\.
+ `prob` — Specifies the probability \(as a decimal\) that an individual record is included\. Default is 1\.

For example, the following call would sample the dataset by selecting each record with a 20 percent probability and stopping after 200 records have been written:

```
{{{
  df.spigot("s3://my_bucket/my_path", JsonOptions(Map("topk" -&gt; 200, "prob" -&gt; 0.2)))
}}}
```

## def splitFields<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-splitFields"></a>

```
def splitFields( paths : Seq[String],
                 transformationContext : String = "",
                 callSite : CallSite = CallSite("Not provided", ""),
                 stageThreshold : Long = 0,
                 totalThreshold : Long = 0
               ) : Seq[DynamicFrame]
```
+ `paths` — The paths to include in the first `DynamicFrame`\.

Returns a sequence of two `DynamicFrame`s\. The first `DynamicFrame` contains the specified paths, and the second contains all other columns\.

## def splitRows<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-splitRows"></a>

```
def splitRows( paths : Seq[String],
               values : Seq[Any],
               operators : Seq[String],
               transformationContext : String,
               callSite : CallSite,
               stageThreshold : Long,
               totalThreshold : Long
             ) : Seq[DynamicFrame]
```

Splits rows based on predicates that compare columns to constants\.
+ `paths` — The columns to use for comparison\.
+ `values` — The constant values to use for comparison\.
+ `operators` — The operators to use for comparison\.

Returns a sequence of two `DynamicFrame`s\. The first contains rows for which the predicate is true and the second contains those for which it is false\.

Predicates are specified using three sequences: '`paths`' contains the \(possibly nested\) column names, '`values`' contains the constant values to compare to, and '`operators`' contains the operators to use for comparison\. All three sequences must be the same length: The `n`th operator is used to compare the `n`th column with the `n`th value\.

Each operator must be one of "`!=`", "`=`", "`&lt;=`", "`&lt;`", "`&gt;=`", or "`&gt;`"\.

As an example, the following call would split a `DynamicFrame` so that the first output frame would contain records of people over 65 from the United States, and the second would contain all other records:

```
{{{
  df.splitRows(Seq("age", "address.country"), Seq(65, "USA"), Seq("&gt;=", "="))
}}}
```

## def stageErrorsCount<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-stageErrorsCount"></a>

```
def stageErrorsCount
```

Returns the number of error records created while computing this `DynamicFrame`\. This excludes errors from previous operations that were passed into this `DynamicFrame` as input\.

## def toDF<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-toDF"></a>

```
def toDF( specs : Seq[ResolveSpec] = Seq.empty[ResolveSpec] ) : DataFrame 
```

Converts this `DynamicFrame` to an Apache Spark SQL `DataFrame` with the same schema and records\.

**Note**  
Because `DataFrame`s don't support `ChoiceType`s, this method automatically converts `ChoiceType` columns into `StructType`s\. For more information and options for resolving choice, see [resolveChoice](#glue-etl-scala-apis-glue-dynamicframe-class-defs-resolveChoice)\.

## def unbox<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-unbox"></a>

```
def unbox( path : String,
           format : String,
           optionString : String = "{}",
           transformationContext : String = "",
           callSite : CallSite = CallSite("Not provided"),
           stageThreshold : Long = 0,
           totalThreshold : Long = 0
         ) : DynamicFrame
```
+ `path` — The column to parse\. Must be a string or binary\.
+ `format` — The format to use for parsing\.
+ `optionString` — Options to pass to the format, such as the CSV separator\.

Parses an embedded string or binary column according to the specified format\. Parsed columns are nested under a struct with the original column name\.

For example, suppose that you have a CSV file with an embedded JSON column:

```
name, age, address
Sally, 36, {"state": "NE", "city": "Omaha"}
...
```

After an initial parse, you would get a `DynamicFrame` with the following schema:

```
{{{
  root
  |-- name: string
  |-- age: int
  |-- address: string
}}}
```

You can call `unbox` on the address column to parse the specific components:

```
{{{
  df.unbox("address", "json")
}}}
```

This gives us a `DynamicFrame` with the following schema:

```
{{{
  root
  |-- name: string
  |-- age: int
  |-- address: struct
  |    |-- state: string
  |    |-- city: string
}}}
```

## def unnest<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-unnest"></a>

```
def unnest( transformationContext : String = "",
            callSite : CallSite = CallSite("Not Provided"),
            stageThreshold : Long = 0,
            totalThreshold : Long = 0
          ) : DynamicFrame
```

Returns a new `DynamicFrame` with all nested structures flattened\. Names are constructed using the '`.`' \(period\) character\.

For example, suppose that you have a `DynamicFrame` with the following schema:

```
{{{
  root
  |-- name: string
  |-- age: int
  |-- address: struct
  |    |-- state: string
  |    |-- city: string
}}}
```

The following call unnests the address struct:

```
{{{
  df.unnest()
}}}
```

The resulting schema is as follows:

```
{{{
  root
  |-- name: string
  |-- age: int
  |-- address.state: string
  |-- address.city: string
}}}
```

This method also unnests nested structs inside of arrays\. But for historical reasons, the names of such fields are prepended with the name of the enclosing array and "`.val`"\.

## def withFrameSchema<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-withFrameSchema"></a>

```
def withFrameSchema( getSchema : () => Schema ) : DynamicFrame 
```
+ `getSchema` — A function that returns the schema to use\. Specified as a zero\-parameter function to defer potentially expensive computation\.

Sets the schema of this `DynamicFrame` to the specified value\. This is primarily used internally to avoid costly schema recomputation\. The passed\-in schema must contain all columns present in the data\.

## def withName<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-withName"></a>

```
def withName( name : String ) : DynamicFrame 
```
+ `name` — The new name to use\.

Returns a copy of this `DynamicFrame` with a new name\.

## def withTransformationContext<a name="glue-etl-scala-apis-glue-dynamicframe-class-defs-withTransformationContext"></a>

```
def withTransformationContext( ctx : String ) : DynamicFrame 
```

Returns a copy of this `DynamicFrame` with the specified transformation context\.