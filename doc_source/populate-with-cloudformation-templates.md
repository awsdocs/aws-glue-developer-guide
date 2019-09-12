# Populating the Data Catalog Using AWS CloudFormation Templates<a name="populate-with-cloudformation-templates"></a>

AWS CloudFormation is a service that can create many AWS resources\. AWS Glue provides API operations to create objects in the AWS Glue Data Catalog\. However, it might be more convenient to define and create AWS Glue objects and other related AWS resource objects in an AWS CloudFormation template file\. Then you can automate the process of creating the objects\. 

AWS CloudFormation provides a simplified syntax—either JSON \(JavaScript Object Notation\) or YAML \(YAML Ain't Markup Language\)—to express the creation of AWS resources\. You can use AWS CloudFormation templates to define Data Catalog objects such as databases, tables, partitions, crawlers, classifiers, and connections\. You can also define ETL objects such as jobs, triggers, and development endpoints\. You create a template that describes all the AWS resources you want, and AWS CloudFormation takes care of provisioning and configuring those resources for you\.

For more information, see [What Is AWS CloudFormation?](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) and [Working with AWS CloudFormation Templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html) in the *AWS CloudFormation User Guide*\.

If you plan to use AWS CloudFormation templates that are compatible with AWS Glue, as an administrator, you must grant access to AWS CloudFormation and to the AWS services and actions on which it depends\. To grant permissions to create AWS CloudFormation resources, attach the following policy to the IAM users that work with AWS CloudFormation: 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [                
        "cloudformation:*"        
      ],
      "Resource": "*"
    }
  ]
}
```

The following table contains the actions that an AWS CloudFormation template can perform on your behalf\. It includes links to information about the AWS resource types and their property types that you can add to an AWS CloudFormation template\. 


| AWS Glue Resource | AWS CloudFormation Template | AWS Glue Samples | 
| --- | --- | --- | 
| Classifier | [AWS::Glue::Classifier](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-glue-classifier.html) | [Grok classifier](#sample-cfn-template-classifier), [JSON classifier](#sample-cfn-template-classifier-json), [XML classifier](#sample-cfn-template-classifier-xml) | 
| Connection | [AWS::Glue::Connection](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-glue-connection.html) | [MySQL connection](#sample-cfn-template-connection) | 
| Crawler | [AWS::Glue::Crawler](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-glue-crawler.html) | [Amazon S3 crawler](#sample-cfn-template-crawler-s3), [MySQL crawler](#sample-cfn-template-crawler-jdbc) | 
| Database | [AWS::Glue::Database](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-glue-database.html) | [Empty database](#sample-cfn-template-database), [Database with tables](#sample-cfn-template-db-table-partition)  | 
| Development endpoint | [AWS::Glue::DevEndpoint](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-glue-devendpoint.html) | [Development endpoint](#sample-cfn-template-devendpoint) | 
| Job | [AWS::Glue::Job](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-glue-job.html) | [Amazon S3 job](#sample-cfn-template-job-s3), [JDBC job](#sample-cfn-template-job-jdbc) | 
| Partition | [AWS::Glue::Partition](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-glue-partition.html) | [Partitions of a table](#sample-cfn-template-db-table-partition) | 
| Table | [AWS::Glue::Table](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-glue-table.html) | [Table in a database](#sample-cfn-template-db-table-partition) | 
| Trigger | [AWS::Glue::Trigger](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-glue-trigger.html) | [On\-demand trigger](#sample-cfn-template-trigger-ondemand), [Scheduled trigger](#sample-cfn-template-trigger-scheduled), [Conditional trigger](#sample-cfn-template-trigger-conditional)  | 

To get started, use the following sample templates and customize them with your own metadata\. Then use the AWS CloudFormation console to create an AWS CloudFormation stack to add objects to AWS Glue and any associated services\. Many fields in an AWS Glue object are optional\. These templates illustrate the fields that are required or are necessary for a working and functional AWS Glue object\. 

 An AWS CloudFormation template can be in either JSON or YAML format\. In these examples, YAML is used for easier readability\. The examples contain comments \(`#`\) to describe the values that are defined in the templates\. 

AWS CloudFormation templates can include a `Parameters` section\. This section can be changed in the sample text or when the YAML file is submitted to the AWS CloudFormation console to create a stack\. The `Resources` section of the template contains the definition of AWS Glue and related objects\. AWS CloudFormation template syntax definitions might contain properties that include more detailed property syntax\. Not all properties might be required to create an AWS Glue object\. These samples show example values for common properties to create an AWS Glue object\.

## Sample AWS CloudFormation Template for an AWS Glue Database<a name="sample-cfn-template-database"></a>

An AWS Glue database in the Data Catalog contains metadata tables\. The database consists of very few properties and can be created in the Data Catalog with an AWS CloudFormation template\. The following sample template is provided to get you started and to illustrate the use of AWS CloudFormation stacks with AWS Glue\. The only resource created by the sample template is a database named `cfn-mysampledatabase`\. You can change it by editing the text of the sample or changing the value on the AWS CloudFormation console when you submit the YAML\.

The following shows example values for common properties to create an AWS Glue database\. For more information about the AWS CloudFormation database template for AWS Glue, see [AWS::Glue::Database](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-glue-database.html)\.  

```
---
AWSTemplateFormatVersion: '2010-09-09'
# Sample CloudFormation template in YAML to demonstrate creating a database named mysampledatabase
# The metadata created in the Data Catalog points to the flights public S3 bucket
#
# Parameters section contains names that are substituted in the Resources section
# These parameters are the names the resources created in the Data Catalog
Parameters:
  CFNDatabaseName:
    Type: String
    Default: cfn-mysampledatabse

# Resources section defines metadata for the Data Catalog
Resources:
# Create an AWS Glue database
  CFNDatabaseFlights:
    Type: AWS::Glue::Database
    Properties:
      # The database is created in the Data Catalog for your account
      CatalogId: !Ref AWS::AccountId   
      DatabaseInput:
        # The name of the database is defined in the Parameters section above
        Name: !Ref CFNDatabaseName	
        Description: Database to hold tables for flights data
        LocationUri: s3://crawler-public-us-east-1/flight/2016/csv/
        #Parameters: Leave AWS database parameters blank
```

## Sample AWS CloudFormation Template for an AWS Glue Database, Table, and Partition<a name="sample-cfn-template-db-table-partition"></a>

An AWS Glue table contains the metadata that defines the structure and location of data that you want to process with your ETL scripts\. Within a table, you can define partitions to parallelize the processing of your data\. A partition is a chunk of data that you defined with a key\. For example, using month as a key, all the data for January is contained in the same partition\. In AWS Glue, databases can contain tables, and tables can contain partitions\.

The following sample shows how to populate a database, a table, and partitions using an AWS CloudFormation template\. The base data format is `csv` and delimited by a comma \(,\)\. Because a database must exist before it can contain a table, and a table must exist before partitions can be created, the template uses the `DependsOn` statement to define the dependency of these objects when they are created\.

The values in this sample define a table that contains flight data from a publicly available Amazon S3 bucket\. For illustration, only a few columns of the data and one partitioning key are defined\. Four partitions are also defined in the Data Catalog\. Some fields to describe the storage of the base data are also shown in the `StorageDescriptor` fields\.

```
---
AWSTemplateFormatVersion: '2010-09-09'
# Sample CloudFormation template in YAML to demonstrate creating a database, a table, and partitions
# The metadata created in the Data Catalog points to the flights public S3 bucket
#
# Parameters substituted in the Resources section
# These parameters are names of the resources created in the Data Catalog
Parameters:
  CFNDatabaseName:
    Type: String
    Default: cfn-database-flights-1
  CFNTableName1:
    Type: String
    Default: cfn-manual-table-flights-1
# Resources to create metadata in the Data Catalog
Resources:
###
# Create an AWS Glue database
  CFNDatabaseFlights:
    Type: AWS::Glue::Database
    Properties:
      CatalogId: !Ref AWS::AccountId
      DatabaseInput:
        Name: !Ref CFNDatabaseName	
        Description: Database to hold tables for flights data
###
# Create an AWS Glue table
  CFNTableFlights:
    # Creating the table waits for the database to be created
    DependsOn: CFNDatabaseFlights
    Type: AWS::Glue::Table
    Properties:
      CatalogId: !Ref AWS::AccountId
      DatabaseName: !Ref CFNDatabaseName
      TableInput:
        Name: !Ref CFNTableName1
        Description: Define the first few columns of the flights table
        TableType: EXTERNAL_TABLE
        Parameters: {
    "classification": "csv"
  }
#       ViewExpandedText: String
        PartitionKeys:
        # Data is partitioned by month
        - Name: mon
          Type: bigint
        StorageDescriptor:
          OutputFormat: org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat
          Columns:
          - Name: year
            Type: bigint
          - Name: quarter
            Type: bigint
          - Name: month
            Type: bigint
          - Name: day_of_month
            Type: bigint			
          InputFormat: org.apache.hadoop.mapred.TextInputFormat
          Location: s3://crawler-public-us-east-1/flight/2016/csv/
          SerdeInfo:
            Parameters:
              field.delim: ","
            SerializationLibrary: org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe
# Partition 1
# Create an AWS Glue partition  
  CFNPartitionMon1:
    DependsOn: CFNTableFlights
    Type: AWS::Glue::Partition
    Properties:
      CatalogId: !Ref AWS::AccountId
      DatabaseName: !Ref CFNDatabaseName
      TableName: !Ref CFNTableName1
      PartitionInput:
        Values:
        - 1
        StorageDescriptor:
          OutputFormat: org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat
          Columns:
          - Name: mon
            Type: bigint
          InputFormat: org.apache.hadoop.mapred.TextInputFormat
          Location: s3://crawler-public-us-east-1/flight/2016/csv/mon=1/
          SerdeInfo:
            Parameters:
              field.delim: ","
            SerializationLibrary: org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe
# Partition 2
# Create an AWS Glue partition 
  CFNPartitionMon2:
    DependsOn: CFNTableFlights
    Type: AWS::Glue::Partition
    Properties:
      CatalogId: !Ref AWS::AccountId
      DatabaseName: !Ref CFNDatabaseName
      TableName: !Ref CFNTableName1
      PartitionInput:
        Values:
        - 2
        StorageDescriptor:
          OutputFormat: org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat
          Columns:
          - Name: mon
            Type: bigint
          InputFormat: org.apache.hadoop.mapred.TextInputFormat
          Location: s3://crawler-public-us-east-1/flight/2016/csv/mon=2/
          SerdeInfo:
            Parameters:
              field.delim: ","
            SerializationLibrary: org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe
# Partition 3
# Create an AWS Glue partition 
  CFNPartitionMon3:
    DependsOn: CFNTableFlights
    Type: AWS::Glue::Partition
    Properties:
      CatalogId: !Ref AWS::AccountId
      DatabaseName: !Ref CFNDatabaseName
      TableName: !Ref CFNTableName1
      PartitionInput:
        Values:
        - 3
        StorageDescriptor:
          OutputFormat: org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat
          Columns:
          - Name: mon
            Type: bigint
          InputFormat: org.apache.hadoop.mapred.TextInputFormat
          Location: s3://crawler-public-us-east-1/flight/2016/csv/mon=3/
          SerdeInfo:
            Parameters:
              field.delim: ","
            SerializationLibrary: org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe
# Partition 4
# Create an AWS Glue partition 
  CFNPartitionMon4:
    DependsOn: CFNTableFlights
    Type: AWS::Glue::Partition
    Properties:
      CatalogId: !Ref AWS::AccountId
      DatabaseName: !Ref CFNDatabaseName
      TableName: !Ref CFNTableName1
      PartitionInput:
        Values:
        - 4
        StorageDescriptor:
          OutputFormat: org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat
          Columns:
          - Name: mon
            Type: bigint
          InputFormat: org.apache.hadoop.mapred.TextInputFormat
          Location: s3://crawler-public-us-east-1/flight/2016/csv/mon=4/
          SerdeInfo:
            Parameters:
              field.delim: ","
            SerializationLibrary: org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe
```

## Sample AWS CloudFormation Template for an AWS Glue Grok Classifier<a name="sample-cfn-template-classifier"></a>

An AWS Glue classifier determines the schema of your data\. One type of custom classifier uses a grok pattern to match your data\. If the pattern matches, then the custom classifier is used to create your table's schema and set the `classification` to the value set in the classifier definition\.

This sample creates a classifier that creates a schema with one column named `message` and sets the classification to `greedy`\.

```
---
AWSTemplateFormatVersion: '2010-09-09'
# Sample CFN YAML to demonstrate creating a classifier
#
# Parameters section contains names that are substituted in the Resources section
# These parameters are the names the resources created in the Data Catalog
Parameters:                                                                                                       
# The name of the classifier to be created
  CFNClassifierName:  
    Type: String
    Default: cfn-classifier-grok-one-column-1                                                               	
#
#
# Resources section defines metadata for the Data Catalog
Resources:
# Create classifier that uses grok pattern to put all data in one column and classifies it as "greedy".	
  CFNClassifierFlights:
    Type: AWS::Glue::Classifier   
    Properties:
      GrokClassifier:
        #Grok classifier that puts all data in one column		
        Name: !Ref CFNClassifierName
        Classification: greedy                                                        	   
        GrokPattern: "%{GREEDYDATA:message}"
        #CustomPatterns: none
```

## Sample AWS CloudFormation Template for an AWS Glue JSON Classifier<a name="sample-cfn-template-classifier-json"></a>

An AWS Glue classifier determines the schema of your data\. One type of custom classifier uses a `JsonPath` string defining the JSON data for the classifier to classify\. AWS Glue supports a subset of the operators for `JsonPath`, as described in [Writing JsonPath Custom Classifiers](https://docs.aws.amazon.com/glue/latest/dg/custom-classifier.html#custom-classifier-json)\. 

If the pattern matches, then the custom classifier is used to create your table's schema\.

This sample creates a classifier that creates a schema with each record in the `Records3` array in an object\.

```
---
AWSTemplateFormatVersion: '2010-09-09'
# Sample CFN YAML to demonstrate creating a JSON classifier
#
# Parameters section contains names that are substituted in the Resources section
# These parameters are the names the resources created in the Data Catalog
Parameters:                                                                                                       
# The name of the classifier to be created
  CFNClassifierName:  
    Type: String
    Default: cfn-classifier-json-one-column-1                                                               	
#
#
# Resources section defines metadata for the Data Catalog
Resources:
# Create classifier that uses a JSON pattern.	
  CFNClassifierFlights:
    Type: AWS::Glue::Classifier   
    Properties:
      JSONClassifier:
        #JSON classifier		
        Name: !Ref CFNClassifierName
        JsonPath: $.Records3[*]
```

## Sample AWS CloudFormation Template for an AWS Glue XML Classifier<a name="sample-cfn-template-classifier-xml"></a>

An AWS Glue classifier determines the schema of your data\. One type of custom classifier specifies an XML tag to designate the element that contains each record in an XML document that is being parsed\. If the pattern matches, then the custom classifier is used to create your table's schema and set the `classification` to the value set in the classifier definition\.

This sample creates a classifier that creates a schema with each record in the `Record` tag and sets the classification to `XML`\.

```
---
AWSTemplateFormatVersion: '2010-09-09'
# Sample CFN YAML to demonstrate creating an XML classifier
#
# Parameters section contains names that are substituted in the Resources section
# These parameters are the names the resources created in the Data Catalog
Parameters:                                                                                                       
# The name of the classifier to be created
  CFNClassifierName:  
    Type: String
    Default: cfn-classifier-xml-one-column-1                                                               	
#
#
# Resources section defines metadata for the Data Catalog
Resources:
# Create classifier that uses the XML pattern and classifies it as "XML".	
  CFNClassifierFlights:
    Type: AWS::Glue::Classifier   
    Properties:
      XMLClassifier:
        #XML classifier		
        Name: !Ref CFNClassifierName
        Classification: XML   
        RowTag: <Records>
```

## Sample AWS CloudFormation Template for an AWS Glue Crawler for Amazon S3<a name="sample-cfn-template-crawler-s3"></a>

An AWS Glue crawler creates metadata tables in your Data Catalog that correspond to your data\. You can then use these table definitions as sources and targets in your ETL jobs\.

This sample creates a crawler, the required IAM role, and an AWS Glue database in the Data Catalog\. When this crawler is run, it assumes the IAM role and creates a table in the database for the public flights data\. The table is created with the prefix "`cfn_sample_1_`"\. The IAM role created by this template allows global permissions; you might want to create a custom role\. No custom classifiers are defined by this classifier\. AWS Glue built\-in classifiers are used by default\. 

When you submit this sample to the AWS CloudFormation console, you must confirm that you want to create the IAM role\.

```
---
AWSTemplateFormatVersion: '2010-09-09'
# Sample CFN YAML to demonstrate creating a crawler
#
# Parameters section contains names that are substituted in the Resources section
# These parameters are the names the resources created in the Data Catalog
Parameters:                                                                                                       
# The name of the crawler to be created
  CFNCrawlerName:  
    Type: String
    Default: cfn-crawler-flights-1
  CFNDatabaseName:
    Type: String
    Default: cfn-database-flights-1
  CFNTablePrefixName:
    Type: String
    Default: cfn_sample_1_	
#
#
# Resources section defines metadata for the Data Catalog
Resources:
#Create IAM Role assumed by the crawler. For demonstration, this role is given all permissions.
  CFNRoleFlights:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          -
            Effect: "Allow"
            Principal:
              Service:
                - "glue.amazonaws.com"
            Action:
              - "sts:AssumeRole"
      Path: "/"
      Policies:
        -
          PolicyName: "root"
          PolicyDocument:
            Version: "2012-10-17"
            Statement:
              -
                Effect: "Allow"
                Action: "*"
                Resource: "*"
 # Create a database to contain tables created by the crawler
  CFNDatabaseFlights:
    Type: AWS::Glue::Database
    Properties:
      CatalogId: !Ref AWS::AccountId
      DatabaseInput:
        Name: !Ref CFNDatabaseName
        Description: "AWS Glue container to hold metadata tables for the flights crawler"
 #Create a crawler to crawl the flights data on a public S3 bucket
  CFNCrawlerFlights:
    Type: AWS::Glue::Crawler
    Properties:
      Name: !Ref CFNCrawlerName
      Role: !GetAtt CFNRoleFlights.Arn
      #Classifiers: none, use the default classifier
      Description: AWS Glue crawler to crawl flights data
      #Schedule: none, use default run-on-demand
      DatabaseName: !Ref CFNDatabaseName
      Targets:
        S3Targets:
          # Public S3 bucket with the flights data
          - Path: "s3://crawler-public-us-east-1/flight/2016/csv"
      TablePrefix: !Ref CFNTablePrefixName
      SchemaChangePolicy:
        UpdateBehavior: "UPDATE_IN_DATABASE"
        DeleteBehavior: "LOG"
      Configuration: "{\"Version\":1.0,\"CrawlerOutput\":{\"Partitions\":{\"AddOrUpdateBehavior\":\"InheritFromTable\"},\"Tables\":{\"AddOrUpdateBehavior\":\"MergeNewColumns\"}}}"
```

## Sample AWS CloudFormation Template for an AWS Glue Connection<a name="sample-cfn-template-connection"></a>

An AWS Glue connection in the Data Catalog contains the JDBC and network information that is required to connect to a JDBC database\. This information is used when you connect to a JDBC database to crawl or run ETL jobs\.

This sample creates a connection to an Amazon RDS MySQL database named `devdb`\. When this connection is used, an IAM role, database credentials, and network connection values must also be supplied\. See the details of necessary fields in the template\.

```
---
AWSTemplateFormatVersion: '2010-09-09'
# Sample CFN YAML to demonstrate creating a connection
#
# Parameters section contains names that are substituted in the Resources section
# These parameters are the names the resources created in the Data Catalog
Parameters:                                                                                                       
# The name of the connection to be created
  CFNConnectionName:  
    Type: String
    Default: cfn-connection-mysql-flights-1
  CFNJDBCString:  
    Type: String
    Default: "jdbc:mysql://xxx-mysql.yyyyyyyyyyyyyy.us-east-1.rds.amazonaws.com:3306/devdb"
  CFNJDBCUser:  
    Type: String
    Default: "master"
  CFNJDBCPassword:  
    Type: String
    Default: "12345678"
    NoEcho: true
#
#
# Resources section defines metadata for the Data Catalog
Resources:
  CFNConnectionMySQL:
    Type: AWS::Glue::Connection
    Properties:
      CatalogId: !Ref AWS::AccountId
      ConnectionInput: 
        Description: "Connect to MySQL database."
        ConnectionType: "JDBC"
        #MatchCriteria: none		
        PhysicalConnectionRequirements:
          AvailabilityZone: "us-east-1d"
          SecurityGroupIdList: 
           - "sg-7d52b812"
          SubnetId: "subnet-84f326ee" 
        ConnectionProperties: {
          "JDBC_CONNECTION_URL": !Ref CFNJDBCString,
          "USERNAME": !Ref CFNJDBCUser,
          "PASSWORD": !Ref CFNJDBCPassword
        }
        Name: !Ref CFNConnectionName
```

## Sample AWS CloudFormation Template for an AWS Glue Crawler for JDBC<a name="sample-cfn-template-crawler-jdbc"></a>

An AWS Glue crawler creates metadata tables in your Data Catalog that correspond to your data\. You can then use these table definitions as sources and targets in your ETL jobs\.

This sample creates a crawler, required IAM role, and an AWS Glue database in the Data Catalog\. When this crawler is run, it assumes the IAM role and creates a table in the database for the public flights data that has been stored in a MySQL database\. The table is created with the prefix "`cfn_jdbc_1_`"\. The IAM role created by this template allows global permissions; you might want to create a custom role\. No custom classifiers can be defined for JDBC data\. AWS Glue built\-in classifiers are used by default\. 

When you submit this sample to the AWS CloudFormation console, you must confirm that you want to create the IAM role\.

```
---
AWSTemplateFormatVersion: '2010-09-09'
# Sample CFN YAML to demonstrate creating a crawler
#
# Parameters section contains names that are substituted in the Resources section
# These parameters are the names the resources created in the Data Catalog
Parameters:                                                                                                       
# The name of the crawler to be created
  CFNCrawlerName:  
    Type: String
    Default: cfn-crawler-jdbc-flights-1
# The name of the database to be created to contain tables	
  CFNDatabaseName:
    Type: String
    Default: cfn-database-jdbc-flights-1
# The prefix for all tables crawled and created	
  CFNTablePrefixName:
    Type: String
    Default: cfn_jdbc_1_
# The name of the existing connection to the MySQL database
  CFNConnectionName:  
    Type: String
    Default: cfn-connection-mysql-flights-1
# The name of the JDBC path (database/schema/table) with wildcard (%) to crawl	
  CFNJDBCPath:  
    Type: String
    Default: saldev/%		
#
#
# Resources section defines metadata for the Data Catalog
Resources:
#Create IAM Role assumed by the crawler. For demonstration, this role is given all permissions.
  CFNRoleFlights:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          -
            Effect: "Allow"
            Principal:
              Service:
                - "glue.amazonaws.com"
            Action:
              - "sts:AssumeRole"
      Path: "/"
      Policies:
        -
          PolicyName: "root"
          PolicyDocument:
            Version: "2012-10-17"
            Statement:
              -
                Effect: "Allow"
                Action: "*"
                Resource: "*"
 # Create a database to contain tables created by the crawler
  CFNDatabaseFlights:
    Type: AWS::Glue::Database
    Properties:
      CatalogId: !Ref AWS::AccountId
      DatabaseInput:
        Name: !Ref CFNDatabaseName
        Description: "AWS Glue container to hold metadata tables for the flights crawler"
 #Create a crawler to crawl the flights data in MySQL database
  CFNCrawlerFlights:
    Type: AWS::Glue::Crawler
    Properties:
      Name: !Ref CFNCrawlerName
      Role: !GetAtt CFNRoleFlights.Arn
      #Classifiers: none, use the default classifier
      Description: AWS Glue crawler to crawl flights data
      #Schedule: none, use default run-on-demand
      DatabaseName: !Ref CFNDatabaseName
      Targets:
        JdbcTargets:
          # JDBC MySQL database with the flights data
          - ConnectionName: !Ref CFNConnectionName
            Path: !Ref CFNJDBCPath
          #Exclusions: none
      TablePrefix: !Ref CFNTablePrefixName
      SchemaChangePolicy:
        UpdateBehavior: "UPDATE_IN_DATABASE"
        DeleteBehavior: "LOG"
	  Configuration: "{\"Version\":1.0,\"CrawlerOutput\":{\"Partitions\":{\"AddOrUpdateBehavior\":\"InheritFromTable\"},\"Tables\":{\"AddOrUpdateBehavior\":\"MergeNewColumns\"}}}"
```

## Sample AWS CloudFormation Template for an AWS Glue Job for Amazon S3 to Amazon S3<a name="sample-cfn-template-job-s3"></a>

An AWS Glue job in the Data Catalog contains the parameter values that are required to run a script in AWS Glue\.

This sample creates a job that reads flight data from an Amazon S3 bucket in `csv` format and writes it to an Amazon S3 Parquet file\. The script that is run by this job must already exist\. You can generate an ETL script for your environment with the AWS Glue console\. When this job is run, an IAM role with the correct permissions must also be supplied\.

Common parameter values are shown in the template\. For example, `AllocatedCapacity` \(DPUs\) defaults to 5\.

```
---
AWSTemplateFormatVersion: '2010-09-09'
# Sample CFN YAML to demonstrate creating a job using the public flights S3 table in a public bucket
#
# Parameters section contains names that are substituted in the Resources section
# These parameters are the names the resources created in the Data Catalog
Parameters:                                                                                                       
# The name of the job to be created
  CFNJobName:  
    Type: String
    Default: cfn-job-S3-to-S3-2
# The name of the IAM role that the job assumes. It must have access to data, script, temporary directory
  CFNIAMRoleName:  
    Type: String
    Default: AWSGlueServiceRoleGA
# The S3 path where the script for this job is located
  CFNScriptLocation:  
    Type: String
    Default: s3://aws-glue-scripts-123456789012-us-east-1/myid/sal-job-test2	
#
#
# Resources section defines metadata for the Data Catalog
Resources:                                      
# Create job to run script which accesses flightscsv table and write to S3 file as parquet.
# The script already exists and is called by this job	
  CFNJobFlights:
    Type: AWS::Glue::Job   
    Properties:
      Role: !Ref CFNIAMRoleName  
      #DefaultArguments: JSON object 
      # If script written in Scala, then set DefaultArguments={'--job-language'; 'scala', '--class': 'your scala class'}
      #Connections:  No connection needed for S3 to S3 job 
      #  ConnectionsList  
      #MaxRetries: Double  
      Description: Job created with CloudFormation  
      #LogUri: String  
      Command:   
        Name: glueetl  
        ScriptLocation: !Ref CFNScriptLocation
             # for access to directories use proper IAM role with permission to buckets and folders that begin with "aws-glue-"					 
             # script uses temp directory from job definition if required (temp directory not used S3 to S3)
             # script defines target for output as s3://aws-glue-target/sal    			 
      AllocatedCapacity: 5  
      ExecutionProperty:   
        MaxConcurrentRuns: 1  
      Name: !Ref CFNJobName
```

## Sample AWS CloudFormation Template for an AWS Glue Job for JDBC to Amazon S3<a name="sample-cfn-template-job-jdbc"></a>

An AWS Glue job in the Data Catalog contains the parameter values that are required to run a script in AWS Glue\.

This sample creates a job that reads flight data from a MySQL JDBC database as defined by the connection named `cfn-connection-mysql-flights-1` and writes it to an Amazon S3 Parquet file\. The script that is run by this job must already exist\. You can generate an ETL script for your environment with the AWS Glue console\. When this job is run, an IAM role with the correct permissions must also be supplied\.

Common parameter values are shown in the template\. For example, `AllocatedCapacity` \(DPUs\) defaults to 5\.

```
---
AWSTemplateFormatVersion: '2010-09-09'
# Sample CFN YAML to demonstrate creating a job using a MySQL JDBC DB with the flights data to an S3 file
#
# Parameters section contains names that are substituted in the Resources section
# These parameters are the names the resources created in the Data Catalog
Parameters:                                                                                                       
# The name of the job to be created
  CFNJobName:  
    Type: String
    Default: cfn-job-JDBC-to-S3-1
# The name of the IAM role that the job assumes. It must have access to data, script, temporary directory
  CFNIAMRoleName:  
    Type: String
    Default: AWSGlueServiceRoleGA
# The S3 path where the script for this job is located
  CFNScriptLocation:  
    Type: String
    Default: s3://aws-glue-scripts-123456789012-us-east-1/myid/sal-job-dec4a	
# The name of the connection used for JDBC data source
  CFNConnectionName:  
    Type: String
    Default: cfn-connection-mysql-flights-1
#
#
# Resources section defines metadata for the Data Catalog
Resources:                                      
# Create job to run script which accesses JDBC flights table via a connection and write to S3 file as parquet.
# The script already exists and is called by this job	
  CFNJobFlights:
    Type: AWS::Glue::Job   
    Properties:
      Role: !Ref CFNIAMRoleName  
      #DefaultArguments: JSON object  
      # For example, if required by script, set temporary directory as DefaultArguments={'--TempDir'; 's3://aws-glue-temporary-xyc/sal'}
      Connections:
        Connections:
        - !Ref CFNConnectionName 
      #MaxRetries: Double  
      Description: Job created with CloudFormation using existing script
      #LogUri: String  
      Command:   
        Name: glueetl  
        ScriptLocation: !Ref CFNScriptLocation
             # for access to directories use proper IAM role with permission to buckets and folders that begin with "aws-glue-"					 
             # if required, script defines temp directory as argument TempDir and used in script like redshift_tmp_dir = args["TempDir"] 
             # script defines target for output as s3://aws-glue-target/sal    			 
      AllocatedCapacity: 5  
      ExecutionProperty:   
        MaxConcurrentRuns: 1  
      Name: !Ref CFNJobName
```

## Sample AWS CloudFormation Template for an AWS Glue On\-Demand Trigger<a name="sample-cfn-template-trigger-ondemand"></a>

An AWS Glue trigger in the Data Catalog contains the parameter values that are required to start a job run when the trigger fires\. An on\-demand trigger fires when you enable it\.

This sample creates an on\-demand trigger that starts one job named `cfn-job-S3-to-S3-1`\. 

```
---
AWSTemplateFormatVersion: '2010-09-09'
# Sample CFN YAML to demonstrate creating an on-demand trigger
#
# Parameters section contains names that are substituted in the Resources section
# These parameters are the names the resources created in the Data Catalog
Parameters:
  # The existing job to be started by this trigger 
  CFNJobName:
    Type: String
    Default: cfn-job-S3-to-S3-1
  # The name of the trigger to be created
  CFNTriggerName:
    Type: String
    Default: cfn-trigger-ondemand-flights-1	
#
# Resources section defines metadata for the Data Catalog
# Sample CFN YAML to demonstrate creating an on-demand trigger for a job	
Resources:                                      
# Create trigger to run an existing job (CFNJobName) on an on-demand schedule.	
  CFNTriggerSample:
    Type: AWS::Glue::Trigger   
    Properties:
      Name:
        Ref: CFNTriggerName		
      Description: Trigger created with CloudFormation
      Type: ON_DEMAND                                                        	   
      Actions:
        - JobName: !Ref CFNJobName                	  
        # Arguments: JSON object
      #Schedule: 
      #Predicate:
```

## Sample AWS CloudFormation Template for an AWS Glue Scheduled Trigger<a name="sample-cfn-template-trigger-scheduled"></a>

An AWS Glue trigger in the Data Catalog contains the parameter values that are required to start a job run when the trigger fires\. A scheduled trigger fires when it is enabled and the cron timer pops\.

This sample creates a scheduled trigger that starts one job named `cfn-job-S3-to-S3-1`\. The timer is a cron expression to run the job every 10 minutes on weekdays\.

```
---
AWSTemplateFormatVersion: '2010-09-09'
# Sample CFN YAML to demonstrate creating a scheduled trigger
#
# Parameters section contains names that are substituted in the Resources section
# These parameters are the names the resources created in the Data Catalog
Parameters:
  # The existing job to be started by this trigger 
  CFNJobName:
    Type: String
    Default: cfn-job-S3-to-S3-1
  # The name of the trigger to be created
  CFNTriggerName:
    Type: String
    Default: cfn-trigger-scheduled-flights-1	
#
# Resources section defines metadata for the Data Catalog
# Sample CFN YAML to demonstrate creating a scheduled trigger for a job
#	
Resources:                                      
# Create trigger to run an existing job (CFNJobName) on a cron schedule.	
  TriggerSample1CFN:
    Type: AWS::Glue::Trigger   
    Properties:
      Name:
        Ref: CFNTriggerName		
      Description: Trigger created with CloudFormation
      Type: SCHEDULED                                                        	   
      Actions:
        - JobName: !Ref CFNJobName                	  
        # Arguments: JSON object
      # # Run the trigger every 10 minutes on Monday to Friday 		
      Schedule: cron(0/10 * ? * MON-FRI *) 
      #Predicate:
```

## Sample AWS CloudFormation Template for an AWS Glue Conditional Trigger<a name="sample-cfn-template-trigger-conditional"></a>

An AWS Glue trigger in the Data Catalog contains the parameter values that are required to start a job run when the trigger fires\. A conditional trigger fires when it is enabled and its conditions are met, such as a job completing successfully\.

This sample creates a conditional trigger that starts one job named `cfn-job-S3-to-S3-1`\. This job starts when the job named `cfn-job-S3-to-S3-2 ` completes successfully\.

```
---
AWSTemplateFormatVersion: '2010-09-09'
# Sample CFN YAML to demonstrate creating a conditional trigger for a job, which starts when another job completes
#
# Parameters section contains names that are substituted in the Resources section
# These parameters are the names the resources created in the Data Catalog
Parameters:
  # The existing job to be started by this trigger 
  CFNJobName:
    Type: String
    Default: cfn-job-S3-to-S3-1
  # The existing job that when it finishes causes trigger to fire
  CFNJobName2:
    Type: String
    Default: cfn-job-S3-to-S3-2	
  # The name of the trigger to be created
  CFNTriggerName:
    Type: String
    Default: cfn-trigger-conditional-1	
#	
Resources:                                      
# Create trigger to run an existing job (CFNJobName) when another job completes (CFNJobName2).	
  CFNTriggerSample:
    Type: AWS::Glue::Trigger   
    Properties:
      Name:
        Ref: CFNTriggerName		
      Description: Trigger created with CloudFormation
      Type: CONDITIONAL                                                        	   
      Actions:
        - JobName: !Ref CFNJobName                	  
        # Arguments: JSON object
      #Schedule: none 
      Predicate:
        #Value for Logical is required if more than 1 job listed in Conditions	  
        Logical: AND
        Conditions:
          - LogicalOperator: EQUALS	
            JobName: !Ref CFNJobName2
            State: SUCCEEDED
```

## Sample AWS CloudFormation Template for an AWS Glue Development Endpoint<a name="sample-cfn-template-devendpoint"></a>

An AWS Glue development endpoint is an environment that you can use to develop and test your AWS Glue scripts\.

This sample creates a development endpoint with the minimal network parameter values required to successfully create it\. For more information about the parameters that you need to set up a development endpoint, see [Setting Up Your Environment for Development Endpoints](start-development-endpoint.md)\.

You provide an existing IAM role ARN \(Amazon Resource Name\) to create the development endpoint\. Supply a valid RSA public key and keep the corresponding private key available if you plan to create a notebook server on the development endpoint\.

**Note**  
For any notebook server that you create that is associated with a development endpoint, you manage it\. Therefore, if you delete the development endpoint, to delete the notebook server, you must delete the AWS CloudFormation stack on the AWS CloudFormation console\.

```
---
AWSTemplateFormatVersion: '2010-09-09'
# Sample CFN YAML to demonstrate creating a development endpoint
#
# Parameters section contains names that are substituted in the Resources section
# These parameters are the names the resources created in the Data Catalog
Parameters:                                                                                                       
# The name of the crawler to be created
  CFNEndpointName:  
    Type: String
    Default: cfn-devendpoint-1
  CFNIAMRoleArn:
    Type: String
    Default: arn:aws:iam::123456789012/role/AWSGlueServiceRoleGA	
#
#
# Resources section defines metadata for the Data Catalog
Resources:
  CFNDevEndpoint:
    Type: AWS::Glue::DevEndpoint
    Properties:
      EndpointName: !Ref CFNEndpointName
      #ExtraJarsS3Path: String
      #ExtraPythonLibsS3Path: String
      NumberOfNodes: 5
      PublicKey: ssh-rsa public.....key myuserid-key
      RoleArn: !Ref CFNIAMRoleArn
      SecurityGroupIds: 
        - sg-64986c0b
      SubnetId: subnet-c67cccac
```