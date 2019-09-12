# AWS Glue Scala ObjectNode APIs<a name="glue-etl-scala-apis-glue-types-objectnode"></a>

**Topics**
+ [ObjectNode Object](#glue-etl-scala-apis-glue-types-objectnode-object)
+ [ObjectNode Case Class](#glue-etl-scala-apis-glue-types-objectnode-case-class)

**Package: com\.amazonaws\.services\.glue\.types**

## ObjectNode Object<a name="glue-etl-scala-apis-glue-types-objectnode-object"></a>

**ObjectNode**

```
object ObjectNode
```

### ObjectNode def Methods<a name="glue-etl-scala-apis-glue-types-objectnode-object-defs"></a>

```
def apply( frameKeys : Set[String],
           v1 : mutable.Map[String, DynamicNode],
           v2 : mutable.Map[String, DynamicNode],
           resolveWith : String
         ) : ObjectNode
```

## ObjectNode Case Class<a name="glue-etl-scala-apis-glue-types-objectnode-case-class"></a>

 **ObjectNode**

```
case class ObjectNode extends MapLikeNode(value)  (
           val value : mutable.Map[String, DynamicNode] )
```

### ObjectNode def Methods<a name="glue-etl-scala-apis-glue-types-objectnode-case-class-defs"></a>

```
def clone
```

```
def equals( other : Any )
```

```
def hashCode : Int 
```

```
def nodeType
```

```
def this
```