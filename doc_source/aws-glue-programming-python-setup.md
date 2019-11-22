# Setting Up to Use Python with AWS Glue<a name="aws-glue-programming-python-setup"></a>

Use Python to develop your ETL scripts for Spark jobs\. The supported Python versions for ETL jobs depend on the Glue version of the job\. For more information on Glue versions, see the [Glue version job property](add-job.md#glue-version-table)\.

**To set up your system for using Python with AWS Glue**

Follow these steps to install Python and to be able to invoke the AWS Glue APIs\. 

1. If you don't already have Python installed, download and install it from the [Python\.org download page](https://www.python.org/downloads/)\.

1. Install the AWS Command Line Interface \(AWS CLI\) as documented in the [AWS CLI documentation](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.

   The AWS CLI is not directly necessary for using Python\. However, installing and configuring it is a convenient way to set up AWS with your account credentials and verify that they work\.

1. Install the AWS SDK for Python \(Boto 3\), as documented in the [Boto3 Quickstart](http://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html)\.

   Boto 3 resource APIs are not yet available for AWS Glue\. Currently, only the Boto 3 client APIs can be used\.

   For more information about Boto 3, see [AWS SDK for Python \(Boto 3\) Getting Started](http://boto3.amazonaws.com/v1/documentation/api/latest/index.html)\.

You can find Python code examples and utilities for AWS Glue in the [AWS Glue samples repository](https://github.com/awslabs/aws-glue-samples) on the GitHub website\.