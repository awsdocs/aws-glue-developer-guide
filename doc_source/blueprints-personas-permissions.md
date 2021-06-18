# Permissions for Personas and Roles for AWS Glue Blueprints<a name="blueprints-personas-permissions"></a>


|  | 
| --- |
| The AWS Glue blueprints feature is in preview release for AWS Glue and is subject to change\. | 

The following are the typical personas and suggested AWS Identity and Access Management \(IAM\) permissions policies for personas and roles for AWS Glue blueprints\.

**Topics**
+ [Blueprint Personas](#blueprints-personas)
+ [Permissions for Blueprint Personas](#blueprints-permssions)
+ [Permissions for Blueprint Roles](#blueprints-role-permissions)

## Blueprint Personas<a name="blueprints-personas"></a>

The following are the personas typically involved in the lifecycle of AWS Glue blueprints\.


| Persona | Description | 
| --- | --- | 
| AWS Glue developer | Develops, tests, and publishes blueprints\. | 
| AWS Glue administrator | Registers, maintains, and grants permissions on blueprints\. | 
| Data analyst | Runs blueprints to create workflows\. | 

For more information, see [Overview of Blueprints in AWS Glue](blueprints-overview.md)\.

## Permissions for Blueprint Personas<a name="blueprints-permssions"></a>

The following are the suggested permissions for each blueprint persona\.



### AWS Glue Developer Permissions for Blueprints<a name="bp-persona-dev"></a>

The AWS Glue developer must have write permissions on the Amazon S3 bucket that is used to publish the blueprint\. Often, the developer registers the blueprint after uploading it\. In that case, the developer needs the permissions listed in [AWS Glue Administrator Permissions for Blueprints](#bp-persona-admin)\. Additionally, if the developer wishes to test the blueprint after its registered, he or she also needs the permissions listed in [Data Analyst Permissions for Blueprints](#bp-persona-analyst)\. 

### AWS Glue Administrator Permissions for Blueprints<a name="bp-persona-admin"></a>

The following policy grants permissions to register, view, and maintain AWS Glue blueprints\.

**Important**  
In the following policy, replace *<s3\-bucket\-name>* and *<prefix>* with the Amazon S3 path to uploaded blueprint ZIP archives to register\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "glue:CreateBluePrint",
                "glue:UpdateBlueprint",
                "glue:DeleteBluePrint",                
                "glue:GetBluePrint",
                "glue:ListBlueprints",
                "glue:BatchGetBlueprints"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::<s3-bucket-name>/<prefix>/*"
        }
    ]
}
```

### Data Analyst Permissions for Blueprints<a name="bp-persona-analyst"></a>

The following policy grants permissions to run blueprints and to view the resulting workflow and workflow components\. It also grants `PassRole` on the role that AWS Glue assumes to create the workflow and workflow components\.

The policy grants permissions on any resource\. If you want to configure fine\-grained access to individual blueprints, use the following format for blueprint ARNs:

```
arn:aws:glue:<region>:<account-id>:blueprint/<blueprint-name>
```

**Important**  
In the following policy, replace *<account\-id>* with a valid AWS account and replace *<role\-name>* with the name of the role used to run a blueprint\. See [Permissions for Blueprint Roles](#blueprints-role-permissions) for the permissions that this role requires\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "glue:ListBlueprints",
                "glue:GetBlueprint",
                "glue:StartBlueprintRun",
                "glue:GetBlueprintRun",
                "glue:GetBlueprintRuns",
                "glue:GetCrawler",
                "glue:ListTriggers",
                "glue:ListJobs",
                "glue:BatchGetCrawlers",
                "glue:GetTrigger",
                "glue:BatchGetWorkflows",
                "glue:BatchGetTriggers",
                "glue:BatchGetJobs",
                "glue:BatchGetBlueprints",
                "glue:GetWorkflowRun",
                "glue:GetWorkflowRuns",
                "glue:ListCrawlers",
                "glue:ListWorkflows",
                "glue:GetJob",
                "glue:GetWorkflow",
                "glue:StartWorkflowRun"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "arn:aws:iam::<account-id>:role/<role-name>"
        }
    ]
}
```

## Permissions for Blueprint Roles<a name="blueprints-role-permissions"></a>

The following are the suggested permissions for the IAM role used to create a workflow from a blueprint\. The role has to have a trust relationship with `glue.amazonaws.com`\.

**Important**  
In the following policy, replace *<account\-id>* with a valid AWS account, and replace *<role\-name>* with the name of the role\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "glue:CreateJob",
                "glue:GetCrawler",
                "glue:GetTrigger",
                "glue:DeleteCrawler",
                "glue:CreateTrigger",
                "glue:DeleteTrigger",
                "glue:DeleteJob",
                "glue:CreateWorkflow",
                "glue:DeleteWorkflow",
                "glue:GetJob",
                "glue:GetWorkflow",
                "glue:CreateCrawler"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "arn:aws:iam::<account-id>:role/<role-name>"
        }
    ]
}
```

**Note**  
If the jobs and crawlers in the workflow assume a role other than this role, this policy must include the `iam:PassRole` permission on that other role instead of on the blueprint role\.