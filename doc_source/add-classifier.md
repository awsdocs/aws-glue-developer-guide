# Adding Classifiers to a Crawler<a name="add-classifier"></a>

A classifier reads the data in a data store\. If it recognizes the format of the data, it generates a schema\. The classifier also returns a certainty number to indicate how certain the format recognition was\. 

AWS Glue provides a set of built\-in classifiers, but you can also create custom classifiers\. AWS Glue invokes custom classifiers first, in the order that you specify in your crawler definition\. Depending on the results that are returned from custom classifiers, AWS Glue might also invoke built\-in classifiers\. If a classifier returns `certainty=1.0` during processing, it indicates that it's 100 percent certain that it can create the correct schema\. AWS Glue then uses the output of that classifier\. 

If no classifier returns `certainty=1.0`, AWS Glue uses the output of the classifier that has the highest certainty\. If no classifier returns a certainty greater than `0.0`, AWS Glue returns the default classification string of `UNKNOWN`\.

## When Do I Use a Classifier?<a name="classifier-when-used"></a>

You use classifiers when you crawl a data store to define metadata tables in the AWS Glue Data Catalog\. You can set up your crawler with an ordered set of classifiers\. When the crawler invokes a classifier, the classifier determines whether the data is recognized\.  If the classifier can't recognize the data or is not 100 percent certain, the crawler invokes the next classifier in the list to determine whether it can recognize the data\.  

 For more information about creating a classifier using the AWS Glue console, see [Working with Classifiers on the AWS Glue Console](console-classifiers.md)\. 

## Custom Classifiers<a name="classifier-defining"></a>

The output of a classifier includes a string that indicates the file's classification or format \(for example, `json`\) and the schema of the file\. For custom classifiers, you define the logic for creating the schema based on the type of classifier\. Classifier types include defining schemas based on grok patterns, XML tags, and JSON paths\.

If you change a classifier definition, any data that was previously crawled using the classifier is not reclassified\. A crawler keeps track of previously crawled data\. New data is classified with the updated classifier, which might result in an updated schema\. If the schema of your data has evolved, update the classifier to account for any schema changes when your crawler runs\. To reclassify data to correct an incorrect classifier, create a new crawler with the updated classifier\.   

For more information about creating custom classifiers in AWS Glue, see [Writing Custom Classifiers](custom-classifier.md)\.

**Note**  
If your data format is recognized by one of the built\-in classifiers, you don't need to create a custom classifier\.

## Built\-In Classifiers in AWS Glue<a name="classifier-built-in"></a>

 AWS Glue provides built\-in classifiers for various formats, including JSON, CSV, web logs, and many database systems\.

If AWS Glue doesn't find a custom classifier that fits the input data format with 100 percent certainty, it invokes the built\-in classifiers in the order shown in the following table\. The built\-in classifiers return a result to indicate whether the format matches \(`certainty=1.0`\) or does not match \(`certainty=0.0`\)\. The first classifier that has `certainty=1.0` provides the classification string and schema for a metadata table in your Data Catalog\.


| Classifier type | Classification string | Notes | 
| --- | --- | --- | 
| Apache Avro | avro | Reads the schema at the beginning of the file to determine format\. | 
| Apache ORC | orc | Reads the file metadata to determine format\. | 
| Apache Parquet | parquet | Reads the schema at the end of the file to determine format\. | 
| JSON | json | Reads the beginning of the file to determine format\. | 
| Binary JSON | bson | Reads the beginning of the file to determine format\. | 
| XML | xml | Reads the beginning of the file to determine format\. AWS Glue determines the table schema based on XML tags in the document\.  For information about creating a custom XML classifier to specify rows in the document, see [Writing XML Custom Classifiers](custom-classifier.md#custom-classifier-xml)\.  | 
| Amazon Ion | ion | Reads the beginning of the file to determine format\. | 
| Combined Apache log | combined\_apache | Determines log formats through a grok pattern\. | 
| Apache log | apache | Determines log formats through a grok pattern\. | 
| Linux kernel log | linux\_kernel | Determines log formats through a grok pattern\. | 
| Microsoft log | microsoft\_log | Determines log formats through a grok pattern\. | 
| Ruby log | ruby\_logger | Reads the beginning of the file to determine format\. | 
| Squid 3\.x log | squid | Reads the beginning of the file to determine format\. | 
| Redis monitor log | redismonlog | Reads the beginning of the file to determine format\. | 
| Redis log | redislog | Reads the beginning of the file to determine format\. | 
| CSV | csv | Checks for the following delimiters: comma \(,\), pipe \(\|\), tab \(\\t\), semicolon \(;\), and Ctrl\-A \(\\u0001\)\. Ctrl\-A is the Unicode control character for Start Of Heading\. | 
| Amazon Redshift | redshift | Uses JDBC connection to import metadata\. | 
| MySQL | mysql | Uses JDBC connection to import metadata\. | 
| PostgreSQL | postgresql | Uses JDBC connection to import metadata\. | 
| Oracle database | oracle | Uses JDBC connection to import metadata\. | 
| Microsoft SQL Server | sqlserver | Uses JDBC connection to import metadata\. | 
| Amazon DynamoDB | dynamodb | Reads data from the DynamoDB table\. | 

Files in the following compressed formats can be classified:
+ ZIP \(supported for archives containing only a single file\)\. Note that Zip is not well\-supported in other services \(because of the archive\)\.
+ BZIP
+ GZIP
+ LZ4
+ Snappy \(as standard Snappy format, not as Hadoop native Snappy format\)

### Built\-In CSV Classifier<a name="classifier-builtin-rules"></a>

The built\-in CSV classifier parses CSV file contents to determine the schema for an AWS Glue table\. This classifier checks for the following delimiters:
+ Comma \(,\)
+ Pipe \(\|\)
+ Tab \(\\t\)
+ Semicolon \(;\)
+ Ctrl\-A \(\\u0001\)

  Ctrl\-A is the Unicode control character for `Start Of Heading`\.

To be classified as CSV, the table schema must have at least two columns and two rows of data\. The CSV classifier uses a number of heuristics to determine whether a header is present in a given file\. If the classifier can't determine a header from the first row of data, column headers are displayed as `col1`, `col2`, `col3`, and so on\. The built\-in CSV classifier determines whether to infer a header by evaluating the following characteristics of the file:
+ Every column in a potential header parses as a STRING data type\.
+ Except for the last column, every column in a potential header has content that is fewer than 150 characters\. To allow for a trailing delimiter, the last column can be empty throughout the file\.
+ Every column in a potential header must meet the AWS Glue `regex` requirements for a column name\.
+ The header row must be sufficiently different from the data rows\. To determine this, one or more of the rows must parse as other than STRING type\. If all columns are of type STRING, then the first row of data is not sufficiently different from subsequent rows to be used as the header\.

**Note**  
If the built\-in CSV classifier does not create your AWS Glue table as you want, you might be able to use one of the following alternatives:  
Change the column names in the Data Catalog, set the `SchemaChangePolicy` to LOG, and set the partition output configuration to `InheritFromTable` for future crawler runs\.
Create a custom grok classifier to parse the data and assign the columns that you want\.
The built\-in CSV classifier creates tables referencing the `LazySimpleSerDe` as the serialization library, which is a good choice for type inference\. However, if the CSV data contains quoted strings, edit the table definition and change the SerDe library to `OpenCSVSerDe`\. Adjust any inferred types to STRING, set the `SchemaChangePolicy` to LOG, and set the partitions output configuration to `InheritFromTable` for future crawler runs\. For more information about SerDe libraries, see [SerDe Reference](https://docs.aws.amazon.com/athena/latest/ug/serde-reference.html) in the Amazon Athena User Guide\.