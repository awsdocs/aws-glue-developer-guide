# The AWS Glue Scala ScalarNode APIs<a name="glue-etl-scala-apis-glue-types-scalarnode"></a>

**Topics**
+ [The ScalarNode Class](#glue-etl-scala-apis-glue-types-scalarnode-class)
+ [The ScalarNode Object](#glue-etl-scala-apis-glue-types-scalarnode-object)

**Package:   com\.amazonaws\.services\.glue\.types**

## The ScalarNode Class<a name="glue-etl-scala-apis-glue-types-scalarnode-class"></a>

abstract **ScalarNode**

```
class ScalarNode extends DynamicNode  (
           value : Any,
           scalarType : TypeCode )
```

### ScalarNode def Methods<a name="glue-etl-scala-apis-glue-types-scalarnode-class-defs"></a>

```
def compare( other : Any,
             operator : String
           ) : Boolean
```

```
def getValue
```

```
def hashCode : Int 
```

```
def nodeType
```

```
def toJson
```

## The ScalarNode Object<a name="glue-etl-scala-apis-glue-types-scalarnode-object"></a>

 **ScalarNode**

```
object ScalarNode
```

### ScalarNode def Methods<a name="glue-etl-scala-apis-glue-types-scalarnode-object-defs"></a>

```
def apply( v : Any ) : DynamicNode 
```

```
def compare( tv : Ordered[T],
             other : T,
             operator : String
           ) : Boolean
```

```
def compareAny( v : Any,
                y : Any,
                o : String )
```

```
def withEscapedSpecialCharacters( jsonToEscape : String ) : String 
```