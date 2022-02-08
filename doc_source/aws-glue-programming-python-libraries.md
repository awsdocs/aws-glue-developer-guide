# Using Python Libraries with AWS Glue<a name="aws-glue-programming-python-libraries"></a>

AWS Glue lets you install additional Python modules and libraries for use with AWS Glue ETL\.

**Topics**
+ [Installing Additional Python Modules in AWS Glue 2\.0 with pip](#addl-python-modules-support)
+ [Python Modules Already Provided in AWS Glue Version 2\.0](#glue20-modules-provided)
+ [Zipping Libraries for Inclusion](#aws-glue-programming-python-libraries-zipping)
+ [Loading Python Libraries in a Development Endpoint](#aws-glue-programming-python-libraries-dev-endpoint)
+ [Using Python Libraries in a Job or JobRun](#aws-glue-programming-python-libraries-job)

## Installing Additional Python Modules in AWS Glue 2\.0 with pip<a name="addl-python-modules-support"></a>

AWS Glue uses the Python Package Installer \(pip3\) to install additional modules to be used by AWS Glue ETL\. You can use the `--additional-python-modules` option with a list of comma\-separated Python modules to add a new module or change the version of an existing module\. You can pass additional options specified by the `python-modules-installer-option` to pip3 for installing the modules\. Any incompatibly or limitations from pip3 will apply\.

For example to update or to add a new `scikit-learn` module use the following key/value: `"--additional-python-modules", "scikit-learn==0.21.3"`\.

Also, within the `--additional-python-modules` option you can specify an Amazon S3 path to a Python wheel module\. For example:

```
--additional-python-modules s3://aws-glue-native-spark/tests/j4.2/ephem-3.7.7.1-cp37-cp37m-linux_x86_64.whl,s3://aws-glue-native-spark/tests/j4.2/fbprophet-0.6-py3-none-any.whl,scikit-learn==0.21.3
```

You specify the `--additional-python-modules` option in the `DefaultArguments` or `NonOverridableArguments` job parameters, or in the **Job parameters** field of the AWS Glue console\.

## Python Modules Already Provided in AWS Glue Version 2\.0<a name="glue20-modules-provided"></a>

AWS Glue version 2\.0 supports the following python modules out of the box:
+ boto3==1\.12\.4
+ botocore==1\.15\.4
+ certifi==2019\.11\.28
+ chardet==3\.0\.4
+ cycler==0\.10\.0
+ Cython==0\.29\.15
+ docutils==0\.15\.2
+ enum34==1\.1\.9
+ fsspec==0\.6\.2
+ idna==2\.9
+ jmespath==0\.9\.4
+ joblib==0\.14\.1
+ kiwisolver==1\.1\.0
+ matplotlib==3\.1\.3
+ mpmath==1\.1\.0
+ numpy==1\.18\.1
+ pandas==1\.0\.1
+ patsy==0\.5\.1
+ pmdarima==1\.5\.3
+ ptvsd==4\.3\.2
+ pyarrow==0\.16\.0
+ pydevd==1\.9\.0
+ pyhocon==0\.3\.54
+ PyMySQL==0\.9\.3
+ pyparsing==2\.4\.6
+ python\_dateutil==2\.8\.1
+ pytz==2019\.3
+ requests==2\.23\.0
+ s3fs==0\.4\.0
+ s3transfer==0\.3\.3
+ scikit\-learn==0\.22\.1
+ scipy==1\.4\.1
+ setuptools==45\.2\.0
+ six==1\.14\.0
+ statsmodels==0\.11\.1
+ subprocess32==3\.5\.4
+ sympy==1\.5\.1
+ tbats==1\.0\.9
+ urllib3==1\.25\.8

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
sc.addPyFile("/home/glue/downloads/python/yourZipFileName.zip")
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
