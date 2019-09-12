# Overview of Workflows<a name="workflows_overview"></a>

In AWS Glue, you can use workflows to create and visualize complex extract, transform, and load \(ETL\) activities involving multiple crawlers, jobs, and triggers\. Each workflow manages the execution and monitoring of all its components\. As a workflow runs each component, it records execution progress and status, providing you with an overview of the larger task and the details of each step\. The AWS Glue console provides a visual representation of a workflow as a graph\.

To share and manage state throughout a workflow run, you can define default workflow run properties\. These properties, which are name/value pairs, are available to all the jobs in the workflow\. Using the AWS Glue API, jobs can retrieve the workflow run properties and modify them for jobs that come later in the workflow\.

The following image shows the graph of a basic workflow on the AWS Glue console\. Your workflow could have dozens of components\.

![\[Console screenshot showing the Graph tab of a workflow. The graph contains 5 icons representing a schedule trigger, 2 jobs, an event success trigger, and a crawler that updates the schema.\]](http://docs.aws.amazon.com/glue/latest/dg/images/graph-complete-with-tabs.png)

This workflow is started by a schedule trigger, which starts two jobs\. Upon successful completion of both jobs, an event trigger starts a crawler\.

## Static and Dynamic Workflow Views<a name="design-and-run-graphs"></a>

For each workflow, there is the notion of *static view* and *dynamic view*\. The static view indicates the design of the workflow\. The dynamic view is a run time view that includes the latest run information for each of the jobs and crawlers\. Run information includes success status and error details\. 

When a workflow is running, the console displays the dynamic view, graphically indicating the jobs that have completed and that are yet to be run\. You can also retrieve a dynamic view of a running workflow using the AWS Glue API\. For more information, see [Querying Workflows Using the AWS Glue API](workflows_api_concepts.md)\.

## Workflow Restrictions<a name="workflow-restrictions"></a>

Keep the following workflow restrictions in mind:
+ A trigger can be associated with only one workflow\.
+ Only one starting trigger \(on\-demand or schedule\) is permitted\.