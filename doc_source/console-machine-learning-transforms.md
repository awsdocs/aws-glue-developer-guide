# Working with Machine Learning Transforms on the AWS Glue Console<a name="console-machine-learning-transforms"></a>

You can use AWS Glue to create custom machine learning transforms that can be used to cleanse your data\. You can use these transforms when you create a job on the AWS Glue console\. 

For information about how to create a machine learning transform, see [Matching Records with AWS Lake Formation FindMatches](machine-learning.md)\.

**Topics**
+ [Transform Properties](#console-machine-learning-properties)
+ [Adding and Editing Machine Learning Transforms](#console-machine-learning-transforms-actions)
+ [Viewing Transform Details](#console-machine-learning-transforms-details)

## Transform Properties<a name="console-machine-learning-properties"></a>

To view an existing machine learning transform, sign in to the AWS Management Console, and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\. Then choose the **ML transforms** tab\.

The **Machine Learning Transforms** list displays the following properties for each transform:

**Transform name**  
The unique name you gave the transform when you created it\.

**Transform ID**  
A unique identifier of the transform\. 

**Type**  
The type of machine learning transform; for example, **Find matching records**\.

**Glue version**  
This value determines which version of AWS Glue this machine learning transform is compatible with\. Glue 1\.0 is recommended for most customers\. If the value is not set, the Glue compatibility defaults to Glue 0\.9\. For more information, see [AWS Glue Versions](https://docs.aws.amazon.com/glue/latest/dg/release-notes.html#release-notes-versions)\.

**Status**  
Indicates whether the transform is **Ready** or **Needs teaching**\. To run a machine learning transform successfully in a job, it must be `Ready`\. 

When you create a **FindMatches** transform, you specify the following configuration information:

**Primary key**  
The name of a column that uniquely identifies rows in the source table\.

**Type**  
The type of machine learning transform; for example, **Find matches**\.

**Merge matching records**  
Indicates whether the transform is to remove duplicates in the target\. The record with the lowest primary key value is written to the output of the transform\.

## Adding and Editing Machine Learning Transforms<a name="console-machine-learning-transforms-actions"></a>

You can view, delete, set up and teach, or tune a transform on the AWS Glue console\. Select the check box next to the transform in the list, choose **Action**, and then choose the action that you want to take\.

To add a new machine learning transform, choose the **Jobs** tab, and then choose **Add job**\. Follow the instructions in the **Add job** wizard to add a job with a machine learning transform such as `FindMatches`\. For more information, see [Matching Records with AWS Lake Formation FindMatches](machine-learning.md)\.

## Viewing Transform Details<a name="console-machine-learning-transforms-details"></a>

Transform details include the information that you defined when you created the transform\. To view the details of a transform, select the transform in the **Machine learning transforms** list, and review the information on the following tabs:
+ History
+ Details
+ Estimate quality

### History<a name="console-machine-learning-transforms-history"></a>

The **History** tab shows your transform task run history\. Several types of tasks are run to teach a transform\. For each task, the run metrics include the following:
+ **Run ID** is an identifier created by AWS Glue for each run of this task\.
+ **Task type** shows the type of task run\.
+ **Status** shows the success of each task listed with the most recent run at the top\.
+ **Error** shows the details of an error message if the run was not successful\.
+ **Start time** shows the date and time \(local time\) that the task started\.
+ **Execution time** shows the length of time during which the job run consumed resources\. The amount is calculated from when the job run starts consuming resources until it finishes\.
+ **Last modified** shows the date and time \(local time\) that the task was last modified\.
+ **Logs** links to the logs written to `stdout` for this job run\.

  The **Logs** link takes you to Amazon CloudWatch Logs\. There you can view the details about the tables that were created in the AWS Glue Data Catalog and any errors that were encountered\. You can manage your log retention period on the CloudWatch console\. The default log retention is `Never Expire`\. For more information about how to change the retention period, see [Change Log Data Retention in CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html#SettingLogRetention) in the *Amazon CloudWatch Logs User Guide*\.
+ **Error logs** links to the logs written to `stderr` for this task run\. 

  This link takes you to CloudWatch Logs, where you can see details about any errors that were encountered\. You can manage your log retention period on the CloudWatch console\. The default log retention is `Never Expire`\. For more information about how to change the retention period, see [Change Log Data Retention in CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html#SettingLogRetention) in the *Amazon CloudWatch Logs User Guide*\.
+ **Download label file** shows a link to Amazon S3 for a generated labeling file\.

### Details<a name="console-machine-learning-transforms-details"></a>

The **Details** tab includes attributes of your transform\. It shows you the details about the transform definition, including the following:
+ **Transform name** shows the name of the transform\.
+ **Type** lists the type of the transform\.
+ **Status** displays whether the transform is ready to be used in a script or job\.
+ **Force output to match labels** displays whether the transform forces the output to match the labels provided by the user\.

### Estimate quality<a name="console-machine-learning-transforms-metrics"></a>

The **Estimate quality** tab shows the metrics that you use to measure the quality of the transform\. Estimates are calculated by comparing the transform match predictions using a subset of your labeled data against the labels you have provided\. These estimates are approximate\.You can invoke an **Estimate quality** task run from this tab\.

The **Estimate quality** tab shows the metrics from the last **Estimate quality** run including the following properties:
+ **Area under the Precision\-Recall curve** is a single number estimating the upper bound of the overall quality of the transform\. It is independent of the choice made for the precision\-recall parameter\. Higher values indicate that you have a more attractive precision\-recall tradeoff\. 
+ **Precision** estimates how often the transform is correct when it predicts a match\.
+ **Recall upper limit** estimates that for an actual match, how often the transform predicts the match\.
+ **Max F1** estimates the transform's accuracy between 0 and 1, where 1 is the best accuracy\. For more information, see [F1 score](https://en.wikipedia.org/wiki/F1_score) in Wikipedia\.

For information about understanding quality estimates versus true quality, see [Quality Estimates Versus End\-to\-End \(True\) Quality](#console-machine-learning-quality-estimates-true-quality)\.

For more information about tuning your transform, see [Tuning Machine Learning Transforms in AWS Glue](add-job-machine-learning-transform-tuning.md)\.

### Quality Estimates Versus End\-to\-End \(True\) Quality<a name="console-machine-learning-quality-estimates-true-quality"></a>

In the `FindMatches` machine learning transform, AWS Glue estimates the quality of your transform by presenting the internal machine\-learned model with a number of pairs of records that you provided matching labels for but that the model has not seen before\. These quality estimates are a function of the quality of the machine\-learned model \(which is influenced by the number of records that you label to “teach” the transform\)\. The end\-to\-end, or *true* recall \(which is not automatically calculated by the `FindMatches` transform\) is also influenced by the `FindMatches` filtering mechanism that proposes a wide variety of possible matches to the machine\-learned model\. 

You can tune this filtering method primarily by using the **Lower Cost\-Accuracy** slider\. As you move this slider closer to the **Accuracy** end, the system does a more thorough and expensive search for pairs of records that might be matches\. More pairs of records are fed to your machine\-learned model, and your `FindMatches` transform's end\-to\-end or true recall gets closer to the estimated recall metric\. As a result, changes in the end\-to\-end quality of your matches as a result of changes in your matches's cost/accuracy tradeoff will typically not be reflected in the quality estimate\.