# GlueTransform Base Class<a name="aws-glue-api-crawler-pyspark-transforms-GlueTransform"></a>

The base class that all the `awsglue.transforms` classes inherit from\.

The classes all define a `__call__` method\. They either override the `GlueTransform` class methods listed in the following sections, or they are called using the class name by default\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-GlueTransform-_methods"></a>
+ [apply\(cls, \*args, \*\*kwargs\)](#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)
+ [name\(cls\)](#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)
+ [describeArgs\(cls\)](#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)
+ [describeReturn\(cls\)](#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)
+ [describeTransform\(cls\)](#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)
+ [describeErrors\(cls\)](#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)
+ [describe\(cls\)](#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply"></a>

Applies the transform by calling the transform class, and returns the result\.
+ `cls` – The `self` class object\.

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-GlueTransform-name"></a>

Returns the name of the derived transform class\.
+ `cls` – The `self` class object\.

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs"></a>
+ `cls` – The `self` class object\.

Returns a list of dictionaries, each corresponding to a named argument, in the following format:

```
[
  {
    "name": "(name of argument)",
    "type": "(type of argument)",
    "description": "(description of argument)",
    "optional": "(Boolean, True if the argument is optional)",
    "defaultValue": "(Default value string, or None)(String; the default value, or None)"
  },
...
]
```

Raises a `NotImplementedError` exception when called in a derived transform where it is not implemented\.

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn"></a>
+ `cls` – The `self` class object\.

Returns a dictionary with information about the return type, in the following format:

```
{
  "type": "(return type)",
  "description": "(description of output)"
}
```

Raises a `NotImplementedError` exception when called in a derived transform where it is not implemented\.

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform"></a>

Returns a string describing the transform\.
+ `cls` – The `self` class object\.

Raises a `NotImplementedError` exception when called in a derived transform where it is not implemented\.

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors"></a>
+ `cls` – The `self` class object\.

Returns a list of dictionaries, each describing a possible exception thrown by this transform, in the following format:

```
[
  {
    "type": "(type of error)",
    "description": "(description of error)"
  },
...
]
```

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe"></a>
+ `cls` – The `self` class object\.

Returns an object with the following format:

```
{
  "transform" : {
    "name" : cls.name( ),
    "args" : cls.describeArgs( ),
    "returns" : cls.describeReturn( ),
    "raises" : cls.describeErrors( ),
    "location" : "internal"
  }
}
```