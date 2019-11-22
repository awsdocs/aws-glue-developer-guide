# Tutorial: Set Up PyCharm Professional with a Development Endpoint<a name="dev-endpoint-tutorial-pycharm"></a>

This tutorial shows you how to connect the [PyCharm Professional](https://www.jetbrains.com/pycharm/) Python IDE running on your local machine to a development endpoint so that you can interactively run, debug, and test AWS Glue ETL \(extract, transfer, and load\) scripts before deploying them\.

To connect to a development endpoint interactively, you must have PyCharm Professional installed\. You can't do this using the free edition\.

The tutorial assumes that you have already taken the steps outlined in [Tutorial Prerequisites](dev-endpoint-tutorial-prerequisites.md)\.

**Topics**
+ [Connecting PyCharm Professional to a Development Endpoint](#dev-endpoint-tutorial-pycharm-connect)
+ [Deploying the Script to Your Development Endpoint](#dev-endpoint-tutorial-pycharm-deploy)
+ [Starting the Debug Server on `localhost` and a Local Port](#dev-endpoint-tutorial-pycharm-debug-server)
+ [Initiating Port Forwarding](#dev-endpoint-tutorial-pycharm-debug-port-forward)
+ [Running Your Script on the Development Endpoint](#dev-endpoint-tutorial-pycharm-debug-run)

## Connecting PyCharm Professional to a Development Endpoint<a name="dev-endpoint-tutorial-pycharm-connect"></a>

1. Create a new pure\-Python project in PyCharm named `legislators`\.

1. Create a file named `get_person_schema.py` in the project with the following content:

   ```
   import sys
   import pydevd
   from pyspark.context import SparkContext
   from awsglue.context import GlueContext
   from awsglue.transforms import *
   
   def main():
     # Invoke pydevd
     pydevd.settrace('169.254.76.0', port=9001, stdoutToServer=True, stderrToServer=True)
   
     # Create a Glue context
     glueContext = GlueContext(SparkContext.getOrCreate())
   
     # Create a DynamicFrame using the 'persons_json' table
     persons_DyF = glueContext.create_dynamic_frame.from_catalog(database="legislators", table_name="persons_json")
   
     # Print out information about this data
     print "Count:  ", persons_DyF.count()
     persons_DyF.printSchema()
   
   if __name__ == "__main__":
      main()
   ```

1. Do one of the following:
   + For Glue version 0\.9, download the AWS Glue Python library file, `PyGlue.zip`, from `https://s3.amazonaws.com/aws-glue-jes-prod-us-east-1-assets/etl/python/PyGlue.zip` to a convenient location on your local machine\.
   + For Glue version 1\.0, download the AWS Glue Python library file, `PyGlue.zip`, from `https://s3.amazonaws.com/aws-glue-jes-prod-us-east-1-assets/etl-1.0/python/PyGlue.zip` to a convenient location on your local machine\.

1. Add `PyGlue.zip` as a content root for your project in PyCharm:
   + In PyCharm, choose **File**, **Settings** to open the **Settings** dialog box\. \(You can also use the gear\-and\-wrench icon on the toolbar, or press `Ctrl+Alt+S`\.\)
   + Expand the `legislators` project and choose **Project Structure**\. Then in the right pane, choose **\+ Add Content Root**\.
   + Navigate to the location where you saved `PyGlue.zip`, select it, then choose **Apply**\.

    The **Settings** screen should look something like the following:  
![\[The PyCharm Settings screen with PyGlue.zip added as a content root.\]](http://docs.aws.amazon.com/glue/latest/dg/images/PyCharm_AddContentRoot.png)

   Leave the **Settings** dialog box open after you choose **Apply**\.

1. Configure deployment options to upload the local script to your development endpoint using SFTP \(this capability is available only in PyCharm Professional\):
   + In the **Settings** dialog box, expand the **Build, Execution, Deployment** section\. Choose the **Deployment** subsection\.
   + Choose the **\+** icon at the top of the middle pane to add a new server\. Give it a name and set its **Type** to `SFTP`\.
   + Set the **SFTP host** to the **Public address** of your development endpoint, as listed on its details page \(choose the name of your development endpoint in the AWS Glue console to display the details page\)\.
   + Set the **User name** to `glue`\.
   + Set the **Auth type** to **Key pair \(OpenSSH or Putty\)**\. Set the **Private key file** by browsing to the location where your development endpoint's private key file is located\. Note that PyCharm only supports DSA, RSA and ECDSA OpenSSH key types, and does not accept keys in Putty's private format\. You can use an up\-to\-date version of `ssh-keygen` to generate a key\-pair type that PyCharm accepts, using syntax like the following:

     ```
     ssh-keygen -t rsa -f my_key_file_name -C "my_email@example.com"
     ```
   + Choose **Test SFTP connection**, and allow the connection to be tested\. If the connection succeeds, choose **Apply**\.

    The **Settings** screen should now look something like the following:  
![\[The PyCharm Settings screen with an SFTP server defined.\]](http://docs.aws.amazon.com/glue/latest/dg/images/PyCharm_SFTP.png)

   Again, leave the **Settings** dialog box open after you choose **Apply**\.

1. Map the local directory to a remote directory for deployment:
   + In the right pane of the **Deployment** page, choose the middle tab at the top, labeled **Mappings**\.
   + In the **Deployment Path** column, enter a path under `/home/glue/scripts/` for deployment of your project path\.
   + Choose **Apply**\.

    The **Settings** screen should now look something like the following:  
![\[The PyCharm Settings screen after a deployment mapping.\]](http://docs.aws.amazon.com/glue/latest/dg/images/PyCharm_Mapping.png)

   Choose **OK** to close the **Settings**dialog box\.

## Deploying the Script to Your Development Endpoint<a name="dev-endpoint-tutorial-pycharm-deploy"></a>

To deploy your script to the development endpoint, choose **Tools**, **Deployment**, and then choose the name under which you set up your development endpoint, as shown in the following image:

![\[The menu item for deploying your script.\]](http://docs.aws.amazon.com/glue/latest/dg/images/PyCharm_Deploy.png)

After your script has been deployed, the bottom of the screen should look something like the following:

![\[The bottom of the PyCharm screen after a successful deployment.\]](http://docs.aws.amazon.com/glue/latest/dg/images/PyCharm_Deployed.png)

## Starting the Debug Server on `localhost` and a Local Port<a name="dev-endpoint-tutorial-pycharm-debug-server"></a>

To start the debug server, take the following steps:

1. Choose **Run**, **Edit Configuration**\.

1. Expand **Defaults** in the left pane, and choose **Python Remote Debug**\.

1. Enter a port number, such as `9001`, for the **Port**:  
![\[The PyCharm default remote debugging configuration.\]](http://docs.aws.amazon.com/glue/latest/dg/images/PyCharm_DebugServer.png)

1. Note items 2 and 3 in the instructions in this screen\. The script file that you created does import `pydevd`\. But in the call to `settrace`, it replaces `localhost` with **169\.254\.76\.0**, which is a special link local IP address that is accessible to your development endpoint\.

1. Choose **Apply** to save this default configuration\.

1. Choose the **\+** icon at the top of the screen to create a new configuration based on the default that you just saved\. In the drop\-down menu, choose **Python Remote Debug**\. Name this configuration **demoDevEndpoint**, and choose **OK**\.

1. On the **Run** menu, choose **Debug 'demoDevEndpoint'**\. Your screen should now look something like the following:  
![\[The PyCharm waiting for remote connection screen.\]](http://docs.aws.amazon.com/glue/latest/dg/images/PyCharm_Waiting.png)

## Initiating Port Forwarding<a name="dev-endpoint-tutorial-pycharm-debug-port-forward"></a>

To invoke silent\-mode remote port forwarding over SSH, open a terminal window that supports SSH, such as Bash \(or on Windows, Git Bash\)\. Type this command with the replacements that follow:

```
ssh -i private-key-file-path -nNT -g -R :9001:localhost:9001 glue@ec2-12-345-678-9.compute-1.amazonaws.com
```

**Replacements**
+ Replace `private-key-file-path` with the path to the private\-key `.pem` file that corresponds to your development endpoint's public key\.
+ Replace `ec2-12-345-678-9.compute-1.amazonaws.com` with the public address of your development endpoint\. You can find the public address in the AWS Glue console by choosing **Dev endpoints**\. Then choose the name of the development endpoint to open its **Endpoint details** page\.

## Running Your Script on the Development Endpoint<a name="dev-endpoint-tutorial-pycharm-debug-run"></a>

To run your script on your development endpoint, open another terminal window that supports SSH, and type this command with the replacements that follow:

```
ssh -i private-key-file-path \
    glue@ec2-12-345-678-9.compute-1.amazonaws.com \
    -t gluepython deployed-script-path/script-name
```

**Replacements**
+ Replace `private-key-file-path` with the path to the private\-key `.pem` file that corresponds to your development endpoint's public key\.
+ Replace `ec2-12-345-678-9.compute-1.amazonaws.com` with the public address of your development endpoint\. You can find the public address in the AWS Glue console by navigating to **Dev endpoints**\. Then choose the name of the development endpoint to open its **Endpoint details** page\.
+ Replace `-t gluepython` with `-t gluepython3` if you are running with Python 3\.
+ Replace `deployed-script-path` with the path that you entered in the **Deployment Mappings** tab \(for example, `/home/glue/scripts/legislators/`\)\.
+ Replace `script-name` with the name of the script that you uploaded \(for example, `get_person_schema.py`\)\.

PyCharm now prompts you to provide a local source file equivalent to the one being debugged remotely:

![\[The menu item for deploying your script.\]](http://docs.aws.amazon.com/glue/latest/dg/images/PyCharm_Autodetect.png)

Choose **Autodetect**\.

You are now set up to debug your script remotely on your development endpoint\.