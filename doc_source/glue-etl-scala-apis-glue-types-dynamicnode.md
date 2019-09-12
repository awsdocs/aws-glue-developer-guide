# AWS Glue Scala DynamicNode APIs<a name="glue-etl-scala-apis-glue-types-dynamicnode"></a>

**Topics**
+ [DynamicNode Class](#glue-etl-scala-apis-glue-types-dynamicnode-class)
+ [DynamicNode Object](#glue-etl-scala-apis-glue-types-dynamicnode-object)

**Package: com\.amazonaws\.services\.glue\.types**

## DynamicNode Class<a name="glue-etl-scala-apis-glue-types-dynamicnode-class"></a>

**DynamicNode**

```
class DynamicNode extends Serializable with Cloneable 
```

### DynamicNode def Methods<a name="glue-etl-scala-apis-glue-types-dynamicnode-class-defs"></a>

```
def getValue : Any
```

Get plain value and bind to the current record:

```
def nodeType : TypeCode
```

```
def toJson : String
```

Method for debug:

```
def toRow( schema : Schema,
           options : Map[String, ResolveOption]
         ) : Row
```

```
def typeName : String 
```

## DynamicNode Object<a name="glue-etl-scala-apis-glue-types-dynamicnode-object"></a>

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