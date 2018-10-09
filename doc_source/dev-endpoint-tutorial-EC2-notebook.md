# Tutorial: Set Up an Apache Zeppelin Notebook Server on Amazon EC2<a name="dev-endpoint-tutorial-EC2-notebook"></a>

In this tutorial, you create an Apache Zeppelin Notebook server that is hosted on an Amazon EC2 instance\. The notebook connects to one of your development endpoints so that you can interactively run, debug, and test AWS Glue ETL \(extract, transform, and load\) scripts before deploying them\.

The tutorial assumes that you have already taken the steps outlined in [Tutorial Prerequisites](dev-endpoint-tutorial-prerequisites.md)\.

## Creating an Apache Zeppelin Notebook Server on an Amazon EC2 Instance<a name="dev-endpoint-tutorial-EC2-notebook-server"></a>

To create a notebook server on Amazon EC2, you must have permission to create resources in AWS CloudFormation, Amazon EC2, and other services\. For more information about required user permissions, see [Step 3: Attach a Policy to IAM Users That Access AWS Glue](attach-policy-iam-user.md)\.

1. On the AWS Glue console, choose **Dev endpoints** to go to the development endpoints list\.

1. Choose an endpoint by selecting the box next to it\. Choose and endpoint with an empty SSH public key because the key is generated with a later action on the Amazon EC2 instance\. Then choose **Actions**, and choose **Create notebook server**\.

   To host the notebook server, an Amazon EC2 instance is spun up using an AWS CloudFormation stack on your development endpoint\. If you create the Zeppelin server with an SSL certificate, the Zeppelin HTTPS server is started on port 443\.

1. Enter an AWS CloudFormation stack server name such as `demo-cf`, using only alphanumeric characters and hyphens\.

1. Choose an IAM role that you have set up with a trust relationship to Amazon EC2, as documented in [Step 5: Create an IAM Role for Notebook Servers](create-an-iam-role-notebook.md)\.

1. Choose an Amazon EC2 key pair that you have generated on the Amazon EC2 console \([https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\), or choose **Create EC2 key pair** to generate a new one\. Remember where you have downloaded and saved the private key portion of the pair\. This key pair is different from the SSH key you used when creating your development endpoint \(the keys that Amazon EC2 uses are 2048\-bit SSH\-2 RSA keys\)\. For more information about Amazon EC2 keys, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\. 

   It is to generally a good practice to ensure that the private\-key file is write\-protected so that it is not accidentally modified\. On macOS and Linux systems, do this by opening a terminal and entering `chmod 400 private-key-file path`\. On Windows, open the console and enter `attrib -r private-key-file path`\.

1. Choose a user name to access your Zeppelin notebook\.

1. Choose an Amazon S3 path for your notebook state to be stored in\.

1. Choose **Create**\. 

You can view the status of the AWS CloudFormation stack in the AWS CloudFormation console **Events** tab \([https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\)\. You can view the Amazon EC2 instances created by AWS CloudFormation in the Amazon EC2 console \([https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\)\. Search for instances that are tagged with the key name **aws\-glue\-dev\-endpoint** and value of the name of your development endpoint\.

After the notebook server is created, its status changes to **CREATE\_COMPLETE** in the Amazon EC2 console\. Details about your server also appear in the development endpoint details page\. When the creation is complete, you can connect to a notebook on the new server\.

To complete the setup of the Zeppelin notebook server, you must run a script on the Amazon EC2 instance\. This tutorial requires that you upload an SSL certificate when you create the Zeppelin server on the Amazon EC2 instance\. But there is also an SSH local port forwarding method to connect\. For additional setup instructions, see [Creating a Notebook Server Associated with a Development Endpoint](dev-endpoint-notebook-server-considerations.md)\. When the creation is complete, you can connect to a notebook on the new server using HTTPS\. 

**Note**  
For any notebook server that you create that is associated with a development endpoint, you manage it\. Therefore, if you delete the development endpoint, to delete the notebook server, you must delete the AWS CloudFormation stack on the AWS CloudFormation console\.

## Connecting to Your Notebook Server on Amazon EC2<a name="dev-endpoint-tutorial-EC2-notebook-connect"></a>

1. In the AWS Glue console, choose Dev endpoints to navigate to the development endpoints list\. Choose the name of the development endpoint for which you created a notebook server\. Choosing the name opens its details page\.

1. On the **Endpoint details** page, copy the URL labeled **HTTPS URL** for your notebook server\.

1. Open a web browser, and paste in the notebook server URL\. This lets you access the server using HTTPS on port 443\. Your browser may not recognize the server's certificate, in which case you have to override its protection and proceed anyway\.

1. Log in to Zeppelin using the user name and password that you provided when you created the notebook server\.

## Running a Simple Script Fragment in a Notebook Paragraph<a name="dev-endpoint-tutorial-EC2-notebook-run-code"></a>

1. Choose **Create new note** and name it **Legislators**\. Confirm `spark` as the **Default Interpreter**\.

1. You can verify that your notebook is now set up correctly by typing the statement `spark.version` and running it\. This returns the version of Apache Spark that is running on your notebook server\.

1. Type the following script into the next paragraph in your notebook and run it\. This script reads metadata from the **persons\_json** table that your crawler created, creates a `DynamicFrame` from the underlying data, and displays the number of records and the schema of the data\.

   ```
   %pyspark
   import sys
   from pyspark.context import SparkContext
   from awsglue.context import GlueContext
   from awsglue.transforms import *
   from awsglue.utils import getResolvedOptions
   
   # Create a Glue context
   glueContext = GlueContext(SparkContext.getOrCreate())
   
   # Create a DynamicFrame using the 'persons_json' table
   persons_DyF = glueContext.create_dynamic_frame.from_catalog(database="legislators", table_name="persons_json")
   
   # Print out information about this data
   print "Count:  ", persons_DyF.count()
   persons_DyF.printSchema()
   ```

   The output of the script should be:

   ```
    Count:  1961
    root
    |-- family_name: string
    |-- name: string
    |-- links: array
    |    |-- element: struct
    |    |    |-- note: string
    |    |    |-- url: string
    |-- gender: string
    |-- image: string
    |-- identifiers: array
    |    |-- element: struct
    |    |    |-- scheme: string
    |    |    |-- identifier: string
    |-- other_names: array
    |    |-- element: struct
    |    |    |-- note: string
    |    |    |-- name: string
    |    |    |-- lang: string
    |-- sort_name: string
    |-- images: array
    |    |-- element: struct
    |    |    |-- url: string
    |-- given_name: string
    |-- birth_date: string
    |-- id: string
    |-- contact_details: array
    |    |-- element: struct
    |    |    |-- type: string
    |    |    |-- value: string
    |-- death_date: string
   ```