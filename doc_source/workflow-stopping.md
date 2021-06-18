# Stopping a Workflow Run<a name="workflow-stopping"></a>

You can use the AWS Glue console, AWS Command Line Interface \(AWS CLI\) or AWS Glue API to stop a workflow run\. When you stop a workflow run, all running jobs and crawlers are immediately terminated, and jobs and crawlers that are not yet started never start\. It might take up to a minute for all running jobs and crawlers to stop\. The workflow run status goes from **Running** to **Stopping**, and when the workflow run is completely stopped, the status goes to **Stopped**\.

After the workflow run is stopped, you can view the run graph to see which jobs and crawlers completed and which never started\. You can then determine if you must perform any steps to ensure data integrity\. Stopping a workflow run causes no automatic rollback operations to be performed\.

**To stop a workflow run \(console\)**

1. Open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, under **ETL**, choose **Workflows**\.

1. Choose a running workflow, and then choose the **History** tab\.

1. Choose the workflow run, and then choose **Stop run**\.

   The run status changes to **Stopping**\.

1. \(Optional\) Choose the workflow run, choose **View run details**, and review the run graph\.

**To stop a workflow run \(AWS CLI\)**
+ Enter the following command\. Replace *<workflow\-name>* with the name of the workflow and *<run\-id>* with the run ID of the workflow run to stop\.

  ```
  aws glue stop-workflow-run --name <workflow-name> --run-id <run-id>
  ```

  The following is an example of the stop\-workflow\-run command\.

  ```
  aws glue stop-workflow-run --name my-workflow --run-id wr_137b88917411d128081069901e4a80595d97f719282094b7f271d09576770354
  ```