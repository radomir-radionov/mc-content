# Hadoop — Interview

Answers are filled.

[← Back to hub](./0_system_design.md)

### Related docs

- [Hadoop — Interview](./0_sd_hadoop_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Hadoop](https://devinterview.io/questions/machine-learning-and-data-science/hadoop-interview-questions/).

### Hadoop Fundamentals

<details>
<summary>What is Hadoop and what are its core components?</summary>

### What is Hadoop and what are its core components?

**Definition:**

Hadoop is an open-source stack for keeping very large files on a cluster of ordinary servers and running batch jobs beside those files.

**How it works:**

- HDFS splits each file into large blocks, stores copies on DataNodes, and keeps the directory tree plus the block map on the NameNode. The file bytes never pass through the NameNode.
- YARN hands out containers of CPU and memory. A ResourceManager tracks the cluster, a NodeManager runs on each machine, and each application has its own ApplicationMaster.
- MapReduce is the original batch model: mappers emit key-value pairs, a shuffle groups equal keys, and reducers aggregate one key at a time.
- Hive adds a SQL-like warehouse over those files, Pig a data-flow script language, HBase random row access, ZooKeeper cluster coordination, and Oozie multi-step workflows. Mahout was the older library of batch learning jobs.
</details>

<details>
<summary>Explain the concept of a Hadoop Distributed File System (HDFS) and its architecture.</summary>

### Explain the concept of a Hadoop Distributed File System (HDFS) and its architecture.

**Definition:**

HDFS is a distributed file system for commodity disks. It favors long sequential reads of huge files over low-latency updates of small ones.

**How it works:**

- A file is a sequence of blocks. The default block is 128 MB and the usual replication factor is three; both are set per file. Only the last block may be short.
- The NameNode owns the namespace: create, rename, delete, permissions, and which DataNodes hold each block. DataNodes store the blocks and serve client reads and writes.
- A write pipelines a block through DataNodes so replicas are filled as the bytes arrive. A read asks the NameNode for locations, then streams from the closest replica.
- Design assumptions are write-once (append and truncate aside), one writer at a time, and moving the job to the data instead of shipping the file to the job.
- Small files waste NameNode memory because every block is metadata. HDFS is a poor fit for interactive random updates that need a strong per-row transaction.

**Examples:**

A client opens `/logs/2026-10-04/events.txt`. The NameNode returns the DataNodes that hold block `blk_42`. The client then streams that block from the nearest DataNode and never pulls the payload through the NameNode.
</details>

<details>
<summary>How does MapReduce programming model work in Hadoop?</summary>

### How does MapReduce programming model work in Hadoop?

**Definition:**

MapReduce turns a large batch into two functions: map, which labels records, and reduce, which aggregates every record that shares a label.

**How it works:**

- The input is split into pieces, typically aligned with HDFS blocks. Each mapper reads one split and emits intermediate key-value pairs.
- A partitioner decides which reducer owns a key. The shuffle sorts and groups pairs so one reducer sees every value for its keys.
- An optional combiner pre-aggregates pairs on the mapper host, which cuts shuffle traffic when the combine is safe (sums, counts).
- The framework prefers to schedule a task on a node that already holds the block. A failed or slow task is retried on another node.

**Examples:**

```text
map("red fish blue fish") -> (red, 1), (fish, 1), (blue, 1), (fish, 1)
shuffle groups fish -> [1, 1]
reduce(fish, [1, 1]) -> (fish, 2)
```
</details>

<details>
<summary>What is YARN, and how does it improve Hadoop's resource management?</summary>

### What is YARN, and how does it improve Hadoop's resource management?

**Definition:**

YARN separates cluster scheduling from the MapReduce programming model, so several engines can share one pool of CPU and memory.

**How it works:**

- In the older JobTracker design, one process both assigned slots and drove the MapReduce job. A single kind of slot map made Spark or Tez awkward guests.
- The ResourceManager is the cluster scheduler. Each NodeManager advertises free CPU and memory and launches containers. Each application elects an ApplicationMaster that requests containers and tracks its own tasks.
- A container is a slice of CPU and memory, not a fixed map slot or reduce slot. A job asks for what a task needs.
- Capacity or fair queues divide the cluster among teams. MapReduce, Spark, and Tez can run side by side because they are applications, not the scheduler.
</details>

<details>
<summary>Explain the role of the Namenode and Datanode in HDFS.</summary>

### Explain the role of the Namenode and Datanode in HDFS.

**Definition:**

The NameNode is the metadata master. DataNodes are the workers that hold block bytes and talk to clients.

**How it works:**

- The NameNode stores the directory tree, permissions, replication factor, and the list of DataNodes for each block. User data does not flow through it.
- A DataNode stores blocks on local disks, serves reads and writes, and creates, deletes, or copies blocks when the NameNode tells it to.
- Each DataNode sends a heartbeat and a periodic block report. A missing heartbeat means those replicas are gone, and the NameNode schedules new copies until the factor is restored.
- Namespace edits go to an edit log and periodic checkpoints (fsimage). A standby NameNode, fed by a quorum of journal nodes, can take over; the older secondary NameNode only built checkpoints and was not a hot spare.
</details>

<details>
<summary>What is a Rack Awareness algorithm in HDFS, and why is it important?</summary>

### What is a Rack Awareness algorithm in HDFS, and why is it important?

**Definition:**

Rack awareness is the NameNode's map from DataNode to rack, used so replicas survive a switch failure without paying a cross-rack copy on every write.

**How it works:**

- A topology script or a static mapping tells the NameNode which rack each DataNode sits in. Bandwidth inside a rack is treated as cheaper than bandwidth through the core switch.
- With three replicas the usual policy puts the first on the writer (or a node in the writer's rack), the second on a node in a different rack, and the third on another node in that same remote rack.
- A whole rack can die and one replica remains. Putting all three replicas on three different racks would also survive a rack loss, but every write would cross the core twice.
- A reader is steered to the closest replica: same node, then same rack, then the local data center.
</details>

<details>
<summary>What are some of the characteristics that differentiate Hadoop from traditional RDBMS?</summary>

### What are some of the characteristics that differentiate Hadoop from traditional RDBMS?

**Definition:**

Hadoop is a file-and-batch platform. A relational database is a transactional store with a schema enforced on every write.

**How it works:**

- Hadoop applies structure when a job reads the files (schema on read). An RDBMS rejects a row that does not match the table (schema on write).
- HDFS scales by adding cheap machines and is comfortable with huge scans. An RDBMS is built for indexed lookups and short updates on structured tables.
- MapReduce and SQL-on-Hadoop jobs scan and aggregate. An RDBMS updates single rows under locks and expects many concurrent small transactions.
- HDFS keeps several block copies and allows one writer, append, and truncate. An RDBMS aims for committed transactions that other sessions see consistently.
- Logs, clicks, and dumps land in Hadoop first. Orders, balances, and inventory that must not tear belong in the database.
</details>

<details>
<summary>How can you secure a Hadoop cluster? Name some of the security mechanisms available.</summary>

### How can you secure a Hadoop cluster? Name some of the security mechanisms available.

**Definition:**

A production cluster authenticates users, authorizes each service, and encrypts data on disk and on the wire.

**How it works:**

- Kerberos is the usual authentication. User accounts often come from LDAP or Active Directory. After login, services issue delegation tokens so tasks do not repeat the Kerberos exchange.
- HDFS applies owner, group, and other permissions, plus ACLs on files and directories. Apache Ranger (and the older Sentry) add central policies for Hive, HBase, and similar services.
- Encryption zones in HDFS store files encrypted at rest. A key management service unwraps the zone key for authorized clients. RPC and data transfer can use SASL and TLS.
- Proxy-user settings let a service such as Hive impersonate the end user so the file-system check sees that user, not the service account. Shell and host access still need ordinary OS hardening.
</details>

### Hadoop Ecosystem and Tools

<details>
<summary>Describe the role of HBase in Hadoop ecosystem.</summary>

### Describe the role of HBase in Hadoop ecosystem.

**Definition:**

HBase is a column-family store on HDFS for large tables that need random reads and writes, which batch scans in Hive serve too slowly.

**How it works:**

- Rows are ordered by a row key and split into regions. RegionServers serve those ranges. An HMaster assigns regions, and ZooKeeper tracks the active master and region locations.
- A column family groups columns stored together. Versions of a cell are kept by timestamp. One row is strongly consistent; the system is not a multi-row transaction database.
- On the CAP axis, HBase prefers consistency and partition behavior for a single row and gives up availability of that region while a server is down.
- Typical tables are time series, click or device events, feature lookups, and metadata that must be fetched by key rather than scanned.
</details>

<details>
<summary>What is Apache Hive and what types of problems does it solve?</summary>

### What is Apache Hive and what types of problems does it solve?

**Definition:**

Hive is a warehouse layer: analysts write HiveQL, and Hive plans that query as jobs over files that already live in HDFS or object storage.

**How it works:**

- The metastore records databases, table schemas, partitions, and the file locations. The bytes stay in the files; the metastore does not hold the table body.
- A driver parses HiveQL, an optimizer rewrites the plan, and an execution engine (MapReduce, Tez, or Spark) runs it. HiveServer2 accepts remote JDBC and ODBC sessions.
- It fits scans, aggregations, joins, and ETL on large batches. It does not replace a database for single-row transactions.
- Partitioned external tables let a query skip whole directories, which is how daily log dumps stay cheap to query.
</details>

<details>
<summary>How does Apache Pig fit into the Hadoop ecosystem?</summary>

### How does Apache Pig fit into the Hadoop ecosystem?

**Definition:**

Pig is a script language, Pig Latin, for multi-step file pipelines when the author does not want to write mappers and reducers by hand.

**How it works:**

- A script loads files, filters, joins, groups, and stores a result. The steps describe a data flow, and the schema can stay loose until a field is used.
- The script is compiled into a logical plan. Work starts when a store or dump is requested, so unused branches are not executed.
- The plan runs as MapReduce or a newer engine. User-defined functions in Java or Python cover steps the language does not express.
- Hive is the better fit for SQL analysts. Pig is the better fit for a procedural pipeline with custom functions.

**Examples:**

```text
clicks = LOAD '/logs/clicks' AS (user:chararray, path:chararray);
home = FILTER clicks BY path == '/';
per_user = GROUP home BY user;
STORE per_user INTO '/out/home-users';
```

Nothing is computed until `STORE`.
</details>

<details>
<summary>Explain how Apache Flume helps with log and event data collection for Hadoop.</summary>

### Explain how Apache Flume helps with log and event data collection for Hadoop.

**Definition:**

Flume moves event streams, usually logs, from many producers into HDFS or another sink without a custom collector on every host.

**How it works:**

- An agent is three pieces: a source that receives events, a channel that buffers them, and a sink that writes them out.
- A durable file channel keeps accepted events until the sink commits, so a process restart does not drop that batch. A memory channel is faster and can lose the buffer.
- Agents form tiers: many edge agents forward to fewer aggregators, which roll files in HDFS. Interceptors can stamp, filter, or rewrite an event on the way.
- Flume is aimed at continuous log streams. Sqoop is the tool for bulk copies of relational tables.

**Examples:**

```text
web servers -> Flume source (avro) -> file channel -> HDFS sink
rolls /logs/app/2026-10-04/events.0001
```
</details>

<details>
<summary>What is Apache Sqoop and how does it interact with Hadoop?</summary>

### What is Apache Sqoop and how does it interact with Hadoop?

**Definition:**

Sqoop bulk-copies tables between a relational database and Hadoop, using parallel map tasks instead of one client cursor.

**How it works:**

- An import reads slices of a table through JDBC and writes them to HDFS, Hive, or HBase. An export reads files from HDFS and inserts or updates rows back in the database.
- The split column, often a numeric primary key, divides the table into ranges. Each range is one mapper.
- Connectors cover common databases through JDBC. A direct mode exists for some engines and bypasses JDBC for speed.
- Incremental import can pull only rows newer than a last-value column, which is how a nightly warehouse stays in step with an operational table.

**Examples:**

```text
sqoop import --table orders --split-by id --target-dir /warehouse/orders
```

Four mappers might read `id` ranges 1–250000, 250001–500000, and so on, and write four part files.
</details>

<details>
<summary>How does Apache Oozie help in workflow scheduling in Hadoop?</summary>

### How does Apache Oozie help in workflow scheduling in Hadoop?

**Definition:**

Oozie runs a directed graph of Hadoop actions and can start that graph on a clock or when input data appears.

**How it works:**

- A workflow is the graph: MapReduce, Hive, Pig, Sqoop, shell, and similar action nodes, plus fork, join, and decision nodes.
- A coordinator fires the workflow on a schedule or when a dataset instance is ready. A bundle groups coordinators that should be started and stopped together.
- Each action reports success or failure. The graph can retry a step or follow an error transition. Parameters use expression language so one definition serves many dates.
- The Oozie server tracks state outside the short-lived YARN application, so a multi-hour pipeline can resume after a single action fails.

**Examples:**

A nightly graph forks a Sqoop import and a Flume roll-up, joins, then runs a Hive aggregation. The coordinator starts it when `/raw/2026-10-04` exists, and a failed Hive step follows the error path instead of marking the whole day successful.
</details>

<details>
<summary>What is Apache ZooKeeper and why is it important for Hadoop?</summary>

### What is Apache ZooKeeper and why is it important for Hadoop?

**Definition:**

ZooKeeper is a small replicated store for coordination: configuration, membership, and who the current leader is.

**How it works:**

- Clients see a tree of znodes. Updates are totally ordered. A session can create an ephemeral node that disappears when the session dies, and a sequential node whose name gets a counter.
- A watch notifies a client when a znode changes, so services do not poll every peer. The ensemble commits only when a majority agrees.
- HDFS high availability uses ZooKeeper so one NameNode is active. HBase uses it to elect the master and publish which server hosts a region. YARN can store the ResourceManager's recovery state there.
- It is a coordination service, not a database for application records. The data in a znode is meant to stay small.
</details>

<details>
<summary>Discuss the role of Apache Spark in the Hadoop ecosystem.</summary>

### Discuss the role of Apache Spark in the Hadoop ecosystem.

**Definition:**

Spark runs as an application on YARN, Hadoop's resource manager, and reads data from HDFS. It is not a replacement for the filesystem.

**How it works:**

- HDFS stores large files as blocks on DataNodes. The NameNode keeps the namespace and the block map. User data does not flow through the NameNode. The model is write-once, read-many, aimed at high throughput on commodity hardware.
- Spark's RDD guide says an RDD can be created from a file in the Hadoop file system. To touch HDFS you add a hadoop-client dependency and point HADOOP_CONF_DIR or YARN_CONF_DIR at the client configuration.
- spark-submit --master yarn uses the ResourceManager address from that Hadoop configuration. In cluster mode the driver runs inside the YARN application master and the client can leave. In client mode the driver stays in the client process.
- Spark can persist an RDD in memory for reuse across operations, which is the point of running it beside batch MapReduce on the same HDFS files. Recovery of a lost partition is Spark's RDD lineage. Recovery of a lost disk is still HDFS re-replication.
- Java 17 is required by Spark 4.0.0 and later. Hadoop supports Java 17 from 3.5.0. An older YARN cluster needs a separate JDK for the Spark application.

**When to use:**

Use Spark on YARN when the bytes already live in HDFS and you want in-memory parallel operations. Use HDFS replication and the NameNode whether or not Spark is the compute engine.

**Sources:**

- https://spark.apache.org/docs/latest/running-on-yarn.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

### Data Management and Processing

<details>
<summary>How does Hadoop handle the failure of a datanode?</summary>

### How does Hadoop handle the failure of a datanode?

**Definition:**

HDFS expects DataNodes to fail. The NameNode notices a missing heartbeat and replicates the blocks that fell below their replication factor.

**How it works:**

- Each DataNode sends a Heartbeat and a Blockreport. A heartbeat means the node is functioning. A Blockreport lists its blocks.
- No recent heartbeat: the NameNode marks the DataNode dead and stops sending it new IO. The default timeout is conservatively long, over 10 minutes, to avoid a replication storm from flapping. A shorter stale interval can be configured.
- Death, a corrupt replica, a failed disk, or a raised replication factor all make the NameNode re-replicate under-replicated blocks.
- Clients checksum data. If a replica fails the checksum, the client can read another replica.
- User data does not flow through the NameNode. Losing a DataNode loses copies of blocks, not the namespace.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Explain the process of data replication in HDFS.</summary>

### Explain the process of data replication in HDFS.

**Definition:**

Each file is a sequence of blocks, and each block is stored on multiple DataNodes. The NameNode decides placement. Clients write through a pipeline.

**How it works:**

- Block size and replication factor are per file. All blocks except the last are the same size. A typical block is 128 MB. Files are write-once except for appends and truncates, with one writer at a time.
- The NameNode chooses the DataNodes for a block. With replication factor three, the client writes to the first DataNode, which forwards each portion to the second, which forwards to the third. A node can receive and forward at the same time.
- Replica placement is rack-aware so copies are not all on one rack. The NameNode makes every replication decision.
- A client verifies checksums and can switch to another replica.
- More replicas cost disk and network. They are how HDFS stays available when hardware fails, which the design treats as normal.

**Examples:**

```text
client -> DN1 -> DN2 -> DN3
replication factor 3, block typically 128 MB
```

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>What is speculative execution in Hadoop, and why is it used?</summary>

### What is speculative execution in Hadoop, and why is it used?

**Definition:**

The MapReduce framework reserves task slots for speculative-tasks and for failed tasks so one slow task does not have to be the only copy of that work.

**How it works:**

- The tutorial's sizing note keeps the number of reduce tasks slightly under a whole-number scaling factor in order to leave a few reduce slots for speculative-tasks and failed tasks.
- Failed tasks need somewhere to run again. The same spare capacity is what the framework uses for speculative-tasks.
- More reduce tasks add framework overhead and improve load balancing, and they lower the cost of a failure. Zero reduce tasks is legal: map output then goes straight to the FileSystem and is not sorted.
- This is capacity inside the job, not HDFS re-replication. A dead DataNode is healed by the NameNode copying blocks. A slow task is a MapReduce scheduling concern.
- Do not spend every slot on the first attempt. The tutorial's reason for the spare slots is those extra tasks.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>How are large datasets processed in Hadoop?</summary>

### How are large datasets processed in Hadoop?

**Definition:**

HDFS stores the file as large replicated blocks. MapReduce gives each mapper one input split, and reducers combine by key after a shuffle.

**How it works:**

- HDFS is built for streaming throughput on large files, not low-latency interactive edits. A typical file is gigabytes to terabytes. Computation is moved to the data.
- An InputSplit is what one Mapper processes. FileSplit is the default. A RecordReader turns the byte-oriented split into records.
- The mapper emits key-value pairs. A combiner may aggregate locally. The partitioner assigns keys to reducers. The reducer receives an iterable of values per key.
- The WordCount tutorial wires this with Job, TokenizerMapper, and IntSumReducer used both as combiner and reducer.
- Spark on YARN can read the same HDFS file into an RDD and transform it in memory. The bytes still live in HDFS.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
</details>

<details>
<summary>What is the significance of the input split in MapReduce jobs?</summary>

### What is the significance of the input split in MapReduce jobs?

**Definition:**

An InputSplit is the data one Mapper processes. It is usually a byte range. The RecordReader turns that range into records.

**How it works:**

- FileSplit is the default InputSplit. It sets mapreduce.map.input.file to the path of the input file for that logical split.
- The split is not required to be one HDFS block, but locality is the point of moving computation to the data. A mapper should run near the bytes it reads.
- Record boundaries are the RecordReader's job. The mapper sees keys and values, not the raw split edges.
- Too many tiny splits mean too many map tasks and more framework overhead. The tutorial notes that more tasks improve balance and also add overhead.
- The split is a compute boundary. Replication of the underlying block is an HDFS concern.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>How does partitioning work in Hadoop, and when is it used?</summary>

### How does partitioning work in Hadoop, and when is it used?

**Definition:**

The partitioner assigns intermediate keys to reduce tasks. Keys that must be aggregated together have to land on the same reducer.

**How it works:**

- The tutorial states it directly: the partitioner partitions the key space and controls partitioning of the keys of the intermediate map outputs.
- After the map, pairs are shuffled to the reducer selected for that key. The reducer then sees every value for the keys it owns.
- Use it whenever more than one reducer should run. With zero reducers there is no partition step: map output is written straight to the FileSystem and is not sorted.
- A bad partition sends all keys to one reducer. The tutorial's note on more reducers is about load balancing. Spare slots are still left for speculative-tasks and failures.
- This is not HDFS block placement. The NameNode places blocks. The partitioner places keys.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
</details>

<details>
<summary>Explain how reducers work in MapReduce and their interaction with shufflers.</summary>

### Explain how reducers work in MapReduce and their interaction with shufflers.

**Definition:**

A reducer receives, for each key it owns, the values that mappers emitted for that key. The shuffle is the movement of those pairs from mappers to the right reducer.

**How it works:**

- In the WordCount tutorial the reducer implements reduce(Text key, Iterable values, Context). It sums the values for that key.
- The partitioner chooses which reducer owns the key. Shuffle delivers the pairs. The framework sorts map outputs before the reducer when reducers exist.
- A combiner can sum locally on the map side first, using the same IntSumReducer class in the example, so fewer pairs cross the network.
- Setting the number of reduce tasks to zero skips this. Map outputs go to the path from FileOutputFormat.setOutputPath and are not sorted.
- More reducers spread the shuffle and lower the cost of one failure, at the cost of framework overhead.

**Examples:**

```java
job.setMapperClass(TokenizerMapper.class);
job.setCombinerClass(IntSumReducer.class);
job.setReducerClass(IntSumReducer.class);
```

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
</details>

<details>
<summary>What are SequenceFiles in Hadoop?</summary>

### What are SequenceFiles in Hadoop?

**Definition:**

MapReduce's own output path is a file of records written by a RecordWriter. HDFS stores that file as blocks; the record format is a job choice, not a block size.

**How it works:**

- OutputFormat describes the job output. The framework uses a RecordWriter to write records. FileOutputFormat.setOutputPath sets where map output goes when there are no reducers, and where the job output goes otherwise.
- HDFS then stores that file as replicated blocks, typically 128 MB, write-once except for append and truncate.
- The NameNode stores the namespace entry. The bytes sit on DataNodes. A compact container of many records keeps the namespace smaller than one file per record, which matches HDFS being tuned for large files.
- The WordCount job writes Text keys and IntWritable values. That pair is the record. The block boundaries are HDFS's concern.
- Spark can read a Hadoop-supported file into an RDD afterward. The file format does not change the NameNode's replication decisions.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

### Performance Tuning and Optimization

<details>
<summary>Describe the ways to optimize a MapReduce job.</summary>

### Describe the ways to optimize a MapReduce job.

**Definition:**

Cut bytes on the shuffle, balance reducers, and leave spare slots. Do not expect the NameNode to stream the file data.

**How it works:**

- A combiner aggregates on the map side. WordCount uses IntSumReducer for both combiner and reducer so counts cross the network instead of every token.
- The partitioner must spread keys. One hot key makes one reducer the job.
- More reducers improve balance and lower the cost of a failure, and they add overhead. The tutorial keeps a few slots for speculative-tasks and failed tasks.
- Input splits should track data locality. HDFS's rule is that moving computation is cheaper than moving data.
- Compressing map output is listed in the tutorial under useful features. The output path is still FileOutputFormat. Zero reducers skip the sort when you truly do not need one.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>How can you diagnose and troubleshoot Hadoop performance issues?</summary>

### How can you diagnose and troubleshoot Hadoop performance issues?

**Definition:**

Separate a metadata problem on the NameNode from a data problem on DataNodes and from a skewed MapReduce shuffle.

**How it works:**

- If jobs cannot open or list files, look at the NameNode. It holds the namespace. FsImage and EditLog are its central structures. User data does not flow through it.
- If reads fail or are slow, check dead DataNodes and under-replicated blocks. Missing heartbeats, with the long default timeout, delay re-replication. Checksums tell you a replica is corrupt.
- If one reducer runs forever, the partitioner or a hot key is the cause. Counters are a tutorial feature for seeing task progress.
- Task logs are part of the tutorial's task environment. A map task far from its split violates locality.
- Spark on the same cluster has its own driver UI. Do not debug an RDD stage with only NameNode heartbeats.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>What is the significance of combiner in the Hadoop MapReduce framework?</summary>

### What is the significance of combiner in the Hadoop MapReduce framework?

**Definition:**

A combiner aggregates map output locally, before the shuffle, so the network carries less data. In WordCount it is the same class as the reducer.

**How it works:**

- The tutorial sets job.setCombinerClass(IntSumReducer.class) and the same class as the reducer. Each map's pairs are sorted by key and then aggregated locally.
- The documented local result is counts such as Hello 1 and World 2 on that map, not every raw token.
- The reducer still sees an iterable per key and sums again. The combiner must be safe to apply more than once for the job's aggregation. Sum is. An average written as a single number is not, unless the value carries the count.
- The combiner does not change partitioning. Keys still go to the reducer the partitioner chose.
- With zero reducers there is no shuffle to shrink.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
</details>

<details>
<summary>Explain what you can do to optimize the performance of HDFS.</summary>

### Explain what you can do to optimize the performance of HDFS.

**Definition:**

Keep files large, keep replicas rack-aware, and keep the NameNode off the data path.

**How it works:**

- HDFS is tuned for large files and streaming throughput. A typical block is 128 MB. A namespace full of tiny files spends the NameNode on metadata.
- Replication factor is per file. Raise it for data you cannot lose. Do not raise it for every temporary file. The write pipeline copies every portion to each replica.
- Rack-aware placement improves reliability and bandwidth. The NameNode chooses targets.
- The dead-node timeout is long by default to avoid replication storms. For latency-sensitive reads, the design allows a shorter stale interval so clients avoid stale nodes.
- Multiple synchronous copies of FsImage and EditLog protect metadata and can slow namespace operations. That trade-off is accepted because applications are data-intensive, not metadata-intensive. HA with a Journal is the recommended resilience option.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>How can job scheduling be optimized in Hadoop?</summary>

### How can job scheduling be optimized in Hadoop?

**Definition:**

Submit the MapReduce job to a queue, size map and reduce tasks to the splits and the key space, and leave room for retries.

**How it works:**

- The tutorial lists submitting jobs to queues as a user-facing feature, next to counters and the distributed cache.
- Map parallelism follows input splits. Reduce parallelism follows how you partition keys. Too few reducers skew. Too many add overhead.
- Reserve slots for speculative-tasks and failed tasks instead of filling every slot with the first wave.
- YARN is the resource manager Spark also uses. spark-submit --master yarn --queue thequeue places the application on a queue. Cluster mode runs the driver in the application master.
- A queue does not repair under-replicated blocks. That stays with the NameNode.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
</details>

<details>
<summary>What are the best practices for managing memory and CPU resources in a Hadoop cluster?</summary>

### What are the best practices for managing memory and CPU resources in a Hadoop cluster?

**Definition:**

The tutorial treats task memory and map/reduce parameters as job configuration. HDFS block placement is a separate budget from task heaps.

**How it works:**

- MapReduce task execution has a memory-management section and separate map parameters and shuffle/reduce parameters. Set those on the job instead of hoping the default heap fits the split.
- A combiner cuts shuffle bytes, which cuts memory on the reduce side.
- Zero reducers avoid the sort buffer when you do not need aggregation.
- On YARN, Spark's example sets --driver-memory and --executor-memory explicitly. The same idea applies: the container size is a request, not whatever the machine has free.
- DataNodes store blocks on disk. Do not size the task heap as if the whole 128 MB block must be one Java object. The RecordReader walks records.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

### Coding Challenges

<details>
<summary>Write a MapReduce job in Java that counts the number of words in a text file.</summary>

### Write a MapReduce job in Java that counts the number of words in a text file.

**Definition:**

The MapReduce tutorial's WordCount job maps tokens to 1 and sums them in a combiner and a reducer.

**How it works:**

- A Job is created with a name. The jar class, mapper, combiner, reducer, and output key and value classes are set.
- FileInputFormat adds the input path. FileOutputFormat sets the output path. waitForCompletion runs the job.
- The mapper emits Text and IntWritable. The reducer sums the iterable for each word. The combiner is the same reducer class so each map aggregates first.
- Input splits and record readers feed the mapper. You do not open the HDFS block yourself.
- The process exit code is 0 only if waitForCompletion returns true.

**Examples:**

```java
Job job = Job.getInstance(conf, "word count");
job.setJarByClass(WordCount.class);
job.setMapperClass(TokenizerMapper.class);
job.setCombinerClass(IntSumReducer.class);
job.setReducerClass(IntSumReducer.class);
job.setOutputKeyClass(Text.class);
job.setOutputValueClass(IntWritable.class);
FileInputFormat.addInputPath(job, new Path(args[0]));
FileOutputFormat.setOutputPath(job, new Path(args[1]));
System.exit(job.waitForCompletion(true) ? 0 : 1);
```

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
</details>

<details>
<summary>Create an HDFS command script that copies files from a local file system to HDFS.</summary>

### Create an HDFS command script that copies files from a local file system to HDFS.

**Definition:**

The File System shell command put copies one or more local paths into the destination file system.

**How it works:**

- Usage is hadoop fs -put with optional flags, then the sources and the destination.
- Copying fails if the destination file already exists, unless -f is set. -f overwrites.
- -p preserves times, ownership, and permissions when they can cross filesystems.
- -l lets the DataNode persist lazily and forces replication factor 1. The shell docs say this reduces durability. Do not use it for data you must keep.
- The same shell also has mkdir, rm, and cat, documented on the HDFS design page with bin/hadoop dfs and bin/hadoop fs forms. put is the copy-in command on the FileSystemShell page.

**Examples:**

```text
hadoop fs -put ./events/ /data/events
# overwrite:
hadoop fs -put -f ./events/part-0 /data/events/part-0
```

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/FileSystemShell.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Implement a simple Hive query to summarize data from a Hive table.</summary>

### Implement a simple Hive query to summarize data from a Hive table.

**Definition:**

HiveQL SELECT reads a table, filters with WHERE, and aggregates with GROUP BY. Table and column names are case insensitive.

**How it works:**

- SELECT select_expr FROM table_reference WHERE condition GROUP BY col_list is the shape in the language manual.
- A summary is a GROUP BY plus an aggregate in the select list. HAVING can filter groups. The manual's example is SELECT col1 FROM t1 GROUP BY col1 HAVING SUM(col2) > 10.
- Without a partition predicate, a SELECT scans the whole table. If the table is PARTITIONED BY a column, a predicate on that column in WHERE prunes partitions. The manual shows this on page_views.date.
- LIMIT n caps rows. ORDER BY with LIMIT returns an ordered prefix, as in the customers example.
- Hive does not place HDFS blocks. The table's files are still HDFS files with the usual replication.

**Examples:**

```sql
SELECT region, SUM(amount)
FROM sales
WHERE amount > 10
GROUP BY region
```

**Sources:**

- https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Code a Pig Latin script to process and transform a dataset into a desired format.</summary>

### Code a Pig Latin script to process and transform a dataset into a desired format.

**Definition:**

The same pipeline the MapReduce tutorial spells out in Java is: read records, project fields, filter, and aggregate by key. Pig Latin is a separate language for that pipeline. The operations below are the ones the MapReduce and Hive manuals actually specify.

**How it works:**

- A mapper emits the fields you keep. A combiner and reducer aggregate by key, as WordCount does with TokenizerMapper and IntSumReducer.
- Hive's SELECT … WHERE … GROUP BY is the declarative form of that filter and aggregate when the data is a Hive table.
- HDFS put lands the source file. The job then uses FileInputFormat on that path.
- Do not invent a Pig operator name in place of these. If the estate standardizes on Pig, match its current manual. The semantics you must preserve are the ones above.
- Keep the result in large files. HDFS is tuned for large blocks, typically 128 MB, not one file per record.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/FileSystemShell.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Automate a process to import data from a MySQL database into HDFS using Sqoop.</summary>

### Automate a process to import data from a MySQL database into HDFS using Sqoop.

**Definition:**

The landed bytes have to become an HDFS file via the File System shell or a job input path. A database import tool is only useful if its output is that file.

**How it works:**

- hadoop fs -put copies local files into HDFS. If an export already produced files, put is the documented copy. -f overwrites. Avoid -l when you need normal replication. It forces replication factor 1.
- Once the path exists, a MapReduce job points FileInputFormat at it, or a Hive table reads it.
- HDFS will replicate the blocks, typically with the file's replication factor, through the write pipeline.
- Do not point the NameNode at the database. User data does not flow through the NameNode. The import writes blocks to DataNodes.
- A failed DataNode during the write is handled by the pipeline and later by re-replication if a replica is lost. The import is not done until the file is closed and the replicas exist.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/FileSystemShell.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
</details>

<details>
<summary>Implement a Spark job in Scala or Python to perform a join operation on two datasets.</summary>

### Implement a Spark job in Scala or Python to perform a join operation on two datasets.

**Definition:**

On RDDs of pairs, join returns (K, (V, W)) for every pair of values that share a key. Spark can read both inputs from HDFS.

**How it works:**

- The RDD guide defines join(otherDataset, [numPartitions]) on datasets of type (K, V) and (K, W). leftOuterJoin, rightOuterJoin, and fullOuterJoin are the outer forms.
- Build one SparkContext from a SparkConf. Only one context per JVM. On a cluster, let spark-submit set the master. For YARN that master is yarn.
- textFile on an HDFS path needs the Hadoop configuration on the classpath, HADOOP_CONF_DIR or YARN_CONF_DIR, and a hadoop-client dependency.
- numPartitions on the join controls the shuffle partitions. That is the same concern as a MapReduce partitioner: one hot key lands in one partition.
- persist an RDD you join more than once. The guide says persistence reuses the dataset across actions.

**Examples:**

```scala
val left = sc.textFile(pathA).map(parseLeft)   // (K, V)
val right = sc.textFile(pathB).map(parseRight) // (K, W)
val joined = left.join(right)
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
</details>

<details>
<summary>Develop an Oozie workflow that schedules and runs a set of MapReduce and Hive jobs.</summary>

### Develop an Oozie workflow that schedules and runs a set of MapReduce and Hive jobs.

**Definition:**

A workflow is a sequence of a MapReduce job and a Hive query. The primitives you can actually run from the fetched manuals are waitForCompletion and a Hive SELECT.

**How it works:**

- The MapReduce job is the WordCount-shaped Job: set mapper, combiner, reducer, input path, output path, then waitForCompletion.
- The Hive step is a SELECT with WHERE and GROUP BY against a table whose files are already in HDFS. Partition predicates prune the scan.
- Order them so the Hive table path exists before the query. put or the MapReduce output path creates that file.
- YARN queues are how the cluster schedules the containers. The MapReduce tutorial lists submitting jobs to queues. Spark uses --queue when the compute is Spark instead.
- A scheduler that ignores a failed waitForCompletion and still runs the query will summarize a missing path. The exit code of the job is the gate.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select
- https://spark.apache.org/docs/latest/running-on-yarn.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/FileSystemShell.html
</details>

<details>
<summary>Write a Java program to implement custom input and output format classes in Hadoop.</summary>

### Write a Java program to implement custom input and output format classes in Hadoop.

**Definition:**

A custom input format must still produce InputSplits and a RecordReader. A custom output format must still provide a RecordWriter. The job then names those classes.

**How it works:**

- InputSplit is the byte-oriented piece one mapper processes. FileSplit is the default and sets mapreduce.map.input.file. RecordReader presents records to the mapper.
- OutputFormat is the output specification. The framework writes through a RecordWriter. FileOutputFormat.setOutputPath is where the default file output goes.
- Your format class is selected on the Job the same way the tutorial selects the mapper and reducer classes. The mapper should still see keys and values, not HDFS block offsets, unless the record is that offset.
- The bytes you write are an HDFS file. Replication and block size stay HDFS settings, typically 128 MB blocks.
- Keep record boundaries inside the RecordReader. A split that cuts a record in half is the reader's problem to fix, which is why the tutorial assigns boundaries to the RecordReader.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

### Advanced Hadoop Features and Architecture

<details>
<summary>What is the concept of erasure coding in HDFS, and how does it differ from replication?</summary>

### What is the concept of erasure coding in HDFS, and how does it differ from replication?

**Definition:**

The HDFS architecture's documented fault tolerance for file data is replication: full copies of each block, not a parity layout.

**How it works:**

- A file is a sequence of blocks. Blocks are replicated. The factor is configurable per file. With factor three, the write pipeline stores three full copies.
- A lost DataNode drops the replication factor of its blocks. The NameNode re-replicates until the factor is met again. A client that fails a checksum reads another full replica.
- That costs disk equal to the factor times the data. The design accepts the cost because hardware failure is normal.
- The architecture page does not define a parity or erasure layout. Do not describe one as if it were this document. The mechanism it specifies is extra full replicas and checksums.
- Metadata is different again: multiple FsImage and EditLog copies, or HA with a Journal. That is not block erasure coding either.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Explain how Hadoop uses data locality to improve performance.</summary>

### Explain how Hadoop uses data locality to improve performance.

**Definition:**

HDFS places blocks on DataNodes and expects the computation to run near those blocks, because moving computation is cheaper than moving the data.

**How it works:**

- The design states that assumption directly and says HDFS gives applications interfaces to move themselves closer to the data.
- A mapper's InputSplit should be the bytes on a nearby node. FileSplit names the file. The scheduler's win is a task on a node that already stores the block.
- Replication makes locality easier: factor three means three nodes can host the task. Rack-aware placement keeps copies in more than one rack so locality and failure tolerance are both possible.
- The NameNode does not stream the user data. A job that reads everything through the NameNode has abandoned locality.
- Spark on YARN has the same interest. An RDD created from an HDFS file is partitioned. A shuffle, such as join, moves data and is the expensive part. The read of the file should still be local.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>How does Hadoop support different file formats, and what are some of them?</summary>

### How does Hadoop support different file formats, and what are some of them?

**Definition:**

HDFS stores bytes in blocks. The record layout is chosen by the MapReduce input and output formats, or by a reader such as Spark.

**How it works:**

- FileSplit plus a RecordReader is the default way a mapper sees records. A different RecordReader is a different format on the same blocks.
- OutputFormat and RecordWriter choose how results are written. WordCount writes Text and IntWritable through the job's output classes.
- Hive reads tables. A SELECT does not care about the 128 MB block size. Partition pruning cares about the partition column.
- Spark's textFile reads a Hadoop-supported filesystem into an RDD of lines. That is one format: text lines. join then works on pairs you parse.
- The NameNode still stores one namespace entry per file. A format that creates millions of small files pressures metadata even if each file is a valid format.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>What is Hadoop federation, and how can it scale a Hadoop cluster?</summary>

### What is Hadoop federation, and how can it scale a Hadoop cluster?

**Definition:**

The documented HDFS design has one NameNode for the namespace. That is simple, and it is also the metadata ceiling. Data scales by adding DataNodes.

**How it works:**

- A single NameNode arbitrates the namespace. User data never flows through it. Capacity for bytes is more DataNodes and more blocks.
- The NameNode can keep multiple synchronous copies of FsImage and EditLog. That protects metadata and can reduce namespace operations per second. Applications are expected to be data-intensive, not metadata-intensive.
- High availability uses multiple NameNodes with shared storage or, preferably, a distributed edit log called a Journal. That is failover of one namespace, not a second independent namespace.
- Adding DataNodes scales storage and the places mappers can run locally. It does not remove the namespace role of the NameNode.
- Spark and MapReduce scale compute on YARN by more tasks and more nodes. They still look up blocks through that namespace.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
</details>

<details>
<summary>Discuss the concept and benefits of a journal node in HDFS HA configuration.</summary>

### Discuss the concept and benefits of a journal node in HDFS HA configuration.

**Definition:**

A Journal is the recommended distributed edit log for high availability with multiple NameNodes.

**How it works:**

- FsImage and EditLog are the central metadata structures. Corruption of them can make the instance non-functional.
- The NameNode can keep multiple copies and update them synchronously. That is safe and can slow namespace throughput.
- The design's other option is high availability using multiple NameNodes, either with shared NFS storage or with a distributed edit log called a Journal. The Journal is the recommended approach.
- On restart, a NameNode selects the latest consistent FsImage and EditLog.
- The Journal does not replicate file blocks. Block replicas stay on DataNodes under the replication factor. The Journal is how standby NameNodes share the edit stream of one namespace.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>What are the implications of small files on HDFS performance and how can this be mitigated?</summary>

### What are the implications of small files on HDFS performance and how can this be mitigated?

**Definition:**

HDFS is tuned for large files. A typical block is 128 MB. Many small files turn into many namespace entries on the single NameNode and into many map tasks.

**How it works:**

- The NameNode holds the namespace and the block map. User data does not flow through it, but every file is still a metadata object. The design says applications are not metadata-intensive. A flood of small files breaks that assumption.
- A file smaller than the block still occupies a block mapping. You pay NameNode memory for little data.
- MapReduce maps a split toward a task. A pile of tiny files becomes a pile of tasks and the framework overhead the tutorial warns about.
- Mitigation that stays inside the docs: write large files, let a RecordWriter pack many records into one output path, and use Hive partitions only where a predicate can prune a scan. Do not create one HDFS file per event.
- Replication multiplies whatever you stored. Factor three on millions of tiny files multiplies the metadata and the blocks.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select
</details>

### Troubleshooting and Maintenance

<details>
<summary>How would you recover a Hadoop cluster from a Namenode failure?</summary>

### How would you recover a Hadoop cluster from a Namenode failure?

**Definition:**

Metadata recovery uses the latest consistent FsImage and EditLog, or a standby NameNode fed by the Journal. Block data is still on the DataNodes.

**How it works:**

- The design lists NameNode failures, DataNode failures, and network partitions as the three common failures.
- FsImage and EditLog are central. The NameNode can maintain multiple copies, updated synchronously. On restart it selects the latest consistent pair.
- The recommended HA path is multiple NameNodes with a distributed edit log, the Journal, rather than only shared NFS.
- While the NameNode is down, the namespace is down. DataNodes still hold blocks, but clients need the NameNode to resolve paths. This is not fixed by re-replicating blocks.
- After the namespace is back, under-replicated blocks are a separate pass: heartbeats and re-replication. Snapshots can roll a corrupted instance back to a known time.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>What considerations should be made for Hadoop cluster backup and disaster recovery?</summary>

### What considerations should be made for Hadoop cluster backup and disaster recovery?

**Definition:**

Back up the namespace and keep block replicas. A snapshot can return the instance to a known time. Replication is not a backup of a bad write.

**How it works:**

- FsImage and EditLog are the metadata you cannot reconstruct from DataNodes alone. Multiple synchronous copies, or a Journal in HA, are the design's protection. A backup plan has to include those files.
- Block replicas survive a DataNode disk. They do not survive an application that overwrites or deletes the file on every replica. Snapshots store a copy at an instant and can roll back a corrupted instance.
- put -l forces replication factor 1 and the docs say it reduces durability. Do not use it for data you must restore.
- Restoring an older FsImage rewinds the namespace. Files created after that image disappear from the namespace even if blocks remain.
- Compute jobs are replaceable. The WordCount job can be submitted again. The file it read cannot, unless the replicas or a snapshot still hold it.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/FileSystemShell.html
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
</details>

<details>
<summary>How would you monitor the health of a Hadoop cluster, and what tools would you use?</summary>

### How would you monitor the health of a Hadoop cluster, and what tools would you use?

**Definition:**

Watch NameNode liveness, DataNode heartbeats, under-replicated blocks, and MapReduce task progress.

**How it works:**

- DataNodes send heartbeats and block reports. Absence of a heartbeat is how the NameNode declares a node dead. The long default timeout means a dashboard should also show stale nodes, which the design lets you mark sooner.
- Under-replicated blocks are the queue the NameNode is trying to heal. A rising queue after a disk failure is expected. A queue that never drains is not.
- Checksum failures mean clients are rejecting replicas. That is corruption, not slowness.
- The MapReduce tutorial exposes counters, task logs, and job submission status through waitForCompletion. A stuck reducer is a skew signal.
- FsImage and EditLog health is the metadata signal. HA with a Journal should show which NameNode is active. Spark's own UI is a different process when the job is Spark on YARN.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
</details>

<details>
<summary>Discuss a strategy for Hadoop cluster capacity planning and scaling.</summary>

### Discuss a strategy for Hadoop cluster capacity planning and scaling.

**Definition:**

Plan bytes as blocks on DataNodes and plan metadata as namespace objects on the NameNode. They do not scale the same way.

**How it works:**

- A 128 MB block and a replication factor of three turn one terabyte of user data into more than three terabytes of disk, plus pipeline traffic on the write.
- The NameNode holds the map of those blocks. Large files keep that map smaller. Small files exhaust it first.
- Add DataNodes to add disk and local map slots. Rebalancing can move data when free space is uneven. The design says the architecture is compatible with rebalancing schemes.
- Add reduce and map slots with the tutorial's overhead in mind. Leave room for speculative-tasks and failures.
- For compute beyond MapReduce, add YARN capacity and run Spark with explicit executor memory. The HDFS namespace is still one NameNode unless you are in the HA Journal setup, which is still one namespace.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
</details>

### Scenario-Based Questions and Use Case Discussions

<details>
<summary>A company wants to process clickstream data in real-time. How would you integrate Hadoop and Spark to meet this requirement?</summary>

### A company wants to process clickstream data in real-time. How would you integrate Hadoop and Spark to meet this requirement?

**Definition:**

Land the stream as files in HDFS, and let Spark on YARN transform those files. HDFS is the durable store. Spark is the parallel compute.

**How it works:**

- hadoop fs -put, or the write pipeline, gets bytes onto DataNodes with replication. Do not use put -l if you need the normal factor.
- spark-submit --master yarn runs the application on the same cluster. Cluster mode keeps the driver in the application master. HADOOP_CONF_DIR must point at the Hadoop client config.
- An RDD from textFile is partitioned across the cluster. map and flatMap transform lines. A join is the pair RDD join. persist if several actions reread the same data.
- HDFS is built for high throughput, not for low-latency interactive edits. A true event-at-a-time path still has to bound its buffer. The batch file in HDFS is the durable copy the design actually specifies.
- Keep files large. A file per click will exhaust the NameNode. Roll clicks into files on the order of the 128 MB block.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/FileSystemShell.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Propose a data pipeline using Hadoop components to manage and analyze sensor data from IoT devices.</summary>

### Propose a data pipeline using Hadoop components to manage and analyze sensor data from IoT devices.

**Definition:**

Copy sensor files into HDFS, keep them large and replicated, then summarize with Hive or MapReduce and optionally join in Spark.

**How it works:**

- hadoop fs -put lands the files. Replication, not put -l, keeps durability. Blocks are typically 128 MB, pipelined to the replica DataNodes.
- A MapReduce job or a Hive SELECT … GROUP BY summarizes by sensor or by hour. A partition column in WHERE prunes the Hive scan.
- Spark join combines the summary with a dimension table if both are pair RDDs, running on YARN against the same HDFS paths.
- The NameNode must stay small. Roll sensor readings into large files. Monitor heartbeats and under-replicated blocks.
- Snapshots or the Journal-backed NameNode are the recovery path. Replicas protect disks. They do not protect a bad delete on every copy.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/FileSystemShell.html
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Select
- https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/running-on-yarn.html
</details>
