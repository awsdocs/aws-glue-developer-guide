# Tutorial: Set Up a Local Apache Zeppelin Notebook to Test and Debug ETL Scripts<a name="dev-endpoint-tutorial-local-notebook"></a>

In this tutorial, you connect an Apache Zeppelin Notebook on your local machine to a development endpoint so that you can interactively run, debug, and test AWS Glue ETL \(extract, transform, and load\) scripts before deploying them\. This tutorial uses SSH port forwarding to connect your local machine to an AWS Glue development endpoint\. For more information, see [Port forwarding](https://en.wikipedia.org/wiki/Port_forwarding) in Wikipedia\.

The tutorial assumes that you have already taken the steps outlined in [Tutorial Prerequisites](dev-endpoint-tutorial-prerequisites.md)\.

## Installing an Apache Zeppelin Notebook<a name="dev-endpoint-tutorial-local-notebook-zeppelin"></a>

1. Make sure that you have Java Development Kit 1\.7 installed on your local machine \(see [the Java home page](https://www.java.com/en/)\)\.

   If you are running on Microsoft Windows, make sure that the `JAVA_HOME` environment variable points to the right Java directory\. It's possible to update Java without updating this variable, and if it points to a folder that no longer exists, Zeppelin fails to start\.

1. Download the version of Apache Zeppelin with all interpreters from [the Zeppelin download page](http://zeppelin.apache.org/download.html) onto your local machine\. Choose the file to download according to the following compatibility table, and follow the download instructions\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/dev-endpoint-tutorial-local-notebook.html)

   Start Zeppelin in the way that's appropriate for your operating system\. Leave the terminal window that starts the notebook server open while you are using Zeppelin\. When the server has started successfully, you can see a line in the console that ends with "Done, zeppelin server started\." 

1. Open Zeppelin in your browser by navigating to `http://localhost:8080`\.

1. In Zeppelin in the browser, open the drop\-down menu at **anonymous** in the upper\-right corner of the page, and choose **Interpreter**\. On the interpreters page, search for `spark`, and choose **edit** on the right\. Make the following changes:
   + Select the **Connect to existing process** check box, and then set **Host** to `localhost` and **Port** to `9007` \(or whatever other port you are using for port forwarding\)\.
   + In **Properties**, set **master** to `yarn-client`\.
   + If there is a `spark.executor.memory` property, delete it by choosing the **x** in the **action** column\.
   + If there is a `spark.driver.memory` property, delete it by choosing the **x** in the **action** column\.

   Choose **Save** at the bottom of the page, and then choose **OK** to confirm that you want to update the interpreter and restart it\. Use the browser back button to return to the Zeppelin start page\.

## Initiating SSH Port Forwarding to Connect to Your DevEndpoint<a name="dev-endpoint-tutorial-local-notebook-port-forward"></a>

Next, use SSH local port forwarding to forward a local port \(here, `9007`\) to the remote destination defined by AWS Glue \(`169.254.76.1:9007`\)\. 

Open a terminal window that gives you access to the SSH secure\-shell protocol\. On Microsoft Windows, you can use the BASH shell provided by [Git for Windows](https://git-scm.com/downloads), or install [Cygwin](https://www.cygwin.com/)\.

Run the following SSH command, modified as follows:
+ Replace `private-key-file-path` with a path to the `.pem` file that contains the private key corresponding to the public key that you used to create your development endpoint\.
+ If you are forwarding a different port than `9007`, replace `9007` with the port number that you are actually using locally\. The address, `169.254.76.1:9007`, is the remote port and not changed by you\.
+ Replace `dev-endpoint-public-dns` with the public DNS address of your development endpoint\. To find this address, navigate to your development endpoint in the AWS Glue console, choose the name, and copy the **Public address** that's listed in the **Endpoint details** page\.

```
ssh -i private-key-file-path -NTL 9007:169.254.76.1:9007 glue@dev-endpoint-public-dns
```

You will likely see a warning message like the following:

```
The authenticity of host 'ec2-xx-xxx-xxx-xx.us-west-2.compute.amazonaws.com (xx.xxx.xxx.xx)'
can't be established.  ECDSA key fingerprint is SHA256:4e97875Brt+1wKzRko+JflSnp21X7aTP3BcFnHYLEts.
Are you sure you want to continue connecting (yes/no)?
```

Type **yes** and leave the terminal window open while you use your Zeppelin notebook\. 

## Running a Simple Script Fragment in a Notebook Paragraph<a name="dev-endpoint-tutorial-local-notebook-list-schema"></a>

In the Zeppelin start page, choose **Create new note**\. Name the new note `Legislators`, and confirm `spark` as the interpreter\.

Type the following script fragment into your notebook and run it\. It uses the person's metadata in the AWS Glue Data Catalog to create a DynamicFrame from your sample data\. It then prints out the item count and the schema of this data\.

```
%pyspark
import sys
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.transforms import *

# Create a Glue context
glueContext = GlueContext(SparkContext.getOrCreate())

# Create a DynamicFrame using the 'persons_json' table
persons_DyF = glueContext.create_dynamic_frame.from_catalog(database="legislators", table_name="persons_json")

# Print out information about this data
print "Count:  ", persons_DyF.count()
persons_DyF.printSchema()
```

The output of the script is as follows:

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

## Troubleshooting Your Local Notebook Connection<a name="dev-endpoint-tutorial-local-notebook-troubleshooting"></a>
+ If you encounter a *connection refused* error, you might be using a development endpoint that is out of date\. Try creating a new development endpoint and reconnecting\.
+ If your connection times out or stops working for any reason, you may need to take the following steps to restore it:

  1. In Zeppelin, in the drop\-down menu in the upper\-right corner of the page, choose **Interpreters**\. On the interpreters page, search for `spark`\. Choose **edit**, and clear the **Connect to existing process** check box\. Choose **Save** at the bottom of the page\.

  1. Initiate SSH port forwarding as described earlier\.

  1. In Zeppelin, re\-enable the `spark` interpreter's **Connect to existing process** settings, and then save again\.

  Resetting the interpreter like this should restore the connection\. Another way to accomplish this is to choose **restart** for the Spark interpreter on the **Interpreters** page\. Then wait for up to 30 seconds to ensure that the remote interpreter has restarted\.
+ Ensure your development endpoint has permission to access the remote Zeppelin interpreter\. Without the proper networking permissions you might encounter errors such as open failed: connect failed: Connection refused\.