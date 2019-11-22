# Step 4: Create an IAM Policy for Notebook Servers<a name="create-notebook-policy"></a>

If you plan to use notebooks with development endpoints, you must specify permissions when you create the notebook server\. You provide those permissions by using AWS Identity and Access Management \(IAM\)\.

This policy grants permission for some Amazon S3 actions to manage resources in your account that are needed by AWS Glue when it assumes the role using this policy\. Some of the resources that are specified in this policy refer to default names used by AWS Glue for Amazon S3 buckets, Amazon S3 ETL scripts, and Amazon EC2 resources\. For simplicity, AWS Glue defaults writing some Amazon S3 objects into buckets in your account prefixed with `aws-glue-*`\. 

**Note**  
You can skip this step if you use the AWS managed policy **AWSGlueServiceNotebookRole**\.

In this step, you create a policy that is similar to `AWSGlueServiceNotebookRole`\. You can find the most current version of `AWSGlueServiceNotebookRole` on the IAM console\.

**To create an IAM policy for notebooks**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the left navigation pane, choose **Policies**\.

1. Choose **Create Policy**\.

1. On the **Create Policy** screen, navigate to a tab to edit JSON\. Create a policy document with the following JSON statements, and then choose **Review policy**\.

   ```
   {  
      "Version":"2012-10-17",
      "Statement":[  
         {  
            "Effect":"Allow",
            "Action":[  
               "glue:CreateDatabase",
               "glue:CreatePartition",
               "glue:CreateTable",
               "glue:DeleteDatabase",
               "glue:DeletePartition",
               "glue:DeleteTable",
               "glue:GetDatabase",
               "glue:GetDatabases",
               "glue:GetPartition",
               "glue:GetPartitions",
               "glue:GetTable",
               "glue:GetTableVersions",
               "glue:GetTables",
               "glue:UpdateDatabase",
               "glue:UpdatePartition",
               "glue:UpdateTable",
               "glue:CreateBookmark",
               "glue:GetBookmark",
               "glue:UpdateBookmark",
               "glue:GetMetric",
               "glue:PutMetric",
               "glue:CreateConnection",
               "glue:CreateJob",
               "glue:DeleteConnection",
               "glue:DeleteJob",
               "glue:GetConnection",
               "glue:GetConnections",
               "glue:GetDevEndpoint",
               "glue:GetDevEndpoints",
               "glue:GetJob",
               "glue:GetJobs",
               "glue:UpdateJob",
               "glue:BatchDeleteConnection",
               "glue:UpdateConnection",
               "glue:GetUserDefinedFunction",
               "glue:UpdateUserDefinedFunction",
               "glue:GetUserDefinedFunctions",
               "glue:DeleteUserDefinedFunction",
               "glue:CreateUserDefinedFunction",
               "glue:BatchGetPartition",
               "glue:BatchDeletePartition",
               "glue:BatchCreatePartition",
               "glue:BatchDeleteTable",
               "glue:UpdateDevEndpoint",
               "s3:GetBucketLocation",
               "s3:ListBucket",
               "s3:ListAllMyBuckets",
               "s3:GetBucketAcl"
            ],
            "Resource":[  
               "*"
            ]
         },
         {  
            "Effect":"Allow",
            "Action":[  
               "s3:GetObject"
            ],
            "Resource":[  
               "arn:aws:s3:::crawler-public*",
               "arn:aws:s3:::aws-glue*"
            ]
         },
         {  
            "Effect":"Allow",
            "Action":[  
               "s3:PutObject",
               "s3:DeleteObject"			
            ],
            "Resource":[  
               "arn:aws:s3:::aws-glue*"
            ]
         },
         {  
            "Effect":"Allow",
            "Action":[  
               "ec2:CreateTags",
               "ec2:DeleteTags"
            ],
            "Condition":{  
               "ForAllValues:StringEquals":{  
                  "aws:TagKeys":[  
                     "aws-glue-service-resource"
                  ]
               }
            },
            "Resource":[  
               "arn:aws:ec2:*:*:network-interface/*",
               "arn:aws:ec2:*:*:security-group/*",
               "arn:aws:ec2:*:*:instance/*"
            ]
         }
      ]
   }
   ```

   The following table describes the permissions granted by this policy\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/glue/latest/dg/create-notebook-policy.html)

1. On the **Review Policy** screen, enter your **Policy Name**, for example **GlueServiceNotebookPolicyDefault**\. Enter an optional description, and when you're satisfied with the policy, choose **Create policy**\.