# Querying Workflows Using the AWS Glue API<a name="workflows_api_concepts"></a>

AWS Glue provides a rich API for managing workflows\. You can retrieve a static view of a workflow or a dynamic view of a running workflow using the AWS Glue API\. For more information, see [Workflows](aws-glue-api-workflow.md)\.

**Topics**
+ [Querying Static Views](#workflows_api_concepts_static)
+ [Querying Dynamic Views](#workflows_api_concepts_dynamic)

## Querying Static Views<a name="workflows_api_concepts_static"></a>

Use the `GetWorkflow` API operation to get a static view that indicates the design of a workflow\. This operation returns a directed graph consisting of nodes and edges, where a node represents a trigger, a job, or a crawler\. Edges define the relationships between nodes\. They are represented by connectors \(arrows\) on the graph in the AWS Glue console\. You can also use this operation with popular graph\-processing libraries such as NetworkX, igraph, JGraphT, and the Java Universal Network/Graph \(JUNG\) Framework\. Because all these libraries represent graphs similarly, minimal transformations are needed\.

The static view returned by this API is the most up\-to\-date view according to the latest definition of triggers associated with the workflow\.

### Graph Definition<a name="workflows_api_concepts_static_graph"></a>

A workflow graph G is an ordered pair \(N, E\), where N is a set of nodes and E a set of edges\. *Node* is a vertex in the graph identified by a unique number\. A node can be of type trigger, job, or crawler; for example: `{name:T1, type:Trigger, uniqueId:1}, {name:J1, type:Job, uniqueId:2}`

*Edge* is a 2\-tuple of the form \(`src, dest`\), where `src` and `dest` are nodes and there is a directed edge from `src` to `dest`\. 

### Example of Querying a Static View<a name="workflows_api_concepts_static_example"></a>

Consider a conditional trigger T, which triggers job J2 upon completion of job J1\. 

```
J1 ---> T ---> J2
```

Nodes: J1, T, J2 

Edges: \(J1, T\), \(T, J2\)

## Querying Dynamic Views<a name="workflows_api_concepts_dynamic"></a>

Use the `GetWorkflowRun` API operation to get a dynamic view of a running workflow\. This operation returns the same static view of the graph along with metadata related to the workflow run\.

For instance, nodes representing jobs in the `GetWorkflowRun` call have a list of job runs initiated as part of the latest run of the workflow\. You can use this list to display the run status of each job in the graph itself\. For downstream dependencies that are not yet executed, this field is set to `null`\. The graphed information makes you aware of the current state of any workflow at any point of time\.

The dynamic view returned by this API is as per the static view that was present when the workflow run was started\.

*Run\-time nodes example:* `{name:T1, type: Trigger, uniqueId:1}`, `{name:J1, type:Job, uniqueId:2, jobDetails:{jobRuns}}`, `{name:C1, type:Crawler, uniqueId:3, crawlerDetails:{crawls}}` 

### Example 1: Dynamic View<a name="workflows_api_concepts_dynamic_examples"></a>

The following example illustrates a simple two\-trigger workflow\. 
+ Nodes: t1, j1, t2, j2 
+ Edges: \(t1, j1\), \(j1, t2\), \(t2, j2\)

The `GetWorkflow` response contains the following\.

```
{
    Nodes : [
        {
            "type" : Trigger,
            "name" : "t1",
            "uniqueId" : 1
        },
        {
            "type" : Job,
            "name" : "j1",
            "uniqueId" : 2
        },
        {
            "type" : Trigger,
            "name" : "t2",
            "uniqueId" : 3
        },
        {
            "type" : Job,
            "name" : "j2",
            "uniqueId" : 4
        }
    ],
    Edges : [
        {
            "sourceId" : 1,
            "destinationId" : 2
        },
        {
            "sourceId" : 2,
            "destinationId" : 3
        },
        {
            "sourceId" : 3,
            "destinationId" : 4
        }
}
```

The `GetWorkflowRun` response contains the following\.

```
{
    Nodes : [
        {
            "type" : Trigger,
            "name" : "t1",
            "uniqueId" : 1,
            "jobDetails" : null,
            "crawlerDetails" : null
        },
        {
            "type" : Job,
            "name" : "j1",
            "uniqueId" : 2,
            "jobDetails" : [
                {
                    "id" : "jr_12334",
                    "jobRunState" : "SUCCEEDED",
                    "errorMessage" : "error string"
                }
            ],
            "crawlerDetails" : null
        },
        {
            "type" : Trigger,
            "name" : "t2",
            "uniqueId" : 3,
            "jobDetails" : null,
            "crawlerDetails" : null
        },
        {
            "type" : Job,
            "name" : "j2",
            "uniqueId" : 4,
            "jobDetails" : [
                {
                    "id" : "jr_1233sdf4",
                    "jobRunState" : "SUCCEEDED",
                    "errorMessage" : "error string"
                }
            ],
            "crawlerDetails" : null
        }
    ],
    Edges : [
        {
            "sourceId" : 1,
            "destinationId" : 2
        },
        {
            "sourceId" : 2,
            "destinationId" : 3
        },
        {
            "sourceId" : 3,
            "destinationId" : 4
        }
}
```

### Example 2: Multiple Jobs with a Conditional Trigger<a name="workflows_api_concepts_dynamic_example_2"></a>

The following example shows a workflow with multiple jobs and a conditional trigger \(t3\)\.

```
Consider Flow:
T(t1) ---> J(j1) ---> T(t2) ---> J(j2)
             |                    |
             |                    |
             >+------> T(t3) <-----+
                        |
                        |
                      J(j3)

Graph generated:
Nodes: t1, t2, t3, j1, j2, j3
Edges: (t1, j1), (j1, t2), (t2, j2), (j1, t3), (j2, t3), (t3, j3)
```