# Adding a Development Endpoint<a name="add-dev-endpoint"></a>

Use development endpoints to iteratively develop and test your extract, transform, and load \(ETL\) scripts in AWS Glue\. You can add a development endpoint using the AWS Glue console or the AWS Command Line Interface \(AWS CLI\)\.

**To add a development endpoint \(console\)**

1. Open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\. Sign in as a user who has the IAM permission `glue:CreateDevEndpoint`\.

1. In the navigation pane, choose **Dev endpoints**, and then choose **Add endpoint**\.

1. Follow the steps in the AWS Glue **Add endpoint** wizard to provide the properties that are required to create an endpoint\. Specify an IAM role that permits access to your data\. 

   If you choose to provide an SSH public key when you create your development endpoint, save the SSH private key to access the development endpoint later\.

1. Choose **Finish** to complete the wizard\. Then check the console for development endpoint status\. When the status changes to `READY`, the development endpoint is ready to use\.

   When creating the endpoint, you can provide the following optional information:  
**Security configuration**  
To specify at\-rest encryption options, add a security configuration to the development endpoint\.   
**Worker type**  
The type of predefined worker that is allocated to the development endpoint\. Accepts a value of `Standard`, `G.1X`, or `G.2X`\.  
   + For the `Standard` worker type, each worker provides 4 vCPU, 16 GB of memory, a 50 GB disk, and 2 executors per worker\.
   + For the `G.1X` worker type, each worker maps to 1 DPU \(4 vCPU, 16 GB of memory, and a 64 GB disk\), and provides 1 executor per worker\. We recommend this worker type for memory\-intensive jobs\.
   + For the `G.2X` worker type, each worker maps to 2 DPU \(8 vCPU, 32 GB of memory, and a 128 GB disk\), and provides 1 executor per worker\. We recommend this worker type for memory\-intensive jobs\.
**Known issue:** When you create a development endpoint with the `G.2X` `WorkerType` configuration, the Spark drivers for the development endpoint run on 4 vCPU, 16 GB of memory, and a 64 GB disk\.   
**Number of workers**  
The number of workers of a defined `workerType` that are allocated to the development endpoint\. This field is available only when you choose worker type G\.1X or G\.2X\.  
The maximum number of workers you can define is 299 for `G.1X`, and 149 for `G.2X`\.   
**Data processing units \(DPUs\)**  
The number of DPUs that AWS Glue uses for your development endpoint\. The number must be greater than 1\.  
**Python library path**  
Comma\-separated Amazon Simple Storage Service \(Amazon S3\) paths to Python libraries that are required by your script\. Multiple values must be complete paths separated by a comma \(`,`\)\. Only individual files are supported, not a directory path\.  
You can use only pure Python libraries\. Libraries that rely on C extensions, such as the Pandas Python data analysis library, are not yet supported\.  
**Dependent jars path**  
Comma\-separated Amazon S3 paths to JAR files that are required by the script\.  
Currently, you can use only pure Java or Scala \(2\.11\) libraries\.  
**Glue Version**  
Specifies the versions of Python and Apache Spark to use\. Defaults to AWS Glue version 1\.0 \(Python version 3 and Spark version 2\.4\)\. For more information, see the [Glue version job property](add-job.md#glue-version-table)\.  
**Tags**  
Tag your development endpoint with a **Tag key** and optional **Tag value**\. After tag keys are created, they are read\-only\. Use tags on some resources to help you organize and identify them\. For more information, see [AWS Tags in AWS Glue](monitor-tags.md)\.   
**Spark UI**  
Enable the use of Spark UI for monitoring Spark applications runnning on this development endpoint\. For more information, see [Enabling the Apache Spark Web UI for Development Endpoints](monitor-spark-ui-dev-endpoints.md)\.   
**Use Glue Data Catalog as the Hive metastore \(under Catalog Options\)**  
Enables you to use the AWS Glue Data Catalog as a Spark Hive metastore\.

**To add a development endpoint \(AWS CLI\)**

1. In a command line window, enter a command similar to the following\.

   ```
   aws glue create-dev-endpoint --endpoint-name "endpoint1" --role-arn "arn:aws:iam::account-id:role/role-name" --number-of-nodes "3" --glue-version "1.0" --arguments '{"GLUE_PYTHON_VERSION": "3"}' --region "region-name"
   ```

   This command specifies AWS Glue version 1\.0\. Because this version supports both Python 2 and Python 3, you can use the `arguments` parameter to indicate the desired Python version\. If the `glue-version` parameter is omitted, AWS Glue version 0\.9 is assumed\. For more information about AWS Glue versions, see the [Glue version job property](add-job.md#glue-version-table)\.

   For information about additional command line parameters, see [create\-dev\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/glue/create-dev-endpoint.html) in the *AWS CLI Command Reference*\.

1. \(Optional\) Enter the following command to check the development endpoint status\. When the status changes to `READY`, the development endpoint is ready to use\.

   ```
   aws glue get-dev-endpoint --endpoint-name "endpoint1"
   ```