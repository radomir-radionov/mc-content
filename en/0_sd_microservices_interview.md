# Microservices — Interview

Interview answers in this file are filled.

[← Back to hub](./0_system_design.md)

### Related docs

- [Microservices — Interview](./0_sd_microservices_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Microservices](https://devinterview.io/questions/software-architecture-and-system-design/microservices-interview-questions/).

### Microservices Fundamentals

<details>
<summary>What is a microservice and how does it differ from a monolithic architecture?</summary>

### What is a microservice and how does it differ from a monolithic architecture?

**Definition:**

Microservices and monolithic architecture are two distinct software design paradigms, each with its unique traits.

A monolithic architecture consolidates all software components into a single program, whereas a microservices architecture divides the application into separate, self-contained services.

Microservices offer several advantages but also have their own challenges, requiring careful consideration in the software design process.

**How it works:**

#### Key Differences

Decomposition: Monolithic applications are not easily separable, housing all functionality in a single codebase. Microservices are modular, each responsible for a specific set of tasks.

Deployment Unit: The entire monolithic application is packaged and deployed as a single unit. In contrast, microservices are deployed individually.

Communication: In a monolith, modules communicate through in-process calls. Microservices use standard communication protocols like HTTP/REST or message brokers.

Data Management: A monolith typically has a single database, whereas microservices may use multiple databases.

Scaling: Monoliths scale by replicating the entire application. Microservices enable fine-grained scaling, allowing specific parts to scale independently.

Technology Stack: While a monolithic app often uses a single technology stack, microservices can employ a diverse set of technologies.

Development Team: Monoliths can be developed by a single team, whereas microservices are often the domain of distributed teams.

**When to use:**

#### When to Use Microservices

Microservices are advantageous for certain types of projects:

Complex Systems: They are beneficial when developing complex, business-critical applications where modularity is crucial.

Scalability: If you anticipate varying scaling needs across different functions or services, microservices might be the best pick.

Technology Diversification: When specific functions are better suited to certain technologies or when you want to use the best tools for unique tasks.

Autonomous Teams: For bigger organizations with multiple teams that need to work independently.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/microservices-interview-questions/
</details>

<details>
<summary>Can you describe the principles behind the microservices architecture?</summary>

### Can you describe the principles behind the microservices architecture?

**Definition:**

Microservices is an architectural style that structures an application as a collection of small, loosely coupled services. Each service is self-contained, focused on a specific business goal, and can be developed, deployed, and maintained independently.

**How it works:**

#### Core Principles of Microservices

#### Codebase & Infrastructure as a Service

Each microservice manages its own codebase and data storage. It uses its own independent infrastructure, ranging from the number of virtual machines to persistence layers, messaging systems, or even data models.

#### Antifragility

Microservices, instead of resisting failure, respond to it favorably. They self-adapt and become more resilient in the face of breakdowns.

#### Ownership

Development teams are responsible for the entire lifecycle of their respective microservices - from development and testing to deployment, updates, and scaling.

#### Design for Failure

Microservices are built to anticipate and handle failures at various levels, ensuring the graceful degradation of the system.

#### Decentralization

Services are autonomous, making their own decisions without requiring overarching governance. This agility permits independent deployments and ensures that changes in one service do not disrupt others.

#### Built Around Business Capability

Each service is crafted to provide specific and well-defined business capabilities. This focus increases development speed and makes it easier to comprehend and maintain the system.

#### Service Coupling

Services are related through well-defined contracts, mainly acting as providers of specific functionalities. This reduces dependencies and integration challenges.

#### Directed Transparency

Each service exposes a well-defined API, sharing only the necessary information. Teams can independently choose the best technology stack, avoiding the need for a one-size-fits-all solution.

#### Infrastructure Automation

Deployments, scaling, and configuration undergo automation, preserving development velocity and freeing teams from manual, error-prone tasks.

#### Organizational Alignment

Teams are structured around services, aligning with Conway's Law to support the Microservices architecture and promote efficiency.

#### Continuous Small Revisions

Services are frequently and iteratively improved, aiming for continual enhancement over major, infrequent overhauls.

#### Discoverability

Services make their features, capabilities, and interfaces discoverable via well-documented APIs, fostering an environment of interoperability.

#### The "DevOps" Connection

The DevOps method for software development merges software development (Dev) with software operation (Ops). It focuses on shortening the system's software development life cycle and providing consistent delivery. The "you build it, you run it" approach, where developers are also responsible for operating their software in production, is often associated with both Microservices and DevOps.

#### Code Example: Loan Approval Microservice

Here is the sample Java code:

**Examples:**

```java
@RestController
@RequestMapping("/loan")
public class LoanService {
    @Autowired
    private CreditCheckService creditCheckService;

    @PostMapping("/apply")
    public ResponseEntity<String> applyForLoan(@RequestBody Customer customer) {
        if(creditCheckService.isEligible(customer))
            return ResponseEntity.ok("Congratulations! Your loan is approved.");
        else
            return ResponseEntity.status(HttpStatus.FORBIDDEN).body("We regret to inform you that your credit rating did not meet our criteria.");
    }
}
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/microservices-interview-questions/
</details>

<details>
<summary>What are the main benefits of using microservices?</summary>

### What are the main benefits of using microservices?

**Definition:**

Let's look at the main advantages of using microservices:

**How it works:**

#### Key Benefits

#### 1. Scalability

Each microservice can be scaled independently, which is particularly valuable in dynamic, going-viral, or resource-intensive scenarios.

#### 2. Flexibility

Decoupling services means one service's issues or updates generally won't affect others, promoting agility.

#### 3. Technology Diversity

Different services can be built using varied languages or frameworks. While this adds some complexity, it allows for best-tool-for-the-job selection.

#### 4. Improved Fault Tolerance

If a microservice fails, it ideally doesn't bring down the entire system, making the system more resilient.

#### 5. Agile Development

Microservices mesh well with Agile, enabling teams to iterate independently, ship updates faster, and adapt to changing requirements more swiftly.

#### 6. Easier Maintenance

No more unwieldy, monolithic codebases to navigate. With microservices, teams can focus on smaller, specific codebases, thereby enabling more targeted maintenance.

#### 7. Tailored Security Measures

Security policies and mechanisms can be tailored to individual services, potentially reducing the overall attack surface.

#### 8. Improved Team Dynamics

Thanks to reduced codebase ownership and the interoperability of services, smaller, focused teams can thrive and communicate more efficiently.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/microservices-interview-questions/
</details>

<details>
<summary>What are some of the challenges you might face when designing a microservices architecture?</summary>

### What are some of the challenges you might face when designing a microservices architecture?

**Definition:**

When designing a microservices architecture, you are likely to encounter the following challenges:

**How it works:**

#### Data Management

Database Per Microservice: Ensuring that each microservice has its own database can be logistically complex. Data relationships and consistency might be hard to maintain.

Eventual Consistency: Different microservices could be using data that might not be instantly synchronized. Dealing with eventual consistency can raise complications in some scenarios.

#### Service Communication

Service Synchronization: Maintaining a synchronous communication between numerous services can result in a more tightly coupled and less scalable architecture.

Service Discovery: As the number of services grows, discovering and properly routing requests to the appropriate service becomes more challenging.

#### Security and Access Control

Decentralized Security: Implementing consistent security measures, such as access control and authentication, across all microservices can be intricate.

Externalized Authorization: When security-related decisions are taken outside the service, coherent and efficient integration is crucial.

#### Infrastructure Management

Server Deployment: Managing numerous server deployments entails additional overhead and might increase the risk of discrepancies among them.

Monitoring Complexity: With each microservice operating independently, gauging the collective functionality of the system necessitates more extensive monitoring capabilities.

#### Business Logic Distribution

Domain and Data Coupling: Microservices, especially those representing different business domains, may find it challenging to process complex business transactions that require data and logic from several services.

Cross-Cutting Concerns Duplication: Ensuring a uniform application of cross-cutting concerns like logging or caching across microservices is non-trivial.

#### Scalability

Fine-Grained Scalability: While microservices allow selective scale-up, guaranteeing uniform performance across varying scales might be troublesome.

Service Bottlenecks: Certain services might be hit more frequently, potentially becoming bottlenecks.

#### Development and Testing

Integration Testing: Interactions between numerous microservices in real-world scenarios might be challenging to replicate in testing environments.

#### Consistency and Atomicity

System-Wide Transactions: Ensuring atomic operations across multiple microservices is complex and might conflict with certain microservice principles.

Data Integrity: Without a centralized database, governing data integrity could be more intricate, especially for related sets of data that multiple microservices handle.

#### Challenges in Updating and Versioning

Deployment Orchestration: Coordinated updates or rollbacks, particularly in hybrid environments, can present difficulties.

Version Compatibility: Assuring that multiple, potentially differently-versioned microservices can still work together smoothly.

#### Team Structure and Organizational Alignment

Siloed Teams: Without a unified architectural vision or seamless communication, different teams developing diverse microservices might make decisions that are not entirely compatible with the overall system.

Documentation and Onboarding: With an extensive number of microservices, their functionalities, interfaces, and usage need to be well-documented for efficient onboarding and upkeep.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/microservices-interview-questions/
</details>

<details>
<summary>How do microservices communicate with each other?</summary>

### How do microservices communicate with each other?

**Definition:**

Microservices often work together, and they need efficient communication mechanisms...

**How it works:**

#### Communication Patterns

Synchronous: Web services and RESTful APIs synchronize requests and responses. They are simpler to implement but can lead to tighter coupling between services. For dynamic traffic or workflow-specific requests, this is a suitable choice.

Asynchronous: Even with service unavailability or high loads, queues lead to the delivery of messages. The services do not communicate or interact beyond their immediate responsibilities and workloads. For unpredictable or lengthy processes, use asynchronous communication.

Data Streaming: For continuous data needs or applications that work with high-frequency data, such as stock prices or real-time analytics, this method is highly effective. Kafka or AWS Kinesis are examples of this pattern.

#### Inter-Service Communication Methods

RESTful APIs: Simple and clean, they utilize HTTP's request-response mechanism. Ideal for stateless, cacheable, and stateless resource interactions.

Messaging: Deploys a message broker whereby services use HTTP or a messaging protocol (like AMQP or MQTT). This approach offers decoupling, and the broker ensures message delivery. Common tools include RabbitMQ, Apache Kafka, or AWS SQS.

Service Mesh and Sidecars: A sidecar proxy, typically running in a container, works alongside each service. They assist in monitoring, load balancing, and authorization.

Remote Procedure Call (RPC): It involves a client and server where the client sends requests to the server with a defined set of parameters. They're efficient but not perfectly decoupled.

Event-Based Communication: Here, services interact by producing and consuming events. A service can publish events into a shared event bus, and other services can subscribe to these events and act accordingly. This pattern supports decoupling and scalability. Common tools include Apache Kafka, AWS SNS, and GCP Pub/Sub.

Database per Service: It involves each microservice owning and managing its database. If a service A needs data from service B, it uses B's API to retrieve or manipulate data.

API Gateway: Acts as a single entry point for services and consumers. Netscaler, HAProxy, and Kong are popular API Gateway tools.

#### Code Example: REST API

Here is the Python code:

Here is the Python code:

# Import the generated server and client classes.

What is the best way to Implement Microservices?
Ease of Development: If you need to onboard a large number of developers or have strict timelines, RESTful APIs are often easier to work with.

Performance: gRPC and other RPC approaches are superior to RESTful APIs in terms of speed, making them ideal when performance is paramount.

Type Safety: gRPC, due to its use of Protocol Buffers, ensures better type safety at the cost of being slightly less human-readable when compared to RESTful JSON payloads.

Portability: RESTful APIs, being HTTP-based, are more portable across platforms and languages. On the other hand, gRPC is tailored more towards microservices built with Protobufs.

**Examples:**

```python
import requests

# Make a GET request to receive a list of users.
response = requests.get('https://my-api/users')
users = response.json()
Code Example: gRPC
```

```python
import users_pb2
import users_pb2_grpc

# Create a gRPC channel and a stub.
channel = grpc.insecure_channel('localhost:50051')
stub = users_pb2_grpc.UserStub(channel)

# Call the remote procedure.
response = stub.GetUsers(users_pb2.UserRequest())
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/microservices-interview-questions/
</details>

<details>
<summary>What is Domain-Driven Design (DDD) and how is it related to microservices?</summary>

### What is Domain-Driven Design (DDD) and how is it related to microservices?

**Definition:**

Domain-Driven Design (DDD) provides a model for designing and structuring microservices around specific business domains. It helps teams reduce complexity and align better with domain experts.

**How it works:**

#### Context Boundaries

In DDD, a Bounded Context establishes clear boundaries for a domain model, focusing on a specific domain of knowledge. These boundaries help microservice teams to operate autonomously, evolving their services within a set context.

#### Ubiquitous Language

Ubiquitous Language is a shared vocabulary that unites developers and domain experts. Microservices within a Bounded Context are built around this common language, facilitating clear communication and a deeper domain understanding.

#### Strong Consistency and Relational Databases

Within a Bounded Context, microservices share a consistent data model, often dealing with strong consistency and using relational databases. This cohesion simplifies integrity checks and data relationships.

Code Example

PaymentService Microservice:

OrderService Microservice:

OrderDetailsService Microservice:

**Examples:**

```java
@Entity
public class Payment {
    @Id
    private String paymentId;
    private String orderId;
    // ... other fields and methods
}
```

```java
@Entity
public class Order {
    @Id
    private String orderId;
    // ... other fields and methods
}

public void updateOrderWithPayment(String orderId, String paymentId) {
    // Update the order
}
```

```java
@Entity
public class OrderDetail {
    @EmbeddedId
    private OrderDetailId orderDetailId;
    private String orderId;
    private String itemId;
    private int quantity;
    // ... other fields and methods
}
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/microservices-interview-questions/
</details>

<details>
<summary>How would you decompose a monolithic application into microservices?</summary>

### How would you decompose a monolithic application into microservices?

**Definition:**

Decomposing a monolithic application into microservices involves breaking down a larger piece of software into smaller, interconnected services. This process allows for greater development agility, flexibility, and often better scalability.

**How it works:**

#### Key Considerations

Domain-Driven Design (DDD): Microservices should be independently deployable and manageable pieces of the application, typically built around distinct business areas or domains.

Database Strategy: Each microservice should have its own data storage, but for ease of data access and management, it's beneficial for microservices to share a database when practical.

Communication: The microservices should interact with each other in a well-coordinated manner. Two common models are Direct communication via HTTP APIs or using events for asynchronous communication.

#### Steps to Decompose

Identify Domains: Break down the application into major business areas or domains.
Data Segregation: Determine the entities and relationships within each microservice. Use techniques like database-per-service or shared-database.
Service Boundary: Define the boundaries of each microservice - what data and functionality does it control?
Define Contracts: Intelligently design the APIs or events used for communication between microservices.
Decouple Services: The services should be loosely coupled, to the maximum extent possible. This is especially important in scenarios where you have independent development teams working on the various microservices.

#### Code Example: Decomposition with DDD

Here is the Java code:

In this example, a Product microservice could manage products and expose its services through RESTful endpoints, and an Order microservice could manage orders. The two microservices would communicate indirectly through APIs, following DDD principles. Each would have its own database schema and set of tables.

**Examples:**

```java
@Entity
@Table(name = "product")
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private double price;
    //...
}

@Entity
@Table(name = "order_item")
public class OrderItem {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private Long productId;
    private Integer quantity;
    private double price;
    //...
}

public interface OrderService {
    Order createOrder(String customerId, List<OrderItem> items);
    List<Order> getOrdersForCustomer(String customerId);
    //...
}

@RestController
@RequestMapping("/orders")
public class OrderController {
    //...
    @PostMapping("/")
    public ResponseEntity<?> createOrder(@RequestBody Map<String, Object> order) {
        //...
    }
    //...
}
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/microservices-interview-questions/
</details>

<details>
<summary>What strategies can be employed to manage transactions across multiple microservices?</summary>

### What strategies can be employed to manage transactions across multiple microservices?

**Definition:**

Managing transactions across multiple microservices presents certain challenges, primarily due to the principles of independence and isolation that microservices are designed around. However, there are both traditional and modern strategies to handle multi-service transactions, each with its own benefits and trade-offs.

**How it works:**

#### Traditional Approaches

Two-Phase Commit (2PC)
Two-Phase Commit is a transaction management protocol in which a global coordinator communicates with all participating services to ensure that the transaction can either be committed globally or rolled back across all involved services.

While it offers transactional integrity, 2PC has seen reduced popularity due to its potential for blocking scenarios, performance overhead, and the difficulties associated with its management in distributed ecosystems.

Three-Phase Commit (3PC)
A direct evolution of the 2PC model, 3PC provides a more robust alternative. By incorporating an extra phase, it tries to overcome some of the drawbacks of 2PC, such as the potential for indefinite blocking.

While 3PC is an improvement over 2PC in this regard, it's not without its complexities and can still introduce performance penalties and maintenance overhead.

#### Transactional Outbox

The Transactional Outbox pattern involves using messaging systems as a mechanism to coordinate transactions across multiple microservices. In this approach:

The primary DB records changes in the outbox.
An event message is added to a message broker.
Subscribers read the message and execute the corresponding local transaction.
Transactional outbox offers high decoupling but does not provide the same level of strong consistency as the previous pattern.

#### SAGA Pattern

Derived from the Greek word for a "long, epic poem," a saga is a sequence of local transactions, each initiated within a microservice. In a distributed setting, a saga is a coordination mechanism between these local transactions, aiming for eventual consistency.

With SAGA, you trade immediate consistency for long-term consistency. If something goes wrong during the saga, you need to define a strategy for compensation actions to bring the overall system back to a consistent state, hence the "epic journey" metaphor.

#### Modern Approaches

#### Acknowledged Unreliability

The philosophy here is one of embracing a partially reliable set of distributed systems. Instead of trying to guarantee strong consistency across services, the focus is on managing and mitigating inconsistencies and failures through robust service designs and effective monitoring.

#### DDD and Bounded Contexts

When microservices are designed using Domain-Driven Design (DDD), each microservice focuses on a specific business domain, or "Bounded Context." By doing so, services tend to be more independent, leading to fewer cross-service transactions in the first place.

This approach promotes a strong focus on clear service boundaries and effective communication and collaboration between the stakeholders who understand those boundaries and the associated service behavior.

#### CQRS and Event Sourcing

The Command Query Responsibility Segregation (CQRS) pattern involves separating read and write operations. This clear separation of concerns reduces the need for cross-service transactions.

With Event Sourcing, each state change is represented as an event, providing a reliable mechanism to propagate changes to multiple services in an asynchronous and non-blocking manner.

What is crucial here is that the proliferation of these patterns and concepts in modern software and system design is a direct result of the unique needs and opportunities presented by new paradigms such as microservices. Instead of retrofitting old ways of thinking into a new environment, the focus is on adapting notions of consistency and reliability to the realities of modern, decentralized, and highly dynamic systems.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/microservices-interview-questions/
</details>

<details>
<summary>Explain the concept of ‘Bounded Context’ in the microservices architecture.</summary>

### Explain the concept of ‘Bounded Context’ in the microservices architecture.

**Definition:**

In the context of microservices architecture, the principle of "Bounded Context" emphasizes the need to segment a complex business domain into distinct and manageable sections.

It suggests a partitioning based on business context and clearly defined responsibilities to enable individual teams to develop and manage independent microservices.

**How it works:**

#### Core Concepts

#### Ubiquitous Language

Each microservice and its bounded context must have a clearly defined "domain language" that is comprehensible to all the members of the team and aligns with the business context.

#### Context Boundaries

A bounded context delineates the scope within which a particular model or concept is operating, ensuring that the model is consistent and meaningful within that context.

It establishes clear boundaries, acting as a bridge between domain models, so that inside the context a specific language or model is used.

For instance: in the context of a customer, it might use a notion of "sales leads" to represent potential customers, while in the context of sales, it would define leads as initial contact or interest in a product.

#### Data Consistency

The data consistency and integrity is local to the bounded context. Each context's data is safeguarded using transactions, and data is only propagated carefully to other contexts to which it has a relationship.

It ensures that the understanding of data by each service or bounded context is relevant and up-to-date.

Example: In an e-commerce system, the product catalog context is responsible for maintaining product data consistency.

#### Teams & Autonomy

Each bounded context is maintained and evolved by a specific team responsible for understanding the business logic, making it self-governing and allowing teams to work independently without needing to understand the logic of other contexts.

#### Relationship with Source Code

The concept of a bounded context is implemented and realized within the source code using Domain-Driven Design (DDD) principles. Each bounded context typically has its own codebase.
Code Example: Bounded Context and Ubiquitous Language

Here is the Tic Tac Toe game Model:

**Examples:**

```java
// Very specific to the context of the game
public enum PlayerSymbol {
    NOUGHT, CROSS
}

// Specific to the game context
public class TicTacToeBoard {
    private PlayerSymbol[][] board;
    // Methods to manipulate board
}

// This event is purely for the game context to indicate the game has a winner.
public class GameWonEvent {
    private PlayerSymbol winner;
    // getter for winner
}
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/microservices-interview-questions/
</details>

<details>
<summary>How do you handle failure in a microservice?</summary>

### How do you handle failure in a microservice?

**Definition:**

In a microservices architecture, multiple smaller components, or microservices, work together to deliver an application. Consequently, a failure in one of the services can have downstream effects, potentially leading to system-wide failure. To address this, several best practices and resilience mechanisms are implemented.

**How it works:**

#### Best Practices for Handling Failure

#### Fault Isolation

Circuit Breaker Pattern: Implement a circuit breaker that isolates the failing service from the rest of the system. This way, the failure doesn't propagate and affect other services.

Bulkhead Pattern: Use resource pools and set limits on the resources each service can consume. This limits the impact of failure, ensuring that it doesn't exhaust the whole system's resources.

#### Error Recovery

Retry Strategy: Implement a retry mechanism that enables services to recover from transient errors. However, it's important to determine the maximum limit and backoff policies during retries to prevent overload.

Failsafe Services: Set up backup systems so that essential functionalities are not lost. For example, while one service is down, you can temporarily switch to a reduced-functionality mode or data backup to avoid complete system failure.

#### Resilience Mechanisms

#### Auto-scaling

Resource Reallocation: Implement auto-scaling to dynamically adjust resources based on load and performance metrics, ensuring the system is capable of handling the current demand.

#### Data Integrity

Eventual Consistency: In asynchronous communication between services, strive for eventual consistency of data to keep services decoupled. This ensures data integrity is maintained even when a service is temporarily unavailable.

Transaction Management: Use a two-phase commit mechanism to ensure atomicity of transactions across multiple microservices. However, this approach can introduce performance bottlenecks.

#### Data Management

Data Redundancy: Introduce redundancy (data duplication) in services that need access to the same data, ensuring data availability if one of the services fails.

Caching: Implement data caching to reduce the frequency of data requests, thereby lessening the impact of failure in the data source.

Data Sharding: Distribute data across multiple databases or data stores in a partitioned manner. This reduces the risk of data loss due to a single point of failure, such as a database server outage.

#### Communication

Versioning: Maintain backward compatibility using API versioning. This ensures that services can communicate with older versions if the newer one is experiencing issues.

Message Queues: Decouple services using a message queuing system, which can help with load leveling and buffering of traffic to handle temporary fluctuations in demand.

Health Checks: Regularly monitor the health of microservices to identify and isolate services that are malfunctioning or underperforming.

#### Best Practices for Handling Failure

Self-Healing Components: Develop microservices capable of self-diagnosing and recovering from transient faults, decreasing reliance on external mechanisms for recovery.

Graceful Degradation: When a service fails or becomes overloaded, gracefully degrade the quality of service provided to users.

Continuous Monitoring: Regularly monitor all microservices and alert teams in real-time when there is a deviation from the expected behavior.

Failure Isolation: Localize and contain the impact of failures, and provide backup operations and data whenever possible to provide ongoing service.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/microservices-interview-questions/
</details>


### Design Patterns and Best Practices

<details>
<summary>What design patterns are commonly used in microservices architectures?</summary>

### What design patterns are commonly used in microservices architectures?

**Definition:**

Several design patterns lend themselves well to microservices architectures, offering best practices in their design and implementation.

**How it works:**

#### Common Design Patterns

API Gateway: A single entry point for clients, responsible for routing requests to the appropriate microservice.

Circuit Breaker: A fault-tolerance pattern that automatically switches from a failing service to a fallback to prevent service cascading failures.

Service Registry: Microservices register their network location, making it possible to discover and interact with them dynamically. This is essential in a dynamic environment where services frequently start and stop or migrate to new hosts.

Service Discovery: The ability for a microservice to locate and invoke another through its endpoint, typically facilitated by a service registry or through an intermediary like a load balancer.

Bulkhead: The concept of isolating different parts of a system from each other to prevent the failure of one from affecting the others.

Event Sourcing: Instead of persisting the current state of an entity, the system persists a sequence of events that describe changes to that entity, allowing users to reconstruct any state of the system.

Database per Service: Each microservice has a dedicated database, ensuring autonomy and loose coupling.

Saga Pattern: Orchestrates multiple microservices to execute a series of transactions in a way that maintains data consistency across the services.

Strangler Fig: A deployment pattern that gradually replaces monolithic or(conventional systems with a modern architecture, such as microservices.

Blue-Green Deployment: This strategy reduces downtime and risk by running two identical production environments. Only one of them serves live traffic at any point. Once the new version is tested and ready, it switches.

A/B Testing: A/B testing refers to the practice of making two different versions of something and then seeing which version performs better.

Cache-Aside: A pattern where an application is responsible for loading data into the cache from the storage system.

Chained Transactions: Instead of each service managing its transactions, the orchestration service controls the transactions between multiple microservices.

Code Example: Circuit Breaker using Hystrix Library

Here is the Java code:

The term "Circuit Breaker" is from Martin Fowler's original description. It's a well-known hardware pattern used in electrical engineering. When the current is too high, the circuit "breaks" or stops working until it is manually reset. The software equivalent, in a microservices architecture, is designed to stop sending requests to a failing service, giving it time to recover.

**Examples:**

```java
@CircuitBreaker(name = "backendA", fallbackMethod = "fallback")
public String doSomething() {
  // Call the service
}

public String fallback(Throwable t) {
  // Fallback logic
}
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/microservices-interview-questions/
</details>

<details>
<summary>Can you describe the API Gateway pattern and its benefits?</summary>

### Can you describe the API Gateway pattern and its benefits?

**Definition:**

The API Gateway acts as a single entry point for a client to access various capabilities of microservices.

**How it works:**

#### Gateway Responsibilities

Request Aggregation: Merges multiple service requests into a unified call to optimize client-server interaction.
Response Aggregation: Collects and combines responses before returning them, benefiting clients by reducing network traffic.
Caching: Stores frequently accessed data to speed up query responses.
Authentication and Authorization: Enforces security policies, often using JWT or OAuth 2.0.
Rate Limiting: Controls the quantity of requests to safeguard services from being overwhelmed.
Load Balancing: Distributes incoming requests evenly across backend servers to ensure performance and high availability.
Service Discovery: Provides a mechanism to identify the location and status of available services.

#### Key Benefits

Reduced Latency: By optimizing network traffic, it minimizes latency for both requests and responses.
Improved Fault-Tolerance: Service failures are isolated, preventing cascading issues. It also helps in providing fallback functionality.
Enhanced Security: Offers a centralized layer for various security measures, such as end-to-end encryption.
Simplified Client Interface: Clients interact with just one gateway, irrespective of the underlying complicated network of services.
Protocol Normalization: Allows backend services to use different protocols (like REST and SOAP) while offering a consistent interface to clients.
Data Shape Management: Can transform and normalize data to match what clients expect, hiding backend variations.
Operational Insights: Monitors and logs activities across services, aiding in debugging and analytics.

#### The gateway pattern is particularly useful:

In systems built on SOA, where it is used to adapt to modern web-friendly protocols.
For modern applications built with microservices, especially when multiple services need to be accessed for a single user action.
When integrating with third-party services, helping in managing and securing the integration.
Code Example: Setting Up an API Gateway

Here is the Python code:

**When to use:**

#### Contextual Use

**Examples:**

```python
from flask import Flask, request
import requests

app = Flask(__name__)

@app.route('/')
def api_gateway():
    # Example: Aggregating and forwarding requests
    response1 = requests.get('http://service1.com')
    response2 = requests.get('http://service2.com')

    # Further processing of responses

    return 'Aggregated response'
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/microservices-interview-questions/
</details>

<details>
<summary>Explain the ‘Circuit Breaker’ pattern. Why is it important in a microservices ecosystem?</summary>

### Explain the ‘Circuit Breaker’ pattern. Why is it important in a microservices ecosystem?

**Definition:**

The Circuit Breaker pattern is a key mechanism in microservices architecture that aims to enhance fault tolerance and resilience.

**How it works:**

#### Core Mechanism

State Management: The circuit breaker can be in one of three states: Closed (normal operation), Open (indicating a failure to communicate with the service), and Half-Open (an intermittent state to test if the service is again available).
State Transition: The circuit breaker can transition between states based on predefined triggers like the number of consecutive failures or timeouts.

#### Benefits

Failure Isolation: Preventing cascading failures ensures that malfunctioning services do not drag down the entire application.
Latency Control: The pattern can quickly detect slow responses, preventing unnecessary resource consumption and improving overall system performance.
Graceful Degradation: It promotes a better user experience by continuing to operate, though possibly with reduced functionality, even when services are partially or completely unavailable.
Fast Recovery: After the system or service recovers from a failure, the circuit breaker transitions to its closed or half-open state, restoring normal operations promptly.

#### Practical Application

In a microservices environment, the circuit breaker pattern is often employed with libraries like Netflix's Hystrix or Resilience4J and languages like Java or .NET.

Hystrix Example

Here is the Java code:

Resilience4J Example

Here is the Java code:

.NET's Polly Example

Here is the C# code:

**When to use:**

#### Asynchronous Use Cases

For asynchronous activities, such as making API calls in a microservices context, the strategy can adapt to handle these as well. Libraries like Polly and Resilience4j are designed to cater to asynchronous workflows.

**Examples:**

```java
HystrixCommand<?> command = new HystrixCommand<>(HystrixCommand.Setter
  .withGroupKey(HystrixCommandGroupKey.Factory.asKey("ExampleGroup"))
  .andCommandPropertiesDefaults(HystrixCommandProperties.Setter()
     .withCircuitBreakerErrorThresholdPercentage(50)));
```

```java
CircuitBreakerConfig config = CircuitBreakerConfig.custom()
  .failureRateThreshold(20)
  .ringBufferSizeInClosedState(5)
  .build();

CircuitBreaker circuitBreaker = CircuitBreakerRegistry.of(config).circuitBreaker("example");
```

```csharp
var circuitBreakerPolicy = Policy
  .Handle<SomeExceptionType>()
  .CircuitBreaker(3, TimeSpan.FromSeconds(60));
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/microservices-interview-questions/
</details>

<details>
<summary>What is a ‘Service Mesh’? How does it aid in managing microservices?</summary>

### What is a ‘Service Mesh’? How does it aid in managing microservices?

**Definition:**

A Service Mesh is a dedicated infrastructure layer that simplifies network requirements for microservices, making communication more reliable, secure, and efficient. It is designed to reduce the operational burden of communication between microservices.

Why Service Mesh?
Zero Trust: Service Meshes ensure secure communication, without relying on individual services to implement security measures consistently.

Service Health Monitoring: Service Meshes automate health checks, reducing the risk of misconfigurations.

Traffic Management: They provide tools for controlling traffic, such as load balancing, as well as for A/B testing and canary deployments.

Adaptive Routing: In response to dynamic changes in service availability and performance, Service Meshes can redirect traffic to healthier services.

**How it works:**

#### Elements of Service Mesh

The Service Mesh architecture comprises two primary components:

Data Plane: Controls the actual service-to-service traffic. It's made up of proxy servers, such as Envoy or Linkerd, which sit alongside running services to manage traffic.

Control Plane: Manages the configuration and policies that the Data Plane enforces. It includes systems like Istio and Consul.

#### Key Capabilities

Load Balancing: Service Meshes provide intelligent load balancing, distributing requests based on various criteria, like latency or round-robin.

Security Features: They offer a suite of security capabilities, including encryption, authentication, and authorization.

Traffic Control: Service Meshes enable fine-grained traffic control, allowing you to manage traffic routing, failover, and versioning.

Metrics and Tracing: They collect and provide key operational telemetry, making it easier to monitor the health and performance of your microservices.

Code Example: Service Mesh Components in Kubernetes

Here is the YAML configuration:

For the Control Plane:

For the Data Plane:

In this example, Envoy serves as the sidecar proxy (Data Plane) injected alongside service-1 and service-2, and the control-plane-pod and control-plane-service represent the control plane.

**Examples:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: control-plane-pod
  labels:
    component: control-plane
spec:
  containers:
  - name: controller
    image: control-plane-image
    ports:
    - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: control-plane-service
spec:
  selector:
    component: control-plane
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: service-1-pod
  labels:
    app: service-1
spec:
  containers:
  - name: service-1-container
    image: service-1-image
    ports:
    - containerPort: 8080
  - name: proxy
    image: envoyproxy/envoy-alpine
  containers:
  - name: service-2-container
    image: service-2-image
    ports:
    - containerPort: 8080
  - name: proxy
    image: envoyproxy/envoy-alpine
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/microservices-interview-questions/
</details>

<details>
<summary>How do you ensure data consistency across microservices?</summary>

### How do you ensure data consistency across microservices?

**Definition:**

Data consistency in a microservices architecture is crucial for ensuring that business-critical operations are executed accurately.

**How it works:**

#### Approaches to Data Consistency in Microservices

Synchronous Communication: Via REST or gRPC, which ensures immediate consistency but can lead to performance issues and tight coupling.
Asynchronous Communication: Using message queues which ensure eventual consistency but are more resilient and performant.
Eventual Consistency with Compensating Actions: Involves completing a series of potentially inconsistent operations within a microservice and compensating for any errors, often orchestrated through a dedicated event handler.

#### Code Example: Synchronous Communication

Here is the Python code:

# Synchronous Communication with RESTful APIs

Code Example: Asynchronous Communication with Event Bus

Here is the RabbitMQ code:

Producer:

Consumer:

# Consumes the 'order_queue'
# Processes the order asynchronously

#### Eventual Consistency with Compensating Actions

#### CAP Theorem

The CAP theorem states that it's impossible for a distributed data store to simultaneously provide more than two of the following three guarantees: Consistency, Availability, and Partition Tolerance.

BASE (Basically Available, Soft state, Eventually consistent)
BASE is an acronym that describes the properties of many NoSQL databases. It includes:

Basically Available: The system remains operational most of the time.
Soft state: The state of the system may change over time, even without input.
Eventually Consistent: The system will become consistent over time, given that the applications do not input any new data.

#### Transactional Outbox Pattern

This pattern, used in conjunction with an event store or message broker, ensures atomic operations across services. The microservice first writes an event to an "outbox" table within its own database before committing the transaction. A specialized, outbox-reading process then dispatches these events to the message broker.

#### Advantages

Ensures atomicity, preventing events from being disclosed due to a partially committed transaction.
Mitigates the risk of message loss that might occur if an external event publishing action happens after the transaction is committed.

#### Code Example: Transactional Outbox Pattern

Here is the Java code:

**Examples:**

```python
import requests

def place_order(order):
    response = requests.post('http://order-service/api/v1/orders', json=order)
    if response.status_code == 201:
        return "Order placed successfully"
    else:
        return "Order placement failed"

# Potential downside: If the order service is down, the basket service cannot commit the transaction.
```

```python
import pika

def send_order(order):
    connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
    channel = connection.channel()
    channel.queue_declare(queue='order_queue')
    channel.basic_publish(exchange='', routing_key='order_queue', body=order)
    connection.close()

# No blocking operation or transactional context ensures high performance.
```

```java
// Using Java's Spring Data JPA and RabbitMQ
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Modifying;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;

public interface OutboxEventRepository extends JpaRepository<OutboxEvent, Long> {

    @Modifying
    @Query(value = "INSERT INTO outbox_event (id, eventType, payload) VALUES (:id, :eventType, :payload)", nativeQuery = true)
    void create(@Param("id") long id, @Param("eventType") String eventType, @Param("payload") String payload);
}

public class OrderService {
    private final OutboxEventRepository outboxEventRepository;

    public void placeOrder(Order order) {
        // ... Perform order placement

        outboxEventRepository.create(order.getId(), "OrderPlacedEvent", toJson(order));

        // ... Commit transaction
    }
}
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/microservices-interview-questions/
</details>

<details>
<summary>Discuss the strategies you would use for testing microservices.</summary>

### Discuss the strategies you would use for testing microservices.

**Definition:**

microservices.io separates two automated checks from a full end-to-end run. A service component test exercises one service with test doubles for the services it calls. A service integration contract test is a suite written by a consumer that checks the provider still meets that consumer's expectations. Both exist because end-to-end tests that launch many services are difficult, slow, brittle, and expensive.

**How it works:**

Component tests run the service in isolation, so a passing run is faster and cheaper, but the doubles can drift from the real collaborator. Contract tests move that check to the provider's API as the consumer defined it. The pattern page names Spring Cloud Contract as an implementation and notes the open issue: the consumer-provided tests must still match what the consumer actually does. The same pattern index also lists these two under the microservice architecture's testing patterns. A passing isolated test can still fail in production, which is the drawback both pages state.

**When to use:**

Prefer component and contract tests for day-to-day feedback. Keep a smaller end-to-end set for the paths the isolated tests cannot see, accepting that those runs are the expensive ones the pattern pages warn about.

**Examples:**

```text
component test: service + test doubles of its dependencies
contract test: consumer's expectations executed against the provider
end-to-end: several real services; slow and brittle
```

**Sources:**

- [microservices.io — Service Component Test](https://microservices.io/patterns/testing/service-component-test.html)
- [microservices.io — Service Integration Contract Test](https://microservices.io/patterns/testing/service-integration-contract-test.html)
- [microservices.io — Microservice Architecture](https://microservices.io/patterns/microservices.html)

</details>

<details>
<summary>How can you prevent configuration drift in a microservices environment?</summary>

### How can you prevent configuration drift in a microservices environment?

**Definition:**

Configuration drift here is the gap between the configuration a service expects and the configuration a deploy actually supplies. Twelve-factor and Richardson's externalized-configuration pattern both keep that configuration outside the build.

**How it works:**

Twelve-factor defines config as everything that varies between deploys: database and cache handles, credentials for external services, and the canonical hostname. Code does not vary between deploys, so constants in code are a violation. The litmus test is whether the codebase could be open-sourced without leaking credentials. Config files that are not in revision control are better than constants, but they are easy to check in by mistake, they scatter across formats, and they are often language-specific. Environment variables are changed per deploy without a code change, are unlikely to be committed, and are a language- and OS-agnostic standard. Grouping them into named environments such as `development` and `production` does not scale: extra deploys invent names like `joes-staging` and the combinations become brittle. Twelve-factor keeps each variable orthogonal and manages it per deploy.

Richardson's pattern reads that external configuration at startup, including from environment variables, property files, and command-line arguments, so one build runs in dev, test, QA, staging, and production. The pattern's open issue is how to ensure the supplied configuration matches what the service expects. Twelve-factor's answer to scattered files is one mechanism, environment variables, reviewed with the deploy rather than copied as a named environment bundle.

**When to use:**

Externalize every value that changes between deploys. Keep internal structure, such as how modules are wired, in code, because twelve-factor says that kind of config does not vary per deploy.

**Sources:**

- https://12factor.net/config
- https://microservices.io/patterns/externalized-configuration.html
</details>

<details>
<summary>When should you use synchronous vs. asynchronous communication in microservices?</summary>

### When should you use synchronous vs. asynchronous communication in microservices?

**Definition:**

Chris Richardson's messaging pattern contrasts synchronous communication, where both sides must be available for the whole request, with asynchronous messaging, where services exchange messages over a channel.

**How it works:**

Synchronous communication creates tight runtime coupling. If the callee is down, the caller cannot finish. Messaging styles on the channel are request/response, a notification with no reply, request/asynchronous response, publish/subscribe to zero or more recipients, and publish/asynchronous response. The broker buffers until the consumer can process, which raises availability, and the sender is decoupled from the consumer. The costs are a broker that must itself be highly available, and extra complexity for request/reply. RabbitMQ stores those messages in a FIFO queue. Priorities and requeueing can change the order a consumer sees.

**When to use:**

Use synchronous calls when the caller needs a prompt result and both processes will be up for the duration. Use asynchronous messaging when the sender should succeed even if the consumer is down, or when zero or more subscribers should receive an event, as in the pattern's `Order Created` publish.

**Sources:**

- https://microservices.io/patterns/communication-style/messaging.html
- https://www.rabbitmq.com/docs/queues
</details>

<details>
<summary>What role does containerization play in microservices?</summary>

### What role does containerization play in microservices?

**Definition:**

Docker packages a service in a container: a loosely isolated environment that contains what the application needs, so the service does not depend on what is installed on the host.

**How it works:**

An image is a read-only template built in layers from a Dockerfile. A container is a runnable instance. The same image runs on a laptop, in a data center, or on a cloud provider. Docker uses Linux namespaces for isolation. Removing the container drops state that was not in persistent storage. The delivery loop Docker describes is to develop in containers, push the image to test, and push the same image to production. Containers are lighter than hypervisor virtual machines, so more services fit on one machine. Kubernetes then runs those containers as Pods behind a Service, because Pods are ephemeral and clients should not track their IPs.

**When to use:**

Package each microservice as an image when it must move unchanged through test and production and when copies are started and stopped with load. Put durable data in a volume or a backing service, not in the container filesystem.

**Sources:**

- https://docs.docker.com/get-started/docker-overview/
- https://kubernetes.io/docs/concepts/services-networking/service/
</details>

### Deployment and Operations

<details>
<summary>What are the challenges of deploying microservices?</summary>

### What are the challenges of deploying microservices?

**Definition:**

Deploying microservices means shipping many independently deployable units, keeping more than one version running during a rollout, and finding Pods whose addresses change.

**How it works:**

Sato's canary release reduces the risk of a new version by sending it to a subset of users first. The drawback he names is that you manage multiple versions at once, and the number of concurrent versions should stay small. The database must support both versions during the rollout, which he handles with a parallel change. Installed and mobile clients are harder because you do not control when they upgrade.

Kubernetes Pods are ephemeral. A Deployment creates and destroys them, each Pod has its own IP, and a frontend cannot keep a list of those IPs. A Service selects the current Pods and is the stable endpoint. Richardson's messaging broker must be highly available or the buffer itself is a new failure point. Twelve-factor config must be supplied per deploy or the new instances point at the wrong backing services.

**When to use:**

Plan a canary when a bad version must not hit every user. Put a Service in front of Pods so clients survive replacement. Externalize configuration so the new deploy does not inherit another environment's endpoints.

**Sources:**

- https://martinfowler.com/bliki/CanaryRelease.html
- https://kubernetes.io/docs/concepts/services-networking/service/
- https://microservices.io/patterns/communication-style/messaging.html
- https://12factor.net/config
</details>

<details>
<summary>Describe blue-green deployment and how it applies to microservices.</summary>

### Describe blue-green deployment and how it applies to microservices.

**Definition:**

Martin Fowler's blue-green deployment keeps two production environments and switches the router from the live one to the one that just passed final testing. A microservice can use the same cut-over per service, because each service is deployed on its own.

**How it works:**

The idle color runs the new build. After it works, the router (or, in one variation Fowler cites, a bounced web server) sends traffic there. Switching back is the rollback. Database schema changes are not switched in the same instant as the code: first deploy a schema that serves both versions, then deploy the new service, then drop the old schema support.

**When to use:**

Use it when a service release must avoid a long cut-over and must keep the previous color ready. Fowler notes the two environments also act as the hot-standby switch.

**Examples:**

```text
order-service blue: v1 live
order-service green: v2 tested
router to order-service flips to green
```

**Sources:**

- [Martin Fowler — Blue Green Deployment](https://martinfowler.com/bliki/BlueGreenDeployment.html)

</details>

<details>
<summary>How does canary releasing work, and how is it beneficial for microservices deployments?</summary>

### How does canary releasing work, and how is it beneficial for microservices deployments?

**Definition:**

Danilo Sato defines a canary release as rolling a new version out to a small subset of users before the rest of the infrastructure, so a problem is visible before everyone is affected.

**How it works:**

You choose the subset at random, as internal users, by profile, by region, or by brand. Confidence grows by routing more users and more servers. Sato's rollout uses Phoenix or immutable servers rather than patching machines in place. Rollback reroutes users to the old version. The benefit he names is capacity testing on production traffic with that rollback, by ramping load and watching metrics, instead of a separate capacity environment. He separates this from A/B testing: a canary looks for regressions and should finish in minutes or hours, while an A/B test waits for statistical significance. Facebook's first canary is internal, with feature flags on. You must run multiple versions, so keep that number small. Database changes use a parallel change so both versions work. Kubernetes notes canary deployments as a common use of Services, which select which Pods receive traffic.

**When to use:**

Use a canary when a microservice can run two versions and you can route a slice of traffic. Use a parallel database change for the schema. Do not treat the canary as the A/B test.

**Sources:**

- https://martinfowler.com/bliki/CanaryRelease.html
- https://kubernetes.io/docs/concepts/services-networking/service/
</details>

<details>
<summary>Explain the concept of ‘Infrastructure as Code’ and how it benefits microservices management.</summary>

### Explain the concept of ‘Infrastructure as Code’ and how it benefits microservices management.

**Definition:**

HashiCorp defines Terraform as an infrastructure as code tool: you define cloud and on-premises resources in human-readable files that you version, reuse, and share, then provision them through a write, plan, and apply workflow.

**How it works:**

Providers call platform APIs. A configuration can describe virtual machines, a VPC, security groups, a load balancer, DNS, Kubernetes, or Helm. Plan shows what will be created, updated, or destroyed. Apply runs that plan in dependency order and can change independent resources in parallel. A state file is the source of truth. Modules package a reusable set of resources so each microservice environment is not hand-built. Configuration in version control is the record of what the services run on. HCP Terraform runs the workflow for a team, with access control and shared state. Twelve-factor still puts per-deploy secrets in environment variables rather than in those files.

**When to use:**

Use Terraform when each microservice's network, runtime, and DNS should be reviewed as a plan and repeated for another environment. Keep credentials out of the configuration that you commit.

**Sources:**

- https://developer.hashicorp.com/terraform/intro
- https://12factor.net/config
</details>

<details>
<summary>Describe what Continuous Integration/Continuous Deployment (CI/CD) pipelines look like for microservices.</summary>

### Describe what Continuous Integration/Continuous Deployment (CI/CD) pipelines look like for microservices.

**Definition:**

Docker describes a continuous integration and delivery path whose unit is the container image: build locally, test that image, then push the same image to production. A canary and a release toggle sit on that path so the push does not expose every user at once.

**How it works:**

Developers share work in containers, push the application into a test environment, and run automated and manual tests. A fix is redeployed to test and then pushed as an updated image to production. Pete Hodgson's release toggles let incomplete code stay on the trunk that this pipeline deploys. The toggle is usually static and short-lived. Danilo Sato's canary then routes a subset of users to the new image and ramps that share while metrics are watched. Rollback reroutes to the old version. The database uses a parallel change so both images work. Kubernetes Services are how a canary selects which Pods receive traffic.

**When to use:**

Use the image as the artifact that test and production run. Add a release toggle when unfinished work must ride the trunk. Add a canary when the new image should meet a slice of production traffic before the rest.

**Sources:**

- https://docs.docker.com/get-started/docker-overview/
- https://martinfowler.com/articles/feature-toggles.html
- https://martinfowler.com/bliki/CanaryRelease.html
- https://kubernetes.io/docs/concepts/services-networking/service/
</details>

<details>
<summary>How do you monitor health and performance of microservices?</summary>

### How do you monitor health and performance of microservices?

**Definition:**

Richardson's application-metrics pattern gathers statistics about individual operations and aggregates them for reporting and alerting. Distributed tracing records the start and end of the operations that make up one request. Kubernetes drops a failed Pod from a Service so traffic reaches only healthy Pods.

**How it works:**

Metrics aggregation is push or pull. The pattern names Prometheus client libraries and AWS CloudWatch. External monitoring only shows overall response time and invocation count, which is why tracing assigns an external request id, passes it between services, and stores timing in a central service such as Zipkin. That shows sources of latency. A canary ramps production load and watches metrics as the health check of a new version. Kubernetes self-healing restarts a failed container and replaces a failed Pod so the replica count holds.

**When to use:**

Instrument operations when you need alerts. Trace a request when you need the latency of each hop. Let the Service endpoints follow Pod health so a dead Pod is not a routing target.

**Sources:**

- https://microservices.io/patterns/observability/application-metrics.html
- https://microservices.io/patterns/observability/distributed-tracing.html
- https://kubernetes.io/docs/concepts/architecture/self-healing
- https://martinfowler.com/bliki/CanaryRelease.html
</details>

### Microservices and Data Management

<details>
<summary>How do you handle database schema changes in a microservice architecture?</summary>

### How do you handle database schema changes in a microservice architecture?

**Definition:**

Danilo Sato says a canary has to manage database changes because two versions of the service run together. A parallel change lets the database support both versions until the old one is gone.

**How it works:**

During the rollout, some traffic still hits the old service and some hits the new one. Each must read and write. The parallel change is the migrate phase of that period. When every user is on the new version, the old infrastructure is decommissioned. If the new version fails, traffic goes back to the old one, so the database cannot require only the new shape yet. Sato says to keep the number of concurrent versions small. IBM notes that a microservice often has its own database, so this change is local to that service's store rather than to one shared schema for the whole system.

**When to use:**

Apply a parallel change whenever two versions of a service will use the database at once, including a canary and a period when old mobile clients are still installed. Contract the schema after the old version's usage has fallen.

**Sources:**

- https://martinfowler.com/bliki/CanaryRelease.html
- https://www.ibm.com/think/topics/cap-theorem
</details>

<details>
<summary>Discuss the pros and cons of using a shared database vs. a database-per-service.</summary>

### Discuss the pros and cons of using a shared database vs. a database-per-service.

**Definition:**

microservices.io's database-per-service pattern keeps each service's persistent data private and reachable only through that service's API. A service's transactions touch only its own database. The microservice architecture page states the corresponding cost: loose coupling pushes operations that used to be one ACID transaction into eventually consistent collaboration.

**How it works:**

Privacy does not require a server per service. On a relational database the options are private tables, a private schema, or a private server. Schema-per-service makes ownership clearer; private tables and a private schema have the lowest overhead; a high-throughput service may need its own server. Grants, or a distinct database user per service, stop other teams from bypassing the API. A shared database is the alternative the pattern rejects when services must be developed, deployed, and scaled independently, or when one service wants a different store (the page cites Elasticsearch for text and Neo4j for a graph).

**When to use:**

Use a private database when independent deploy and an independent data model matter. Accept a shared database only when you are deliberately choosing the simpler ACID interaction the architecture page calls one of the forces pulling subdomains back together. Cross-service commands then use a saga; cross-service queries use API composition or CQRS.

**Examples:**

```text
Order Service  -> orders schema   (user order_svc)
Customer Service -> customers schema (user customer_svc)
```

**Sources:**

- [microservices.io — Database per service](https://microservices.io/patterns/data/database-per-service.html)
- [microservices.io — Microservice Architecture](https://microservices.io/patterns/microservices.html)

</details>

<details>
<summary>Explain the concept of ‘Event Sourcing’ in the context of microservices.</summary>

### Explain the concept of ‘Event Sourcing’ in the context of microservices.

**Definition:**

Martin Fowler's event sourcing stores each change of application state as an event in application order, and keeps that log for the life of the state. In a microservice architecture, microservices.io uses those events as the feed for CQRS views and for the messages a transactional outbox must deliver.

**How it works:**

The service appends an event and applies it to the current aggregate. Other services do not read its tables; they subscribe to the events. CQRS builds a read model from that stream. The outbox pattern is what makes the local commit and the publish of such an event atomic without 2PC.

**When to use:**

Use it when a service must rebuild state from history and when other services need a log of what changed. microservices.io notes that an event-sourced write model is a poor shape for queries, which is why the CQRS view exists.

**Examples:**

```text
Order service appends OrderPlaced
outbox relay publishes it
Order History service updates its read model
```

**Sources:**

- [Martin Fowler — Event Sourcing](https://martinfowler.com/eaaDev/EventSourcing.html)
- [microservices.io — CQRS](https://microservices.io/patterns/data/cqrs.html)
- [microservices.io — Transactional outbox](https://microservices.io/patterns/data/transactional-outbox.html)

</details>

<details>
<summary>What is Command Query Responsibility Segregation (CQRS) and how can it be applied to microservices?</summary>

### What is Command Query Responsibility Segregation (CQRS) and how can it be applied to microservices?

**Definition:**

microservices.io defines CQRS here as a view database: a read-only replica shaped for one query or a group of related queries. The application updates that view by subscribing to domain events from the services that own the data. The view's store and schema are chosen for the query, often a document or key-value database.

**How it works:**

Database-per-service makes a join across services no longer a single SQL join. Event sourcing makes the write log a poor shape for those queries. A subscriber builds the denormalized view. The architecture page lists CQRS as one of four collaboration patterns for a distributed query, next to API composition, and says it uses asynchronous messaging.

**When to use:**

Use it when a query must combine data owned by several services and a purpose-built read model is worth the extra moving parts. The page lists scalable denormalized views, a simpler split between command and query models, and necessity in an event-sourced architecture. The drawbacks are complexity, possible duplication, and replication lag: the view is eventually consistent.

**Examples:**

```text
Customer and Order services publish events
Order History service updates a read model for "customers in a region and their recent orders"
```

**Sources:**

- [microservices.io — CQRS](https://microservices.io/patterns/data/cqrs.html)
- [microservices.io — Microservice Architecture](https://microservices.io/patterns/microservices.html)

</details>

<details>
<summary>Can you discuss strategies for dealing with data consistency without using distributed transactions?</summary>

### Can you discuss strategies for dealing with data consistency without using distributed transactions?

**Definition:**

microservices.io says a 2PC transaction is a poor fit once each service has its own database, and the architecture page prefers a local ACID transaction inside one service over a distributed BASE transaction. Cross-service work is a sequence of local transactions.

**How it works:**

A command that spans services is a saga: local transactions in series. A query that spans services is either API composition (the caller performs the join) or CQRS (a view maintained from domain events). The transactional outbox writes the outgoing message in the same local transaction as the business update, so the service does not need a distributed transaction with the broker. Consumers of those messages must be idempotent because the relay can publish a message more than once.

**When to use:**

Use a single local transaction when the operation stays inside one service's database. Use saga plus outbox when several services must change, and CQRS or API composition when the read side must combine their data.

**Examples:**

```text
local transaction: update aggregate + insert outbox row
saga: each participant runs its own local transaction
```

**Sources:**

- [microservices.io — Database per service](https://microservices.io/patterns/data/database-per-service.html)
- [microservices.io — Transactional outbox](https://microservices.io/patterns/data/transactional-outbox.html)
- [microservices.io — Microservice Architecture](https://microservices.io/patterns/microservices.html)

</details>

### Security

<details>
<summary>How do you implement authentication and authorization in microservices?</summary>

### How do you implement authentication and authorization in microservices?

**Definition:**

RFC 6749 separates authentication of the resource owner from the credential the client uses. The client obtains an access token from an authorization server and presents it to the resource server. A JSON Web Token is one signed token format for that credential.

**How it works:**

OAuth does not give the third party the owner's password. The authorization server issues an access token with a scope and a lifetime after the owner approves. The client sends that token to the resource server. jwt.io describes a JWT as three Base64url parts: header, payload, and signature. The header names the algorithm, such as HMAC SHA256 or RSA. The signature covers the encoded header and payload, so the receiver can detect a change. Registered claims include `iss`, `exp`, `sub`, and `aud`. The client typically sends `Authorization: Bearer <token>`. Signed tokens are readable. jwt.io says not to put secret information in the payload unless the token is encrypted. GraphQL's HTTP guide puts authentication in middleware before execution, and field authorization inside resolvers. Richardson's API gateway can verify that the client is authorized before fanning out to services.

**When to use:**

Use an authorization server and an access token when a caller should receive limited access without the resource owner's password. Use a JWT when the resource server should check a signature and claims such as `exp` and `aud`. Check the token at the gateway or in authentication middleware, and enforce per-field or per-operation rules in the service.

**Sources:**

- https://www.rfc-editor.org/rfc/rfc6749.html
- https://jwt.io/introduction
- https://microservices.io/patterns/apigateway.html
- https://graphql.org/learn/serving-over-http/
</details>

<details>
<summary>What are some common security concerns when handling inter-service communication?</summary>

### What are some common security concerns when handling inter-service communication?

**Definition:**

RFC 6749's concern is that a caller which holds the resource owner's password can store it, exceed the intended access, and expose it if the caller is compromised. jwt.io's concern is that a signed token's header and payload are readable by anyone who sees the message.

**How it works:**

OAuth replaces the shared password with an access token of limited scope and lifetime, issued by an authorization server. The token is a different credential from the owner's. jwt.io says the signature, HMAC or a public/private key, detects tampering, but secrecy needs encryption. Do not put secrets in a signed payload. Tokens are credentials and should not be kept longer than required. Twelve-factor puts service credentials in environment variables so they are not in the source tree. An API gateway can check authorization before the call is fanned out to internal services. Docker isolates containers with namespaces, which limits what one container can see on the host, but that isolation is not a substitute for the token check.

**When to use:**

Issue a scoped access token instead of sharing a password between services. Sign the token and still treat the payload as public. Keep service credentials in the environment of the deploy that needs them.

**Sources:**

- https://www.rfc-editor.org/rfc/rfc6749.html
- https://jwt.io/introduction
- https://12factor.net/config
- https://microservices.io/patterns/apigateway.html
- https://docs.docker.com/get-started/docker-overview/
</details>

<details>
<summary>Describe how you would use OAuth2 or JWT in a microservices architecture.</summary>

### Describe how you would use OAuth2 or JWT in a microservices architecture.

**Definition:**

OAuth 2.0, in RFC 6749, lets a client obtain limited access to an HTTP resource. JWT, in the jwt.io introduction of RFC 7519, is a compact signed JSON object the client can present as that access token.

**How it works:**

The resource owner approves the client at the authorization server. The server issues an access token with a scope and a lifetime. The client calls the resource server with that token, not with the owner's password. A JWT is `header.payload.signature`. The header carries `alg` and `typ`. The payload carries claims. HMAC uses a secret. RSA or ECDSA uses a key pair, and the signature then also shows that the holder of the private key signed it. The usual HTTP form is `Authorization: Bearer <token>`. Services check the signature, `exp`, `iss`, and `aud`. Because the payload of a signed token is readable, they do not store secrets in it. An API gateway can perform that check once before it routes to a service. GraphQL's guide still wants authentication finished before execution, with finer authorization in the resolver.

**When to use:**

Use the authorization-code style approval when a user delegates access. Use a bearer JWT between services when each service can verify the signature and the claims without storing the owner's password. Encrypt the token if the claims themselves must stay secret.

**Examples:**

```text
Authorization: Bearer <token>
```

**Sources:**

- https://www.rfc-editor.org/rfc/rfc6749.html
- https://jwt.io/introduction
- https://microservices.io/patterns/apigateway.html
- https://graphql.org/learn/serving-over-http/
</details>

<details>
<summary>What mechanisms would you implement to prevent or detect security breaches at the microservices level?</summary>

### What mechanisms would you implement to prevent or detect security breaches at the microservices level?

**Definition:**

RFC 6749 prevents one class of breach by not giving a caller the resource owner's password. jwt.io detects tampering by verifying a signature. Richardson's metrics and tracing patterns detect unexpected behavior after the fact by aggregating operation statistics and tying logs to a request id.

**How it works:**

If a third party stores the owner's password, compromise of that party exposes the password and every resource it protects. An access token is a different credential, with a scope and a lifetime, so one client's token is not the owner's password. A JWT signature is computed over the header and payload. A mismatch means the content changed or the signer is not the holder of the key. Validation checks `exp` and the shape of the token. Verification checks the signature, `iss`, and `aud`. The payload of a signed token is still readable, so secrets do not go in it. An API gateway can reject an unauthorized call before it reaches a service. Application metrics feed a service that reports and alerts. Tracing puts one external request id on every log line so a suspicious call can be followed across services.

**When to use:**

Issue short-lived scoped tokens and verify them on each call. Alert on the operation metrics the metrics pattern aggregates. Search logs by request id when an alert says one call crossed several services.

**Sources:**

- https://www.rfc-editor.org/rfc/rfc6749.html
- https://jwt.io/introduction
- https://microservices.io/patterns/apigateway.html
- https://microservices.io/patterns/observability/application-metrics.html
- https://microservices.io/patterns/observability/distributed-tracing.html
</details>

<details>
<summary>How do you ensure that sensitive data is protected when using microservices?</summary>

### How do you ensure that sensitive data is protected when using microservices?

**Definition:**

Sensitive data in these sources is either a credential, a secret inside a token, or data that should stay near its source. The protections they describe are environment variables, token scope, and not placing secrets in a readable JWT payload.

**How it works:**

Twelve-factor stores credentials for databases and external services in environment variables. The test is that publishing the code would not reveal them. Named environment files are easier to commit by mistake. RFC 6749 avoids copying the resource owner's password into each service. The access token is limited by scope and lifetime. jwt.io says a signed JWT is protected against tampering but anyone can read the header and payload, so secret information does not belong there unless the token is encrypted. AWS's data-lake description says stored data must be secured, cataloged, and placed under access controls, or the lake becomes a data swamp. AWS's edge-computing description says information sent back to a data center can be encrypted before transmission, and sensitive data can stay close to the source.

**When to use:**

Put secrets in the deploy's environment, not in images or source. Pass a scoped token between services. Encrypt a JWT when the claims are themselves sensitive. Keep raw data under the catalog and access controls of the store that holds it.

**Sources:**

- https://12factor.net/config
- https://www.rfc-editor.org/rfc/rfc6749.html
- https://jwt.io/introduction
- https://aws.amazon.com/what-is/data-lake/
- https://aws.amazon.com/what-is/edge-computing/
</details>

### Scalability and Performance

<details>
<summary>How do you ensure that a microservice is scalable?</summary>

### How do you ensure that a microservice is scalable?

**Definition:**

Chris Richardson's Scale Cube scales a service by cloning it, by splitting it, or by giving each copy a subset of the data. Kubernetes keeps a replica count. A twelve-factor process stays stateless so any copy can take the next request.

**How it works:**

X-axis scaling runs N copies behind a load balancer; each handles 1/N of the load. A Kubernetes Deployment creates and destroys Pods to match the desired count, and a Service sends traffic to the Pods that match its selector. Those Pods are fungible for a stateless backend. Session data goes to a backing store such as Redis or Memcached, not into the process. Y-axis scaling is the split into services. Z-axis scaling partitions data so each copy's cache and I/O stay smaller. Lambda functions are a managed form of the same horizontal idea: each invocation is independent and the service adds execution environments as demand grows.

**When to use:**

Add Pods or X-axis copies when the service is stateless and one more copy can take 1/N of the traffic. Partition on the Z axis when each copy would otherwise cache the whole data set. Split on the Y axis when the scale problem is development complexity, not only request rate.

**Sources:**

- https://microservices.io/articles/scalecube.html
- https://kubernetes.io/docs/concepts/services-networking/service/
- https://12factor.net/processes
- https://docs.aws.amazon.com/lambda/latest/dg/welcome.html
</details>

<details>
<summary>What metrics would you monitor to assess a microservice’s performance?</summary>

### What metrics would you monitor to assess a microservice’s performance?

**Definition:**

Richardson's application-metrics pattern says to gather statistics about individual operations and to aggregate them where they can be reported and alerted on. Distributed tracing adds the start and end time of each operation on a request, which is how the pattern locates latency.

**How it works:**

External monitoring only tells overall response time and the number of invocations. That is not enough to see which operation inside the service is slow. The metrics pattern instruments the operations and either pushes the numbers or lets a scraper pull them. Named aggregators are Prometheus and AWS CloudWatch. Tracing records timing for the operations that handled one external request and stores it centrally. The example uses Spring Cloud Sleuth and Zipkin. A canary watches production metrics while load on the new version ramps, including business metrics if you are looking for a regression. Aggregation can need significant infrastructure, and the instrumentation should have minimal runtime overhead.

**When to use:**

Collect per-operation statistics plus the overall response time and invocation count. Add trace timings when you need the latency of a specific hop. Watch the same metrics during a canary before you route the rest of the users.

**Sources:**

- https://microservices.io/patterns/observability/application-metrics.html
- https://microservices.io/patterns/observability/distributed-tracing.html
- https://martinfowler.com/bliki/CanaryRelease.html
</details>

<details>
<summary>Discuss strategies to handle high-load or peak traffic in a microservices architecture.</summary>

### Discuss strategies to handle high-load or peak traffic in a microservices architecture.

**Definition:**

Peak traffic is handled by spreading it across copies, buffering it, or turning off expensive non-critical work. Those are X-axis scaling, messaging, and ops toggles.

**How it works:**

X-axis scaling runs more copies behind a load balancer so each takes a fraction of the load. A Kubernetes Deployment raises the replica count; the Service keeps sending traffic to healthy Pods. NGINX's methods include Round Robin, Least Connections, and Least Time. Richardson's broker buffers messages until a consumer can process them, which absorbs a burst the consumer cannot match immediately. The broker has to stay available. Pete Hodgson's ops toggles, including kill switches, let operators disable a non-critical feature under load without a new deploy. His example is a recommendations panel that is expensive to generate. Lambda functions scale by adding execution environments, with one request per environment at a time.

**When to use:**

Add copies when the service is stateless and more instances can do the work. Buffer with a broker when the work can wait. Flip an ops toggle when a specific feature is the expensive part and operators must turn it off immediately.

**Sources:**

- https://microservices.io/articles/scalecube.html
- https://kubernetes.io/docs/concepts/services-networking/service/
- https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/
- https://microservices.io/patterns/communication-style/messaging.html
- https://martinfowler.com/articles/feature-toggles.html
- https://docs.aws.amazon.com/lambda/latest/dg/welcome.html
</details>

<details>
<summary>How do Microservices handle load balancing?</summary>

### How do Microservices handle load balancing?

**Definition:**

Kubernetes load-balances a Service across the Pods selected for that Service. NGINX load-balances HTTP across an upstream group. Clients use the stable Service or upstream name, not a Pod IP.

**How it works:**

Pods are created and destroyed, and each has its own IP, so a client cannot track them. A Service defines a logical set of endpoints, usually by a label selector, and a port that maps to a `targetPort`. The controller updates EndpointSlices as matching Pods change. For a stateless backend the replicas are fungible. A failed Pod is removed from the endpoints. NGINX, in front of that or instead of it, distributes requests with Round Robin by default, or with Least Connections, Least Time, IP Hash, or Generic Hash. `proxy_pass` names the upstream group. A `backup` server is used when the others are unavailable.

**When to use:**

Use a Service when Pods inside the cluster are the backends and callers should keep using one DNS name. Use NGINX when you need an HTTP algorithm, weights, or a backup upstream. Avoid sticky sessions if the service is twelve-factor: session state belongs in a backing store.

**Examples:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app.kubernetes.io/name: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: http
```

**Sources:**

- https://kubernetes.io/docs/concepts/services-networking/service/
- https://kubernetes.io/docs/concepts/services-networking/cluster-ip-allocation
- https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/
- https://12factor.net/processes
</details>

<details>
<summary>In terms of performance, what would influence your decision to use a message broker vs direct service-to-service communication?</summary>

### In terms of performance, what would influence your decision to use a message broker vs direct service-to-service communication?

**Definition:**

Richardson's messaging pattern uses a broker when the sender and consumer should not have to be available together. Direct synchronous communication requires both for the whole request.

**How it works:**

A broker buffers messages, so a slow or down consumer does not stop the sender, and one publish can reach zero or more subscribers. RabbitMQ's queue is FIFO, though priorities and requeueing can change the order a consumer sees. The performance costs are the broker hop, a broker that must itself be highly available, and more complex request/reply. A direct call has no buffer: the caller waits, and if the callee is slow or down the caller is slow or down. NGINX's note on load balancing is the direct-call side of the same choice: spreading live requests reduces latency when the backends are up to take them.

**When to use:**

Use direct calls when the caller needs the reply before it can continue and both sides are expected to be up. Use a broker when a burst should queue, when the consumer may be down, or when several services should each receive the event.

**Sources:**

- https://microservices.io/patterns/communication-style/messaging.html
- https://www.rabbitmq.com/docs/queues
- https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/
</details>

### Inter-Process Communication

<details>
<summary>What are the advantages and drawbacks of using REST over gRPC in microservice communication?</summary>

### What are the advantages and drawbacks of using REST over gRPC in microservice communication?

**Definition:**

MDN describes REST as architecture constraints that aim at efficient, reliable, and scalable distributed systems, with a resource transferred by standardized client/server interactions. The gRPC site describes gRPC as an RPC framework whose interface and messages are defined in Protocol Buffers and compiled to stubs. HTTP JSON is the comparison point used in the gRPC mobile benchmark.

**How it works:**

A REST-style HTTP API is what MDN says beginners can call with standard web libraries. MDN also says an HTTP API is not automatically REST. GraphQL's HTTP guide notes the contrast: REST is built around resources and URLs, while another style may use one endpoint. gRPC compiles an IDL to language-specific stubs. The gRPC-Web write-up says those calls are strongly typed and the library does serialization, whereas a REST `fetch` of JSON still needs the caller to check that the payload matches the expected shape. The mobile benchmark compares a gRPC unary call with a POST of an equivalent JSON object and measures end-to-end latency and message size over a 60-second ping-pong. Deadlines and retries are part of the gRPC client: `withDeadlineAfter` limits a call, and a retry policy can retry `UNAVAILABLE` with backoff and jitter.

**When to use:**

Use HTTP APIs that follow REST's resource constraints when clients are ordinary HTTP tools and a URL identifies the resource. Use gRPC when both sides can share a protobuf contract and you want generated stubs, deadlines, and retries. The benchmark's method is to measure latency and message size on your payload rather than to assume one is always smaller.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/REST
- https://github.com/grpc/grpc.io/blob/main/content/en/blog/mobile-benchmarks.md
- https://github.com/grpc/grpc.io/blob/main/content/en/blog/postman-grpcweb.md
- https://github.com/grpc/grpc.io/blob/main/content/en/docs/guides/retry.md
- https://github.com/grpc/grpc.io/blob/main/content/en/blog/deadlines.md
</details>

<details>
<summary>How would you implement versioning in microservices API?</summary>

### How would you implement versioning in microservices API?

**Definition:**

Danilo Sato versions a running system by keeping the old and new versions up together and moving traffic, rather than by replacing every caller at one instant. Kubernetes versions a port binding by name so clients survive a change of the container port.

**How it works:**

A canary runs the new version for a subset of users, a region, or a brand, and can run more than two versions, though Sato says to keep that number small. Rollback is routing back to the old version. Installed and mobile clients upgrade on their own schedule. Sato's parallel change supports both client versions on the backend until usage of the old one falls, and then the backend contracts to the new version. The database uses the same parallel change. A Kubernetes Service maps its `port` to a named `targetPort`. The next backend can listen on a different number without breaking clients that still use the Service port.

**When to use:**

Run two versions when you can route a slice of traffic and roll it back. Support both contracts until old clients drop off, then remove the old one. Name the Pod port when the container port will change independently of the Service port.

**Sources:**

- https://martinfowler.com/bliki/CanaryRelease.html
- https://kubernetes.io/docs/concepts/services-networking/service/
</details>

<details>
<summary>What are the challenges of network latency in microservices and how can they be mitigated?</summary>

### What are the challenges of network latency in microservices and how can they be mitigated?

**Definition:**

Richardson's API gateway page says a WAN is much slower than a LAN, and a mobile network is slower and higher latency than a non-mobile network, so a client that makes many fine-grained calls pays for each round trip. An API gateway adds one more hop of its own.

**How it works:**

Fine-grained services force a client to call several of them for one screen. A server-side web app on a LAN can do that without the user noticing. A mobile client cannot. The gateway fans out server-side and returns one response, which cuts the client's round trips. Richardson says the gateway's own extra hop is insignificant for most applications. CloudFront reduces latency by serving from the edge location closest to the viewer, or by fetching from the origin only on a miss. AWS edge computing moves processing next to the device so less data crosses the network. Tracing records start and end times so you can see which hop is the latency. gRPC clients set a deadline so a slow hop does not wait forever.

**When to use:**

Put a gateway in front of mobile and other high-latency clients. Cache at the edge when the same content is fetched from far away. Trace before you add copies, so you know which call is slow. Set a deadline on an RPC that must finish within a bound.

**Sources:**

- https://microservices.io/patterns/apigateway.html
- https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html
- https://aws.amazon.com/what-is/edge-computing/
- https://microservices.io/patterns/observability/distributed-tracing.html
- https://github.com/grpc/grpc.io/blob/main/content/en/blog/deadlines.md
</details>

<details>
<summary>Explain the difference between message queues and event buses. In which scenarios would you use each?</summary>

### Explain the difference between message queues and event buses. In which scenarios would you use each?

**Definition:**

RabbitMQ defines a queue as an ordered collection: messages are enqueued at the tail and dequeued from the head, FIFO unless priorities or requeueing change what a consumer observes. Richardson's publish/subscribe style sends one message to zero or more recipients. That is the split these sources actually draw.

**How it works:**

A queue is the buffer a consumer reads. RabbitMQ says topic-based protocols such as STOMP and MQTT still use a queue as the accumulation buffer for consumers. Streams are a different RabbitMQ structure, not a queue. Publish/subscribe on Richardson's list does not expect a single consumer to drain one FIFO. A notification goes to a recipient and expects no reply. Request/response expects a prompt reply.

**When to use:**

Use a queue when one stream of work should be consumed in order by a worker that may be busy, because the queue holds the messages. Use publish/subscribe when zero or more services should each see the event, as when `OrderService` publishes `Order Created`. Use request/response on the same channel only when you accept the extra complexity Richardson names for reply-style messaging.

**Sources:**

- https://www.rabbitmq.com/docs/queues
- https://microservices.io/patterns/communication-style/messaging.html
</details>

<details>
<summary>How can transactional outbox patterns be used in microservices?</summary>

### How can transactional outbox patterns be used in microservices?

**Definition:**

microservices.io's transactional outbox updates the database and records the outgoing message in the same local transaction. A separate message relay later delivers those rows to the broker. The pattern exists because a 2PC transaction across the database and the broker is often unavailable or undesirable, and sending the message before or after the commit can lose it.

**How it works:**

The sender writes business rows and an outbox row together. In a relational database the outbox is a table; in a document store it can be a property of the record. The relay reads the outbox and publishes. Messages leave in the order the application recorded them, including across instances that update the same aggregate. The relay can publish twice if it crashes after publish and before marking the row done, so consumers must be idempotent. The architecture page says saga, command-side replica, and CQRS typically need this pattern to update state and send a message atomically.

**When to use:**

Use it when a command must both change local state and emit an event or saga message. The page's drawback is that a developer can forget to write the outbox row after changing the entity.

**Examples:**

```text
BEGIN
  UPDATE orders ...
  INSERT INTO outbox(aggregate_id, type, payload)
COMMIT
-- relay process publishes outbox rows to the broker
```

**Sources:**

- [microservices.io — Transactional outbox](https://microservices.io/patterns/data/transactional-outbox.html)
- [microservices.io — Microservice Architecture](https://microservices.io/patterns/microservices.html)

</details>

### Resiliency and Reliability

<details>
<summary>How would you design a microservice to be fault-tolerant?</summary>

### How would you design a microservice to be fault-tolerant?

**Definition:**

A fault-tolerant microservice keeps serving when a process, a Pod, or a callee fails. The mechanisms in the fetched pages are several replicas, a Service that drops dead endpoints, a broker buffer, and a database standby.

**How it works:**

Kubernetes restarts a failed container according to `restartPolicy` and replaces a failed Deployment Pod so the replica count holds. The Service removes that Pod from its endpoints. NGINX can keep a backup upstream. Messaging decouples the sender from the consumer: the broker holds the message until the consumer is up. The broker must be highly available or it becomes the new single point. PostgreSQL promotes a standby when the primary fails. Synchronous replication does not lose committed transactions; asynchronous replication might. Z-axis partitioning limits a failure to one subset of the data. gRPC retries `UNAVAILABLE` and a deadline stops a call that will not finish.

**When to use:**

Run more than one Pod for a stateless service. Buffer work that can wait. Add a standby for the database and choose sync or async from the loss you can accept. Set a deadline and a bounded retry on calls to another service.

**Sources:**

- https://kubernetes.io/docs/concepts/architecture/self-healing
- https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/
- https://microservices.io/patterns/communication-style/messaging.html
- https://www.postgresql.org/docs/current/high-availability.html
- https://github.com/grpc/grpc.io/blob/main/content/en/docs/guides/retry.md
- https://microservices.io/articles/scalecube.html
</details>

<details>
<summary>Discuss the importance of timeouts and retry logic in a microservices architecture.</summary>

### Discuss the importance of timeouts and retry logic in a microservices architecture.

**Definition:**

gRPC deadlines bound how long a call may run. A retry policy repeats a failed call for specific statuses, with backoff, instead of failing the caller on the first error.

**How it works:**

A Java client sets the bound with `withDeadlineAfter`. The call then has that many milliseconds. The documented retry policy example uses `maxAttempts` 4, `initialBackoff` 0.1s, `maxBackoff` 1s, `backoffMultiplier` 2, and retries only `UNAVAILABLE`. Jitter of plus or minus 20 percent is applied, so a 100 ms initial backoff lands between 80 ms and 120 ms. Without a deadline, a slow callee holds the caller. Without a bounded retry, a brief `UNAVAILABLE` fails the request, and an unbounded retry can amplify load. Messaging is the alternative when the work should wait in a broker instead of being retried in line.

**When to use:**

Set a deadline on every synchronous call that must finish or fail. Retry only the statuses the policy lists, with a cap and backoff. Do not retry a call whose work may already have been applied unless the operation can be repeated safely; the retry page shows the status filter, not a general rule that every call is safe to repeat.

**Examples:**

```json
"retryPolicy": {
  "maxAttempts": 4,
  "initialBackoff": "0.1s",
  "maxBackoff": "1s",
  "backoffMultiplier": 2,
  "retryableStatusCodes": ["UNAVAILABLE"]
}
```

**Sources:**

- https://github.com/grpc/grpc.io/blob/main/content/en/blog/deadlines.md
- https://github.com/grpc/grpc.io/blob/main/content/en/docs/guides/retry.md
- https://microservices.io/patterns/communication-style/messaging.html
</details>

<details>
<summary>What strategies can be used to achieve high availability in microservices?</summary>

### What strategies can be used to achieve high availability in microservices?

**Definition:**

High availability in these sources means a second copy can take the work when the first fails, and clients keep a stable address while Pods come and go.

**How it works:**

PostgreSQL keeps a standby that can take over if the primary fails. A hot standby can already serve reads. Synchronous replication commits only after every server commits, so failover loses no data. Asynchronous replication can lose transactions that have not arrived. Kubernetes keeps a Deployment's replica count by replacing a failed Pod and removes that Pod from Service endpoints so callers hit a live one. A Service is the stable endpoint in front of changing Pod IPs. NGINX adds a backup server in the upstream group. A message broker buffers work until the consumer is up, and that broker has to be highly available itself.

**When to use:**

Add replicas and a Service for a stateless microservice. Add a database standby when the data has one primary. Choose synchronous replication when losing a committed write is unacceptable.

**Sources:**

- https://www.postgresql.org/docs/current/high-availability.html
- https://kubernetes.io/docs/concepts/architecture/self-healing
- https://kubernetes.io/docs/concepts/services-networking/service/
- https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/
- https://microservices.io/patterns/communication-style/messaging.html
</details>

<details>
<summary>How do you approach disaster recovery in a microservices-based system?</summary>

### How do you approach disaster recovery in a microservices-based system?

**Definition:**

PostgreSQL's disaster path is failover: a standby takes over when the primary fails. Kubernetes's path is to restart or replace the failed unit and, when a node dies, reattach its volume to a Pod on another node.

**How it works:**

A warm standby cannot be connected to until it is promoted. A hot standby can already answer read-only queries. Synchronous replication means a committed transaction is on every server, so the promotion does not drop it. Asynchronous replication can lose transactions that were committed on the primary and not yet on the standby. Kubernetes restarts a container per `restartPolicy`, replaces a Deployment Pod, and can move a PersistentVolume off a failed node. Docker containers lose any state that was not in persistent storage when they are removed, so recovery depends on that volume or on a backing database. A canary's rollback, rerouting users to the old version, is the recovery for a bad release rather than for a dead data center.

**When to use:**

Promote a standby when the primary is the thing that failed. Use synchronous replication when that promotion must not lose commits. Keep service state in a volume or a database, because a replaced container does not keep its writable layer. Reroute a canary when the failure is the new version, not the host.

**Sources:**

- https://www.postgresql.org/docs/current/high-availability.html
- https://kubernetes.io/docs/concepts/architecture/self-healing
- https://docs.docker.com/get-started/docker-overview/
- https://martinfowler.com/bliki/CanaryRelease.html
</details>

<details>
<summary>Explain how you would handle a cascading failure in a microservice ecosystem.</summary>

### Explain how you would handle a cascading failure in a microservice ecosystem.

**Definition:**

A cascade here is one failed service making its callers fail too. Richardson's synchronous style has that coupling, because both sides must be up for the whole request. The sourced breaks are a broker buffer, a deadline, a bounded retry, and removing the dead Pod from the load-balancing set.

**How it works:**

If the caller waits on the callee, the callee's outage becomes the caller's outage, and that caller's callers fail next. Messaging stores the message until the consumer can process it, so the sender is not blocked on the consumer being up. The broker has to stay up. A gRPC deadline ends a call that will not finish. A retry policy that only retries `UNAVAILABLE`, with a max attempt count and backoff, avoids turning one outage into a retry storm; the documented example caps attempts at 4. Kubernetes removes a failed Pod from Service endpoints so the remaining calls go to healthy Pods. An ops toggle can turn off an expensive non-critical feature, which Hodgson compares to a manually managed circuit breaker, so that feature stops calling a struggling dependency.

**When to use:**

Buffer work that can wait. Put a deadline on work that cannot. Cap retries. Drop dead endpoints. Disable a non-critical caller when the dependency is the thing that is failing.

**Sources:**

- https://microservices.io/patterns/communication-style/messaging.html
- https://github.com/grpc/grpc.io/blob/main/content/en/blog/deadlines.md
- https://github.com/grpc/grpc.io/blob/main/content/en/docs/guides/retry.md
- https://kubernetes.io/docs/concepts/architecture/self-healing
- https://martinfowler.com/articles/feature-toggles.html
</details>

### Observability and Monitoring

<details>
<summary>What tools or practices would you recommend for logging in a distributed microservices system?</summary>

### What tools or practices would you recommend for logging in a distributed microservices system?

**Definition:**

Richardson's distributed-tracing pattern starts from the fact that log entries for one request are scattered across numerous logs. The practice is to put one external request id on every log line and to keep that id as the request moves between services.

**How it works:**

Each external request gets a unique id. Every service that handles it receives the id and writes it into its log messages. Timing for the operations is also recorded in a central service. The example uses Spring Cloud Sleuth to instrument Spring components and RabbitMQ to deliver traces to a Zipkin server. Developers then search the aggregated logs for that id. The pattern says the instrumentation should have minimal runtime overhead, and that storing the traces can require significant infrastructure. Application metrics are the companion for numbers and alerts; they are not a substitute for the id that joins the logs.

**When to use:**

Put the request id on every log line as soon as a request crosses more than one service. Ship the lines to one place you can search. Sample if the overhead or the storage is too high; the example sets the sampler to all requests, which is a configuration choice, not a requirement to keep every trace forever.

**Sources:**

- https://microservices.io/patterns/observability/distributed-tracing.html
- https://microservices.io/patterns/observability/application-metrics.html
</details>

<details>
<summary>How do you trace requests across boundaries of different microservices?</summary>

### How do you trace requests across boundaries of different microservices?

**Definition:**

Distributed tracing assigns each external request a unique id, passes that id to every service involved, and records the operations in one place so a request can be followed across service boundaries.

**How it works:**

The id is included in every log message. Start and end times of the operations performed for that request go to a centralized service. In the example, Spring Cloud Sleuth instruments Spring components and can deliver traces to Zipkin. RabbitMQ carries the traces. The sample configuration sets `SPRING_SLEUTH_ENABLED` and a sampler percentage, and it skips paths such as health and metrics. Searching aggregated logs by the external request id shows how that one request was handled. The benefit the pattern names is insight into behavior, including the sources of latency. External monitoring of response time and invocation count does not show the individual operations.

**When to use:**

Trace when a user-facing call is handled by more than one service and you need to see which service added the time. Pass the same id on every outbound call that belongs to that request.

**Sources:**

- https://microservices.io/patterns/observability/distributed-tracing.html
</details>

<details>
<summary>Discuss the importance of metrics and alerts in maintaining a microservices architecture.</summary>

### Discuss the importance of metrics and alerts in maintaining a microservices architecture.

**Definition:**

Richardson's application-metrics pattern exists so you can understand behavior and troubleshoot. It aggregates statistics and provides reporting and alerting. Without that, external monitoring only shows overall response time and how many times something was called.

**How it works:**

Each service is instrumented around individual operations. Aggregation is push or pull. Prometheus and AWS CloudWatch are the named aggregation services. Alerts come from that centralized service. The drawbacks are metrics code mixed into business logic and the infrastructure aggregation can require. A canary uses the same idea in release: ramp traffic and watch metrics, including business metrics, and route back to the old version if the new one regresses. Tracing complements alerts by showing which hop of one request was slow once an alert fires.

**When to use:**

Alert on the aggregated operation statistics rather than on a single instance's log. Keep the instrumentation cheap. Use the alert as the point where you open a trace for one request id.

**Sources:**

- https://microservices.io/patterns/observability/application-metrics.html
- https://martinfowler.com/bliki/CanaryRelease.html
- https://microservices.io/patterns/observability/distributed-tracing.html
</details>

<details>
<summary>How do you handle performance bottlenecks in microservices?</summary>

### How do you handle performance bottlenecks in microservices?

**Definition:**

A bottleneck is the operation that dominates the time of a request. Tracing is how Richardson's pattern finds it. The fixes in the fetched pages are fewer client round trips, more copies, a closer cache, or a deadline so a slow hop cannot run without bound.

**How it works:**

External monitoring shows only the overall response time. Tracing records start and end for each operation and names the sources of latency. An API gateway collapses many fine-grained calls into one response for a high-latency client. X-axis copies behind a load balancer split traffic; NGINX can choose least time or least connections. Z-axis partitioning shrinks the data each copy touches, which improves cache use and cuts I/O. CloudFront serves a cached object from the nearest edge. A gRPC deadline stops a call that has already taken too long.

**When to use:**

Trace first and name the slow operation. Add copies when that operation is CPU or concurrency on one instance. Cut round trips when the client is chatty. Cache at the edge when the same bytes are fetched from far away. Partition when one copy's data set is the limit.

**Sources:**

- https://microservices.io/patterns/observability/distributed-tracing.html
- https://microservices.io/patterns/apigateway.html
- https://microservices.io/articles/scalecube.html
- https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/
- https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html
- https://github.com/grpc/grpc.io/blob/main/content/en/blog/deadlines.md
</details>

<details>
<summary>What is distributed tracing and which tools help you accomplish it in a microservices setup?</summary>

### What is distributed tracing and which tools help you accomplish it in a microservices setup?

**Definition:**

microservices.io's distributed tracing pattern instruments services so one external request can be followed across process boundaries. External monitoring only shows overall latency and call counts, and a request's log lines are scattered.

**How it works:**

Each external request gets an id. That id is passed to every service that helps handle the request and is written into log lines. Start and end times of the operations are recorded in a central service. The example on the page uses Spring Cloud Sleuth to instrument Spring components and a Zipkin server to collect traces, with RabbitMQ carrying them. `SPRING_SLEUTH_SAMPLER_PERCENTAGE` is set to 1 in that example, which samples every request. The benefits are latency broken down by operation and the ability to search aggregated logs by the request id. The issue the page names is that storing traces can need significant infrastructure.

**When to use:**

Use it when a request crosses several services and the overall response time is not enough to see which hop is slow. The pattern asks for minimal runtime overhead; sampling is the knob the example configures.

**Examples:**

```text
gateway assigns request id
Order Service and Customer Service propagate the same id
Zipkin stores the spans
```

**Sources:**

- [microservices.io — Distributed tracing](https://microservices.io/patterns/observability/distributed-tracing.html)

</details>

### Containers and Orchestration

<details>
<summary>Explain the role of Docker in developing and deploying microservices.</summary>

### Explain the role of Docker in developing and deploying microservices.

**Definition:**

Docker's documentation defines a container as a loosely isolated process with the files it needs to run. Containers on one host share the kernel, unlike a virtual machine, which ships a whole operating system and kernel. The image is what you distribute; the container is the running unit.

**How it works:**

The same container is the unit of development, test, and production deploy, whether production is a local data center, a cloud, or a hybrid. Docker's lifecycle events include create, start, stop, die, kill, and destroy. The microservice architecture page wants each service independently deployable with its own pipeline; a container is the package that pipeline ships. Docker's comparison with virtual machines is that isolating one process in a VM wastes a kernel, while several containers share the host kernel. Cloud machines are often still VMs that then run a container runtime.

**When to use:**

Use a container per service when the service must run the same way on a laptop and in production and must deploy without dragging the rest of the application. Orchestrating many of those containers is a separate step from building the image.

**Examples:**

```text
docker build  -> image
docker run    -> container (the service process + its files)
```

**Sources:**

- [Docker docs — What is a container?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/)
- [Docker docs — Docker overview](https://docs.docker.com/get-started/docker-overview/)
- [microservices.io — Microservice Architecture](https://microservices.io/patterns/microservices.html)

</details>

<details>
<summary>How do container orchestration tools like Kubernetes help with microservice deployment?</summary>

### How do container orchestration tools like Kubernetes help with microservice deployment?

**Definition:**

Kubernetes runs a microservice as Pods whose count and addresses change, and it gives clients a Service so they do not track those addresses. A Deployment is the object that keeps the desired number of Pods.

**How it works:**

Pods are ephemeral. Each has its own IP. A Deployment creates and destroys them to match the desired state. If one fails, Kubernetes creates a replacement. A Service selects Pods by labels and publishes one endpoint. Its controller updates EndpointSlices as the set changes. A failed Pod leaves the endpoints. The Service can map its port to a named container port, so a later image can listen on a different port without breaking clients. Docker's role is the image: the same container is what you tested and what the cluster runs. A canary is a documented use of Services, selecting which Pods receive the new traffic.

**When to use:**

Use a Deployment and a Service when more than one copy of a microservice should run and callers should keep a stable address. Use the Service selector for a canary split. Do not point clients at a Pod IP.

**Sources:**

- https://kubernetes.io/docs/concepts/services-networking/service/
- https://kubernetes.io/docs/concepts/architecture/self-healing
- https://docs.docker.com/get-started/docker-overview/
</details>

<details>
<summary>Describe the lifecycle of a container within a microservices architecture.</summary>

### Describe the lifecycle of a container within a microservices architecture.

**Definition:**

Docker's container lifecycle is create, start, stop, and delete a runnable instance of an image. Kubernetes wraps that instance in a Pod that it will restart or replace, and it can run a hook before the container stops.

**How it works:**

`docker run` pulls the image if needed, creates the container, adds a read-write filesystem layer, attaches a network interface and an IP, and starts the process. `exit` stops the container and does not remove it. You can start it again or remove it. Removing it drops state that was not in persistent storage. A Kubernetes Deployment owns the Pods. If the container fails, `restartPolicy` decides the restart. If the Pod is gone, a replacement is created to keep the replica count. The frontend sample uses a `preStop` hook that runs `nginx -s quit` so the process can leave before the Pod is removed. The Service stops sending traffic once the Pod is no longer a healthy endpoint.

**When to use:**

Treat the image as the thing you ship and the container as the thing you start and stop. Put data that must survive removal in a volume or a database. Use `preStop` when the process should finish in-flight work before the orchestrator removes the Pod.

**Examples:**

```console
$ docker run -i -t ubuntu /bin/bash
```

**Sources:**

- https://docs.docker.com/get-started/docker-overview/
- https://kubernetes.io/docs/concepts/architecture/self-healing
- https://kubernetes.io/docs/tasks/access-application-cluster/connecting-frontend-backend
</details>

<details>
<summary>How do you ensure that containers are secure and up-to-date?</summary>

### How do you ensure that containers are secure and up-to-date?

**Definition:**

Docker's security claim for a container is isolation: namespaces give the container its own workspace, and by default it is isolated from other containers and the host. Staying up to date, in Docker's delivery description, means building a new image and pushing that image, not patching a running container in place.

**How it works:**

An image is built in layers from a Dockerfile. A change rebuilds only the layers that changed. Registries store the image. Docker Hub is the default public registry; you can run a private one. `docker pull` fetches the image you are about to run. The tested image is the one pushed to production. Sato's canary then rolls that new image out to a subset first, and rollback is a route back to the old image. Isolation is not a token check: credentials still belong in environment variables, and service calls still present an access token. Lambda's welcome page says Lambda itself patches the servers under functions. That patching statement is about Lambda's infrastructure, not about a container image you built.

**When to use:**

Rebuild and push an image when the base or the application must change. Run a private registry when images should not be public. Roll the new image out with a canary. Keep secrets out of the image layers.

**Sources:**

- https://docs.docker.com/get-started/docker-overview/
- https://martinfowler.com/bliki/CanaryRelease.html
- https://12factor.net/config
- https://docs.aws.amazon.com/lambda/latest/dg/welcome.html
</details>

<details>
<summary>What are the best practices for container networking in the context of microservices?</summary>

### What are the best practices for container networking in the context of microservices?

**Definition:**

Docker gives each new container a network interface and an IP on the default network unless you say otherwise. Kubernetes does not make clients use that Pod IP. A Service is the stable endpoint in front of the Pods.

**How it works:**

On `docker run` with no network options, Docker attaches the container to the default network, assigns an IP, and lets it reach external networks through the host. You can control how isolated that network is. In Kubernetes each Pod also gets an IP, and that IP changes as Pods are replaced. A Service selects Pods by labels, maps a port to a `targetPort`, and load-balances across them. EndpointSlices track the current set. A failed Pod is removed. If the workload is HTTP, Ingress is an entry point in front of Services, not a Service type, and it gathers routing rules behind one listener. Callers inside the cluster use the Service. Twelve-factor services stay stateless so any Pod behind that Service can handle the next request. An API gateway is the application-level entry when clients outside should not know the internal partition.

**When to use:**

Publish a Service for every set of Pods that other services call. Do not bake a Pod IP into configuration. Put Ingress or an API gateway at the edge when outside clients need one listener. Keep the container's network isolated to what that service must reach.

**Sources:**

- https://docs.docker.com/get-started/docker-overview/
- https://kubernetes.io/docs/concepts/services-networking/service/
- https://12factor.net/processes
- https://microservices.io/patterns/apigateway.html
</details>
