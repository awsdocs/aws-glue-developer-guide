# Overview of Developing Blueprints<a name="developing-blueprints-overview"></a>

The first step in your development process is to identify a common use case that would benefit from a blueprint\. A typical use case involves a recurring ETL problem that you believe should be solved in a general manner\. Next, design a blueprint that implements the generalized use case, and define the blueprint input parameters that together can define a specific use case from the generalized use case\.

A blueprint consists of a project that contains a blueprint parameter configuration file and a script that defines the *layout* of the workflow to generate\. The layout defines the jobs and crawlers \(or *entities* in blueprint script terminology\) to create\.

You do not directly specify any triggers in the layout script\. Instead you write code to specify the dependencies between the jobs and crawlers that the script creates\. AWS Glue generates the triggers based on your dependency specifications\. The output of the layout script is a workflow object, which contains specifications for all workflow entities\.

You build your workflow object using the following AWS Glue blueprint libraries:
+ `awsglue.blueprint.base_resource` – A library of base resources used by the libraries\.
+ `awsglue.blueprint.workflow` – A library for defining a `Workflow` class\.
+ `awsglue.blueprint.job` – A library for defining a `Job` class\.
+ `awsglue.blueprint.crawler` – A library for defining a `Crawler` class\.

The only other libraries that are supported for layout generation are those libraries that are available for the Python shell\.

Before publishing your blueprint, you can use methods defined in the blueprint libraries to test the blueprint locally\.

When you're ready to make the blueprint available to data analysts, you package the script, the parameter configuration file, and any supporting files, such as additional scripts and libraries, into a single deployable asset\. You then upload the asset to Amazon S3 and ask an administrator to register it with AWS Glue\.

For information about more sample blueprint projects, see [Sample Blueprint Project](developing-blueprints-sample.md) and [Blueprint Samples](developing-blueprints-samples.md)\.