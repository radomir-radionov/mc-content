# Reactive Systems — Interview

Answers are filled.

[← Back to hub](./0_system_design.md)

### Related docs

- [Reactive Systems — Interview](./0_sd_reactive_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Reactive Systems](https://devinterview.io/questions/software-architecture-and-system-design/reactive-systems-interview-questions/).

### Reactive Systems Fundamentals

<details>
<summary>What are the defining characteristics of a Reactive System according to the Reactive Manifesto?</summary>

### What are the defining characteristics of a Reactive System according to the Reactive Manifesto?

**Definition:**

The Reactive Manifesto defines a reactive system by four properties: responsive, resilient, elastic, and message-driven.

**How it works:**

- Responsive: it answers in a timely way, and slow or failed parts do not freeze the caller.
- Resilient: it stays responsive when something fails, by isolating the failure and recovering.
- Elastic: it stays responsive under changing load by adding or removing resources.
- Message-driven: components collaborate by asynchronous messages, which gives the boundaries the other three need.
- A library of non-blocking operators is reactive programming. The manifesto is about the system, not only the API.

**Examples:**

A checkout publishes OrderPlaced and returns. Payment, stock, and mail each consume the message. One of them can be down without holding the HTTP call open.
</details>

<details>
<summary>How is back-pressure implemented in Reactive Systems to manage data flow?</summary>

### How is back-pressure implemented in Reactive Systems to manage data flow?

**Definition:**

Back-pressure is how a slow consumer tells a fast producer to ease off, instead of buffering until memory runs out.

**How it works:**

- In a pull model the consumer requests n more items. The producer does not send the rest. That is the Reactive Streams contract.
- In a push model the producer sends until a bounded buffer fills, then it blocks, drops, or fails the stream.
- Buffering absorbs a short burst and adds latency. An unbounded buffer is how you run out of memory.
- Dropping keeps the latest item and discards the rest. It fits metrics, not payments.
- Throttling or batching caps the rate. Scaling consumers is the other lever, and it does not help if the consumer is slow per item.

**Examples:**

```text
consumer: request(16)
producer: send at most 16, then wait
consumer: request(16) after it has finished the batch
```
</details>

<details>
<summary>Contrast Elasticity with Scalability with specific examples as they pertain to Reactive Systems.</summary>

### Contrast Elasticity with Scalability with specific examples as they pertain to Reactive Systems.

**Definition:**

Scalability is the ability to handle more load by adding resources. Elasticity is doing that automatically, in both directions, as the load changes.

**How it works:**

- Horizontal scale adds instances and a balancer. A social feed adds app servers as the user base grows. That growth can be a planned purchase.
- Vertical scale gives one machine more CPU or RAM. One box still has a ceiling, and it does not isolate failure.
- Elasticity watches the load and adds or removes instances without a ticket. A container platform scales a service from 2 pods to 20 for a sale and back to 2 after.
- A reactive system needs elasticity so responsiveness holds through a spike, not only through next quarter’s capacity plan.

**Examples:**

Buying a larger database server for the year is vertical scale. A Kubernetes HPA that adds checkout pods while CPU is high, and removes them when it drops, is elasticity.
</details>

<details>
<summary>Describe a strategy a Reactive System might use to maintain responsiveness during a component failure.</summary>

### Describe a strategy a Reactive System might use to maintain responsiveness during a component failure.

**Definition:**

Keep the caller responsive by not waiting on the failed part: isolate it, route around it, and answer with a degraded result.

**How it works:**

- Messages already decouple the caller from the callee’s process. A down consumer leaves messages in the queue.
- Location transparency means the sender names a service, not a host, so a router can pick a live instance.
- A circuit breaker stops sending to an instance that is timing out, and a balancer sends the next message elsewhere.
- Graceful degradation returns a smaller answer, such as a page without recommendations, instead of hanging.
- Work that can be stale stays available. Work that must be linearizable fails that one operation, not the whole process.

**Examples:**

Recommendations time out. The product page still returns in milliseconds with an empty recommendation block. The breaker stops calling that service until a probe succeeds.
</details>

<details>
<summary>How does message-driven architecture contribute to the resilience of Reactive Systems?</summary>

### How does message-driven architecture contribute to the resilience of Reactive Systems?

**Definition:**

Messages give components a boundary in time and in failure. The sender does not share a call stack with the receiver, so the receiver’s crash is a delayed message, not a dead caller.

**How it works:**

- Loose coupling: the sender knows a channel, not the receiver’s internals.
- Asynchronous delivery lets the sender stay responsive while the work happens later.
- A bounded queue is back-pressure. When it fills, the producer slows instead of allocating without limit.
- Location does not matter. The message can be handled by whichever instance is up.
- A transient outage becomes a retry from the queue, if the handler is safe to run twice.

**Examples:**

Checkout writes OrderPlaced and returns “accepted”. If mail is down, the message waits. Checkout’s success does not depend on SMTP.
</details>

<details>
<summary>Identify a resiliency strategy in Reactive Systems and explain how it minimizes the impact of failures.</summary>

### Identify a resiliency strategy in Reactive Systems and explain how it minimizes the impact of failures.

**Definition:**

Event sourcing keeps an append-only log of facts. Recovery replays the log, so a crashed process rebuilds state instead of hoping a mutable row was flushed.

**How it works:**

- A command is a request to change something. An event is the fact that it changed.
- The event store is the source of truth. The current state is a fold of the events.
- A crash after the event is durable loses nothing that was committed. Restart and replay.
- Consumers that missed events catch up from the log. They do not need the failed process to still be alive.
- Isolation still matters: one aggregate’s log does not have to share a failure domain with another’s.

**Examples:**

Account debits are events, not an overwritten balance. After a process dies, a new one replays Debited and Credited and reaches the same balance. A lost in-memory total is recomputed.
</details>

### Design Principles of Reactive Systems

<details>
<summary>Why are non-blocking I/O operations a necessity in Reactive Programming, and what problems do they solve?</summary>

### Why are non-blocking I/O operations a necessity in Reactive Programming, and what problems do they solve?

**Definition:**

Non-blocking I/O lets a few threads keep many slow operations in flight. A thread blocked on a socket cannot do other work, so responsiveness collapses as connections grow.

**How it works:**

- A blocking read pins a thread for the whole wait. Threads are expensive, and a pool runs out.
- While threads are stuck, new requests queue, and the process looks down even though the CPU is idle.
- Non-blocking calls register interest and return. A callback or an event loop continues when the socket is ready.
- That is what makes back-pressure and high connection counts possible without one thread per request.
- It does not by itself isolate failure. A non-blocking call to a dead dependency still needs a timeout.

**Examples:**

```text
blocking:  1 request waits on a thread until the database answers
non-blocking: the thread handles other requests; the database result resumes this one
```
</details>

<details>
<summary>In what ways can Domain-Driven Design (DDD) principles enhance the design of a Reactive System?</summary>

### In what ways can Domain-Driven Design (DDD) principles enhance the design of a Reactive System?

**Definition:**

DDD decides what the messages mean and where a model may change. A reactive runtime decides how those messages flow without blocking.

**How it works:**

- A bounded context is a natural service boundary: one model, one failure domain, scaled on its own.
- Domain events are already facts in the past tense, which is what a message-driven system wants to publish.
- A context map says how contexts integrate — customer–supplier, anti-corruption layer — so the async contract is explicit.
- The domain model stays free of broker details. The application edge turns a domain event into a message.
- Back-pressure at the boundary stops one context from flooding another.

**Examples:**

Sales emits LeadConverted. Billing’s anti-corruption layer maps it to OpenAccount. Sales does not know billing’s tables, and billing’s downtime does not block the sales command once the event is stored.
</details>

<details>
<summary>Provide an example of a system that is reactive without fulfilling all the Reactive Manifesto traits. Why does it qualify?</summary>

### Provide an example of a system that is reactive without fulfilling all the Reactive Manifesto traits. Why does it qualify?

**Definition:**

A system can be asynchronous and still miss resilience or elasticity. “Reactive” in the loose sense only means it reacts to messages. The manifesto wants all four traits.

**How it works:**

- Responsive and message-driven are the easy half: accept work onto a queue and answer quickly.
- Resilience needs isolation. If the worker and the checkout share one database pool, a stuck worker stalls checkout too.
- Elasticity needs the worker count to follow the queue depth. A single fixed process does not.
- So a mailer that queues messages is reactive in the everyday sense, and it is not a reactive system until a failure stays contained and capacity moves with load.

**Examples:**

An email worker reads a queue and sends mail, so the web request returns immediately. If that worker holds the only database connections, a hang in SMTP exhausts the pool and the site stops. The queue made it message-driven. It did not make it resilient.
</details>

<details>
<summary>Describe how a Reactive System would differently address a transient failure versus a network partition.</summary>

### Describe how a Reactive System would differently address a transient failure versus a network partition.

**Definition:**

A transient failure is a short blip, so the system retries. A partition is a lasting cut, so retrying forever only hides a choice between waiting and moving on.

**How it works:**

- Transient: a timeout, a reset, a brief overload. Retry with backoff and a limit, because the peer is expected back.
- The handler must be idempotent, or the retry doubles the effect.
- Partition: a peer stays unreachable. A circuit breaker opens so callers stop waiting out every timeout.
- Work that can lag stays queued and completes when the cut heals. Work that cannot lag fails that operation.
- Treating a partition as a transient error is how you pin threads on retries until the process is unresponsive.

**Examples:**

```text
one timeout: retry twice with backoff
breaker opens after repeated timeouts: stop calling, queue or fail
link returns: half-open probe, then drain the queue
```
</details>

<details>
<summary>Discuss a particular transport layer technology you would recommend for Reactive Systems’ asynchronous communication and why.</summary>

### Discuss a particular transport layer technology you would recommend for Reactive Systems’ asynchronous communication and why.

**Definition:**

A durable log such as Kafka fits reactive service-to-service traffic: producers append, consumers pull at their own pace, and a slow consumer does not push the producer out of memory.

**How it works:**

- The consumer commits offsets, which is pull-based back-pressure. It reads the next batch when it is ready.
- A crashed consumer resumes from the last committed offset instead of losing the in-flight call.
- Many consumers can share a hot stream, or a new consumer can read from the start. That is the hot-versus-replay choice.
- Keys keep an aggregate’s events in order on one partition. Other aggregates run in parallel on other partitions.
- Errors stay in the consumer: a poison message goes to a dead-letter path instead of failing the producer’s request.
- For a single process, Reactive Streams over gRPC or RSocket carries the same request(n) idea without a cluster. Across services, the log is the transport.

**Examples:**

Checkout appends OrderPlaced to the `orders` topic and returns. Mail and billing are two consumer groups. Mail can lag an hour without slowing billing or the HTTP handler.
</details>

<details>
<summary>Explain the role of Reactive Systems in processing continuous data streams, providing industry use cases.</summary>

### Explain the role of Reactive Systems in processing continuous data streams, providing industry use cases.

**Definition:**

A reactive stream processor stays responsive while events never stop arriving: it handles each event without blocking a thread per event, and it slows the source when it falls behind.

**How it works:**

- The input is unbounded. There is no “query finished”, only windows, aggregates, and late events.
- Non-blocking I/O and asynchronous handoff keep one machine on many streams.
- Trading and fraud look at market or payment events as they happen, not in a nightly batch.
- Telemetry, IoT, and patient monitors are the same shape: sensors emit, a service reacts, and a slow sink must push back.
- Chat, live inventory, and ad auctions are product versions of the same loop.

**Examples:**

A chat server treats each message as an event on a WebSocket. Fans see it without a refresh. If one client’s socket blocks, that client is dropped or buffered with a limit. The room’s other clients keep receiving.
</details>

### Implementing Reactive Systems

<details>
<summary>What is Event Sourcing, and how does it benefit Reactive Systems?</summary>

### What is Event Sourcing, and how does it benefit Reactive Systems?

**Definition:**

Event sourcing stores the sequence of facts and derives current state by replaying them. A reactive system benefits because the log is already an asynchronous, replayable stream.

**How it works:**

- Writes append an event. They do not overwrite a row and lose the reason.
- Reads use a projection folded from the events. The projection can lag, which matches eventual consistency.
- A new consumer builds its own projection from the same log without asking the writer to change.
- Audit and debugging are the log itself: you can see the fact that led to a bad state.
- The cost is designing events so they can be replayed, and versioning them when the fact’s shape changes.

**Examples:**

```text
append StockReserved {sku:lamp, qty:1}
append StockReleased {sku:lamp, qty:1}
projection: on-hand = fold(events) 
```
</details>

<details>
<summary>Differentiate between hot and cold reactive streams with examples of use cases.</summary>

### Differentiate between hot and cold reactive streams with examples of use cases.

**Definition:**

A cold stream produces items because someone subscribed, and each subscriber gets its own sequence. A hot stream produces items whether or not anyone is listening, and subscribers share what is happening now.

**How it works:**

- Cold: the work starts at subscribe and can be repeated. A database query or an HTTP call per subscriber is cold.
- Hot: the work is already running. A late subscriber misses what already happened unless the stream also keeps a replay log.
- Cold fits request/response and one-off computations.
- Hot fits sensors, prices, and UI events that exist independently of one listener.

**Examples:**

Asking for a weather snapshot starts a fetch for that caller: cold. A station that emits a temperature every second, shared by every dashboard, is hot. A dashboard that connects at 12:00 does not see 11:00 unless you replay.
</details>

<details>
<summary>Compare synchronous request-response communication with reactive message-driven communication in terms of scalability.</summary>

### Compare synchronous request-response communication with reactive message-driven communication in terms of scalability.

**Definition:**

Synchronous request-response scales only as far as the slowest hop the caller is waiting on. Message-driven work scales by adding consumers that pull from a queue, without holding the original request.

**How it works:**

- A sync chain uses a thread or a connection for the whole wait. More clients means more waiting callers, and one slow service stalls the chain.
- A message lets the acceptor return, and the queue absorbs the burst. Consumers scale out independently of the acceptors.
- Parallel consumers on separate partitions do the work at the same time. The caller’s latency is “accepted”, not “finished”.
- Failure of one consumer does not fail the acceptor. The message remains for another consumer.
- You give up a single immediate result. If the user must see the outcome now, that step stays synchronous and becomes the scale limit.

**Examples:**

```text
sync:    client waits while checkout calls stock calls payment
message: client gets "accepted"; stock and payment consume in parallel
```
</details>

<details>
<summary>Describe a design pattern uniquely suited for Reactive Systems and why it’s effective.</summary>

### Describe a design pattern uniquely suited for Reactive Systems and why it’s effective.

**Definition:**

Supervision fits Reactive Systems: a component does not repair itself. Another component is delegated to restart it, and the failure stays inside that boundary.

**How it works:**

- The manifesto achieves resilience by replication, containment, isolation, and delegation. The client of a failed component is not the one handling the failure.
- Failures can be messages. The same message-passing boundary works inside one host or across a cluster, which is location transparency.
- A supervisor decides stop, restart, or escalate. That is containment: one bad stream does not have to take the process down with it.
- Pair it with back-pressure so the restarted component is not immediately flooded by a queue that grew without a bound.

**Sources:**

- https://www.reactivemanifesto.org/
- https://www.reactive-streams.org/
</details>

<details>
<summary>Recommend a framework for developing Reactive Systems and discuss its core features.</summary>

### Recommend a framework for developing Reactive Systems and discuss its core features.

**Definition:**

RxJava is a JVM library for composing asynchronous sequences. Use Flowable when the volume is unbounded and the consumer must be able to slow the producer.

**How it works:**

- map applies a function to each item and emits the results. flatMap turns each item into an inner sequence and merges those inners, so items may interleave. concatMap keeps order.
- Schedulers stand in for threads. subscribeOn(Schedulers.io()) moves a blocking call off the caller. computation is for CPU work. A flow stays sequential: one lambda runs items one after another on the scheduler thread it was given.
- Reactive Streams, which JDK Flow matches, is the interop layer: Publisher, and a Subscriber that requests credit so a fast source cannot force an unbounded buffer.
- The manifesto's message-driven property is the system-level version of the same idea. RxJava is the in-process library, not the whole cluster.

**When to use:**

RxJava or Reactor on the JVM when you compose asynchronous calls in one process. A message broker when the boundary is another service.

**Examples:**

```text
Flowable.fromCallable(this::blockingRead)
    .subscribeOn(Schedulers.io())
    .observeOn(Schedulers.single())
```

**Sources:**

- https://github.com/ReactiveX/RxJava/wiki/Transforming-Observables
- https://reactivex.io/documentation/operators/map.html
- https://reactivex.io/documentation/operators/flatmap.html
- https://www.reactive-streams.org/
- https://www.reactivemanifesto.org/
</details>

<details>
<summary>Explain how statefulness is managed in Reactive microservices while ensuring consistency.</summary>

### Explain how statefulness is managed in Reactive microservices while ensuring consistency.

**Definition:**

Keep durable state in one place that can still choose consistency during a partition, and keep the reactive pipeline as a stream of messages about that state.

**How it works:**

- Brewer: during a partition you either stop updates on one side or you accept divergence. Eventual consistency is the availability-leaning choice and needs a recovery step that compensates.
- A reactive service should not hide that choice inside an unbounded buffer. Reactive Streams requires the receiver to grant credit so the queue between threads stays bounded.
- The manifesto isolates components with asynchronous messages. Location-transparent messaging means the same failure handling works on one host or across a cluster. The store behind the messages is still subject to CAP.
- Soft state in the pipeline, such as a window that can be rebuilt, is Brewer's BASE. Do not treat that window as the only copy.

**Sources:**

- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://www.reactive-streams.org/
- https://www.reactivemanifesto.org/
</details>

### Reactive Programming Paradigms

<details>
<summary>Discuss how functional programming principles can be leveraged in Reactive Systems to enhance resiliency.</summary>

### Discuss how functional programming principles can be leveraged in Reactive Systems to enhance resiliency.

**Definition:**

Pure transformations make a stage restartable: the same input produces the same output, so a supervisor can run it again without repairing hidden state.

**How it works:**

- map is that shape. It applies a function to each item. The operator does not by itself pick a thread.
- Side effects inside the function, such as writing a shared variable, break the assumption. RxJava documents that a single flow processes items one after another on the assigned thread, but a shared mutable cell is still a race if another flow touches it.
- Isolation in the manifesto is the system version: a failed component is restarted from the messages it was given, and the client does not hold its internal variables.
- Impure edges, such as a database write, stay at the boundary. The core of the pipeline stays a function of its messages.

**Sources:**

- https://reactivex.io/documentation/operators/map.html
- https://www.reactivemanifesto.org/
- https://docs.scala-lang.org/overviews/scala-book/pure-functions.html
</details>

<details>
<summary>Contrast the Observer pattern with the Publish-Subscribe pattern, providing use cases in Reactive Programming.</summary>

### Contrast the Observer pattern with the Publish-Subscribe pattern, providing use cases in Reactive Programming.

**Definition:**

Observer is a direct callback list. Publish-subscribe puts a message boundary between the producer and the consumers so they do not share a call stack.

**How it works:**

- The manifesto relies on asynchronous message-passing for loose coupling, isolation, and location transparency. Failures are delegated as messages. That is publish-subscribe at system scale.
- An Rx observable is closer to observer until it crosses a scheduler or a process. subscribe attaches a consumer. observeOn moves the notification onto another scheduler.
- Publish-subscribe matters when the consumer may be slow or remote. Reactive Streams then requires demand signaling so the publisher does not buffer without a bound.
- Use a direct observable for UI events in one process. Use a queue or broker when a down consumer must not stop the producer, and when you need the manifesto's isolation.

**When to use:**

Observer when both sides are in one process and the rate is already bounded. Publish-subscribe when the consumer can be down or far away.

**Sources:**

- https://www.reactivemanifesto.org/
- https://www.reactive-streams.org/
- https://github.com/reactivex/rxjava/blob/4.x/README.md
</details>

<details>
<summary>Provide a use case where Reactive Programming offers significant benefits over an imperative approach.</summary>

### Provide a use case where Reactive Programming offers significant benefits over an imperative approach.

**Definition:**

A fan-out of per-item async calls is the case: each id becomes its own request, and flatMap merges the results without a thread blocked per call.

**How it works:**

- flatMap applies a function that returns an inner observable and merges the inners. Order is not guaranteed. An optional concurrency argument caps how many inners are subscribed at once.
- The imperative version waits for each call, or starts threads and a shared list. The shared list is a race. The reactive version keeps the join inside the operator.
- Reactive Streams back-pressure still applies if the merged result is faster than the subscriber. Otherwise the merge only moved the buffer.
- This pays off for many in-flight network calls. A short CPU loop over an array does not need it. map is enough when each item becomes one value, not a new request.

**Examples:**

```text
ids.flatMap(id -> fetch(id), 8)
```

**Sources:**

- https://reactivex.io/documentation/operators/flatmap.html
- https://www.reactive-streams.org/
</details>

<details>
<summary>Define a “stream” in Reactive Programming, demonstrating its advantages over traditional collections like arrays or lists.</summary>

### Define a “stream” in Reactive Programming, demonstrating its advantages over traditional collections like arrays or lists.

**Definition:**

A stream is a sequence of elements that arrive over time, possibly without a known end, pushed across an asynchronous boundary with back-pressure.

**How it works:**

- Reactive Streams exists because live data has no predetermined volume. A fast source must not force the destination to buffer arbitrarily. Asynchrony lets cores or hosts work in parallel.
- A list is a finished collection. You can ask for its size and index it. A stream may still be producing. Completion and error are events, not a return value you already hold.
- The JDK Flow interfaces match the Reactive Streams interfaces. Libraries interoperate if they follow that protocol.
- map and flatMap are how you transform the stream without copying it into a new list first. flatMap is for when one element becomes another stream.

**Sources:**

- https://www.reactive-streams.org/
- https://reactivex.io/documentation/operators/map.html
- https://reactivex.io/documentation/operators/flatmap.html
</details>

### Fault-tolerance and Reliability

<details>
<summary>How can the circuit breaker pattern prevent cascading failures in a Reactive System?</summary>

### How can the circuit breaker pattern prevent cascading failures in a Reactive System?

**Definition:**

A circuit breaker stops calling a dependency that is already failing, so the caller stays responsive and the dependency gets a bounded load instead of a retry storm.

**How it works:**

- The manifesto's resilience is containment and isolation. A component that keeps retrying a dead neighbor spends its own threads and spreads the failure.
- Brewer's deadline is the same decision: past the timeout you cancel, which lowers availability for that call, or you proceed and risk a bad result. A breaker is the cancel path made explicit, with a cool-down before the next try.
- Reactive Streams back-pressure limits a fast healthy source. It does not by itself notice that the remote side is returning errors. The breaker is that extra gate.
- When the breaker is open, answer from a fallback or fail that call. Do not buffer every rejected request. An unbounded buffer recreates the overload the spec forbids.
- Half-open means a few trial calls, not a return to full concurrency. flatMap's max-concurrency cap is a related knob on the success path.

**Sources:**

- https://www.reactivemanifesto.org/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://www.reactive-streams.org/
- https://reactivex.io/documentation/operators/flatmap.html
</details>

<details>
<summary>Outline an effective back-pressure strategy for a Reactive System experiencing high load.</summary>

### Outline an effective back-pressure strategy for a Reactive System experiencing high load.

**Definition:**

The subscriber requests a finite credit, the publisher sends only that many elements, and overflow is handled by a bounded policy rather than an ever-growing queue.

**How it works:**

- Reactive Streams governs exchange across an asynchronous boundary so the destination is not forced to buffer an arbitrary amount. Back-pressure signaling itself must be asynchronous. A synchronous request would cancel the benefit.
- JDK Flow is semantically the same protocol. Version 1.0.4 is the JVM artifact set: API, specification, and a TCK.
- The manifesto applies the same idea to queues between components: shape the queue and apply back-pressure when necessary.
- flatMap accepts a concurrency limit so one outer stream cannot subscribe to unbounded inners.
- If the producer cannot slow down, drop or fail with a bounded buffer and a metric. Do not pretend an infinite queue is back-pressure. During a partition, Brewer's choice still applies: refuse or diverge.

**Sources:**

- https://www.reactive-streams.org/
- https://www.reactivemanifesto.org/
- https://reactivex.io/documentation/operators/flatmap.html
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>

<details>
<summary>Describe a supervision strategy in Reactive Systems and its role in managing failures.</summary>

### Describe a supervision strategy in Reactive Systems and its role in managing failures.

**Definition:**

Supervision delegates recovery to a parent. The child reports failure as a message. The parent restarts, stops, or escalates, and the caller of the parent stays responsive.

**How it works:**

- The manifesto: failures are contained, components are isolated, and recovery of each component is delegated to another. The client is not burdened with handling those failures. Replication keeps the service available while one copy is down.
- Message-passing is the boundary that makes this work the same way in-process and across a cluster.
- Restart only works if the child can be rebuilt from its inputs. A pure map stage can. A stage that mutated a hidden counter cannot, unless that counter is in the messages.
- Do not restart in a tight loop against a dead dependency. Combine supervision with a breaker and with back-pressure so the restart is not immediately overloaded.
- Escalate when the parent cannot restore the invariant. That matches Brewer's recovery step: compensate, do not silently resume both writers.

**Sources:**

- https://www.reactivemanifesto.org/
- https://www.reactive-streams.org/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>

### Performance and Scalability

<details>
<summary>Explain how Reactive Systems exploit multicore and distributed resources to improve performance.</summary>

### Explain how Reactive Systems exploit multicore and distributed resources to improve performance.

**Definition:**

Asynchrony lets stages run on other cores or other hosts while the caller does not block, and back-pressure keeps that parallelism from allocating unbounded queues.

**How it works:**

- Reactive Streams states the reason explicitly: asynchrony enables parallel use of cores on one machine or of collaborating hosts. The protocol keeps the queues between them bounded.
- RxJava schedulers are the in-process tool. computation for CPU-bound work, io for blocking calls, single for a sequential handoff. observeOn shifts the downstream stage onto another scheduler. Items inside one stage still run one after another.
- The manifesto's elasticity is the cluster version: shard or replicate, avoid a central bottleneck, and scale from live measurements.
- Parallelism does not remove CAP. Two hosts that both write during a partition diverge. Moving work to more cores does not create a single up-to-date copy.

**Sources:**

- https://www.reactive-streams.org/
- https://github.com/reactivex/rxjava/blob/4.x/README.md
- https://www.reactivemanifesto.org/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>

<details>
<summary>What does “location transparency” mean in Reactive Systems, and how does this concept apply to a distributed data store?</summary>

### What does “location transparency” mean in Reactive Systems, and how does this concept apply to a distributed data store?

**Definition:**

Location transparency means a message is handled with the same semantics whether the receiver is local or remote. The store behind it still has a real place, and a partition still forces a consistency choice.

**How it works:**

- The manifesto: explicit message-passing creates a boundary for loose coupling, isolation, and location transparency. Failure management uses the same constructs on one host or across a cluster.
- That does not make the data one copy. Brewer: if the nodes cannot talk, either one side stops taking updates or the copies diverge. Asynchronous remote replicas, as in his PNUTS and Facebook examples, are stale until they catch up.
- A client that addresses a logical name can be moved when a supervisor restarts the owner elsewhere. The bytes still live on the nodes that hold them.
- Back-pressure must cross that boundary too. A local call that blocks until the remote buffer grows is not the non-blocking communication the manifesto describes.

**Sources:**

- https://www.reactivemanifesto.org/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://www.reactive-streams.org/
</details>

<details>
<summary>Identify potential performance bottlenecks in a streaming Reactive System and suggest monitoring strategies.</summary>

### Identify potential performance bottlenecks in a streaming Reactive System and suggest monitoring strategies.

**Definition:**

The usual bottlenecks are an unbounded queue, a blocking call on a small scheduler, and a stage that cannot request credit fast enough.

**How it works:**

- Reactive Streams calls out the failure mode: if back-pressure were synchronous, or if the destination buffered without a bound, the point of asynchrony is lost.
- RxJava's io scheduler is for blocking work. Running a blocking call on the computation pool, which is sized for CPU work, stalls every other stage on that pool.
- flatMap without a concurrency cap subscribes to every inner at once. That is a bottleneck of sockets and memory, not of CPU.
- The manifesto says elasticity depends on live performance measures. Watch queue depth, request credit, and error rate. An open breaker or a dropped element is a signal, not only a log line.
- A timeout that never fires looks like high latency and is Brewer's hidden choice of consistency. Measure how often calls pass the deadline.

**Sources:**

- https://www.reactive-streams.org/
- https://github.com/reactivex/rxjava/blob/4.x/README.md
- https://reactivex.io/documentation/operators/flatmap.html
- https://www.reactivemanifesto.org/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>

<details>
<summary>Describe a method or tools you would use to test and ensure a Reactive System’s scalability under a simulated load.</summary>

### Describe a method or tools you would use to test and ensure a Reactive System’s scalability under a simulated load.

**Definition:**

Drive the stream with a controlled producer, assert that the consumer's demand bounds the queue, and include a scheduler you can advance in tests.

**How it works:**

- Reactive Streams publishes a TCK with the 1.0.4 artifacts so an implementation can be checked against the specification, including demand and completion rules.
- RxJava's test scheduler is one of the schedulers the parallel-flow notes name, alongside computation and io. It lets a test decide when time moves instead of sleeping.
- A load test should show a bounded queue while the producer is faster than the consumer. If heap grows with input rate, back-pressure is not in effect.
- Also fail a dependency and confirm the supervisor or breaker contains it. The manifesto's resilience is part of scalability: extra load plus one dead neighbor must not exhaust threads.
- Do not only test the happy path on a single thread. Asynchrony is the case the spec is about.

**Sources:**

- https://www.reactive-streams.org/
- https://github.com/reactivex/rxjava/wiki/Parallel-flows
- https://www.reactivemanifesto.org/
</details>

### Tools and Technologies

<details>
<summary>List some key Reactive programming libraries and provide a brief overview of one framework’s core feature set.</summary>

### List some key Reactive programming libraries and provide a brief overview of one framework’s core feature set.

**Definition:**

Reactive Streams is the small interop spec. RxJava is a full operator library on the JVM that can speak that protocol through Flowable.

**How it works:**

- The spec's scope is deliberately minimal: interfaces and rules for asynchronous streams with non-blocking back-pressure. End-user DSLs are out of scope so each platform can stay idiomatic. JVM interfaces are the 1.0.4 release. JDK Flow is semantically equivalent.
- RxJava adds the DSL: map for one-to-one transforms, flatMap to merge inner sequences, schedulers for concurrency, and subscribeOn / observeOn to place work.
- The Reactive Manifesto is not a library. It names the four system properties those libraries are often used to approach: responsive, resilient, elastic, message-driven.
- Pick the library for in-process composition. Pick a broker when the stream must leave the process. The spec also points at network protocols as a separate working group.

**Sources:**

- https://www.reactive-streams.org/
- https://reactivex.io/documentation/operators/map.html
- https://reactivex.io/documentation/operators/flatmap.html
- https://www.reactivemanifesto.org/
</details>

<details>
<summary>What role do message queues play in Reactive Systems, and what characteristics are most important for them to have?</summary>

### What role do message queues play in Reactive Systems, and what characteristics are most important for them to have?

**Definition:**

Queues are the boundary between components. They must be bounded, or they must push back, or they become the buffer the Reactive Streams spec exists to forbid.

**How it works:**

- The manifesto: explicit message-passing gives loose coupling, isolation, and location transparency. Load is managed by shaping and monitoring queues and by applying back-pressure. Non-blocking communication uses resources only while the recipient is active.
- A queue that grows without a limit hides overload until memory runs out. That is not elasticity.
- Asynchronous replication of a queue, like any other copy, can lose the latest message if the primary dies first. Brewer's recovery step still applies. Do not treat the queue as a single up-to-date copy unless the broker gives you that.
- The important properties are a bound, demand or credit, isolation of a slow consumer, and a visible depth so you can scale or refuse.
- Failures travel as messages too, so a poison message needs a stop or a dead-letter path rather than a restart loop.

**Sources:**

- https://www.reactivemanifesto.org/
- https://www.reactive-streams.org/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>

<details>
<summary>What are some common challenges faced when integrating Reactive Systems with non-reactive legacy systems, and how can they be mitigated?</summary>

### What are some common challenges faced when integrating Reactive Systems with non-reactive legacy systems, and how can they be mitigated?

**Definition:**

A legacy API blocks a thread until it returns. Dropping that call onto an event loop stalls every other stream on that loop. The mitigation is a scheduler meant for blocking work, plus a bound on how many calls run.

**How it works:**

- RxJava's own example of a slow callable uses subscribeOn(Schedulers.io()) and then observeOn to hop back. io is the pool for that blocking edge. computation is not.
- The reactive side still needs credit. If the legacy system cannot slow down, put a bounded queue in front and fail or shed load when it is full. An infinite adapter queue breaks the Reactive Streams goal.
- Timeouts are mandatory. A legacy call with no deadline is Brewer's "retry until consistent": availability waits. Cancel and surface the failure to the supervisor.
- Do not wrap the legacy client in flatMap with unlimited concurrency. Cap the inners.
- Consistency does not improve because the edge is reactive. If the legacy store is the single copy, say so. If it replicates asynchronously, readers can see a stale value.

**Sources:**

- https://github.com/reactivex/rxjava/blob/4.x/README.md
- https://www.reactive-streams.org/
- https://reactivex.io/documentation/operators/flatmap.html
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>
