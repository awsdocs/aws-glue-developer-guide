# Workload Partitioning with Bounded Execution<a name="bounded-execution"></a>

Errors in Spark applications commonly arise from inefficient Spark scripts, distributed in\-memory execution of large\-scale transformations, and dataset abnormalities\. There are many reasons that may cause driver or executor out of memory issues, for example a data skew, listing too many objects, or large data shuffles\. These issues often appear when you are processing huge amounts of backlog data with Spark\.

AWS Glue allows you to solve OOM issues and make your ETL processing easier with workload partitioning\. With workload partitioning enabled, each ETL job run only picks unprocessed data, with an upper bound on the dataset size or the number of files to be processed with this job run\. Future job runs will process the remaining data\. For example, if there are 1000 files need to be processed, you can set the number of files to be 500 and separate them into two job runs\.

Workload partitioning is supported only for Amazon S3 data sources\.

## Enabling Workload Partitioning<a name="bounded-execution-enable"></a>

You can enable bounded execution by manually setting the options in your script or by adding catalog table properties\.

**To enable workload partitioning with bounded execution in your script:**

1. To avoid reprocessing data, enable job bookmarks in the new job or existing job\. For more information, see [Tracking Processed Data Using Job Bookmarks](https://docs.aws.amazon.com/glue/latest/dg/monitor-continuations.html)\.

1. Modify your script and set the bounded limit in the additional options in the AWS Glue `getSource` API\. You should also set the transformation context for the job bookmark to store the `state` element\. For example:

   Python

   ```
   glueContext.create_dynamic_frame.from_catalog(
       database = "database",
       tableName = "table_name",
       redshift_tmp_dir = "",
       transformation_ctx = "datasource0",
       additional_options = {
           "boundedFiles" : "500", # need to be string
         # "boundedSize" : "1000000000" unit is byte
       }
   )
   ```

   Scala

   ```
   val datasource0 = glueContext.getCatalogSource(
       database = "database", tableName = "table_name", redshiftTmpDir = "", 
       transformationContext = "datasource0",
       additionalOptions = JsonOptions(
           Map("boundedFiles" -> "500") // need to be string
         //"boundedSize" -> "1000000000" unit is byte
       ) 
   ).getDynamicFrame()
   ```

   ```
   val connectionOptions = JsonOptions(
       Map("paths" -> List(baseLocation), "boundedFiles" -> "30")
   )
   val source = glueContext.getSource("s3", connectionOptions, "datasource0", "")
   ```

**To enable workload partitioning with bounded execution in your Data Catalog table:**

1. Set the key\-value pairs in the `parameters` field of your table structure in the Data Catalog\. For more information, see [Viewing and Editing Table Details](https://docs.aws.amazon.com/glue/latest/dg/console-tables.html#console-tables-details)\.

1. Set the upper limit for the dataset size or the number of files processed:
   + Set `boundedSize` to the target size of the dataset in bytes\. The job run will stop after reaching the target size from the table\.
   + Set `boundedFiles` to the target number of files\. The job run will stop after processing the target number of files\.
**Note**  
You should only set one of `boundedSize` or `boundedFiles`, as only a single boundary is supported\.

## Setting up an AWS Glue Trigger to Automatically Run the Job<a name="bounded-execution-trigger"></a>

Once you have enabled bounded execution, you can set up an AWS Glue trigger to automatically run the job and incrementally load the data in sequential runs\. Go to the AWS Glue Console and create a trigger, setup the schedule time, and attach to your job\. Then it will automatically trigger the next job run and process the new batch of data\. 

You can also use AWS Glue workflows to orchestrate multiple jobs to process data from different partitions in parallel\. For more information, see [AWS Glue Triggers](https://docs.aws.amazon.com/glue/latest/dg/about-triggers.html) and [AWS Glue Workflows](https://docs.aws.amazon.com/glue/latest/dg/workflows_overview.html)\.

For more information on use cases and options, please refer to the blog [Optimizing Spark applications with workload partitioning in AWS Glue](https://aws.amazon.com/blogs/big-data/optimizing-spark-applications-with-workload-partitioning-in-aws-glue/)\.