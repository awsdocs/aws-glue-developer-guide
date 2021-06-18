# AWS Glue Identity\-Based \(IAM\) Access Control Policy Examples<a name="glue-policy-examples-iam"></a>

This section contains example AWS Identity and Access Management \(IAM\) policies that grant permissions for various AWS Glue actions and resources\. You can copy these examples and edit them on the IAM console\. Then you can attach them to IAM identities such as users, roles, and groups\.

**Note**  
These examples all use the `us-west-2` Region\. You can replace this with whatever AWS Region you are using\.

## Example 1: Grant Read\-Only Permission to a Table<a name="example-glue-iam-access-policy-grants-read-only"></a>

The following policy grants read\-only permission to a `books` table in database `db1`\. For more information about resource Amazon Resource Names \(ARNs\), see [Data Catalog ARNs](glue-specifying-resource-arns.md#data-catalog-resource-arns)\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "GetTablesActionOnBooks",
      "Effect": "Allow",
      "Action": [
        "glue:GetTables",
        "glue:GetTable"        
      ],
      "Resource": [
        "arn:aws:glue:us-west-2:123456789012:catalog",      
        "arn:aws:glue:us-west-2:123456789012:database/db1",
        "arn:aws:glue:us-west-2:123456789012:table/db1/books"
      ]
    }
  ]
}
```

This policy grants read\-only permission to a table named `books` in the database named `db1`\. Notice that to grant `Get` permission to a table that permission to the catalog and database resources is also required\.  

The following policy grants the minimum necessary permissions to create table `tb1` in database `db1`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "glue:CreateTable"
      ],
      "Resource": [
        "arn:aws:glue:us-west-2:123456789012:table/db1/tbl1",
        "arn:aws:glue:us-west-2:123456789012:database/db1",
        "arn:aws:glue:us-west-2:123456789012:catalog"        
      ]
    }
  ]
}
```

## Example 2: Filter Tables by GetTables Permission<a name="example-glue-iam-access-policy-filter-tables"></a>

Assume that there are three tables—`customers`, `stores`, and `store_sales`—in database `db1`\. The following policy grants `GetTables` permission to `stores` and `store_sales`, but not to `customers`\. When you call `GetTables` with this policy, the result contains only the two authorized tables \(the `customers` table is not returned\)\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "GetTablesExample",
            "Effect": "Allow",
            "Action": [
                "glue:GetTables"
            ],
            "Resource": [
                "arn:aws:glue:us-west-2:123456789012:catalog",            
                "arn:aws:glue:us-west-2:123456789012:database/db1",
                "arn:aws:glue:us-west-2:123456789012:table/db1/store_sales",
                "arn:aws:glue:us-west-2:123456789012:table/db1/stores"
            ]
        }
    ]
}
```

You can simplify the preceding policy by using `store*` to match any table names that start with `store`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "GetTablesExample2",
            "Effect": "Allow",
            "Action": [
                "glue:GetTables"
            ],
            "Resource": [
                "arn:aws:glue:us-west-2:123456789012:catalog",            
                "arn:aws:glue:us-west-2:123456789012:database/db1",
                "arn:aws:glue:us-west-2:123456789012:table/db1/store*"
            ]
        }
    ]
}
```

Similarly, using `/db1/*` to match all tables in `db1`, the following policy grants `GetTables` access to all the tables in `db1`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "GetTablesReturnAll",
            "Effect": "Allow",
            "Action": [
                "glue:GetTables"
            ],
            "Resource": [
                "arn:aws:glue:us-west-2:123456789012:catalog",            
                "arn:aws:glue:us-west-2:123456789012:database/db1",
                "arn:aws:glue:us-west-2:123456789012:table/db1/*"
            ]
        }
    ]
}
```

If no table ARN is provided, a call to `GetTables` succeeds, but it returns an empty list\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "GetTablesEmptyResults",
            "Effect": "Allow",
            "Action": [
                "glue:GetTables"
            ],
            "Resource": [
                "arn:aws:glue:us-west-2:123456789012:catalog",            
                "arn:aws:glue:us-west-2:123456789012:database/db1"
            ]
        }
    ]
}
```

If the database ARN is missing in the policy, a call to `GetTables` fails with an `AccessDeniedException`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "GetTablesAccessDeny",
            "Effect": "Allow",
            "Action": [
                "glue:GetTables"
            ],
            "Resource": [
                "arn:aws:glue:us-west-2:123456789012:catalog",            
                "arn:aws:glue:us-west-2:123456789012:table/db1/*"
            ]
        }
    ]
}
```

## Example 3: Grant Full Access to a Table and All Partitions<a name="example-glue-iam-access-policy-full-access"></a>

The following policy grants all permissions on a table named `books` in database `db1`\. This includes read and write permissions on the table itself, on archived versions of it, and on all its partitions\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "FullAccessOnTable",
            "Effect": "Allow",
            "Action": [
                "glue:CreateTable",
                "glue:GetTable",
                "glue:GetTables",                
                "glue:UpdateTable",
                "glue:DeleteTable",
                "glue:BatchDeleteTable",
                "glue:GetTableVersion",
                "glue:GetTableVersions",
                "glue:DeleteTableVersion",
                "glue:BatchDeleteTableVersion",
                "glue:CreatePartition",
                "glue:BatchCreatePartition",
                "glue:GetPartition",
                "glue:GetPartitions",
                "glue:BatchGetPartition",
                "glue:UpdatePartition",
                "glue:DeletePartition",
                "glue:BatchDeletePartition"
            ],
            "Resource": [
                        "arn:aws:glue:us-west-2:123456789012:catalog",
                        "arn:aws:glue:us-west-2:123456789012:database/db1",
                        "arn:aws:glue:us-west-2:123456789012:table/db1/books" 
                        ]
        }
    ]
}
```

The preceding policy can be simplified in practice\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "FullAccessOnTable",
            "Effect": "Allow",
            "Action": [
                "glue:*Table*",
                "glue:*Partition*"
            ],
            "Resource": [
                "arn:aws:glue:us-west-2:123456789012:catalog",            
                "arn:aws:glue:us-west-2:123456789012:database/db1",
                "arn:aws:glue:us-west-2:123456789012:table/db1/books"
            ]
        }
    ]
}
```

Notice that the minimum granularity of fine\-grained access control is at the table level\. This means that you can't grant a user access to some partitions in a table but not others, or to some table columns but not to others\. A user either has access to all of a table, or to none of it\.

## Example 4: Control Access by Name Prefix and Explicit Denial<a name="example-glue-iam-access-policy-name-prefix"></a>

In this example, suppose that the databases and tables in your AWS Glue Data Catalog are organized using name prefixes\. The databases in the development stage have the name prefix `dev-`, and those in production have the name prefix `prod-`\. You can use the following policy to grant developers full access to all databases, tables, UDFs, and so on, that have the `dev-` prefix\. But you grant read\-only access to everything with the `prod-` prefix\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DevAndProdFullAccess",
            "Effect": "Allow",
            "Action": [
                "glue:*Database*",
                "glue:*Table*",
                "glue:*Partition*",
                "glue:*UserDefinedFunction*",
                "glue:*Connection*"
            ],
            "Resource": [
                "arn:aws:glue:us-west-2:123456789012:catalog",
                "arn:aws:glue:us-west-2:123456789012:database/dev-*",
                "arn:aws:glue:us-west-2:123456789012:database/prod-*",
                "arn:aws:glue:us-west-2:123456789012:table/dev-*/*",
                "arn:aws:glue:us-west-2:123456789012:table/*/dev-*",
                "arn:aws:glue:us-west-2:123456789012:table/prod-*/*",
                "arn:aws:glue:us-west-2:123456789012:table/*/prod-*",
                "arn:aws:glue:us-west-2:123456789012:userDefinedFunction/dev-*/*",
                "arn:aws:glue:us-west-2:123456789012:userDefinedFunction/*/dev-*",
                "arn:aws:glue:us-west-2:123456789012:userDefinedFunction/prod-*/*",
                "arn:aws:glue:us-west-2:123456789012:userDefinedFunction/*/prod-*",
                "arn:aws:glue:us-west-2:123456789012:connection/dev-*",
                "arn:aws:glue:us-west-2:123456789012:connection/prod-*"
            ]
        },
        {
            "Sid": "ProdWriteDeny",
            "Effect": "Deny",
            "Action": [
                "glue:*Create*",
                "glue:*Update*",
                "glue:*Delete*"
            ],
            "Resource": [
                "arn:aws:glue:us-west-2:123456789012:database/prod-*",
                "arn:aws:glue:us-west-2:123456789012:table/prod-*/*",
                "arn:aws:glue:us-west-2:123456789012:table/*/prod-*",
                "arn:aws:glue:us-west-2:123456789012:userDefinedFunction/prod-*/*",
                "arn:aws:glue:us-west-2:123456789012:userDefinedFunction/*/prod-*",
                "arn:aws:glue:us-west-2:123456789012:connection/prod-*"
            ]
        }
    ]
}
```

The second statement in the preceding policy uses explicit `deny`\. You can use explicit `deny` to overwrite any `allow` permissions that are granted to the principal\. This lets you lock down access to critical resources and prevent another policy from accidentally granting access to them\.

In the preceding example, even though the first statement grants full access to `prod-` resources, the second statement explicitly revokes write access to them, leaving only read access to `prod-` resources\.