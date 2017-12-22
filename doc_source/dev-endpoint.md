# Using Development Endpoints for Developing Scripts<a name="dev-endpoint"></a>

AWS Glue can create an environment for you to iteratively develop and test your extract, transform, and load \(ETL\) scripts\. You can develop your script in a notebook  and point to an AWS Glue endpoint to test it\. When you're satisfied with the results of your development process, you can create an ETL job that runs your script\. With this process, you can add functions and debug your script in an interactive manner\.

**Note**  
Your ETL scripts must target Python 2\.7, because AWS Glue development endpoints do not support Python 3 yet\.

## Managing Your Development Environment<a name="dev-endpoint-actions"></a>

With AWS Glue, you can create, edit, and delete development endpoints\. You provide configuration values to provision the development environments\. These values tell AWS Glue how to set up the network so that you can access your development endpoint securely, and your endpoint can access your data stores\. Then, create a notebook that connects to the development endpoint, and use your notebook to author and test your ETL script\.

 For more information about managing a development endpoint using the AWS Glue console, see [Working with Development Endpoints on the AWS Glue Console](console-development-endpoint.md)\. 

## How to Use a Development Endpoint<a name="dev-endpoint-workflow"></a>

To use a development endpoint, you can follow this workflow\.

1. Create an AWS Glue development endpoint through the console or API\. This endpoint is launched in your virtual private cloud \(VPC\) with your defined security groups\.

1. The console or API can poll the development endpoint until it is provisioned and ready for work\. When it's ready, you can connect to the development endpoint to create and test AWS Glue scripts\.

   + You can install an Apache Zeppelin notebook on your local machine, connect it to a development endpoint, and then develop on it using your browser\.

   + You can create an Apache Zeppelin notebook server in its own Amazon EC2 instance in your account using the AWS Glue console, and then connect to it using your browser\.

   + You can open a terminal window to connect directly to a development endpoint\.

   + If you have the Professional edition of the JetBrains [PyCharm Python IDE](https://www.jetbrains.com/pycharm/), you can connect it to a development endpoint and use it to develop interactively\. PyCharm can then support remote breakpoints if you insert `pydevd` statements in your script\.

1. When you finish debugging and testing on your development endpoint, you can delete it\.