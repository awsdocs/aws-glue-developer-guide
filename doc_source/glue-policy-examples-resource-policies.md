# AWS Glue Resource\-Based Access Control Policy Examples<a name="glue-policy-examples-resource-policies"></a>

This section contains example resource policies, including policies that grant cross\-account access\.

**Important**  
By changing an AWS Glue resource policy, you might accidentally revoke permissions for existing AWS Glue users in your account and cause unexpected disruptions\. Try these examples only in development or test accounts, and ensure that they don't break any existing workflows before you make the changes\.

**Note**  
Both IAM policies and an AWS Glue resource policy take a few seconds to propagate\. After you attach a new policy, you might notice that the old policy is still in effect until the new policy has propagated through the system\.

The following examples use the AWS Command Line Interface \(AWS CLI\) to interact with AWS Glue service APIs\. You can perform the same operations on the AWS Glue console or using one of the AWS SDKs\.

**To set up the AWS CLI**

1. Install the AWS CLI by following the instructions in [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

1. Configure the AWS CLI by following the instructions in [Configuration and Credential Files](https://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html)\. Create an admin profile using your AWS account administrator credentials\. Configure the default AWS Region to us\-west\-2 \(or a Region that you use\), and set the default output format to **JSON**\.

1. Test access to the AWS Glue API by running the following command \(replacing *Alice* with a real IAM user or role in your account\)\.

   ```
   # Run as admin of account account-id
   $ aws glue put-resource-policy --profile administrator-name --region us-west-2 --policy-in-json '{
     "Version": "2012-10-17",
     "Statement": [
       {
         "Principal": {
           "AWS": [
             "arn:aws:iam::account-id:user/Alice"
           ]
         },
         "Effect": "Allow",
         "Action": [
           "glue:*"
         ],
         "Resource": [
           "arn:aws:glue:us-west-2:account-id:*"
         ]
       }
     ]
   }'
   ```

1. Configure a user profile for each IAM user in the accounts that you use for testing your resource policy and cross\-account access\.

## Example 1\. Use a Resource Policy to Control Access in the Same Account<a name="glue-policy-resource-policies-example-same-account"></a>

In this example, an admin user in Account A creates a resource policy that grants IAM user `Alice` in Account A full access to the catalog\. Alice has no IAM policy attached\.

To do this, the administrator user runs the following AWS CLI command\.

```
# Run as admin of Account A
$ aws glue put-resource-policy --profile administrator-name --region us-west-2 --policy-in-json '{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Principal": {
        "AWS": [
          "arn:aws:iam::account-A-id:user/Alice"
        ]
      },
      "Effect": "Allow",
      "Action": [
        "glue:*"
      ],
      "Resource": [
        "arn:aws:glue:us-west-2:account-A-id:*"
      ]
    }
  ]
}'
```

Instead of entering the JSON policy document as a part of your AWS CLI command, you can save a policy document in a file and reference the file path in the AWS CLI command, prefixed by `file://`\. The following is an example of how you might do that\.

```
$ echo '{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Principal": {
        "AWS": [
          "arn:aws:iam::account-A-id:user/Alice"
        ]
      },
      "Effect": "Allow",
      "Action": [
        "glue:*"
      ],
      "Resource": [
        "arn:aws:glue:us-west-2:account-A-id:*"
      ]
    }
  ]
}' > /temp/policy.json

$ aws glue put-resource-policy --profile admin1 \
    --region us-west-2 --policy-in-json file:///temp/policy.json
```

After this resource policy has propagated, Alice can access all AWS Glue resources in Account A, as follows\.

```
# Run as user Alice
$ aws glue create-database --profile alice --region us-west-2 --database-input '{
    "Name": "new_database",
    "Description": "A new database created by Alice",
    "LocationUri": "s3://my-bucket"
}'

$ aws glue get-table --profile alice --region us-west-2 --database-name "default" --table-name "tbl1"}
```

In response to Alice's `get-table` call, the AWS Glue service returns the following\.

```
{
  "Table": {
    "Name": "tbl1",
    "PartitionKeys": [],
    "StorageDescriptor": {
        ......
    },
    ......
  }
}
```

## Example 2\. Use a Resource Policy to Grant Cross\-Account Access<a name="glue-policy-resource-policies-example-cross-account"></a>

In this example, a resource policy in Account A is used to grant to Bob in Account B read\-only access to all Data Catalog resources in Account A\. To do this, four steps are needed:

1. **Verify that Account A has migrated its Amazon Athena data catalog to AWS Glue\.**

   Cross\-account access to AWS Glue is not allowed if the resource\-owner account has not migrated its Athena data catalog to AWS Glue\. For more details on how to migrate the Athena catalog to AWS Glue, see [Upgrading to the AWS Glue Data Catalog Step\-by\-Step](https://docs.aws.amazon.com/athena/latest/ug/glue-upgrade.html) in the *Amazon Athena User Guide*\.

   ```
   # Verify that the value "ImportCompleted" is true. This value is region specific.
   $ aws glue get-catalog-import-status --profile admin1 --region us-west-2
   {
       "ImportStatus": {
           "ImportCompleted": true,
           "ImportTime": 1502512345.0,
           "ImportedBy": "StatusSetByDefault"
       }
   }
   ```

1. **An admin in Account A creates a resource policy granting Account B access\.**

   ```
   # Run as admin of Account A
   $ aws glue put-resource-policy --profile admin1 --region us-west-2 --policy-in-json '{
     "Version": "2012-10-17",
     "Statement": [
       {
         "Principal": {
           "AWS": [
             "arn:aws:iam::account-B-id:root"
           ]
         },
         "Effect": "Allow",
         "Action": [
           "glue:Get*",
           "glue:BatchGet*"
         ],
         "Resource": [
           "arn:aws:glue:us-west-2:account-A-id:*"
         ]
       }
     ]
   }'
   ```

1. **An admin in Account B grants Bob access to Account A using an IAM policy\.**

   ```
   # Run as admin of Account B
   $ aws iam put-user-policy --profile admin2 --user-name Bob --policy-name CrossAccountReadOnly --policy-document '{
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "glue:Get*",
           "glue:BatchGet*"
         ],
         "Resource": [
           "arn:aws:glue:us-west-2:account-A-id:*"
         ]
       }
     ]
   }'
   ```

1. **Verify Bob's access to a resource in Account A\.**

   ```
   # Run as user Bob. This call succeeds in listing Account A databases.
   $ aws glue get-databases --profile bob --region us-west-2 --catalog-id account-A-id
   {
     "DatabaseList": [
       {
         "Name": "db1"
       },
       {
         "Name": "db2"
       },
       ......
     ]
   }
   
   # This call succeeds in listing tables in the 'default' Account A database.
   $ aws glue get-table --profile alice --region us-west-2 --catalog-id account-A-id \
         --database-name "default" --table-name "tbl1"
   {
     "Table": {
       "Name": "tbl1",
       "PartitionKeys": [],
       "StorageDescriptor": {
         ......
       },
       ......
     }
   }
   
   # This call fails with access denied, because Bob has only been granted read access.
   $ aws glue create-database --profile bob --region us-west-2 --catalog-id account-A-id --database-input '{
       "Name": "new_database2",
       "Description": "A new database created by Bob",
       "LocationUri": "s3://my-bucket2"
   }'
   
   An error occurred (AccessDeniedException) when calling the CreateDatabase operation:
   User: arn:aws:iam::account-B-id:user/Bob is not authorized to perform:
   glue:CreateDatabase on resource: arn:aws:glue:us-west-2:account-A-id:database/new_database2
   ```

In step 2, the administrator in Account A grants permission to the root user of Account B\. The root user can then delegate the permissions it owns to all IAM principals \(users, roles, groups, and so forth\) by attaching IAM policies to them\. Because an admin user already has a full\-access IAM policy attached, an administrator automatically owns the permissions granted to the root user, and also the permission to delegate permissions to other IAM users in the account\.

Alternatively, in step 2, you could grant permission to the Amazon Resource Name \(ARN\) of user Bob directly\. This restricts the cross\-account access permission to Bob alone\. However, step 3 is still required for Bob to actually gain the cross\-account access\. For cross\-account access, *both* the resource policy in the resource account *and* an IAM policy in the user's account are required for access to work\. This is different from the same\-account access in Example 1, where either the resource policy or the IAM policy can grant access without needing the other\.