# Reading Input Files in Larger Groups<a name="grouping-input-files"></a>

You can set properties of your tables to enable an AWS Glue ETL job to group files when they are read from an Amazon S3 data store\. These properties enable each ETL task to read a group of input files into a single in\-memory partition, this is especially useful when there is a large number of small files in your Amazon S3 data store\. When you set certain properties, you instruct AWS Glue to group files within an Amazon S3 data partition and set the size of the groups to be read\. You can also set these options when reading from an Amazon S3 data store with the `create_dynamic_frame_from_options` method\. 

To enable grouping files for a table, you set key\-value pairs in the parameters field of your table structure\. Use JSON notation to set a value for the parameter field of your table\. For more information about editing the properties of a table, see [Viewing and Editing Table Details](console-tables.md#console-tables-details)\. 

You can use this method to enable grouping for tables in the Data Catalog with Amazon S3 data stores\. 

**groupFiles**  
Set **groupFiles** to `inPartition` to enable the grouping of files within an Amazon S3 data partition\. AWS Glue automatically enables grouping if there are more than 50,000 input files\. For example:  

```
  'groupFiles': 'inPartition'
```

**groupSize**  
Set **groupSize** to the target size of groups in bytes\. The **groupSize** property is optional, if not provided, AWS Glue calculates a size to use all the CPU cores in the cluster while still reducing the overall number of ETL tasks and in\-memory partitions\.   
For example, to set the group size to 1 MB:  

```
  'groupSize': '1048576'
```
Note that the `groupsize` should be set with the result of a calculation\. For example 1024 \* 1024 = 1048576\.

**recurse**  
Set **recurse** to `True` to recursively read files in all subdirectories when specifying `paths` as an array of paths\. You do not need to set **recurse** if `paths` is an array of object keys in Amazon S3\. For example:  

```
  'recurse':True
```

If you are reading from Amazon S3 directly using the `create_dynamic_frame_from_options` method, add these connection options\. For example, the following attempts to group files into 1 MB groups:

```
df = glueContext.create_dynamic_frame_from_options("s3", {'paths': ["s3://s3path/"], 'recurse':True, 'groupFiles': 'inPartition', 'groupSize': '1048576'}, format="json")
```