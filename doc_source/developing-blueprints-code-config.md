# Creating the Configuration File<a name="developing-blueprints-code-config"></a>

The blueprint configuration file is a required file that defines the script entry point for generating the workflow, and the parameters that the blueprint accepts\. The file must be named `blueprint.cfg`\.

Here is a sample configuration file\.

```
{
    "layoutGenerator": "DemoBlueprintProject.Layout.generate_layout",
    "parameterSpec" : {
           "WorkflowName" : {
                "type": "String",
                "collection": false
           },
           "WorkerType" : {
                "type": "String",
                "collection": false,
                "allowedValues": ["G1.X", "G2.X"],
                "defaultValue": "G1.X"
           },
           "Dpu" : {
                "type" : "Integer",
                "allowedValues" : [2, 4, 6],
                "defaultValue" : 2
           },
           "DynamoDBTableName": {
                "type": "String",
                "collection" : false
           },
           "ScriptLocation" : {
                "type": "String",
                "collection": false
    	}
    }
}
```

The `layoutGenerator` property specifies the fully qualified name of the function in the script that generates the layout\.

The `parameterSpec` property specifies the parameters that this blueprint accepts\. For more information, see [Specifying Blueprint Parameters](developing-blueprints-code-parameters.md)\.

**Important**  
Your configuration file must include the workflow name as a blueprint parameter, or you must generate a unique workflow name in your layout script\.