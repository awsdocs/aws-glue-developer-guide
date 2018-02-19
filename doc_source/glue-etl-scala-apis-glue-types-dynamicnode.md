# The AWS Glue Scala DynamicNode APIs<a name="glue-etl-scala-apis-glue-types-dynamicnode"></a>


+ [The DynamicNode Class](#glue-etl-scala-apis-glue-types-dynamicnode-class)
+ [The DynamicNode Object](#glue-etl-scala-apis-glue-types-dynamicnode-object)

**Package:   com\.amazonaws\.services\.glue\.types**

## The DynamicNode Class<a name="glue-etl-scala-apis-glue-types-dynamicnode-class"></a>

abstract **DynamicNode**

```
class DynamicNode extends Serializable with Cloneable 
```

### DynamicNode def Methods<a name="glue-etl-scala-apis-glue-types-dynamicnode-class-defs"></a>

```
def getValue : Any
```

get plain value bind to current record

```
def nodeType : TypeCode
```

```
def toJson : String
```

method for debug

```
def toRow( schema : Schema,
           options : Map[String, ResolveOption]
         ) : Row
```

```
def typeName : String 
```

## The DynamicNode Object<a name="glue-etl-scala-apis-glue-types-dynamicnode-object"></a>

 **DynamicNode**

```
object DynamicNode
```

### DynamicNode def Methods<a name="glue-etl-scala-apis-glue-types-dynamicnode-object-defs"></a>

```
def quote( field : String,
           useQuotes : Boolean
         ) : String
```

```
def quote( node : DynamicNode,
           useQuotes : Boolean
         ) : String
```