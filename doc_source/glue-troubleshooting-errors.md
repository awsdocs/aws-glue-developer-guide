# Troubleshooting Errors in AWS Glue<a name="glue-troubleshooting-errors"></a>

If you encounter errors in AWS Glue, use the following solutions to help you find the source of the problems and fix them\.

**Note**  
 The AWS Glue GitHub repository contains additional troubleshooting guidance in [ AWS Glue Frequently Asked Questions](https://github.com/awslabs/aws-glue-samples/blob/master/FAQ_and_How_to.md)\. 

**Topics**
+ [Error: Resource Unavailable](#error-resource-unavailable)
+ [Error: Could Not Find S3 Endpoint or NAT Gateway for subnetId in VPC](#error-s3-subnet-vpc-NAT-configuration)
+ [Error: Inbound Rule in Security Group Required](#error-inbound-self-reference-rule)
+ [Error: Outbound Rule in Security Group Required](#error-outbound-self-reference-rule)
+ [Error: Job Run Failed Because the Role Passed Should Be Given Assume Role Permissions for the AWS Glue Service](#error-assume-role-user-policy)
+ [Error: DescribeVpcEndpoints Action Is Unauthorized\. Unable to Validate VPC ID vpc\-id](#error-DescribeVpcEndpoints-permission)
+ [Error: DescribeRouteTables Action Is Unauthorized\. Unable to Validate Subnet Id: subnet\-id in VPC id: vpc\-id](#error-DescribeRouteTables-permission)
+ [Error: Failed to Call ec2:DescribeSubnets](#error-DescribeSubnets-permission)
+ [Error: Failed to Call ec2:DescribeSecurityGroups](#error-DescribeSecurityGroups-permission)
+ [Error: Could Not Find Subnet for AZ](#error-az-not-available)
+ [Error: Job Run Exception When Writing to a JDBC Target](#error-job-run-jdbc-target)
+ [Error: Amazon S3 Timeout](#error-s3-timeout)
+ [Error: Amazon S3 Access Denied](#error-s3-access-denied)
+ [Error: Amazon S3 Access Key ID Does Not Exist](#error-s3-accesskeyid-not-found)
+ [Error: Job Run Fails When Accessing Amazon S3 with an `s3a://` URI](#error-s3a-uri-directory-listing)
+ [Error: Amazon S3 Service Token Expired](#error-s3-service-token-expired)
+ [Error: No Private DNS for Network Interface Found](#error-no-private-DNS)
+ [Error: Development Endpoint Provisioning Failed](#error-development-endpoint-failed)
+ [Error: Notebook Server CREATE\_FAILED](#error-notebook-server-ec2-instance-profile)
+ [Error: Local Notebook Fails to Start](#error-local-notebook-fails-to-start)
+ [Error: Notebook Usage Errors](#error-notebook-usage-errors)
+ [Error: Running Crawler Failed](#error-running-crawler-failed)
+ [Error: Upgrading Athena Data Catalog](#error-running-athena-upgrade)
+ [Error: A Job is Reprocessing Data When Job Bookmarks Are Enabled](#error-job-bookmarks-reprocess-data)

## Error: Resource Unavailable<a name="error-resource-unavailable"></a>

If AWS Glue returns a resource unavailable message, you can view error messages or logs to help you learn more about the issue\. The following tasks describe general methods for troubleshooting\.
+ For any connections and development endpoints that you use, check that your cluster has not run out of elastic network interfaces\.

## Error: Could Not Find S3 Endpoint or NAT Gateway for subnetId in VPC<a name="error-s3-subnet-vpc-NAT-configuration"></a>

Check the subnet ID and VPC ID in the message to help you diagnose the issue\.
+ Check that you have an Amazon S3 VPC endpoint set up, which is required with AWS Glue\. In addition, check your NAT gateway if that's part of your configuration\. For more information, see [Amazon VPC Endpoints for Amazon S3](vpc-endpoints-s3.md)\.

## Error: Inbound Rule in Security Group Required<a name="error-inbound-self-reference-rule"></a>

At least one security group must open all ingress ports\. To limit traffic, the source security group in your inbound rule can be restricted to the same security group\.
+ For any connections that you use, check your security group for an inbound rule that is self\-referencing\. For more information, see [Setting Up Your Environment to Access Data Stores](start-connecting.md)\.
+ When you are using a development endpoint, check your security group for an inbound rule that is self\-referencing\. For more information, see [Setting Up Your Environment to Access Data Stores](start-connecting.md)\.

## Error: Outbound Rule in Security Group Required<a name="error-outbound-self-reference-rule"></a>

At least one security group must open all egress ports\. To limit traffic, the source security group in your outbound rule can be restricted to the same security group\.
+ For any connections that you use, check your security group for an outbound rule that is self\-referencing\. For more information, see [Setting Up Your Environment to Access Data Stores](start-connecting.md)\.
+ When you are using a development endpoint, check your security group for an outbound rule that is self\-referencing\. For more information, see [Setting Up Your Environment to Access Data Stores](start-connecting.md)\.

## Error: Job Run Failed Because the Role Passed Should Be Given Assume Role Permissions for the AWS Glue Service<a name="error-assume-role-user-policy"></a>

The user who defines a job must have permission for `iam:PassRole` for AWS Glue\.
+ When a user creates an AWS Glue job, confirm that the user's role contains a policy that contains `iam:PassRole` for AWS Glue\. For more information, see [Step 3: Attach a Policy to IAM Users That Access AWS Glue](attach-policy-iam-user.md)\.

## Error: DescribeVpcEndpoints Action Is Unauthorized\. Unable to Validate VPC ID vpc\-id<a name="error-DescribeVpcEndpoints-permission"></a>
+ Check the policy passed to AWS Glue for the `ec2:DescribeVpcEndpoints` permission\.

## Error: DescribeRouteTables Action Is Unauthorized\. Unable to Validate Subnet Id: subnet\-id in VPC id: vpc\-id<a name="error-DescribeRouteTables-permission"></a>
+ Check the policy passed to AWS Glue for the `ec2:DescribeRouteTables` permission\.

## Error: Failed to Call ec2:DescribeSubnets<a name="error-DescribeSubnets-permission"></a>
+ Check the policy passed to AWS Glue for the `ec2:DescribeSubnets` permission\.

## Error: Failed to Call ec2:DescribeSecurityGroups<a name="error-DescribeSecurityGroups-permission"></a>
+ Check the policy passed to AWS Glue for the `ec2:DescribeSecurityGroups` permission\.

## Error: Could Not Find Subnet for AZ<a name="error-az-not-available"></a>
+ The Availability Zone might not be available to AWS Glue\. Create and use a new subnet in a different Availability Zone from the one specified in the message\.

## Error: Job Run Exception When Writing to a JDBC Target<a name="error-job-run-jdbc-target"></a>

When you are running a job that writes to a JDBC target, the job might encounter errors in the following scenarios:
+ If your job writes to a Microsoft SQL Server table, and the table has columns defined as type `Boolean`, then the table must be predefined in the SQL Server database\. When you define the job on the AWS Glue console using a SQL Server target with the option **Create tables in your data target**, don't map any source columns to a target column with data type `Boolean`\. You might encounter an error when the job runs\.

  You can avoid the error by doing the following:
  + Choose an existing table with the **Boolean** column\.
  + Edit the `ApplyMapping` transform and map the **Boolean** column in the source to a number or string in the target\.
  + Edit the `ApplyMapping` transform to remove the **Boolean** column from the source\.
+ If your job writes to an Oracle table, you might need to adjust the length of names of Oracle objects\. In some versions of Oracle, the maximum identifier length is limited to 30 bytes or 128 bytes\. This limit affects the table names and column names of Oracle target data stores\.

  You can avoid the error by doing the following:
  + Name Oracle target tables within the limit for your version\.
  + The default column names are generated from the field names in the data\. To handle the case when the column names are longer than the limit, use `ApplyMapping` or `RenameField` transforms to change the name of the column to be within the limit\.

## Error: Amazon S3 Timeout<a name="error-s3-timeout"></a>

If AWS Glue returns a connect timed out error, it might be because it is trying to access an Amazon S3 bucket in another AWS Region\. 
+ An Amazon S3 VPC endpoint can only route traffic to buckets within an AWS Region\. If you need to connect to buckets in other Regions, a possible workaround is to use a NAT gateway\. For more information, see [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)\.

## Error: Amazon S3 Access Denied<a name="error-s3-access-denied"></a>

If AWS Glue returns an access denied error to an Amazon S3 bucket or object, it might be because the IAM role provided does not have a policy with permission to your data store\.
+ An ETL job must have access to an Amazon S3 data store used as a source or target\. A crawler must have access to an Amazon S3 data store that it crawls\. For more information, see [Step 2: Create an IAM Role for AWS Glue](create-an-iam-role.md)\.

## Error: Amazon S3 Access Key ID Does Not Exist<a name="error-s3-accesskeyid-not-found"></a>

If AWS Glue returns an access key ID does not exist error when running a job, it might be because of one of the following reasons:
+ An ETL job uses an IAM role to access data stores, confirm that the IAM role for your job was not deleted before the job started\.
+ An IAM role contains permissions to access your data stores, confirm that any attached Amazon S3 policy containing `s3:ListBucket` is correct\.

## Error: Job Run Fails When Accessing Amazon S3 with an `s3a://` URI<a name="error-s3a-uri-directory-listing"></a>

If a job run returns an error like *Failed to parse XML document with handler class *, it might be because of a failure trying to list hundreds of files using an `s3a://` URI\. Access your data store using an `s3://` URI instead\. The following exception trace highlights the errors to look for:

```
1.	com.amazonaws.SdkClientException: Failed to parse XML document with handler class com.amazonaws.services.s3.model.transform.XmlResponsesSaxParser$ListBucketHandler
2.	at com.amazonaws.services.s3.model.transform.XmlResponsesSaxParser.parseXmlInputStream(XmlResponsesSaxParser.java:161)
3.	at com.amazonaws.services.s3.model.transform.XmlResponsesSaxParser.parseListBucketObjectsResponse(XmlResponsesSaxParser.java:317)
4.	at com.amazonaws.services.s3.model.transform.Unmarshallers$ListObjectsUnmarshaller.unmarshall(Unmarshallers.java:70)
5.	at com.amazonaws.services.s3.model.transform.Unmarshallers$ListObjectsUnmarshaller.unmarshall(Unmarshallers.java:59)
6.	at com.amazonaws.services.s3.internal.S3XmlResponseHandler.handle(S3XmlResponseHandler.java:62)
7.	at com.amazonaws.services.s3.internal.S3XmlResponseHandler.handle(S3XmlResponseHandler.java:31)
8.	at com.amazonaws.http.response.AwsResponseHandlerAdapter.handle(AwsResponseHandlerAdapter.java:70)
9.	at com.amazonaws.http.AmazonHttpClient$RequestExecutor.handleResponse(AmazonHttpClient.java:1554)
10.	at com.amazonaws.http.AmazonHttpClient$RequestExecutor.executeOneRequest(AmazonHttpClient.java:1272)
11.	at com.amazonaws.http.AmazonHttpClient$RequestExecutor.executeHelper(AmazonHttpClient.java:1056)
12.	at com.amazonaws.http.AmazonHttpClient$RequestExecutor.doExecute(AmazonHttpClient.java:743)
13.	at com.amazonaws.http.AmazonHttpClient$RequestExecutor.executeWithTimer(AmazonHttpClient.java:717)
14.	at com.amazonaws.http.AmazonHttpClient$RequestExecutor.execute(AmazonHttpClient.java:699)
15.	at com.amazonaws.http.AmazonHttpClient$RequestExecutor.access$500(AmazonHttpClient.java:667)
16.	at com.amazonaws.http.AmazonHttpClient$RequestExecutionBuilderImpl.execute(AmazonHttpClient.java:649)
17.	at com.amazonaws.http.AmazonHttpClient.execute(AmazonHttpClient.java:513)
18.	at com.amazonaws.services.s3.AmazonS3Client.invoke(AmazonS3Client.java:4325)
19.	at com.amazonaws.services.s3.AmazonS3Client.invoke(AmazonS3Client.java:4272)
20.	at com.amazonaws.services.s3.AmazonS3Client.invoke(AmazonS3Client.java:4266)
21.	at com.amazonaws.services.s3.AmazonS3Client.listObjects(AmazonS3Client.java:834)
22.	at org.apache.hadoop.fs.s3a.S3AFileSystem.getFileStatus(S3AFileSystem.java:971)
23.	at org.apache.hadoop.fs.s3a.S3AFileSystem.deleteUnnecessaryFakeDirectories(S3AFileSystem.java:1155)
24.	at org.apache.hadoop.fs.s3a.S3AFileSystem.finishedWrite(S3AFileSystem.java:1144)
25.	at org.apache.hadoop.fs.s3a.S3AOutputStream.close(S3AOutputStream.java:142)
26.	at org.apache.hadoop.fs.FSDataOutputStream$PositionCache.close(FSDataOutputStream.java:74)
27.	at org.apache.hadoop.fs.FSDataOutputStream.close(FSDataOutputStream.java:108)
28.	at org.apache.parquet.hadoop.ParquetFileWriter.end(ParquetFileWriter.java:467)
29.	at org.apache.parquet.hadoop.InternalParquetRecordWriter.close(InternalParquetRecordWriter.java:117)
30.	at org.apache.parquet.hadoop.ParquetRecordWriter.close(ParquetRecordWriter.java:112)
31.	at org.apache.spark.sql.execution.datasources.parquet.ParquetOutputWriter.close(ParquetOutputWriter.scala:44)
32.	at org.apache.spark.sql.execution.datasources.FileFormatWriter$SingleDirectoryWriteTask.releaseResources(FileFormatWriter.scala:252)
33.	at org.apache.spark.sql.execution.datasources.FileFormatWriter$$anonfun$org$apache$spark$sql$execution$datasources$FileFormatWriter$$executeTask$3.apply(FileFormatWriter.scala:191)
34.	at org.apache.spark.sql.execution.datasources.FileFormatWriter$$anonfun$org$apache$spark$sql$execution$datasources$FileFormatWriter$$executeTask$3.apply(FileFormatWriter.scala:188)
35.	at org.apache.spark.util.Utils$.tryWithSafeFinallyAndFailureCallbacks(Utils.scala:1341)
36.	at org.apache.spark.sql.execution.datasources.FileFormatWriter$.org$apache$spark$sql$execution$datasources$FileFormatWriter$$executeTask(FileFormatWriter.scala:193)
37.	at org.apache.spark.sql.execution.datasources.FileFormatWriter$$anonfun$write$1$$anonfun$3.apply(FileFormatWriter.scala:129)
38.	at org.apache.spark.sql.execution.datasources.FileFormatWriter$$anonfun$write$1$$anonfun$3.apply(FileFormatWriter.scala:128)
39.	at org.apache.spark.scheduler.ResultTask.runTask(ResultTask.scala:87)
40.	at org.apache.spark.scheduler.Task.run(Task.scala:99)
41.	at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:282)
42.	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
43.	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
44.	at java.lang.Thread.run(Thread.java:748)
```

## Error: Amazon S3 Service Token Expired<a name="error-s3-service-token-expired"></a>

When moving data to and from Amazon Redshift, temporary Amazon S3 credentials, which expire after 1 hour, are used\. If you have a long running job, it might fail\. For information about how to set up your long running jobs to move data to and from Amazon Redshift, see [Moving Data to and from Amazon Redshift](aws-glue-programming-etl-redshift.md)\.

## Error: No Private DNS for Network Interface Found<a name="error-no-private-DNS"></a>

If a job fails or a development endpoint fails to provision, it might be because of a problem in the network setup\.
+ If you are using the Amazon provided DNS, the value of `enableDnsHostnames` must be set to true\. For more information, see [DNS](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html)\. 

## Error: Development Endpoint Provisioning Failed<a name="error-development-endpoint-failed"></a>

If AWS Glue fails to successfully provision a development endpoint, it might be because of a problem in the network setup\.
+ When you define a development endpoint, the VPC, subnet, and security groups are validated to confirm that they meet certain requirements\.
+ If you provided the optional SSH public key, check that it is a valid SSH public key\.
+ Check in the VPC console that your VPC uses a valid **DHCP option set**\. For more information, see [DHCP option sets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html)\. 
+ If the cluster remains in the PROVISIONING state, contact AWS Support\.

## Error: Notebook Server CREATE\_FAILED<a name="error-notebook-server-ec2-instance-profile"></a>

If AWS Glue fails to create the notebook server for a development endpoint, it might be because of one of the following problems: 
+ AWS Glue passes an IAM role to Amazon EC2 when it is setting up the notebook server\. The IAM role must have a trust relationship to Amazon EC2\.
+ The IAM role must have an instance profile of the same name\. When you create the role for Amazon EC2 with the IAM console, the instance profile with the same name is automatically created\. Check for an error in the log regarding an invalid instance profile name `iamInstanceProfile.name`\. For more information, see [Using Instance Profiles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html)\. 
+ Check that your role has permission to access `aws-glue*` buckets in the policy that you pass to create the notebook server\. 

## Error: Local Notebook Fails to Start<a name="error-local-notebook-fails-to-start"></a>

If your local notebook fails to start and reports errors that a directory or folder cannot be found, it might be because of one of the following problems: 
+ If you are running on Microsoft Windows, make sure that the `JAVA_HOME` environment variable points to the correct Java directory\. It's possible to update Java without updating this variable, and if it points to a folder that no longer exists, Zeppelin notebooks fail to start\.

## Error: Notebook Usage Errors<a name="error-notebook-usage-errors"></a>

When using an Apache Zeppelin notebook, you might encounter errors due to your setup or environment\. 
+ You provide an IAM role with an attached policy when you created the notebook server\. If the policy does not include all the required permissions, you might get an error such as `assumed-role/name-of-role/i-0bf0fa9d038087062 is not authorized to perform some-action AccessDeniedException`\. Check the policy that is passed to your notebook server in the IAM console\. 
+ If the Zeppelin notebook does not render correctly in your web browser, check the Zeppelin requirements for browser support\. For example, there might be specific versions and setup required for the Safari browser\. You might need to update your browser or use a different browser\. 

## Error: Running Crawler Failed<a name="error-running-crawler-failed"></a>

If AWS Glue fails to successfully run a crawler to catalog your data, it might be because of one of the following reasons\. First check if an error is listed in the AWS Glue console crawlers list\. Check if there is an exclamation  icon next to the crawler name and hover over the icon to see any associated messages\. 
+ Check the logs for the crawler run in CloudWatch Logs under `/aws-glue/crawlers`\.  

## Error: Upgrading Athena Data Catalog<a name="error-running-athena-upgrade"></a>

If you encounter errors while upgrading your Athena Data Catalog to the AWS Glue Data Catalog, see the *Amazon Athena User Guide* topic [Upgrading to the AWS Glue Data Catalog Step\-by\-Step](https://docs.aws.amazon.com/athena/latest/ug/glue-upgrade.html)\. 

## Error: A Job is Reprocessing Data When Job Bookmarks Are Enabled<a name="error-job-bookmarks-reprocess-data"></a>

There might be cases when you have enabled AWS Glue job bookmarks, but your ETL job is reprocessing data that was already processed in an earlier run\. Check for these common causes of this error: 

**Max Concurrency**  
Ensure that the maximum number of concurrent runs for the job is 1\. For more information, see the discussion of max concurrency in [Adding Jobs in AWS Glue](add-job.md)\. When you have multiple concurrent jobs with job bookmarks and the maximum concurrency is set to 1, the job bookmark doesn't work correctly\.

**Missing Job Object**  
Ensure that your job run script ends with the following commit:

```
job.commit()
```

When you include this object, AWS Glue records the timestamp and path of the job run\. If you run the job again with the same path, AWS Glue processes only the new files\. If you don't include this object and job bookmarks are enabled, the job reprocesses the already processed files along with the new files and creates redundancy in the job's target data store\.

**Missing Transformation Context Parameter**  
Transformation context is an optional parameter in the `GlueContext` class, but job bookmarks don't work if you don't include it\. To resolve this error, add the transformation context parameter when you [create the DynamicFrame](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-api-crawler-pyspark-extensions-glue-context.html#aws-glue-api-crawler-pyspark-extensions-glue-context-create_dynamic_frame_from_catalog), as shown following:

```
sample_dynF=create_dynamic_frame_from_catalog(database, table_name,transformation_ctx="sample_dynF") 
```

**Input Source**  
If you are using a relational database \(a JDBC connection\) for the input source, job bookmarks work only if the table's primary keys are in sequential order\. Job bookmarks work for new rows, but not for updated rows\. That is because job bookmarks look for the primary keys, which already exist\. This does not apply if your input source is Amazon Simple Storage Service \(Amazon S3\)\.

**Last Modified Time**  
For Amazon S3 input sources, job bookmarks check the last modified time of the objects, rather than the file names, to verify which objects need to be reprocessed\. If your input source data has been modified since your last job run, the files are reprocessed when you run the job again\. 