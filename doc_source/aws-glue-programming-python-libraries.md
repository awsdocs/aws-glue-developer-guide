# Using Python Libraries with AWS Glue<a name="aws-glue-programming-python-libraries"></a>

You can use Python extension modules and libraries with your AWS Glue ETL scripts as long as they are written in pure Python\. C libraries such as `pandas` are not supported at the present time, nor are extensions written in other languages\.

## Zipping Libraries for Inclusion<a name="aws-glue-programming-python-libraries-zipping"></a>

Unless a library is contained in a single `.py` file, it should be packaged in a `.zip` archive\. The package directory should be at the root of the archive, and must contain an `__init__.py` file for the package\. Python will then be able to import the package in the normal way\.

If your library only consists of a single Python module in one `.py` file, you do not need to place it in a `.zip` file\.

## Loading Python Libraries in a Development Endpoint<a name="aws-glue-programming-python-libraries-dev-endpoint"></a>

If you are using different library sets for different ETL scripts, you can either set up a separate development endpoint for each set, or you can overwrite the library `.zip` file\(s\) that your development endpoint loads every time you switch scripts\.

You can use the console to specify one or more library \.zip files for a development endpoint when you create it\. After assigning a name and an IAM role, choose **Script Libraries and job parameters \(optional\)** and enter the full Amazon S3 path to your library `.zip` file in the **Python library path** box\. For example:

```
s3://bucket/prefix/site-packages.zip
```

If you want, you can specify multiple full paths to files, separating them with commas but no spaces, like this:

```
s3://bucket/prefix/lib_A.zip,s3://bucket_B/prefix/lib_X.zip
```

If you update these `.zip` files later, you can use the console to re\-import them into your development endpoint\. Navigate to the developer endpoint in question, check the box beside it, and choose **Update ETL libraries** from the **Action** menu\.

In a similar way, you can specify library files using the AWS Glue APIs\. When you create a development endpoint by calling [CreateDevEndpoint Action \(Python: create\_dev\_endpoint\)](aws-glue-api-dev-endpoint.md#aws-glue-api-dev-endpoint-CreateDevEndpoint), you can specify one or more full paths to libraries in the `ExtraPythonLibsS3Path` parameter, in a call that looks this:

```
dep = glue.create_dev_endpoint(
             EndpointName="testDevEndpoint",
             RoleArn="arn:aws:iam::123456789012",
             SecurityGroupIds="sg-7f5ad1ff",
             SubnetId="subnet-c12fdba4",
             PublicKey="ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCtp04H/y...",
             NumberOfNodes=3,
             ExtraPythonLibsS3Path="s3://bucket/prefix/lib_A.zip,s3://bucket_B/prefix/lib_X.zip")
```

When you update a development endpoint, you can also update the libraries it loads using a [DevEndpointCustomLibraries](aws-glue-api-dev-endpoint.md#aws-glue-api-dev-endpoint-DevEndpointCustomLibraries) object and setting the `UpdateEtlLibraries ` parameter to `True` when calling [UpdateDevEndpoint \(update\_dev\_endpoint\)](aws-glue-api-dev-endpoint.md#aws-glue-api-dev-endpoint-UpdateDevEndpoint)\.

If you are using a Zeppelin Notebook with your development endpoint, you will need to call the following PySpark function before importing a package or packages from your `.zip` file:

```
sc.addPyFile(“/home/glue/downloads/python/yourZipFileName.zip”)
```

## Using Python Libraries in a Job or JobRun<a name="aws-glue-programming-python-libraries-job"></a>

When you are creating a new Job on the console, you can specify one or more library \.zip files by choosing **Script Libraries and job parameters \(optional\)** and entering the full Amazon S3 library path\(s\) in the same way you would when creating a development endpoint:

```
s3://bucket/prefix/lib_A.zip,s3://bucket_B/prefix/lib_X.zip
```

If you are calling [CreateJob \(create\_job\)](aws-glue-api-jobs-job.md#aws-glue-api-jobs-job-CreateJob), you can specify one or more full paths to default libraries using the `--extra-py-files` default parameter, like this:

```
job = glue.create_job(Name='sampleJob',
                      Role='Glue_DefaultRole',
                      Command={'Name': 'glueetl',
                               'ScriptLocation': 's3://my_script_bucket/scripts/my_etl_script.py'},
                      DefaultArguments={'--extra-py-files': 's3://bucket/prefix/lib_A.zip,s3://bucket_B/prefix/lib_X.zip'})
```

Then when you are starting a JobRun, you can override the default library setting with a different one:

```
runId = glue.start_job_run(JobName='sampleJob',
                           Arguments={'--extra-py-files': 's3://bucket/prefix/lib_B.zip'})
```