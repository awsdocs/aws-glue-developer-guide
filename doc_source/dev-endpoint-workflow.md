# Development Endpoint Workflow<a name="dev-endpoint-workflow"></a>

To use an AWS Glue development endpoint, you can follow this workflow:

1. Create a development endpoint using the console or API\. The endpoint is launched in a virtual private cloud \(VPC\) with your defined security groups\.

1. The console or API polls the development endpoint until it is provisioned and ready for work\. When it's ready, connect to the development endpoint using one of the following methods to create and test AWS Glue scripts\.
   + Install an Apache Zeppelin notebook on your local machine, connect it to a development endpoint, and then develop on it using your browser\.
   + Create a Zeppelin notebook server in its own Amazon EC2 instance in your account using the AWS Glue console, and then connect to it using your browser\. For more information about how to create a notebook server, see [Creating a Notebook Server Associated with a Development Endpoint](dev-endpoint-notebook-server-considerations.md)\. 
   + Create an Amazon SageMaker notebook in your account using the AWS Glue console\. For more information about how to create a notebook, see [Working with Notebooks on the AWS Glue Console](console-notebooks.md)\. 
   + Open a terminal window to connect directly to a development endpoint\.
   + If you have the professional edition of the JetBrains [PyCharm Python IDE](https://www.jetbrains.com/pycharm/), connect it to a development endpoint and use it to develop interactively\. If you insert `pydevd` statements in your script, PyCharm can support remote breakpoints\.

1. When you finish debugging and testing on your development endpoint, you can delete it\.