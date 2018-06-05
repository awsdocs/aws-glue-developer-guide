# AWS Glue Scala ArrayNode APIs<a name="glue-etl-scala-apis-glue-types-arraynode"></a>

**Package: com\.amazonaws\.services\.glue\.types**

## ArrayNode Case Class<a name="glue-etl-scala-apis-glue-types-arraynode-case-class"></a>

 **ArrayNode**

```
case class ArrayNode extends DynamicNode  (
           value : ArrayBuffer[DynamicNode] )
```

### ArrayNode def Methods<a name="glue-etl-scala-apis-glue-types-arraynode-case-class-defs"></a>

```
def add( node : DynamicNode )
```

```
def clone
```

```
def equals( other : Any )
```

```
def get( index : Int ) : Option[DynamicNode] 
```

```
def getValue
```

```
def hashCode : Int 
```

```
def isEmpty : Boolean 
```

```
def nodeType
```

```
def remove( index : Int )
```

```
def this
```

```
def toIterator : Iterator[DynamicNode] 
```

```
def toJson : String 
```

```
def update( index : Int,
            node : DynamicNode )
```