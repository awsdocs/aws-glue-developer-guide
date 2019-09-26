# Managing Notebooks<a name="notebooks-with-glue"></a>

A notebook enables interactive development and testing of your ETL \(extract, transform, and load\) scripts on a development endpoint\. AWS Glue provides an interface to Amazon SageMaker notebooks and Apache Zeppelin notebook servers\.
+ Amazon SageMaker provides an integrated Jupyter authoring notebook instance\. With AWS Glue, you create and manage Amazon SageMaker notebooks\. You can also open Amazon SageMaker notebooks from the AWS Glue console\.

  In addition, you can use Apache Spark with Amazon SageMaker on AWS Glue development endpoints which support Amazon SageMaker \(but not AWS Glue ETL jobs\)\. SageMaker Spark is an open source Apache Spark library for Amazon SageMaker\. For more information, see see [Using Apache Spark with Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/apache-spark.html)\. 
+ Apache Zeppelin notebook servers are run on Amazon EC2 instances\. You can create these instances on the AWS Glue console\.

 For more information about creating and accessing your notebooks using the AWS Glue console, see [Working with Notebooks on the AWS Glue Console](console-notebooks.md)\. 

 For more information about creating development endpoints, see [Viewing Development Endpoint Properties](console-development-endpoint.md)\. 


| Region | Code | 
| --- | --- | 
|   Managing Amazon SageMaker notebooks with AWS Glue development endpoints is available in the following AWS Regions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/notebooks-with-glue.html)   | 
| US East \(Ohio\) | `us-east-2` | 
| US East \(N\. Virginia\) | `us-east-1` | 
| US West \(N\. California\) | `us-west-1` | 
| US West \(Oregon\) | `us-west-2` | 
| Asia Pacific \(Tokyo\) | `ap-northeast-1` | 
| Asia Pacific \(Seoul\) | `ap-northeast-2` | 
| Asia Pacific \(Mumbai\) | `ap-south-1` | 
| Asia Pacific \(Singapore\) | `ap-southeast-1` | 
| Asia Pacific \(Sydney\) | `ap-southeast-2` | 
| Canada \(Central\) | `ca-central-1` | 
| EU \(Frankfurt\) | `eu-central-1` | 
| EU \(Ireland\) | `eu-west-1` | 
| EU \(London\) | `eu-west-2` | 
| AWS GovCloud \(US\-West\) | `us-gov-west-1` | 

**Topics**
+ [Creating a Notebook Server Associated with a Development Endpoint](dev-endpoint-notebook-server-considerations.md)
+ [Working with Notebooks on the AWS Glue Console](console-notebooks.md)