# Editing Scripts in AWS Glue<a name="edit-script"></a>

A script contains the code that extracts data from sources, transforms it, and loads it into targets\. AWS Glue runs a script when it starts a job\.

AWS Glue ETL scripts can be coded in Python or Scala\. Python scripts use a language that is an extension of the PySpark Python dialect for extract, transform, and load \(ETL\) jobs\. The script contains *extended constructs* to deal with ETL transformations\. When you automatically generate the source code logic for your job, a script is created\. You can edit this script, or you can provide your own script to process your ETL work\.

 For information about defining and editing scripts using the AWS Glue console, see [Working with Scripts on the AWS Glue Console](console-edit-script.md)\.

## Defining a Script<a name="script-defining"></a>

Given a source and target, AWS Glue can generate a script to transform the data\. This proposed script is an initial version that fills in your sources and targets, and suggests transformations in PySpark\. You can verify and modify the script to fit your business needs\. Use the script editor in AWS Glue to add arguments that specify the source and target, and any other arguments that are required to run\. Scripts are run by jobs, and jobs are started by triggers, which can be based on a schedule or an event\. For more information about triggers, see [Triggering Jobs in AWS Glue](trigger-job.md)\.

In the AWS Glue console, the script is represented as code\. You can also view the script as a diagram that uses annotations \(\#\#\) embedded in the script\.  These annotations describe the parameters, transform types, arguments, inputs, and other characteristics of the script that are used to generate a diagram in the AWS Glue console\.

The diagram of the script shows the following:

+ Source inputs to the script

+ Transforms 

+ Target outputs written by the script

Scripts can contain the following annotations:


| Annotation | Usage | 
| --- | --- | 
| @params | Parameters from the ETL job that the script requires\. | 
| @type | Type of node in the diagram, such as the transform type, data source, or data sink\. | 
| @args | Arguments passed to the node, except reference to input data\. | 
| @return | Variable returned from script\. | 
| @inputs | Data input to node\. | 

To learn about the code constructs within a script, see [Program AWS Glue ETL Scripts in Python](aws-glue-programming-python.md)\.