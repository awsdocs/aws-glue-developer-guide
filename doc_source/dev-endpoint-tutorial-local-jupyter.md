# Tutorial: Set Up a Jupyter Notebook in JupyterLab to Test and Debug ETL Scripts<a name="dev-endpoint-tutorial-local-jupyter"></a>

In this tutorial, you connect a Jupyter notebook in JupyterLab running on your local machine to a development endpoint\. You do this so that you can interactively run, debug, and test AWS Glue extract, transform, and load \(ETL\) scripts before deploying them\. This tutorial uses Secure Shell \(SSH\) port forwarding to connect your local machine to an AWS Glue development endpoint\. For more information, see [Port forwarding](https://en.wikipedia.org/wiki/Port_forwarding) on Wikipedia\.

This tutorial assumes that you have already taken the steps outlined in [Tutorial Prerequisites](dev-endpoint-tutorial-prerequisites.md)\.

## Step 1: Install JupyterLab and Sparkmagic<a name="dev-endpoint-tutorial-local-jupyter-install"></a>

You can install JupyterLab by using `conda` or `pip`\. `conda` is an open\-source package management system and environment management system that runs on Windows, macOS, and Linux\. `pip` is the package installer for Python\.

If you're installing on macOS, you must have Xcode installed before you can install Sparkmagic\.

1. Install JupyterLab, Sparkmagic, and the related extensions\.

   ```
   $ conda install -c conda-forge jupyterlab
   $ pip install sparkmagic
   $ jupyter nbextension enable --py --sys-prefix widgetsnbextension
   $ jupyter labextension install @jupyter-widgets/jupyterlab-manager
   ```

1. Check the `sparkmagic` directory from `Location`\. 

   ```
   $ pip show sparkmagic | grep Location
   Location: /Users/username/.pyenv/versions/anaconda3-5.3.1/lib/python3.7/site-packages
   ```

1. Change your directory to the one returned for `Location`, and install the kernels for Scala and PySpark\.

   ```
   $ cd /Users/username/.pyenv/versions/anaconda3-5.3.1/lib/python3.7/site-packages
   $ jupyter-kernelspec install sparkmagic/kernels/sparkkernel
   $ jupyter-kernelspec install sparkmagic/kernels/pysparkkernel
   ```

1. Download a sample `config` file\. 

   ```
   $ curl -o ~/.sparkmagic/config.json https://raw.githubusercontent.com/jupyter-incubator/sparkmagic/master/sparkmagic/example_config.json
   ```

   In this configuration file, you can configure Spark\-related parameters like `driverMemory` and `executorCores`\.

## Step 2: Start JupyterLab<a name="dev-endpoint-tutorial-local-jupyter-start"></a>

When you start JupyterLab, your default web browser is automatically opened, and the URL `http://localhost:8888/lab/workspaces/{workspace_name}` is shown\.

```
$ jupyter lab
```

## Step 3: Initiate SSH Port Forwarding to Connect to Your Development Endpoint<a name="dev-endpoint-tutorial-local-jupyter-port-forward"></a>

Next, use SSH local port forwarding to forward a local port \(here, `8998`\) to the remote destination that is defined by AWS Glue \(`169.254.76.1:8998`\)\. 

1. Open a separate terminal window that gives you access to SSH\. In Microsoft Windows, you can use the BASH shell provided by [Git for Windows](https://git-scm.com/downloads), or you can install [Cygwin](https://www.cygwin.com/)\.

1. Run the following SSH command, modified as follows:
   + Replace `private-key-file-path` with a path to the `.pem` file that contains the private key corresponding to the public key that you used to create your development endpoint\.
   + If you're forwarding a different port than `8998`, replace `8998` with the port number that you're actually using locally\. The address `169.254.76.1:8998` is the remote port and isn't changed by you\.
   + Replace `dev-endpoint-public-dns` with the public DNS address of your development endpoint\. To find this address, navigate to your development endpoint in the AWS Glue console, choose the name, and copy the **Public address** that's listed on the **Endpoint details** page\.

   ```
   ssh -i private-key-file-path -NTL 8998:169.254.76.1:8998 glue@dev-endpoint-public-dns
   ```

   You will likely see a warning message like the following:

   ```
   The authenticity of host 'ec2-xx-xxx-xxx-xx.us-west-2.compute.amazonaws.com (xx.xxx.xxx.xx)'
   can't be established.  ECDSA key fingerprint is SHA256:4e97875Brt+1wKzRko+JflSnp21X7aTP3BcFnHYLEts.
   Are you sure you want to continue connecting (yes/no)?
   ```

   Enter **yes** and leave the terminal window open while you use JupyterLab\. 

1. Check that SSH port forwarding is working with the development endpoint correctly\.

   ```
   $ curl localhost:8998/sessions
   {"from":0,"total":0,"sessions":[]}
   ```

## Step 4: Run a Simple Script Fragment in a Notebook Paragraph<a name="dev-endpoint-tutorial-local-jupyter-list-schema"></a>

Now your notebook in JupyterLab should work with your development endpoint\. Enter the following script fragment into your notebook and run it\.

1. Check that Spark is running successfully\. The following command instructs Spark to calculate `1` and then print the value\.

   ```
   spark.sql("select 1").show()
   ```

1. Check if AWS Glue Data Catalog integration is working\. The following command lists the tables in the Data Catalog\.

   ```
   spark.sql("show tables").show()
   ```

1. Check that a simple script fragment that uses AWS Glue libraries works\.

   The following script uses the `persons_json` table metadata in the AWS Glue Data Catalog to create a `DynamicFrame` from your sample data\. It then prints out the item count and the schema of this data\. 

```
import sys
from pyspark.context import SparkContext
from awsglue.context import GlueContext
 
# Create a Glue context
glueContext = GlueContext(SparkContext.getOrCreate())
 
# Create a DynamicFrame using the 'persons_json' table
persons_DyF = glueContext.create_dynamic_frame.from_catalog(database="legislators", table_name="persons_json")
 
# Print out information about *this* data
print("Count:  ", persons_DyF.count())
persons_DyF.printSchema()
```

The output of the script is as follows\.

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

## Troubleshooting<a name="dev-endpoint-tutorial-local-jupyter-troubleshooting"></a>
+ During the installation of JupyterLab, if your computer is behind a corporate proxy or firewall, you might encounter HTTP and SSL errors due to custom security profiles managed by corporate IT departments\.

  The following is an example of a typical error that occurs when `conda` can't connect to its own repositories:

  ```
  CondaHTTPError: HTTP 000 CONNECTION FAILED for url <https://repo.anaconda.com/pkgs/main/win-64/current_repodata.json>
  ```

  This might happen because your company can block connections to widely used repositories in Python and JavaScript communities\. For more information, see [Installation Problems](https://jupyterlab.readthedocs.io/en/stable/getting_started/installation.html#installation-problems) on the JupyterLab website\.
+ If you encounter a *connection refused* error when trying to connect to your development endpoint, you might be using a development endpoint that is out of date\. Try creating a new development endpoint and reconnecting\.