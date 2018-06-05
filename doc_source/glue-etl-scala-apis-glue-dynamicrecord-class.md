# AWS Glue Scala DynamicRecord Class<a name="glue-etl-scala-apis-glue-dynamicrecord-class"></a>

**Topics**
+ [def addField](#glue-etl-scala-apis-glue-dynamicrecord-class-defs-addField)
+ [def dropField](#glue-etl-scala-apis-glue-dynamicrecord-class-defs-dropField)
+ [def setError](#glue-etl-scala-apis-glue-dynamicrecord-class-defs-setError)
+ [def isError](#glue-etl-scala-apis-glue-dynamicrecord-class-defs-isError)
+ [def getError](#glue-etl-scala-apis-glue-dynamicrecord-class-defs-getError)
+ [def clearError](#glue-etl-scala-apis-glue-dynamicrecord-class-defs-clearError)
+ [def write](#glue-etl-scala-apis-glue-dynamicrecord-class-defs-write)
+ [def readFields](#glue-etl-scala-apis-glue-dynamicrecord-class-defs-readFields)
+ [def clone](#glue-etl-scala-apis-glue-dynamicrecord-class-defs-clone)
+ [def schema](#glue-etl-scala-apis-glue-dynamicrecord-class-defs-schema)
+ [def getRoot](#glue-etl-scala-apis-glue-dynamicrecord-class-defs-getRoot)
+ [def toJson](#glue-etl-scala-apis-glue-dynamicrecord-class-defs-toJson)
+ [def getFieldNode](#glue-etl-scala-apis-glue-dynamicrecord-class-defs-getFieldNode)
+ [def getField](#glue-etl-scala-apis-glue-dynamicrecord-class-defs-getField)
+ [def hashCode](#glue-etl-scala-apis-glue-dynamicrecord-class-defs-hashCode)
+ [def equals](#glue-etl-scala-apis-glue-dynamicrecord-class-defs-equals)
+ [DynamicRecord Object](#glue-etl-scala-apis-glue-dynamicrecord-object)
+ [RecordTraverser Trait](#glue-etl-scala-apis-glue-recordtraverser-trait)

**Package: com\.amazonaws\.services\.glue**

```
class DynamicRecord extends Serializable with Writable with Cloneable
```

A `DynamicRecord` is a self\-describing data structure that represents a row of data in the dataset that is being processed\. It is self\-describing in the sense that you can get the schema of the row that is represented by the `DynamicRecord` by inspecting the record itself\. A `DynamicRecord` is similar to a `Row` in Apache Spark\.

## def addField<a name="glue-etl-scala-apis-glue-dynamicrecord-class-defs-addField"></a>

```
def addField( path : String,
              dynamicNode : DynamicNode
            ) : Unit
```

Adds a [DynamicNode](glue-etl-scala-apis-glue-types-dynamicnode.md) to the specified path\.
+ `path` — The path for the field to be added\.
+ `dynamicNode` — The [DynamicNode](glue-etl-scala-apis-glue-types-dynamicnode.md) to be added at the specified path\.

## def dropField<a name="glue-etl-scala-apis-glue-dynamicrecord-class-defs-dropField"></a>

```
 def dropField(path: String, underRename: Boolean = false): Option[DynamicNode]
```

Drops a [DynamicNode](glue-etl-scala-apis-glue-types-dynamicnode.md) from the specified path and returns the dropped node if there is not an array in the specified path\.
+ `path` — The path to the field to drop\.
+ `underRename` — True if `dropField` is called as part of a rename transform, or false otherwise \(false by default\)\.

Returns a `scala.Option Option` \([DynamicNode](glue-etl-scala-apis-glue-types-dynamicnode.md)\)\.

## def setError<a name="glue-etl-scala-apis-glue-dynamicrecord-class-defs-setError"></a>

```
def setError( error : Error )
```

Sets this record as an error record, as specified by the `error` parameter\.

Returns a `DynamicRecord`\.

## def isError<a name="glue-etl-scala-apis-glue-dynamicrecord-class-defs-isError"></a>

```
def isError
```

Checks whether this record is an error record\.

## def getError<a name="glue-etl-scala-apis-glue-dynamicrecord-class-defs-getError"></a>

```
def getError
```

Gets the `Error` if the record is an error record\. Returns `scala.Some Some` \(Error\) if this record is an error record, or otherwise `scala.None` \.

## def clearError<a name="glue-etl-scala-apis-glue-dynamicrecord-class-defs-clearError"></a>

```
def clearError
```

Set the `Error` to `scala.None.None` \.

## def write<a name="glue-etl-scala-apis-glue-dynamicrecord-class-defs-write"></a>

```
override def write( out : DataOutput ) : Unit 
```

## def readFields<a name="glue-etl-scala-apis-glue-dynamicrecord-class-defs-readFields"></a>

```
override def readFields( in : DataInput ) : Unit 
```

## def clone<a name="glue-etl-scala-apis-glue-dynamicrecord-class-defs-clone"></a>

```
override def clone : DynamicRecord 
```

Clones this record to a new `DynamicRecord` and returns it\.

## def schema<a name="glue-etl-scala-apis-glue-dynamicrecord-class-defs-schema"></a>

```
def schema
```

Gets the `Schema` by inspecting the record\.

## def getRoot<a name="glue-etl-scala-apis-glue-dynamicrecord-class-defs-getRoot"></a>

```
def getRoot : ObjectNode 
```

Gets the root `ObjectNode` for the record\.

## def toJson<a name="glue-etl-scala-apis-glue-dynamicrecord-class-defs-toJson"></a>

```
def toJson : String 
```

Gets the JSON string for the record\.

## def getFieldNode<a name="glue-etl-scala-apis-glue-dynamicrecord-class-defs-getFieldNode"></a>

```
def getFieldNode( path : String ) : Option[DynamicNode] 
```

Gets the field's value at the specified `path` as an option of `DynamicNode`\.

Returns `scala.Some Some` \([DynamicNode](glue-etl-scala-apis-glue-types-dynamicnode.md)\) if the field exists, or otherwise `scala.None.None` \.

## def getField<a name="glue-etl-scala-apis-glue-dynamicrecord-class-defs-getField"></a>

```
def getField( path : String ) : Option[Any] 
```

Gets the field's value at the specified `path` as an option of `DynamicNode`\.

Returns `scala.Some Some` \(value\)\.

## def hashCode<a name="glue-etl-scala-apis-glue-dynamicrecord-class-defs-hashCode"></a>

```
override def hashCode : Int 
```

## def equals<a name="glue-etl-scala-apis-glue-dynamicrecord-class-defs-equals"></a>

```
override def equals( other : Any )
```

## DynamicRecord Object<a name="glue-etl-scala-apis-glue-dynamicrecord-object"></a>

```
object DynamicRecord
```

### def apply<a name="glue-etl-scala-apis-glue-dynamicrecord-object-defs-apply"></a>

```
def apply( row : Row,
           schema : SparkStructType )
```

Apply method to convert an Apache Spark SQL `Row` to a [DynamicRecord](#glue-etl-scala-apis-glue-dynamicrecord-class)\.
+ `row` — A Spark SQL `Row`\.
+ `schema` — The `Schema` of that row\.

Returns a `DynamicRecord`\.

## RecordTraverser Trait<a name="glue-etl-scala-apis-glue-recordtraverser-trait"></a>

```
trait RecordTraverser {
  def nullValue(): Unit
  def byteValue(value: Byte): Unit
  def binaryValue(value: Array[Byte]): Unit
  def booleanValue(value: Boolean): Unit
  def shortValue(value: Short) : Unit
  def intValue(value: Int) : Unit
  def longValue(value: Long) : Unit
  def floatValue(value: Float): Unit
  def doubleValue(value: Double): Unit
  def decimalValue(value: BigDecimal): Unit
  def stringValue(value: String): Unit
  def dateValue(value: Date): Unit
  def timestampValue(value: Timestamp): Unit
  def objectStart(length: Int): Unit
  def objectKey(key: String): Unit
  def objectEnd(): Unit
  def mapStart(length: Int): Unit
  def mapKey(key: String): Unit
  def mapEnd(): Unit
  def arrayStart(length: Int): Unit
  def arrayEnd(): Unit
}
```