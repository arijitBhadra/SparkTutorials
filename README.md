## Index
![deep](https://user-images.githubusercontent.com/12748752/144213667-6c093ddd-04d3-455a-9c7b-57b3cb7fa5ed.png)
![light](https://user-images.githubusercontent.com/12748752/144213674-89ceed2c-eca9-43ab-a8cd-53385fe25440.png)

### What is Apache Spark?
![deep](https://user-images.githubusercontent.com/12748752/144213667-6c093ddd-04d3-455a-9c7b-57b3cb7fa5ed.png)
* Apache Spark is a cluster computing platform designed to be fast and general purpose.
* Apache Spark is a fast, in-memory data processing engine with elegant and expressive development APIs in Scala, Java, Python and R that allow data workers to efficiently execute machine learning algorithms that require fast iterative access to datasets.
* Spark on Apache Hadoop YARN enables deep integration with Hadoop and other YARN enabled workloads in the enterprise.

### Resilient Distributed Datasets (RDDs)
![light](https://user-images.githubusercontent.com/12748752/144213674-89ceed2c-eca9-43ab-a8cd-53385fe25440.png)
> #### "In Spark all work is expressed as creating new RDDs, transforming existing RDDs, or calling operations on RDDs to compute a result."
> #### RDD is -
  * _**an immutable,fault-tolerant collection of objects.**_
  * _**can partitioned and distributed across multiple physical nodes of a YARN cluster.**_
  * _**can be operated in parallel.**_
  
* There are two ways to create a RDDs:
    1. parallelizing an existing collection in your driver program , or
    2. referencing a dataset in an external storage system, such as a shared filesystem, HDFS, HBase, or any data source offering a Hadoop InputFormat.
  
  
### a. Parallelized Collections:
![light](https://user-images.githubusercontent.com/12748752/144213674-89ceed2c-eca9-43ab-a8cd-53385fe25440.png)
* Parallelized collections are created by calling SparkContext’s parallelize method on an existing collection in your driver program (a Scala Seq). 
* The elements of the collection are copied to form a distributed dataset that can be operated on in parallel. 
> #### Example:
```scala
val data = Array(1, 2, 3, 4, 5)
val distData = sc.parallelize(data)
distData.collect()
```
* Once created, the distributed dataset (`distData`) can be operated on in 
```scala
parallel(distData.reduce((a, b) => a + b) )
```
* Another important parameter for parallel collections is the number of partitions to cut the dataset into.
  - Spark will run one task for each partition of the cluster.
  - Typically you want 2-4 partitions for each CPU in your cluster.
  - Spark tries to set the number of partitions automatically based on your cluster.
  - Manually set (second parameter to parallelize (e.g. sc.parallelize(data, 10)))

### b. External Datasets:
* Spark can create distributed datasets from any storage source supported by Hadoop, including local file system, HDFS, Cassandra, HBase, Amazon S3, etc. Spark supports text files, SequenceFiles, and any other Hadoop InputFormat. 
  - Text file RDDs can be created using SparkContext’s "textFile()" method. 
  - This method takes an URI for the file (either a local path on the machine, or a hdfs://, s3n://, etc URI), reads it as a collection of lines.
  - Referencing a dataset in an external storage system, such as a shared filesystem, HDFS, HBase, or any data source offering a Hadoop InputFormat.
```scala
val rdd1 = sc.textFile("hdfs///sparkwork/input/apple.txt")        //for HDFS
sc.textFile("file:///home/hadoop/apple.txt")      // for local FS
rdd1.collect()
val lines = sc.textFile("file:///home/hadoop/applet.txt")
val lineLengths = lines.map(s => s.length)
val totalLength = lineLengths.reduce((a, b) => a + b)			
```

