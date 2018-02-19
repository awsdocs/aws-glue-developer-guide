# Using Scala to Program AWS Glue ETL Scripts<a name="glue-etl-scala-using"></a>

You can autogenerate a Scala ETL program using the AWS Glue console and modify it as needed before assigning it to a job, or you can write your own program from scratch \(see [Adding Jobs in AWS Glue](add-job.md) for more information\)\. AWS Glue then compiles your Scala program on the server before running the associated job\.

To make sure that your program compiles without errors and runs as expected, it is very important to load it on a development endpoint in a REPL or an Apache Zeppelin Notebook and test it there before running it in a job\. Because the compile process occurs on the server, you will not have good visibility into any problems that happen there\.

## Testing a Scala ETL Program in a Zeppelin Notebook on a DevEndpoint<a name="aws-glue-programming-scala-using-notebook"></a>

Set up an AWS Glue development endpoint as outlined in [Using Development Endpoints](dev-endpoint.md)\.

Next, connect it to an Apache Zeppelin Notebook running locally on your machine or remotely on an EC2 notebook server\. To install a local version of Zeppelin Notebook, follow the instructions in [Tutorial: Local Zeppelin Notebook](dev-endpoint-tutorial-local-notebook.md)\.

The only difference between running Scala code and running PySpark code on your Notebook is that you should start each paragraph on the Notebook with:

```
%spark
```

This prevents the Notebook server from defaulting to the PySpark flavor of the Spark interpreter\.

## Testing a Scala ETL Program in a Scala REPL<a name="aws-glue-programming-scala-using-repl"></a>

To test a Scala program on a development endpoint using the AWS Glue Scala REPL, follow the instructions in [Tutorial: Use a REPL Shell](dev-endpoint-tutorial-repl.md), with the difference that in the SSH\-to\-REPL command, replace the `-t gluepyspark` at the end of the command with `-t glue-spark-shell`, to invoke the AWS Glue Scala REPL\.

To close the REPL when you are finished, type `sys.exit`\.