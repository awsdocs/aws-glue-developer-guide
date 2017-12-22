# Working with Triggers on the AWS Glue Console<a name="console-triggers"></a>

A trigger controls when an ETL job runs in AWS Glue\. To view your existing triggers, sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\. Then choose the **Triggers** tab\.

The **Triggers** list displays properties for each trigger:

**Trigger name**  
The unique name you gave the trigger when you created it\.

**Trigger type**  
Indicates whether the trigger is time\-based \(**Schedule**\), event\-based \(**Jobs completed**\), or started by you \(**On\-demand**\)\.

**Trigger status**  
Indicates whether the trigger is **Enabled** or **ACTIVATED** and ready to invoke associated jobs when it fires\. The trigger can also be **Disabled** or **DEACTIVATED** and paused so that it doesn't determine whether a job is invoked\.

**Trigger parameters**  
For **Schedule** triggers, this includes the details about the frequency and time to fire the trigger\. For **Jobs completed** triggers, it includes the list of jobs to watch for completion that fire the trigger\.

**Jobs to trigger**  
Lists the jobs associated with the trigger that are invoked when this trigger fires\.

## Adding and Editing Triggers<a name="console-triggers-wizard"></a>

To edit, delete, or start a trigger, select the check box next to the trigger in the list, and then choose **Action**\. Here you can also disable a trigger to prevent it from starting any associated jobs, or enable it to start associated jobs when it fires\.

Choose a trigger in the list to view details for the trigger\. Trigger details include the information you defined when you created the trigger\.

To add a new trigger, choose **Add trigger**, and follow the instructions in the **Add trigger** wizard\. 

You provide the following properties:

**Name**  
Give your trigger a unique name\.

**Trigger type**  
Specify one of the following:  

+ **Schedule:** The trigger fires at a specific time\.

+ **Jobs completed:** The trigger fires when a job in the list completes\. For the trigger to fire, the job that completes must have been started by a trigger\.

+ **On\-demand:** The trigger fires when it is started from the triggers list page\.

**Jobs to trigger**  
List of jobs that are started by this trigger\.

For more information, see [Triggering Jobs in AWS Glue](trigger-job.md)\.