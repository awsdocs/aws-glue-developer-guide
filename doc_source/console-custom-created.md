# Providing Your Own Custom Scripts<a name="console-custom-created"></a>

Scripts perform the extract, transform, and load \(ETL\) work in AWS Glue\. A script is created when you automatically generate the source code logic for a job\. You can either edit this generated script, or you can provide your own custom script\.

**Important**  
Different versions of AWS Glue support different versions of Apache Spark\. Your custom script must be compatible with the supported Apache Spark version\. For information about AWS Glue versions, see the [Glue version job property](add-job.md#glue-version-table)\.

To provide your own custom script in AWS Glue, follow these general steps:

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. Choose the **Jobs** tab, and then choose **Add job** to start the **Add job** wizard\.

1. In the **Job properties** screen, choose the **IAM role** that is required for your custom script to run\. For more information, see [Identity and Access Management in AWS Glue](authentication-and-access-control.md)\.

1. Under **This job runs**, choose one of the following:
   + An existing script that you provide
   + A new script to be authored by you

1. Choose any connections that your script references\. These objects are needed to connect to the necessary JDBC data stores\.

   An elastic network interface is a virtual network interface that you can attach to an instance in a virtual private cloud \(VPC\)\. Choose the elastic network interface that is required to connect to the data store that's used in the script\.

1. If your script requires additional libraries or files, you can specify them as follows:  
**Python library path**  
Comma\-separated Amazon Simple Storage Service \(Amazon S3\) paths to Python libraries that are required by the script\.  
Only pure Python libraries can be used\. Libraries that rely on C extensions, such as the pandas Python Data Analysis Library, are not yet supported\.  
**Dependent jars path**  
Comma\-separated Amazon S3 paths to JAR files that are required by the script\.  
Currently, only pure Java or Scala \(2\.11\) libraries can be used\.  
**Referenced files path**  
Comma\-separated Amazon S3 paths to additional files \(for example, configuration files\) that are required by the script\.

1. If you want, you can add a schedule to your job\. To change a schedule, you must delete the existing schedule and add a new one\.

For more information about adding jobs in AWS Glue, see [Adding Jobs in AWS Glue](add-job.md)\. 

For step\-by\-step guidance, see the **Add job** tutorial in the AWS Glue console\.