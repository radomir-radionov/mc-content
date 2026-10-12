# Software Architecture — Interview

Interview answers in this file are filled.

[← Back to hub](./0_system_design.md)

### Related docs

- [Software Architecture — Interview](./0_sd_architecture_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

### Software Architecture Fundamentals

<details>
<summary>What is the difference between software architecture and software design?</summary>

### What is the difference between software architecture and software design?

**Definition:**

Software architecture and software design are both vital components of the software development process. While there is some overlap between the two, they cater to different aspects of project management and execution.

**How it works:**

#### Core Distinctions

- **Scope**: Architecture defines the macro structure of the system, whereas design zooms in on specific components or modules.

- **Focus**: Architecture concentrates on high-level concepts, like system requirements and global decisions, while design is concerned with the detailed mechanism and strategies for each system component.

- **Abstraction Levels**: The architecture operates on the high-level abstractions, focusing on the overall system, where the design is generally on the low-level abstractions dealing with detailed mechanisms.

- **Design Goals**: The ultimate objective of architecture is to ensure that the system's global structure supports its requirements, while design aims at reaching the specific module-level functionalities and behaviors.

#### Visual Representation

- **Architecture**: Often described via diagrams such as UML Component or Deployment Diagrams, architecture designs provide a top-down visualization of the system.

- **Design**: Realized through UML class and sequence diagrams, these illustrate module-level details and internal functionalities.

#### Ownership and Duration

- **Architecture**: Typically conceptualized by senior engineers or architects; it tends to remain relatively stable throughout the project.

- **Design**: Involving more granular, frequently changing details, designs are often implemented and owned by individual or small teams.

#### Event-based Modeling

- **Architecture**: Key operations and system-wide events are handled, showcasing high-level transitions and behavioral triggers.

- **Design**: Delivers more in-depth insights into individual modules, including state transitions and behavior specifics.

#### Flexibility and Refactoring

A well-architected system might limit the degree of flexibility, ensuring consistency and adherence to architectural design decisions.

- **Design**: Offers a more modular, adaptable approach, with components open to individual changes and refactoring.

#### The Process

- **Architecture**: Typically a "big picture" approach, involves the decisions and strategies conceptualized during the early stages of software development.

- **Design**: A continuous, iterative process, often refined and expanded as the project evolves and features develop.

#### Change Management

- **Architecture**: Endorses stable, long-lasting system structures, making any modifications a multi-stakeholder decision due to potential widespread effects.

- **Design**: Allows for regular, more localized updates, with changes mainly affecting components or modules.

</details>

<details>
<summary>Explain separation of concerns in software architecture.</summary>

### Explain separation of concerns in software architecture.

**Definition:**

Separation of Concerns (SoC) is a fundamental design principle that advocates for the division of a system into distinct sections—each addressing a particular set of functionalities.

**How it works:**

#### Elements of SoC

- **Single Responsibility**: Modules, classes, and methods should only have one reason to change. For example, a User class should handle user data, but not also display user data.

- **Low Coupling**: Components should minimize their interdependence.

- **High Cohesion**: Elements within a component should pertain to the same functionality.

#### Improvement:

- **Separate Class for Admin Role**: This ensures that the User class only represents user data, realizing Single Responsibility.

**Examples:**

#### Code Example: Concerns Separation

Here is the Python code:

```python
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email
        self.is_admin = False  # Representing role this way couples different concerns

    def display_info(self):
        if self.is_admin:  # Role-based display couples concerns
            print(f"Admin User: {self.name}")
        else:
            print(f"Regular User: {self.name}")
```

```python
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email

class Admin(User):
    def display_info(self):
        print(f"Admin User: {self.name}")
```

</details>

<details>
<summary>Define a system quality attribute and its importance in software architecture.</summary>

### Define a system quality attribute and its importance in software architecture.

**Definition:**

System quality attributes, often referred to as non-functional requirements, complement functional requirements in shaping a system's architecture and design. They define characteristics centering around reliability, maintainability, and other performance aspects.

Developers aim to ensure these attributes right from the conceptual stages, thus they shape the software's foundation and guide architectural decision-making throughout the development cycle.

**How it works:**

#### Importance of Quality Attributes

- **Holistic User Experience:** While functional requirements capture what the system should do, quality attributes capture how well it should do it. Together, these define a user's complete experience with the system.

- **Design Focus:** Quality attributes help guide the system design, ensuring that not only the main features but also the system's overall environment, security, and utility are optimized and secure.

- **Balanced Decisions:** Engineering decisions must often balance competing objectives. Quality attributes help communicate these objectives, making it easier for architects and developers to make informed decisions.

- **Technical Compatibility:** Different quality attributes can complement or contradict each other, and it's important to balance them to ensure a cohesive, efficient system.

#### Common System Quality Attributes

- **Performance**: Describes the system's responsiveness, throughput, and resource consumption levels, typically under specific conditions. For instance, the system might need to perform optimally when handling a large number of concurrent users or a heavy workload.

- **Reliability**: Refers to the system's ability to perform consistently and accurately, without unexpected failures. Systems with high reliability often integrate fault tolerance mechanisms and have a defined recovery strategy in place, like data backups or redundant components.

- **Availability**: This attribute specifies the system's uptime and accessibility. It's often expressed as a percentage of time the system is expected to be operational. For example, "99.99% uptime."

- **Security**: A mandatory system attribute that addresses the protection of data and resources from unauthorized access, breaches, or corruption. It is vital for systems where data confidentiality, integrity, and availability are paramount.

- **Maintainability**: Represents a system's ease of maintaining or modifying its components. It focuses on the efficiency of repairs, upgrades, and adaptions. Key metrics include time for updates, code complexity post-changes, and number of errors after a modification.

- **Portability**: Defines a system's adaptability to run across different environments, such as diverse hardware, operating systems, or cloud providers. A more portable system is generally preferred as it offers flexibility and future-proofing.

- **Scalability**: Refers to the system's ability to accommodate growing workloads. It might be realized through vertical scaling (upgrading hardware) or horizontal scaling (adding more instances).

- **Usability**: Emphasizes the system's ease of use and intuitive operation, catering to user experience aspects.

- **Interoperability**: Describes a system's capability to communicate and share data with other systems or components, and its compatibility with different technologies.

- **Testability**: The degree to which a system facilitates the generation of test cases and testing processes.

- **Flexibility**: Represents the system's capacity to adapt to new situations through customization.

#### Key Performance Indicators

Each quality attribute can measure its adherence, typically using quantitative metrics or key performance indicators (KPIs):

- **Performance**: Utilization metrics, response times, and throughput.

- **Reliability**: Measured often in uptime percentages.

- **Availability**: Can be measured using uptime metrics, such as "five nines" (99.999%).

- **Security**: Can be evaluated using penetration testing results, compliance indicators, security frameworks adhered to, and specific security protocols' success rates.

#### Architectural Decisions

Architectural patterns and styles, as well as design strategies, are thoroughly informed by quality attributes, ensuring that the completed software system best meets its operational goals.

- **High availability:** A system focusing on high availability like a cloud-based ERP might adopt a microservices architecture and utilize load balancers and auto-scaling clusters.

- **High reliability:** A system that requires high reliability, such as a medical equipment monitoring system, might employ a modular architecture with strict data consistency mechanisms and undergo stringent testing procedures.

</details>

<details>
<summary>Describe the concept of a software architectural pattern.</summary>

### Describe the concept of a software architectural pattern.

**Definition:**

A Software Architectural Pattern is a proven, structured solution to a recurring design problem. These patterns offer a blueprint for conceptualizing systems and addressing common challenges in software architecture. They provide a vocabulary for developers and ensure commonly-faced problems are solved in a consistent manner.

**How it works:**

#### Common Architectural Patterns

- **Layers**: Segregates functionality based on roles like presentation, domain logic, and data access.

- **MVC**: Divides an application into three interconnected components: Model, View, and Controller, each with specific responsibilities.

- **REST**: Utilizes common HTTP verbs and status codes, along with stateless communication, for easy data transfer in client-server setups.

- **Event-Driven**: Emphasizes communication through events, with publishers and subscribers decoupled from one another.

- **Microkernel**: Centralizes core operations in a lightweight kernel, while other services can be dynamically loaded and interact via messaging.

- **Microservices**: Distributes applications into small, independently deployable services that communicate via network calls.

- **Space-Based**: Leverages a distributed data grid for data sharing and event-driven workflows.

- **Client-Server**: Divides an application into client-side and server-side components, with the server providing resources or services.

- **Peer-to-Peer (P2P)**: Emphasizes equal share in roles for nodes, promoting decentralized communication and resources sharing.

- **Domain-Driven Design (DDD)**: Encourages close alignment between development and a domain model, integrating logic and data into one unit.

#### Best Practices for Architectural Patterns

- **Understanding before Application**: Ensure you are truly solving a problem specific to your context, and not adopting a pattern prematurely or needlessly complicating your design.

- **Design Flexibility**: A good architecture allows for future changes and is not overly rigid or exhaustive without reason.

- **Code Reusability**: Aim to minimize duplication by design and code reuse strategies.

- **Separation of Concerns**: Each component should have a clear, singular role, and should need to understand as little as possible about the rest of the system.

- **Scalability**: The architecture should be able to scale with complexity, requirements, and user load.

- **Maintainability**: It should be relatively easy to debug, enhance, and maintain the system.

- **Security and Compliance**: Your architecture should account for security standards in your domain, including data protection laws and best practices.

- **Clear Communication**: Developers should share a consistent vocabulary to understand the architecture, especially when collaborating on the system.

**When to use:**

#### Real-World Examples

- **MVC**: It is widely used in web applications, where the model represents data, the view displays the data, and the controller handles user inputs.

- **Microservices**: This architecture is prevalent in cloud-based systems like Netflix and Amazon. Services are loosely coupled and focus on specific business functionalities.

- **Event-Driven**: Used in various applications like chat systems, stock trading platforms, and IoT solutions where real-time data processing is crucial.

- **Space-Based**: Apache Spark and other big data technologies often use it for stream and batch processing.

- **Client-Server**: Common in web and mobile applications, where the server serves as a centralized resource.

- **P2P**: Popular in file-sharing applications and some blockchain implementations like BitTorrent and Bitcoin.

- **DDD**: Many enterprise-level applications, CRM systems, and portfolio management tools leverage this model for a more domain-focused design approach.

</details>

<details>
<summary>What is the layered architectural pattern?</summary>

### What is the layered architectural pattern?

**Definition:**

The Layered Architecture Pattern is characterized by the hierarchical organization of the software components into distinct layers, each serving a specific role and potentially necessitating communication with adjacent layers. It's also known as the N-Tier Architecture or the Multi-Tier Architecture.

**How it works:**

#### Key Components

- **Layers**: The logical groupings of software elements that collaborate to perform specific tasks or operations. There can be any level of separation, with most applications distinguishing between three primary layers: Presentation, Business Logic, and Data.

- **Inter-Layer Communication**: Layers communicate with one another in a strictly defined order. Typically, lower layers serve as a foundation and are only aware of themselves and those directly above (direct dependency), while higher layers are cognizant of the layers beneath them, often using defined interfaces (dependencies could be direct or transitive).

#### Advantages

- **Modularity**: By compartmentalizing functionalities, the architecture enhances manageability and promotes code reusability.

- **Isolation of Concerns**: Each layer focuses on a specific aspect of the application, aiding in code simplicity and maintainability.

- **Flexibility in Development and Updating**: Since layers are relatively independent, teams can work on different layers concurrently, and modifications are contained within specific areas, reducing the probability of ripple effects.

- **Consistently Defined Structure**: The anticipated interactions between the various layers are consistently outlined, offering a robust blueprint for development and maintenance processes.

- **Scalability**: The architecture can adapt to scaling demands. For instance, if the business layer is strained, more resources can be allocated to it without necessitating changes in the presentation or data layers.

#### Drawbacks

- **Potential Overhead**: The need for data and control flow to traverse layers might lead to performance implications.

- **Rigidity in Change Management**: Modifying one layer might necessitate adjustments in other dependent layers. This domino effect can make the system less flexible.

- **Complexity with Many Layers**: While the architecture can include numerous layers, this can lead to increased complexity, making the system challenging to comprehend and maintain.

#### Layers

- **Presentation Layer:** This layer is responsible for displaying information to users and handling user interactions. In a web application, it might correspond to the View. In a Windows Forms app, it is the form itself.

- **Business Logic Layer:** This layer implements the business rules and processes. It acts as an intermediary between the presentation and data layers.

- **Data Layer:** This layer is responsible for data storage and access, such as a database, file system, or web service.

#### Wiring the Layers

**When to use:**

#### Common Use Cases

- **Web Applications**: They frequently adopt a 3-Tier architecture, dividing responsibilities across the client-side interface (presentation), server-side processing (business logic), and database management systems (data).

- **Enterprise Solutions**: Complex business operations can often benefit from an architecture that rigorously separates UI, logic, and data.

- **Systems with Numerous Users**: Scalability is essential for products and services that have many users. A layered architecture helps manage this by compartmentalizing components.

**Examples:**

#### Code Example: Basic Three-Layer Architecture

```csharp
public class UserController
{
    private readonly UserService _userService;

    public UserController(UserService userService)
    {
        _userService = userService;
    }

    public void DisplayUserInfo(string userId)
    {
        var userInfo = _userService.GetUserInfo(userId);
        // Pass userInfo to the view for display
    }
}
```

```csharp
public class UserService
{
    private readonly UserRepository _userRepository;

    public UserService(UserRepository userRepository)
    {
        _userRepository = userRepository;
    }

    public UserInfo GetUserInfo(string userId)
    {
        // Apply any business rules or logic here before retrieving the user data
        var userInfo = _userRepository.GetUserById(userId);
        return userInfo;
    }
}
```

```csharp
public class UserRepository
{
    public UserInfo GetUserById(string userId)
    {
        // Code to interact with the data storage medium to retrieve user information
    }
}
```

In a real-world application, you might perform dependency injection to wire up the layers. Here is the C# code:

```csharp
// In your Main method or application entry point
var userRepository = new UserRepository();  // A concrete implementation
var userService = new UserService(userRepository);
var userController = new UserController(userService);
```

In many modern systems, this wiring could be handled by an IoC container.

</details>

<details>
<summary>What are the elements of a good software architecture?</summary>

### What are the elements of a good software architecture?

**Definition:**

Robust software architecture makes systems sustainable and manageable. Here are the key elements:

**How it works:**

#### Core Components

- **Software Components**: Building pieces optimized for specific functions.

- **Connectors**: Mechanisms to link components, such as interfaces or integration patterns.

- **Constraints**: Design rules and standards that components and connectors must adhere to.

#### Principles of Design

- **Modularity**: Dividing components into logical, self-contained units for versatility, reusability, and reduced complexity.

- **Consistency**: Maintaining uniformity throughout for ease of comprehension and usage.

- **Simplicity**: Favoring straightforward, understandable solutions over intricate ones.

#### Technical Elements

- **Layers**: Segregating components based on their responsibilities into distinct layers.

- **Tiers**: Dividing components based on where in the system they execute, such as client, server, or data tiers.

- **Data Management**: Outlining strategies for data storage, access, and integrity.

#### Techniques and Design Patterns

- **Pattern Selection**: Applying battle-tested templates such as CRUD or MVC to common software challenges.

- **Design Metaphors**: Conceptual frameworks like pipes-and-filters or pub-sub to bring logical consistency for certain applications.

- **Refactoring**: Iterative, continuous improvement to maintain architectural integrity over the system's lifespan.

#### Integrations

- **Interfacing**: Clear contracts, such as APIs or event definitions, between modules for seamless interactions.

- **External Services**: Efficiently incorporating third-party services, like payment gateways or cloud storage, while ensuring system reliability.

#### Quality Factors

- **Performance**: Design that minimizes latency and resource consumption.

- **Reliability**: Ensuring the system can respond appropriately to disturbances.

- **Maintainability**: Making the system easy to understand and modify over time to incorporate changes or resolve issues.

#### Lifecycle Considerations

- **Development and Testing**: Designs that enable components to be tested in isolation or through stubs and mocks, if needed.

- **Evolution**: Facilities for gradual, controlled system enhancements to accommodate emerging or altered requirements.

</details>

<details>
<summary>Define “modularity” in software architecture.</summary>

### Define “modularity” in software architecture.

**Definition:**

Modularity in software architecture refers to the degree to which a software system can be broken down into separate functional or logical modules or components. These modules are often designed to be distinct, yet interrelated, promoting ease of development, flexibility, maintainability, and reusability.

**How it works:**

#### Core Attributes of Modularity

- **Encapsulation**: Modules expose only a well-defined, limited interface, keeping internal functionalities hidden. This reduces complexity and the possibilities of unintended interactions.

- **High Cohesion**: Modules contain closely-related functions, promoting focused responsibilities. This characteristic is vital for both the maintenance and reusability of code.

- **Loose Coupling**: Modules should be connected in a way that minimizes their interdependence. Reducing the dependencies between modules makes it easier to replace, update, and reconfigure individual components.

- **Abstraction**: Modules are self-contained units with defined interfaces, abstracted away from unnecessary internal details.

#### Benefits of Modularity

- **Enhanced Maintainability**: Simplified testing, debugging, and maintenance procedures.

- **Clear Design Boundaries**: Improved team workflows, as individual developers or groups can focus on specific modules without needing to understand the entire system.

- **Reusability**: Modules that aren't tightly coupled often lead to more reusable code.

- **Parallel Development**: Modularity lends itself well to parallel development, enabling team members to work on different modules simultaneously.

- **Flexibility**: Modules can often be replaced, updated, or augmented with new functionality easily.

**When to use:**

#### Real-World Application

- **Android Applications**: Based on a modular architecture, developers can build individual modules known as "feature modules" that represent a specific set of features or functionalities in the app.

- **Cloud Computing**: The microservices architectural style is modular, where each microservice is a self-contained unit that can be developed, deployed, and scaled independently.

- **Game Development**: Engines such as Unity and Unreal Engine use modular structures made of components and subsystems to manage game objects and systems.

- **Web Development**: Frameworks like Angular or React structure applications as modular components, each handling a particular piece of the user interface or corresponding functionality.

</details>

<details>
<summary>Discuss the concepts of coupling and cohesion.</summary>

### Discuss the concepts of coupling and cohesion.

**Definition:**

Coupling is the level of dependence that modules have on one another. Low coupling is a design goal, indicating that modules are fairly independent.

In contrast, cohesion reflects the degree to which the elements within a module belong together. High cohesion suggests that all elements in a module are closely related.

**How it works:**

- **Practical Example**: Data Validation Form

Consider a form where email and phone number are mandatory.

- **Tight Coupling**: Data validation for email and phone are directly within the form submit function.

- **Low Cohesion**: The form has loose validation responsibilities, such as checking if email is unique.

This approach can lead to redundant and error-prone code, especially as the form grows more complex. Instead, one could use a ValidateEmail and ValidatePhoneNumber module, enforcing responsibility and independence.

</details>

<details>
<summary>What is the principle of least knowledge (Law of Demeter) in architecture?</summary>

### What is the principle of least knowledge (Law of Demeter) in architecture?

**Definition:**

The principle of least knowledge (LoD), also known as the law of Demeter, emphasizes reducing the dependencies between modules and classes to make systems more modular, easier to maintain, and less prone to errors.

**How it works:**

#### Core Tenets

The Law of Demeter distills the following key principles:

- **Locality of Knowledge**: Each module has detailed knowledge about its immediate collaborators.

- **Black Box Design**: A module's internal state is considered its private information, and interactions are based on public interfaces.

#### Verbal Expressions

The Law of Demeter has been formulated using several verbal expressions, offering different perspectives on its core tenets:

- **"Don't talk to strangers":** This phrase is an analogue for the principle that objects or methods should have limited access to other entities.

- **"Only talk to your immediate friends":** This expression emphasizes that a module should interact with its closely related modules and avoid extensive collaboration, mitigating the risk of creating tightly coupled systems.

**Examples:**

#### Code Example: Demeter Principle Violation

Here is the code:

```java
public class Owner {
    private Car car;

    public Owner() {
        this.car = new Car();
    }

    public void startCar() {
        car.start();
    }
}

public class Car {
    private Engine engine;

    public Car() {
        this.engine = new Engine();
    }

    public void start() {
        engine.start();
    }
}

public class Engine {
    public void start() {
        System.out.println("Starting engine");
    }
}
```

In this system, the Owner class knows too much about the Engine class.

#### Code Example: Applying the Law of Demeter

Here is the code:

```java
public class Owner {
    private Car car;

    public Owner() {
        this.car = new Car();
    }

    public void startCar() {
        car.startEngine();
    }
}

public class Car {
    private Engine engine;

    public Car() {
        this.engine = new Engine();
    }

    public void startEngine() {
        engine.start();
    }
}

public class Engine {
    public void start() {
        System.out.println("Starting engine");
    }
}
```

In this improved structure, the Owner class directly communicates with the Car class, maintaining a more logical and appropriate communication pattern.

</details>

<details>
<summary>How are cross-cutting concerns addressed in software architecture?</summary>

### How are cross-cutting concerns addressed in software architecture?

**Definition:**

Cross-cutting concerns in software development represent aspects that span multiple modules and layers, such as logging, security, and caching. Tackling these concerns with a straightforward and coherent approach often simplifies both development and maintenance.

**How it works:**

#### Addressing Cross-Cutting Concerns

- **Separation of Concerns (SoC)**: A design principle that suggests partitioning a program into distinct sections, such as presentation or data storage. Implementing SoC often involves strategies like modularization and layering.

- **Design Patterns**: They are recurring, reliable solutions to typical design problems that help direct the structure and flow of programs. Patterns like Singleton, Factory, and Observer all take on cross-cutting concerns.

- **Aspect-Oriented Programming (AOP)**: This paradigm provides a unique programming technique that specifically targets cross-cutting concerns by isolating responsibilities and execution contexts. AOP tools enable the clear distinction of primary software logic from ancillary functions.

- **Frameworks and Libraries**: Many frameworks and libraries assist in cross-cutting concern management. Spring serves as a prime example by offering built-in modules that manage security, transactions, and more.

- **Code Generation**: Constructing code, rather than writing it manually, can help with cross-cutting concerns. For example, developers might use code-generation tools for tasks like boilerplate code generation or data validation implementation.

- **Development Tools**: Tools like debuggers, linters, and IDEs come into play by aiding in the identification and management of cross-cutting concerns.

- **Peer Reviews and Pair Programming**: Collaborative activities such as peer reviews and pair programming are effective for identifying and addressing cross-cutting concerns, particularly during initial development stages.

- **Documentation and Training**: Providing comprehensive documentation and training can help teams to understand, recognize, and manage cross-cutting concerns effectively.

#### Non-traditional Approaches

- **Language Integrated Query (LINQ)**: In .NET, LINQ allows for data querying in a language-agnostic manner, addressing database access across various modules.

- **Data Management Platforms**: Modern no-code or low-code platforms such as Airtable integrate data management directly into application design, reducing the need to handle data consistency and integrity across multiple components.

</details>

### Design Patterns and Principles

<details>
<summary>Describe the Model-View-Controller (MVC) architectural pattern.</summary>

### Describe the Model-View-Controller (MVC) architectural pattern.

**Definition:**

Model-View-Controller (MVC) is an architectural pattern known for its clear separation of concerns. It divides software into three main components, promoting maintainability and scalability.

**How it works:**

#### Visual Representation

#### MVC Pattern

#### Key Components

- **Model**: Represents the data and logic. It sends updates to the View and Controller.

- **View**: User interface components that present data from the Model to the user.

- **Controller**: Acts as an interface between Model and View. It processes user input and orchestrates the actions to perform based on that input.

#### Core Concepts

- **Event-driven Communication**: Components communicate through events or other mechanisms rather than directly calling one another.

- **Unidirectional Data Flow**: Data primarily moves from the Model to the View.

- **Loose Coupling**: Components are designed to be self-contained and interact with each other through well-defined interfaces, promoting reusability.

#### Interaction Flow

- **User Action**: Typically begins with a user input through View components like buttons or forms.

- **Controller Action**: The Controller receives the user input, interprets it for the Model, and triggers appropriate Model updates.

- **Data Update**: The Model is responsible for implementing the requested changes and notifying the View.

- **View Update**: Upon receiving notifications from the Model, the View updates its components to reflect the modified Model state.

- **User Feedback**: The updated View might request user input or display appropriate feedback, initiating another cycle if needed.

#### Benefits and Limitations

#### Advantages

- **Conceptual Simplicity**: Offers a clear structure and predictable data flow.

- **Parallel Development**: Different teams or developers can work on each component without interfering with others.

- **Reusability**: Each component is designed to be independent and reusable, which reduces code duplication.

- **Testability**: Easier to test component logic in isolation.

- **Decoupled Maintenance**: Changes to one component (like the UI) can be implemented without affecting others.

#### Limitations

- **Potential Complexity**: Managing the interactions between Model, View, and Controller can become intricate, especially in large applications.

- **Synchronization**: It may not always be straightforward to ensure that the View is in sync with the underlying Model.

- **Two-Way Communication**: While primarily unidirectional, MVC can support bi-directional data flow, which can lead to additional complexity.

</details>

<details>
<summary>Explain the Publish-Subscribe pattern and its applications.</summary>

### Explain the Publish-Subscribe pattern and its applications.

**Definition:**

The Publish-Subscribe pattern is a messaging pattern where senders, also known as publishers, do not program the message to be sent directly to a recipient. Instead, they define the character of the message using classes and descriptions, and subscribers subscribed interested subjects receive those messages.

**How it works:**

#### Core Components

- **Publisher**: The sender of the messages. It doesn't direct messages to specific recipients but instead classifies published messages. Each message has a category or topic that is used to route it to the interested subscribers.

- **Subscriber**: Recipient of the messages. It expresses an interest in one or more topics and only receives messages that are classified with these topics.

- **Message Broker/Mediator**: An intermediary that takes on the task of forwarding messages from publishers to subscribers. This component dispatches messages based on the topics to which subscribers have subscribed.

#### Mechanism

- **Registration**: Subscribers express their interest or topic preferences to the broker. In some systems, publishers may not be aware of any subscribers.

- **Message Delivery**: When a publisher sends a message, they publish it to a specific topic. The message broker, then, finds the relevant subscribers who have subscribed to that topic and forwards the message to them.

**When to use:**

#### Application Scenarios

- **Network and Messaging Systems**: Pub-Sub is essential for computers and devices to exchange information in distributed systems, like IoT networks, financial systems, and multiplayer games.

- **User Interface, Model-View-Controller (MVC), and User Interface**: Front-end frameworks like React and Angular use a variant known as Flux architecture. Subscriber Components or Views subscribe to managed data stores or actions, receiving updates when the underlying data changes.

- **Business Logic and Event-Driven Systems**: In complex or multilayered systems with myriad dependencies, Pub-Sub provides a clean separation between components or layers.

- **Serverless Architectures and Microservices**: By supporting asynchronous, decoupled communication, Pub-Sub allows for sound architectural designs, improved concurrency, and scalability, and cost-effective resource consumption.

- **Data Analytics and Monitoring Tools**: Tools that collect and analyze data from various sources and then act upon rules or conditions often use the Pub-Sub pattern for efficient data distribution and analysis.

- **Database Synchronization and Data Distribution**: Distributed data systems like Apache Kafka use the Pub-Sub pattern to synchronize data across nodes, ensuring consistency and fault-tolerance.

</details>

<details>
<summary>Define Microservices architecture and contrast it with Monolithic architecture.</summary>

### Define Microservices architecture and contrast it with Monolithic architecture.

**Definition:**

Microservices and Monolithic architectures represent distinct approaches to software design, each with its unique advantages and challenges.

**How it works:**

#### Monolithic Architecture

- **Monolithic architecture:** All components of a system are interconnected and work together as a single unit. For instance, web servers, application servers, and databases are often integrated into one platform.

#### Key Characteristics

- **Homogeneity**: The entire codebase is written in one language and deployed as a single unit.

- **Tight Coupling**: Each module and component are interdependent, making it challenging to scale and update selectively.

- **Centralized Management**: A centralized system for the entire application.

#### Benefits

- **Simplicity**: It's easier to design and develop a single application than a distributed system.

- **Consistent Transactions**: Traditional ACID transactions can be used across the application without integration concerns.

- **Easier Debugging**: Debugging can be simpler due to everything running in one process.

#### Limitations

- **Scalability Challenges**: All components must scale together, leading to inefficient resource utilization.

- **Limited Technology Flexibility**: All components are built using the same stack.

- **Bottlenecks**: A failure in one module or resource can potentially affect the entire system.

#### Microservices Architecture

- **Microservices architecture:** The system is broken down into small, independent services, each with a specific business function and its dedicated data store. These services communicate through well-defined APIs.

#### Key Characteristics

- **Decentralization**: Each microservice operates independently, managing its data and logic.

- **Flexibility and Autonomy**: Different services can be built using different technologies and deployed and scaled individually.

- **Polyglot Persistence**: Each service can use the most suitable data storage mechanism for its needs.

#### Benefits

- **Scalability**: Components can be scaled based on their individual requirements, optimizing resources.

- **Technological Flexibility**: Developers can use the best tools for each microservice function.

- **Enhanced Resilience**: Isolation means that a failure in one service is less likely to affect others.

#### Challenges

- **Distributed Systems Complexity**: Communication between the services introduces new complexities.

- **Operational Overhead**: Managing multiple services, each with its stack, adds operational complexity.

</details>

<details>
<summary>What are the SOLID principles of object-oriented design?</summary>

### What are the SOLID principles of object-oriented design?

**Definition:**

SOLID is an acronym that represents the five basic principles of object-oriented programming. These guidelines help to enhance code readability, reusability, and maintainability.

**How it works:**

#### The SOLID Principles

- **Single Responsibility Principle (SRP):** A class should have only one reason to change. In other words, it should have only one responsibility.

- **Open/Closed Principle (OCP):** A module (i.e., a function or a class) should be open for extension, but closed for modification.

- **Liskov Substitution Principle (LSP):** Derived classes should be substitutable for their base classes, meaning that they should share the same interface and be used interchangeably with objects of the base class.

- **Interface Segregation Principle (ISP):** Many client-specific interfaces are better than one general-purpose interface.

- **Dependency Inversion Principle (DIP):** High-level modules should not depend on low-level modules. Both should depend on abstractions. Additionally, abstractions should not depend on details; details should depend on abstractions.

#### What the SOLID Principles Mean

- **SRP**: A class should be responsible for doing one thing and doing it well.

- **OCP**: Systems should be designed so that they are open for extension but closed for modification. This generally means that when new functionality is required or specifications change, the existing code should not need to be modified. Instead, the code should be easy to extend so that new functionality can be added.

- **LSP**: This principle deals with whether a derived class is a true subtype of the base class. Essentially, it means that derived classes should not change the behavior of the base class.

- **ISP**: This principle deals with the idea that classes or modules should not have to depend on interfaces that they don't use. It's better to have multiple small, specific interfaces than one large general one.

- **DIP**: A high-level class should not care about the details of its dependencies. This means that interfaces or abstractions should be used instead of concrete implementations.

</details>

<details>
<summary>When should the Singleton pattern be applied and what are its drawbacks?</summary>

### When should the Singleton pattern be applied and what are its drawbacks?

**Definition:**

While the Singleton pattern offers benefits such as a single point of access and delayed instantiation, its drawbacks and potential misapplications are worth considering.

**How it works:**

#### Key Considerations

#### Scope of Singleton Behavior

- **Singleton scope:** The Singleton pattern isn't always the most suitable for enabling unique global access. Classes managed by dependency injection (DI) frameworks, for instance, often provide more adaptable and testable mechanisms for context-driven singletons.

#### Drawbacks of the Singleton Pattern

- **Hidden Dependencies**: The use of singletons can introduce implicit and potentially unexpected dependencies. This can make the codebase more challenging to understand and debug.

- **Violation of the Single Responsibility Principle**: Singletons often manage their own lifespan and state, going beyond the scope of their primary responsibilities.

- **Memory Management Baggage**: Systems using singletons need to manage memory manually, which can be tedious and error-prone.

- **Thread Safety Complexity**: Ensuring thread safety in a multithreaded environment can be complex and, if done incorrectly, lead to performance bottlenecks or data inconsistencies.

- **Testability Concerns**: Code featuring singletons can be hard to test in isolation, as they introduce global state.

- **Encapsulation Limitations**: Although singletons encapsulate their state within their class, the structure can lead to tight coupling throughout the codebase.

- **Potential for Abuse and Overuse**: Over-reliance on singleton patterns can lead to a monolithic architecture, making the system less flexible and harder to maintain.

#### Best Practices for Singleton Usage

Considering the potential drawbacks, it's good to adhere to these best practices:

- **Use Singleton with Caution**: Evaluate if other design patterns, such as factory patterns, or frameworks like DI might be a better fit.

- **Deliver Concise Responsibilities**: Let a singleton manage one responsibility or functionality.

- **Apply Lazy Initialization Judiciously**: While delaying creation can save resources, verify that it doesn't introduce state inconsistency.

- **Ensure Thread Safety When Appropriate**: Utilize methods like double-checked locking or initialize-on-demand patterns in multithreaded environments to maintain data integrity.

- **Focus on Maintaining Global State**: If your primary goal is to preserve global state, view the singleton pattern as one of the tools at your disposal.

- **Use DI for Wider Flexibility**: Combine the benefits of DI with the clarity and convenience of singleton where it makes sense.

</details>

<details>
<summary>Define the Repository pattern and its use cases.</summary>

### Define the Repository pattern and its use cases.

**Definition:**

Martin Fowler's catalog, written up by Edward Hieatt and Rob Mee, defines a Repository as a mediator between the domain and the data-mapping layers. It offers a collection-like interface for accessing domain objects.

**How it works:**

Client code builds a query specification and submits it to the Repository. Objects are added and removed as they would be on an in-memory collection. The mapping code inside the Repository runs the real store operations. The dependency points one way: the domain does not depend on the mapping layer. Fowler notes this pays off when there are many domain classes or a lot of querying, because query logic would otherwise be duplicated. It sits on top of a layer such as Data Mapper.

**When to use:**

Use it for a complex domain model that should look like a collection of objects, not like SQL. The catalog points to the Domain-Driven Design write-up of the same pattern.

**Examples:**

```python
class OrderRepository:
    def __init__(self, mapper):
        self._mapper = mapper

    def add(self, order):
        self._mapper.insert(order)

    def remove(self, order):
        self._mapper.delete(order)

    def matching(self, specification):
        return self._mapper.query(specification)
```

**Sources:**

- [Martin Fowler — Repository](https://martinfowler.com/eaaCatalog/repository.html)

</details>

<details>
<summary>Describe the Service-Oriented Architecture (SOA) pattern and its components.</summary>

### Describe the Service-Oriented Architecture (SOA) pattern and its components.

**Definition:**

IBM defines SOA as a way to make software components reusable and interoperable through service interfaces. Each service holds the code and data for one discrete business function. The interface is a contract: callers need little or no knowledge of the implementation.

**How it works:**

Interfaces are often described with WSDL, an XML vocabulary. Calls travel over SOAP/HTTP or RESTful HTTP with JSON. A registry publishes services so other teams can find them. Governance controls the lifecycle. An enterprise service bus can transform models, route, and translate protocols; IBM says SOA can exist without an ESB, but then every caller integrates point to point. Services are often a new interface over a legacy system of record.

**When to use:**

IBM positions SOA as an enterprise integration style: expose a business function once and reuse it across applications. That is a wider scope than microservices, which IBM describes as a way to split one application into independently changeable pieces.

**Examples:**

```text
caller --SOAP or HTTP/JSON--> service contract (WSDL) --> implementation
                                      |
                                   registry
```

**Sources:**

- [IBM — What is service-oriented architecture (SOA)?](https://www.ibm.com/topics/soa)

</details>

<details>
<summary>Explain the Decorator pattern with an example.</summary>

### Explain the Decorator pattern with an example.

**Definition:**

Refactoring.guru defines Decorator, also called Wrapper, as a structural pattern that attaches behavior by placing an object inside wrapper objects that carry the extra behavior. The wrapper implements the same interface as the wrapped object and may run code before or after delegating.

**How it works:**

The component interface is shared by the concrete component and every decorator. The base decorator holds a reference typed as that interface and forwards every call. A concrete decorator overrides a method, does its extra work, and calls the wrapped object. Clients stack wrappers at runtime. Inheritance cannot do this: it is static, and most languages allow only one parent class.

**When to use:**

Use it when extra behavior must be added or removed at runtime, or when the class cannot be subclassed. Do not use it when you need a different interface; that is the Adapter, which Refactoring.guru separates from Decorator.

**Examples:**

```python
class DataSource:
    def write(self, data):
        raise NotImplementedError

class FileSource(DataSource):
    def write(self, data):
        return f"file:{data}"

class Encryption(DataSource):
    def __init__(self, wrappee):
        self._wrappee = wrappee

    def write(self, data):
        return self._wrappee.write(f"enc({data})")
```

**Sources:**

- [Refactoring.guru — Decorator](https://refactoring.guru/design-patterns/decorator)

</details>

<details>
<summary>What is the Command pattern and its implementation?</summary>

### What is the Command pattern and its implementation?

**Definition:**

Refactoring.guru defines Command, also called Action or Transaction, as a behavioral pattern that turns a request into an object holding everything needed to perform it. That object can be passed, queued, logged, or undone.

**How it works:**

The command interface usually has one `execute` method. A concrete command stores the receiver and the arguments and calls the receiver; it does not have to do the work itself. The sender (invoker) holds a command and triggers it. The client creates the receiver, the command, and the sender. For undo, the command saves a backup before `execute` and the history is a stack of commands.

**When to use:**

Use it to parameterize an object with an operation, to delay or queue work, or to support undo. The cost Refactoring.guru names is an extra layer between senders and receivers.

**Examples:**

```python
class Command:
    def execute(self):
        raise NotImplementedError

class Paste(Command):
    def __init__(self, editor, clipboard):
        self._editor = editor
        self._clipboard = clipboard

    def execute(self):
        self._editor.insert(self._clipboard)
```

**Sources:**

- [Refactoring.guru — Command](https://refactoring.guru/design-patterns/command)

</details>

<details>
<summary>When would you use the Adapter pattern?</summary>

### When would you use the Adapter pattern?

**Definition:**

Refactoring.guru defines Adapter, also called Wrapper, as a structural pattern that lets objects with incompatible interfaces collaborate. The adapter implements the interface the client already uses and translates each call into the service object's interface.

**How it works:**

An object adapter holds the service and converts arguments and results. A class adapter, possible in languages with multiple inheritance, inherits both sides and adapts inside overridden methods. The client talks only to the client interface, so a new adapter does not change client code.

**When to use:**

Use it for a legacy or third-party class whose interface does not match the rest of the code, and when you cannot change that class. Refactoring.guru contrasts it with Decorator: Adapter presents a different interface and does not stack recursively. It contrasts with Facade: Adapter usually wraps one object so an existing interface becomes usable, while Facade defines a new interface over a subsystem.

**Examples:**

```python
class RoundPeg:
    def __init__(self, radius):
        self.radius = radius

class SquarePegAdapter(RoundPeg):
    def __init__(self, width):
        self._width = width
        super().__init__(width * (2 ** 0.5) / 2)
```

**Sources:**

- [Refactoring.guru — Adapter](https://refactoring.guru/design-patterns/adapter)

</details>

### Scalability and Performance Considerations

<details>
<summary>What strategies can be used to scale a software application?</summary>

### What strategies can be used to scale a software application?

**Definition:**

The Scale Cube, from _The Art of Scalability_ as summarized by Chris Richardson, describes three ways to scale an application: X-axis cloning, Y-axis functional decomposition, and Z-axis data partitioning.

**How it works:**

- **X-axis scaling:** Runs multiple identical copies behind a load balancer. If there are N copies, each handles 1/N of the load. Each copy can still access all of the data, so caches need more memory, and this axis does not reduce development or application complexity.

- **Y-axis scaling:** Splits the application into different services. Each service owns one or more closely related functions. Decomposition can be verb-based (a service for one use case, such as checkout) or noun-based (a service for one entity, such as customer management). An application can combine both. Richardson describes the microservice architecture as an application of Y-axis scaling.

- **Z-axis scaling:** Also runs identical code, but each server owns only a subset of the data. A router sends each request to the right server, often by a primary key or by customer type. Databases are commonly sharded this way. A query may go to every partition and then be aggregated. Each server uses less memory and I/O, transactions spread across servers, and a failure hides only part of the data. Partitioning and repartitioning add complexity, and Z-axis scaling still does not by itself reduce development complexity.

**When to use:**

Use X-axis when identical copies behind a balancer are enough. Use Z-axis when one copy should not hold the whole data set. Use Y-axis when the goal is also to split development and application complexity into services.

**Sources:**

- https://microservices.io/articles/scalecube.html
</details>

<details>
<summary>Describe load balancing and its significance in software architecture.</summary>

### Describe load balancing and its significance in software architecture.

**Definition:**

Load balancing spreads requests across multiple application instances. NGINX describes it as a way to optimize resource use, maximize throughput, reduce latency, and improve fault tolerance.

**How it works:**

NGINX and NGINX Plus provide Layer 7 load balancing for HTTP and HTTPS. A group of servers is declared with the `upstream` directive, and `proxy_pass` sends requests to that group. Open Source methods are Round Robin (the default; weights are honored), Least Connections, Least Time, IP Hash, and Generic Hash. NGINX Plus also supports Random. A `backup` server is used when the others are unavailable. NGINX Plus can reintroduce a recovered server with slow-start and can pin a client to the same upstream with session persistence.

Layer 4 balancing, for TCP and UDP, is a separate NGINX capability.

**When to use:**

Use a load balancer when more than one instance should share traffic, including the X-axis clones on the Scale Cube. IP hash or session persistence is for clients that must stay on one upstream; twelve-factor apps treat sticky sessions as a violation and keep session state in a backing store instead.

**Examples:**

```nginx
http {
    upstream backend {
        server backend1.example.com;
        server backend2.example.com;
        server 192.0.0.1 backup;
    }

    server {
        location / {
            proxy_pass http://backend;
        }
    }
}
```

**Sources:**

- https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/
- https://12factor.net/processes
</details>

<details>
<summary>Explain the concept of a stateless architecture.</summary>

### Explain the concept of a stateless architecture.

**Definition:**

In the twelve-factor model, the app runs as one or more stateless, share-nothing processes. Anything that must persist lives in a stateful backing service, typically a database.

**How it works:**

A process may use its memory or filesystem as a brief, single-transaction cache, for example while downloading a file, transforming it, and writing the result to the database. The app must not assume that memory or disk cache will still be there for a later request or job. With many processes, the next request is likely to hit a different process. Even a single process loses local state on restart from a deploy, a config change, or a move to another machine.

Sticky sessions, which cache the user session in the process and require later requests from that visitor to return to the same process, violate twelve-factor. Session data belongs in a store with time expiration, such as Memcached or Redis. Compiled assets should be produced in the build stage, not cached on the process filesystem at runtime.

**When to use:**

Use stateless processes when instances are cloned, restarted, or moved. Keep durable and session state in a backing service so any process can serve the next request.

**Sources:**

- https://12factor.net/processes
</details>

<details>
<summary>How does caching improve system performance?</summary>

### How does caching improve system performance?

**Definition:**

A cache keeps a copy of data closer to the reader so a later request can be served without fetching the definitive copy again. Amazon CloudFront does this at edge locations. A twelve-factor process may also keep a short-lived cache in memory or on disk for a single transaction.

**How it works:**

CloudFront routes a request to the edge location with the lowest latency. If the object is already there, CloudFront delivers it immediately. If it is not, CloudFront retrieves it from an origin you define, such as an S3 bucket or an HTTP server, and can then serve the cached copy. Copies in many edge locations lower latency to the first byte, raise transfer rates, and add reliability because the object is held in more than one place. By default each file stays in an edge location for 24 hours. The origin can set headers that change that lifetime. The minimum expiration is 0 seconds.

On the Scale Cube, Z-axis partitioning improves cache utilization because each server deals with a subset of the data, which reduces memory use and I/O. X-axis clones that each see all of the data need more cache memory to be effective.

A process-local cache is only a brief, single-transaction aid. Twelve-factor says a later request must not depend on it. Session state that must survive the next request goes to Memcached or Redis.

**When to use:**

Cache content at the edge when users are geographically spread and the same objects are requested again. Use a shared store with expiration for session data. Do not treat process memory as a durable cache.

**Sources:**

- https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html
- https://12factor.net/processes
- https://microservices.io/articles/scalecube.html
</details>

<details>
<summary>What practices are vital for designing high availability systems?</summary>

### What practices are vital for designing high availability systems?

**Definition:**

PostgreSQL describes high availability as database servers working together so a second server can take over quickly if the primary fails. The same chapter separates that from load balancing, where several computers serve the same data.

**How it works:**

- **Read-only servers:** Relatively easy to combine.

- **Read/write servers:** Harder — a write on any server must reach the others or later reads disagree. Some designs allow only one primary to modify data. Standbys track the primary.

- **Warm standby:** Cannot be connected to until it is promoted.

- **Hot standby:** Accepts connections and serves read-only queries.

- **Synchronous replication:** Does not treat a modifying transaction as committed until all servers have committed it. Failover then loses no data, and load-balanced servers return consistent results.

- **Asynchronous replication:** Allows delay, so a switch to a backup can lose transactions and load-balanced reads can be slightly stale. Asynchronous mode is used when synchronous mode would be too slow. A fully synchronous solution over a slow network might cut performance by more than half.

NGINX improves fault tolerance by spreading HTTP traffic across a group and by keeping a backup server. Kubernetes removes a failed Pod from a Service's endpoints and replaces a failed Pod in a Deployment so the replica count holds.

**When to use:**

Choose synchronous replication when a failover must not lose committed data. Choose asynchronous replication when commit latency matters more than a possible loss of the latest transactions. Use a hot standby when the secondary should serve reads before promotion.

**Sources:**

- https://www.postgresql.org/docs/current/high-availability.html
- https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/
- https://kubernetes.io/docs/concepts/architecture/self-healing
</details>

<details>
<summary>Detail the trade-offs in the CAP theorem.</summary>

### Detail the trade-offs in the CAP theorem.

**Definition:**

IBM states Brewer's theorem: a distributed system can deliver only two of consistency, availability, and partition tolerance. Eric Brewer advanced it in 2000; Gilbert and Lynch published a proof in 2002.

**How it works:**

- **Consistency:** Every client sees the same data at the same time, so a write is successful only after it has been replicated.

- **Availability:** Every working node returns a valid response.

- **Partition tolerance:** A partition is a lost or delayed link between nodes; the cluster keeps working anyway.

- **CP system:** During a partition it refuses the inconsistent node. MongoDB's replica set is described as CP: one primary takes writes, and clients cannot write while a new primary is elected.

- **AP system:** During a partition it stays up and may serve older data. Cassandra is described as AP: any node accepts writes, and inconsistencies are repaired afterward.

- **CA system:** Cannot keep both consistency and availability once a partition exists. IBM says partitions cannot be avoided in a distributed system, so a CA distributed database is not a practical choice.

**When to use:**

Choose CP, such as the PostgreSQL case IBM cites for payments, when a stale read is worse than a refused write. Choose AP, such as Cassandra, when the system must answer during a partition and eventual consistency is acceptable.

**Examples:**

```text
partition happens
  CP: reject writes on the side that cannot confirm the latest value
  AP: answer anyway; reconcile after the link returns
```

**Sources:**

- [IBM — What is the CAP theorem?](https://www.ibm.com/topics/cap-theorem)

</details>

<details>
<summary>How would you prevent single points of failure in software architecture?</summary>

### How would you prevent single points of failure in software architecture?

**Definition:**

A single point of failure is one component whose loss stops the system. The sourced practices below remove that dependency by keeping a second database server, a backup upstream, and replacement Pods.

**How it works:**

PostgreSQL lets a second server take over if the primary fails. A warm standby is promoted before it accepts connections. A hot standby can already serve read-only queries. Synchronous replication commits only after every server has committed, so failover does not lose data. Asynchronous replication can lose transactions that have not propagated yet.

NGINX can list a `backup` server in an `upstream` group so traffic has somewhere to go when the other servers are unavailable. Spreading requests across the group is how NGINX improves fault tolerance.

Kubernetes restarts a failed container according to `restartPolicy`. If a Pod in a Deployment fails, it creates a replacement to keep the replica count. A failed Pod is removed from a Service's endpoints so traffic goes only to healthy Pods. A PersistentVolume on a failed node can be reattached to a new Pod on another node.

On the Scale Cube, Z-axis partitioning improves fault isolation: a failure makes only part of the data inaccessible.

**When to use:**

Add a standby when the database must survive loss of the primary, and choose sync or async from the data-loss and latency trade-off. Add a backup upstream and multiple Pods when one instance must not be the only place that can serve traffic.

**Sources:**

- https://www.postgresql.org/docs/current/high-availability.html
- https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/
- https://kubernetes.io/docs/concepts/architecture/self-healing
- https://microservices.io/articles/scalecube.html
</details>

<details>
<summary>Describe the role of a Content Delivery Network (CDN) in an architecture.</summary>

### Describe the role of a Content Delivery Network (CDN) in an architecture.

**Definition:**

Amazon CloudFront is a web service that speeds up distribution of static and dynamic web content, such as HTML, CSS, JavaScript, and image files, through a worldwide network of data centers called edge locations.

**How it works:**

A request is routed to the edge location that provides the lowest latency. If the content is already cached there, CloudFront delivers it immediately. If not, CloudFront retrieves it from an origin: an S3 bucket, a MediaPackage channel, or an HTTP server that holds the definitive version. Traffic goes over the AWS backbone to that edge, which cuts the number of networks a request crosses. Viewers get lower latency to the first byte and higher transfer rates. Copies in multiple edge locations also raise reliability and availability.

You create a distribution that names the origins and how delivery is tracked. CloudFront assigns a domain name and sends the distribution configuration, not the content, to its edge locations. Objects are uploaded to the origin. By default a file stays cached for 24 hours; origin headers can change that, down to 0 seconds. Private objects can be served with signed URLs and signed cookies. A standard distribution is for one site's own settings. A multi-tenant distribution is for a SaaS provider serving many similar customer sites from shared configuration.

**When to use:**

Put a CDN in front of content that users fetch from many places, when the origin should not answer every request directly. Use a multi-tenant distribution when many similar sites share one configuration.

**Sources:**

- https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html
</details>

<details>
<summary>Discuss techniques for optimizing database performance architecturally.</summary>

### Discuss techniques for optimizing database performance architecturally.

**Definition:**

PostgreSQL treats synchronization as the main cost when database servers work together. Architectural choices are which server may write, whether replication is synchronous, and whether data is partitioned so each server holds less of it.

**How it works:**

Read-only servers can be combined by load-balancing requests. Read/write servers are harder because a write must reach every server that will later be read. Allowing only one primary to modify data avoids write conflicts. A hot standby can serve read-only queries and take read load off the primary.

Synchronous replication waits until all servers commit. PostgreSQL says a fully synchronous solution over a slow network might cut performance by more than half, while an asynchronous one might have a minimal performance impact. Asynchronous mode can return slightly stale reads.

On the Scale Cube, Z-axis sharding gives each server a subset of the rows, often by primary key. That improves cache utilization and reduces memory use and I/O, and it spreads transactions. X-axis clones that each see all data need larger caches. Repartitioning is the costly part of a Z-axis split.

**When to use:**

Offload reads to a hot standby when the workload is read-heavy. Prefer asynchronous replication when commit latency on a slow network dominates. Shard when one server's cache and I/O are the limit and you can define a partition key.

**Sources:**

- https://www.postgresql.org/docs/current/high-availability.html
- https://microservices.io/articles/scalecube.html
</details>

<details>
<summary>Explain database replication and failover in your architecture.</summary>

### Explain database replication and failover in your architecture.

**Definition:**

PostgreSQL replication keeps standby servers in step with a primary. Failover is the promotion of a standby so it can take over when the primary fails.

**How it works:**

Servers that modify data are the read/write, master, or primary servers. Standbys track changes on the primary. A warm standby cannot be connected to until it is promoted. A hot standby accepts connections and serves read-only queries while it is still a standby.

Synchronous replication does not consider a data-modifying transaction committed until all servers have committed it. A failover then loses no data, and every load-balanced server returns consistent results. Asynchronous replication allows a delay between the commit and its propagation. Some transactions might be lost in the switch to a backup, and load-balanced servers might return slightly stale results. Asynchronous communication is used when synchronous would be too slow.

The documentation also distinguishes granularity: some solutions cover a whole database server, others a table or a database. Streaming replication, replication slots, cascading replication, and synchronous replication are the mechanisms in the log-shipping standby chapter. Hot standby can hit query conflicts with replay; those are handled as a separate concern from the decision to fail over.

**When to use:**

Use synchronous replication when the standby must not miss a committed write. Use asynchronous replication when the latency of waiting for every standby is too high and a small loss window is acceptable. Promote a warm standby only at failover; use a hot standby when you also want read scaling before promotion.

**Sources:**

- https://www.postgresql.org/docs/current/high-availability.html
</details>

### Cloud Computing and DevOps

<details>
<summary>How does cloud computing influence software architecture design?</summary>

### How does cloud computing influence software architecture design?

**Definition:**

Cloud design moves capacity, patching, and placement off the application and into managed services, while the application is described as versioned infrastructure and as stateless units that can be copied.

**How it works:**

AWS Lambda runs code without provisioning or managing servers. Lambda manages maintenance, capacity provisioning, scaling, and patching. A Lambda function runs a handler for an event or API call. Each invocation is independent, with no shared state, and scales horizontally with demand. You pay for the request plus GB-seconds. Execution environments may be reused as warm starts, but state might not persist across invocations. A function invocation lasts up to 15 minutes.

Terraform defines cloud and on-premises resources in configuration files that you version, reuse, and share. A plan describes what will be created, updated, or destroyed. Apply performs that plan in dependency order. State is the source of truth for what exists.

The Scale Cube still applies: clone instances on the X axis, split services on the Y axis, and partition data on the Z axis. Twelve-factor processes stay stateless because the platform may restart or move them. Docker packages the same container for a laptop, a data center, or a cloud provider.

**When to use:**

Use a managed compute service such as Lambda when the workload is a short, stateless handler and you do not want to run servers. Use Terraform when the surrounding network, compute, and DNS must be reviewed as a plan before they change. Keep process state in a backing service because the cloud may relocate the process.

**Sources:**

- https://docs.aws.amazon.com/lambda/latest/dg/welcome.html
- https://developer.hashicorp.com/terraform/intro
- https://12factor.net/processes
- https://microservices.io/articles/scalecube.html
</details>

<details>
<summary>Define Infrastructure as Code (IaC) and its relationship to architecture.</summary>

### Define Infrastructure as Code (IaC) and its relationship to architecture.

**Definition:**

HashiCorp defines Terraform as an infrastructure as code tool that lets you build, change, and version cloud and on-premises resources safely and efficiently. Architecture is written down as human-readable configuration instead of being applied by hand.

**How it works:**

You define resources that may span providers: virtual machines in a VPC, security groups, a load balancer, DNS, or SaaS features. Providers call the platform APIs. The workflow is write, plan, and apply. The plan describes what Terraform will create, update, or destroy given the configuration and the real infrastructure. On approval, apply runs the operations in dependency order and can change independent resources in parallel. A state file is the source of truth Terraform compares against the configuration. Modules are reusable, configurable collections of infrastructure. Configuration can be committed to version control. HCP Terraform runs that workflow in a shared environment with access control and a private registry.

Twelve-factor config, such as database credentials, stays in the environment of each deploy. Terraform's configuration describes the resources. It is not a substitute for secrets checked into the repo.

**When to use:**

Use IaC when compute, network, and DNS are part of the architecture and must be reviewed, repeated, and versioned. Write modules when the same collection of resources is deployed more than once.

**Sources:**

- https://developer.hashicorp.com/terraform/intro
- https://12factor.net/config
</details>

<details>
<summary>Describe microservices in cloud-native design.</summary>

### Describe microservices in cloud-native design.

**Definition:**

Chris Richardson describes the microservice architecture as Y-axis scaling on the Scale Cube: the application is split into multiple different services, each responsible for one or more closely related functions.

**How it works:**

Services can be split by verb, such as a checkout use case, or by noun, such as customer management, or by a combination. Unlike X-axis clones, the services are not identical copies. They collaborate. An API gateway can be the single entry point, proxying some calls and fanning out others, and it can hide how the application is partitioned and where instances live.

Docker packages each service in a container that runs the same way on a laptop, in a data center, or on a cloud provider. Kubernetes runs a Deployment of Pods and a Service that load-balances to the Pods that match its selector. Twelve-factor processes stay stateless; durable data goes to a backing service. IBM notes that microservices are loosely coupled, independently deployable components that include their own stack, including their own database, and talk over a network, including across cloud and on-premises locations.

**When to use:**

Split into microservices when X-axis cloning is not enough to contain development and application complexity. Keep a database with the service when the data model should change independently. Put an API gateway in front when clients should not track every service location.

**Sources:**

- https://microservices.io/articles/scalecube.html
- https://microservices.io/patterns/apigateway.html
- https://docs.docker.com/get-started/docker-overview/
- https://www.ibm.com/think/topics/cap-theorem
</details>

<details>
<summary>Explain containerization and its architectural benefits.</summary>

### Explain containerization and its architectural benefits.

**Definition:**

Docker packages and runs an application in a loosely isolated environment called a container. The container holds what the application needs to run, so it does not depend on what is installed on the host.

**How it works:**

An image is a read-only template. Each instruction in a Dockerfile adds a layer. A rebuild rewrites only the layers that changed. A container is a runnable instance of an image. You can create, start, stop, move, or delete it through the API or CLI. By default it is isolated from other containers and from the host. Docker uses Linux namespaces so each aspect of the container runs in its own namespace. When the container is removed, state that was not in persistent storage disappears.

`docker run` pulls the image if it is missing, creates the container, adds a read-write filesystem layer, creates a network interface and an IP address, and starts the process. Registries such as Docker Hub store images. The same container runs on a laptop, on machines in a data center, or on a cloud provider. Docker describes this as a lighter alternative to hypervisor virtual machines, so more workloads fit on the same hardware, and as the unit you test and then push to production.

**When to use:**

Use containers when a service must run the same way in development, test, and production, and when you need to start or stop copies as load changes. Attach a volume when the container must keep state after it is removed.

**Examples:**

```console
$ docker run -i -t ubuntu /bin/bash
```

**Sources:**

- https://docs.docker.com/get-started/docker-overview/
</details>

<details>
<summary>Discuss the role of CI/CD in architectural design.</summary>

### Discuss the role of CI/CD in architectural design.

**Definition:**

Continuous delivery, in Pete Hodgson's feature-toggle article, separates feature release from code deployment. The architecture has to let a new build reach production while unfinished behavior stays off, and let a new version meet real traffic before everyone uses it.

**How it works:**

Docker's delivery loop is: develop in containers, push the image to a test environment, run automated and manual tests, and push the updated image to production when testing is done. The container is the unit that moves through that path.

- **Release toggles:** Let incomplete code sit on the trunk and still be deployed. The toggle point chooses the old or new path at runtime. Hodgson treats release toggles as short-lived, often a week or two, with a mostly static on/off decision.

- **Canary release:** Routes a small subset of users to the new version before the rest of the infrastructure. Sato's rollout grows that share, or starts with internal users, a region, or a brand. Rollback is routing users back to the old version. The database has to support both versions during the rollout, which Sato handles with a parallel change. Facebook's first canary is internal, with feature flags on, so problems show up early.

**When to use:**

Use a release toggle when unfinished work must stay on the trunk that you deploy. Use a canary when you want production traffic on the new version with a way back. Retire the toggle after the release so the architecture does not accumulate unused branches.

**Sources:**

- https://martinfowler.com/articles/feature-toggles.html
- https://martinfowler.com/bliki/CanaryRelease.html
- https://docs.docker.com/get-started/docker-overview/
</details>

<details>
<summary>How do serverless architectures operate and their benefits?</summary>

### How do serverless architectures operate and their benefits?

**Definition:**

AWS Lambda is a serverless compute service. You run code without provisioning or managing servers. Lambda manages the underlying infrastructure, including maintenance, capacity provisioning, scaling, and patching.

**How it works:**

Lambda offers two primitives:

- **Lambda function:** Runs a handler in response to an event or an API call. You connect a trigger such as API Gateway, S3, SQS, or EventBridge. Each invocation is independent, with no shared state, and Lambda scales it horizontally to match demand. One execution environment handles one request at a time. Environments can be reused as warm starts, but state might not persist across invocations. A single invocation lasts up to 15 minutes. Longer workflows use Lambda Durable Functions, up to a year. You pay per request plus GB-seconds. Isolation uses Firecracker microVMs.

- **Lambda MicroVMs:** A separate primitive — an isolated environment with a total lifespan of up to 8 hours, for a dedicated environment per user or job. You create, suspend, resume, and terminate them. Memory and disk state are preserved across suspend and resume. That is a different model from the stateless function handler.

**When to use:**

Use Lambda functions for request-response or event-driven work such as APIs, data processing, and automation, when you do not want to manage servers. Use MicroVMs when each user or job needs its own compute environment, including untrusted code. Do not put durable state only in the function environment, because a later invocation may not see it.

**Sources:**

- https://docs.aws.amazon.com/lambda/latest/dg/welcome.html
</details>

<details>
<summary>What is feature toggling and how does it support DevOps practices?</summary>

### What is feature toggling and how does it support DevOps practices?

**Definition:**

Pete Hodgson defines feature toggles, also called feature flags, as a technique that lets a team modify system behavior without changing code. Feature toggling is a set of patterns for delivering new functionality rapidly but safely.

**How it works:**

- **Toggle point:** Chooses between code paths at runtime.

- **Toggle router:** Answers whether a feature is on. The decision can be a static on/off value, or it can depend on the user. Hodgson groups toggles by how long they live and how dynamic the decision is.

- **Release toggles:** Ship incomplete code on the trunk as latent code so the branch can still be deployed. They support the continuous-delivery idea of separating feature release from code deployment. They are transitional, often removed within a week or two, and the decision is usually static.

- **Experiment toggles:** Place each user in a cohort for A/B or multivariate tests and must stay stable long enough for a significant result.

- **Ops toggles:** Let operators disable or degrade a feature in production, including long-lived kill switches that turn off non-critical work under load.

- **Permissioning toggles:** Change what a specific set of users sees, such as premium, alpha, or beta features.

Toggles add validation complexity. Hodgson's guidance is to constrain how many exist and to remove transient ones.

**When to use:**

Use a release toggle to keep unfinished work on the deployable trunk. Use an experiment toggle for a measured cohort test. Use an ops toggle when operators must turn a feature off without a new deploy. Use a permissioning toggle when a named set of users should see a feature. Do not leave a release toggle in place after the release.

**Examples:**

```javascript
function reticulateSplines() {
  if (featureIsEnabled("use-new-SR-algorithm")) {
    return enhancedSplineReticulation();
  } else {
    return oldFashionedSplineReticulation();
  }
}
```

**Sources:**

- https://martinfowler.com/articles/feature-toggles.html
</details>

<details>
<summary>How would you incorporate monitoring and logging in a cloud-based architecture?</summary>

### How would you incorporate monitoring and logging in a cloud-based architecture?

**Definition:**

Chris Richardson's application-metrics pattern instruments a service so a centralized metrics service can report and alert. Distributed tracing adds a request id so logs that are scattered across services can be tied to one request.

**How it works:**

Instrument individual operations and aggregate the statistics. Aggregation is either push, where the service sends metrics, or pull, where the metrics service scrapes them. The pattern's examples are Prometheus client libraries and AWS CloudWatch, plus the Coda Hale metrics library. The benefit is insight into application behavior. The drawbacks are that metrics code mixes with business logic and that aggregation can need significant infrastructure.

External monitoring only shows overall response time and invocation count. Tracing assigns each external request a unique id, passes that id to every service involved, puts the id on every log message, and records start and end times in a central service. In the example, Spring Cloud Sleuth samples requests and delivers traces to Zipkin through RabbitMQ. Searching aggregated logs by the external request id shows how that request was handled, including sources of latency.

**When to use:**

Use metrics when you need rates and alerts with a push or pull aggregator. Use tracing when one request crosses services and the logs would otherwise be disconnected. Both add infrastructure; the patterns say the runtime overhead should stay small.

**Sources:**

- https://microservices.io/patterns/observability/application-metrics.html
- https://microservices.io/patterns/observability/distributed-tracing.html
</details>

<details>
<summary>What is blue-green deployment and its role in minimizing downtime?</summary>

### What is blue-green deployment and its role in minimizing downtime?

**Definition:**

Martin Fowler describes blue-green deployment as two production environments, as identical as possible. One is live. The new release is finished and tested on the idle one, then the router sends all traffic to it.

**How it works:**

Suppose blue is live. Final testing happens on green. The router cut-over makes green live and blue idle, which is the rollback path: point the router back at blue. Fowler notes the leftover problem of transactions that landed only on the side that was live, and two mitigations he names: feed transactions to both, or switch the application to read-only before cut-over. The idle side then becomes the staging environment for the next release, so the colors swap roles. Schema changes are deployed separately from the application: first a database change that supports both versions, then the new application, then removal of the old schema support. The two sides can be separate machines, virtual machines, or zones with different IP addresses.

**When to use:**

Use it when the cut-over itself must be short and a tested rollback path matters. Fowler also notes it exercises the same switch you would use for a hot standby.

**Examples:**

```text
router -> blue (live)
test green
router -> green (live); blue is the rollback
```

**Sources:**

- [Martin Fowler — Blue Green Deployment](https://martinfowler.com/bliki/BlueGreenDeployment.html)

</details>

<details>
<summary>Describe approaches for achieving multi-tenancy in the cloud.</summary>

### Describe approaches for achieving multi-tenancy in the cloud.

**Definition:**

Multi-tenancy here means one deployed system serves many customers, with isolation chosen either in routing or in CDN configuration. Two sourced approaches are Z-axis routing by customer type and CloudFront multi-tenant distributions.

**How it works:**

On the Scale Cube, Z-axis scaling runs identical code but gives each server a subset of the data. One routing criterion Richardson names is customer type. An application can send paying customers to a different set of servers, with more capacity, than free customers, and give the paying set a higher SLA. Each server then deals only with its subset, which improves cache use and limits a failure to part of the data. The partitioning scheme has to be implemented, and repartitioning is hard.

CloudFront's multi-tenant distribution, through CloudFront SaaS Manager, is for a SaaS provider that serves many customer sites or applications. Several distribution tenants share configuration under centralized control. A standard distribution remains the choice when each site needs its own settings. A standard distribution is what most people start with.

**When to use:**

Route by customer type when tenants need different capacity or a different SLA and the data can be partitioned. Use a CloudFront multi-tenant distribution when many similar sites should share CDN configuration. Use a standard distribution when each site is configured on its own.

**Sources:**

- https://microservices.io/articles/scalecube.html
- https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html
</details>

### Data Management and Integration

<details>
<summary>Discuss the concept and role of a data lake.</summary>

### Discuss the concept and role of a data lake.

**Definition:**

AWS defines a data lake as a centralized repository that stores all structured and unstructured data at any scale. You store data as-is, without structuring it first, and run different analytics on it.

**How it works:**

Data is collected from many sources and moved in its original format, including real-time streams. That avoids defining schema and transformations before storage. The lake holds relational data from operational databases and line-of-business applications, and non-relational data from mobile apps, IoT devices, and social media. Crawling, cataloging, and indexing make it possible to know what is stored. Data must be secured.

Analysts, data developers, and data scientists use tools such as Apache Hadoop, Presto, and Apache Spark, or warehouse and BI products, without moving the data to a separate analytics system. Uses include historical reporting and machine learning models that forecast outcomes.

A data warehouse is different. Its schema is defined in advance for fast SQL, and data is cleaned and transformed so it can be a single source of truth for operational reporting. AWS says an organization often needs both. The main lake risk is raw data with no oversight. Without cataloging and security it becomes a data swamp. Governance, semantic consistency, and access controls are what make a wider audience able to use it. AWS notes public cloud object storage as the environment those surveyed use or plan to use for the primary lake, because compute and storage scale separately.

**When to use:**

Use a lake when you must keep structured and unstructured data before you know every question, and analyze it in place. Use a warehouse when the schema is known and the result must be a cleaned source for operational reports. Plan the catalog and access controls with the lake, or the repository is not usable.

**Sources:**

- https://aws.amazon.com/what-is/data-lake/
</details>

<details>
<summary>Compare ETL and ELT processes.</summary>

### Compare ETL and ELT processes.

**Definition:**

AWS defines ETL as extract, transform, and load: combine data from multiple sources into a data warehouse. Business rules clean and organize the raw data before it is stored for analytics and machine learning. ELT is the extension that reverses the order: load first, then transform.

**How it works:**

ETL moves data from source to destination at intervals, in three steps. Extract copies raw data into a staging area. Transform cleans, deduplicates, reformats, derives, joins, splits, summarizes, or encrypts it. Load moves the result into the warehouse, either as a full load the first time or as an incremental delta after that. ELT loads data directly into the target and transforms it there. The staging area is not required, because the warehouse can map the data. AWS says ELT became practical when cloud warehouses had the processing power for those transformations. ELT fits high-volume unstructured data that is loaded often, and big data where the analytics plan comes after storage. ETL needs the target types, structures, and relationships defined up front. AWS says data scientists mainly use ETL to load legacy databases into a warehouse, and that ELT has become the norm.

**When to use:**

Use ETL when the warehouse schema must be decided before load and the data is being prepared for a known analytical model. Use ELT when the target can transform large or unstructured data after it lands and you do not want a separate staging transform.

**Sources:**

- https://aws.amazon.com/what-is/etl/
</details>

<details>
<summary>How is big data processing handled in software architecture?</summary>

### How is big data processing handled in software architecture?

**Definition:**

AWS handles large and varied data by landing it in a data lake and running analytics in place, including big data processing, real-time analytics, and machine learning, instead of forcing a schema before storage.

**How it works:**

The lake accepts any amount of data, including real-time collection, in the original format. Relational operational data and non-relational data from mobile apps, IoT, and social media sit in the same repository. Processing uses frameworks named on the page: Apache Hadoop, Presto, and Apache Spark, plus commercial warehouse and BI tools. Queries, full-text search, real-time analytics, and machine learning run without moving the data to a separate analytics system.

A cloud lake decouples compute from storage so each scales on its own. The 2021 AWS storage whitepaper lists the same shape: ingest into a central store, catalog, secure, transform raw assets in place, query in place, and keep data in separate buckets as it goes through extraction, transformation, and load. The whitepaper is marked as historical reference.

Z-axis database partitioning is the related Scale Cube move when the processing path is many servers each holding a subset of rows, with a router and a query aggregator.

**When to use:**

Land data in a lake when volume and variety arrive before the questions are known. Keep a warehouse path when the output must be cleaned relational data for operational reporting. Partition a database on the Z axis when one server cannot hold the working set.

**Sources:**

- https://aws.amazon.com/what-is/data-lake/
- https://docs.aws.amazon.com/whitepapers/latest/building-data-lakes/data-lake-vs-data-warehouse-comparison.html
- https://microservices.io/articles/scalecube.html
</details>

<details>
<summary>Describe the role of message brokers in system integration.</summary>

### Describe the role of message brokers in system integration.

**Definition:**

A message broker carries messages between services so the sender and the consumer do not have to be available at the same time. RabbitMQ's queue is the ordered collection those messages wait in. Richardson's messaging pattern is the integration style that uses that channel.

**How it works:**

In RabbitMQ a queue is FIFO: messages are enqueued at the tail and dequeued from the head. Priorities and consumer requeueing can change the order a consumer observes. Queues can be durable, replicated, temporary, or exclusive. Streams are a separate RabbitMQ structure. For topic-based protocols such as STOMP and MQTT, queues still accumulate messages for consumers.

Richardson's styles on that channel:

- **Request/response**
- **Notifications** with no reply
- **Request/asynchronous response**
- **Publish/subscribe** to zero or more recipients
- **Publish/asynchronous response**

The example `OrderService` publishes an `Order Created` event after it saves the order. Benefits are loose runtime coupling, because the sender is decoupled from the consumer, and higher availability, because the broker buffers until the consumer can process. The broker itself must be highly available. Request/reply over messages is more complex than a direct call.

**When to use:**

Use a broker when the sender should not fail just because the consumer is down, or when one event must reach zero or more subscribers. Use a direct call when you need a prompt reply and both sides will be up for the whole request. Synchronous communication is the case Richardson contrasts: both client and service must be available for the duration.

**Sources:**

- https://www.rabbitmq.com/docs/queues
- https://microservices.io/patterns/communication-style/messaging.html
</details>

<details>
<summary>Explain the significance of an API Gateway.</summary>

### Explain the significance of an API Gateway.

**Definition:**

Chris Richardson's API gateway is the single entry point for all clients of a microservice application. It proxies some requests to one service and fans other requests out to several services.

**How it works:**

Clients often need data that is spread across fine-grained services. A product page in the pattern's store example needs product info, price, orders, inventory, and reviews. Different clients need different shapes of that data, and a mobile network is slower and higher latency than a LAN, so a mobile client can make only a few calls while a server-side web app can make many. Instance locations change, the partition into services can change, and internal protocols may not be web-friendly.

The gateway hides that partition and the host and port of each instance. It can expose a different API per client. The backends-for-frontends variation is a separate gateway for each kind of client: web, mobile, and external third parties. The gateway can also check that the client is authorized. Netflix is the known use. One extra network hop adds response time. Richardson says that for most applications the cost of that extra round trip is insignificant, while the gateway removes round trips the client would otherwise make. For high load he prefers an event-driven implementation, naming Netty, Spring Reactor, and Node.js.

**When to use:**

Use a gateway when clients would otherwise call many services, track moving instances, or use internal protocols. Use a separate backend for each frontend when web, mobile, and third-party clients need different APIs. Skip it only when a single client already talks to one stable service and the extra hop is not worth another component.

**Sources:**

- https://microservices.io/patterns/apigateway.html
</details>

<details>
<summary>How is Event Sourcing applied in architecture?</summary>

### How is Event Sourcing applied in architecture?

**Definition:**

Martin Fowler defines event sourcing as capturing every change to application state as an event object, stored in the order the events were applied, for the same lifetime as the state itself.

**How it works:**

A ship-tracking service can update a ship's current port in place. With event sourcing the service first records an event such as "King Roy departed San Francisco" and then applies that event to the current state. The event log can be queried and can rebuild a past state. Fowler presents this as an alternative to storing only the latest application state.

**When to use:**

Use it when the architecture must answer how the current state was reached, not only what it is now. The draft notes that the log is also a base for adjusting state when a past event has to be reinterpreted.

**Examples:**

```text
depart(King Roy, San Francisco) -> event stored, then location updated
arrive(King Roy, Hong Kong)     -> event stored, then location updated
```

**Sources:**

- [Martin Fowler — Event Sourcing](https://martinfowler.com/eaaDev/EventSourcing.html)

</details>

<details>
<summary>Discuss strategies for managing database schema migrations.</summary>

### Discuss strategies for managing database schema migrations.

**Definition:**

Danilo Sato's canary-release note says database changes need attention during a rollout because two versions of the application run at once. The mitigation he names is a parallel change, so the database supports both versions during that phase.

**How it works:**

A canary routes a subset of users to the new version and can roll back by routing them to the old version. While both versions run, each must be able to use the database. A parallel change is the technique Sato cites for that window. The same note says a parallel change is also how you support old and new clients when you do not control when installed or mobile software upgrades. PostgreSQL replication is a separate concern: standbys follow a primary, and a synchronous commit waits for every server, which changes latency but does not itself version a schema.

**When to use:**

Use a parallel change whenever a canary or a mixed set of client versions will read and write during the migration. Contract the schema only after the old version is gone. Sato says to keep the number of concurrent versions small.

**Sources:**

- https://martinfowler.com/bliki/CanaryRelease.html
</details>

<details>
<summary>Best practices for data consistency in distributed systems?</summary>

### Best practices for data consistency in distributed systems?

**Definition:**

IBM states the CAP theorem: a distributed system can deliver only two of three characteristics — consistency, availability, and partition tolerance. PostgreSQL states the related replication choice between waiting for every server and allowing lag.

**How it works:**

IBM defines consistency as every client seeing the same data at the same time, which requires a write to be replicated to the other nodes before it is successful. Availability means every working node returns a valid response. A partition is a lost or delayed connection between nodes; partition tolerance means the cluster keeps working despite that. IBM says partitions cannot be avoided in a distributed system, so a CA database is not practical there. A CP system, such as MongoDB in IBM's classification, stays consistent by making a non-consistent node unavailable during a partition. An AP system, such as Cassandra in that classification, stays available and may return older data until nodes resync. PostgreSQL, IBM says, can deliver consistency and availability and be deployed on multiple nodes using replication.

PostgreSQL's own documentation separates synchronous replication, which does not commit a modifying transaction until all servers have committed it, from asynchronous replication, which can lose transactions on failover and return slightly stale reads.

**When to use:**

Choose a CP store or synchronous replication when readers must not see a stale write. Choose an AP store or asynchronous replication when the system must answer during a partition and a later resync is acceptable. IBM's example: an e-commerce or payment service that depends on consistency may use a relational database such as PostgreSQL; a service that can tolerate eventual consistency may use an AP database such as Cassandra.

**Sources:**

- https://www.ibm.com/think/topics/cap-theorem
- https://www.postgresql.org/docs/current/high-availability.html
</details>

<details>
<summary>How to integrate third-party services securely into your architecture?</summary>

### How to integrate third-party services securely into your architecture?

**Definition:**

RFC 6749 defines OAuth 2.0 as a way for a third-party application to obtain limited access to an HTTP service, either on behalf of a resource owner or on its own behalf, without receiving the resource owner's credentials.

**How it works:**

The RFC lists what goes wrong if the owner shares a password with the third party: the third party stores that password, often in clear text; it gets broader access than it needs; the owner cannot revoke one third party without changing the password for all of them; and a compromise of the third party exposes the password. OAuth separates the client from the resource owner. An authorization server issues an access token — a string for a specific scope, lifetime, and other access attributes — after the owner approves. The client presents that token to the resource server. The RFC's example is a printing service accessing photos without the user's password. OAuth 2.0 is specified for HTTP and is not backward compatible with OAuth 1.0.

Twelve-factor config keeps credentials for external services, such as S3 or Twitter, in environment variables, not in code. The litmus test is that the codebase could be open-sourced without leaking credentials. Richardson's externalized-configuration pattern reads database location and credentials from an external source at startup so the same build runs in each environment. An API gateway can also verify that the client is authorized before it fans out.

**When to use:**

Use OAuth when a third party needs limited access to a user's HTTP resource. Put that client's own credentials in environment variables for each deploy. Do not check them into the repository.

**Sources:**

- https://www.rfc-editor.org/rfc/rfc6749.html
- https://12factor.net/config
- https://microservices.io/patterns/externalized-configuration.html
- https://microservices.io/patterns/apigateway.html
</details>

<details>
<summary>When choosing between SQL and NoSQL databases, what are the considerations?</summary>

### When choosing between SQL and NoSQL databases, what are the considerations?

**Definition:**

IBM contrasts vertically scalable SQL (relational) databases with NoSQL databases that are horizontally scalable and distributed by design. The choice IBM ties to that contrast is which two CAP properties the store keeps when a network partition happens.

**How it works:**

IBM classifies NoSQL stores by the CAP pair they deliver.

- **CP database:** Keeps consistency and partition tolerance and gives up availability — during a partition it shuts down the non-consistent node. IBM places MongoDB here. MongoDB is a single-primary system; secondaries replicate the primary's operation log. If the primary is unavailable, the secondary with the newest log is elected, and clients cannot write until the others catch up, so the data stays consistent.

- **AP database:** Keeps availability and partition tolerance and gives up consistency for a time. IBM places Cassandra here: there is no primary, clients can write to any node, and repair closes the gaps.

- **CA pair:** Not practical once partitions are possible. IBM says many relational databases, including PostgreSQL, deliver consistency and availability and can be placed on multiple nodes with replication.

IBM also names a product reason: if you must iterate the data model quickly and scale horizontally, and you can tolerate eventual consistency, an AP database such as Cassandra or Apache CouchDB fits. If the application depends on consistency, as in e-commerce or payments, IBM points to a relational database such as PostgreSQL.

**When to use:**

Prefer a relational database when the write must be the same value for every reader and you will run replication, accepting the commit wait PostgreSQL documents for synchronous mode. Prefer a horizontally scaled NoSQL store when the data is distributed across nodes and you have chosen either CP or AP explicitly.

**Sources:**

- https://www.ibm.com/think/topics/cap-theorem
- https://www.postgresql.org/docs/current/high-availability.html
</details>

### Reliability, Maintenance, and Evolution

<details>
<summary>Explain fault tolerance and its incorporation into software architecture.</summary>

### Explain fault tolerance and its incorporation into software architecture.

**Definition:**

Fault tolerance here means the system keeps serving after a piece fails. NGINX names it as a goal of load balancing. Kubernetes self-healing and a message broker's buffer are two concrete mechanisms.

**How it works:**

NGINX spreads HTTP and HTTPS traffic across an upstream group so one server is not the only place a request can land, and a `backup` server is there when the others are unavailable. Kubernetes restarts a failed container according to `restartPolicy`, replaces a failed Pod in a Deployment to keep the replica count, drops a failed Pod from Service endpoints, and can reattach a PersistentVolume on a new node. Richardson's messaging pattern buffers messages until the consumer can process them, so the sender is not tied to the consumer being up. The broker itself has to be highly available. PostgreSQL failover promotes a standby when the primary fails. Synchronous replication avoids losing committed transactions; asynchronous replication might lose some.

Z-axis partitioning on the Scale Cube limits a failure to the partition that died.

**When to use:**

Add more than one upstream or Pod when a single process must not stop the service. Put a buffer between services when the consumer may be down while the sender should continue. Add a database standby when the primary's disk or host can fail. Choose sync or async from the loss window you can accept.

**Sources:**

- https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/
- https://kubernetes.io/docs/concepts/architecture/self-healing
- https://microservices.io/patterns/communication-style/messaging.html
- https://www.postgresql.org/docs/current/high-availability.html
- https://microservices.io/articles/scalecube.html
</details>

<details>
<summary>What architectural practices facilitate maintainability and evolution?</summary>

### What architectural practices facilitate maintainability and evolution?

**Definition:**

Martin Fowler defines refactoring as a change to the internal structure of software to make it easier to understand and cheaper to modify without changing observable behavior. Pete Hodgson adds that feature toggles, which let you evolve behavior without a separate branch, have a carrying cost that has to be removed.

**How it works:**

Fowler's noun and verb are the same idea: restructure by a series of refactorings and do not change what observers see. Richardson's Repository, already a separate answer in this file, is one structure that keeps query logic from being copied through the domain. Y-axis scaling splits the system into services so one function can change without cloning the whole application, at the cost of distribution.

Hodgson's release toggles let unfinished code land on the trunk that you deploy, which is how continuous delivery separates release from deployment. He also says toggles add complexity. A release toggle should usually live a week or two. A long-lived permissioning toggle should not be a scattered `if`. Canary releases evolve production by running the new version for a subset first, with a parallel database change so both versions work, and then removing the old version.

**When to use:**

Refactor when the structure makes the next change expensive and the observable behavior must stay. Use a short-lived release toggle when the trunk must stay deployable during the change. Remove it when the new path is the only path. Split a service on the Y axis when the change is isolated to one function and the team should not edit the whole system.

**Sources:**

- https://martinfowler.com/bliki/DefinitionOfRefactoring.html
- https://martinfowler.com/articles/feature-toggles.html
- https://martinfowler.com/bliki/CanaryRelease.html
- https://microservices.io/articles/scalecube.html
</details>

<details>
<summary>Why is documentation crucial for software architecture maintenance?</summary>

### Why is documentation crucial for software architecture maintenance?

**Definition:**

Michael Nygard argues that the hard thing to track is why an architecture decision was made. Without that record, a later developer can only accept the decision blindly or change it blindly.

**How it works:**

Blind acceptance is fine only while the context is unchanged. If too many decisions are accepted without understanding, the team becomes afraid to change anything. Blind reversal can damage a non-functional requirement that has not been tested yet. Agile projects still need documentation, but only documents the team will keep current. Large specifications are not read or updated. Nygard's architecture decision record is a one- or two-page file: title, context, decision, status, and consequences, including the negative ones. Records live in the repository, numbered, and a reversed decision is kept and marked superseded. He writes each record as a conversation with a future developer. The SEI's ATAM benefits list the same maintenance need from the evaluation side: improved architecture documentation and a documented basis for architectural decisions.

**When to use:**

Write an ADR when a decision affects structure, non-functional characteristics, dependencies, interfaces, or construction. Keep it short enough that the next person will update it. Mark it superseded instead of deleting it when the decision changes.

**Sources:**

- https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions
- https://www.sei.cmu.edu/library/architecture-tradeoff-analysis-method-collection/
</details>

<details>
<summary>How do you manage technical debt within a software architecture?</summary>

### How do you manage technical debt within a software architecture?

**Definition:**

Martin Fowler treats technical debt as Ward Cunningham's metaphor for cruft: internal-quality gaps that make the next change cost more than it would in a clean design. The extra time is the interest. Cleaning the structure is paying the principal.

**How it works:**

Fowler's example: a feature takes four days in a clear module structure and six days with the cruft. Two days are interest. Spending five days to remove the cruft is a loss for that one feature and a gain if similar features follow. Because the estimates are rough, he prefers paying principal gradually on the code you are already changing. Cruft in a stable area that nobody touches does not charge interest until someone edits it. Deliberate, prudent debt is a different case from reckless debt; he points to the technical debt quadrant for that split. Taking on debt to ship faster only works below the design-payoff line of the design stamina hypothesis, which he says teams hit in weeks.

**When to use:**

Spend the cleanup on the parts of the architecture that change often. Leave untouched, stable cruft alone until a change actually goes there.

**Examples:**

```text
clear design: 4 days per feature
with cruft:   6 days per feature
pay down on the next change in that area, not on a module nobody edits
```

**Sources:**

- [Martin Fowler — Technical Debt](https://martinfowler.com/bliki/TechnicalDebt.html)

</details>

<details>
<summary>Discuss the importance of automated testing for architectural resilience.</summary>

### Discuss the importance of automated testing for architectural resilience.

**Definition:**

Chris Richardson's service component test is an automated suite that checks one service in isolation, with test doubles for every service it calls. He contrasts that with end-to-end tests, which launch many services and are difficult, slow, brittle, and expensive.

**How it works:**

The benefit is that an isolated test is easier, faster, more reliable, and cheaper. The drawback he names is that the tests might pass while the application fails in production. The open issue is how to keep the doubles faithful to the real callees. A canary is the production-side check that unit of isolation does not replace: a subset of users hits the new version, metrics are watched, and rollback is a route back to the old version. Kubernetes self-healing restarts a failed container and replaces a failed Pod, which is runtime resilience rather than a test. The component test is what tells you the service's own behavior before that traffic arrives, and Richardson's drawback is why a passing suite is not evidence the deployed system will hold.

**When to use:**

Test a service in isolation when end-to-end runs are too slow to be the only check. Treat a green suite as incomplete until the doubles match production behavior, and use a canary when the risk is the interaction between services.

**Sources:**

- https://microservices.io/patterns/testing/service-component-test.html
- https://martinfowler.com/bliki/CanaryRelease.html
- https://kubernetes.io/docs/concepts/architecture/self-healing
</details>

<details>
<summary>Define “refactoring” in the context of software architecture.</summary>

### Define “refactoring” in the context of software architecture.

**Definition:**

Martin Fowler gives two definitions:

- **As a noun:** Refactoring is a change made to the internal structure of software to make it easier to understand and cheaper to modify without changing its observable behavior.

- **As a verb:** To restructure software by applying a series of refactorings without changing its observable behavior.

**How it works:**

The constraint is the observable behavior. Callers, including users and other services, still see the same results. What changes is the structure those results come from. In an architecture that is already split into services, a refactoring can sit inside one service. A Y-axis split that moves a function into a new service changes the deployment shape; Fowler's definition still requires the observable behavior to stay the same while that move happens. Pete Hodgson's release toggle is one way to ship both structures and choose the old behavior until the new structure is ready. The toggle is not the refactoring. It is a way to keep behavior stable while the structure changes.

**When to use:**

Refactor when understanding or the next modification is too expensive and the external behavior is not the thing you intend to change. If the behavior must change, that is a feature change, and Fowler's definition does not cover it.

**Sources:**

- https://martinfowler.com/bliki/DefinitionOfRefactoring.html
- https://martinfowler.com/articles/feature-toggles.html
</details>

<details>
<summary>What is graceful degradation in system design?</summary>

### What is graceful degradation in system design?

**Definition:**

Pete Hodgson describes graceful degradation as an ops toggle that turns off non-vital functionality when the system is under unusually high load, so the rest keeps working.

**How it works:**

An ops toggle controls an operational aspect of behavior. Operators must be able to flip it quickly. Waiting for a new release is too slow. A short-lived ops toggle is retired once a new feature's performance is trusted. A few become long-lived kill switches. Hodgson's example is disabling a recommendations panel that is expensive to generate, and a retailer that could disable many non-critical features in the purchasing flow before a high-demand launch. He calls those long-lived toggles a manually managed circuit breaker. They are dynamic: the decision changes in production without a deploy.

**When to use:**

Add a kill switch for a feature whose cost can be dropped without stopping the core path. Do not use it for a function the system cannot serve without. Remove a short-lived ops toggle after the rollout is trusted.

**Sources:**

- https://martinfowler.com/articles/feature-toggles.html
</details>

<details>
<summary>How do you plan for backward compatibility when evolving architecture?</summary>

### How do you plan for backward compatibility when evolving architecture?

**Definition:**

Danilo Sato's plan is to keep the old and new versions running together and to make the shared database support both, until every caller has moved.

**How it works:**

A canary routes only a subset to the new version. Rollback sends them back, so the old version must still work. A parallel change is how the database supports both versions during that migrate phase. Installed and mobile clients upgrade when they choose. Sato says to support both on the backend and, once usage of the old client falls to a chosen level, contract the backend to the new version only. Kubernetes keeps clients compatible across a port change by mapping the Service `port` to a named `targetPort`. The next Pod can listen on a different number. The client still calls the Service port. Nygard says a reversed architecture decision stays in the record, marked superseded, so the old contract's reason is still visible.

**When to use:**

Run both versions whenever you do not control the upgrade of every caller. Contract only after the old callers are gone. Name container ports when the wire port clients use must stay stable.

**Sources:**

- https://martinfowler.com/bliki/CanaryRelease.html
- https://kubernetes.io/docs/concepts/services-networking/service/
- https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions
</details>

<details>
<summary>Define feature deprecation and its architectural considerations.</summary>

### Define feature deprecation and its architectural considerations.

**Definition:**

Deprecation here is the period when an old behavior still runs but is no longer the one you are moving toward. Sato describes that period for client versions. Nygard describes it for a decision record. Hodgson describes it for a release toggle that must be removed.

**How it works:**

Sato keeps the old backend until usage of the old clients falls, then contracts so only the new version is supported. Until then both contracts exist, and the database must support both through a parallel change. He warns that the number of concurrent versions should stay small. Nygard does not delete a reversed decision. The ADR is marked deprecated or superseded and points at its replacement, because it is still relevant that this used to be the decision. Hodgson's release toggles are transitional, often a week or two. Leaving them in place is the carrying cost. A permissioning toggle can live for years if it is the product rule, such as a premium feature, and that one is not a deprecation.

**When to use:**

Mark a decision superseded when a new ADR replaces it, and keep the old file. Support an old client contract only while those clients are still in use, then remove it. Delete a release toggle once the new path is the only path.

**Sources:**

- https://martinfowler.com/bliki/CanaryRelease.html
- https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions
- https://martinfowler.com/articles/feature-toggles.html
</details>

<details>
<summary>Discuss architectural strategies for effective debugging.</summary>

### Discuss architectural strategies for effective debugging.

**Definition:**

Richardson's distributed tracing is the debugging strategy for a request that crosses services: one external request id is passed along, written into every log line, and used to retrieve the operations from a central store.

**How it works:**

External monitoring shows only overall response time and invocation count. Logs for one request are otherwise scattered. The id is assigned at the edge of the system and handed to every service involved. Start and end times of the operations go to a centralized service. The example instruments Spring with Spring Cloud Sleuth and delivers traces to Zipkin over RabbitMQ. You search the aggregated logs for that id. The pattern says this shows sources of latency. Storing traces can require significant infrastructure, so the overhead of instrumentation should stay small. Application metrics tell you which operation is hot across many requests. The trace tells you the path of one request once you are debugging it.

**When to use:**

Pass a request id on every call as soon as more than one service handles the work. Open the trace when a metric or a user report names a slow or failed request. Do not expect a single service's local log to show the whole path.

**Sources:**

- https://microservices.io/patterns/observability/distributed-tracing.html
- https://microservices.io/patterns/observability/application-metrics.html
</details>

### Mobile and IoT Architecture

<details>
<summary>Discuss considerations for mobile application architecture.</summary>

### Discuss considerations for mobile application architecture.

**Definition:**

Richardson's API gateway treats a mobile client as a different caller from a server-side web app, because the mobile network is slower and higher latency and the screen needs a different shape of data.

**How it works:**

A mobile client can make only a few calls. A server-side app on a LAN can make many. The gateway fans out to fine-grained services and returns one response, which is why Richardson says an API gateway is essential for mobile applications. The backends-for-frontends variation is a separate gateway whose API matches the mobile client rather than the desktop page. CloudFront serves cached content from the edge location with the lowest latency. Sato notes that software installed on a device upgrades on the user's schedule, so the backend uses a parallel change until old client versions fall off, then contracts. Twelve-factor processes stay stateless. The device is not a durable store the server can assume will be the same process next time.

**When to use:**

Give mobile its own gateway API when the payload and the call count must be smaller than the desktop UI. Support the previous client version until its usage drops. Cache static content at the edge.

**Sources:**

- https://microservices.io/patterns/apigateway.html
- https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html
- https://martinfowler.com/bliki/CanaryRelease.html
- https://12factor.net/processes
</details>

<details>
<summary>How does IoT architecture differ from traditional architectures?</summary>

### How does IoT architecture differ from traditional architectures?

**Definition:**

AWS describes the traditional path as transmitting data from smart devices, such as sensors and smartphones, to a central data center for processing. Edge and lake designs change that path because the volume has outpaced the network and the data is not only relational rows.

**How it works:**

Edge computing moves storage and compute closer to the devices that produce the data and the users who consume it. Upstream IoT work collects from sensors, filters locally, and sends on only what the center needs. AWS groups that data as redundant measurements, data that must be stored long term, and data with an immediate effect, such as a temperature that must turn a heater on. A factory robot that must shut down cannot wait on a remote round trip. A data lake stores the non-relational result, including IoT and mobile data, without a schema chosen before capture. A traditional warehouse instead defines the schema in advance for cleaned relational reporting.

**When to use:**

Keep processing next to the sensor when the decision is local or the link is poor. Send the reduced stream to a lake when the questions are not all known yet. Use a warehouse when the IoT feed has already been reduced to a known relational model.

**Sources:**

- https://aws.amazon.com/what-is/edge-computing/
- https://aws.amazon.com/what-is/data-lake/
</details>

<details>
<summary>Define edge computing in the context of IoT.</summary>

### Define edge computing in the context of IoT.

**Definition:**

AWS defines edge computing as bringing information storage and computing closer to the devices that produce the information and the users who consume it.

**How it works:**

Traditionally, sensors and smartphones sent data to a central data center. Edge systems move that processing closer, which AWS says improves performance, reduces bandwidth, and gives faster real-time insight. For IoT, one placement is compute on the device itself: a sensor filters with predetermined rules before transmission. Another is a local data center next to the source, such as server racks and a LAN inside a wind turbine. A third is a regional edge that processes several sensors in one geography. Upstream work classifies readings into redundant data, data for long-term storage, and data that must act immediately, and sends only the critical part onward. Manufacturing uses sensors and gateways for on-site collection and machine-to-machine communication. Connectivity at remote sites is often poor, so the edge still processes, analyzes, and stores when the network is down.

**When to use:**

Put compute on or next to the IoT device when a round trip to a central data center is too slow or the link is unreliable. Send only the filtered result to the center.

**Sources:**

- https://aws.amazon.com/what-is/edge-computing/
</details>

<details>
<summary>How do you manage data synchronization between mobile devices and servers?</summary>

### How do you manage data synchronization between mobile devices and servers?

**Definition:**

AWS edge computing manages what moves from a device to a center by classifying data at the source and transmitting only the critical part. Sato manages the other direction: the server must keep working with device software that has not upgraded yet.

**How it works:**

Upstream, a sensor or device produces redundant readings, readings worth keeping long term, and readings that must cause an immediate local action. The edge keeps the immediate decision local and sends the rest that matters. AWS says sending everything is expensive in bandwidth, and a poor link is a reason to store and process at the edge until the network can take the transfer. Information that does go back can be encrypted first. Downstream, a CDN caches content closer to the device so the device is not the one pulling every byte from the origin. On the contract side, Sato's parallel change lets the server support both the old and the new client until usage of the old version falls, because a mobile install does not upgrade on the server's schedule.

**When to use:**

Filter on the device when the link cannot carry every reading. Encrypt what you do send. Keep the previous API alive until the installed base has moved.

**Sources:**

- https://aws.amazon.com/what-is/edge-computing/
- https://martinfowler.com/bliki/CanaryRelease.html
- https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html
</details>

<details>
<summary>Address battery life and resource constraints in mobile/IoT architectures.</summary>

### Address battery life and resource constraints in mobile/IoT architectures.

**Definition:**

AWS frames the resource limit as bandwidth, latency, and unreliable connectivity, not as a battery-capacity API. The architectural response on that page is to process locally and send less data.

**How it works:**

A mobile network, in Richardson's gateway note, is slower and higher latency than a LAN, so the client must make few calls. The gateway does the fan-out. Edge computing reduces the data sent from the site to the data center because only critical data is transmitted, which AWS ties to lower bandwidth cost. Local filtering on the device drops redundant readings, such as a room temperature sampled every five minutes, before they cross the network. Remote sites with scarce connectivity store and analyze on the edge so a dropped link does not stop the operation. CloudFront shortens the path for content the device does fetch by serving it from a nearby edge. The pages do not publish a battery budget or a radio-duty-cycle setting.

**When to use:**

Cut round trips and payload size for a mobile client. Filter IoT readings on the device before upload. Serve repeated content from a nearby cache. Treat a specific battery model as a measurement you make, not as a figure these architecture pages provide.

**Sources:**

- https://aws.amazon.com/what-is/edge-computing/
- https://microservices.io/patterns/apigateway.html
- https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html
</details>

### Communication and Networking

<details>
<summary>Explain RESTful API design principles.</summary>

### Explain RESTful API design principles.

**Definition:**

MDN defines REST, Representational State Transfer, as a group of architecture constraints that bring about efficient, reliable, and scalable distributed systems. A resource, such as a document, is transferred through well-recognized, language-agnostic, standardized client/server interactions. A service is RESTful when it adheres to these constraints.

**How it works:**

MDN's glossary distinguishes that idea from the everyday name. HTTP APIs are often called RESTful even when they do not follow every constraint. For a beginner, MDN says a REST API means an HTTP service you can call with standard web libraries and tools. GraphQL's HTTP guide draws the contrast that matters for design: REST's core concept is the resource, identified by URLs, whereas a GraphQL service uses a single endpoint because its model is an entity graph. An API gateway can still expose that HTTP API as the public, web-friendly protocol and translate it to whatever the internal services speak.

**When to use:**

Call an HTTP API REST when it actually follows the constraints, not merely because it uses HTTP. Use resource URLs when clients should identify each entity by a URL and use ordinary HTTP tools. Use a single-endpoint graph API when the client must ask for a shape that is not one resource.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Glossary/REST
- https://graphql.org/learn/serving-over-http/
- https://microservices.io/patterns/apigateway.html
</details>

<details>
<summary>Considerations for designing a GraphQL API?</summary>

### Considerations for designing a GraphQL API?

**Definition:**

GraphQL's HTTP guide says the specification does not require a particular transport, and HTTP is the common choice. The model is an entity graph, so the server uses one endpoint, usually `/graphql`, not a URL per entity.

**How it works:**

Authentication middleware runs before GraphQL validation, so execution sees the same session and user as other HTTP handlers. Authorization waits until execution. Field-level checks live in the business logic resolvers call, which allows a partial response when an authorization error is raised. Clients send `Accept: application/graphql-response+json`. `POST` is required for queries and mutations. `GET` is optional and only for queries, which can help HTTP or CDN caching, but long documents can exceed URL length limits. Persisted documents, automatic persisted queries, or trusted documents let the client send an id instead of the full text. A JSON body carries `query`, and optionally `operationName`, `variables`, and `extensions`. `operationName` is required when the document has several operations. The response is `data`, and `errors` only when there are errors. A non-null `data` value is a `2xx` even if some errors are present, because HTTP has no partial-success status. These guidelines are for stateless queries and mutations. Subscriptions use other transports. The page aligns with the GraphQL-over-HTTP draft, which is not yet a mandatory spec.

**When to use:**

Use one HTTP endpoint when clients must ask for a graph rather than a single resource. Authenticate before execution and authorize per field inside it. Prefer `POST` unless a query is short enough for a cacheable `GET`.

**Sources:**

- https://graphql.org/learn/serving-over-http/
</details>

<details>
<summary>Describe WebSocket communication and when it’s preferred.</summary>

### Describe WebSocket communication and when it’s preferred.

**Definition:**

MDN defines the WebSocket API as a way to open a two-way interactive session between the browser and a server. You send messages and receive responses without polling the server for a reply.

**How it works:**

The stable interface is `WebSocket`. It has no backpressure, so if messages arrive faster than the page can handle them, the browser may buffer until memory fills, or the CPU may sit at 100 percent, or both. `WebSocketStream` is a promise- and streams-based alternative that applies backpressure, and MDN marks it non-standard and limited to one rendering engine. The opening handshake uses HTTP headers the browser sets, including `Sec-WebSocket-Key`, and the server answers with `Sec-WebSocket-Accept`. An open socket can keep the page out of the back-forward cache, so the page should close it when the user leaves. MDN says WebTransport is expected to replace WebSocket for many applications because it adds unidirectional streams, datagrams, and backpressure, at the cost of complexity and narrower support. Use WebSocket when a standard two-way session is enough and you need wide browser support.

**When to use:**

Prefer WebSocket over polling when the server must push messages on an open session and browser support has to be wide. Prefer `WebSocketStream` or WebTransport when you need backpressure and can accept the support limits MDN states.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API
</details>

<details>
<summary>What is long-polling and how is it supported architecturally?</summary>

### What is long-polling and how is it supported architecturally?

**Definition:**

MDN's WebSocket page does not specify the long-polling request loop. It names long polling as part of how Socket.IO transfers data, alongside WebSocket, and it says the point of WebSocket is to receive responses without polling the server.

**How it works:**

WebSocket opens a two-way session. The browser sends and the server pushes on that session. The handshake is still HTTP, using `Sec-WebSocket-Key` and `Sec-WebSocket-Accept`, and then the traffic is no longer a series of polls. MDN lists Socket.IO as a long-polling and WebSocket transfer protocol for Node.js, and SignalR as a library that uses WebSockets when they are available and falls back to other techniques when they are not, while application code stays the same. An architecture that needs this fallback keeps a server that can speak both the socket and the fallback SignalR selects. The page does not document the timer or the held HTTP request of long polling itself.

**When to use:**

Prefer a WebSocket session when the browser support MDN describes is enough and you want the server to push without polling. Keep a fallback path, as SignalR does, when some clients will not get a WebSocket. Do not treat this note as a specification of the long-poll loop.

**Sources:**

- https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API
</details>

<details>
<summary>How can network latency impact architecture and how is it mitigated?</summary>

### How can network latency impact architecture and how is it mitigated?

**Definition:**

Latency is the delay the fetched pages keep returning to: CloudFront's time to the first byte, a mobile network that is slower than a LAN, and an extra gateway hop. Architecture changes where the bytes are and how many round trips the client makes.

**How it works:**

CloudFront routes a request to the edge location with the lowest latency and serves a cached object immediately. A miss still goes to the origin, over the AWS backbone, which cuts the number of networks crossed. Richardson says a mobile client can afford only a few calls, so an API gateway fans out on the server and returns one response. He also says the gateway's own extra hop is insignificant for most applications. Edge computing moves the work next to the device so a factory robot is not waiting on a remote data center. NGINX lists reduced latency as a reason to spread requests across an upstream group. Tracing records start and end times so the slow hop is visible. A gRPC deadline stops a call that has already waited too long.

**When to use:**

Cache at the edge when the same content is fetched from far away. Collapse chatty calls behind a gateway for high-latency clients. Move the decision next to the device when the round trip itself is the failure. Trace before you add capacity.

**Sources:**

- https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html
- https://microservices.io/patterns/apigateway.html
- https://aws.amazon.com/what-is/edge-computing/
- https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/
- https://microservices.io/patterns/observability/distributed-tracing.html
- https://github.com/grpc/grpc.io/blob/main/content/en/blog/deadlines.md
</details>

### Architecture Analysis and Evaluation

<details>
<summary>How do you assess the quality of a software architecture?</summary>

### How do you assess the quality of a software architecture?

**Definition:**

The SEI's Architecture Tradeoff Analysis Method evaluates an architecture against quality-attribute goals before the system is built, and shows how those goals trade off. A fitness function, as Thoughtworks cites _Building Evolutionary Architectures_, is an objective check that some characteristic still holds as the system changes.

**How it works:**

ATAM elicits business drivers and the architecture, refines them into scenarios, and analyzes the decisions behind each scenario. The output is risks, non-risks, sensitivity points, and tradeoff points, then risk themes tied to business drivers. A utility tree breaks utility into attributes such as performance, availability, security, modifiability, and usability, down to prioritized scenarios. An evaluation gathers a trained team, architects, and stakeholders and typically takes three to four days. The benefit list includes risks found early, clearer quality requirements, and a documented basis for decisions. A fitness function summarizes how close a design is to its aims and can be made of tests, metrics, and monitors that run continually, which is how Thoughtworks says architects preserve characteristics in an evolutionary architecture. The two are different cadences: ATAM is a convened evaluation. A fitness function is an automated check you keep.

**When to use:**

Run an ATAM when stakeholders must see risks and tradeoffs before or during a major architectural choice. Add fitness functions when a characteristic must be rechecked on later changes, not only in a workshop.

**Sources:**

- https://www.sei.cmu.edu/library/architecture-tradeoff-analysis-method-collection/
- https://www.thoughtworks.com/radar/techniques/architectural-fitness-function
</details>

<details>
<summary>Describe the Architecture Tradeoff Analysis Method (ATAM).</summary>

### Describe the Architecture Tradeoff Analysis Method (ATAM).

**Definition:**

The Software Engineering Institute defines ATAM as a method for evaluating software architectures relative to quality-attribute goals. It shows both how well the architecture meets each goal and how the goals trade off against each other. Evaluations expose risks that can block the organization's business goals.

**How it works:**

An evaluation typically takes three to four days and brings a trained evaluation team, architects, and stakeholder representatives. Nine steps follow. Present the method. A project spokesperson presents business drivers such as high availability, time to market, or high security. The architect presents how the architecture addresses those drivers. Architectural approaches are identified and not yet analyzed. A utility tree specifies quality factors down to scenarios with stimuli and responses, and prioritizes them. High-priority approaches are analyzed for risks, sensitivity points, and tradeoff points. The whole stakeholder group brainstorms and votes on a larger scenario set. Those scenarios are analyzed again as test cases. The team presents findings: approaches, the utility tree, scenarios, attribute-specific questions and answers, risks, non-risks, sensitivity points, tradeoffs, and risk themes that threaten business drivers. SEI says the most important result is an improved architecture. Benefits include earlier risks, more communication, clearer quality requirements, better documentation, and a recorded basis for decisions.

**When to use:**

Use ATAM when a system must be modifiable, fast, secure, or reliable and you need to know whether the architecture can do that without building it first. Include the stakeholders whose goals pull the architect in different directions: users, maintainers, development management, and the customer.

**Sources:**

- https://www.sei.cmu.edu/library/architecture-tradeoff-analysis-method-collection/
</details>

<details>
<summary>What are architectural fitness functions?</summary>

### What are architectural fitness functions?

**Definition:**

Thoughtworks, citing _Building Evolutionary Architectures_, defines an architectural fitness function as an objective integrity assessment of some architectural characteristic. The term is borrowed from evolutionary computing, where a fitness function says how close a candidate is to the aims, and thus what "better" means.

**How it works:**

The assessment may be made of verification you already have: unit testing, metrics, monitors, and so on. Thoughtworks says architects can communicate, validate, and preserve architectural characteristics in an automated, continual way, and that this is the key to evolutionary architectures. The Radar entry was Trial in November 2017 and May 2018 and is not on the current edition. The page says an older blip might be judged differently today. It does not list a required tool or a numeric threshold.

**When to use:**

Add a fitness function when a characteristic, such as a latency or a structural rule, must be rechecked as the architecture evolves, and you can express that check as a test, a metric, or a monitor. It does not replace a convened evaluation such as ATAM. It keeps one characteristic honest between evaluations.

**Sources:**

- https://www.thoughtworks.com/radar/techniques/architectural-fitness-function
- https://www.sei.cmu.edu/library/architecture-tradeoff-analysis-method-collection/
</details>

<details>
<summary>Conducting performance analysis on software architectures: methodologies?</summary>

### Conducting performance analysis on software architectures: methodologies?

**Definition:**

The sourced methods are an ATAM scenario analysis aimed at a performance goal, a production canary that ramps load and watches metrics, and a trace that records where the time went. PostgreSQL adds the replication tradeoff you measure when the architecture copies a database.

**How it works:**

In ATAM step 6, an approach aimed at a performance goal is subjected to a performance analysis, and that analysis identifies risks, sensitivity points, and tradeoff points. The scenarios carry stimuli and responses. A canary is the production method Sato describes: route a subset, increase load, and capture how the new version affects the environment, with rollback if it regresses. He separates that from an A/B test, which waits longer for statistical significance. Tracing records start and end times of each operation on one request so the latency source is a specific hop, not only the overall response time. Application metrics aggregate those operation statistics for alerts. PostgreSQL says a fully synchronous replication design over a slow network might cut performance by more than half, while asynchronous replication might have minimal impact and can return stale reads. That is the measurement the replication choice requires.

**When to use:**

Use ATAM scenarios when the system is not built and stakeholders must see the performance tradeoff. Use a canary when the version is ready for a slice of production. Use tracing when you already have traffic and need the slow operation. Measure sync versus async when the database standby is on a slow network.

**Sources:**

- https://www.sei.cmu.edu/library/architecture-tradeoff-analysis-method-collection/
- https://martinfowler.com/bliki/CanaryRelease.html
- https://microservices.io/patterns/observability/distributed-tracing.html
- https://microservices.io/patterns/observability/application-metrics.html
- https://www.postgresql.org/docs/current/high-availability.html
</details>

<details>
<summary>Define a risk-driven architectural approach and its application.</summary>

### Define a risk-driven architectural approach and its application.

**Definition:**

The SEI's ATAM is a risk-driven evaluation: it exists to expose architectural risks that can stop the organization reaching its business goals, and to show the tradeoffs among quality attributes.

**How it works:**

Business drivers and the architecture are turned into scenarios. Analysis of the decisions behind those scenarios produces risks, non-risks, sensitivity points, and tradeoff points. Risks are then grouped into themes, each tied to a business driver it threatens. The group includes architects and the stakeholders who pull the design in different directions. Findings are presented back to them. SEI lists early identification of risks as a benefit, along with a documented basis for the decisions. Nygard's ADR is the record you keep after a risk is accepted or rejected: context, the decision, and all consequences, including the negative ones. A reversed decision is marked superseded rather than forgotten. The 1998 SEI paper's public abstract calls ATAM a spiral of candidate architectures, analysis, and risk mitigation. This collection page is the one that states the nine steps and the risk outputs used here.

**When to use:**

Apply ATAM when a quality goal such as performance, security, or modifiability could fail because of an architectural choice, and you need the risks named before the system is fully built. Write an ADR for the decision that follows, so the risk and the response stay attached.

**Sources:**

- https://www.sei.cmu.edu/library/architecture-tradeoff-analysis-method-collection/
- https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions
</details>

### Emerging Technologies and Future Trends

<details>
<summary>What role does AI play in modern software architecture?</summary>

### What role does AI play in modern software architecture?

**Definition:**

The fetched pages place machine learning as a consumer of a data lake, as a companion to edge decisions, and as a reason for an isolated compute environment. They do not define a single AI architectural style.

**How it works:**

AWS's data-lake page says a lake supports analytics from dashboards to big data processing, real-time analytics, and machine learning, without moving the data to a separate analytics system. Models are built to forecast outcomes. The tools named for processing include Apache Spark. Edge computing can be teamed with AI and machine learning tools to derive insight from data collected at the source. Energy operators, in that page's example, deploy edge artificial intelligence to detect hazards. Lambda functions suit event-driven processing. Lambda MicroVMs are the primitive AWS describes for a dedicated environment per user or job, including persistent environments running user or AI-produced untrusted code, with a lifespan up to 8 hours and state kept across suspend and resume. That is a different isolation model from a stateless function.

**When to use:**

Land varied data in a lake when models need historical and unstructured inputs in place. Run inference next to the device when the decision cannot wait on a data center. Use a MicroVM when the code being run is untrusted and needs its own environment. Use a function when the work is a short, stateless handler.

**Sources:**

- https://aws.amazon.com/what-is/data-lake/
- https://aws.amazon.com/what-is/edge-computing/
- https://docs.aws.amazon.com/lambda/latest/dg/welcome.html
</details>

<details>
<summary>How can blockchain technology be integrated into software architectures?</summary>

### How can blockchain technology be integrated into software architectures?

**Definition:**

AWS defines blockchain as an advanced database mechanism that lets a business network share information transparently. Data is stored in blocks linked in a chain. You cannot delete or modify the chain without consensus from the network, so the ledger of orders, payments, accounts, and other transactions is immutable.

**How it works:**

AWS contrasts this with each party keeping its own record of a sale, which the other party cannot trust, and with a central authority that validates the transaction and becomes a single point of vulnerability. A blockchain ledger is decentralized. In the property example, buyer and seller each have a ledger, both must approve a transaction, and both ledgers update. Corrupting a historical transaction corrupts the chain. Built-in mechanisms reject unauthorized entries and keep a shared view consistent. AWS says this has been used to create digital currency such as Bitcoin, and that Bitcoin is not the same thing as the database mechanism. Industries on the page include peer-to-peer energy trading. The page also distinguishes the chain from an ordinary database and from the cloud, and names Blockchain as a Service as a way to run it. Integration in an application is the choice to record a class of transactions on that shared ledger instead of only in one party's database.

**When to use:**

Use a chain when multiple parties must share one history and none of them should be the sole writer who can alter it. Keep ordinary application state in a database when a single operator is the trusted writer and consensus across organizations is not the requirement.

**Sources:**

- https://aws.amazon.com/what-is/blockchain/
</details>

<details>
<summary>Potential impact of quantum computing on future architectures?</summary>

### Potential impact of quantum computing on future architectures?

**Definition:**

IBM defines quantum computing as an emergent field that uses properties of quantum mechanics to solve problems beyond the ability of even the most powerful classical computers, or beyond the speed at which those computers can solve them.

**How it works:**

A quantum computer replaces binary bit circuits with qubits, particles whose behavior is described by quantum mechanics rather than by the everyday bit. IBM says large-scale machines would tackle certain complex problems many times faster, in minutes or hours where a conventional machine would take millennia. The two expected uses are modeling physical systems, which matters for chemistry and materials, and finding patterns in information with mathematics classical algorithms do not have, with examples in biology such as protein folding and in finance. The page says the technology is still in development, that much of the work is searching for algorithms, and that building the machines is a parallel effort. It names IBM, Amazon, Microsoft, Google, Rigetti, and IonQ as investors. It does not specify a wire protocol, a data-center layout, or a change to ordinary request routing.

**When to use:**

Treat quantum hardware as a specialized processor for those two classes of problem, not as a replacement for the stateless services, gateways, and databases in the rest of this file. Keep the classical architecture for request handling until a problem is one IBM places in physical simulation or in pattern finding that classical machines cannot finish in time.

**Sources:**

- https://www.ibm.com/think/topics/quantum-computing
</details>

<details>
<summary>Architectural changes to support AR and VR applications?</summary>

### Architectural changes to support AR and VR applications?

**Definition:**

AWS places virtual reality with downstream edge computing: the priority is delivering data to the user with low enough latency that events are experienced as they happen. Live video and online games are the neighboring examples on the same page.

**How it works:**

Downstream edge is the opposite of shipping every byte from a central data center. A CDN caches content on edge servers closer to the user. Cloud edge services run the latency-sensitive part of the application local to the endpoints in a geography. Mobile edge computing uses 5G networks and 5G-based mobile cloud services to deploy applications that need ultra-low latency. Riot's example on the page is operational rather than a VR headset: AWS Outposts placed game servers so latency fell by 10 to 20 milliseconds. WebSocket, on MDN, is the browser session for two-way messages without polling, which is the communication shape a live view needs when the server must push. The pages do not specify a rendering pipeline or a tracking SDK.

**When to use:**

Put the latency-sensitive part of an AR or VR feed at an edge close to the user. Cache what can be cached. Use a two-way session when the server must push updates instead of waiting for the client to poll. Measure the latency, as the game example does, rather than assuming the central origin is close enough.

**Sources:**

- https://aws.amazon.com/what-is/edge-computing/
- https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API
</details>

<details>
<summary>Discuss 5G technology and its effect on software architectures.</summary>

### Discuss 5G technology and its effect on software architectures.

**Definition:**

AWS describes 5G as infrastructure for mobile edge computing: networks and 5G-based mobile cloud services used to develop, deploy, and scale applications that need ultra-low latency.

**How it works:**

The page groups this with downstream delivery, next to CDN caching and cloud edge services that run latency-sensitive work near the user. The effect on architecture is where the work runs. A central data center remains the place for what is not latency-sensitive. The ultra-low-latency part is deployed on the mobile edge. AWS says its edge offering extends managed infrastructure outside its own data centers, including onto 5G networks, on-premises sites, and disconnected locations. The page does not publish radio parameters or a required application protocol.

**When to use:**

Split an application so the part that cannot tolerate a round trip to a regional data center runs on the mobile edge that 5G makes available. Leave bulk and durable processing where the rest of the architecture already puts it.

**Sources:**

- https://aws.amazon.com/what-is/edge-computing/
</details>

### Collaboration and Team Dynamics

<details>
<summary>How do you communicate architecture decisions to non-technical stakeholders?</summary>

### How do you communicate architecture decisions to non-technical stakeholders?

**Definition:**

Nygard writes each architecture decision record as a short conversation with a future reader, in full sentences, so stakeholders who are not in the code can still see why a decision was made. ATAM puts those stakeholders in the room while the decisions are evaluated.

**How it works:**

An ADR is one or two pages: title, context, decision, status, and consequences. Context is value-neutral facts. The decision is active voice, "We will …". Status is proposed until stakeholders agree, then accepted. Consequences include the negative ones. Bullets are only visual. Nygard notes the objection that project managers and client stakeholders do not live in version control. His practice is a link to the file on GitHub, which renders the Markdown. ATAM step 2 has a project spokesperson, ideally the project manager or the customer, present the business drivers. Step 7 has the whole stakeholder group brainstorm and vote on scenarios. Step 9 presents risks, tradeoffs, and the utility tree back to that group. SEI lists increased communication among stakeholders as a benefit.

**When to use:**

Write the ADR in sentences a stakeholder can read, and send the rendered link rather than asking them to learn the repository layout. Use an ATAM when the communication is a facilitated evaluation, not only a document.

**Sources:**

- https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions
- https://www.sei.cmu.edu/library/architecture-tradeoff-analysis-method-collection/
</details>

<details>
<summary>How do you define the architect’s role within an agile development team?</summary>

### How do you define the architect’s role within an agile development team?

**Definition:**

Nygard says architecture on an agile project is not a single up-front document. Decisions are made over time, and the architect's durable output is a small record of each significant one. SEI adds that the architect presents how the architecture meets the business drivers and helps analyze it with stakeholders.

**How it works:**

Agile methods, in Nygard's note, are not opposed to documentation. They are opposed to documents nobody updates. Large specifications lose. An ADR of one or two pages, stored with the code, is what a rotating team can read. His experience report says six to ten developers rotated through projects using ADRs and valued the context. The architect's record covers decisions that affect structure, non-functional characteristics, dependencies, interfaces, or construction techniques. In ATAM the architect presents the architecture against the drivers a project spokesperson stated, identifies approaches, and joins the analysis that produces risks and tradeoffs. SEI says the architect is a primary beneficiary because every other stakeholder is pulling the design toward a different goal.

**When to use:**

Keep the architect's agile deliverable as the next ADR, not as a book written before the project starts. Join the ATAM when the team needs the tradeoffs named with the people who hold the business drivers.

**Sources:**

- https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions
- https://www.sei.cmu.edu/library/architecture-tradeoff-analysis-method-collection/
</details>

<details>
<summary>How do you handle conflicting architectural decisions among team members?</summary>

### How do you handle conflicting architectural decisions among team members?

**Definition:**

Nygard handles a conflict that reverses an earlier decision by keeping both records: the old one is marked superseded and points at the new one. ATAM handles a conflict among stakeholder goals by turning it into scenarios, a vote, and an explicit tradeoff.

**How it works:**

Changing a decision without its motivation can damage a requirement that has not been tested. Accepting it forever is just as wrong when the context has changed. The ADR status stays proposed until stakeholders agree. After agreement it is accepted. A later record that reverses it marks the old one deprecated or superseded. Numbers are not reused, so the history stays. ATAM step 7 elicits scenarios from the whole group and prioritizes them by vote. Step 6 and step 8 then show the tradeoff points, where improving one quality attribute worsens another. The findings go back to the same group. SEI's stakeholder list is the source of the conflict: users want function, maintainers want modifiability, management wants a system the current team can build, and the customer wants schedule and budget.

**When to use:**

Write the disagreement down as context and consequences, and do not delete the losing decision. Use the ATAM vote and tradeoff list when the conflict is between quality goals held by different stakeholders, not only between two implementers.

**Sources:**

- https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions
- https://www.sei.cmu.edu/library/architecture-tradeoff-analysis-method-collection/
</details>

<details>
<summary>What is the importance and usage of architecture decision records (ADRs)?</summary>

### What is the importance and usage of architecture decision records (ADRs)?

**Definition:**

Michael Nygard defines an architecture decision record as a short file for one architecturally significant decision: something that affects structure, non-functional characteristics, dependencies, interfaces, or construction techniques.

**How it works:**

He keeps the records in the repository at `doc/arch/adr-NNN.md`, numbered sequentially, numbers never reused. Each record has a short noun-phrase title, a value-neutral Context of the forces, a Decision in active voice ("We will …"), a Status (`proposed`, `accepted`, or `deprecated` / `superseded` with a pointer to the replacement), and Consequences that include the negative ones. A reversed decision stays in the tree and is marked superseded. The document is about one or two pages, written as a conversation with a future developer. Large specification documents, he argues, are not kept up to date and are not read; the point of an ADR is that a newcomer can see why a decision was made instead of accepting it blindly or reversing it blindly.

**When to use:**

Write one when the decision will change how the rest of the project is built. Nygard notes the consequences of one ADR become context for the next.

**Examples:**

```text
# ADR 1: Deployment on Ruby on Rails 3.0.10
## Status
Accepted
## Context
...
## Decision
We will ...
## Consequences
...
```

**Sources:**

- [Michael Nygard — Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions)

</details>

<details>
<summary>How do you ensure team-wide comprehension and adherence to the defined software architecture?</summary>

### How do you ensure team-wide comprehension and adherence to the defined software architecture?

**Definition:**

Nygard makes the architecture comprehensible by keeping each significant decision in a short record the whole team can read, including people who join later. Adherence is the status of that record: proposed until the stakeholders agree, accepted after they do, and superseded when a later record replaces it.

**How it works:**

A newcomer who cannot see the rationale will either accept every decision blindly or change one blindly. The ADR is written in full sentences, one or two pages, in the repository at `doc/arch/adr-NNN.md`, numbered and never renumbered. Context, decision, and consequences, including negative ones, are the parts a reader needs. Nygard's rotating developers said the records gave them context. GitHub links make the same file readable for people who do not work in the repository all day. ATAM's evaluation adds a shared pass over the architecture: stakeholders vote on scenarios, and the team leaves with risks, non-risks, and tradeoffs written down. SEI lists improved documentation and a documented basis for decisions among the results. A fitness function is the automated form of adherence Thoughtworks describes: a continual check, built from tests, metrics, or monitors, that a characteristic still holds after later changes.

**When to use:**

Require an ADR before a significant structural or interface decision is treated as accepted. Point new people at the records instead of at an oral history. Add a fitness function when adherence can be executed, and an ATAM when the team needs a shared evaluation of the tradeoffs.

**Sources:**

- https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions
- https://www.sei.cmu.edu/library/architecture-tradeoff-analysis-method-collection/
- https://www.thoughtworks.com/radar/techniques/architectural-fitness-function
</details>
