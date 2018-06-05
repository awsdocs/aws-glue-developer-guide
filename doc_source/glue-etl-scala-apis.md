# APIs in the AWS Glue Scala Library<a name="glue-etl-scala-apis"></a>

AWS Glue supports an extension of the PySpark Scala dialect for scripting extract, transform, and load \(ETL\) jobs\. The following sections describe the APIs in the AWS Glue Scala library\.

## com\.amazonaws\.services\.glue<a name="glue-etl-scala-apis-glue"></a>

The **com\.amazonaws\.services\.glue** package in the AWS Glue Scala library contains the following APIs:
+ [ChoiceOption](glue-etl-scala-apis-glue-choiceoption.md)
+ [DataSink](glue-etl-scala-apis-glue-datasink-class.md)
+ [DataSource trait](glue-etl-scala-apis-glue-datasource-trait.md)
+ [DynamicFrame](glue-etl-scala-apis-glue-dynamicframe.md)
+ [DynamicRecord](glue-etl-scala-apis-glue-dynamicrecord-class.md)
+ [GlueContext](glue-etl-scala-apis-glue-gluecontext.md)
+ [MappingSpec](#glue-etl-scala-apis-glue-mappingspec)
+ [ResolveSpec](glue-etl-scala-apis-glue-resolvespec.md)

## com\.amazonaws\.services\.glue\.types<a name="glue-etl-scala-apis-glue-types"></a>

The **com\.amazonaws\.services\.glue\.types** package in the AWS Glue Scala library contains the following APIs:
+ [ArrayNode](glue-etl-scala-apis-glue-types-arraynode.md)
+ [BinaryNode](glue-etl-scala-apis-glue-types-binarynode.md)
+ [BooleanNode](glue-etl-scala-apis-glue-types-booleannode.md)
+ [ByteNode](glue-etl-scala-apis-glue-types-bytenode.md)
+ [DateNode](glue-etl-scala-apis-glue-types-datenode.md)
+ [DecimalNode](glue-etl-scala-apis-glue-types-decimalnode.md)
+ [DoubleNode](glue-etl-scala-apis-glue-types-doublenode.md)
+ [DynamicNode](glue-etl-scala-apis-glue-types-dynamicnode.md)
+ [FloatNode](glue-etl-scala-apis-glue-types-floatnode.md)
+ [IntegerNode](glue-etl-scala-apis-glue-types-integernode.md)
+ [LongNode](glue-etl-scala-apis-glue-types-longnode.md)
+ [MapLikeNode](glue-etl-scala-apis-glue-types-maplikenode.md)
+ [MapNode](glue-etl-scala-apis-glue-types-mapnode.md)
+ [NullNode](glue-etl-scala-apis-glue-types-nullnode.md)
+ [ObjectNode](glue-etl-scala-apis-glue-types-objectnode.md)
+ [ScalarNode](glue-etl-scala-apis-glue-types-scalarnode.md)
+ [ShortNode](glue-etl-scala-apis-glue-types-shortnode.md)
+ [StringNode](glue-etl-scala-apis-glue-types-stringnode.md)
+ [TimestampNode](glue-etl-scala-apis-glue-types-timestampnode.md)

## com\.amazonaws\.services\.glue\.util<a name="glue-etl-scala-apis-glue-util"></a>

The **com\.amazonaws\.services\.glue\.util** package in the AWS Glue Scala library contains the following APIs:
+ [GlueArgParser](glue-etl-scala-apis-glue-util-glueargparser.md)
+ [Job](glue-etl-scala-apis-glue-util-job.md)

## MappingSpec<a name="glue-etl-scala-apis-glue-mappingspec"></a>

**Package: com\.amazonaws\.services\.glue**

### MappingSpec Case Class<a name="glue-etl-scala-apis-glue-mappingspec-case-class"></a>

```
case class MappingSpec( sourcePath: SchemaPath,
                        sourceType: DataType,
                        targetPath: SchemaPath,
                        targetType: DataTyp
                       ) extends Product4[String, String, String, String] {
  override def _1: String = sourcePath.toString
  override def _2: String = ExtendedTypeName.fromDataType(sourceType)
  override def _3: String = targetPath.toString
  override def _4: String = ExtendedTypeName.fromDataType(targetType)
}
```
+ `sourcePath` — The `SchemaPath` of the source field\.
+ `sourceType` — The `DataType` of the source field\.
+ `targetPath` — The `SchemaPath` of the target field\.
+ `targetType` — The `DataType` of the target field\.

A `MappingSpec` specifies a mapping from a source path and a source data type to a target path and a target data type\. The value at the source path in the source frame appears in the target frame at the target path\. The source data type is cast to the target data type\.

It extends from `Product4` so that you can handle any `Product4` in your `applyMapping` interface\.

### MappingSpec Object<a name="glue-etl-scala-apis-glue-mappingspec-object"></a>

```
object MappingSpec
```

The `MappingSpec` object has the following members:

### val orderingByTarget<a name="glue-etl-scala-apis-glue-mappingspec-object-val-orderingbytarget"></a>

```
val orderingByTarget: Ordering[MappingSpec]
```

### def apply<a name="glue-etl-scala-apis-glue-mappingspec-object-defs-apply-1"></a>

```
def apply( sourcePath : String,
           sourceType : DataType,
           targetPath : String,
           targetType : DataType
         ) : MappingSpec
```

Creates a `MappingSpec`\.
+ `sourcePath` — A string representation of the source path\.
+ `sourceType` — The source `DataType`\.
+ `targetPath` — A string representation of the target path\.
+ `targetType` — The target `DataType`\.

Returns a `MappingSpec`\.

### def apply<a name="glue-etl-scala-apis-glue-mappingspec-object-defs-apply-2"></a>

```
def apply( sourcePath : String,
           sourceTypeString : String,
           targetPath : String,
           targetTypeString : String
         ) : MappingSpec
```

Creates a `MappingSpec`\.
+ `sourcePath` — A string representation of the source path\.
+ `sourceType` — A string representation of the source data type\.
+ `targetPath` — A string representation of the target path\.
+ `targetType` — A string representation of the target data type\.

Returns a MappingSpec\.

### def apply<a name="glue-etl-scala-apis-glue-mappingspec-object-defs-apply-3"></a>

```
def apply( product : Product4[String, String, String, String] ) : MappingSpec 
```

Creates a `MappingSpec`\.
+ `product` — The `Product4` of the source path, source data type, target path, and target data type\.

Returns a `MappingSpec`\.