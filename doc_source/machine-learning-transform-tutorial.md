# Tutorial: Creating a Machine Learning Transform with AWS Glue<a name="machine-learning-transform-tutorial"></a>

This tutorial guides you through the actions to create and manage a machine learning \(ML\) transform using AWS Glue\. Before using this tutorial, you should be familiar with using the AWS Glue console to add crawlers and jobs and edit scripts\. You should also be familiar with finding and downloading files on the Amazon Simple Storage Service \(Amazon S3\) console\.

In this example, you create a `FindMatches` transform to find matching records, teach it how to identify matching and nonmatching records, and use it in an AWS Glue job\. The AWS Glue job writes a new Amazon S3 file with an additional column named `match_id`\. 

The source data used by this tutorial is a file named `dblp_acm_records.csv`\. This file is a modified version of academic publications \(DBLP and ACM\) available from the original [DBLP ACM dataset](https://doi.org/10.3886/E100843V2)\. The `dblp_acm_records.csv` file is a comma\-separated values \(CSV\) file in UTF\-8 format with no byte\-order mark \(BOM\)\. 

A second file, `dblp_acm_labels.csv`, is an example labeling file that contains both matching and nonmatching records used to teach the transform as part of the tutorial\. 

**Topics**
+ [Step 1: Crawl the Source Data](#ml-transform-tutorial-crawler)
+ [Step 2: Add a Machine Learning Transform](#ml-transform-tutorial-create)
+ [Step 3: Teach Your Machine Learning Transform](#ml-transform-tutorial-teach)
+ [Step 4: Estimate the Quality of Your Machine Learning Transform](#ml-transform-tutorial-estimate-quality)
+ [Step 5: Add and Run a Job with Your Machine Learning Transform](#ml-transform-tutorial-add-job)
+ [Step 6: Verify Output Data from Amazon S3](#ml-transform-tutorial-data-output)

## Step 1: Crawl the Source Data<a name="ml-transform-tutorial-crawler"></a>

First, crawl the source Amazon S3 CSV file to create a corresponding metadata table in the Data Catalog\.

**Important**  
To direct the crawler to create a table for only the CSV file, store the CSV source data in a different Amazon S3 folder from other files\.

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, choose **Crawlers**, **Add crawler**\. 

1. Follow the wizard to create and run a crawler named `demo-crawl-dblp-acm` with output to database `demo-db-dblp-acm`\. When running the wizard, create the database `demo-db-dblp-acm` if it doesn't already exist\. Choose an Amazon S3 include path to sample data in the current AWS Region\. For example, for `us-east-1`, the Amazon S3 include path to the source file is `s3://ml-transforms-public-datasets-us-east-1/dblp-acm/records/dblp_acm_records.csv`\. 

   If successful, the crawler creates the table `dblp_acm_records_csv` with the following columns: id, title, authors, venue, year, and source\.

## Step 2: Add a Machine Learning Transform<a name="ml-transform-tutorial-create"></a>

Next, add a machine learning transform that is based on the schema of your data source table created by the crawler named `demo-crawl-dblp-acm`\.

1. On the AWS Glue console, in the navigation pane, choose **ML Transforms**, **Add transform**\. Then follow the wizard to create a `Find matches` transform with the following properties\. 

   1. For **Transform name**, enter **demo\-xform\-dblp\-acm**\. This is the name of the transform that is used to find matches in the source data\.

   1. For **IAM role**, choose an IAM role that has permission to the Amazon S3 source data, labeling file, and AWS Glue API operations\. For more information, see [Create an IAM Role for AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/create-an-iam-role.html) in the *AWS Glue Developer Guide*\.

   1. For **Data source**, choose the table named **dblp\_acm\_records\_csv** in database **demo\-db\-dblp\-acm**\.

   1. For **Primary key**, choose the primary key column for the table, **id**\.

   1. For **Predictive columns**, choose the **title**, **authors**, **venue**, **year**, and **source** columns in the data source to act as predictive columns\.

1. In the wizard, choose **Finish** and return to the **ML transforms** list\.

## Step 3: Teach Your Machine Learning Transform<a name="ml-transform-tutorial-teach"></a>

Next, you teach your machine learning transform using the tutorial sample labeling file\.

You can't use a machine language transform in an extract, transform, and load \(ETL\) job until its status is **Ready for use**\. To get your transform ready, you must teach it how to identify matching and nonmatching records by providing examples of matching and nonmatching records\. To teach your transform, you can **Generate a label file**, add labels, and then **Upload label file**\. In this tutorial, you can use the example labeling file named `dblp_acm_labels.csv`\. For more information about the labeling process, see [Labeling](machine-learning.md#machine-learning-labeling)\.

1. On the AWS Glue console, in the navigation pane, choose **ML Transforms**\.

1. Choose the `demo-xform-dblp-acm` transform, and then choose **Action**, **Teach**\. Follow the wizard to teach your `Find matches` transform\. 

1. On the transform properties page, choose **I have labels**\. Choose an Amazon S3 path to the sample labeling file in the current AWS Region\. For example, for `us-east-1`, upload the provided labeling file from the Amazon S3 path `s3://ml-transforms-public-datasets-us-east-1/dblp-acm/labels/dblp_acm_labels.csv` with the option to **overwrite** existing labels\. The labeling file must be located in Amazon S3 in the same Region as the AWS Glue console\.

   When you upload a labeling file, a task is started in AWS Glue to add or overwrite the labels used to teach the transform how to process the data source\.

1. On the final page of the wizard, choose **Finish**, and return to the **ML transforms** list\.

## Step 4: Estimate the Quality of Your Machine Learning Transform<a name="ml-transform-tutorial-estimate-quality"></a>

Next, you can estimate the quality of your machine learning transform\. The quality depends on how much labeling you have done\. For more information about estimating quality, see [Estimate quality](console-machine-learning-transforms.md#console-machine-learning-transforms-metrics)\.

1. On the AWS Glue console, in the navigation pane, choose **ML Transforms**\. 

1. Choose the `demo-xform-dblp-acm` transform, and choose the **Estimate quality** tab\. This tab displays the current quality estimates, if available, for the transform\. 

1. Choose **Estimate quality** to start a task to estimate the quality of the transform\. The accuracy of the quality estimate is based on the labeling of the source data\.

1. Navigate to the **History** tab\. In this pane, task runs are listed for the transform, including the **Estimating quality** task\. For more details about the run, choose **Logs**\. Check that the run status is **Succeeded** when it finishes\.

## Step 5: Add and Run a Job with Your Machine Learning Transform<a name="ml-transform-tutorial-add-job"></a>

In this step, you use your machine learning transform to add and run a job in AWS Glue\. When the transform `demo-xform-dblp-acm` is **Ready for use**, you can use it in an ETL job\.

1. On the AWS Glue console, in the navigation pane, choose **Jobs**\.

1. Choose **Add job**, and follow the steps in the wizard to create an ETL Spark job with a generated script\. Choose the following property values for your transform:

   1. For **Name**, choose the example job in this tutorial, **demo\-etl\-dblp\-acm**\.

   1. For **IAM role**, choose an IAM role with permission to the Amazon S3 source data, labeling file, and AWS Glue API operations\. For more information, see [Create an IAM Role for AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/create-an-iam-role.html) in the *AWS Glue Developer Guide*\.

   1. For **ETL language**, choose **Scala**\. This is the programming language in the ETL script\.

   1. For **Script file name**, choose **demo\-etl\-dblp\-acm**\. This is the file name of the Scala script \(same as the job name\)\.

   1. For **Data source**, choose **dblp\_acm\_records\_csv**\. The data source you choose must match the machine learning transform data source schema\.

   1. For **Transform type**, choose **Find matching records** to create a job using a machine learning transform\.

   1. Clear **Remove duplicate records**\. You don't want to remove duplicate records because the output records written have an additional `match_id` field added\. 

   1. For **Transform**, choose **demo\-xform\-dblp\-acm**, the machine learning transform used by the job\.

   1. For **Create tables in your data target**, choose to create tables with the following properties:
      + **Data store type** — **Amazon S3**
      + **Format** — **CSV**
      + **Compression type** — **None**
      + **Target path** — The Amazon S3 path where the output of the job is written \(in the current console AWS Region\)

1. Choose **Save job and edit script** to display the script editor page\.

1. Edit the script to add a statement to cause the job output to the **Target path** to be written to a single partition file\. Add this statement immediately following the statement that runs the `FindMatches` transform\. The statement is similar to the following\.

   ```
   val single_partition = findmatches1.repartition(1) 
   ```

   You must modify the `.writeDynamicFrame(findmatches1)` statement to write the output as `.writeDynamicFrame(single_partion)`\. 

1. After you edit the script, choose **Save**\. The modified script looks similar to the following code, but customized for your environment\.

   ```
   import com.amazonaws.services.glue.GlueContext
   import com.amazonaws.services.glue.errors.CallSite
   import com.amazonaws.services.glue.ml.FindMatches
   import com.amazonaws.services.glue.util.GlueArgParser
   import com.amazonaws.services.glue.util.Job
   import com.amazonaws.services.glue.util.JsonOptions
   import org.apache.spark.SparkContext
   import scala.collection.JavaConverters._
   
   object GlueApp {
     def main(sysArgs: Array[String]) {
       val spark: SparkContext = new SparkContext()
       val glueContext: GlueContext = new GlueContext(spark)
       // @params: [JOB_NAME]
       val args = GlueArgParser.getResolvedOptions(sysArgs, Seq("JOB_NAME").toArray)
       Job.init(args("JOB_NAME"), glueContext, args.asJava)
       // @type: DataSource
       // @args: [database = "demo-db-dblp-acm", table_name = "dblp_acm_records_csv", transformation_ctx = "datasource0"]
       // @return: datasource0
       // @inputs: []
       val datasource0 = glueContext.getCatalogSource(database = "demo-db-dblp-acm", tableName = "dblp_acm_records_csv", redshiftTmpDir = "", transformationContext = "datasource0").getDynamicFrame()
       // @type: FindMatches
       // @args: [transformId = "tfm-123456789012", emitFusion = false, survivorComparisonField = "<primary_id>", transformation_ctx = "findmatches1"]
       // @return: findmatches1
       // @inputs: [frame = datasource0]
       val findmatches1 = FindMatches.apply(frame = datasource0, transformId = "tfm-123456789012", transformationContext = "findmatches1")
     
     
       // Repartition the previous DynamicFrame into a single partition. 
       val single_partition = findmatches1.repartition(1)    
    
       
       // @type: DataSink
       // @args: [connection_type = "s3", connection_options = {"path": "s3://aws-glue-ml-transforms-data/sal"}, format = "csv", transformation_ctx = "datasink2"]
       // @return: datasink2
       // @inputs: [frame = findmatches1]
       val datasink2 = glueContext.getSinkWithFormat(connectionType = "s3", options = JsonOptions("""{"path": "s3://aws-glue-ml-transforms-data/sal"}"""), transformationContext = "datasink2", format = "csv").writeDynamicFrame(single_partition)
       Job.commit()
     }
   }
   ```

1. Choose **Run job** to start the job run\. Check the status of the job in the jobs list\. When the job finishes, in the **ML transform**, **History** tab, there is a new **Run ID** row added of type **ETL job**\.

1. Navigate to the **Jobs**, **History** tab\. In this pane, job runs are listed\. For more details about the run, choose **Logs**\. Check that the run status is **Succeeded** when it finishes\.

## Step 6: Verify Output Data from Amazon S3<a name="ml-transform-tutorial-data-output"></a>

In this step, you check the output of the job run in the Amazon S3 bucket that you chose when you added the job\. You can download the output file to your local machine and verify that matching records were identified\.

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Download the target output file of the job `demo-etl-dblp-acm`\. Open the file in a spreadsheet application \(you might need to add a file extension `.csv` for the file to properly open\)\.

   The following image shows an excerpt of the output in Microsoft Excel\.  
![\[Excel spreadsheet showing the output of the transform.\]](http://docs.aws.amazon.com/glue/latest/dg/images/demo_output_dblp_acm.png)

   The data source and target file both have 4,911 records\. However, the `Find matches` transform adds another column named `match_id` to identify matching records in the output\. Rows with the same `match_id` are considered matching records\.

1. Sort the output file by `match_id` to easily see which records are matches\. Compare the values in the other columns to see if you agree with the results of the `Find matches` transform\. If you don't, you can continue to teach the transform by adding more labels\. 

   You can also sort the file by another field, such as `title`, to see if records with similar titles have the same `match_id`\. 