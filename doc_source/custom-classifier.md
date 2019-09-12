# Writing Custom Classifiers<a name="custom-classifier"></a>

You can provide a custom classifier to classify your data in AWS Glue\. You can create a custom classifier using a grok pattern, an XML tag, JavaScript Object Notation \(JSON\), or comma\-separated values \(CSV\)\. An AWS Glue crawler calls a custom classifier\. If the classifier recognizes the data, it returns the classification and schema of the data to the crawler\. You might need to define a custom classifier if your data doesn't match any built\-in classifiers, or if you want to customize the tables that are created by the crawler\.

 For more information about creating a classifier using the AWS Glue console, see [Working with Classifiers on the AWS Glue Console](console-classifiers.md)\. 

AWS Glue runs custom classifiers before built\-in classifiers, in the order you specify\. When a crawler finds a classifier that matches the data, the classification string and schema are used in the definition of tables that are written to your AWS Glue Data Catalog\.

**Topics**
+ [Writing Grok Custom Classifiers](#custom-classifier-grok)
+ [Writing XML Custom Classifiers](#custom-classifier-xml)
+ [Writing JSON Custom Classifiers](#custom-classifier-json)
+ [Writing CSV Custom Classifiers](#custom-classifier-csv)

## Writing Grok Custom Classifiers<a name="custom-classifier-grok"></a>

Grok is a tool that is used to parse textual data given a matching pattern\. A grok pattern is a named set of regular expressions \(regex\) that are used to match data one line at a time\. AWS Glue uses grok patterns to infer the schema of your data\. When a grok pattern matches your data, AWS Glue uses the pattern to determine the structure of your data and map it into fields\.

AWS Glue provides many built\-in patterns, or you can define your own\. You can create a grok pattern using built\-in patterns and custom patterns in your custom classifier definition\. You can tailor a grok pattern to classify custom text file formats\.

**Note**  
AWS Glue grok custom classifiers use the `GrokSerDe` serialization library for tables created in the AWS Glue Data Catalog\. If you are using the AWS Glue Data Catalog with Amazon Athena, Amazon EMR, or Redshift Spectrum, check the documentation about those services for information about support of the `GrokSerDe`\. Currently, you might encounter problems querying tables created with the `GrokSerDe` from Amazon EMR and Redshift Spectrum\.

The following is the basic syntax for the components of a grok pattern:

```
%{PATTERN:field-name}
```

Data that matches the named `PATTERN` is mapped to the `field-name` column in the schema, with a default data type of `string`\. Optionally, the data type for the field can be cast to `byte`, `boolean`, `double`, `short`, `int`, `long`, or `float` in the resulting schema\.

```
%{PATTERN:field-name:data-type}
```

For example, to cast a `num` field to an `int` data type, you can use this pattern: 

```
%{NUMBER:num:int}
```

Patterns can be composed of other patterns\. For example, you can have a pattern for a `SYSLOG` timestamp that is defined by patterns for month, day of the month, and time \(for example, `Feb 1 06:25:43`\)\. For this data, you might define the following pattern:

```
SYSLOGTIMESTAMP %{MONTH} +%{MONTHDAY} %{TIME}
```

**Note**  
Grok patterns can process only one line at a time\. Multiple\-line patterns are not supported\. Also, line breaks within a pattern are not supported\.

### Custom Classifier Values in AWS Glue<a name="classifier-values"></a>

When you define a grok classifier, you supply the following values to AWS Glue to create the custom classifier\.

**Name**  
Name of the classifier\.

**Classification**  
The text string that is written to describe the format of the data that is classified; for example, `special-logs`\.

**Grok pattern**  
The set of patterns that are applied to the data store to determine whether there is a match\. These patterns are from AWS Glue [built\-in patterns](#classifier-builtin-patterns) and any custom patterns that you define\.  
The following is an example of a grok pattern:  

```
%{TIMESTAMP_ISO8601:timestamp} \[%{MESSAGEPREFIX:message_prefix}\] %{CRAWLERLOGLEVEL:loglevel} : %{GREEDYDATA:message}
```
When the data matches `TIMESTAMP_ISO8601`, a schema column `timestamp` is created\. The behavior is similar for the other named patterns in the example\.

**Custom patterns**  
Optional custom patterns that you define\. These patterns are referenced by the grok pattern that classifies your data\. You can reference these custom patterns in the grok pattern that is applied to your data\. Each custom component pattern must be on a separate line\.  [Regular expression \(regex\)](http://en.wikipedia.org/wiki/Regular_expression) syntax is used to define the pattern\.   
The following is an example of using custom patterns:  

```
CRAWLERLOGLEVEL (BENCHMARK|ERROR|WARN|INFO|TRACE)
MESSAGEPREFIX .*-.*-.*-.*-.*
```
The first custom named pattern, `CRAWLERLOGLEVEL`, is a match when the data matches one of the enumerated strings\. The second custom pattern, `MESSAGEPREFIX`, tries to match a message prefix string\.

AWS Glue keeps track of the creation time, last update time, and version of your classifier\.

### AWS Glue Built\-In Patterns<a name="classifier-builtin-patterns"></a>

AWS Glue provides many common patterns that you can use to build a custom classifier\. You add a named pattern to the `grok pattern` in a classifier definition\.

The following list consists of a line for each pattern\. In each line, the pattern name is followed its definition\. [Regular expression \(regex\)](http://en.wikipedia.org/wiki/Regular_expression) syntax is used in defining the pattern\.

```
#AWS Glue Built-in patterns
 USERNAME [a-zA-Z0-9._-]+
 USER %{USERNAME:UNWANTED}
 INT (?:[+-]?(?:[0-9]+))
 BASE10NUM (?<![0-9.+-])(?>[+-]?(?:(?:[0-9]+(?:\.[0-9]+)?)|(?:\.[0-9]+)))
 NUMBER (?:%{BASE10NUM:UNWANTED})
 BASE16NUM (?<![0-9A-Fa-f])(?:[+-]?(?:0x)?(?:[0-9A-Fa-f]+))
 BASE16FLOAT \b(?<![0-9A-Fa-f.])(?:[+-]?(?:0x)?(?:(?:[0-9A-Fa-f]+(?:\.[0-9A-Fa-f]*)?)|(?:\.[0-9A-Fa-f]+)))\b
 BOOLEAN (?i)(true|false)
 
 POSINT \b(?:[1-9][0-9]*)\b
 NONNEGINT \b(?:[0-9]+)\b
 WORD \b\w+\b
 NOTSPACE \S+
 SPACE \s*
 DATA .*?
 GREEDYDATA .*
 #QUOTEDSTRING (?:(?<!\\)(?:"(?:\\.|[^\\"])*"|(?:'(?:\\.|[^\\'])*')|(?:`(?:\\.|[^\\`])*`)))
 QUOTEDSTRING (?>(?<!\\)(?>"(?>\\.|[^\\"]+)+"|""|(?>'(?>\\.|[^\\']+)+')|''|(?>`(?>\\.|[^\\`]+)+`)|``))
 UUID [A-Fa-f0-9]{8}-(?:[A-Fa-f0-9]{4}-){3}[A-Fa-f0-9]{12}
 
 # Networking
 MAC (?:%{CISCOMAC:UNWANTED}|%{WINDOWSMAC:UNWANTED}|%{COMMONMAC:UNWANTED})
 CISCOMAC (?:(?:[A-Fa-f0-9]{4}\.){2}[A-Fa-f0-9]{4})
 WINDOWSMAC (?:(?:[A-Fa-f0-9]{2}-){5}[A-Fa-f0-9]{2})
 COMMONMAC (?:(?:[A-Fa-f0-9]{2}:){5}[A-Fa-f0-9]{2})
 IPV6 ((([0-9A-Fa-f]{1,4}:){7}([0-9A-Fa-f]{1,4}|:))|(([0-9A-Fa-f]{1,4}:){6}(:[0-9A-Fa-f]{1,4}|((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3})|:))|(([0-9A-Fa-f]{1,4}:){5}(((:[0-9A-Fa-f]{1,4}){1,2})|:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3})|:))|(([0-9A-Fa-f]{1,4}:){4}(((:[0-9A-Fa-f]{1,4}){1,3})|((:[0-9A-Fa-f]{1,4})?:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){3}(((:[0-9A-Fa-f]{1,4}){1,4})|((:[0-9A-Fa-f]{1,4}){0,2}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){2}(((:[0-9A-Fa-f]{1,4}){1,5})|((:[0-9A-Fa-f]{1,4}){0,3}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){1}(((:[0-9A-Fa-f]{1,4}){1,6})|((:[0-9A-Fa-f]{1,4}){0,4}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:))|(:(((:[0-9A-Fa-f]{1,4}){1,7})|((:[0-9A-Fa-f]{1,4}){0,5}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:)))(%.+)?
 IPV4 (?<![0-9])(?:(?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2}))(?![0-9])
 IP (?:%{IPV6:UNWANTED}|%{IPV4:UNWANTED})
 HOSTNAME \b(?:[0-9A-Za-z][0-9A-Za-z-_]{0,62})(?:\.(?:[0-9A-Za-z][0-9A-Za-z-_]{0,62}))*(\.?|\b)
 HOST %{HOSTNAME:UNWANTED}
 IPORHOST (?:%{HOSTNAME:UNWANTED}|%{IP:UNWANTED})
 HOSTPORT (?:%{IPORHOST}:%{POSINT:PORT})
 
 # paths
 PATH (?:%{UNIXPATH}|%{WINPATH})
 UNIXPATH (?>/(?>[\w_%!$@:.,~-]+|\\.)*)+
 #UNIXPATH (?<![\w\/])(?:/[^\/\s?*]*)+
 TTY (?:/dev/(pts|tty([pq])?)(\w+)?/?(?:[0-9]+))
 WINPATH (?>[A-Za-z]+:|\\)(?:\\[^\\?*]*)+
 URIPROTO [A-Za-z]+(\+[A-Za-z+]+)?
 URIHOST %{IPORHOST}(?::%{POSINT:port})?
 # uripath comes loosely from RFC1738, but mostly from what Firefox
 # doesn't turn into %XX
 URIPATH (?:/[A-Za-z0-9$.+!*'(){},~:;=@#%_\-]*)+
 #URIPARAM \?(?:[A-Za-z0-9]+(?:=(?:[^&]*))?(?:&(?:[A-Za-z0-9]+(?:=(?:[^&]*))?)?)*)?
 URIPARAM \?[A-Za-z0-9$.+!*'|(){},~@#%&/=:;_?\-\[\]]*
 URIPATHPARAM %{URIPATH}(?:%{URIPARAM})?
 URI %{URIPROTO}://(?:%{USER}(?::[^@]*)?@)?(?:%{URIHOST})?(?:%{URIPATHPARAM})?
 
 # Months: January, Feb, 3, 03, 12, December
 MONTH \b(?:Jan(?:uary)?|Feb(?:ruary)?|Mar(?:ch)?|Apr(?:il)?|May|Jun(?:e)?|Jul(?:y)?|Aug(?:ust)?|Sep(?:tember)?|Oct(?:ober)?|Nov(?:ember)?|Dec(?:ember)?)\b
 MONTHNUM (?:0?[1-9]|1[0-2])
 MONTHNUM2 (?:0[1-9]|1[0-2])
 MONTHDAY (?:(?:0[1-9])|(?:[12][0-9])|(?:3[01])|[1-9])
 
 # Days: Monday, Tue, Thu, etc...
 DAY (?:Mon(?:day)?|Tue(?:sday)?|Wed(?:nesday)?|Thu(?:rsday)?|Fri(?:day)?|Sat(?:urday)?|Sun(?:day)?)
 
 # Years?
 YEAR (?>\d\d){1,2}
 # Time: HH:MM:SS
 #TIME \d{2}:\d{2}(?::\d{2}(?:\.\d+)?)?
 # TIME %{POSINT<24}:%{POSINT<60}(?::%{POSINT<60}(?:\.%{POSINT})?)?
 HOUR (?:2[0123]|[01]?[0-9])
 MINUTE (?:[0-5][0-9])
 # '60' is a leap second in most time standards and thus is valid.
 SECOND (?:(?:[0-5]?[0-9]|60)(?:[:.,][0-9]+)?)
 TIME (?!<[0-9])%{HOUR}:%{MINUTE}(?::%{SECOND})(?![0-9])
 # datestamp is YYYY/MM/DD-HH:MM:SS.UUUU (or something like it)
 DATE_US %{MONTHNUM}[/-]%{MONTHDAY}[/-]%{YEAR}
 DATE_EU %{MONTHDAY}[./-]%{MONTHNUM}[./-]%{YEAR}
 DATESTAMP_US %{DATE_US}[- ]%{TIME}
 DATESTAMP_EU %{DATE_EU}[- ]%{TIME}
 ISO8601_TIMEZONE (?:Z|[+-]%{HOUR}(?::?%{MINUTE}))
 ISO8601_SECOND (?:%{SECOND}|60)
 TIMESTAMP_ISO8601 %{YEAR}-%{MONTHNUM}-%{MONTHDAY}[T ]%{HOUR}:?%{MINUTE}(?::?%{SECOND})?%{ISO8601_TIMEZONE}?
 TZ (?:[PMCE][SD]T|UTC)
 DATESTAMP_RFC822 %{DAY} %{MONTH} %{MONTHDAY} %{YEAR} %{TIME} %{TZ}
 DATESTAMP_RFC2822 %{DAY}, %{MONTHDAY} %{MONTH} %{YEAR} %{TIME} %{ISO8601_TIMEZONE}
 DATESTAMP_OTHER %{DAY} %{MONTH} %{MONTHDAY} %{TIME} %{TZ} %{YEAR}
 DATESTAMP_EVENTLOG %{YEAR}%{MONTHNUM2}%{MONTHDAY}%{HOUR}%{MINUTE}%{SECOND}
 CISCOTIMESTAMP %{MONTH} %{MONTHDAY} %{TIME}
 
 # Syslog Dates: Month Day HH:MM:SS
 SYSLOGTIMESTAMP %{MONTH} +%{MONTHDAY} %{TIME}
 PROG (?:[\w._/%-]+)
 SYSLOGPROG %{PROG:program}(?:\[%{POSINT:pid}\])?
 SYSLOGHOST %{IPORHOST}
 SYSLOGFACILITY <%{NONNEGINT:facility}.%{NONNEGINT:priority}>
 HTTPDATE %{MONTHDAY}/%{MONTH}/%{YEAR}:%{TIME} %{INT}
 
 # Shortcuts
 QS %{QUOTEDSTRING:UNWANTED}
 
 # Log formats
 SYSLOGBASE %{SYSLOGTIMESTAMP:timestamp} (?:%{SYSLOGFACILITY} )?%{SYSLOGHOST:logsource} %{SYSLOGPROG}:
 
 MESSAGESLOG %{SYSLOGBASE} %{DATA}
 
 COMMONAPACHELOG %{IPORHOST:clientip} %{USER:ident} %{USER:auth} \[%{HTTPDATE:timestamp}\] "(?:%{WORD:verb} %{NOTSPACE:request}(?: HTTP/%{NUMBER:httpversion})?|%{DATA:rawrequest})" %{NUMBER:response} (?:%{Bytes:bytes=%{NUMBER}|-})
 COMBINEDAPACHELOG %{COMMONAPACHELOG} %{QS:referrer} %{QS:agent}
 COMMONAPACHELOG_DATATYPED %{IPORHOST:clientip} %{USER:ident;boolean} %{USER:auth} \[%{HTTPDATE:timestamp;date;dd/MMM/yyyy:HH:mm:ss Z}\] "(?:%{WORD:verb;string} %{NOTSPACE:request}(?: HTTP/%{NUMBER:httpversion;float})?|%{DATA:rawrequest})" %{NUMBER:response;int} (?:%{NUMBER:bytes;long}|-)
 
 
 # Log Levels
 LOGLEVEL ([A|a]lert|ALERT|[T|t]race|TRACE|[D|d]ebug|DEBUG|[N|n]otice|NOTICE|[I|i]nfo|INFO|[W|w]arn?(?:ing)?|WARN?(?:ING)?|[E|e]rr?(?:or)?|ERR?(?:OR)?|[C|c]rit?(?:ical)?|CRIT?(?:ICAL)?|[F|f]atal|FATAL|[S|s]evere|SEVERE|EMERG(?:ENCY)?|[Ee]merg(?:ency)?)
```

## Writing XML Custom Classifiers<a name="custom-classifier-xml"></a>

XML defines the structure of a document with the use of tags in the file\. With an XML custom classifier, you can specify the tag name used to define a row\.

### Custom Classifier Values in AWS Glue<a name="classifier-values-xml"></a>

When you define an XML classifier, you supply the following values to AWS Glue to create the classifier\. The classification field of this classifier is set to `xml`\.

**Name**  
Name of the classifier\.

**Row tag**  
The XML tag name that defines a table row in the XML document, without angle brackets `< >`\. The name must comply with XML rules for a tag\.  
The element containing the row data **cannot** be a self\-closing empty element\. For example, this empty element is **not** parsed by AWS Glue:  

```
            <row att1=”xx” att2=”yy” />  
```
 Empty elements can be written as follows:  

```
            <row att1=”xx” att2=”yy”> </row> 
```

AWS Glue keeps track of the creation time, last update time, and version of your classifier\.

For example, suppose that you have the following XML file\. To create an AWS Glue table that only contains columns for author and title, create a classifier in the AWS Glue console with **Row tag** as `AnyCompany`\. Then add and run a crawler that uses this custom classifier\.

```
<?xml version="1.0"?>
<catalog>
   <book id="bk101">
     <AnyCompany>
       <author>Rivera, Martha</author>
       <title>AnyCompany Developer Guide</title>
     </AnyCompany>
   </book>
   <book id="bk102">
     <AnyCompany>   
       <author>Stiles, John</author>
       <title>Style Guide for AnyCompany</title>
     </AnyCompany>
   </book>
</catalog>
```

## Writing JSON Custom Classifiers<a name="custom-classifier-json"></a>

JSON is a data\-interchange format\. It defines data structures with name\-value pairs or an ordered list of values\. With a JSON custom classifier, you can specify the JSON path to a data structure that is used to define the schema for your table\.

### Custom Classifier Values in AWS Glue<a name="classifier-values-json"></a>

When you define a JSON classifier, you supply the following values to AWS Glue to create the classifier\. The classification field of this classifier is set to `json`\.

**Name**  
Name of the classifier\.

**JSON path**  
A JSON path that points to an object that is used to define a table schema\. The JSON path can be written in dot notation or bracket notation\. The following operators are supported:      
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/custom-classifier.html)

AWS Glue keeps track of the creation time, last update time, and version of your classifier\.

**Example of Using a JSON Classifier to Pull Records from an Array**  
Suppose that your JSON data is an array of records\. For example, the first few lines of your file might look like the following:  

```
[
  {
    "type": "constituency",
    "id": "ocd-division\/country:us\/state:ak",
    "name": "Alaska"
  },
  {
    "type": "constituency",
    "id": "ocd-division\/country:us\/state:al\/cd:1",
    "name": "Alabama's 1st congressional district"
  },
  {
    "type": "constituency",
    "id": "ocd-division\/country:us\/state:al\/cd:2",
    "name": "Alabama's 2nd congressional district"
  },
  {
    "type": "constituency",
    "id": "ocd-division\/country:us\/state:al\/cd:3",
    "name": "Alabama's 3rd congressional district"
  },
  {
    "type": "constituency",
    "id": "ocd-division\/country:us\/state:al\/cd:4",
    "name": "Alabama's 4th congressional district"
  },
  {
    "type": "constituency",
    "id": "ocd-division\/country:us\/state:al\/cd:5",
    "name": "Alabama's 5th congressional district"
  },
  {
    "type": "constituency",
    "id": "ocd-division\/country:us\/state:al\/cd:6",
    "name": "Alabama's 6th congressional district"
  },
  {
    "type": "constituency",
    "id": "ocd-division\/country:us\/state:al\/cd:7",
    "name": "Alabama's 7th congressional district"
  },
  {
    "type": "constituency",
    "id": "ocd-division\/country:us\/state:ar\/cd:1",
    "name": "Arkansas's 1st congressional district"
  },
  {
    "type": "constituency",
    "id": "ocd-division\/country:us\/state:ar\/cd:2",
    "name": "Arkansas's 2nd congressional district"
  },
  {
    "type": "constituency",
    "id": "ocd-division\/country:us\/state:ar\/cd:3",
    "name": "Arkansas's 3rd congressional district"
  },
  {
    "type": "constituency",
    "id": "ocd-division\/country:us\/state:ar\/cd:4",
    "name": "Arkansas's 4th congressional district"
  }
]
```
When you run a crawler using the built\-in JSON classifier, the entire file is used to define the schema\. Because you don’t specify a JSON path, the crawler treats the data as one object, that is, just an array\. For example, the schema might look like the following:  

```
root
|-- record: array
```
However, to create a schema that is based on each record in the JSON array, create a custom JSON classifier and specify the JSON path as `$[*]`\. When you specify this JSON path, the classifier interrogates all 12 records in the array to determine the schema\. The resulting schema contains separate fields for each object, similar to the following example:  

```
root
|-- type: string
|-- id: string
|-- name: string
```

**Example of Using a JSON Classifier to Examine Only Parts of a File**  
Suppose that your JSON data follows the pattern of the example JSON file `s3://awsglue-datasets/examples/us-legislators/all/areas.json` drawn from [http://everypolitician\.org/](http://everypolitician.org/)\. Example objects in the JSON file look like the following:  

```
{
  "type": "constituency",
  "id": "ocd-division\/country:us\/state:ak",
  "name": "Alaska"
}
{
  "type": "constituency",
  "identifiers": [
    {
      "scheme": "dmoz",
      "identifier": "Regional\/North_America\/United_States\/Alaska\/"
    },
    {
      "scheme": "freebase",
      "identifier": "\/m\/0hjy"
    },
    {
      "scheme": "fips",
      "identifier": "US02"
    },
    {
      "scheme": "quora",
      "identifier": "Alaska-state"
    },
    {
      "scheme": "britannica",
      "identifier": "place\/Alaska"
    },
    {
      "scheme": "wikidata",
      "identifier": "Q797"
    }
  ],
  "other_names": [
    {
      "lang": "en",
      "note": "multilingual",
      "name": "Alaska"
    },
    {
      "lang": "fr",
      "note": "multilingual",
      "name": "Alaska"
    },
    {
      "lang": "nov",
      "note": "multilingual",
      "name": "Alaska"
    }
  ],
  "id": "ocd-division\/country:us\/state:ak",
  "name": "Alaska"
}
```
When you run a crawler using the built\-in JSON classifier, the entire file is used to create the schema\. You might end up with a schema like this:  

```
root
|-- type: string
|-- id: string
|-- name: string
|-- identifiers: array
|    |-- element: struct
|    |    |-- scheme: string
|    |    |-- identifier: string
|-- other_names: array
|    |-- element: struct
|    |    |-- lang: string
|    |    |-- note: string
|    |    |-- name: string
```
However, to create a schema using just the "`id`" object, create a custom JSON classifier and specify the JSON path as `$.id`\. Then the schema is based on only the "`id`" field:  

```
root
|-- record: string
```
The first few lines of data extracted with this schema look like this:  

```
{"record": "ocd-division/country:us/state:ak"}
{"record": "ocd-division/country:us/state:al/cd:1"}
{"record": "ocd-division/country:us/state:al/cd:2"}
{"record": "ocd-division/country:us/state:al/cd:3"}
{"record": "ocd-division/country:us/state:al/cd:4"}
{"record": "ocd-division/country:us/state:al/cd:5"}
{"record": "ocd-division/country:us/state:al/cd:6"}
{"record": "ocd-division/country:us/state:al/cd:7"}
{"record": "ocd-division/country:us/state:ar/cd:1"}
{"record": "ocd-division/country:us/state:ar/cd:2"}
{"record": "ocd-division/country:us/state:ar/cd:3"}
{"record": "ocd-division/country:us/state:ar/cd:4"}
{"record": "ocd-division/country:us/state:as"}
{"record": "ocd-division/country:us/state:az/cd:1"}
{"record": "ocd-division/country:us/state:az/cd:2"}
{"record": "ocd-division/country:us/state:az/cd:3"}
{"record": "ocd-division/country:us/state:az/cd:4"}
{"record": "ocd-division/country:us/state:az/cd:5"}
{"record": "ocd-division/country:us/state:az/cd:6"}
{"record": "ocd-division/country:us/state:az/cd:7"}
```
To create a schema based on a deeply nested object, such as "`identifier`," in the JSON file, you can create a custom JSON classifier and specify the JSON path as `$.identifiers[*].identifier`\. Although the schema is similar to the previous example, it is based on a different object in the JSON file\.   
The schema looks like the following:  

```
root
|-- record: string
```
Listing the first few lines of data from the table shows that the schema is based on the data in the "`identifier`" object:  

```
{"record": "Regional/North_America/United_States/Alaska/"}
{"record": "/m/0hjy"}
{"record": "US02"}
{"record": "5879092"}
{"record": "4001016-8"}
{"record": "destination/alaska"}
{"record": "1116270"}
{"record": "139487266"}
{"record": "n79018447"}
{"record": "01490999-8dec-4129-8254-eef6e80fadc3"}
{"record": "Alaska-state"}
{"record": "place/Alaska"}
{"record": "Q797"}
{"record": "Regional/North_America/United_States/Alabama/"}
{"record": "/m/0gyh"}
{"record": "US01"}
{"record": "4829764"}
{"record": "4084839-5"}
{"record": "161950"}
{"record": "131885589"}
```
To create a table based on another deeply nested object, such as the "`name`" field in the "`other_names`" array in the JSON file, you can create a custom JSON classifier and specify the JSON path as `$.other_names[*].name`\. Although the schema is similar to the previous example, it is based on a different object in the JSON file\. The schema looks like the following:  

```
root
|-- record: string
```
Listing the first few lines of data in the table shows that it is based on the data in the "`name`" object in the "`other_names`" array:  

```
{"record": "Alaska"}
{"record": "Alaska"}
{"record": "Аляска"}
{"record": "Alaska"}
{"record": "Alaska"}
{"record": "Alaska"}
{"record": "Alaska"}
{"record": "Alaska"}
{"record": "Alaska"}
{"record": "ألاسكا"}
{"record": "ܐܠܐܣܟܐ"}
{"record": "الاسكا"}
{"record": "Alaska"}
{"record": "Alyaska"}
{"record": "Alaska"}
{"record": "Alaska"}
{"record": "Штат Аляска"}
{"record": "Аляска"}
{"record": "Alaska"}
{"record": "আলাস্কা"}
```

## Writing CSV Custom Classifiers<a name="custom-classifier-csv"></a>

You can use a custom CSV classifier to infer the schema of various types of CSV data\. The custom attributes that you can provide for your classifier include delimiters, options about the header, and whether to perform certain validations on the data\.

### Custom Classifier Values in AWS Glue<a name="classifier-values-csv"></a>

When you define a CSV classifier, you provide the following values to AWS Glue to create the classifier\. The classification field of this classifier is set to `csv`\.

**Name**  
Name of the classifier\.

**Column delimiter**  
A custom symbol to denote what separates each column entry in the row\.

**Quote symbol**  
A custom symbol to denote what combines content into a single column value\. Must be different from the column delimiter\.

**Column headings**  
Indicates the behavior for how column headings should be detected in the CSV file\. If your custom CSV file has column headings, enter a comma\-delimited list of the column headings\.

**Processing options: Allow files with single column**  
Enables the processing of files that contain only one column\.

**Processing options: Trim white space before identifying column values**  
Specifies whether to trim values before identifying the type of column values\.