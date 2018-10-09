# Specifying AWS Glue Resource ARNs<a name="glue-specifying-resource-arns"></a>

In AWS Glue, you can control access to resources by using an IAM policy\. In a policy, you use an Amazon Resource Name \(ARN\) to identify the resource that the policy applies to\. Not all resources in AWS Glue support ARNs\. 

**Topics**
+ [Amazon Resource Names \(ARNs\) for Non\-Catalog Objects](#non-catalog-resource-arns)
+ [Access Control for AWS Glue Non\-Catalog Singular API Operations](#non-catalog-singular-apis)
+ [Access\-Control for AWS Glue Non\-Catalog API Operations That Retrieve Multiple Items](#non-catalog-plural-apis)

## Amazon Resource Names \(ARNs\) for Non\-Catalog Objects<a name="non-catalog-resource-arns"></a>

Some AWS Glue resources allow resource\-level permissions to control access using an ARN\. You can use these ARNs in your IAM policies to enable fine\-grained access control\. The following table lists the resources that can contain resource ARNs\.


| **Resource Type**  |  **ARN Format**  | 
| --- | --- | 
| Development endpoint |  `arn:aws:glue:region:account-id:devEndpoint/development-endpoint-name` For example: `arn:aws:glue:us-east-1:123456789012:devEndpoint/temporarydevendpoint`  | 

## Access Control for AWS Glue Non\-Catalog Singular API Operations<a name="non-catalog-singular-apis"></a>

AWS Glue non\-catalog *singular* API operations act on a single item \(development endpoint\)\. Examples are `GetDevEndpoint`, `CreateUpdateDevEndpoint`, and `UpdateDevEndpoint`\. For these operations, a policy must put the API name in the `"action"` block and the resource ARN in the `"resource"` block\.

Suppose that you want to allow a user to call the `GetDevEndpoint` operation\. The following policy grants the minimum necessary permissions to an endpoint named `myDevEndpoint-1`:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Minimum permissions",
            "Effect": "Allow",
            "Action": "glue:GetDevEndpoint",
            "Resource": "arn:aws:glue:us-east-1:123456789012:devEndpoint/myDevEndpoint-1"
        }
    ]
}
```

The following policy allows `UpdateDevEndpoint` access to resources that match `myDevEndpoint-` with a wildcard \(\*\):

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Permission with wildcard",
            "Effect": "Allow",
            "Action": "glue:UpdateDevEndpoint",
            "Resource": "arn:aws:glue:us-east-1:123456789012:devEndpoint/myDevEndpoint-*"
        }
    ]
}
```

You can combine the two policies as in the following example\. Although you might see `EntityNotFoundException` for any development endpoint whose name begins with `A`, an access denied error is returned when you try to access other development endpoints:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Combined permissions",
            "Effect": "Allow",
            "Action": [
                "glue:UpdateDevEndpoint",
                "glue:GetDevEndpoint"
            ],
            "Resource": "arn:aws:glue:us-east-1:123456789012:devEndpoint/A*"
        }
    ]
}
```

## Access\-Control for AWS Glue Non\-Catalog API Operations That Retrieve Multiple Items<a name="non-catalog-plural-apis"></a>

Some AWS Glue API operations retrieve multiple items \(such as multiple development endpoints\); for example, `GetDevEndpoints`\. For this operation, you can specify only a wildcard \(\*\) resource, not specific ARNs\.

For example, to allow `GetDevEndpoints`, the following policy must scope the resource to the wildcard \(\*\) because `GetDevEndpoints` is a plural API operation\. The singular operations \(`GetDevEndpoint`, `CreateDevEndpoint`, and `DeleteDevendpoint`\) are also scoped to all \(\*\) resources in the example\.

```
{
            "Sid": "Plural API included",
            "Effect": "Allow",
            "Action": [
                "glue:GetDevEndpoints",
                "glue:GetDevEndpoint",
                "glue:CreateDevEndpoint",
                "glue:UpdateDevEndpoint"
            ],
            "Resource": [
                "*"
            ]
}
```