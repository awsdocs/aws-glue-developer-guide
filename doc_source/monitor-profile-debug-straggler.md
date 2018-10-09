# Debugging Demanding Stages and Straggler Tasks<a name="monitor-profile-debug-straggler"></a>

You can use AWS Glue job profiling to identify demanding stages and straggler tasks in your extract, transform, and load \(ETL\) jobs\. A straggler task takes much longer than the rest of the tasks in a stage of an AWS Glue job\. As a result, the stage takes longer to complete, which also delays the total execution time of the job\.

## Coalescing Small Input Files into Larger Output Files<a name="monitor-profile-debug-straggler-scenario-1"></a>

A straggler task can occur when there is a non\-uniform distribution of work across the different tasks, or a data skew results in one task processing more data\.

You can profile the following code—a common pattern in Apache Spark—to coalesce a large number of small files into larger output files\. For this example, the input dataset is 32 GB of JSON Gzip compressed files\. The output dataset has roughly 190 GB of uncompressed JSON files\. 

The profiled code is as follows:

```
datasource0 = spark.read.format("json").load("s3://input_path")
df = datasource0.coalesce(1)
df.write.format("json").save(output_path)
```

### Visualize the Profiled Metrics on the AWS Glue Console<a name="monitor-debug-straggler-visualize"></a>

You can profile your job to examine four different sets of metrics:
+ ETL data movement
+ Data shuffle across executors
+ Job execution
+ Memory profile

**ETL data movement**: In the **ETL Data Movement** profile, the bytes are [read](monitoring-awsglue-with-cloudwatch-metrics.md#glue.ALL.s3.filesystem.read_bytes) fairly quickly by all the executors in the first stage that completes within the first six minutes\. However, the total job execution time is around one hour, mostly consisting of the data [writes](monitoring-awsglue-with-cloudwatch-metrics.md#glue.ALL.s3.filesystem.write_bytes)\.

![\[Graph showing the ETL Data Movement profile.\]](http://docs.aws.amazon.com/glue/latest/dg/images/monitor-debug-straggler-1.png)

**Data shuffle across executors:** The number of bytes [read](monitoring-awsglue-with-cloudwatch-metrics.md#glue.driver.aggregate.shuffleLocalBytesRead) and [written](monitoring-awsglue-with-cloudwatch-metrics.md#glue.driver.aggregate.shuffleBytesWritten) during shuffling also shows a spike before Stage 2 ends, as indicated by the **Job Execution** and **Data Shuffle** metrics\. After the data shuffles from all executors, the reads and writes proceed from executor number 3 only\.

![\[The metrics for data shuffle across the executors.\]](http://docs.aws.amazon.com/glue/latest/dg/images/monitor-debug-straggler-2.png)

**Job execution:** As shown in the graph below, all other executors are idle and are eventually relinquished by the time 10:09\. At that point, the total number of executors decreases to only one\. This clearly shows that executor number 3 consists of the straggler task that is taking the longest execution time and is contributing to most of the job execution time\.

![\[The execution metrics for the active executors.\]](http://docs.aws.amazon.com/glue/latest/dg/images/monitor-debug-straggler-3.png)

**Memory profile:** After the first two stages, only [executor number 3](monitoring-awsglue-with-cloudwatch-metrics.md#glue.executorId.jvm.heap.used) is actively consuming memory to process the data\. The remaining executors are simply idle or have been relinquished shortly after the completion of the first two stages\. 

![\[The metrics for the memory profile after the first two stages.\]](http://docs.aws.amazon.com/glue/latest/dg/images/monitor-debug-straggler-4.png)

### Fix Straggling Executors Using Grouping<a name="monitor-debug-straggler-fix"></a>

You can avoid straggling executors by using the *grouping* feature in AWS Glue\. Use grouping to distribute the data uniformly across all the executors and coalesce files into larger files using all the available executors on the cluster\. For more information, see [Reading Input Files in Larger Groups](grouping-input-files.md)\.

To check the ETL data movements in the AWS Glue job, profile the following code with grouping enabled:

```
df = glueContext.create_dynamic_frame_from_options("s3", {'paths': ["s3://input_path"], "recurse":True, 'groupFiles': 'inPartition'}, format="json")
datasink = glueContext.write_dynamic_frame.from_options(frame = df, connection_type = "s3", connection_options = {"path": output_path}, format = "json", transformation_ctx = "datasink4")
```

**ETL data movement:** The data writes are now streamed in parallel with the data reads throughout the job execution time\. As a result, the job finishes within eight minutes—much faster than previously\.

![\[The ETL data movements showing the issue is fixed.\]](http://docs.aws.amazon.com/glue/latest/dg/images/monitor-debug-straggler-5.png)

**Data shuffle across executors:** As the input files are coalesced during the reads using the grouping feature, there is no costly data shuffle after the data reads\.

![\[The data shuffle metrics showing the issue is fixed.\]](http://docs.aws.amazon.com/glue/latest/dg/images/monitor-debug-straggler-6.png)

**Job execution:** The job execution metrics show that the total number of active executors running and processing data remains fairly constant\. There is no single straggler in the job\. All executors are active and are not relinquished until the completion of the job\. Because there is no intermediate shuffle of data across the executors, there is only a single stage in the job\.

![\[The metrics for the Job Execution widget showing that there are no stragglers in the job.\]](http://docs.aws.amazon.com/glue/latest/dg/images/monitor-debug-straggler-7.png)

**Memory profile:** The metrics show the [active memory consumption](monitoring-awsglue-with-cloudwatch-metrics.md#glue.executorId.jvm.heap.used) across all executors—reconfirming that there is activity across all executors\. As data is streamed in and written out in parallel, the total memory footprint of all executors is roughly uniform and well below the safe threshold for all executors\.

![\[The memory profile metrics showing active memory consumption across all executors.\]](http://docs.aws.amazon.com/glue/latest/dg/images/monitor-debug-straggler-8.png)