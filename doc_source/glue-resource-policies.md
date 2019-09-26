# AWS Glue Resource Policies for Access Control<a name="glue-resource-policies"></a>

A *resource policy* is a policy that is attached to a resource rather than to an IAM identity\. For example, in Amazon Simple Storage Service \(Amazon S3\), a resource policy is attached to an Amazon S3 bucket\. AWS Glue supports using resource policies to control access to Data Catalog resources\. These resources include databases, tables, connections, and user\-defined functions, along with the Data Catalog APIs that interact with these resources\.

An AWS Glue resource policy can only be used to manage permissions for Data Catalog resources\. You can't attach it to any other AWS Glue resources such as jobs, triggers, development endpoints, crawlers, or classifiers\.

A resource policy is attached to a *catalog*, which is a virtual container for all the kinds of Data Catalog resources mentioned previously\. Each AWS account owns a single catalog in an AWS Region whose catalog ID is the same as the AWS account ID\. A catalog cannot be deleted or modified\. 

A resource policy is evaluated for all API calls to the catalog where the caller principal is included in the `"Principal"` block of the policy document\.

**Note**  
Currently, only *one* resource policy is allowed per catalog, and its size is limited to 10 KB\.

You use a policy document written in JSON format to create or modify a resource policy\. The policy syntax is the same as for an IAM policy \(see [IAM JSON Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html)\), with the following exceptions:
+ A `"Principal"` or `"NotPrincipal"` block is required for each policy statement\.
+ The `"Principal"` or `"NotPrincipal"` must identify valid existing AWS root users or IAM users, roles, or groups\. Wildcard patterns \(like `arn:aws:iam::account-id:user/*`\) are not allowed\.
+ The `"Resource"` block in the policy requires all resource ARNs to match the following regular expression syntax \(where the first `%s` is the *region* and the second `%s` is the *account\-id*\):

  ```
  *arn:aws:glue:%s:%s:(\*|[a-zA-Z\*]+\/?.*)
  ```

  For example, both `arn:aws:glue:us-west-2:account-id:*` and `arn:aws:glue:us-west-2:account-id:database/default` are allowed, but `*` is not allowed\.
+ Unlike identity\-based policies, an AWS Glue resource policy must only contain Amazon Resource Names \(ARNs\) of resources belonging to the catalog to which the policy is attached\. Such ARNs always start with `arn:aws:glue:`\.
+ A policy cannot cause the identity creating it to be locked out of further policy creation or modification\.
+ A resource\-policy JSON document cannot exceed 10 KB in size\.

As an example, suppose that the following policy is attached to the catalog in Account A\. It grants to the IAM identity `dev` in Account A permission to create any table in database `db1` in Account A\. It also grants the same permission to the root user in Account B\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "glue:CreateTable"
      ],
      "Principal": {"AWS": [
        "arn:aws:iam::account-A-id:user/dev",
        "arn:aws:iam::account-B-id:root"
      ]},
      "Resource": [
        "arn:aws:glue:us-east-1:account-A-id:table/db1/*",
        "arn:aws:glue:us-east-1:account-A-id:database/db1",
        "arn:aws:glue:us-east-1:account-A-id:catalog"        
      ]
    }
  ]
}
```

The following are some examples of resource policy documents that are *not* valid\.

For example, a policy is not valid if it specifies a user that does not exist in the account of the catalog to which it is attached\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "glue:CreateTable"
      ],
      "Principal": {"AWS": [
        "arn:aws:iam::account-A-id:user/(non-existent-user)"
      ]},
      "Resource": [
        "arn:aws:glue:us-east-1:account-A-id:table/db1/tbl1",
        "arn:aws:glue:us-east-1:account-A-id:database/db1",
        "arn:aws:glue:us-east-1:account-A-id:catalog"        
      ]
    }
  ]
}
```

A policy is not valid if it contains a resource ARN for a resource in a different account than the catalog to which it is attached\. In the following example, this is an incorrect policy if attached to account\-A\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "glue:CreateTable"
      ],
      "Principal": {"AWS": [
        "arn:aws:iam::account-A-id:user/dev"
      ]},
      "Resource": [
        "arn:aws:glue:us-east-1:account-B-id:table/db1/tbl1",
        "arn:aws:glue:us-east-1:account-B-id:database/db1",
        "arn:aws:glue:us-east-1:account-B-id:catalog"  
      ]
    }
  ]
}
```

A policy is not valid if it contains a resource ARN for a resource that is not an AWS Glue resource \(in this case, an Amazon S3 bucket\)\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "glue:CreateTable"
      ],
      "Principal": {"AWS": [
        "arn:aws:iam::account-A-id:user/dev"
      ]},
      "Resource": [
        "arn:aws:glue:us-east-1:account-A-id:table/db1/tbl1",
        "arn:aws:glue:us-east-1:account-A-id:database/db1",
        "arn:aws:glue:us-east-1:account-A-id:catalog",  
        "arn:aws:s3:::bucket/my-bucket"
      ]
    }
  ]
}
```

## AWS Glue Resource Policy APIs<a name="resource-policy-apis"></a>

You can use the following AWS Glue Data Catalog APIs to create, retrieve, modify, and delete an AWS Glue resource policy:
+ [PutResourcePolicy \(put\_resource\_policy\)](aws-glue-api-jobs-security.md#aws-glue-api-jobs-security-PutResourcePolicy)
+ [GetResourcePolicy \(get\_resource\_policy\)](aws-glue-api-jobs-security.md#aws-glue-api-jobs-security-GetResourcePolicy)
+ [DeleteResourcePolicy \(delete\_resource\_policy\)](aws-glue-api-jobs-security.md#aws-glue-api-jobs-security-DeleteResourcePolicy)

You can also use the AWS Glue console to view and edit a resource policy\. For more information, see [Working with Data Catalog Settings on the AWS Glue Console](console-data-catalog-settings.md)\.