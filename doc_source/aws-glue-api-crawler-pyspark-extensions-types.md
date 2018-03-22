# PySpark Extension Types<a name="aws-glue-api-crawler-pyspark-extensions-types"></a>

The types that are used by the AWS Glue PySpark extensions\.

## DataType<a name="aws-glue-api-crawler-pyspark-extensions-types-awsglue-datatype"></a>

The base class for the other AWS Glue types\.

**`__init__(properties={})`**
+ `properties` – Properties of the data type \(optional\)\.

 

**`typeName(cls)`**

Returns the type of the AWS Glue type class \(that is, the class name with "Type" removed from the end\)\.
+ `cls` – An AWS Glue class instance derived from `DataType`\.

 

`jsonValue( )`

Returns a JSON object that contains the data type and properties of the class:

```
  {
    "dataType": typeName,
    "properties": properties
  }
```

## AtomicType and Simple Derivatives<a name="aws-glue-api-crawler-pyspark-extensions-types-awsglue-atomictype"></a>

Inherits from and extends the [DataType](#aws-glue-api-crawler-pyspark-extensions-types-awsglue-datatype) class, and serves as the base class for all the AWS Glue atomic data types\.

**`fromJsonValue(cls, json_value)`**

Initializes a class instance with values from a JSON object\.
+ `cls` – An AWS Glue type class instance to initialize\.
+ `json_value` – The JSON object to load key\-value pairs from\.

 

The following types are simple derivatives of the [AtomicType](#aws-glue-api-crawler-pyspark-extensions-types-awsglue-atomictype) class:
+ `BinaryType` – Binary data\.
+ `BooleanType` – Boolean values\.
+ `ByteType` – A byte value\.
+ `DateType` – A datetime value\.
+ `DoubleType` – A floating\-point double value\.
+ `IntegerType` – An integer value\.
+ `LongType` – A long integer value\.
+ `NullType` – A null value\.
+ `ShortType` – A short integer value\.
+ `StringType` – A text string\.
+ `TimestampType` – A timestamp value \(typically in seconds from 1/1/1970\)\.
+ `UnknownType` – A value of unidentified type\.

## DecimalType\(AtomicType\)<a name="aws-glue-api-crawler-pyspark-extensions-types-awsglue-decimaltype"></a>

Inherits from and extends the [AtomicType](#aws-glue-api-crawler-pyspark-extensions-types-awsglue-atomictype) class to represent a decimal number \(a number expressed in decimal digits, as opposed to binary base\-2 numbers\)\.

**`__init__(precision=10, scale=2, properties={})`**
+ `precision` – The number of digits in the decimal number \(optional; the default is 10\)\.
+ `scale` – The number of digits to the right of the decimal point \(optional; the default is 2\)\.
+ `properties` – The properties of the decimal number \(optional\)\.

## EnumType\(AtomicType\)<a name="aws-glue-api-crawler-pyspark-extensions-types-awsglue-enumtype"></a>

Inherits from and extends the [AtomicType](#aws-glue-api-crawler-pyspark-extensions-types-awsglue-atomictype) class to represent an enumeration of valid options\.

**`__init__(options)`**
+ `options` – A list of the options being enumerated\.

##  Collection Types<a name="aws-glue-api-crawler-pyspark-extensions-types-awsglue-collections"></a>
+ [ArrayType\(DataType\)](#aws-glue-api-crawler-pyspark-extensions-types-awsglue-arraytype)
+ [ChoiceType\(DataType\)](#aws-glue-api-crawler-pyspark-extensions-types-awsglue-choicetype)
+ [MapType\(DataType\)](#aws-glue-api-crawler-pyspark-extensions-types-awsglue-maptype)
+ [Field\(Object\)](#aws-glue-api-crawler-pyspark-extensions-types-awsglue-field)
+ [StructType\(DataType\)](#aws-glue-api-crawler-pyspark-extensions-types-awsglue-structtype)
+ [EntityType\(DataType\)](#aws-glue-api-crawler-pyspark-extensions-types-awsglue-entitytype)

## ArrayType\(DataType\)<a name="aws-glue-api-crawler-pyspark-extensions-types-awsglue-arraytype"></a>

**`__init__(elementType=UnknownType(), properties={})`**
+ `elementType` – The type of elements in the array \(optional; the default is UnknownType\)\.
+ `properties` – Properties of the array \(optional\)\.

## ChoiceType\(DataType\)<a name="aws-glue-api-crawler-pyspark-extensions-types-awsglue-choicetype"></a>

**`__init__(choices=[], properties={})`**
+ `choices` – A list of possible choices \(optional\)\.
+ `properties` – Properties of these choices \(optional\)\.

 

**`add(new_choice)`**

Adds a new choice to the list of possible choices\.
+ `new_choice` – The choice to add to the list of possible choices\.

 

**`merge(new_choices)`**

Merges a list of new choices with the existing list of choices\.
+ `new_choices` – A list of new choices to merge with existing choices\.

## MapType\(DataType\)<a name="aws-glue-api-crawler-pyspark-extensions-types-awsglue-maptype"></a>

**`__init__(valueType=UnknownType, properties={})`**
+ `valueType` – The type of values in the map \(optional; the default is UnknownType\)\.
+ `properties` – Properties of the map \(optional\)\.

## Field\(Object\)<a name="aws-glue-api-crawler-pyspark-extensions-types-awsglue-field"></a>

Creates a field object out of an object that derives from [DataType](#aws-glue-api-crawler-pyspark-extensions-types-awsglue-datatype)\.

**`__init__(name, dataType, properties={})`**
+ `name` – The name to be assigned to the field\.
+ `dataType` – The object to create a field from\.
+ `properties` – Properties of the field \(optional\)\.

## StructType\(DataType\)<a name="aws-glue-api-crawler-pyspark-extensions-types-awsglue-structtype"></a>

Defines a data structure \(`struct`\)\.

**`__init__(fields=[], properties={})`**
+ `fields` – A list of the fields \(of type `Field`\) to include in the structure \(optional\)\.
+ `properties` – Properties of the structure \(optional\)\.

 

**`add(field)`**
+ `field` – An object of type `Field` to add to the structure\.

 

**`hasField(field)`**

Returns `True` if this structure has a field of the same name, or `False` if not\.
+ `field` – A field name, or an object of type `Field` whose name is used\.

 

**`getField(field)`**
+ `field` – A field name or an object of type `Field` whose name is used\. If the structure has a field of the same name, it is returned\.

## EntityType\(DataType\)<a name="aws-glue-api-crawler-pyspark-extensions-types-awsglue-entitytype"></a>

`__init__(entity, base_type, properties)`

This class is not yet implemented\.

##  Other Types<a name="aws-glue-api-crawler-pyspark-extensions-types-awsglue-other-types"></a>
+ [DataSource\(object\)](#aws-glue-api-crawler-pyspark-extensions-types-awsglue-data-source)
+ [DataSink\(object\)](#aws-glue-api-crawler-pyspark-extensions-types-awsglue-data-sink)

## DataSource\(object\)<a name="aws-glue-api-crawler-pyspark-extensions-types-awsglue-data-source"></a>

**`__init__(j_source, sql_ctx, name)`**
+ `j_source` – The data source\.
+ `sql_ctx` – The SQL context\.
+ `name` – The data\-source name\.

 

**`setFormat(format, **options)`**
+ `format` – The format to set for the data source\.
+ `options` – A collection of options to set for the data source\.

 

`getFrame()`

Returns a `DynamicFrame` for the data source\.

## DataSink\(object\)<a name="aws-glue-api-crawler-pyspark-extensions-types-awsglue-data-sink"></a>

**`__init__(j_sink, sql_ctx)`**
+ `j_sink` – The sink to create\.
+ `sql_ctx` – The SQL context for the data sink\.

 

**`setFormat(format, **options)`**
+ `format` – The format to set for the data sink\.
+ `options` – A collection of options to set for the data sink\.

 

**`setAccumulableSize(size)`**
+ `size` – The accumulable size to set, in bytes\.

 

**`writeFrame(dynamic_frame, info="")`**
+ `dynamic_frame` – The `DynamicFrame` to write\.
+ `info` – Information about the `DynamicFrame` \(optional\)\.

 

**`write(dynamic_frame_or_dfc, info="")`**

Writes a `DynamicFrame` or a `DynamicFrameCollection`\.
+ `dynamic_frame_or_dfc` – Either a `DynamicFrame` object or a `DynamicFrameCollection` object to be written\.
+ `info` – Information about the `DynamicFrame` or `DynamicFrames` to be written \(optional\)\.