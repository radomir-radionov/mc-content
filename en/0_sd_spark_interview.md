# Apache Spark — Interview

Answers are filled.

[← Back to hub](./0_system_design.md)

### Related docs

- [Apache Spark — Interview](./0_sd_spark_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Apache Spark](https://devinterview.io/questions/machine-learning-and-data-science/apache-spark-interview-questions/).

### Spark Fundamentals

<details>
<summary>What is Apache Spark and what are its main components?</summary>

### What is Apache Spark and what are its main components?

**Definition:**

Apache Spark is a distributed engine for batch jobs, SQL, machine learning, and streaming, with intermediate data kept in memory when it fits.

**How it works:**

- Spark Core schedules jobs, partitions data, and provides the RDD execution model. Applications are written in Scala, Java, Python, R, or SQL.
- Spark SQL adds DataFrames and Datasets, a catalog, and the Catalyst planner. Structured Streaming uses the same tables for incremental queries.
- MLlib supplies distributed learning pipelines. GraphX supplies graph-parallel operators. Both sit on the same runtime.
- A cluster manager — standalone, YARN, or Kubernetes — places the driver and the executors. Spark itself is not the resource manager.

**Examples:**

```text
SparkSession
  -> Spark SQL / DataFrame job
  -> Catalyst plan
  -> tasks on executors
```
</details>

<details>
<summary>Explain how Apache Spark differs from Hadoop MapReduce.</summary>

### Explain how Apache Spark differs from Hadoop MapReduce.

**Definition:**

Both can scan data on a cluster. Spark keeps a pipeline in memory and offers more than one map phase plus one reduce phase.

**How it works:**

- MapReduce writes each stage to disk before the next job starts. Spark passes partitions in memory and spills only when a partition does not fit.
- A MapReduce job is a fixed map then reduce. A Spark job is a graph of transformations, including SQL, streaming, and iterative loops in one application.
- Spark APIs are higher level: DataFrames, SQL, and typed Datasets. MapReduce asks the author to write the mapper and reducer classes.
- A lost Spark partition is rebuilt from lineage. MapReduce reruns the failed task from its input split. Both retry failed tasks.
- Spark still has shuffle latency. A single heavy shuffle of data larger than memory can look closer to a MapReduce job than to a pure in-memory one.
</details>

<details>
<summary>Describe the concept of RDDs (Resilient Distributed Datasets) in Spark.</summary>

### Describe the concept of RDDs (Resilient Distributed Datasets) in Spark.

**Definition:**

An RDD is an immutable, partitioned collection. Transformations record a new RDD; they do not run until an action needs a result.

**How it works:**

- Partitions are the units of parallelism. A narrow transformation such as map or filter stays inside a partition. A wide one such as groupByKey or join shuffles across the network.
- Lineage is the chain of parent RDDs and the function that built each partition. If a partition is lost, Spark recomputes it from those parents.
- Actions — count, collect, save — launch a job. Calling collect on a huge RDD pulls the result to the driver and can exhaust it.
- cache or persist keeps a computed RDD for the next action. Checkpoint cuts a long lineage by writing a materialised copy.
</details>

<details>
<summary>What are DataFrames in Spark and how do they compare to RDDs?</summary>

### What are DataFrames in Spark and how do they compare to RDDs?

**Definition:**

A DataFrame is a distributed table with named columns and a schema. The planner can rewrite it; an RDD of opaque objects usually cannot.

**How it works:**

- Rows are stored in a compact columnar format, so filters and aggregations touch only the columns they need.
- Catalyst sees relational operators: filter, project, join, aggregate. It can push a predicate into the source and choose a join strategy. An RDD map is a black-box function.
- The same logical join written with keyBy and join on RDDs is longer and skips those rewrites. A Dataset adds compile-time types on top of the same plan in Scala and Java.
- Use an RDD when records have no useful columns. Use a DataFrame for structured files, JDBC, and SQL.

**Examples:**

```text
events.filter(col("day") == "2026-10-04").groupBy("path").count()
```

The planner can drop unused columns and skip files whose partition is another day. The equivalent RDD `filter` plus `reduceByKey` hides that structure inside lambdas.
</details>

<details>
<summary>What is lazy evaluation and how does it benefit Spark computations?</summary>

### What is lazy evaluation and how does it benefit Spark computations?

**Definition:**

Spark records transformations and runs them only when an action asks for a result, so the whole chain can be planned as one job.

**How it works:**

- map, filter, select, and join build a logical plan. count, show, write, and collect trigger execution.
- The planner fuses narrow steps, drops unused work, and pipelines partitions so data is not materialised between every call.
- A transformation that is never used by an action is not computed. Two actions on the same uncached lineage compute it twice.
- Debugging is less local: a failure appears at the action, and the stack points back through the lineage.

**Examples:**

```text
df = spark.read.parquet("/events")
df2 = df.filter(df.day == "2026-10-04")   # no read yet
df2.count()                                # one job: read, filter, count
```
</details>

<details>
<summary>How does Spark achieve fault tolerance?</summary>

### How does Spark achieve fault tolerance?

**Definition:**

Spark treats a lost partition as something to recompute from its parents, and it retries failed tasks on other executors.

**How it works:**

- Each RDD partition stores lineage: the parent partitions and the function that produced it. Immutable inputs make that replay safe.
- Shuffle files are written so a downstream stage can fetch them again. A lost shuffle map output is recomputed.
- checkpoint writes a materialised RDD and forgets the lineage before that point, which bounds replay of a long iterative job.
- The driver can rerun a failed task. Speculative execution starts a second copy of a straggler. Cache is an optimisation; it is not the durability mechanism.
- An action's result is consistent with recomputation as long as the functions are deterministic. A non-deterministic map makes a replay diverge.
</details>

<details>
<summary>What is the role of Spark Driver and Executors?</summary>

### What is the role of Spark Driver and Executors?

**Definition:**

The driver builds the plan and schedules tasks. Executors run those tasks and hold cached partitions.

**How it works:**

- The driver hosts the SparkSession, turns the logical plan into stages, and sends tasks to executors. It also collects small results such as count and the rows from take.
- An executor is a worker process with a thread pool. It runs tasks, stores shuffle blocks, and keeps cached data.
- The cluster manager starts executors. In cluster deploy mode the driver itself runs inside the cluster; in client mode it stays on the submitting machine.
- A wide transformation becomes a stage boundary. The driver launches the next stage only after the shuffle outputs of the previous stage are available.
</details>

<details>
<summary>How does Spark's DAG (Directed Acyclic Graph) Scheduler work?</summary>

### How does Spark's DAG (Directed Acyclic Graph) Scheduler work?

**Definition:**

The DAG scheduler turns a job's lineage into stages split at shuffles, then the task scheduler runs the tasks inside a stage.

**How it works:**

- Narrow dependencies pipeline into one stage: several maps and filters on the same partition become one task.
- A wide dependency, such as a shuffle join or groupBy, ends the stage. The next stage cannot start until those outputs exist.
- The graph is acyclic because each RDD points only at older parents. The scheduler walks it from the action back to the sources.
- Failed tasks are resubmitted. If the failure rate in a stage is too high, the stage fails and lineage replay may rerun an earlier stage.

**Examples:**

```text
read -> filter -> map          stage 1 (no shuffle)
groupBy("user")                stage boundary
count                          stage 2
```
</details>

### Spark Architecture and Ecosystem

<details>
<summary>Explain the concept of a Spark Session and its purpose.</summary>

### Explain the concept of a Spark Session and its purpose.

**Definition:**

SparkSession is the entry object for one application: configuration, the catalog, SQL, and DataFrame reads and writes.

**How it works:**

- Builder sets the app name, master, and configs, then getOrCreate returns the session. A second call in the same process reuses it.
- spark.read and DataFrameWriter go through the session. spark.sql runs a query against temporary views and catalog tables.
- The catalog creates and drops views, caches tables, and lists databases. UDFs are registered on the session so SQL can call them.
- spark.sparkContext still exposes the older RDD entry point. Stopping the session releases the context and the executors it owns.

**Examples:**

```text
spark = SparkSession.builder.appName("daily").getOrCreate()
spark.read.parquet("/events").createOrReplaceTempView("events")
spark.sql("SELECT path, count(*) FROM events GROUP BY path")
```
</details>

<details>
<summary>How does Spark integrate with Hadoop components like HDFS and YARN?</summary>

### How does Spark integrate with Hadoop components like HDFS and YARN?

**Definition:**

Spark reads and writes HDFS with the Hadoop file APIs, and it can ask YARN to place the driver and the executors.

**How it works:**

- Paths such as `hdfs://namenode/events` use the cluster's Hadoop configuration: `core-site.xml` and `hdfs-site.xml`. The same readers open Parquet, ORC, and text.
- `--master yarn` submits the application to the ResourceManager. YARN allocates containers; Spark decides which tasks run inside them.
- Deploy mode `cluster` runs the driver in a YARN container, so the laptop can disconnect. Deploy mode `client` keeps the driver on the submitter, which must stay reachable by the executors.
- HDFS block locations still influence preferred task placement, the same data-local idea MapReduce uses.
</details>

<details>
<summary>Describe the various ways to run Spark applications (cluster, client, local modes).</summary>

### Describe the various ways to run Spark applications (cluster, client, local modes).

**Definition:**

Local mode runs everything in one process. Client and cluster modes differ in where the driver lives, not in how the job is written.

**How it works:**

- `local` and `local[n]` start driver and tasks in the submitting JVM. `local[*]` uses one thread per core. This is for development, not for a shared cluster.
- Standalone is Spark's own master and workers. YARN and Kubernetes are external managers. The application code stays the same.
- Client deploy mode leaves the driver on the machine that submitted the job. Executors must reach that driver, and a dead laptop kills the job.
- Cluster deploy mode places the driver inside the cluster. `spark-submit` only ships the application and can exit. Logs for the driver are then on the cluster, not in the local terminal.

**Examples:**

```text
spark-submit --master local[*] app.py
spark-submit --master yarn --deploy-mode client app.py
spark-submit --master yarn --deploy-mode cluster app.py
```
</details>

<details>
<summary>What are Spark's data source APIs and how do you use them?</summary>

### What are Spark's data source APIs and how do you use them?

**Definition:**

DataFrameReader and DataFrameWriter are the common API: pick a format, set options, and load or save a table.

**How it works:**

- `spark.read.format(name)` selects a source such as parquet, orc, json, csv, text, or jdbc. Options cover paths, schemas, and credentials. `load` returns a DataFrame.
- `df.write.format(name).mode(...)` saves. Modes include error if the path exists, overwrite, append, and ignore.
- Partition discovery treats directory names such as `day=2026-10-04` as columns. `partitionBy` on write creates those directories.
- A short name such as `spark.read.parquet(path)` is the same reader with the format already chosen. The table catalog API registers that DataFrame as a metastore table.

**Examples:**

```text
spark.read.format("csv").option("header", "true").schema(schema).load("/in")
df.write.mode("overwrite").partitionBy("day").parquet("/out")
```
</details>

<details>
<summary>Discuss the role of accumulators and broadcast variables in Spark.</summary>

### Discuss the role of accumulators and broadcast variables in Spark.

**Definition:**

Accumulators collect a side total from tasks. Broadcast variables ship one read-only copy of a small value to every executor.

**How it works:**

- An accumulator is write-only on executors and readable on the driver. Tasks add to it; only the driver should read `value`. Reliable updates are those performed inside actions, because a retried stage can add twice if the accumulator is not the kind that Spark corrects.
- A broadcast variable is copied once per executor and reused by every task there. Tasks must treat it as immutable.
- Use an accumulator for a diagnostic count, such as malformed rows. Use a broadcast for a lookup table that would otherwise be shipped inside every task closure.
- Neither replaces a shuffle. A large lookup belongs in a join, not in a broadcast that does not fit executor memory.

**Examples:**

```text
bad_rows = sc.accumulator(0)

def parse(line):
    if line.count(",") != 3:
        bad_rows.add(1)
        return None
    return line.split(",")

rates = sc.broadcast({"usd": 1.0, "eur": 0.92})
amount_usd = lambda row: row.amount / rates.value[row.currency]
```
</details>

<details>
<summary>What is the significance of the Catalyst optimizer in Spark SQL?</summary>

### What is the significance of the Catalyst optimizer in Spark SQL?

**Definition:**

Catalyst turns a DataFrame or SQL query into an executable plan by analysing it, rewriting it with rules, and picking physical operators.

**How it works:**

- The analyser resolves names, types, and functions against the catalog and rejects a query that does not type-check.
- Logical rules fold constants, push filters toward the source, prune columns, and simplify predicates. Extra rules can be added; the engine is a set of tree rewrites.
- Physical planning chooses operators such as broadcast hash join versus sort-merge join, and how to partition the shuffle, using available statistics when it has them.
- Generated code then runs the chosen plan. Caching a table is a separate storage decision; Catalyst's job is the plan, not a manual memory tier.

**Examples:**

```text
SELECT path FROM events WHERE day = '2026-10-04'
```

Analysis binds `events`. A logical rule can push `day` into a partitioned reader and drop every column except `path` before the scan.
</details>

<details>
<summary>How does Tungsten contribute to Spark's performance?</summary>

### How does Tungsten contribute to Spark's performance?

**Definition:**

Tungsten is Spark's execution layer that stores rows in a compact binary layout and runs generated code so the CPU spends less time on objects and garbage collection.

**How it works:**

- The binary format keeps column values in a cache-friendly layout instead of a graph of Java objects per field.
- Managed memory can sit off the Java heap, which avoids scanning those bytes during garbage collection. Execution memory and storage memory share one pool and can borrow from each other.
- Whole-stage code generation fuses several operators into one Java method, which removes virtual calls and intermediate rows between them.
- Encoders convert between JVM objects and that binary form at the boundary. Operators in the middle can work on bytes without serialising a row for every step.

**Examples:**

A cached DataFrame of `(name, age)` is stored as Tungsten rows. A later `filter` and `count` can run as generated code over that layout instead of allocating a Java object per field per row.
</details>

### Spark Programming and APIs

<details>
<summary>Briefly describe the Spark Core API and its features.</summary>

### Briefly describe the Spark Core API and its features.

**Definition:**

Spark Core is the driver program plus resilient distributed datasets. An RDD is a partitioned collection you transform in parallel, and it can be recomputed if a node is lost.

**How it works:**

- Every application has a driver that runs main and launches parallel operations. You build a SparkConf with an app name and a master, then a SparkContext. Only one SparkContext may be active per JVM. Call stop before creating another.
- On a cluster, do not hardcode master. spark-submit supplies it. local is for tests. In the shell, sc already exists. Creating another context fails.
- An RDD comes from a Hadoop-supported file, including HDFS, or from a Scala collection on the driver. Transformations describe a new RDD. Actions kick off computation.
- You can persist an RDD in memory so later actions reuse it. Shared variables are broadcast values, copied to every node, and accumulators, which tasks only add to.
- Scala applications depend on spark-core_2.13 at the Spark version you run. Spark 4.2.0 is built for Scala 2.13 by default. HDFS access also needs hadoop-client for your HDFS version. Python uses pyspark and Python 3.10 or newer for that release.

**Examples:**

```scala
val conf = new SparkConf().setAppName(appName).setMaster(master)
val sc = new SparkContext(conf)
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>What are the transformations and actions in Spark RDDs?</summary>

### What are the transformations and actions in Spark RDDs?

**Definition:**

A transformation builds a new RDD and stays lazy. An action runs the computation and returns a value to the driver, or writes data out.

**How it works:**

- `map` is the guide's example of a transformation: each element goes through a function and the result is a new RDD. `reduce` is the example of an action: it aggregates and returns the final value to the driver.
- Nothing runs at the `map`. Spark remembers the lineage. The `reduce` is what splits the work into tasks. Each machine can map its partition and reduce locally, and only that answer goes back to the driver.
- The same lazy RDD is recomputed for every later action unless you `persist` or `cache` it.
- Other transformations in the guide include `filter`, `flatMap`, `reduceByKey`, `join`, `repartition`, and `coalesce`. Other actions include `collect`, `count`, `take`, `takeOrdered`, `saveAsTextFile`, and `foreach`.
- `collect` pulls the whole RDD to the driver and can exhaust its memory. `take` is the safer way to look at a few rows. `foreach` that mutates a driver variable is undefined on a cluster. Use an accumulator.

**Examples:**

```python
lines = sc.textFile("data.txt")
lineLengths = lines.map(lambda s: len(s))
totalLength = lineLengths.reduce(lambda a, b: a + b)
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>How do you handle partitioning in Spark to optimize performance?</summary>

### How do you handle partitioning in Spark to optimize performance?

**Definition:**

A task runs on one partition. You change the number of partitions only when the shuffle or the filter has made the current split wasteful.

**How it works:**

- `repartition(numPartitions)` reshuffles every row at random to make more or fewer partitions. The guide says this always shuffles all data over the network.
- `coalesce(numPartitions)` decreases the number of partitions. The guide points it at the case after you have filtered a large dataset down.
- `repartitionAndSortWithinPartitions(partitioner)` repartitions and sorts inside each partition in one shuffle. That is cheaper than repartitioning and then sorting.
- Key aggregations take an optional `numPartitions`: `reduceByKey`, `aggregateByKey`, `groupByKey`, `join`. That number is how many reduce tasks the shuffle will run.
- A shuffle writes intermediate files and can spill sort tables to disk. Fewer, fuller partitions after a filter avoid tasks that have nothing to do. More partitions only help when one partition is the bottleneck.

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Illustrate the differences between map and flatMap functions in Spark.</summary>

### Illustrate the differences between map and flatMap functions in Spark.

**Definition:**

`map` turns each input element into one output element. `flatMap` turns each input element into zero or more output elements, and the results are flattened into one RDD.

**How it works:**

- The RDD guide defines `map(func)` as a new dataset formed by passing each element through `func`.
- `flatMap(func)` is similar, but `func` returns a sequence rather than a single item, so one input can become many outputs or none.
- A line of text mapped to its length stays one row. The same line flat-mapped by splitting on spaces becomes one row per word.
- Both are lazy transformations. Neither returns a value to the driver. An action such as `count` or `reduce` is what runs them.
- Structured Streaming uses the same idea on a socket line: `flatMap` (Scala) or `split` plus `explode` (Python) turns one line into word rows before `groupBy` and `count`.

**Examples:**

```python
lengths = lines.map(lambda s: len(s))          # one int per line
words = lines.flatMap(lambda s: s.split(" "))  # zero or more words per line
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/streaming/getting-started.html
</details>

<details>
<summary>What are Key-Value pair RDDs, and when would you use them?</summary>

### What are Key-Value pair RDDs, and when would you use them?

**Definition:**

A key-value RDD is an RDD of pairs. The pair operations are the ones that group or aggregate by key, and those are the shuffle operations.

**How it works:**

- In Python the pairs are tuples such as `(1, 2)`. In Scala they are `Tuple2`. In Java you build them with `mapToPair` into a `JavaPairRDD`.
- The guide's count is `map` to `(line, 1)` and then `reduceByKey`. `sortByKey` orders those pairs. `collect` brings the result to the driver.
- Use pairs when the question is per key: a count, a join, a cogroup. `groupByKey` returns `(K, Iterable)`. For a sum or average the guide says `reduceByKey` or `aggregateByKey` performs better.
- A custom key needs `equals` and a matching `hashCode`.
- Do not use a pair shuffle to mutate a counter in the closure. That counter is a copy on the executor.

**When to use:**

Use pair RDDs when you aggregate, sort, or join by a key. Keep ordinary RDDs when each row is independent.

**Examples:**

```python
pairs = lines.map(lambda s: (s, 1))
counts = pairs.reduceByKey(lambda a, b: a + b)
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Explain how to perform a join operation in Spark.</summary>

### Explain how to perform a join operation in Spark.

**Definition:**

On RDDs of pairs, `join` returns `(K, (V, W))` for every pair of values that share a key. It is a shuffle.

**How it works:**

- The guide defines `join(otherDataset, [numPartitions])` on datasets of type `(K, V)` and `(K, W)`. Outer forms are `leftOuterJoin`, `rightOuterJoin`, and `fullOuterJoin`.
- `cogroup` (also called `groupWith`) returns `(K, (Iterable, Iterable))` instead of one pair per match.
- Optional `numPartitions` sets the number of reduce tasks. Keys must meet, so this copies data across the network. The shuffle section lists `join` and `cogroup` among the operations that shuffle.
- A custom key still needs matching `equals` and `hashCode`, or equal keys will miss each other.
- `cartesian` is the other pair builder and it returns every pair of elements, not pairs that share a key. That is not a join.

**Examples:**

```python
joined = left.join(right)  # (K, V) join (K, W) -> (K, (V, W))
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Detail how window functions work in Spark SQL.</summary>

### Detail how window functions work in Spark SQL.

**Definition:**

Spark SQL's getting-started page separates a scalar function, one value per row, from an aggregate function, one value per group. Structured Streaming's window is that group defined on event time.

**How it works:**

- Built-in aggregates include `count()`, `count_distinct()`, `avg()`, `max()`, and `min()`. A user can also define an aggregate. The documented DataFrame form is `groupBy` then `count`, as in `df.groupBy("age").count()`.
- In Structured Streaming, an event-time window is a group on the event-time column. `window(timestamp, "10 minutes", "5 minutes")` is the call in the watermark example, used inside `groupBy` together with another column, then `count()`.
- A row can belong to more than one window. The getting-started streaming page says each time window is a group.
- The result is still a table Spark updates. Output mode decides whether you write the whole result (`complete`), only new rows (`append`), or only changed rows (`update`).
- This is not a per-row scalar. A scalar returns one value for that row. The window aggregate returns one value for the group.

**Sources:**

- https://spark.apache.org/docs/latest/sql-getting-started.html
- https://spark.apache.org/docs/latest/streaming/getting-started.html
- https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.withWatermark.html
</details>

### Spark Streaming and Real-Time Processing

<details>
<summary>Describe the concept of discretized streams (DStreams) in Spark.</summary>

### Describe the concept of discretized streams (DStreams) in Spark.

**Definition:**

Spark 4.2's streaming documentation specifies Structured Streaming: the live stream is an unbounded table, and the engine runs the query incrementally. That is the model these pages define.

**How it works:**

- Every arriving item is a new row on the input table. On each trigger the engine updates a result table. It does not keep the whole input. It keeps the intermediate state the result needs, such as running counts.
- By default the engine is micro-batch: small batch jobs, with end-to-end latency the overview puts as low as 100 milliseconds, and exactly-once when the source can replay offsets and the sink is idempotent. Checkpointing and write-ahead logs record the offset range of each trigger.
- Since Spark 2.3 there is also continuous processing, documented as latencies as low as 1 millisecond with at-least-once guarantees. The Dataset operations stay the same. The mode changes.
- The getting-started page contrasts this with engines where you maintain the running aggregation yourself and have to reason about at-least-once or at-most-once. Here Spark updates the result table.
- A socket word count is the concrete shape: `readStream.format("socket")`, split the line, `groupBy` and `count`, then `writeStream.outputMode("complete")`.

**Sources:**

- https://spark.apache.org/docs/latest/streaming/
- https://spark.apache.org/docs/latest/streaming/getting-started.html
</details>

<details>
<summary>How does Structured Streaming differ from DStream-based streaming?</summary>

### How does Structured Streaming differ from DStream-based streaming?

**Definition:**

Structured Streaming runs a batch-shaped query on an unbounded table, on the Spark SQL engine, and Spark keeps the result up to date. You write `groupBy` and `count`, not a hand-maintained running total.

**How it works:**

- The overview says you express the streaming computation the same way as a batch computation. The SQL engine runs it incrementally. Languages are the Dataset and DataFrame APIs.
- Default execution is micro-batch with exactly-once when sources have offsets and sinks are idempotent. Continuous processing, since 2.3, is the low-latency mode and is documented as at-least-once. You do not rewrite the query to switch.
- Late data is an event-time column plus a watermark, not a separate stream type. The engine can update an old aggregate and can drop state once the watermark passes the window.
- Output is a mode on the result table: complete, append, or update. Append applies only when existing result rows are not expected to change.
- Fault tolerance is checkpointing and write-ahead logs of the offset range, plus replayable sources and idempotent sinks. The user does not store the aggregate in an external variable.

**Sources:**

- https://spark.apache.org/docs/latest/streaming/
- https://spark.apache.org/docs/latest/streaming/getting-started.html
- https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html
</details>

<details>
<summary>What are the fault-tolerance mechanisms in Spark Streaming?</summary>

### What are the fault-tolerance mechanisms in Spark Streaming?

**Definition:**

Structured Streaming records the offset range of each trigger in a checkpoint and a write-ahead log, then restarts from that progress. RDD partitions that are lost are recomputed from lineage.

**How it works:**

- Sources are assumed to have offsets, analogous to a Kafka offset. The engine records the range being processed. Sinks are designed to be idempotent so a replay does not double-apply. Together that is the documented end-to-end exactly-once path.
- Continuous processing is the exception the overview states: at-least-once, not exactly-once.
- Inside an RDD job, a lost partition of a persisted RDD is recomputed from the transformations that created it. Shuffle files are kept so the lineage does not have to rebuild them while those RDDs are still referenced.
- Dynamic allocation must not delete shuffle files when an executor goes away. The job-scheduling page requires an external shuffle service, shuffle tracking, or decommission that preserves shuffle blocks.
- A driver variable incremented inside `foreach` is not a fault-tolerant counter. The closure holds a copy. An accumulator is the guide's mechanism for a cluster-wide aggregate.

**Sources:**

- https://spark.apache.org/docs/latest/streaming/getting-started.html
- https://spark.apache.org/docs/latest/streaming/
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/job-scheduling.html
</details>

<details>
<summary>How do you handle late data and stateful processing in Spark Streaming?</summary>

### How do you handle late data and stateful processing in Spark Streaming?

**Definition:**

Event time is a column. A watermark is how far behind that column you still accept late rows. State for a window is kept until the watermark passes the end of the window.

**How it works:**

- The getting-started page defines event time as the time embedded in the data, not the time Spark receives the row. Window aggregation is grouping on that column. A row may fall in more than one window.
- `withWatermark(eventTime, delayThreshold)` tracks a point before which no more late data is assumed. The Python API computes the watermark from the maximum event time seen across partitions, minus the delay. Coordination cost means the watermark used is only guaranteed to be at least that delay behind.
- For a window ending at `T`, the API page keeps state and allows late updates until max event time minus the late threshold is past `T`. Rows later than the threshold start getting dropped. Rows inside the threshold still update the aggregate.
- That bound is what lets a query run for days without keeping every old count. Update mode keeps updating a window until it is older than the watermark.
- Output mode still matters. Append can emit a window once it is finalized. Complete rewrites the result table.

**Sources:**

- https://spark.apache.org/docs/latest/streaming/getting-started.html
- https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html
- https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.withWatermark.html
</details>

<details>
<summary>Explain watermarks and windowing operations in Structured Streaming.</summary>

### Explain watermarks and windowing operations in Structured Streaming.

**Definition:**

A window groups rows by an event-time interval. A watermark is the lateness threshold on that event-time column so old window state can be dropped.

**How it works:**

- `withWatermark("timestamp", "10 minutes")` is the documented call. Then `groupBy(window(timestamp, "10 minutes", "5 minutes"), value).count()`.
- The watermark value is max event time across partitions minus the delay threshold. It can sit further behind than the threshold. Some records later than the threshold may still be processed. The API says the guarantee is at least the delay, not a hard cutoff in every case.
- The API page's rule for a window ending at `T`: keep the state while `max event time - late threshold` is not yet greater than `T`. After that, later rows for that window start to be dropped.
- Purposes named by the API: know when a window aggregation can be finalized for output modes that do not allow updates, and limit state for ongoing aggregations.
- This exists only for Structured Streaming, as the Python API note says.

**Examples:**

```python
from pyspark.sql.functions import window
query = (df
    .withWatermark("timestamp", "10 minutes")
    .groupBy(window(df.timestamp, "10 minutes", "5 minutes"), df.value)
    .count())
```

**Sources:**

- https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.withWatermark.html
- https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html
- https://spark.apache.org/docs/latest/streaming/getting-started.html
</details>

### Performance Tuning and Optimization

<details>
<summary>What are some common Spark performance issues and how do you resolve them?</summary>

### What are some common Spark performance issues and how do you resolve them?

**Definition:**

The expensive part is the shuffle, a hot partition, and recomputing an RDD you already paid for. The guide names those costs and the calls that avoid them.

**How it works:**

- A shuffle does disk I/O, serialization, and network I/O. `reduceByKey` and `aggregateByKey` build tables on the map side. Other by-key operations build them on the reduce side. If they do not fit, Spark spills to disk and spends more on I/O and garbage collection.
- `groupByKey` followed by a sum moves every value. `reduceByKey` aggregates first. The transformation table says so.
- `repartition` always shuffles everything. After a filter, `coalesce` reduces partitions without that full reshuffle.
- An RDD used twice and not persisted is computed twice. `persist` keeps the partitions. If they fit, `MEMORY_ONLY` is the most CPU-efficient level.
- `collect` of a large RDD exhausts the driver. `take` does not. A closure that updates a driver counter does not aggregate across executors.

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>How can you minimize data shuffling in Spark?</summary>

### How can you minimize data shuffling in Spark?

**Definition:**

Shuffle only when keys must meet. Aggregate before the shuffle, and do not repartition a dataset that is already split well.

**How it works:**

- `reduceByKey` and `aggregateByKey` combine values on the map side. `groupByKey` does not. The guide tells you to prefer the first two for a sum or similar aggregate.
- `join` and `cogroup` shuffle because matches must sit together. Pass `numPartitions` when you need a different number of reduce tasks, not by reflex.
- `coalesce` drops partitions after a filter. `repartition` is the call that always moves every row.
- `repartitionAndSortWithinPartitions` folds the sort into the shuffle you were going to do anyway.
- Narrow steps such as `map` and `filter` do not need a shuffle. They run inside the partition the action already scheduled.

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Discuss the importance and methods of caching/persistence in Spark.</summary>

### Discuss the importance and methods of caching/persistence in Spark.

**Definition:**

Persistence keeps partitions in memory across actions so the next action does not recompute them. The guide says later actions are often more than ten times faster.

**How it works:**

- `cache()` is `MEMORY_ONLY`: deserialized Java objects. Partitions that do not fit are recomputed. `persist(StorageLevel)` selects another level.
- `MEMORY_AND_DISK` stores the overflow on disk. `MEMORY_ONLY_SER` stores one serialized byte array per partition in Java and Scala, which is smaller and more CPU to read. Python always uses pickle, so the serialized level is not a different format there.
- Levels ending in `_2` replicate each partition on two nodes. Every level can recompute a lost partition. Replication lets tasks continue without waiting.
- The guide says stay on `MEMORY_ONLY` when the data fits. Do not spill to disk unless recomputing is expensive or the function filtered a lot.
- Spark also persists some shuffle data, such as for `reduceByKey`, even without `persist`. Call `persist` yourself if you reuse the result. LRU drops old partitions. `unpersist()` drops one RDD. `blocking=true` waits until the memory is freed.

**Examples:**

```scala
lineLengths.persist()
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Explain how you would monitor and log a Spark application.</summary>

### Explain how you would monitor and log a Spark application.

**Definition:**

The driver sees the result of an action. Executor prints do not show up on the driver. A cluster-wide number has to be an accumulator, not a closed-over variable.

**How it works:**

- `rdd.foreach(println)` writes to the executor's stdout in cluster mode. The driver's stdout stays empty. `take(n)` or a small `collect` brings rows back, and `collect` of everything can exhaust the driver.
- A counter incremented inside `foreach` stays zero on the driver because each executor got a copy. The guide's replacement is an accumulator.
- Inside one application, jobs are actions. The scheduler runs them FIFO unless `spark.scheduler.mode` is `FAIR`. Pools are set per thread with `spark.scheduler.pool`, which is how concurrent requests share the same SparkContext.
- PySpark does not line up Python threads with JVM threads by default, so `setJobGroup` in another Python thread is not reliable. The page recommends `pyspark.InheritableThread`.
- Structured Streaming's handle is the `StreamingQuery` from `writeStream.start()`. `awaitTermination()` is what keeps the process alive. The console sink prints each batch, which is the example's log.

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/job-scheduling.html
- https://spark.apache.org/docs/latest/streaming/getting-started.html
</details>

<details>
<summary>What is the role of partitioner objects in Spark and how do they affect performance?</summary>

### What is the role of partitioner objects in Spark and how do they affect performance?

**Definition:**

A partitioner decides which key lands in which partition. `repartitionAndSortWithinPartitions` takes one so the shuffle and the in-partition sort happen together.

**How it works:**

- The transformation table's `repartitionAndSortWithinPartitions(partitioner)` repartitions by that partitioner and sorts records by key inside each resulting partition. The guide says this is more efficient than `repartition` plus a later sort, because the sort can go into the shuffle.
- `reduceByKey`, `groupByKey`, and `join` take `numPartitions` instead of a partitioner object. That integer is still the number of reduce tasks, and the default parallelism follows the parent RDD.
- Keys are co-located only after the shuffle. Until then, values for one key can sit on different machines. A bad split leaves one reduce task with most of the rows. The spill and GC cost in the shuffle section is that task.
- Custom keys must implement `equals` and `hashCode` together. Otherwise the partition a key hashes to will not match the key you think is equal.
- `repartition` without a key reshuffles randomly. That balances size. It does not keep a key's rows together.

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

### Spark MLlib (Machine Learning Library)

<details>
<summary>What are the main features of Spark MLlib?</summary>

### What are the main features of Spark MLlib?

**Definition:**

MLlib is Spark's machine learning library. The primary API since Spark 2.0 is the DataFrame API in `spark.ml`. The RDD API in `spark.mllib` is in maintenance mode.

**How it works:**

- The guide lists algorithms for classification, regression, clustering, and collaborative filtering; featurization; pipelines for constructing, evaluating, and tuning; persistence of algorithms, models, and pipelines; and utilities for linear algebra and statistics.
- DataFrames are the reason for the switch: data sources, SQL, Tungsten and Catalyst, and one API across languages. Neither API is deprecated. New features go to DataFrames. The RDD API still gets bug fixes.
- Numerics go through Breeze and `dev.ludovic.netlib`. Those can call Intel MKL or OpenBLAS if they are installed. Spark does not ship those native libraries. Without them you get a warning and a pure JVM implementation. Python needs NumPy 1.21 or newer.
- Spark 3.0 highlights on that page include multi-column `StringIndexer` and `Binarizer`, sample weights on trees and k-means, `RobustScaler`, factorization machines, and two new evaluators.
- "Spark ML" is not an official name. People use it for the `org.apache.spark.ml` package.

**Sources:**

- https://spark.apache.org/docs/latest/ml-guide.html
</details>

<details>
<summary>How does Spark MLlib handle machine learning pipelines?</summary>

### How does Spark MLlib handle machine learning pipelines?

**Definition:**

A pipeline is an ordered list of stages. A transformer changes a DataFrame. An estimator fits a DataFrame and produces a transformer. `Pipeline.fit` runs them in order and returns a `PipelineModel`.

**How it works:**

- The example workflow is split text into words, hash those words into a feature vector, then learn a model. `Tokenizer` and `HashingTF` are transformers. `LogisticRegression` is an estimator.
- On fit, each transformer `transform`s the DataFrame. Each estimator `fit`s and the resulting model becomes a stage of the `PipelineModel`. On test data, `PipelineModel.transform` runs only transformers, so training and test share the same feature steps.
- Stages must be unique instances because each has an ID. A DAG is allowed if stages are listed in topological order. The graph is implied by input and output column names. Type checks happen at runtime from the DataFrame schema.
- Parameters are setters (`lr.setMaxIter(10)`) or a `ParamMap` passed to `fit` or `transform`. A `ParamMap` overrides setters.
- As of Spark 2.3 the DataFrame API can save and load pipelines across Scala, Java, and Python. Minor and patch versions are backwards compatible for loading. Major versions are best-effort. R uses a different format.

**Sources:**

- https://spark.apache.org/docs/latest/ml-pipeline.html
- https://spark.apache.org/docs/latest/ml-guide.html
</details>

<details>
<summary>Describe a use case for MLlib's collaborative filtering algorithms.</summary>

### Describe a use case for MLlib's collaborative filtering algorithms.

**Definition:**

MLlib lists collaborative filtering next to classification, regression, and clustering. The DataFrame API is how any of those algorithms see their training rows.

**How it works:**

- The dataset is a DataFrame. The pipeline guide says a DataFrame can hold labels, feature vectors, text, and predictions in named columns, plus ML `Vector` types.
- A filtering model is still an estimator if it trains: `fit` on the history DataFrame produces a transformer, and `transform` appends a prediction column. That is the same contract as `LogisticRegression`.
- Put the feature steps and the estimator in one `Pipeline` so the same columns are built at training and at serving. Save the `PipelineModel`. Minor Spark versions load it back.
- The RDD API in `spark.mllib` will not gain new algorithms. Use `spark.ml`.
- The pages fetched here name collaborative filtering as a provided algorithm family. They do not show a separate code sample for it. The pipeline contract above is the part they do specify.

**When to use:**

Use it when the training data is a history of known preferences in a DataFrame and the output you need is a prediction column from a saved pipeline.

**Sources:**

- https://spark.apache.org/docs/latest/ml-guide.html
- https://spark.apache.org/docs/latest/ml-pipeline.html
</details>

<details>
<summary>Explain the difference between Spark MLlib and external machine learning libraries.</summary>

### Explain the difference between Spark MLlib and external machine learning libraries.

**Definition:**

MLlib's primary API is DataFrame pipelines in `spark.ml`, executed by Spark. External numerical libraries are optional natives underneath that, not a second ML API.

**How it works:**

- `spark.ml` gives one set of estimators and transformers across languages, on DataFrames, with Catalyst and Tungsten under them. `spark.mllib` on RDDs stays in maintenance and gets bug fixes only.
- Breeze and `dev.ludovic.netlib` do the linear algebra. If Intel MKL or OpenBLAS is on the machine, those packages can call it. Spark does not distribute the native library. Otherwise the JVM implementation runs and logs the failed load of `JNIBLAS`.
- Python MLlib needs NumPy 1.21 or newer. That is a dependency of MLlib, not a replacement for the DataFrame estimators.
- A pipeline stage has to be an MLlib transformer or estimator if it is inside a `Pipeline`. `fit` and `transform` are stateless in the current API.
- Persistence is MLlib's own format. A model saved by another library is not what `PipelineModel` load is documented to read.

**Sources:**

- https://spark.apache.org/docs/latest/ml-guide.html
- https://spark.apache.org/docs/latest/ml-pipeline.html
</details>

### Coding Challenges

<details>
<summary>Write a Scala/Python Spark code snippet that reads a CSV file and calculates the average of a column.</summary>

### Write a Scala/Python Spark code snippet that reads a CSV file and calculates the average of a column.

**Definition:**

`spark.read.csv` loads a file or a directory of CSV files. `avg()` is a built-in aggregate that returns one value for a group of rows.

**How it works:**

- The default separator is a comma. `option("header", True)` uses the first line as column names. `option("delimiter", ";")` overrides the separator. `inferSchema` is false by default and costs an extra pass if you turn it on.
- A folder read expects only CSV files. The CSV page shows a wrong schema when other files are in the same directory.
- After the DataFrame exists, register a temp view and run SQL, or use the DataFrame aggregate. The getting-started page lists `avg()` with `count()`, `max()`, and `min()`.
- `header` false leaves columns named `_c0`, `_c1`. Average the column you actually named.
- Writing uses `dataframe.write.csv`. That is a folder of part files plus `_SUCCESS`, not a single CSV document.

**Examples:**

```python
df = spark.read.option("header", True).option("inferSchema", True).csv(path)
df.createOrReplaceTempView("people")
spark.sql("SELECT avg(age) AS avg_age FROM people")
```

**Sources:**

- https://spark.apache.org/docs/latest/sql-data-sources-csv.html
- https://spark.apache.org/docs/latest/sql-getting-started.html
</details>

<details>
<summary>Implement a Spark streaming application that counts words in text data received from a socket.</summary>

### Implement a Spark streaming application that counts words in text data received from a socket.

**Definition:**

The Structured Streaming quick example reads a TCP socket, splits lines into words, and writes a running count in complete mode.

**How it works:**

- `spark.readStream.format("socket").option("host", "localhost").option("port", 9999)` is the input table. The column is `value`. Nothing is read until `start()`.
- Python splits with `split` and `explode`. Scala uses `flatMap` on a `Dataset[String]`. Then `groupBy` and `count` are the result table.
- `writeStream.outputMode("complete").format("console").start()` prints the whole count table on each trigger. `awaitTermination()` keeps the process up.
- The example server is `nc -lk 9999`. The Python submit path in the guide is `./bin/spark-submit examples/src/main/python/sql/streaming/structured_network_wordcount.py localhost 9999`.
- Complete mode rewrites the result. The engine keeps the counts, not every source line.

**Examples:**

```python
lines = spark.readStream.format("socket").option("host", "localhost").option("port", 9999).load()
words = lines.select(explode(split(lines.value, " ")).alias("word"))
query = words.groupBy("word").count().writeStream.outputMode("complete").format("console").start()
```

**Sources:**

- https://spark.apache.org/docs/latest/streaming/getting-started.html
</details>

<details>
<summary>Code a Spark SQL query that finds the top 3 most occurring words in a DataFrame.</summary>

### Code a Spark SQL query that finds the top 3 most occurring words in a DataFrame.

**Definition:**

Count is a group aggregate. `takeOrdered` on the pair RDD returns the first n elements under an ordering, which is the documented way to keep only the top of a small result.

**How it works:**

- Build word pairs the RDD guide's way, or `groupBy("word").count()` the DataFrame way. Both produce a count per word.
- `collect` of every count can exhaust the driver. `takeOrdered(3, ordering)` returns three elements. The action table defines it as the first n by natural order or a custom comparator.
- A SQL temporary view is `createOrReplaceTempView`, then `spark.sql`. The getting-started page runs `SELECT` that way. The aggregate page lists `count()`.
- Do the heavy count in the cluster. Bring back three rows, not the vocabulary.
- Streaming complete-mode counts are a result table, not a top-3 by themselves. You still have to order that table and keep three.

**Examples:**

```python
counts = words.map(lambda w: (w, 1)).reduceByKey(lambda a, b: a + b)
top3 = counts.takeOrdered(3, key=lambda kv: -kv[1])
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/sql-getting-started.html
</details>

<details>
<summary>Create a Spark job that joins two datasets on a key and summarizes data.</summary>

### Create a Spark job that joins two datasets on a key and summarizes data.

**Definition:**

Make both sides pair RDDs, `join` them, then `reduceByKey` so the summary is combined before the full value lists move.

**How it works:**

- `join` on `(K, V)` and `(K, W)` yields `(K, (V, W))` for every match. `numPartitions` sets the reduce tasks for that shuffle.
- A sum after the join should not `groupByKey` the joined rows. Map to `(key, number)` and `reduceByKey`.
- Outer joins exist when a missing side must still appear: `leftOuterJoin`, `rightOuterJoin`, `fullOuterJoin`.
- Persist the side you join more than once. Otherwise each action recomputes it.
- DataFrame `groupBy` plus `count` is the SQL form of the summary once the join result is a DataFrame. The RDD join itself is specified on pair RDDs.

**Examples:**

```python
joined = orders.join(customers)              # (id, (order, customer))
totals = joined.map(lambda kv: (kv[0], kv[1][0])).reduceByKey(lambda a, b: a + b)
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/sql-getting-started.html
</details>

<details>
<summary>Write a Python function using PySpark to filter out records with null values in a specific column.</summary>

### Write a Python function using PySpark to filter out records with null values in a specific column.

**Definition:**

A null is a value the DataFrame keeps. The people example shows `age` null in `show()` and in `groupBy("age").count()`. Filtering that column out of the result is a `WHERE` on the temp view.

**How it works:**

- `spark.read.json` and `spark.read.csv` both produce a DataFrame. With `header` false, CSV columns are `_c0` and so on. Name the column before you filter it.
- `createOrReplaceTempView("people")` then `spark.sql` is the documented way to run a query. `WHERE age IS NOT NULL` drops the null group the `groupBy` example otherwise counts.
- `df.filter(df["age"] > 21)` is the documented DataFrame filter. A comparison does not describe null removal by itself. The SQL predicate does.
- `inferSchema` on CSV is an extra pass and is off by default. A string column will not compare as a number until the schema says it is numeric.
- Do not `collect` and drop nulls on the driver. The filter belongs in the query.

**Examples:**

```python
def without_null_age(df):
    df.createOrReplaceTempView("people")
    return spark.sql("SELECT * FROM people WHERE age IS NOT NULL")
```

**Sources:**

- https://spark.apache.org/docs/latest/sql-getting-started.html
- https://spark.apache.org/docs/latest/sql-data-sources-csv.html
</details>

<details>
<summary>Implement a logistic regression model in Spark MLlib.</summary>

### Implement a logistic regression model in Spark MLlib.

**Definition:**

`LogisticRegression` is an estimator. `fit` on a DataFrame of `label` and `features` returns a model, and that model's `transform` appends the prediction.

**How it works:**

- The pipeline guide constructs it as `LogisticRegression(maxIter=10, regParam=0.01)` and calls `lr.fit(training)`.
- Training rows in the example are `(label, Vectors.dense(...))` with column names `label` and `features`. `transform` reads `features`.
- A `ParamMap` can override setters, including `probabilityCol`. The example renames that column to `myProbability` and then selects `features`, `label`, `myProbability`, and `prediction`.
- In a pipeline, logistic regression is the estimator stage after `Tokenizer` and `HashingTF`. `Pipeline.fit` produces a `PipelineModel` whose stages are all transformers.
- This class lives in `pyspark.ml.classification`. The RDD package `spark.mllib` is maintenance mode.

**Examples:**

```python
from pyspark.ml.classification import LogisticRegression
lr = LogisticRegression(maxIter=10, regParam=0.01)
model = lr.fit(training)  # columns: label, features
prediction = model.transform(test)
```

**Sources:**

- https://spark.apache.org/docs/latest/ml-pipeline.html
- https://spark.apache.org/docs/latest/ml-guide.html
</details>

<details>
<summary>Create a Spark Streaming application to aggregate event data by time windows.</summary>

### Create a Spark Streaming application to aggregate event data by time windows.

**Definition:**

Group the stream by an event-time window, and set a watermark so the window state does not grow without a bound.

**How it works:**

- Event time is a column on each row. `window(df.timestamp, "10 minutes", "5 minutes")` inside `groupBy`, plus another column, then `count()`, is the example.
- `withWatermark("timestamp", "10 minutes")` must be declared so the engine can drop state for windows older than the lateness threshold.
- Pick an output mode the aggregate allows. Complete writes the whole result table. Update writes rows that changed. Append waits until existing rows are not expected to change, which is what finalizing a window is for.
- The query does not run until `writeStream.start()`. Checkpointing is what makes a restart continue from the recorded offsets.
- A processing-time receipt is not this column. The getting-started page says to use the time in the data when the question is events per minute.

**Examples:**

```python
from pyspark.sql.functions import window
counts = (events
    .withWatermark("timestamp", "10 minutes")
    .groupBy(window("timestamp", "10 minutes", "5 minutes"))
    .count())
```

**Sources:**

- https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.withWatermark.html
- https://spark.apache.org/docs/latest/streaming/getting-started.html
- https://spark.apache.org/docs/latest/streaming/apis-on-dataframes-and-datasets.html
</details>

<details>
<summary>Write Spark code to calculate the PageRank of a simple website link graph.</summary>

### Write Spark code to calculate the PageRank of a simple website link graph.

**Definition:**

A link graph in the RDD API is pairs. `join` lines up a page's rank with its outbound links, and `reduceByKey` sums what comes back to each target. Those are the operations the guide actually defines for this shape.

**How it works:**

- Represent a link as `(source, destination)` and a rank as `(page, rank)`. Both are key-value RDDs.
- `join` produces `(page, (rank, destination))` when the key is the source page. Map that to a contribution aimed at the destination, then `reduceByKey` to add contributions for each target.
- `groupByKey` would ship every contribution. `reduceByKey` combines them first. The guide prefers that for a sum.
- Each iteration is an action or a later use of the ranks. `persist` the link RDD. It does not change. Without persist, every action recomputes it.
- `collect` the ranks only when the page set is small enough for the driver. Otherwise `saveAsTextFile`.

**Examples:**

```python
joined = ranks.join(links)  # (page, (rank, neighbors))
contribs = joined.flatMap(lambda kv: ((dest, kv[1][0] / len(kv[1][1])) for dest in kv[1][1]))
nextRanks = contribs.reduceByKey(lambda a, b: a + b)
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

### Case Studies and Scenario-Based Questions

<details>
<summary>How would you build a recommender system in Spark?</summary>

### How would you build a recommender system in Spark?

**Definition:**

MLlib names collaborative filtering as one of its algorithm families. The way you train and serve any of those algorithms in the primary API is a DataFrame pipeline.

**How it works:**

- Training rows live in a DataFrame with named columns. An estimator `fit`s that frame and returns a model. `transform` appends the prediction.
- Keep feature construction and the estimator in one `Pipeline` so serving runs the same stages. Save the `PipelineModel`. Loading is backwards compatible across minor and patch versions.
- The RDD API will not receive new recommenders. `spark.ml` is the primary package.
- If the history is large, the same persistence rules apply to any RDD you reuse while building that DataFrame: `persist` before repeated actions, and do not `collect` the full history to the driver.
- The fetched MLlib pages do not include a separate recommender code sample. The contract they do give is estimator, transformer, and pipeline.

**Sources:**

- https://spark.apache.org/docs/latest/ml-guide.html
- https://spark.apache.org/docs/latest/ml-pipeline.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Discuss a strategy to perform real-time sentiment analysis using Spark Streaming.</summary>

### Discuss a strategy to perform real-time sentiment analysis using Spark Streaming.

**Definition:**

Score each arriving row with a fitted MLlib model, and aggregate those scores as a Structured Streaming query. The model is a transformer. The stream is a table.

**How it works:**

- Train offline with `LogisticRegression` or a `Pipeline` of `Tokenizer`, `HashingTF`, and logistic regression. `PipelineModel.transform` appends the prediction column. `fit` and `transform` are stateless in the current pipeline API.
- On the stream, the input is `readStream`. Apply the same fitted pipeline's `transform`, then `groupBy` the prediction and `count`, or window that count with `withWatermark` if you need event time.
- `outputMode("complete")` prints the running table, as in the socket word-count example. Checkpoint the query so a restart does not reprocess from an unknown offset.
- Micro-batch is the default and is the exactly-once path when the source has offsets and the sink is idempotent. Continuous processing is at-least-once.
- Do not train inside `foreach` by mutating a driver model. That closure is a copy on the executors.

**Sources:**

- https://spark.apache.org/docs/latest/ml-pipeline.html
- https://spark.apache.org/docs/latest/streaming/getting-started.html
- https://spark.apache.org/docs/latest/streaming/
- https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.withWatermark.html
</details>

<details>
<summary>Propose an ETL pipeline design using Spark to process large datasets.</summary>

### Propose an ETL pipeline design using Spark to process large datasets.

**Definition:**

Read a data source into a DataFrame, transform it with lazy DataFrame or RDD operations, and write it out. Actions are the steps that actually run.

**How it works:**

- CSV is `spark.read.option("header", True).csv(path)`. JSON is `spark.read.json`. A text file can stay an RDD via `textFile` until you need columns.
- Filters and maps are transformations. `filter`, `groupBy().count()`, and SQL through a temp view are the structured forms. `avg()` and `count()` are the built-in aggregates.
- Write with `dataframe.write.csv`. That produces a directory of part files. The CSV guide warns that reading a directory with non-CSV files infers a wrong schema.
- Persist only the DataFrame or RDD that two later actions both need. A shuffle such as `join` or `reduceByKey` is the expensive hop. Aggregate before it.
- A streaming ingest is the same query with `readStream` and `writeStream`, plus a checkpoint. Do not use the driver as the place rows are assembled.

**Sources:**

- https://spark.apache.org/docs/latest/sql-data-sources-csv.html
- https://spark.apache.org/docs/latest/sql-getting-started.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/streaming/getting-started.html
</details>

<details>
<summary>How would you use Spark to detect fraudulent behavior in financial transactions?</summary>

### How would you use Spark to detect fraudulent behavior in financial transactions?

**Definition:**

Fraud labeling in this API is a classification DataFrame: a label column, a features column, and `LogisticRegression` or a pipeline that produces that column. Streaming detection is `transform` on each micro-batch.

**How it works:**

- The estimator example trains on `label` and `features` and `transform` adds `prediction`. You choose the label from historical cases. The library does not define a fraud rule.
- A pipeline keeps the same feature stages on history and on live rows. Save the `PipelineModel` and load it for the job that scores.
- Live rows are a Structured Streaming query. Window counts of events, with a watermark, are how the engine bounds state if the signal is "too many events in ten minutes".
- Join the transaction to an account dimension with pair-RDD `join` or keep both as DataFrames before `transform`. Persist the dimension side if every batch uses it.
- Exactly-once scoring depends on a replayable source, checkpoints, and an idempotent sink. A printed console is the example sink, not a ledger.

**Sources:**

- https://spark.apache.org/docs/latest/ml-pipeline.html
- https://spark.apache.org/docs/latest/streaming/getting-started.html
- https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.withWatermark.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>

<details>
<summary>Illustrate how you would use Spark to optimize marketing strategies based on customer behavior data.</summary>

### Illustrate how you would use Spark to optimize marketing strategies based on customer behavior data.

**Definition:**

Aggregate behavior by customer key, then fit a classifier or a clustering-family algorithm on that DataFrame. MLlib's listed families are classification, regression, clustering, and collaborative filtering.

**How it works:**

- `reduceByKey` counts events per customer without shipping every event. `groupBy` and `count` are the DataFrame form. `avg()` summarizes a numeric column.
- The feature vector is a column of `Vectors.dense` or the output of a transformer such as `HashingTF`. `LogisticRegression.fit` learns a label such as "converted".
- Collaborative filtering is the listed family when the data is preference history rather than a single label. It still trains through the DataFrame API.
- Persist the aggregated customer RDD if several models read it. Do not collect every customer to the driver to sort them. `takeOrdered` keeps the top of a ranking.
- A campaign file is `write.csv` of the scored DataFrame, a directory of part files.

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/sql-getting-started.html
- https://spark.apache.org/docs/latest/ml-guide.html
- https://spark.apache.org/docs/latest/ml-pipeline.html
- https://spark.apache.org/docs/latest/sql-data-sources-csv.html
</details>

<details>
<summary>Explain a big data analytics project where Spark would be a better choice than other big data technologies and why.</summary>

### Explain a big data analytics project where Spark would be a better choice than other big data technologies and why.

**Definition:**

Spark fits when the same in-memory dataset is reused by several actions, which is the case the persistence section calls iterative algorithms and interactive use.

**How it works:**

- A transformation is free until an action. The engine can pipeline a `map` into a `reduce` and return only the aggregate, instead of writing the mapped dataset out.
- `persist` keeps partitions for the next action. The guide's figure is often more than a tenfold speedup. Iterative updates, such as the rank loop built from `join` and `reduceByKey`, read the links on every iteration.
- SQL and DataFrames add `groupBy`, `avg()`, and `count()` on the same engine, and MLlib pipelines consume those DataFrames. One cluster job can aggregate and then `fit`.
- Structured Streaming runs the same query continuously, with checkpoints, instead of a separate engine whose user maintains the count.
- A single map-only write of a huge file does not need the cache. The win is reuse and the SQL and ML APIs on that reused data.

**When to use:**

Prefer Spark when several passes, SQL, or MLlib read the same dataset. A one-pass write does not use the cache.

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/sql-getting-started.html
- https://spark.apache.org/docs/latest/ml-guide.html
- https://spark.apache.org/docs/latest/streaming/
</details>

### Advanced Topics and Research

<details>
<summary>Discuss the advancements in Spark 3.x and their impact on big data processing.</summary>

### Discuss the advancements in Spark 3.x and their impact on big data processing.

**Definition:**

The MLlib guide's Spark 3.0 list is concrete: more estimators accept sample weights and multiple columns, and the DataFrame API stays the one that receives features.

**How it works:**

- Sample weights landed on decision trees, random forests, gradient-boosted trees, k-means, bisecting k-means, Gaussian mixture, and the classification and regression evaluators.
- `StringIndexer`, `Binarizer`, `StopWordsRemover`, and PySpark `QuantileDiscretizer` gained multi-column support. `RobustScaler`, factorization machines, and Gaussian and complement naive Bayes were added.
- `predictRaw` is public on classification models. `predictProbability` is public except on `LinearSVCModel`. Python and Scala ML functions were brought to parity.
- The RDD API did not receive these. It remains maintenance mode, which is the 2.0 decision the same page still states.
- Dynamic allocation is older than 3.0 and still the way a shared cluster returns idle executors. Spark 3.0's `spark.shuffle.service.fetch.rdd.enabled` lets disk-persisted RDD blocks be fetched from the external shuffle service so those executors can be considered idle.

**Sources:**

- https://spark.apache.org/docs/latest/ml-guide.html
- https://spark.apache.org/docs/latest/job-scheduling.html
</details>

<details>
<summary>Explain how Dynamic Resource Allocation works in Spark.</summary>

### Explain how Dynamic Resource Allocation works in Spark.

**Definition:**

With dynamic allocation, an application gives executors back when they are idle and requests more when tasks are waiting. It is off by default.

**How it works:**

- Set `spark.dynamicAllocation.enabled` to true. Also enable an external shuffle service (`spark.shuffle.service.enabled`), or shuffle tracking, or decommission that preserves shuffle blocks, or a shuffle IO plugin that stores shuffle reliably. Otherwise removed executors take their shuffle files with them.
- It is available on standalone, YARN, and Kubernetes. Kubernetes cannot use the external shuffle service. Standalone should set `spark.executor.cores`, or each executor may take every core on the worker.
- Requests start after `spark.dynamicAllocation.schedulerBacklogTimeout` seconds of pending tasks, then repeat every `spark.dynamicAllocation.sustainedSchedulerBacklogTimeout`. Each round asks for exponentially more executors: 1, then 2, 4, 8.
- An executor idle longer than `spark.dynamicAllocation.executorIdleTimeout` is removed. Executors holding cached data are kept unless `spark.dynamicAllocation.cachedExecutorIdleTimeout` says otherwise.
- `killExecutors` on `SparkContext` is not supported while dynamic allocation is on.

**When to use:**

Turn it on when several applications share a cluster and this job's demand changes. Leave it off when you already reserved a fixed set of executors.

**Sources:**

- https://spark.apache.org/docs/latest/job-scheduling.html
</details>

<details>
<summary>How do you implement custom aggregations in Spark?</summary>

### How do you implement custom aggregations in Spark?

**Definition:**

On pair RDDs, `aggregateByKey` takes a zero value, a function that merges a value into the aggregate, and a function that merges aggregates. In SQL, built-in aggregates include `avg()` and `count()`, and you can define your own.

**How it works:**

- The RDD table: `aggregateByKey(zeroValue)(seqOp, combOp, [numPartitions])` on `(K, V)` returns `(K, U)`. `U` can differ from `V`. The zero value must be neutral for the combine.
- `reduceByKey` is the narrower case where the aggregate type stays `V` and the function is `(V, V) => V`. Use it for a sum. Use `aggregateByKey` when the aggregate is a different shape, such as a sum and a count.
- Both shuffle. They build the combine table on the map side, which is why they beat `groupByKey` for this job. `numPartitions` sets the reduce tasks.
- Spark SQL lists `count()`, `count_distinct()`, `avg()`, `max()`, and `min()`, and says users can create their own aggregate functions. The DataFrame call for the built-in count is `groupBy` then `count`.
- The zero value is shipped and used per partition. It must not be a shared mutable object you change in place.

**Examples:**

```python
# (sum, count) per key, then the caller divides
agg = pairs.aggregateByKey((0, 0))(
    lambda acc, v: (acc[0] + v, acc[1] + 1),
    lambda a, b: (a[0] + b[0], a[1] + b[1]))
```

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/sql-getting-started.html
</details>

<details>
<summary>What are the current research areas or challenges in the Apache Spark ecosystem?</summary>

### What are the current research areas or challenges in the Apache Spark ecosystem?

**Definition:**

The current guides still call out cost and guarantees you do not get for free: shuffle cost, exactly-once only on the micro-batch path, and an RDD ML API that no longer receives features.

**How it works:**

- Shuffle is disk, serialization, and network, plus spill when the in-memory tables do not fit. Long jobs keep shuffle files until the RDDs are unused. `spark.local.dir` is that temporary space.
- Structured Streaming's exactly-once path needs offsets, checkpoints, write-ahead logs, and idempotent sinks. Continuous processing is documented as at-least-once. Watermarks are guaranteed to lag by at least the delay, and a late row can still arrive.
- Dynamic allocation cannot see the future. It uses backlog timeouts and an idle timeout, and it must preserve shuffle files or a removed executor forces a recompute. Cached executors are kept by default.
- MLlib's RDD API is frozen at bug fixes. Native BLAS is optional and not shipped. Major-version ML persistence is best-effort.
- A driver is still a single place `collect` can run out of memory, and closures do not share mutable state. Those are the scaling edges the RDD guide states.

**Sources:**

- https://spark.apache.org/docs/latest/rdd-programming-guide.html
- https://spark.apache.org/docs/latest/streaming/
- https://spark.apache.org/docs/latest/streaming/getting-started.html
- https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.withWatermark.html
- https://spark.apache.org/docs/latest/job-scheduling.html
- https://spark.apache.org/docs/latest/ml-guide.html
- https://spark.apache.org/docs/latest/ml-pipeline.html
</details>

<details>
<summary>How does Spark support deep learning workloads and integration with popular deep learning frameworks?</summary>

### How does Spark support deep learning workloads and integration with popular deep learning frameworks?

**Definition:**

The MLlib guide does not specify a deep-learning framework integration. What it does specify is scalable DataFrame estimators and optional native linear algebra under those estimators.

**How it works:**

- The primary API is `spark.ml` on DataFrames: transformers, estimators, and pipelines. Classification, regression, clustering, and collaborative filtering are the listed families. `LogisticRegression` is the estimator the pipeline page shows.
- Numeric kernels go through Breeze and `dev.ludovic.netlib`. Intel MKL or OpenBLAS are used only if you install them. Spark logs a `JNIBLAS` warning and uses the JVM code otherwise. Python needs NumPy 1.21 or newer.
- Training data should stay a DataFrame. `collect` of the full set to one process is the opposite of the RDD guide's warning about driver memory.
- A saved `PipelineModel` is how a fitted MLlib workflow is reloaded. That format is MLlib persistence, with best-effort compatibility across major versions.
- If a workload needs a library this page does not name, it sits outside this contract. Do not assume an estimator name that the guide does not list.

**Sources:**

- https://spark.apache.org/docs/latest/ml-guide.html
- https://spark.apache.org/docs/latest/ml-pipeline.html
- https://spark.apache.org/docs/latest/rdd-programming-guide.html
</details>
