# Availability & Reliability — Interview

Answers are filled.

[← Back to hub](./0_system_design.md)

### Related docs

- [Availability & Reliability — Interview](./0_sd_availability_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Availability & Reliability](https://devinterview.io/questions/software-architecture-and-system-design/availability-and-reliability-interview-questions/).

### Availability & Reliability Fundamentals

<details>
<summary>What is the difference between availability and reliability in the context of a software system?</summary>

### What is the difference between availability and reliability in the context of a software system?

**Definition:**

Availability is the share of time a system can answer. Reliability is how likely it is to keep working correctly for a stretch without failing.

**How it works:**

- Availability is a percentage over a window: uptime divided by the window.
- Reliability is a probability of successful operation over an interval, often summarized by how long the system runs between failures.
- A system can be reliable and still look unavailable if repair takes a long time.
- A system can be patched back quickly, so availability stays high, while it still fails often.

**Examples:**

Down 15 minutes in an hour is 75% availability for that hour. Reliability asks a different question: how often those outages happen across months, not how long this one hour was dark.
</details>

<details>
<summary>How do you define system availability and what are the key components to measure it?</summary>

### How do you define system availability and what are the key components to measure it?

**Definition:**

System availability is the fraction of time the system is up and able to do its job. The usual estimate is mean time between failures divided by that time plus mean time to repair.

**How it works:**

- MTBF is the average time the system runs between failures.
- MTTR is the average time from noticing the failure to restoring service.
- Availability ≈ MTBF / (MTBF + MTTR). Raising MTBF or cutting MTTR both move the number up.
- Measure it over an agreed window, and count only the failures that users actually feel, or you will argue about the percentage.

**Examples:**

MTBF of 100 hours and MTTR of 2 hours gives 100 / 102 ≈ 98.0% availability. Cutting repair to 30 minutes raises it to about 99.5% without making the parts last longer.
</details>

<details>
<summary>Can you explain the concept of “Five Nines” and how it relates to system availability?</summary>

### Can you explain the concept of “Five Nines” and how it relates to system availability?

**Definition:**

Five nines means 99.999% availability: about 5.26 minutes of downtime in a year. It is a target for how little time the service may be dark, not a claim that nothing ever fails.

**How it works:**

- 90% is about 36.5 days down per year. 99% is about 3.65 days. 99.9% is about 8.8 hours.
- 99.99% is about 53 minutes. 99.999% is about 5.3 minutes. 99.9999% is about half a minute.
- Each extra nine costs redundancy, automation, and operational discipline, not a faster server.
- Redundant parts, more than one site, health checks, and automated failover are what make the target plausible.
- Planned maintenance counts unless you have a way to do it without stopping the service.
</details>

<details>
<summary>How does redundancy contribute to the reliability of a system?</summary>

### How does redundancy contribute to the reliability of a system?

**Definition:**

Redundancy keeps a spare ready so one failed part does not stop the service. Reliability goes up because the system can lose a component and continue.

**How it works:**

- Component redundancy mirrors a disk, a power supply, or a network path. RAID-1 keeps a second copy of the blocks.
- Subsystem redundancy duplicates a whole tier, such as two power feeds or two application pools.
- Information redundancy copies the data itself, with replication or erasure coding, so one copy’s loss is not data loss.
- Failover is the switch onto the spare. Load balancing is how traffic avoids the bad path.
- A quorum, such as two of three nodes, uses the extra copies to decide rather than to merely sit idle.

**Examples:**

Two disks in RAID-1: either disk can fail and reads still succeed from the other. One disk alone is a single point of failure.
</details>

<details>
<summary>What is a single point of failure (SPOF), and how can it be mitigated?</summary>

### What is a single point of failure (SPOF), and how can it be mitigated?

**Definition:**

A single point of failure is any part whose death takes the whole service down. Mitigation is a second path plus a way to switch to it.

**How it works:**

- A lone switch, a lone web server, or a lone power supply are the usual examples.
- Add a spare and a failover that moves traffic when health checks fail.
- Put more than one server behind a load balancer, and make the balancer itself a pair.
- Maintenance and a written recovery plan reduce the chance that the spare is stale or nobody knows how to use it.
- Practice the failover. A spare that has never been switched to is a hope, not a mitigation.

**Examples:**

One database primary with no replica: the primary’s disk dies and the site is down. A synchronous replica plus a promoted standby removes that single point.
</details>

<details>
<summary>Discuss the significance of Mean Time Between Failures (MTBF) in reliability engineering.</summary>

### Discuss the significance of Mean Time Between Failures (MTBF) in reliability engineering.

**Definition:**

MTBF is the average operating time between failures. A higher number means the component or system fails less often, which is the reliability side of the availability formula.

**How it works:**

- It is total operating time divided by the number of failures in that time.
- Planners use it to schedule replacement and to decide how much spare capacity to hold before the next failure is likely.
- The simple figure assumes a roughly constant failure rate. Wear-out and early-life failures do not follow that assumption.
- The number depends on load, temperature, and how the part is used. A datasheet MTBF is not your production MTBF.
- Drive vendors publish MTBF to estimate replacement. Designers use it to see whether one copy is enough or a pair is required.

**Examples:**

Two failures in 1,000 hours of operation is an MTBF of 500 hours. That does not say the next failure will land at hour 500. It says the long-run average gap is 500 hours.
</details>

<details>
<summary>What is the role of Mean Time to Repair (MTTR) in maintaining system availability?</summary>

### What is the role of Mean Time to Repair (MTTR) in maintaining system availability?

**Definition:**

MTTR is the average time from detecting a failure to restoring service. Availability rises when this interval shrinks, even if failures stay just as frequent.

**How it works:**

- The clock starts at detection, not at the moment someone starts typing a fix. Slow alerts inflate MTTR.
- It includes diagnosis, repair or failover, and the check that the service is actually back.
- Paired with MTBF: availability ≈ MTBF / (MTBF + MTTR).
- Automation, runbooks, and a warm spare cut MTTR more cheaply than making every part immortal.

**Examples:**

A database that fails twice a month (MTBF about 360 hours) with a 4-hour manual restore is near 99% available. The same failures with a 5-minute failover are near 99.98%.
</details>

<details>
<summary>Can you differentiate between high availability (HA) and fault tolerance (FT)?</summary>

### Can you differentiate between high availability (HA) and fault tolerance (FT)?

**Definition:**

High availability minimizes downtime with failover, and a short blip is acceptable. Fault tolerance keeps the service correct through a component failure, with no visible interruption.

**How it works:**

- HA detects a bad node and shifts traffic. Users may see a retry or a few seconds of errors.
- FT runs redundant parts so a failure is masked. The spare is already in step, not waiting to boot.
- Both use extra copies. HA optimizes recovery time. FT optimizes uninterrupted correctness.
- A load balancer pulling a dead web server is HA. A lockstep pair that continues a transaction when one CPU dies is FT.
- Data for HA is often replicated so another node can serve. Data for FT is kept consistent enough that the surviving path does not serve a torn write.

**Examples:**

A web tier drops one machine and the balancer sends the next request elsewhere: high availability. A storage array that finishes the write on the mirror without the client seeing an error: fault tolerance.
</details>

### Designing for Availability

<details>
<summary>How would you architect a system for high availability?</summary>

### How would you architect a system for high availability?

**Definition:**

Architect for high availability by removing single points, detecting failure quickly, and failing over without a human in the path.

**How it works:**

- Put a redundant balancer in front of several app instances, and replicate the database.
- Prefer active-active when both sides can serve, or active-passive when one warm standby is enough.
- Health checks and auto-scaling replace dead or overloaded instances.
- A second site covers a whole data-center loss. Decide what consistency you will give up during the split.
- Cache and a CDN take read load off the origin so a spike does not become an outage.
- Keep data residency rules in mind if a second region is not allowed to hold the data.

**Examples:**

```text
clients -> anycast / DNS
        -> load balancer pair
        -> app instances in two zones
        -> primary database + replica
```
</details>

<details>
<summary>What design patterns are commonly used to improve system availability?</summary>

### What design patterns are commonly used to improve system availability?

**Definition:**

Availability patterns are mostly about spare capacity, fast detection, and not letting one failed dependency stall everyone.

**How it works:**

- Redundancy and failover keep a second instance ready. A load balancer spreads work and skips unhealthy targets.
- A circuit breaker fails fast when a dependency is down, so callers stay up.
- Bulkheads isolate pools so one slow dependency cannot consume every thread.
- Retries with backoff cover a short blip. A queue absorbs a burst the service cannot handle right now.
- Health checks and leader election decide who is allowed to serve.
- A singleton process or an object pool can limit resource use, and they help availability only if losing that one process is not itself an outage.
</details>

<details>
<summary>How can load balancing improve system availability, and what are some of its potential pitfalls?</summary>

### How can load balancing improve system availability, and what are some of its potential pitfalls?

**Definition:**

A load balancer spreads requests across healthy instances so one overloaded or dead server does not take the service down. The balancer itself can become the outage if it is alone.

**How it works:**

- Round-robin cycles through targets. Least-connections prefers the idle one. A hash on client identity keeps a user on the same server.
- Health checks let the balancer stop sending work to a target that fails its probe.
- Sticky sessions help apps that keep state in memory, and they hurt when that one server dies: the session goes with it.
- Adding instances is how you scale, as long as the balancer and the database can take the extra fan-in.
- A single balancer is a single point of failure. Run a pair.
- A layer-7 balancer can route on the URL, and it terminates TLS, so certificate and config mistakes become site-wide.

**Examples:**

```text
request 1 -> app-a
request 2 -> app-b
request 3 -> app-c
app-b fails health -> next requests skip app-b
```
</details>

<details>
<summary>Explain the role of health checks in maintaining an available system.</summary>

### Explain the role of health checks in maintaining an available system.

**Definition:**

A health check is a repeated probe that decides whether an instance should receive traffic. It is how failover happens before users discover the failure.

**How it works:**

- A shallow check hits an HTTP endpoint and looks for a success status. It proves the process is listening.
- A deep check also opens the database, the queue, or a downstream the instance cannot serve without.
- CPU, memory, and disk tell you the machine is alive but too sick to be useful.
- The balancer, orchestrator, or mesh uses the result to pull the instance out and, when it recovers, to put it back.
- A check that always returns success hides the outage. A check that depends on a flaky downstream will flap the whole tier.

**Examples:**

GET /health returns 200 only if the process can run SELECT 1. The balancer drops any instance that fails twice in a row.
</details>

<details>
<summary>What is the purpose of a circuit breaker pattern in a distributed system?</summary>

### What is the purpose of a circuit breaker pattern in a distributed system?

**Definition:**

A circuit breaker protects availability by refusing to wait on a dependency that is already failing. Callers fail fast or use a fallback, and the dependency gets room to recover.

**How it works:**

- Closed: calls pass, and errors are counted.
- Open: calls do not go out. Latency stays low because nobody waits on the timeout.
- Half-open: a trial call checks whether the dependency is back.
- It limits cascading failure. Threads that would have blocked on the dead service stay free for work that can still succeed.
- The user gets a quick degraded answer instead of a hung page.

**Examples:**

Recommendations time out. The product page opens the circuit and renders without that block, instead of holding the response for 30 seconds.
</details>

### Monitoring & Incident Response

<details>
<summary>What are some key indicators you would monitor to ensure system reliability?</summary>

### What are some key indicators you would monitor to ensure system reliability?

**Definition:**

Reliability shows up in how often the system fails, how fast it returns, and whether the data and the machine underneath are still sound.

**How it works:**

- Availability indicators: uptime, MTBF, and MTTR.
- Performance: latency and throughput. A system that is up but too slow has failed its users.
- Errors: failure rate, and whether a fault is contained or spreading.
- Data: backup success, and whether replicas still match.
- Network: loss, latency, and saturation.
- The room and the power: temperature and supply, because a thermal trip looks like a software outage.
- Security controls matter when a breach is the failure mode, but they do not replace the availability signals.

**Examples:**

Alert when error rate exceeds 1% for five minutes, when p99 latency doubles, or when the last successful backup is older than a day.
</details>

<details>
<summary>How do you implement a monitoring system that accurately reflects system availability?</summary>

### How do you implement a monitoring system that accurately reflects system availability?

**Definition:**

Measure availability the way users experience it: a probe that performs a real action, from outside the cluster, and alert on that signal.

**How it works:**

- Synthetic checks from more than one network hit the public endpoint and record success or failure.
- Inside the cluster, health checks, saturation, and error rate explain why the outside probe failed.
- Count downtime from the user’s view. A process that is up while its load balancer has removed it is not available.
- Page on symptoms that mean users cannot complete the job, not on every CPU twitch.
- Keep the history so you can compute the nines for the month instead of guessing from a dashboard screenshot.

**Examples:**

A check every minute logs in and loads the account page from outside the data center. Minutes with a failed check are the downtime you report. Internal CPU graphs explain the cause, they do not define the outage.
</details>

<details>
<summary>Discuss the importance of alerting and on-call rotations in maintaining system reliability.</summary>

### Discuss the importance of alerting and on-call rotations in maintaining system reliability.

**Definition:**

Alerting is how a person learns that the system stopped being responsive or that a partition was detected. An on-call rotation is who is delegated to recover it.

**How it works:**

- The Reactive Manifesto says responsiveness is what makes problems detectable quickly. A system that only fails silently is not resilient. Recovery is delegated to another component, and for decisions a program cannot make, that delegate is a person.
- Brewer's partition handling starts with detection. An alert is the human-visible form of that detection: a deadline was missed, or a side stopped answering.
- Page on the user-visible failure, not on every internal twitch. A node drain that respects a PodDisruptionBudget is maintenance, not an outage, if minAvailable held.
- The rotation matters because recovery has a time bound. Retrying with no owner and no deadline chooses consistency by waiting forever, which is the availability loss Brewer describes.
- Afterward, the same person should be able to say whether the system entered an explicit degraded mode or kept accepting writes on both sides.

**Sources:**

- https://www.reactivemanifesto.org/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
</details>

<details>
<summary>What steps would you take to respond to an incident that reduces system availability?</summary>

### What steps would you take to respond to an incident that reduces system availability?

**Definition:**

Treat the incident as a partition or a failed component: detect it, limit what still runs, keep a bounded response, then recover.

**How it works:**

- Brewer's partition strategy is three steps: detect the partition, enter an explicit mode that can refuse some operations, then recover and compensate for mistakes made while split.
- During the split, choosing to proceed risks inconsistency. Choosing to cancel the operation reduces availability on purpose. Retrying forever is choosing consistency over availability.
- On Kubernetes, a node that is harming availability is drained so Pods move, while a PodDisruptionBudget stops the drain from removing the last healthy replicas.
- HDFS detects a dead DataNode by missing heartbeats and re-replicates blocks. The NameNode does not sit on the data path, so recovery is about block placement, not about proxying every byte.
- Say what is still served and what is refused. A timeout is already the moment the availability decision is made.

**Sources:**

- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>How can post-mortem analysis improve future system reliability and availability?</summary>

### How can post-mortem analysis improve future system reliability and availability?

**Definition:**

A post-mortem turns one outage into a change in detection, partition behavior, or recovery, so the next failure stays inside a bound.

**How it works:**

- Brewer's point is that partitions are rare, so most of the time you can have both consistency and availability, but only if detection and recovery are designed. The review asks whether those three steps existed.
- Check whether the system forfeited availability by retrying with no deadline, or forfeited consistency by accepting writes on both sides with no compensation.
- On Kubernetes, ask whether a PodDisruptionBudget would have kept minAvailable during the drain or the node loss, and whether etcd was a single copy of API state.
- On HDFS, ask whether replication and heartbeat timeouts matched the failure. Hardware failure is assumed; a review that only says "the disk died" has not found a gap.
- Record the user-visible symptom in the manifesto's terms: the system stopped being responsive, or it answered with data the other side would reject.

**Sources:**

- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://www.reactivemanifesto.org/
- https://kubernetes.io/docs/concepts/overview/components/
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

### Scaling & Performance

<details>
<summary>How does system scalability impact availability?</summary>

### How does system scalability impact availability?

**Definition:**

Scaling adds parts that can fail and paths that can partition. Availability holds only if the extra capacity does not become a new single place that must be up.

**How it works:**

- The manifesto's elastic property is staying responsive as load changes, by sharding or replicating and by avoiding a central bottleneck. Back-pressure keeps a fast source from exhausting the destination.
- HDFS is built for large data sets on many nodes and assumes some component is always down. Throughput, not low latency, is the goal. More nodes raise capacity and make failure detection mandatory.
- Kubernetes horizontal scaling adds Pods. If the cluster has no free CPU or memory, the new Pods stay unscheduled and availability does not rise. Scaling the node pool is the next step the autoscaling docs name.
- Brewer notes that wide-area copies kept for scale often accept staleness. Yahoo's PNUTS kept the master local and copied remotely in the background; that is a scale choice that spends consistency.

**Sources:**

- https://www.reactivemanifesto.org/
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://kubernetes.io/docs/concepts/workloads/autoscaling/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>

<details>
<summary>What strategies can be employed to scale a system while maintaining or improving reliability?</summary>

### What strategies can be employed to scale a system while maintaining or improving reliability?

**Definition:**

Scale by adding copies and shards that can fail independently, and keep a recovery path that does not require every copy to be reachable.

**How it works:**

- Manifesto resilience is replication plus isolation: one component can fail and another recovers it, and the caller is not the one handling that failure.
- HDFS stores each block on multiple DataNodes. A lost disk is repaired by re-replication. Computation is moved toward the data so the network is not the bottleneck that takes the job down.
- Kubernetes spreads Pods with the scheduler and can add replicas with an HPA when utilization rises. A disruption budget keeps a minimum number ready while a node is removed.
- etcd is described as a consistent and highly available store for API data. One etcd process is not that design.
- During a partition, Brewer says independent shards can still make progress. Global invariants cannot. Shard so a cut does not freeze every key.

**Sources:**

- https://www.reactivemanifesto.org/
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://kubernetes.io/docs/concepts/workloads/autoscaling/
- https://kubernetes.io/docs/concepts/overview/components/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>

<details>
<summary>Describe how caching can affect system reliability and what are some trade-offs.</summary>

### Describe how caching can affect system reliability and what are some trade-offs.

**Definition:**

A cache keeps answering when the origin is slow or briefly down. The trade-off is a copy that can disagree with the source.

**How it works:**

- Brewer describes caches and logged updates as the practical way his group raised availability, and the cost was less consistency.
- A read served from the copy is available even when the source is partitioned away. It is not a single up-to-date copy, which is Brewer's definition of consistency.
- Facebook's pattern in the same article keeps a closer copy that may be stale, and sends the user's own reads to the master for a short time after a write. That bounds how long the person who wrote sees the old value.
- Kubernetes Secrets are a poor cache of credentials: they sit in etcd, unencrypted unless encryption at rest is on. Caching them in a ConfigMap would drop even that separation.
- HDFS is not a cache. It is the durable copy, with replication as the reliability mechanism. Do not treat a replicated cache as the only copy of data you cannot regenerate.

**When to use:**

Cache a read that tolerates a bounded stale value. Do not cache the only copy of a write that must survive the cache process.

**Sources:**

- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

<details>
<summary>Explain the role of rate limiting in preserving system availability.</summary>

### Explain the role of rate limiting in preserving system availability.

**Definition:**

Rate limiting and back-pressure stop a fast sender from using up the receiver, so the system stays responsive instead of accepting unbounded work.

**How it works:**

- The manifesto says message queues are how you shape load, and back-pressure is applied when the receiver cannot keep up. Non-blocking communication spends resources only while the recipient is active.
- Reactive Streams exists for the same reason: a fast source must not force the destination to buffer an arbitrary amount. The exchange across a thread or a network is asynchronous, and the receiver grants credit.
- Brewer's latency view is a deadline. Past the deadline you either refuse the operation (less availability for that call) or proceed and risk inconsistency. A limit is the refuse path, chosen before the process runs out of memory.
- HDFS favors high throughput over low latency and assumes batch jobs, not an open flood of interactive calls. A limit in front of a NameNode metadata operation protects the single metadata service.
- An HPA adds Pods as CPU rises. That is capacity, not a limit. Without a limit, the autoscaler can add replicas until the cluster is full and then availability falls anyway.

**Sources:**

- https://www.reactivemanifesto.org/
- https://www.reactive-streams.org/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://kubernetes.io/docs/concepts/workloads/autoscaling/
</details>

### Reliability in Distributed Systems

<details>
<summary>How do eventual consistency and strong consistency differ and what are the reliability implications?</summary>

### How do eventual consistency and strong consistency differ and what are the reliability implications?

**Definition:**

Strong consistency here means a single up-to-date copy. Eventual consistency means copies may disagree for a while and converge after they can talk again.

**How it works:**

- CAP consistency is single-copy consistency: one up-to-date value. ACID consistency is a different word; it means the transaction kept the database rules, such as unique keys.
- BASE, which Brewer names as the availability-leaning end, includes soft state and eventual consistency. Those techniques keep working through a partition and therefore raise availability.
- If both sides of a partition accept updates, the copies diverge. Recovery has to compensate. If only one side accepts updates, the other side is unavailable for writes and the copy stays single.
- Asynchronous remote copies, as in the PNUTS example, make the local master fast and the remote copy stale until replication catches up. A reader of the remote copy can see an older value and still be "successful".
- Redis replication is asynchronous. A lock written on the master can be missing on the replica if the master dies first, so a second client can also hold it. That is an eventual copy that breaks mutual exclusion.

**Sources:**

- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
</details>

<details>
<summary>Describe the CAP theorem and its relevance to system availability.</summary>

### Describe the CAP theorem and its relevance to system availability.

**Definition:**

During a network partition, a shared-data system cannot be both perfectly consistent and perfectly available. The rest of the time it can pursue both.

**How it works:**

- Consistency means one up-to-date copy. Availability means that copy accepts updates. Partition tolerance means the system keeps going when messages are lost.
- The "pick two of three" slogan oversimplifies. Partitions are rare. While the network is whole, there is no reason to give up either property. The prohibition is only perfect C and perfect A while partitioned.
- Two nodes that cannot talk: if either may write, the copies diverge. If writes stop on one side, that side is unavailable. Retrying with no deadline, including via a protocol that needs a quorum, is choosing consistency.
- A partition is a timeout in practice. Some nodes may see it and others may not. Tighter deadlines enter partition mode during slowness, not only during a hard cut.
- The design Brewer recommends: detect, enter an explicit mode that may limit operations, then recover and restore invariants. Availability is a percentage, not a bit.

**Sources:**

- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>

<details>
<summary>Can you discuss how quorum-based decision making in distributed systems affects reliability?</summary>

### Can you discuss how quorum-based decision making in distributed systems affects reliability?

**Definition:**

A quorum lets the side that can still talk to a majority keep a single up-to-date decision. The minority side stops, which spends availability to keep consistency.

**How it works:**

- Brewer describes Paxos and atomic multicast as the case where the primary partition stays consistent and available, and the outside is not served. That is a quorum: progress requires the communicating majority.
- Retrying until a quorum answers delays the call. If the wait has no bound, the operation chooses consistency over availability. A deadline forces the choice into the open.
- Shards can each have their own quorum. A partition that does not cut a shard's majority lets that shard proceed. A global invariant that spans shards cannot be kept by either side alone.
- Redis's warning on replicated locks is the opposite of a quorum. A master and an asynchronous replica are not a majority vote. The replica can miss the lock write and grant it again.
- etcd is the Kubernetes component described as consistent and highly available for API data. Treating a single node as that store drops the quorum.

**Sources:**

- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
- https://kubernetes.io/docs/concepts/overview/components/
</details>

<details>
<summary>What is the role of distributed transactions in reliability, and what are the challenges associated with them?</summary>

### What is the role of distributed transactions in reliability, and what are the challenges associated with them?

**Definition:**

A distributed transaction tries to keep one outcome across nodes. Across a partition it cannot have ACID isolation on both sides, so reliability depends on compensation after the cut heals.

**How it works:**

- Brewer: serializability needs communication, so it fails across a partition. If you require ACID isolation, at most one side may proceed.
- Atomicity and durability are still useful on each side. Recovery is easier when each side has a durable history. Operations that broke an invariant during the split can be detected and compensated.
- Two-phase commit is one of the protocols he names as "retry until consistent". With no deadline it chooses consistency and the caller waits, which is an availability loss.
- ACID's C (rules such as unique keys) is broader than CAP's C (one copy). Partition recovery has to restore the rules, not only pick a winner value.
- HDFS does not offer multi-file transactions. Its coherency model is write-once, read-many, with append and truncate rather than arbitrary updates, which avoids a distributed commit for the common path.

**Sources:**

- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
</details>

### Recovery Strategies

<details>
<summary>What is a disaster recovery plan and how does it relate to reliability?</summary>

### What is a disaster recovery plan and how does it relate to reliability?

**Definition:**

A disaster recovery plan is the recovery half of failure handling: how you restore a single up-to-date copy after a site or a disk is gone, and what you refuse until that is done.

**How it works:**

- Brewer's third step after a partition is a recovery process that restores consistency and compensates for mistakes. A plan that only fails over without a merge step leaves two histories.
- HDFS persists filesystem metadata on the NameNode. A metadata disk failure is a different problem from a DataNode disk failure, which re-replication can heal. The plan has to say which disk you can lose.
- Kubernetes etcd holds all API objects. High availability of that store is the control-plane recovery story. A workload's bytes are a separate story: a PersistentVolume with reclaim Delete disappears with the claim.
- The manifesto puts recovery outside the failed component. Another component is delegated to restart it, and replication is what keeps the service responsive while that happens.
- Practice the deadline. A plan with no time bound retries forever and chooses consistency by accident.

**Sources:**

- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
- https://www.reactivemanifesto.org/
</details>

<details>
<summary>How do backup and restore operations impact system availability?</summary>

### How do backup and restore operations impact system availability?

**Definition:**

Backup copies state so restore can rebuild it. Restore is unavailable time unless a replica is already serving, and a restore from a stale backup spends consistency.

**How it works:**

- HDFS replication is the online copy: clients read from DataNodes while a failed disk is re-replicated. That is not the same as restoring a cold backup, which waits until the bytes are copied back.
- A PersistentVolume reclaim policy of Retain keeps the disk after the claim is deleted so an administrator can recover it. Delete removes the external asset. A backup that assumes Delete has nothing left to copy.
- Brewer's recovery step can reverse durable operations that violated an invariant. A restore that replays every write from both sides without that check brings the mistake back.
- Taking the only NameNode metadata disk offline to copy it is an availability hit on the metadata path. HDFS is designed so user data does not flow through the NameNode; the metadata path is the one a backup must not block casually.
- etcd is the Kubernetes copy of API state. Restoring an older snapshot rewinds objects. Workloads that were created after the snapshot disappear from the API even if their disks remain.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://kubernetes.io/docs/concepts/overview/components/
</details>

<details>
<summary>Discuss the importance and challenges of data replication in a highly available system.</summary>

### Discuss the importance and challenges of data replication in a highly available system.

**Definition:**

Replication keeps a second copy so a lost disk or a lost node does not take the data with it. The challenge is that the copies are not one copy until they have exchanged the latest write.

**How it works:**

- HDFS splits files into blocks and stores them on a set of DataNodes. The NameNode maps blocks to nodes and orders replication. DataNodes send heartbeats; missing heartbeats trigger re-replication. User data does not pass through the NameNode.
- The placement goal is fault tolerance on commodity hardware, where some component is always failing. More replicas cost disk and network.
- Asynchronous replication does not transfer a write before the master can die. Redis documents that race for locks: the replica never saw the key, so mutual exclusion fails.
- Brewer's remote-copy examples accept staleness to avoid wide-area latency. A reader must know which copy is allowed to be old.
- Kubernetes can run many Pods, but Pods are not data replicas. The bytes live on the volume. Replicating the Pod without replicating the volume only replicates the process.

**Sources:**

- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
</details>

<details>
<summary>Explain how you would plan for a failover strategy in a multi-region deployment to ensure reliability.</summary>

### Explain how you would plan for a failover strategy in a multi-region deployment to ensure reliability.

**Definition:**

Multi-region failover is a partition plan: decide which region may accept writes, how the other region catches up, and what clients see during the cut.

**How it works:**

- Brewer: wide-area systems cannot pretend partitions will not happen. During the cut you either stop writes on one side or you accept divergence and compensate later. A failover that promotes both regions is the divergence choice.
- One pattern he cites keeps the master in one place, so remote readers may see a stale copy, and after a user writes, that user's reads go to the master for a short time. Another keeps the master near the user and replicates outward asynchronously.
- Detection is local. Not every region will notice the cut at the same moment. Enter an explicit mode instead of letting timeouts pile up.
- Each region that is a Kubernetes cluster has its own API server and etcd. Draining a node fails Pods over inside one cluster. It does not move a volume to another region. PersistentVolume topology is a per-cluster fact; WaitForFirstConsumer only waits for a Pod in that cluster.
- HDFS replication is inside one cluster's DataNodes. A second region needs its own copy strategy. The NameNode metadata for one cluster is not the namespace of the other.
- Practice the return path. Failover without a rule for who is master after the network returns leaves two writers.

**Sources:**

- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
- https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html
- https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
</details>
