# Creating a Workflow from a Blueprint in AWS Glue<a name="creating_workflow_blueprint"></a>


|  | 
| --- |
| The AWS Glue blueprints feature is in preview release for AWS Glue and is subject to change\. | 

You can create an AWS Glue workflow manually, adding one component at a time, or you can create a workflow from an AWS Glue blueprint\. AWS Glue includes blueprints for common use cases\. Your AWS Glue developers can create additional blueprints\.

When you use a blueprint, you can quickly generate a workflow for a specific use case based on the generalized use case defined by the blueprint\. You define the specific use case by providing values for the blueprint parameters\. For example, a blueprint that partitions a dataset could have the Amazon S3 source and target paths as parameters\.

AWS Glue creates a workflow from a blueprint by *running* the blueprint\. The blueprint run saves the parameter values that you supplied, and is used to track the progress and outcome of the creation of the workflow and its components\. When troubleshooting a workflow, you can view the blueprint run to determine the blueprint parameter values that were used to create a workflow\.

To create and view workflows, you require certain IAM permissions\. For a suggested IAM policy, see [Data Analyst Permissions for Blueprints](blueprints-personas-permissions.md#bp-persona-analyst)\.

You can create a workflow from a blueprint by using the AWS Glue console, AWS Glue API, or AWS Command Line Interface \(AWS CLI\)\.

**To create a workflow from a blueprint \(console\)**

1. Open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

   Sign in as a user that has permissions to create a workflow\.

1. In the navigation pane, choose **Blueprints**\.

1. Select a blueprint, and on the **Actions** menu, choose **Create workflow**\. 

1. On the **Create a workflow from <blueprint\-name>** page, enter the following information:  
**Blueprint parameters**  
These vary depending on the blueprint design\. For questions about the parameters, see the developer\. Blueprints typically include a parameter for the workflow name\.  
**IAM role**  
The role that AWS Glue assumes to create the workflow and its components\. The role must have permissions to create and delete workflows, jobs, crawlers, and triggers\. For a suggested policy for the role, see [Permissions for Blueprint Roles](blueprints-personas-permissions.md#blueprints-role-permissions)\.

1. Choose **Submit**\.

   The **Blueprint Details** page appears, showing a list of blueprint runs at the bottom\.

1. In the blueprint runs list, check the topmost blueprint run for workflow creation status\. 

   The initial status is `RUNNING`\. Choose the refresh button until the status goes to `SUCCEEDED` or `FAILED`\. 

1. Do one of the following:
   + If the completion status is `SUCCEEDED`, you can go to the **Workflows** page, select the newly created workflow, and run it\. Before running the workflow, you can review the design graph\.
   + If the completion status is `FAILED`, select the blueprint run, and on the **Actions** menu, choose **View** to see the error message\.

**See Also**  
[Overview of Workflows in AWS Glue](workflows_overview.md)
[Overview of Blueprints in AWS Glue](blueprints-overview.md)
[Updating a Blueprint in AWS Glue](updating_blueprints.md)
[Creating and Building Out a Workflow Manually in AWS Glue](creating_running_workflows.md)