# Viewing Blueprints in AWS Glue<a name="viewing_blueprints"></a>


|  | 
| --- |
| The AWS Glue blueprints feature is in preview release for AWS Glue and is subject to change\. | 

View a blueprint to review the blueprint description, status, and parameter specifications, and to download the blueprint ZIP archive\.

You can view a blueprint by using the AWS Glue console, AWS Glue API, or AWS Command Line Interface \(AWS CLI\)\.

**To view a blueprint \(console\)**

1. Open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, choose **Blueprints**\.

1. On the **Blueprints** page, select a blueprint\. Then on the **Actions** menu, choose **View**\.

**To view a blueprint \(AWS CLI\)**
+ Enter the following command to view just the blueprint name, description, and status\. Replace *<blueprint\-name>* with the name of the blueprint to view\.

  ```
  aws glue get-blueprint --name <blueprint-name>
  ```

  The command output looks something like the following\.

  ```
  {
      "Blueprint": {
          "Name": "myDemoBP",
          "CreatedOn": 1587414516.92,
          "LastModifiedOn": 1587428838.671,
          "BlueprintLocation": "s3://awsexamplebucket1/demo/DemoBlueprintProject.zip",
          "Status": "ACTIVE"
      }
  }
  ```

  Enter the following command to also view the parameter specifications\.

  ```
  aws glue get-blueprint --name <blueprint-name>  --include-parameter-spec
  ```

  The command output looks something like the following\.

  ```
  {
      "Blueprint": {
          "Name": "myDemoBP",
          "CreatedOn": 1587414516.92,
          "LastModifiedOn": 1587428838.671,
          "ParameterSpec": "{\"WorkflowName\":{\"type\":\"String\",\"collection\":false,\"description\":null,\"defaultValue\":null,\"allowedValues\":null},\"PassRole\":{\"type\":\"String\",\"collection\":false,\"description\":null,\"defaultValue\":null,\"allowedValues\":null},\"DynamoDBTableName\":{\"type\":\"String\",\"collection\":false,\"description\":null,\"defaultValue\":null,\"allowedValues\":null},\"ScriptLocation\":{\"type\":\"String\",\"collection\":false,\"description\":null,\"defaultValue\":null,\"allowedValues\":null}}",
          "BlueprintLocation": "s3://awsexamplebucket1/demo/DemoBlueprintProject.zip",
          "Status": "ACTIVE"
      }
  }
  ```

  Add the `--include-blueprint` argument to include a URL in the output that you can paste into your browser to download the blueprint ZIP archive that AWS Glue stored\.

**See Also:**  
[Overview of Blueprints in AWS Glue](blueprints-overview.md)