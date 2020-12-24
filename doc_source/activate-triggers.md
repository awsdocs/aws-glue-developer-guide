# Activating and Deactivating Triggers<a name="activate-triggers"></a>

You can activate or deactivate a trigger using the AWS Glue console, the AWS Command Line Interface \(AWS CLI\), or the AWS Glue API\.

**Note**  
Currently, the AWS Glue console supports only jobs, not crawlers, when working with triggers\. You can use the AWS CLI or AWS Glue API to configure triggers with both jobs and crawlers\.

**To activate or deactivate a trigger \(console\)**

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\. 

1. In the navigation pane, under **ETL**, choose **Triggers**\.

1. Select the check box next to the desired trigger, and on the **Action** menu choose **Enable trigger** to activate the trigger or **Disable trigger** to deactivate the trigger\.

**To activate or deactivate a trigger \(AWS CLI\)**
+ Enter one of the following commands\.

  ```
  aws glue start-trigger --name MyTrigger  
  
  aws glue stop-trigger --name MyTrigger
  ```

  Starting a trigger activates it, and stopping a trigger deactivates it\. When you activate an on\-demand trigger, it fires immediately\.

For more information, see [AWS Glue Triggers](about-triggers.md)\.