# Developing Scripts Using Development Endpoints<a name="dev-endpoint"></a>

AWS Glue can create an environment—known as a *development endpoint*—that you can use to iteratively develop and test your extract, transform, and load \(ETL\) scripts\. You can create, edit, and delete development endpoints using the AWS Glue console or API\.

When you create a development endpoint, you provide configuration values to provision the development environment\. These values tell AWS Glue how to set up the network so that you can access the endpoint securely and the endpoint can access your data stores\.

You can then create a notebook that connects to the endpoint, and use your notebook to author and test your ETL script\. When you're satisfied with the results of your development process, you can create an ETL job that runs your script\. With this process, you can add functions and debug your scripts in an interactive manner\.

Follow the tutorials in this section to learn how to use your development endpoint with notebooks\.

**Topics**
+ [Development Endpoint Workflow](dev-endpoint-workflow.md)
+ [Adding a Development Endpoint](add-dev-endpoint.md)
+ [Viewing Development Endpoint Properties](console-development-endpoint.md)
+ [Accessing Your Development Endpoint](dev-endpoint-elastic-ip.md)
+ [Creating a Notebook Server Hosted on Amazon EC2](console-ec2-notebook-create.md)
+ [Tutorial Setup: Prerequisites for the Development Endpoint Tutorials](dev-endpoint-tutorial-prerequisites.md)
+ [Tutorial: Set Up a Local Apache Zeppelin Notebook to Test and Debug ETL Scripts](dev-endpoint-tutorial-local-notebook.md)
+ [Tutorial: Set Up an Apache Zeppelin Notebook Server on Amazon EC2](dev-endpoint-tutorial-EC2-notebook.md)
+ [Tutorial: Use a REPL Shell with Your Development Endpoint](dev-endpoint-tutorial-repl.md)
+ [Tutorial: Set Up PyCharm Professional with a Development Endpoint](dev-endpoint-tutorial-pycharm.md)