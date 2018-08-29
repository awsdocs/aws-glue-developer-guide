# Setting Up to Use Python with AWS Glue<a name="aws-glue-programming-python-setup"></a>

Use Python 2\.7 rather than Python 3 to develop your ETL scripts\. The AWS Glue development endpoints that provide interactive testing and development do not work with Python 3 yet\.

**To set up your system for using Python with AWS Glue**

Follow these steps to install Python and to be able to invoke the AWS Glue APIs\. 

1. If you don't already have Python 2\.7 installed, download and install it from the [Python\.org download page](https://www.python.org/downloads/)\.

1. Install the AWS Command Line Interface \(AWS CLI\) as documented in the [AWS CLI documentation](http://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.

   The AWS CLI is not directly necessary for using Python\. However, installing and configuring it is a convenient way to set up AWS with your account credentials and verify that they work\.

1. Install the AWS SDK for Python \(Boto 3\), as documented in the [Boto3 Quickstart](http://boto3.readthedocs.org/en/latest/guide/quickstart.html)\.

   Boto 3 resource APIs are not yet available for AWS Glue\. Currently, only the Boto 3 client APIs can be used\.

   For more information about Boto 3, see [AWS SDK for Python \(Boto 3\) Getting Started](http://boto3.readthedocs.org/en/latest/)\.

You can find Python code examples and utilities for AWS Glue in the [AWS Glue samples repository](https://github.com/awslabs/aws-glue-samples) on the GitHub website\.