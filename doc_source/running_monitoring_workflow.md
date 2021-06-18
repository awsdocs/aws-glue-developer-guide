# Running and Monitoring a Workflow in AWS Glue<a name="running_monitoring_workflow"></a>


|  | 
| --- |
| The AWS Glue blueprints feature is in preview release for AWS Glue and is subject to change\. | 

If the start trigger for a workflow is an on\-demand trigger, you can start the workflow from the AWS Glue console\. Complete the following steps to run and monitor a workflow\. If the workflow fails, you can view the run graph to determine the node that failed\. To help troubleshoot, if the workflow was created from a blueprint, you can view the blueprint run to see the blueprint parameter values that were used to create the workflow\. For more information, see [Viewing Blueprint Runs in AWS Glue](viewing_blueprint_runs.md)\.

You can run and monitor a workflow by using the AWS Glue console, API, or AWS Command Line Interface \(AWS CLI\)\.

**To run and monitor a workflow \(console\)**

1. Open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, under **ETL**, choose **Workflows**\.

1. Select a workflow\. On the **Actions** menu, choose **Run**\.

1. Check the **Last run status** column in the workflows list\. Choose the refresh button to view ongoing workflow status\.

1. While the workflow is running or after it has completed \(or failed\), view the run details by completing the following steps\.

   1. Ensure that the workflow is selected, and choose the **History** tab\.

   1. Choose the current or most recent workflow run, and then choose **View run details**\.

      The workflow runtime graph shows the current run status\.

   1. Choose any node in the graph to view details and status of the node\.  
![\[The run graph shows a start trigger, which starts a job. Another trigger watches for job completion. The job node (a rectangle that encloses a clipboard icon and a job name) is selected, and the job details are shown in a pane at the right. The job details include job run ID and status.\]](http://docs.aws.amazon.com/glue/latest/dg/images/workflow-pre-select-resume.png)

**To run and monitor a workflow \(AWS CLI\)**

1. Enter the following command\. Replace *<workflow\-name>* with the workflow to run\.

   ```
   aws glue start-workflow-run --name <workflow-name>
   ```

   If the workflow is successfully started, the command returns the run ID\.

1. View workflow run status by using the `get-workflow-run` command\. Supply the workflow name and run ID\.

   ```
   aws glue get-workflow-run --name myWorkflow --run-id wr_d2af14217e8eae775ba7b1fc6fc7a42c795aed3cbcd8763f9415452e2dbc8705
   ```

   The following is sample command output\.

   ```
   {
       "Run": {
           "Name": "myWorkflow",
           "WorkflowRunId": "wr_d2af14217e8eae775ba7b1fc6fc7a42c795aed3cbcd8763f9415452e2dbc8705",
           "WorkflowRunProperties": {
               "run_state": "COMPLETED",
               "unique_id": "fee63f30-c512-4742-a9b1-7c8183bdaae2"
           },
           "StartedOn": 1578556843.049,
           "CompletedOn": 1578558649.928,
           "Status": "COMPLETED",
           "Statistics": {
               "TotalActions": 11,
               "TimeoutActions": 0,
               "FailedActions": 0,
               "StoppedActions": 0,
               "SucceededActions": 9,
               "RunningActions": 0,
               "ErroredActions": 0
           }
       }
   }
   ```

**See Also:**  
[Overview of Workflows in AWS Glue](workflows_overview.md)
[Overview of Blueprints in AWS Glue](blueprints-overview.md)