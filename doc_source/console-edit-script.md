# Working with Scripts on the AWS Glue Console<a name="console-edit-script"></a>

A script contains the code that performs extract, transform, and load \(ETL\) work\. You can provide your own script, or AWS Glue can generate a script with guidance from you\. For information about creating your own scripts, see [Providing Your Own Custom Scripts](console-custom-created.md)\.

You can edit a script in the AWS Glue console\. When you edit a script, you can add sources, targets, and transforms\.

**To edit a script**

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\. Then choose the **Jobs** tab\.

1. Choose a job in the list, and then choose **Action**, **Edit script** to open the script editor\.

   You can also access the script editor from the job details page\. Choose the  **Script** tab, and then choose **Edit script**\.

## Script Editor<a name="console-edit-script-editor"></a>

The AWS Glue script editor lets you insert, modify, and delete sources, targets, and transforms in your script\. The script editor displays both the script and a diagram to help you visualize the flow of data\.

To create a diagram for the script, choose **Generate diagram**\. AWS Glue uses annotation lines in the script beginning with **\#\#** to render the diagram\. To correctly represent your script in the diagram, you must keep the parameters in the annotations and the parameters in the Apache Spark code in sync\.

The script editor lets you add code templates wherever your cursor is positioned in the script\. At the top of the editor, choose from the following options:
+ To add a source table to the script, choose **Source**\.
+ To add a target table to the script, choose **Target**\.
+ To add a target location to the script, choose **Target location**\.
+ To add a transform to the script, choose **Transform**\. For information about the functions that are called in your script, see [Program AWS Glue ETL Scripts in Python](aws-glue-programming-python.md)\.
+ To add a Spigot transform to the script, choose **Spigot**\.

In the inserted code, modify the `parameters` in both the annotations and Apache Spark code\. For example, if you add a **Spigot** transform, verify that the `path` is replaced in both the `@args` annotation line and the `output` code line\.

The **Logs** tab shows the logs that are associated with your job as it runs\. The most recent 1,000 lines are displayed\.

The **Schema** tab shows the schema of the selected sources and targets, when available in the Data Catalog\.  