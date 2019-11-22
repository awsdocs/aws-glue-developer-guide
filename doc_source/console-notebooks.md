# Working with Notebooks on the AWS Glue Console<a name="console-notebooks"></a>

A *development endpoint* is an environment that you can use to develop and test your AWS Glue scripts\. A *notebook* enables interactive development and testing of your ETL \(extract, transform, and load\) scripts on a development endpoint\. 

AWS Glue provides an interface to Amazon SageMaker notebooks and Apache Zeppelin notebook servers\. On the AWS Glue notebooks page, you can create Amazon SageMaker notebooks and attach them to a development endpoint\. You can also manage Zeppelin notebook servers that you created and attached to a development endpoint\. To create a Zeppelin notebook server, see [Creating a Notebook Server Hosted on Amazon EC2](console-ec2-notebook-create.md)\. 

The **Notebooks** page on the AWS Glue console lists all the Amazon SageMaker notebooks and Zeppelin notebook servers in your AWS Glue environment\. You can use the console to perform several actions on your notebooks\. To display details for a notebook or notebook server, choose the notebook in the list\. Notebook details include the information that you defined when you created it using the **Create SageMaker notebook** or **Create Zeppelin Notebook server** wizard\. 

You can switch an Amazon SageMaker notebook that is attached to a development endpoint to another development endpoint as needed\. 

**To switch an Amazon SageMaker notebook to another development endpoint**

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, choose **Notebooks**\.

1. Choose the notebook in the list\. Choose **Action**, and then **Switch Dev Endpoint**\.

1. Choose an available development endpoint, and then choose **Apply**\.

   Certain IAM roles are required for this action\. For more information, see [Create an IAM Policy for Amazon SageMaker Notebooks](https://docs.aws.amazon.com/glue/latest/dg/create-sagemaker-notebook-policy.html)\.

An Amazon SageMaker notebook periodically checks whether it is connected to the attached development endpoint\. If it isn't connected, the notebook tries to reconnect automatically\.

## Amazon SageMaker Notebooks on the AWS Glue Console<a name="console-notebooks-sagemaker"></a>

The following are some of the properties for Amazon SageMaker notebooks\. The console displays some of these properties when you view the details of a notebook\.


|  | 
| --- |
|   AWS Glue only manages Amazon SageMaker notebooks in certain AWS Regions\. For more information, see [Managing Notebooks](notebooks-with-glue.md)\.    | 

Before you begin, ensure that you have permissions to manage Amazon SageMaker notebooks on the AWS Glue console\. For more information, see **AWSGlueConsoleSageMakerNotebookFullAccess** in [Step 3: Attach a Policy to IAM Users That Access AWS Glue](attach-policy-iam-user.md)\.

**Notebook name**  
The unique name of the Amazon SageMaker notebook\.

**Development endpoint**  
The name of the development endpoint that this notebook is attached to\.  
This development endpoint must have been **created after August 15, 2018**\.

**Status**  
The provisioning status of the notebook and whether it is **Ready**, **Failed**, **Starting**, **Stopping**, or **Stopped**\. 

**Failure reason**  
If the status is **Failed**, the reason for the notebook failure\.

**Instance type**  
The type of the instance used by the notebook\.

**IAM role**  
The IAM role that was used to create the Amazon SageMaker notebook\.  
This role has a trust relationship to Amazon SageMaker\. You create this role on the AWS Identity and Access Management \(IAM\) console\. When you are creating the role, choose **Amazon SageMaker**, and then attach a policy for the notebook, such as **AWSGlueServiceSageMakerNotebookRoleDefault**\. For more information, see [Step 7: Create an IAM Role for Amazon SageMaker Notebooks](create-an-iam-role-sagemaker-notebook.md)\. 

## Zeppelin Notebook Servers on the AWS Glue Console<a name="console-notebooks-zeppelin-server"></a>

The following are some of the properties for Apache Zeppelin notebook servers\. The console displays some of these properties when you view the details of a notebook\.

**Notebook server name**  
The unique name of the Zeppelin notebook server\.

**Development endpoint**  
The unique name that you give the endpoint when you create it\.

**Provisioning status**  
Describes whether the notebook server is **CREATE\_COMPLETE** or **ROLLBACK\_COMPLETE**\. 

**Failure reason**  
If the status is **Failed**, the reason for the notebook failure\.

**CloudFormation stack**  
The name of the AWS CloudFormation stack that was used to create the notebook server\. 

**EC2 instance**  
The name of Amazon EC2 instance that is created to host your notebook\. This links to the Amazon EC2 console \([https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\), where the instance is tagged with the key **aws\-glue\-dev\-endpoint** and value of the name of the development endpoint\. 

**SSH to EC2 server command**  
Enter this command in a terminal window to connect to the Amazon EC2 instance that is running your notebook server\. The Amazon EC2 address shown in this command is either public or private, depending on whether you chose to **Attach a public IP to the notebook server EC2 instance**\.

**Copy certificate**  
Example `scp` command to copy the keystore that is required to set up the Zeppelin notebook server to the Amazon EC2 instance that hosts the notebook server\. Run the command from a terminal window in the directory where the Amazon EC2 private key is located\. The key to access the Amazon EC2 instance is the parameter to the `-i` option\. You provide the *path\-to\-keystore\-file*\. The remaining part of the command is the location where the development endpoint private SSH key on the Amazon EC2 server is located\.

**HTTPS URL**  
After setting up a notebook server, enter this URL in a browser to connect to your notebook using HTTPS\.