# SOA — Interview

Answers are filled.

[← Back to hub](./0_system_design.md)

### Related docs

- [SOA — Interview](./0_sd_soa_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — SOA](https://devinterview.io/questions/software-architecture-and-system-design/service-oriented-architecture-interview-questions/).

### SOA Fundamentals

<details>
<summary>What are the core principles of Service-Oriented Architecture (SOA)?</summary>

### What are the core principles of Service-Oriented Architecture (SOA)?

**Definition:**

SOA splits a system into services that each own a business capability and publish a contract other systems can call.

**How it works:**

- Loose coupling: callers use an API or a queue, not the provider’s code, tables, or host name.
- Contract-driven: each service publishes operations and a schema so both sides know the messages.
- Platform neutrality: a service may use any stack as long as the messages stay interoperable.
- Granularity: a service does one coherent job. A middleware layer can compose several services into a larger flow.
- Reusability: the same capability, such as billing, is called by many applications instead of being copied.
- Autonomy: a team can change and deploy its service on its own schedule.
- Discovery and composition: consumers find services and assemble them for a process.
- Scale and isolation: load can be spread, and a slow service does not have to stall every other capability.
- Persistence: a service may keep its own durable state rather than borrowing the caller’s database.
- Compared with a monolith, SOA draws boundaries on business capabilities and talks over the network. Microservices push the same idea toward smaller deployable units that each own their data.

**Examples:**

An order screen calls a shared Customer service and a shared Pricing service. Replacing the pricing engine does not require a new checkout binary, as long as the price message stays the same.
</details>

<details>
<summary>Can you describe the difference between SOA and a microservices architecture?</summary>

### Can you describe the difference between SOA and a microservices architecture?

**Definition:**

Both styles split work into services with contracts. They differ in size, data ownership, how they integrate, and how strictly the enterprise governs them.

**How it works:**

- SOA services are usually coarser and may cover several related capabilities. A microservice is typically one capability, small enough for one team.
- Classic SOA often integrates with synchronous calls and an enterprise bus. Microservices mix HTTP with queues and try to avoid a shared bus as the place where business logic lives.
- SOA frequently shares canonical data models through contracts. Microservices prefer a private database per service and accept duplication at the edges.
- SOA tends to standardize the stack and central governance. Microservice teams pick their own language and release on their own pipeline.
- SOA deployments are independent but often coordinated. A microservice can ship without redeploying its neighbors.
- Shared problems remain: finding a healthy instance, partial failure, and data that is no longer in one transaction.
- Principles that carry over: loose coupling, a published contract, and autonomy. What gets stricter is “you own your data” and “you deploy alone.”

**Examples:**

A SOA “Order” service might reserve stock, charge a card, and notify shipping behind one contract. A microservice split would make Inventory, Payments, and Shipping separate deployables, each with its own store, and compose them with events.
</details>

<details>
<summary>Explain the concept of a service contract in SOA.</summary>

### Explain the concept of a service contract in SOA.

**Definition:**

A service contract is the promise a consumer may rely on: operations, message shapes, protocol, faults, and policies. Internals on either side may change while that promise holds.

**How it works:**

- It names the operations, the schema of each message, and which faults can come back.
- Policies ride along: authentication, encryption, idempotency, and sometimes a service level.
- Provider and consumer can be written in different languages because they share the contract, not the code.
- Styles include a WSDL document, an OpenAPI description, or an explicit schema such as JSON Schema or protobuf.
- Versioning is part of the contract. A breaking change is a new contract, not a silent edit.
- The split is what makes independent releases possible: callers bind to the contract, not to a class inside the provider.

**Examples:**

```text
POST /reservations
body: { "sku": "A1", "qty": 2 }
201: { "id": "r-9" }
409: { "error": "out_of_stock" }
```

The inventory team can replace its database as long as this request, this success body, and this conflict error stay valid.
</details>

<details>
<summary>How does SOA facilitate service reusability?</summary>

### How does SOA facilitate service reusability?

**Definition:**

SOA aims at one implementation of a business function, behind a stable contract, so many applications call it instead of copying the logic.

**How it works:**

- Modularity keeps the function in one service with a single purpose.
- A shared contract and a standard message format mean a new channel does not need a private variant.
- Loose coupling lets the provider move or be rewritten without each caller changing.
- Platform-neutral messages let a web shop, a branch app, and a batch job all call the same billing service.
- A registry makes that service findable, so teams do not invent a second one because they cannot see the first.
- Reuse fails when each team still ships a private copy with a slightly different schema.

**Examples:**

Checkout, a call center tool, and a partner API all call `Customer.lookup`. A new field is added to that contract once, not pasted into three codebases.
</details>

<details>
<summary>What is loose coupling in SOA and why is it important?</summary>

### What is loose coupling in SOA and why is it important?

**Definition:**

Loose coupling means a caller depends on another service’s contract, not on its code, database, or address. You can move or replace the provider without redeploying every caller.

**How it works:**

- Interaction goes through messages or an API. A shared table or an in-process call is the opposite.
- Location transparency: the caller looks up an endpoint instead of baking in a host.
- The provider can scale, fail over, or change its stack while the contract holds.
- That buys resilience (one outage need not take down every caller if they can retry or queue) and lets teams ship on separate schedules.
- The hard parts are discovery and versioning, keeping data consistent across a boundary, and agreeing on transport and message format.
- Techniques that keep the coupling low: a published contract, messaging, stateless operations, idempotent retries, and hiding the provider’s data model.

**Examples:**

Checkout calls `POST /prices` and does not know whether pricing runs in one process or twenty. Moving pricing to another cluster changes the registry entry, not the checkout code.
</details>

<details>
<summary>What do you understand by service orchestration and service choreography in the context of SOA?</summary>

### What do you understand by service orchestration and service choreography in the context of SOA?

**Definition:**

Orchestration puts one coordinator in charge of the steps. Choreography has no conductor: each service reacts to events and chooses its own next step.

**How it works:**

- An orchestrator calls inventory, payment, and shipping in order and handles errors in one place. The flow is easy to read and to change.
- That coordinator is also a place that can bottleneck or become a second monolith if too much logic lands there.
- In choreography, “seat reserved” is an event. Billing and notification each subscribe and act alone. No service tells the others what to do next.
- Choreography avoids a central runtime and keeps services independent. The overall flow is harder to see, and a missing subscriber fails quietly unless you trace events.
- Pick orchestration when the business process has a strict order and one owner. Pick choreography when peers should stay decoupled and the set of listeners will grow.

**Examples:**

Orchestration: `placeOrder` calls reserve, then charge, then ship, and compensates if charge fails. Choreography: Order emits `OrderPlaced`; Inventory and Billing each consume it and emit their own result. Nobody waits inside `placeOrder` for both.
</details>

<details>
<summary>How does SOA differ from traditional monolithic application architectures?</summary>

### How does SOA differ from traditional monolithic application architectures?

**Definition:**

A monolith ships as one process with in-process calls and usually one database. SOA splits capabilities into services that talk over the network and can be owned and scaled apart.

**How it works:**

- A monolith’s modules live in one codebase and one deployable. A change or a scale-out touches the whole application.
- SOA modules are services. They call each other with messages, not with a local function.
- Teams can own a service for its whole life and release it without rebuilding every other capability.
- The monolith gets one transaction and one schema. SOA pushes consistency across a network boundary.
- Services may use different languages. A monolith usually shares one stack.
- You pay latency, partial failure, and more operations in exchange for deploying and scaling a capability on its own.

**Examples:**

In one process, `createOrder` calls `reserveStock` as a method and both share a database transaction. After the split, checkout sends a reservation request and must handle a timeout even if the stock service later succeeds.
</details>

<details>
<summary>What role does a service registry play in SOA?</summary>

### What role does a service registry play in SOA?

**Definition:**

A service registry is the directory where providers publish a contract and an endpoint, and where consumers look them up. Lookup replaces hard-coded addresses.

**How it works:**

- A provider registers its name, version, address, and metadata. A consumer queries that entry before it calls.
- The registry can also push changes, so subscribers learn when an instance appears or goes away.
- That is what keeps location transparent: the process can move, and callers still resolve the name.
- Entries should be access-controlled, and updates should be authenticated so a stranger cannot publish a fake endpoint.
- Classic SOA used UDDI. The same job today is often Consul, a mesh control plane, or a cloud service catalog.
- The registry is not the bus. It answers “where is a compatible instance?” It does not transform the message.

**Examples:**

On startup a pricing process registers `pricing` at `10.0.4.8:8080` with version `2`. Checkout asks the registry for `pricing` version `2` and calls whatever address comes back. A second replica registers beside it. Checkout does not store either IP.
</details>

### Designing SOA-based Systems

<details>
<summary>What factors do you consider when designing a service interface in SOA?</summary>

### What factors do you consider when designing a service interface in SOA?

**Definition:**

Shape the interface around one business capability, with a small explicit schema, so callers can reuse it without learning the provider’s internals.

**How it works:**

- Granularity: wide enough to be useful, narrow enough that unrelated changes do not ride along. Group operations that belong in one business step.
- Keep inputs small and make the output the full result of that call, including a clear fault format.
- Hide tables and internal types. The contract is the encapsulation boundary.
- Design for reuse across channels, then let orchestration specialize the flow.
- Put authentication, authorization, and validation in the contract’s rules, not only in one caller.
- Prefer idempotent operations where clients will retry, and make the transactional boundary obvious.
- Separate what is stable from what changes often, so volatile details do not force a new contract every week.
- The service must be findable, measurable (latency, errors, availability), and versioned so old callers survive.
- Name the schema, cache only where staleness is acceptable, and follow the standards the organization already uses.

**Examples:**

`POST /orders` accepts a cart id and a payment token and returns an order id or a typed error. It does not accept a raw SQL fragment, and repeating the same idempotency key does not create a second order.
</details>

<details>
<summary>Explain how versioning of services is handled in a SOA environment.</summary>

### Explain how versioning of services is handled in a SOA environment.

**Definition:**

Publish a new contract version when a change would break current clients, and keep the old version until those clients move.

**How it works:**

- URL versioning puts the version in the path (`/v1/orders`, `/v2/orders`). It is obvious and easy to route. The cost is more paths to keep alive.
- Header versioning puts it in the request, for example `Accept: application/vnd.orders+json;version=2`. URLs stay stable. Caches and some clients handle a custom media type poorly.
- Semantic versions tell clients whether a change is additive (`1.1.0`) or breaking (`2.0.0`).
- Additive fields that old clients ignore can stay on the same major version. Removing or retyping a field needs a new one.
- A gateway can send each version to the right deployment so callers keep one host name.
- Retire a version only after you can see that no traffic still uses it.

**Examples:**

`GET /v1/orders/1` still returns `total`. `GET /v2/orders/1` splits that into `net` and `tax`. The gateway routes `/v1` to the old process until the last client is gone.
</details>

<details>
<summary>Describe a scenario in which you might opt for synchronous communication over asynchronous in SOA.</summary>

### Describe a scenario in which you might opt for synchronous communication over asynchronous in SOA.

**Definition:**

Use a synchronous call when the caller cannot take the next step without the result. A queue would leave that step hanging on a later event.

**How it works:**

- Checkout must know immediately whether the card was authorized, so the screen can confirm or ask for another card.
- A uniqueness check before creating an account has the same shape: the answer decides whether to commit.
- The steps have a required order, and the error has to be shown to the user in that request.
- You accept the cost: the caller now depends on the callee’s availability and latency, and a slow dependency slows the request.
- Work that can finish later (email, analytics, warehouse pick) stays on a queue.

**Examples:**

```text
POST /checkout
  -> POST /payments/authorize   # wait
  <- { "status": "approved" }
  -> create order
```

If authorize times out, checkout returns an error. It does not tell the customer the order exists and hope a message arrives later.
</details>

<details>
<summary>What are some of the common data formats used for service communication in SOA?</summary>

### What are some of the common data formats used for service communication in SOA?

**Definition:**

The contract names the bytes on the wire so every consumer parses the same structure.

**How it works:**

- SOAP services exchange XML envelopes, often with a WSDL description of operations and types.
- Resource-style HTTP APIs more often use JSON: text, easy to read, loosely typed unless you add a schema.
- Binary schemas such as Protocol Buffers or Avro show up when message size and strict evolution rules matter.
- XML still fits when the enterprise already shares XSD contracts or needs namespaces and strict validation.
- Pick one format per contract. Mixing them at one operation forces every caller to guess.

**Examples:**

A partner gateway speaks SOAP with an XSD. An internal order API speaks JSON documented by OpenAPI. A high-volume stock feed uses protobuf so the same message stays small and typed.
</details>

<details>
<summary>Provide an example of how you would refactor a monolithic application into a SOA-based architecture.</summary>

### Provide an example of how you would refactor a monolithic application into a SOA-based architecture.

**Definition:**

Cut the monolith along business capabilities, publish each contract, and move one service out at a time with its data. Leave a facade so the old process still looks the same to callers.

**How it works:**

- Find modules with their own reason to change. In a shop that is often catalog, orders, users, and payments.
- Decide which calls must return now (checkout to payments) and which can be events (email, analytics).
- Write the interface and the error cases before moving code.
- Extract the service, move the tables it owns, and point the monolith at the new endpoint through a thin adapter.
- Register the endpoint, then add authentication and metrics on that boundary.
- Repeat. Do not split every class in one release.
- You gain separate deploys and the freedom to scale payments alone. You take on network failures, and a database transaction that used to be local becomes a saga.

**Examples:**

Week one: `PaymentService` moves out. The monolith’s `charge()` becomes an HTTP call with an idempotency key. Orders and catalog stay in-process. Week two: catalog moves, and the monolith only keeps the checkout facade.
</details>

### Implementing SOA

<details>
<summary>What is an Enterprise Service Bus (ESB) and how does it support SOA?</summary>

### What is an Enterprise Service Bus (ESB) and how does it support SOA?

**Definition:**

An enterprise service bus is middleware that routes, transforms, and mediates messages so services do not each integrate with every peer.

**How it works:**

- A messaging engine moves messages. Adapters speak each system’s protocol.
- A mediation layer transforms formats, maps fields, and validates messages.
- A registry on the bus records which services exist. Routing can depend on content.
- The bus can enforce security and other policy in one place, host a queue, and run an orchestration flow.
- Services talk to the bus instead of holding every peer’s address and schema.
- The risk is the same centralization: if business decisions pile up inside the bus, it becomes a bottleneck and a single place every change must touch.

**Examples:**

A warehouse system emits a fixed-width file message. The bus turns it into the order service’s JSON, checks the schema, and delivers it. Neither system links to the other’s library.
</details>

<details>
<summary>How would you handle transaction management in a SOA system?</summary>

### How would you handle transaction management in a SOA system?

**Definition:**

Inside one service, a local database transaction can still be atomic. Across services, a distributed two-phase commit is brittle, so the usual design is a sequence of local commits plus an explicit undo.

**How it works:**

- A local transaction covers one service’s own tables. Isolation and durability stay inside that database.
- Propagating one ACID transaction across the network ties every participant’s locks together and fails badly when one of them is down.
- Compensation: each step commits locally, and a failure runs an undo such as a refund or a stock release. The undo is a normal operation, not a database rollback.
- A saga records which steps succeeded and runs the matching compensations in reverse. Orchestration can own that record, or each service can react to a failure event.
- Retries must be idempotent, and timeouts must be explicit, because the network can deliver a success after the caller gave up.
- Isolation is weaker than in one database: another caller can see stock reserved before payment finishes. The flow has to allow that intermediate state.

**Examples:**

Reserve stock (commit), then charge the card. If the charge is declined, call `releaseStock` with the same reservation id. A second release is a no-op. There is no transaction that spans the warehouse database and the card processor.
</details>

<details>
<summary>What strategies would you use to ensure high availability in a SOA deployment?</summary>

### What strategies would you use to ensure high availability in a SOA deployment?

**Definition:**

Keep each service able to answer during a failure by replicating it, and decide explicitly what happens when the network between services splits.

**How it works:**

- SOAP is a messaging framework for a decentralized, distributed environment. A service that exists on only one machine is down when that machine is down.
- Brewer: during a partition you either stop updates on one side or you accept divergence and compensate later. Retrying with no deadline chooses consistency and the caller waits.
- The Reactive Manifesto gets resilience from replication, containment, isolation, and delegation. The client of a failed service should not be the component that repairs it.
- Publish the contract in WSDL so a second copy of the service still offers the same operations. Clients bind to the contract, not to one host name baked into code.
- Bound the queue in front of the service. Reactive Streams exists so a fast caller cannot force an unbounded buffer on a slow service.

**Sources:**

- https://www.w3.org/TR/soap12-part1/
- https://www.w3.org/TR/wsdl20/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://www.reactivemanifesto.org/
- https://www.reactive-streams.org/
</details>

<details>
<summary>What is WS-Security and how does it relate to SOA?</summary>

### What is WS-Security and how does it relate to SOA?

**Definition:**

WS-Security is an OASIS standard that adds integrity and confidentiality to SOAP messages and binds security tokens to that content.

**How it works:**

- The specification enhances SOAP messaging. SOAP itself is the envelope. WS-Security is how that envelope carries proof and protection.
- It is built to fit many security models and encryption technologies. It does not pick one token format. A message can carry one token for identity and another for a business certification.
- It also describes binary security tokens, a framework for XML tokens, and opaque encrypted keys.
- In a service-oriented system the caller and the service are different parties. The message has to stay protected after it leaves the caller's process, which is what message-level integrity and confidentiality are for.
- This is not a procedure for breaking a token or a signature. Verification belongs to an implementation of the standard.

**Sources:**

- https://docs.oasis-open.org/wss-m/wss/v1.1.1/os/wss-SOAPMessageSecurity-v1.1.1-os.html
- https://www.w3.org/TR/soap12-part1/
</details>

### SOA Standards and Protocols

<details>
<summary>What are the benefits of using standard protocols like SOAP or REST in SOA?</summary>

### What are the benefits of using standard protocols like SOAP or REST in SOA?

**Definition:**

A standard contract lets a caller and a service evolve separately. SOAP defines a message that can travel over more than one underlying protocol.

**How it works:**

- SOAP 1.2 is a lightweight protocol for exchanging structured information in a decentralized, distributed environment. Part 1 defines an extensible messaging framework and a message construct.
- Because the message is XML and the framework is extensible, a new header such as WS-Security can be added without inventing a new envelope.
- WSDL 2.0 is the XML language that describes what the service offers, from an abstract model. Callers generate or check clients against that description instead of a private binary layout.
- The same description can be honored by a second implementation. That is the SOA point: the consumer depends on the contract, not on one team's library.
- A timeout and a bounded queue still matter. A standard envelope does not by itself keep a slow service from being overwhelmed.

**Sources:**

- https://www.w3.org/TR/soap12-part1/
- https://www.w3.org/TR/wsdl20/
- https://docs.oasis-open.org/wss-m/wss/v1.1.1/os/wss-SOAPMessageSecurity-v1.1.1-os.html
</details>

<details>
<summary>Explain the role of WSDL in SOA.</summary>

### Explain the role of WSDL in SOA.

**Definition:**

WSDL 2.0 is an XML language for describing a Web service from an abstract model of what the service offers.

**How it works:**

- The specification defines the core language and the conformance criteria for documents in that language.
- The description is the contract. Operations, messages, and where the service is reached live in that document, so a caller does not read the service's source.
- SOAP carries the message. WSDL describes the service. WS-Security protects the message. They are three different documents with three jobs.
- A second deployment of the same description is still the same service from the caller's point of view. That is how you put a replica behind one contract.
- Changing the description is a contract change. Callers that were generated from the old document need a deliberate update, not a silent drift.

**Sources:**

- https://www.w3.org/TR/wsdl20/
- https://www.w3.org/TR/soap12-part1/
- https://docs.oasis-open.org/wss-m/wss/v1.1.1/os/wss-SOAPMessageSecurity-v1.1.1-os.html
</details>

<details>
<summary>How does UDDI facilitate service discovery in SOA?</summary>

### How does UDDI facilitate service discovery in SOA?

**Definition:**

Discovery is how a caller finds a service description. The description it needs, once found, is a WSDL document of what the service offers.

**How it works:**

- WSDL 2.0 is the XML language for that description, based on an abstract model of the service. A registry's job is to point a caller at such a description rather than at a hard-coded URL in source.
- SOAP then exchanges the message the description named. The registry does not replace the message framework.
- WS-Security still applies after discovery. Finding a service is not the same as trusting its message. Tokens travel with the SOAP content.
- Treat a discovered endpoint as replaceable. Availability comes from another replica that honors the same WSDL, not from caching one host forever.
- A discovered description can be stale. Compare the contract you call with the contract you generated against, the same way a partition needs an explicit recovery step.

**Sources:**

- https://www.w3.org/TR/wsdl20/
- https://www.w3.org/TR/soap12-part1/
- https://docs.oasis-open.org/wss-m/wss/v1.1.1/os/wss-SOAPMessageSecurity-v1.1.1-os.html
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>

<details>
<summary>What is the significance of OASIS in the context of SOA?</summary>

### What is the significance of OASIS in the context of SOA?

**Definition:**

OASIS publishes standards that sit on top of the SOAP message, including WS-Security for integrity, confidentiality, and tokens.

**How it works:**

- The WS-Security 1.1.1 document is an OASIS standard. It enhances SOAP messaging rather than replacing it.
- That split is the point of a standards body here: the message framework is W3C SOAP, the service description is W3C WSDL, and message security is an OASIS specification. A service can adopt them independently.
- The security specification is extensible about token formats. OASIS did not freeze SOA to one identity product.
- Teams still have to implement the standard. Publishing it does not deploy it. Encryption and signatures exist only if the endpoint follows the specification.
- Governance in a SOA program is largely deciding which of these published contracts are mandatory.

**Sources:**

- https://docs.oasis-open.org/wss-m/wss/v1.1.1/os/wss-SOAPMessageSecurity-v1.1.1-os.html
- https://www.w3.org/TR/soap12-part1/
- https://www.w3.org/TR/wsdl20/
</details>

### Securing SOA Applications

<details>
<summary>What are the main security concerns in a SOA environment?</summary>

### What are the main security concerns in a SOA environment?

**Definition:**

Messages cross a trust boundary. The concerns are integrity, confidentiality, and which token proves the caller, plus availability when a dependency fails.

**How it works:**

- WS-Security names integrity and confidentiality of the SOAP message, and a way to associate security tokens with that content. No single token type is required.
- A token in the message is not protection if the private key or the shared secret leaks. RFC 8017's RSA public key is the modulus and exponent you can publish. The private exponent stays with the holder.
- The description in WSDL tells callers the shape of the service. It does not authenticate them. Discovery and authorization are separate steps.
- A compromised or overloaded dependency is also a security failure. Isolation and a bounded queue keep one bad service from exhausting the caller.
- Do not invent a padding or a token format. Use the scheme the specification names.

**Sources:**

- https://docs.oasis-open.org/wss-m/wss/v1.1.1/os/wss-SOAPMessageSecurity-v1.1.1-os.html
- https://www.rfc-editor.org/rfc/rfc8017#section-3.1
- https://www.w3.org/TR/wsdl20/
- https://www.reactive-streams.org/
</details>

<details>
<summary>How would you approach identity management in SOA?</summary>

### How would you approach identity management in SOA?

**Definition:**

Put proof of identity in the message as a security token, and keep the private material that backs that token with the caller, not in the WSDL.

**How it works:**

- WS-Security associates tokens with message content and allows more than one format. One token can prove identity and another can prove a business certification.
- The service checks the token against the rules of that format. The WSDL describes operations. It is not the identity store.
- SOAP's extensible header is where this proof rides, so intermediaries can see a header without each service inventing a side channel.
- Replica services must accept the same token formats. Otherwise failover changes who the caller is allowed to be.
- Rotate and store secrets as secrets. An RSA private exponent is not part of the public key you publish.

**Sources:**

- https://docs.oasis-open.org/wss-m/wss/v1.1.1/os/wss-SOAPMessageSecurity-v1.1.1-os.html
- https://www.w3.org/TR/soap12-part1/
- https://www.w3.org/TR/wsdl20/
- https://www.rfc-editor.org/rfc/rfc8017#section-3.1
</details>

<details>
<summary>Can you explain the concept of security tokens in SOA?</summary>

### Can you explain the concept of security tokens in SOA?

**Definition:**

A security token is a claim carried with the SOAP message. WS-Security binds tokens to content and does not require one token format.

**How it works:**

- The specification is extensible: multiple token formats are allowed. The text's example is one format for proof of identity and another for proof of a business certification.
- It describes how to encode binary security tokens, a framework for XML-based tokens, and how to include opaque encrypted keys.
- The token supports integrity and confidentiality of the message. It is not a replacement for the SOAP envelope.
- Receivers must understand the format they accept. An unknown token is not silently trusted.
- Publishing the matching public key, such as an RSA modulus and exponent, lets others check a signature. The private exponent stays private.

**Examples:**

```text
SOAP envelope
  WS-Security header
    token: proof of identity
    token: business certification
  body: the operation
```

**Sources:**

- https://docs.oasis-open.org/wss-m/wss/v1.1.1/os/wss-SOAPMessageSecurity-v1.1.1-os.html
- https://www.rfc-editor.org/rfc/rfc8017#section-3.1
</details>

### SOA Testing

<details>
<summary>Describe your approach to testing services in SOA.</summary>

### Describe your approach to testing services in SOA.

**Definition:**

Test the contract in the WSDL and the message on the wire, including the security header, not only the code inside one service.

**How it works:**

- WSDL is the abstract model of what the service offers. A test that calls an operation the description does not list is testing a private path.
- SOAP tests should send a real envelope. A unit test of a function with no envelope misses header handling.
- Include a WS-Security header case: missing token, token present, and a body that must fail integrity if the protected content changes. Do not describe how to forge a token. The test uses keys you own.
- Contract tests belong at the boundary because the caller is another team. A green internal test does not prove their generated client still matches.
- Add a timeout test. A dependency that never returns is the availability failure Brewer describes, and the caller must stop waiting.

**Sources:**

- https://www.w3.org/TR/wsdl20/
- https://www.w3.org/TR/soap12-part1/
- https://docs.oasis-open.org/wss-m/wss/v1.1.1/os/wss-SOAPMessageSecurity-v1.1.1-os.html
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>

<details>
<summary>What are the challenges of testing in a SOA-based system compared to a monolithic one?</summary>

### What are the challenges of testing in a SOA-based system compared to a monolithic one?

**Definition:**

A monolith calls functions in one process. A SOA system calls contracts over a network, so tests must include the message, the token, and the failure of the other side.

**How it works:**

- WSDL and SOAP are the seam. A refactor inside the service is invisible if the description and the envelope stay the same. A test that only loads classes will not see a broken envelope.
- The other service may be down, slow, or partitioned. Brewer's choice appears in the test: after a deadline, does the caller fail or does it proceed with a stale result?
- Security tokens are part of the message. A test identity has to be a token format the service accepts, not a thread-local user object.
- Versions drift. Two teams can deploy two WSDL revisions. The test has to pin the description it was written against.
- Shared mutable fixtures are a poor fit. One test's data is another service's database. Isolate the contract you own and stub the description you call.

**Sources:**

- https://www.w3.org/TR/wsdl20/
- https://www.w3.org/TR/soap12-part1/
- https://docs.oasis-open.org/wss-m/wss/v1.1.1/os/wss-SOAPMessageSecurity-v1.1.1-os.html
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>

<details>
<summary>How would you perform load testing for an SOA service?</summary>

### How would you perform load testing for an SOA service?

**Definition:**

Drive the SOAP operation from the WSDL at a rising rate and watch whether the service stays responsive or the queue grows without a bound.

**How it works:**

- The contract under load is the same description callers use. Hit the operations WSDL lists, with a valid WS-Security header, or you are measuring a path production will reject.
- Reactive Streams states the failure you are looking for: a fast source must not force an arbitrary buffer. If memory grows with the send rate, back-pressure or a limit is missing.
- Record the deadline. Brewer treats a timeout as the moment you either cancel or risk inconsistency. A load test with no deadline hides that choice.
- Scale the replica count only after one replica's limit is known. More copies do not help if every copy waits on the same unlocked dependency.
- Keep the security check on. A load test that skips token validation measures a service you will not deploy.

**Sources:**

- https://www.w3.org/TR/wsdl20/
- https://docs.oasis-open.org/wss-m/wss/v1.1.1/os/wss-SOAPMessageSecurity-v1.1.1-os.html
- https://www.reactive-streams.org/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://www.reactivemanifesto.org/
</details>

### Performance and Scalability

<details>
<summary>How would you monitor the performance of services in SOA?</summary>

### How would you monitor the performance of services in SOA?

**Definition:**

Watch the time to answer, the queue depth, and token failures. The manifesto treats a timely response as the way you notice a problem.

**How it works:**

- Responsiveness is the signal. A service that stops answering, or answers past its deadline, is the availability event. CPU inside one process is a clue, not the definition.
- Queue depth tells you whether back-pressure is working. Reactive Streams forbids an arbitrary buffer. A depth that only grows is the bug.
- Count WS-Security failures separately from application faults. A spike in rejected tokens is an identity problem. A spike in slow successes is capacity or a dependency.
- During a partition, not every caller will see the failure at the same moment. Brewer notes there is no single global view of the cut. Dashboards per caller still matter.
- Alert on the contract: the operation in the WSDL, not on a private method name.

**Sources:**

- https://www.reactivemanifesto.org/
- https://www.reactive-streams.org/
- https://docs.oasis-open.org/wss-m/wss/v1.1.1/os/wss-SOAPMessageSecurity-v1.1.1-os.html
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://www.w3.org/TR/wsdl20/
</details>

<details>
<summary>Describe a strategy to scale a service in a SOA system.</summary>

### Describe a strategy to scale a service in a SOA system.

**Definition:**

Add replicas that honor the same WSDL, and keep each replica from taking unbounded work. Move to more than one site only with an explicit partition plan.

**How it works:**

- Elasticity in the manifesto is staying responsive as load changes, by sharding or replicating and avoiding a central bottleneck.
- Every replica must accept the same SOAP contract and the same token formats. Scaling a binary that drifted from the WSDL splits the service in two.
- Cap concurrency at the edge. flatMap-style fan-out without a limit, or an unlimited SOAP thread per call, exhausts the pool before CPU is the issue.
- A shared dependency that cannot scale becomes the bottleneck. Isolate it and give it a deadline.
- Across regions, Brewer still applies. Two sites that both take writes while they cannot talk will diverge. Pick a writer or plan compensation.

**Sources:**

- https://www.reactivemanifesto.org/
- https://www.w3.org/TR/wsdl20/
- https://docs.oasis-open.org/wss-m/wss/v1.1.1/os/wss-SOAPMessageSecurity-v1.1.1-os.html
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://www.reactive-streams.org/
</details>

<details>
<summary>How do you manage service dependencies to avoid performance bottlenecks in SOA?</summary>

### How do you manage service dependencies to avoid performance bottlenecks in SOA?

**Definition:**

A dependency is another SOAP contract. Call it with a deadline, a concurrency cap, and a fallback that does not pretend the latest write succeeded.

**How it works:**

- SOAP over a network can stall. Reactive Streams requires the caller not to buffer an arbitrary amount while it waits.
- The manifesto isolates the failure. A dead dependency is contained. The caller stays responsive by refusing or by using a clearly stale copy, which is Brewer's availability-versus-consistency choice.
- Do not let every inbound request open an unbounded number of outbound calls. A cap is the same idea as flatMap's concurrency argument.
- WSDL tells you the operations you depend on. A hidden call that is not in the description will not show up in the contract test until production.
- If two services call each other, a slowdown cycles. Break the cycle with a queue that has a bound and a token that does not have to be re-checked in a loop.

**Sources:**

- https://www.w3.org/TR/soap12-part1/
- https://www.reactive-streams.org/
- https://www.reactivemanifesto.org/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://www.w3.org/TR/wsdl20/
</details>

### SOA Governance

<details>
<summary>What does governance mean in the context of SOA?</summary>

### What does governance mean in the context of SOA?

**Definition:**

Governance is the decision about which contracts are mandatory: the WSDL you publish, the SOAP envelope you accept, and the token formats you trust.

**How it works:**

- WSDL is the service description. Governance says who may change it and how callers learn about the change.
- WS-Security is optional until a policy requires it. Governance makes integrity, confidentiality, and an accepted token format part of the contract, not a local habit.
- OASIS and W3C publish the documents. Governance is the local rule that those documents are what this estate implements.
- Without that rule, two teams invent two envelopes and discovery cannot find a single description.
- Governance also includes the availability rule: a deadline, a replica policy, and what a partition is allowed to do. Brewer's detect-limit-recover is a policy, not only a theorem.

**Sources:**

- https://www.w3.org/TR/wsdl20/
- https://docs.oasis-open.org/wss-m/wss/v1.1.1/os/wss-SOAPMessageSecurity-v1.1.1-os.html
- https://www.w3.org/TR/soap12-part1/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>

<details>
<summary>Describe a policy you might enforce as part of SOA governance.</summary>

### Describe a policy you might enforce as part of SOA governance.

**Definition:**

Require every accepted SOAP message to carry a WS-Security token in an allowed format, and reject the message when integrity does not hold.

**How it works:**

- The specification's job is integrity, confidentiality, and associating tokens with content. A policy names which of those this estate turns on.
- No specific token type is required by the standard, so the policy must say which formats this service accepts. An unknown format is rejected.
- The matching WSDL is published only for operations that enforce the policy. A back door that skips the header is outside the contract.
- Pair it with a deadline. A message that is authentic but whose handler never returns is still an availability failure.
- The policy is checked in tests with keys you control. It is not a guide to forging tokens.

**Sources:**

- https://docs.oasis-open.org/wss-m/wss/v1.1.1/os/wss-SOAPMessageSecurity-v1.1.1-os.html
- https://www.w3.org/TR/wsdl20/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>

<details>
<summary>Can you explain the purpose of a service level agreement (SLA) in a SOA context?</summary>

### Can you explain the purpose of a service level agreement (SLA) in a SOA context?

**Definition:**

An SLA states the response bound callers can expect from a described operation, and what the service will do when it cannot meet that bound.

**How it works:**

- The manifesto treats a timely response as the core of usefulness, and a consistent upper bound as what makes failures detectable.
- WSDL names the operation. The SLA names the time and the success rate for that operation. A contract with no bound cannot be monitored.
- Brewer's practical partition is a timeout. The SLA is that timeout made into a promise: past it, the call is failed or degraded on purpose, not retried forever.
- The promise has to survive a replica. Every copy that honors the WSDL is inside the same SLA, or callers see a different service.
- Back-pressure is how you keep the promise. Accepting unlimited SOAP calls and then missing the bound is not an SLA. It is an unbounded queue.

**Sources:**

- https://www.reactivemanifesto.org/
- https://www.w3.org/TR/wsdl20/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://www.reactive-streams.org/
</details>

### Troubleshooting and Maintenance

<details>
<summary>What steps would you take to troubleshoot a failing service in a SOA architecture?</summary>

### What steps would you take to troubleshoot a failing service in a SOA architecture?

**Definition:**

Separate a bad contract, a rejected token, a missed deadline, and a down dependency. They look alike if you only watch one process.

**How it works:**

- Check the WSDL the caller used against the WSDL the service deployed. A drift there fails before business code runs.
- Check the WS-Security header. Integrity and token format failures are specified separately from a fault inside the operation.
- If the call passes the deadline, treat it as Brewer's partition decision. See whether the caller cancelled or kept going. Look at the dependency the same way.
- Watch queue depth. An arbitrary buffer means the service accepted more SOAP work than it can finish. That is the Reactive Streams failure mode.
- A replica that fails alone is containment working. A failure of every replica together points at the shared dependency or at the contract they all share.

**Sources:**

- https://www.w3.org/TR/wsdl20/
- https://docs.oasis-open.org/wss-m/wss/v1.1.1/os/wss-SOAPMessageSecurity-v1.1.1-os.html
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
- https://www.reactive-streams.org/
- https://www.reactivemanifesto.org/
</details>

<details>
<summary>How do you manage and maintain services throughout their lifecycle in a SOA infrastructure?</summary>

### How do you manage and maintain services throughout their lifecycle in a SOA infrastructure?

**Definition:**

A service is born as a WSDL, secured by a token policy, run as replaceable replicas, and retired by withdrawing the description callers discover.

**How it works:**

- Design the description first. WSDL 2.0 is the abstract model of what is offered. Implementation follows that document.
- Before traffic, require the WS-Security policy: integrity, confidentiality as needed, and the token formats you accept.
- In production, keep replicas and a deadline. Monitor responsiveness and queue depth. A partition plan says who may write.
- A compatible change stays inside the published description. An incompatible change is a new description, and old callers keep the old one until they move.
- Retirement removes the description from discovery and drains callers. Deleting one host while the WSDL still advertises it only creates timeouts.

**Sources:**

- https://www.w3.org/TR/wsdl20/
- https://docs.oasis-open.org/wss-m/wss/v1.1.1/os/wss-SOAPMessageSecurity-v1.1.1-os.html
- https://www.reactivemanifesto.org/
- https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed/
</details>
