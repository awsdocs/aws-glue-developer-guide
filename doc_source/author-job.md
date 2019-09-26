# Authoring Jobs in AWS Glue<a name="author-job"></a>

A *job* is the business logic that performs the extract, transform, and load \(ETL\) work in AWS Glue\. When you start a job, AWS Glue runs a script that extracts data from sources, transforms the data, and loads it into targets\. You can create jobs in the ETL section of the AWS Glue console\. For more information, see [Working with Jobs on the AWS Glue Console](console-jobs.md)\.

The following diagram summarizes the basic workflow and steps involved in authoring a job in AWS Glue:

![\[Workflow showing how to author a job with AWS Glue in 6 basic steps.\]](http://docs.aws.amazon.com/glue/latest/dg/images/AuthorJob-overview.png)

**Topics**
+ [Workflow Overview](#author-job-workflow)
+ [Adding Jobs in AWS Glue](add-job.md)
+ [Editing Scripts in AWS Glue](edit-script.md)
+ [Triggering Jobs in AWS Glue](trigger-job.md)
+ [Developing Scripts Using Development Endpoints](dev-endpoint.md)
+ [Managing Notebooks](notebooks-with-glue.md)

## Workflow Overview<a name="author-job-workflow"></a>

When you author a job, you supply details about data sources, targets, and other information\. The result is a generated Apache Spark API \(PySpark\) script\. You can then store your job definition in the AWS Glue Data Catalog\.

The following describes an overall process of authoring jobs in the AWS Glue console:

1. You choose a data source for your job\. The tables that represent your data source must already be defined in your Data Catalog\. If the source requires a connection, the connection is also referenced in your job\. If your job requires multiple data sources, you can add them later by editing the script\.

1. You choose a data target of your job\. The tables that represent the data target can be defined in your Data Catalog, or your job can create the target tables when it runs\. You choose a target location when you author the job\. If the target requires a connection, the connection is also referenced in your job\. If your job requires multiple data targets, you can add them later by editing the script\.

1. You customize the job\-processing environment by providing arguments for your job and generated script\. For more information, see [Adding Jobs in AWS Glue](add-job.md)\.

1. Initially, AWS Glue generates a script, but you can also edit this script to add sources, targets, and transforms\. For more information about transforms, see [Built\-In Transforms](built-in-transforms.md)\.

1. You specify how your job is invoked, either on demand, by a time\-based schedule, or by an event\. For more information, see [Triggering Jobs in AWS Glue](trigger-job.md)\.

1. Based on your input, AWS Glue generates a PySpark or Scala script\. You can tailor the script based on your business needs\. For more information, see [Editing Scripts in AWS Glue](edit-script.md)\.