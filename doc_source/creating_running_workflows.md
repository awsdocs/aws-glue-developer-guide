# Creating and Running Workflows<a name="creating_running_workflows"></a>

You can use the AWS Glue console to create, visualize, and run workflows\. For information about managing workflows using the AWS Glue API, see [Workflows](aws-glue-api-workflow.md)\.

**Topics**
+ [Creating and Building Out a Workflow Using the Console](#creating_workflow)
+ [Running a Workflow](#running_workflow)

## Creating and Building Out a Workflow Using the Console<a name="creating_workflow"></a>

A workflow contains jobs, crawlers, and triggers\. Before creating a workflow, create the jobs and crawlers that the workflow is to include\. It is best to specify run\-on\-demand crawlers for workflows\. You can create new triggers while you are building out your workflow, or you can *clone* existing triggers into the workflow\. When you clone a trigger, all the catalog objects associated with the trigger—the jobs or crawlers that fire it and the jobs or crawlers that it starts—are added to the workflow\.

You build out your workflow by adding triggers to the workflow graph, and defining the watched events and actions for each trigger\. You begin with a *start trigger*, which can be either an on\-demand or schedule trigger, and complete the graph by adding event \(conditional\) triggers\.

### Step 1: Create the workflow<a name="workflow-step1"></a>

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, under **ETL**, choose **Workflows**\.

1. Choose **Add workflow** and complete the **Add a new ETL workflow** form\.

   Any optional default run properties that you add are made available as arguments to all jobs in the workflow\. For more information, see [Getting and Setting Workflow Run Properties](workflow-run-properties-code.md)\.

1. Choose **Add workflow**\.

   The new workflow appears in the list on the **Workflows** page\.

### Step 2: Add a start trigger<a name="workflow-step2"></a>

1. On the **Workflows** page, select your new workflow\. In the tabs at the bottom, choose **Graph**\.

1. Choose **Add trigger**, and in the **Add trigger** dialog box, do one of the following:
   + Choose **Add new**, and complete the **Add trigger** form, selecting **Schedule** or **On demand** for **Trigger Type**\. Then choose **Add**\.

     The trigger appears on the graph, along with a placeholder node \(labeled **Add node**\)\. At this point, the trigger is not yet saved\.  
![\[A graph with two rectangular nodes: a trigger, and a placeholder node. An arrow points from the trigger node to the placeholder node.\]](http://docs.aws.amazon.com/glue/latest/dg/images/graph-start-trigger.png)
   + Choose **Clone existing**, and choose a trigger to clone\. Then choose **Add**\.

     The trigger appears on the graph, along with the jobs and crawlers that it watches and the jobs and crawlers that it starts\.

     If you mistakenly selected the wrong trigger, select the trigger on the graph, and then choose **Remove**\.

1. If you added a new trigger, complete these steps:

   1. Do one of the following:
      + Choose the placeholder node \(**Add node**\)\.
      + Ensure that the start trigger is selected, and on the **Action** menu above the graph, choose **Add jobs/crawlers to trigger**\.

   1. In the **Add jobs\(s\) and crawler\(s\) to trigger** dialog box, select one or more jobs or crawlers, and then choose **Add**\.

      The trigger is saved, and the selected jobs or crawlers appear on the graph with connectors from the trigger\.

      If you mistakenly added the wrong jobs or crawlers, you can select either the trigger or a connector and choose **Remove**\.

### Step 3: \(Optional\) Add more triggers<a name="workflow-step3"></a>

Continue to build out your workflow by adding more triggers\. To zoom in or out or to enlarge the graph canvas, use the icons to the right of the graph\. For each trigger to add, complete the following steps:

1. Do one of the following:
   + To clone an existing trigger, ensure that no node on the graph is selected, and on the **Action** menu, choose **Add trigger**\.
   + To add a new trigger that watches a particular job or crawler on the graph, select the job or crawler node, and then choose the **Add trigger** placeholder node\.

     You can add more jobs or crawlers to watch for this trigger in a later step\.

1.  In the **Add trigger** dialog box, do one of the following:
   + Choose **Add new**, and complete the **Add trigger** form\. Then choose **Add**\.

     The trigger appears on the graph\. You will complete the trigger in a later step\.
   + Choose **Clone existing**, and choose a trigger to clone\. Then choose **Add**\.

     The trigger appears on the graph, along with the jobs and crawlers that it watches and the jobs and crawlers that it starts\.

     If you mistakenly chose the wrong trigger, select the trigger on the graph, and then choose **Remove**\.

1. If you added a new trigger, complete these steps:

   1. Select the new trigger\.

      As the following graph shows, placeholder nodes appear for \(1\) events to watch and \(2\) actions\.  
![\[A graph with many nodes, two of which are placeholder nodes that are called out as numbers 1 and 2.\]](http://docs.aws.amazon.com/glue/latest/dg/images/graph-dual-placeholders.png)

   1. \(Optional if the trigger already watches an event and you want to add more jobs or crawlers to watch\.\) Choose the events\-to\-watch placeholder node, and in the **Add job\(s\) and crawler\(s\) to watch** dialog box, select one or more jobs or crawlers\. Choose an event to watch \(SUCCEEDED, FAILED, etc\.\), and choose **Add**\.

   1. Ensure that the trigger is selected, and choose the actions placeholder node\.

   1. In the **Add job\(s\) and crawler\(s\) to watch dialog** box, select one or more jobs or crawlers, and choose **Add**\.

      The selected jobs and crawlers appear on the graph, with connectors from the trigger\.

## Running a Workflow<a name="running_workflow"></a>

If the start trigger for a workflow is an on\-demand trigger, you can start the workflow from the AWS Glue console\.

**To run a workflow**

1. Open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, under **ETL**, choose **Workflows**\.

1. Select a workflow\. On the **Actions** menu, choose **Run**\.