Flink is considered next generation of stream processing (Flink > Spark > Hadoop), giving us low latency and high throughput applications, robust fault tolerance (restarting from where it failed), has a set of rich libraries and easily scalable while the application is still running while maintaining only once semantics.

Comparing Hadoop and Flink/Spark, Hadoop operations write to disk whereas Flink/Spark do computations in memory. Hadoop requires hand coding each operation whereas Flink/Spark have high level operators. Spark has the RDD abstraction and Flink has the DataFlow abstraction. Flink has Mlib and Flink has FlinkML for machine learning libraries.

## Flink vs Spark
Spark is a near to real time streaming framework because it was initially designed for batch processing. Streaming computation model is based on microbatching. As long as the batch size is small, it appears to be real time. Spark doesn't have an efficiency memory manager resulting in lots of OOM. Uses DAG as its execution engine.

Flink is a true real time processing framework. Flink uses concepts of windowing and checkpointing to split the stream in buckets of finite size. Flink has it's own efficient memory manager, rarely resulting in OOM. Flink uses controlled cyclic depedency graph as its execution engine.

Yahoos Benchmark for Flink vs Storm vs Spark - Flink took 40 secs and Spark took 70 without backpressure and 120 with backpressure. Some other benchmarks were mentioned and Flink general slightly outperformed Spark in most cases, except when there was a large graph dataset of 1 TB. It's possible to configure Spark to be more efficient than Flink, but Flink has the capabilitiy out of the box with it's own efficient memory manager. This can even be shown in the number of OOM posts for Flink vs Spark.

## Ecosystem
First component, is a runtime code engine which does stream prcoessing. On top, has API's for batch and streaming data which are the foundational abstractions for user programming. The DataStream API can only be used in java/scala, whereas DataSet can also be used in python.

DataSet API can be used in relational (table API), graph (gelly API) and ml (FlinkML) libraries. Zeepelin also has a data visualization tool. 

Flink can run on a local JVM or as a distributed cluster of nodes or the cloud.

## Programming Model
The example is "do word count of words starting with N"

A case of 1 worker node performing that can be applied to N number of workers on the same or different machines.
block 1 -- read() --> node a1 -- filter() --> node a2 -- groupby --> node a3 -- sum(),count() --> output.



What if node a1 is down? who will process block 1? In Flink, the intermediary output is stored in memory, instead of on disk so the memory and data of a1 is gone. This is where the datasets abstraction comes into play because of the implemented fault tolerance in Flink. Flink will call another node and perform the operation. How does the node know which task to perform? Flink keeps track of which nodes have performed which operations. The new node will perform the same task as the original node.

How does flink keep track of all the computations performed on the data? with the help of abstractions. Each operation results in a new DataSet on each new node.

The data abstractions are immutable. Any operation on a dataset will result in a new dataset. Any operations will be performed on the entire dataset, can't do half of it. the abstractions also contain all of its dependencies



## Using Maven
mvn archetype:generate -DgroupId=wc -DartifactId=wc


## Flink Read Functions
- readtextFile(path)
- readCsvFile(path) - reads csv file and returns dataset of tuples (tupleN - number of elements in the tuple)
- readFileOfPrimitives(path, Class) - Reads each line of file in the form of Class
- readFileOfPrimitives(path, delimiter, Class)
- readHadoopFile(FileInputFormat, Key, Value, path)
- readSequenceFile(Key, Value, path)

## Filtering
Must implement the FilterFunction interface as a class or write the function inline.

## Tokenizer
A class to create Tuple2 instances so each name is transformed into a tuple like (Name, 1), the 1 is used later in the groupby.sum call