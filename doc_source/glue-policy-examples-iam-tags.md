# AWS Glue Identity\-Based \(IAM\) Access Control Policy with Tags Examples<a name="glue-policy-examples-iam-tags"></a>

This section contains example AWS Identity and Access Management \(IAM\) policies with tags to control permissions for various AWS Glue actions and resources\. You can copy these examples and edit them on the IAM console\. Then you can attach them to IAM identities such as users, roles, and groups\.

You can control access to crawlers, jobs, triggers, and development endpoints by attaching tags and specifying `resourceTag` conditions in IAM policies\. 

## Example Access Control Using Tags<a name="tags-control-access-example-triggers-allow"></a>

 For example, suppose that you want to limit access to a trigger `t2` to a specific user named `Tom` in your account\. All other users, including `Sam`, have access to trigger `t1`\. The triggers `t1` and `t2` have the following properties\. 

```
aws glue get-triggers
{
    "Triggers": [
        {
            "State": "CREATED",
            "Type": "SCHEDULED",
            "Name": "t1",
            "Actions": [
                {
                    "JobName": "j1"
                }
            ],
            "Schedule": "cron(0 0/1 * * ? *)"
        },
        {
            "State": "CREATED",
            "Type": "SCHEDULED",
            "Name": "t2",
            "Actions": [
                {
                    "JobName": "j1"
                }
            ],
            "Schedule": "cron(0 0/1 * * ? *)"
        }
    ]
}
```

The AWS Glue administrator attached a tag value `Tom` \(`glue:resourceTag/Name": "Tom"`\) to trigger `t2`\. The AWS Glue administrator also gave Tom an IAM policy with a condition statement based on the tag\. As a result, Tom can only use an AWS Glue operation that acts on resources with the tag value `Tom`\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
         "Effect": "Allow",
         "Action": "glue:*",
         "Resource": "*",
         "Condition": {
             "StringEquals": {
                "glue:resourceTag/Name": "Tom"
              }
          }
         }
    ]
}
```

When Tom tries to access the trigger `t1`, he receives an access denied message\. Meanwhile, he can successfully retrieve trigger `t2`\. 

```
aws glue get-trigger --name t1

An error occurred (AccessDeniedException) when calling the GetTrigger operation: User: Tom is not authorized to perform: glue:GetTrigger on resource: arn:aws:glue:us-east-1:123456789012:trigger/t1

aws glue get-trigger --name t2
{
    "Trigger": {
        "State": "CREATED",
        "Type": "SCHEDULED",
        "Name": "t2",
        "Actions": [
            {
                "JobName": "j1"
            }
        ],
        "Schedule": "cron(0 0/1 * * ? *)"
    }
}
```

Tom can't use the plural `GetTriggers` API to list triggers because this API operation doesn't support filtering on tags\.

To give Tom access to `GetTriggers`, the AWS Glue administrator creates a policy that splits the permissions into two sections\. One section allows Tom access to all triggers with the `GetTriggers` API operation\. The second section allows Tom access to API operations that are tagged with the value `Tom`\. With this policy, Tom is allowed both `GetTriggers` and `GetTrigger` access to trigger `t2`\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "glue:GetTriggers",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "glue:*",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "glue:resourceTag/Name": "Tom"
                }
            }
        }
    ]
}
```

## Example Access Control Using Tags with Deny<a name="tags-control-access-example-triggers-deny"></a>

Another approach to write a resource policy is to explicitly deny access to resources instead of granting access to a user\. For example, if Tom is considered a special user of a team, the administrator can deny access to Tom's resources to Sam and everyone else on the team\. As a result, Sam can access almost every resource except those tagged with the tag value `Tom`\. Here is the resource policy that AWS Glue administrator grants to Sam\. In the first section of the policy, all AWS Glue API operations are allowed for all resources\. However, in the second section, those resources tagged with `Tom` are denied access\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "glue:*",
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": [
                "glue:*"
            ],
            "Resource": [
                "*"
            ],
            "Condition": {
                "StringEquals": {
                    "glue:resourceTag/Name": "Tom"
                }
            }
        }
    ]
}
```

Using the same triggers as the previous example, Sam can access trigger `t1`, but not trigger `t2`\. The following example shows the results when Sam tries to access `t1` and `t2`\. 

```
aws glue get-trigger --name t1
{
    "Trigger": {
        "State": "CREATED",
        "Type": "SCHEDULED",
        "Name": "t1",
        "Actions": [
            {
                "JobName": "j1"
            }
        ],
        "Schedule": "cron(0 0/1 * * ? *)"
    }
}

aws glue get-trigger  --name t2

An error occurred (AccessDeniedException) when calling the GetTrigger operation: User: Sam is not authorized to perform: glue:GetTrigger on resource: arn:aws:glue:us-east-1:123456789012:trigger/t2 with an explicit deny
```

**Important**  
An explicit denial policy does not work for plural APIs\. Even with the attachment of tag value `Tom` to trigger `t2`, Sam can still call `GetTriggers` to view trigger `t2`\. Because of this, the administrator might not want to allow access to the `GetTriggers` API operations\. The following example shows the results when Sam runs the `GetTriggers` API\.

```
aws glue get-triggers
{
    "Triggers": [
        {
            "State": "CREATED",
            "Type": "SCHEDULED",
            "Name": "t1",
            "Actions": [
                {
                    "JobName": "j1"
                }
            ],
            "Schedule": "cron(0 0/1 * * ? *)"
        },
        {
            "State": "CREATED",
            "Type": "SCHEDULED",
            "Name": "t2",
            "Actions": [
                {
                    "JobName": "j1"
                }
            ],
            "Schedule": "cron(0 0/1 * * ? *)"
        }
    ]
}
```

## Example Access Control Using Tags with List and Batch API Operations<a name="tags-control-access-example-triggers-list-batch"></a>

A third approach to writing a resource policy is to allow access to resources using a `List` API operation to list out resources for a tag value\. Then, use the corresponding `Batch` API operation to allow access to details of specific resources\. With this approach, the administrator doesn't need to allow access to the plural `GetCrawlers`, `GetDevEndpoints`, `GetJobs`, or `GetTriggers` API operations\. Instead, you can allow the ability to list the resources with the following API operations:
+ `ListCrawlers`
+ `ListDevEndpoints`
+ `ListJobs`
+ `ListTriggers`

And, you can allow the ability to get details about individual resources with the following API operations:
+ `BatchGetCrawlers`
+ `BatchGetDevEndpoints`
+ `BatchGetJobs`
+ `BatchGetTriggers`

As an administrator, to use this approach, you can do the following:

1. Add tags to your crawlers, development endpoints, jobs, and triggers\.

1. Deny user access to `Get` API operations such as `GetCrawlers`, `GetDevEndponts`, `GetJobs`, and `GetTriggers`\.

1. To enable users to find out which tagged resources they have access to, allow user access to `List` API operations such as `ListCrawlers`, `ListDevEndponts`, `ListJobs`, and `ListTriggers`\.

1. Deny user access to AWS Glue tagging APIs, such as `TagResource` and `UntagResource`\.

1. Allow user access to resource details with `BatchGet` API operations such as `BatchGetCrawlers`, `BatchGetDevEndponts`, `BatchGetJobs`, and `BatchGetTriggers`\.

For example, when calling the `ListCrawlers` operation, provide a tag value to match the user name\. Then the result is a list of crawlers that match the provided tag values\. Provide the list of names to the `BatchGetCrawlers` to get details about each crawler with the given tag\.

For example, if Tom should only be able to retrieve details of triggers that are tagged with `Tom`, the administrator can add tags to triggers for `Tom`, deny access to the `GetTriggers` API operation to all users and allow access to all users to `ListTriggers` and `BatchGetTriggers`\. The following is the resource policy that the AWS Glue administrator grants to Tom\. In the first section of the policy, AWS Glue API operations are denied for `GetTriggers`\. In the second section of the policy, `ListTriggers` is allowed for all resources\. However, in the third section, those resources tagged with `Tom` are allowed access with the `BatchGetTriggers` access\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": "glue:GetTriggers",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "glue:ListTriggers"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "glue:BatchGetTriggers"
            ],
            "Resource": [
                "*"
            ],
            "Condition": {
                "StringEquals": {
                    "glue:resourceTag/Name": "Tom"
                }
            }
        }
    ]
}
```

Using the same triggers as the previous example, Tom can access trigger `t2`, but not trigger `t1`\. The following example shows the results when Tom tries to access `t1` and `t2` with `BatchGetTriggers`\. 

```
aws glue batch-get-triggers --trigger-names t2
{
    "Triggers": {
        "State": "CREATED",
        "Type": "SCHEDULED",
        "Name": "t2",
        "Actions": [
            {
                "JobName": "j2"
            }
        ],
        "Schedule": "cron(0 0/1 * * ? *)"
    }
}

aws glue batch-get-triggers  --trigger-names t1

An error occurred (AccessDeniedException) when calling the BatchGetTriggers operation: No access to any requested resource.
```

The following example shows the results when Tom tries to access both trigger `t2` and trigger `t3` \(which does not exist\) in the same `BatchGetTriggers` call\. Notice that because Tom has access to trigger `t2` and it exists, only `t2` is returned\. Although Tom is allowed to access trigger `t3`, trigger `t3` does not exist, so `t3` is returned in the response in a list of `"TriggersNotFound": []`\. 

```
aws glue batch-get-triggers --trigger-names t2 t3
{
    "Triggers": {
        "State": "CREATED",
        "Type": "SCHEDULED",
        "Name": "t2",
        "Actions": [
            {
                "JobName": "j2"
            }
        ],
        "TriggersNotFound": ["t3"],
        "Schedule": "cron(0 0/1 * * ? *)"
    }
}
```