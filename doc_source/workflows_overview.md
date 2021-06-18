# Overview of Workflows in AWS Glue<a name="workflows_overview"></a>


|  | 
| --- |
| The AWS Glue blueprints feature is in preview release for AWS Glue and is subject to change\. | 

In AWS Glue, you can use workflows to create and visualize complex extract, transform, and load \(ETL\) activities involving multiple crawlers, jobs, and triggers\. Each workflow manages the execution and monitoring of all its jobs and crawlers\. As a workflow runs each component, it records execution progress and status\. This provides you with an overview of the larger task and the details of each step\. The AWS Glue console provides a visual representation of a workflow as a graph\.

You can create a workflow from an AWS Glue blueprint, or you can manually build a workflow a component at a time using the AWS Management Console or the AWS Glue API\. For more information about blueprints, see [Overview of Blueprints in AWS Glue](blueprints-overview.md)\.

Event triggers within workflows can be fired by both jobs or crawlers, and can start both jobs and crawlers\. Thus you can create large chains of interdependent jobs and crawlers\.

To share and manage state throughout a workflow run, you can define default workflow run properties\. These properties, which are name/value pairs, are available to all the jobs in the workflow\. Using the AWS Glue API, jobs can retrieve the workflow run properties and modify them for jobs that come later in the workflow\.

The following image shows the graph of a very basic workflow on the AWS Glue console\. Your workflow could have dozens of components\.

![\[Console screenshot that shows the Graph tab of a workflow. The graph contains five icons that represent a schedule trigger, two jobs, an event success trigger, and a crawler that updates the schema.\]](http://docs.aws.amazon.com/glue/latest/dg/images/graph-complete-with-tabs.png)

This workflow is started by a schedule trigger, `Month-close1`, which starts two jobs, `De-duplicate` and `Fix phone numbers`\. Upon successful completion of both jobs, an event trigger, `Fix/De-dupe succeeded`, starts a crawler, `Update schema`\.

## Static and Dynamic Workflow Views<a name="design-and-run-graphs"></a>

For each workflow, there is the notion of *static view* and *dynamic view*\. The static view indicates the design of the workflow\. The dynamic view is a runtime view that includes the latest run information for each of the jobs and crawlers\. Run information includes success status and error details\. 

When a workflow is running, the console displays the dynamic view, graphically indicating the jobs that have completed and that are yet to be run\. You can also retrieve a dynamic view of a running workflow using the AWS Glue API\. For more information, see [Querying Workflows Using the AWS Glue API](workflows_api_concepts.md)\.

**See Also**  
[Creating a Workflow from a Blueprint in AWS Glue](creating_workflow_blueprint.md)
[Creating and Building Out a Workflow Manually in AWS Glue](creating_running_workflows.md)
[Workflows](aws-glue-api-workflow.md) \(for the workflows API\)