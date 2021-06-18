# How AWS Glue Development Endpoints Work with SageMaker Notebooks<a name="dev-endpoint-how-it-works"></a>

One of the common ways to access your development endpoints is to use [Jupyter](https://jupyter.org/) on SageMaker notebooks\. The Jupyter notebook is an open\-source web application which is widely used in visualization, analytics, machine learning, etc\. An AWS Glue SageMaker notebook provides you a Jupyter notebook experience with AWS Glue development endpoints\. In the AWS Glue SageMaker notebook, the Jupyter notebook environment is pre\-configured with [SparkMagic](https://github.com/jupyter-incubator/sparkmagic), an open source Jupyter plugin to submit Spark jobs to a remote Spark cluster\. [Apache Livy](https://livy.apache.org) is a service that allows interaction with a remote Spark cluster over a REST API\. In the AWS Glue SageMaker notebook, SparkMagic is configured to call the REST API against a Livy server running on an AWS Glue development endpoint\. 

The following text flow explains how each component works:

*AWS Glue SageMaker notebook: \(Jupyter → SparkMagic\) → \(network\) → AWS Glue development endpoint: \(Apache Livy → Apache Spark\)*

Once you run your Spark script written in each paragraph on a Jupyter notebook, the Spark code is submitted to the Livy server via SparkMagic, then a Spark job named "livy\-session\-N" runs on the Spark cluster\. This job is called a Livy session\. The Spark job will run while the notebook session is alive\. The Spark job will be terminated when you shutdown the Jupyter kernel from the notebook, or when the session is timed out\. One Spark job is launched per notebook \(\.ipynb\) file\.

You can use a single AWS Glue development endpoint with multiple SageMaker notebook instances\. You can create multiple notebook files in each SageMaker notebook instance\. When you open an each notebook file and run the paragraphs, then a Livy session is launched per notebook file on the Spark cluster via SparkMagic\. Each Livy session corresponds to single Spark job\.

## Default Behavior for AWS Glue Development Endpoints and SageMaker Notebooks<a name="dev-endpoint-default-behavior"></a>

The Spark jobs run based on the [Spark configuration](https://spark.apache.org/docs/2.4.3/configuration.html)\. There are multiple ways to set the Spark configuration \(for example, Spark cluster configuration, SparkMagic's configuration, etc\.\)\.

By default, Spark allocates cluster resources to a Livy session based on the Spark cluster configuration\. In the AWS Glue development endpoints, the cluster configuration depends on the worker type\. Here’s a table which explains the common configurations per worker type\.


****  

|  | Standard | G\.1X | G\.2X | 
| --- | --- | --- | --- | 
| spark\.driver\.memory | 5G | 10G | 20G | 
| spark\.executor\.memory | 5G | 10G | 20G | 
| spark\.executor\.cores | 4 | 8 | 16 | 
| spark\.dynamicAllocation\.enabled | TRUE | TRUE | TRUE | 

The maximum number of Spark executors is automatically calculated by combination of DPU \(or `NumberOfWorkers`\) and worker type\. 


****  

|  | Standard | G\.1X | G\.2X | 
| --- | --- | --- | --- | 
| The number of max Spark executors | \(DPU \- 1\) \* 2 \- 1 | \(NumberOfWorkers \- 1\)  | \(NumberOfWorkers \- 1\)  | 

For example, if your development endpoint has 10 workers and the worker type is `G.1X`, then you will have 9 Spark executors and the entire cluster will have 90G of executor memory since each executor will have 10G of memory\.

Regardless of the specified worker type, Spark dynamic resource allocation will be turned on\. If a dataset is large enough, Spark may allocate all the executors to a single Livy session since `spark.dynamicAllocation.maxExecutors` is not set by default\. This means that other Livy sessions on the same dev endpoint will wait to launch new executors\. If the dataset is small, Spark will be able to allocate executors to multiple Livy sessions at the same time\.

**Note**  
For more information about how resources are allocated in different use cases and how you set a configuration to modify the behavior, see [Advanced Configuration: Sharing Development Endpoints among Multiple Users](dev-endpoint-sharing.md)\.