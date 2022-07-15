--Spark : data processing framework

# Spark Configurations
val conf = new SparkConf()
             .setMaster("local[2]")
             .setAppName("CountingSheep")
val sc = new SparkContext(conf)

# spark submit script in Spark’s bin directory is used to launch applications on a cluster.
./bin/spark-submit --name "My app" --master local[4] --conf spark.eventLog.enabled=false
  --conf "spark.executor.extraJavaOptions=-XX:+PrintGCDetails -XX:+PrintGCTimeStamps" myApp.jar
  
# Master slave architecture
In a Spark Application, Driver is responsible for task scheduling and Executor is responsible for executing the concrete tasks in your job.
Master = Driver - 1GB by default. To be increased on need basis - 
Slave = Executor -  512MB per executor by default

# Core components - Spark Core, Spark Sql, Spark ML, Spark Streaming, GraphX

# Driver and its associated memory 
Spark Driver is the program that runs on the master node of the machine and declares transformations and actions on data RDDs. 
In simple terms, driver in Spark creates SparkContext, connected to a given Spark Master.
The memory you need to assign to the driver depends on the job.

1. If the job is based purely on transformations and terminates on some distributed output action like rdd.saveAsTextFile, rdd.saveToCassandra, ... then the memory needs of the driver will be very low. Few 100's of MB will do. The driver is also responsible of delivering files and collecting metrics, but not be involved in data processing.

2. If the job requires the driver to participate in the computation, like e.g. some ML algo that needs to materialize results and broadcast them on the next iteration, then your job becomes dependent of the amount of data passing through the driver. Operations like .collect,.take and takeSample deliver data to the driver and hence, the driver needs enough memory to allocate such data.

e.g. If you have an rdd of 3GB in the cluster and call val myresultArray = rdd.collect, then you will need 3GB of memory in the driver to hold that data plus some extra room for the functions mentioned in the first paragraph.

# Executor memory
 Executor is responsible for executing the concrete tasks in your job.
For instance, all Map-Reduce Operations are executed in Executor(in Spark, they are called ShuffleMapTasks & ResultTasks), and also, whatever RDD you want to cache is also in executor's JVM's heap & disk.

# Total memory = Driver Memory + no of executors * Executor Memory
Spark shell required memory = (Driver Memory + 384 MB) + (Number of executors * (Executor memory + 384 MB))
Here 384 MB is maximum memory (overhead) value that may be utilized by Spark when executing jobs.

# RDD - Resilient Distributed datasets
RDD is a distributed collection of immutable data elements spread across many machines in the cluster. RDDs are a set of Java or Scala objects representing data. DataFrame – A DataFrame is a distributed collection of data organized into named columns. It is conceptually equal to a table in a relational database.

RDDs cannot take advantages of sparks advance optimizers. For example, catalyst optimizer and Tungsten execution engine. Developers optimise each RDD on the basis of its attributes. Spark DataFrame Can serialize the data into off-heap storage(in-memory) in binary format and then perform many transformations directly on this off heap memory because spark understands the schema. There is no need to use java serialization to encode the data. It provides a Tungsten physical execution backend which explicitly manages memory and dynamically generates bytecode for expression evaluation.
