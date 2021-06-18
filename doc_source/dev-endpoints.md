# Development Endpoints<a name="dev-endpoints"></a>

A development endpoint is an environment that you can use to develop and test your AWS Glue scripts\. You can use AWS Glue to create, edit, and delete development endpoints\. The **Dev Endpoints** tab on the AWS Glue console lists all the development endpoints that are created\. You can add, delete, or rotate the SSH key of a development endpoint\. You can also create notebooks that use the development endpoint\.

You provide configuration values to provision the development environments\. These values tell AWS Glue how to set up the network so that you can access the development endpoint securely, and so that your endpoint can access your data stores\. Then, you can create a notebook that connects to the development endpoint\. You use your notebook to author and test your ETL script\.

Use an AWS Identity and Access Management \(IAM\) role with permissions similar to the IAM role that you use to run AWS Glue ETL jobs\. Use a virtual private cloud \(VPC\), a subnet, and a security group to create a development endpoint that can connect to your data resources securely\. You generate an SSH key pair to connect to the development environment using SSH\.

You can create development endpoints for Amazon S3 data and within a VPC that you can use to access datasets using JDBC\.

You can install an Apache Zeppelin notebook on your local machine and use it to debug and test ETL scripts on a development endpoint\. Or, you can host the Zeppelin notebook on an Amazon EC2 instance\. A notebook server is a web\-based environment that you can use to run your PySpark statements\.

AWS Glue tags Amazon EC2 instances with a name that is prefixed with `aws-glue-dev-endpoint`\.

You can set up a notebook server on a development endpoint to run PySpark statements with AWS Glue extensions\. For more information about Zeppelin notebooks, see [Apache Zeppelin](http://zeppelin.apache.org/)\. 