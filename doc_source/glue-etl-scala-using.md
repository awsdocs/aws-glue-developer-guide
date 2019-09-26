# Using Scala to Program AWS Glue ETL Scripts<a name="glue-etl-scala-using"></a>

You can automatically generate a Scala extract, transform, and load \(ETL\) program using the AWS Glue console, and modify it as needed before assigning it to a job\. Or, you can write your own program from scratch\. For more information, see [Adding Jobs in AWS Glue](add-job.md)\. AWS Glue then compiles your Scala program on the server before running the associated job\.

To ensure that your program compiles without errors and runs as expected, it's important that you load it on a development endpoint in a REPL \(Read\-Eval\-Print Loop\) or an Apache Zeppelin Notebook and test it there before running it in a job\. Because the compile process occurs on the server, you will not have good visibility into any problems that happen there\.

## Testing a Scala ETL Program in a Zeppelin Notebook on a Development Endpoint<a name="aws-glue-programming-scala-using-notebook"></a>

To test a Scala program on an AWS Glue development endpoint, set up the development endpoint as described in [Managing Notebooks](dev-endpoint.md)\.

Next, connect it to an Apache Zeppelin Notebook that is either running locally on your machine or remotely on an Amazon EC2 notebook server\. To install a local version of a Zeppelin Notebook, follow the instructions in [Tutorial: Local Zeppelin Notebook](dev-endpoint-tutorial-local-notebook.md)\.

The only difference between running Scala code and running PySpark code on your Notebook is that you should start each paragraph on the Notebook with the the following:

```
%spark
```

This prevents the Notebook server from defaulting to the PySpark flavor of the Spark interpreter\.

## Testing a Scala ETL Program in a Scala REPL<a name="aws-glue-programming-scala-using-repl"></a>

You can test a Scala program on a development endpoint using the AWS Glue Scala REPL\. Follow the instructions in [Tutorial: Use a REPL Shell](dev-endpoint-tutorial-repl.md), except at the end of the SSH\-to\-REPL command, replace `-t gluepyspark` with `-t glue-spark-shell`\. This invokes the AWS Glue Scala REPL\.

To close the REPL when you are finished, type `sys.exit`\.