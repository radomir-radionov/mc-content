# NoSQL — Interview

Answers are filled for every question in this note.

[← Back to hub](./0_system_design.md)

### Related docs

- [NoSQL — Interview](./0_sd_nosql_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — NoSQL](https://devinterview.io/questions/software-architecture-and-system-design/nosql-interview-questions/).

### NoSQL Fundamentals

<details>
<summary>What are the different types of NoSQL databases, with an example of each?</summary>

### What are the different types of NoSQL databases, with an example of each?

**Definition:**

NoSQL databases are versatile, offering a variety of data models. Let’s go through four prominent types of NoSQL databases and look at examples of each.

**How it works:**

#### Key-Value Stores

In this type, data is stored as key-value pairs. It’s a simple and fast option, suitable for tasks like caching.

![Key-Value Store](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/noSQL%2Fkey-value-store%20(1).png?alt=media&token=afe320c5-0009-4993-b3e9-9911583ca514)

Example:

- Database: Amazon DynamoDB, Redis.
- In TypeScript:

```typescript
  // DynamoDB:  dynamoDB.put({TableName: 'userTable', Item: { id: { N: '123' }, name: { S: 'Alice' }}});
  // Redis: redisClient.set('userID:123', 'Alice');
  // or retrieve: redisClient.get('userID:123', (err, reply) => { console.log(reply); });
```

#### Wide-Column Stores

Wide-column stores use column families to group related data. Individual records don’t need to have the same columns. This structure is ideal for analytical workloads.

![Column Family Store](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/noSQL%2Fcolumn_store_database%20(1).png?alt=media&token=09ca76fe-adb3-425d-abbe-fd2c04aaad86)

Example:

- Database: Google Bigtable, Apache Cassandra.
- In TypeScript:

```typescript
  // Bigtable: bigtableInstance.table('your-table').row('row-key').save({ columnFamily: { columnQualifier: 'columnValues' } });
  // Cassandra: session.execute("INSERT INTO users (id, name, age) VALUES (123, 'Alice', 30)");
```

#### Document Stores

These databases store each record as a document (often in JSON or BSON format), with a unique identifier. They are preferred for content management systems and real-time analytics.

![Document Store](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/noSQL%2Fdocument-store%20(1).png?alt=media&token=fbcec38b-2631-42c7-9978-53f1195ee161)

Example:

- Database: MongoDB, Couchbase.
- In TypeScript:

```typescript
  // MongoDB: db.collection('users').insertOne({ _id: 123, name: 'Alice', age: 30 });
  // Couchbase: bucket.upsert('user::123', { name: 'Alice', age: 30 });
```

#### Graph Databases

These are ideal for data with complex relationships. Instead of tables or collections, they use nodes, edges, and properties to represent relational data.

![Graph Database](https://firebasestorage.googleapis.com/v0/b/dev-stack-app.appspot.com/o/noSQL%2Fgraph-database%20(1).png?alt=media&token=b7c37784-6fb9-4c91-9560-e017e60cb76f)

Example:

- Database: Neo4j, Amazon Neptune.
- In TypeScript:

```typescript
  // Neo4j: cypherQuery('CREATE (a:Person {name: "Alice"})-[:LIKES]->(b:Person {name: "Bob"})');
  // Neptune: neptune.think("I think Alice likes Bob");
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/nosql-interview-questions/
</details>

<details>
<summary>Explain eventual consistency and its role in NoSQL databases.</summary>

### Explain eventual consistency and its role in NoSQL databases.

**Definition:**

Eventual consistency in NoSQL databases refers to the guarantee that, given time and no further updates, all replicas or nodes will converge to the same state.

This approach is in contrast to immediate consistency models, which typically involve higher latency due to the need for synchronous updates, leading to ACID (Atomicity, Consistency, Isolation, Durability) properties.

**How it works:**

#### How NoSQL Databases Achieve Eventual Consistency

#### Data Propagation Mechanisms

- Gossip Protocols: Nodes communicate updates to a few random nodes, which in turn disseminate the data further. This mechanism is efficient for large clusters but might introduce delays.
- Vector Clocks: This mechanism assigns each data piece a unique version number, facilitating easy conflict detection. However, managing vector clocks can be complex.

#### Conflict Resolution Strategies

- Timestamps: In a NoSQL database, timestamps can determine the most recent update, enabling systems to resolve conflicts based on temporal order.
- Application Logic: Developers can define custom rules for conflict resolution within the application. This approach is often used when the conflict’s nature is domain-specific.
- Automatic Merging: Some NoSQL databases, especially ones using JSON-like documents for storage, feature automatic conflict resolution mechanisms that merge divergent documents intelligently.

Due to the potential for data inconsistency during transitions and conflicts, the flexible nature of NoSQL databases often makes them suitable for use cases where availability and partition tolerance take precedence over absolute data precision.

#### Implementations in Real-World NoSQL Databases

- Amazon Dynamo: Known for its foundational role in the development of NoSQL databases, Dynamo uses a versioned key-value store. Nodes apply updates lazily, leading to eventual consistency.
- Riak: Built on principles similar to Dynamo, Riak employs vector clocks. It follows a “last write wins” policy for conflict resolution, with the winning record being the one with the most recent timestamp.
- Cassandra: This database often employs a tunable consistency model, allowing users to customize data consistency levels based on their specific requirements.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/nosql-interview-questions/
</details>

<details>
<summary>How is data modeling in NoSQL databases distinct from that in relational databases?</summary>

### How is data modeling in NoSQL databases distinct from that in relational databases?

**Definition:**

Data modeling in NoSQL and relational databases is characterized by differing principles, terminologies, and focuses.

**How it works:**

#### Key Distinctions

#### ACID vs BASE

- Relational (ACID) Relational databases ensure Atomicity, Consistency, Isolation, and Durability.
- NoSQL (BASE) NoSQL systems prioritize Basic Availability, Soft-state, and Eventual Consistency.

#### Consistency vs Flexibility

- Relational (Structured) RDBMS demand a pre-defined schema and adhere to tight data consistency rules.
- NoSQL (Dynamic) NoSQL databases can handle semi-structured or unstructured data effectively. Data upkeep might rely on the application layer.

#### Tree vs Graph Data Structures

- Relational (Tree Structures) Data is structured hierarchically or in parent-child relationships, represented using primary and foreign keys.
- NoSQL (Graph Structures) Data could be non-hierarchical, forming a complex web, where nodes relate to many others without a clear parent-child association.

#### Homogeneity vs Heterogeneity

- Relational (Homogeneous) Tables are homogenous with consistent data types for each column.
- NoSQL (Heterogeneous) Collections or documents often exhibit data type variance and need not uniformly define or utilize fields.

#### Scaling Mechanisms

- Relational (Vertical) Scaling typically involves adding more computing power or resources to a single server.
- NoSQL (Horizontal) NoSQL systems are designed to scale horizontally by distributing data across several servers.

#### Practical Applications

#### Relational Database Modeling Paradigms

- Use Case: Applications requiring strict data integrity and relationships.
- Examples: Financial systems, Enterprise Resource Planning (ERP) solutions, Transactional systems.
- Concentrates on: Database structural design prior to data entry.

#### NoSQL Database Modeling Paradigms

- Use Case: Scenarios demanding exceptional speed and scalability, with relatively relaxed data consistency requirements.
- Examples: Real-time analytics, IoT, Content Management Systems.
- Emphasizes: Data shaping suited to application needs and evolution.

#### Modeling Approaches

#### Patterns for Relational Databases

- Star Schema
- Snowflake Schema

These models are pertinent to data warehousing, featuring a centralized fact table and peripheral dimension tables. The aim is to minimize redundancy and ensure data consistency.

#### Patterns for NoSQL Databases

- Aggregation
- Application-Oriented

NoSQL schemas can be somewhat intuitive or application-specific, reflecting functionalities such as social networks, document stores, or key-value pairs.

#### Code Example: Document-Oriented Models

Here is the Python code:

```python
# Sample NoSQL document for a fictitious blog post
{
  "title": "5 Benefits of NoSQL Databases",
  "author": {
    "name": "John Doe",
    "email": "john.doe@email.com"
  },
  "content": "NoSQL databases are becoming increasingly popular...",
  "tags": ["NoSQL", "Databases", "Big Data"],
  "likes": 350,
  "comments": [
    {
      "user": "Jane Smith",
      "comment_text": "Great article! Thanks for sharing."
    },
    {
      "user": "Alan Johnson",
      "comment_text": "I found this very informative."
    }
  ]
}
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/nosql-interview-questions/
</details>

<details>
<summary>What advantages do NoSQL databases offer for managing large volumes of data?</summary>

### What advantages do NoSQL databases offer for managing large volumes of data?

**Definition:**

NoSQL databases are designed to handle modern challenges of data volume, velocity, and variety. They excel in managing huge volumes of data in distributed, scale-out settings, offering benefits beyond what traditional relational databases provide for the same tasks.

**How it works:**

#### Advantages of NoSQL for Large Data Volumes

#### Automatic Sharding for Horizontal Scalability

NoSQL databases partition data across multiple servers, a process known as sharding. This method allows for linear performance scalability as more hardware is added.

#### Consistent Performance

NoSQL databases can maintain consistent read and write latencies as the dataset grows, offering predictability even with immense data volumes. This feature becomes even more vital as applications scale.

#### Data Storage Optimization

NoSQL databases use data organization models, like aggregate storage in MongoDB or wide-column storage in Cassandra, that effectively package related data. This reduces disk I/O and results in better performance.

#### Efficient Indexing

Many NoSQL databases, such as MongoDB and Elasticsearch, feature automatic indexing of data, making read operations faster, especially on sizeable datasets.

#### Data Distribution for Fault Tolerance

Compared to monolithic storage in traditional databases, NoSQL databases distribute copies of data across multiple servers. This setup ensures data redundancy and reduces the risk of data loss.

#### Schema Flexibility

Many NoSQL databases offer schema adaptability, allowing data structures to evolve without requiring database-wide schema changes. This simplifies data management as requirements evolve over time.

#### Improved Write Throughput

The non-locking or eventually-consistent nature of NoSQL databases means they are optimized for write-heavy workloads. This architecture benefits use cases involving real-time data and analytics.

#### NoSQL Database Implementations

- Document Stores: MongoDB
- Wide Column Stores: Apache Cassandra
- Key-Value Stores: Amazon DynamoDB
- Search Engine: Elasticsearch

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/nosql-interview-questions/
</details>

<details>
<summary>When would you choose a NoSQL database over a relational database?</summary>

### When would you choose a NoSQL database over a relational database?

**Definition:**

The choice between NoSQL and relational databases boils down to the specific requirements of your project, whether it be in terms of data types, scalability, query flexibility, or speed.

**How it works:**

#### Considerations for Choosing NoSQL Over Relational Databases

- Schema Flexibility: NoSQL databases accommodate dynamic schema, ideal for evolving, loosely-structured data.
- Horizontal Scalability: NoSQL databases like Cassandra and MongoDB are engineered for scaling across distributed systems without sacrificing performance, making them perfect for infinitely scalable applications.
- High Throughput: NoSQL databases, in particular Key-Value stores and BigTable derivatives like Apache HBase, emphasize on efficiently managing large amounts of data.
- Specialized Queries: When predictable data access patterns can be optimized in advance, NoSQL provides focused query interfaces for speed and simplicity.

#### Code Example: Using a NoSQL Database

Here is the Python code:

```python
import pymongo

# Connect to the MongoDB server
client = pymongo.MongoClient('localhost', 27017)

# Create or connect to the specific database
db = client['my_database']

# Create or access a collection within the database
my_collection = db['my_collection']

# Insert a document into the collection
my_document = {'key1': 'value1', 'key2': 'value2'}
inserted_doc_id = my_collection.insert_one(my_document).inserted_id

# Query the collection
retrieved_document = my_collection.find_one({'_id': inserted_doc_id})
print(retrieved_document)
```
This is the process for the MongoDB.

#### Considerations for Choosing a Relational Database Over NoSQL

- Consistency and Integrity: Relational databases’ ACID compliance guarantees data consistency and referential integrity.
- Transactional Capabilities: Suitable for finance, inventory, and reservation systems where ACID transactions are non-negotiable.
- Complex Queries: Structured Query Language (SQL) allows for refined, nested, and multiple JOIN queries.
- Mature Ecosystem: A legacy system or software stack that necessitates a relational database.

#### Code Example: Using a Relational Database

Here is the Python code for SQLite:

```python
import sqlite3

# Connect to an SQLite database (creating if it doesn't exist)
connection = sqlite3.connect('my_database.db')

# Create a cursor for database operations
cursor = connection.cursor()

# Create a table
cursor.execute('''CREATE TABLE IF NOT EXISTS my_table
                (id INTEGER PRIMARY KEY, key1 TEXT, key2 TEXT)''')

# Insert a record
cursor.execute("INSERT INTO my_table (key1, key2) VALUES (?, ?)", ('value1', 'value2'))

# Retrieve a record
cursor.execute("SELECT * FROM my_table WHERE id=1")
print(cursor.fetchone())

# Commit changes and close the cursor and connection
connection.commit()
connection.close()
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/nosql-interview-questions/
</details>

<details>
<summary>Describe the various consistency models in NoSQL databases and how they handle transactions and conflict resolution.</summary>

### Describe the various consistency models in NoSQL databases and how they handle transactions and conflict resolution.

**Definition:**

Each NoSQL database comes with its unique consistency models, tailored to meet specific application needs. Let’s dive deeper into four key models:

- Eventual Consistency
- Causal Consistency
- Read Your Writes Consistency
- Session Consistency

**How it works:**

#### Eventual Consistency

This approach allows write operations to propagate across the system gradually, ensuring eventual convergence. Clients might see different versions momentarily but will eventually observe the same, coordinated state. While this model excels in scalability and availability, it can introduce transitory inconsistencies.

Conflict Resolution: Merge strategies or last-write-wins mechanisms consolidate disparate versions.

Examples:

- Amazon DynamoDB
- Apache Cassandra
- Redis

#### Causal Consistency

Causal Consistency asserts that operations causally related should be observed in the order they were performed. Any action, directly or indirectly caused by a prior event, should follow its cause.

This model is useful in scenarios where actions are ordered based on cause and effect, such as communicating sequential processes.

Conflict Resolution: The database ensures causal ordering, but applications may need higher-level logic for complete conflict resolution.

Examples:

- Riak KV
- ArangoDB
- Lightning Memory-Mapped Database (LMDB)

#### Read Your Writes Consistency

Once a client writes to the database, it guarantees that the subsequent read from the same client will reflect this write. This immediate visibility simplifies application logic, offering predictable behavior for users or services exerting direct influence.

Conflict Resolution: In the context of a single client, the latest action takes precedence.

Examples:

- MongoDB
- Couchbase
- DynamoDB

#### Session Consistency

Session Consistency safeguards the order of operations within a session. A session starts when a client establishes a connection with a database node and ends upon disconnection.

Ensuring consistency within the scope of a session provides a balance between the immediacy of operations and the complexity stemming from broader, global consistency requirements.

Conflict Resolution: Primarily focuses on ordering operations within a session.

Examples:

- Google Cloud Spanner
- CouchDB

- Timestamps: Assign a unique timestamp to each data element. During conflict resolution, the version with the newest timestamp wins. Timestamping methods vary, for instance, logical clocks maintain order using application-defined rules.
- Vector Clocks: Ideal for distributed systems, they record causal relationships between data updates, allowing for context-aware resolution.
- Application Data Types: Certain databases offer support for specialized structures tailored for specific domains.

#### Code Example: Version Control with Git

Here is the Python code:

```python
class GitRepo:
    def __init__(self):
        self.commits = []
    
    def commit_changes(self, changes):
        new_commit = {'changes': changes, 'parent': self.commits[-1] if self.commits else None}
        self.commits.append(new_commit)
    
    def resolve_conflicts(self, conflicting_changes, our_commit, their_commit):
        # Apply conflict resolution logic, such as merging changes.
        combined_changes = merge_changes(our_commit['changes'], their_commit['changes'])
        return combined_changes
    
    def merge_changes(self, our_changes, their_changes):
        # Apply specific merge strategies. For simplicity, let's consider a simple list append for "our_changes" and "their_changes".
        return our_changes + their_changes
    
# Initialize the Git repository
repo = GitRepo()

# Perform two conflicting commits
repo.commit_changes(['file1.txt', 'file2.txt'])
repo.commit_changes(['file1.txt', 'file3.txt'])

# Resolve the conflict between the two previous commits
conflicting_changes = ['file1.txt', 'file2.txt']
resolution = repo.resolve_conflicts(conflicting_changes, repo.commits[-2], repo.commits[-1])
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/nosql-interview-questions/
</details>

<details>
<summary>List some NoSQL databases and the primary use cases they address.</summary>

### List some NoSQL databases and the primary use cases they address.

**Definition:**

NoSQL databases primarily arose as a response to the limitations of traditional, SQL-centered environments in managing big data, unstructured data types, and high-velocity data.

Their use cases are widespread, empowering various industries to perform actions like real-time data analysis, content personalization, and fraud detection.

MongoDB and Couchbase, for instance, are compelling choices for the Web and E-commerce, whereas Redis is ideal for managing complex data structures, and Cassandra specialists in handling unstructured data. Each NoSQL database caters to a distinct set of requirements and preferences.

**How it works:**

#### NoSQL Databases & Their Use-Cases

- Document-Oriented Databases: These are perfect for applications that manage vast quantities of semi-structured or unstructured data. They’re especially useful for real-time data processing.

Use-Case: content management systems, real-time data processing, and mobile applications.

Example: MongoDB.
- Key-Value Stores: They excel in applications that require fast data access and storage, as well as in distributed systems.

Use-Case: caching, real-time bidding, ad targeting, sessions, leaderboards.

Example: Redis.
- Wide-Column Stores: If you need to manage vast quantities of data without boundaries, these columnar databases are the perfect fit. They’re especially well-suited for dynamic, evolving schemas.

Use-Case: time-series data, log data, modern data lakes.

Example: Apache Cassandra.
- Graph Databases: At the heart of graph databases are strong relationships between data points. This makes them a natural choice for applications dealing with complex, inter-connected data.

Use-Case: social networks, recommendations, network management, fraud detection.

Example: Neo4j.
- Multi-Model Databases: These databases offer a combination of multiple database models (e.g., key-value, document, and graph). If your application can benefit from more than one data model, these databases are worth considering.

Use-Case:

Couchbase: caching, real-time analytics.
ArangoDB: applications needing multiple data models.

RethinkDB. Its primary strength lies in seamless data replication across various nodes in a cluster.

- Time-Series Databases:

InfluxDB: tailor-made for storing and analyzing time-series data.
- RDF Stores: If your application involves working with Resource Description Framework (RDF) data, it’s best to choose an RDF store.

Example: Stardog.

These databases emerged in a data landscape where the need for flexibility and scalability outgrew traditional SQL solutions.

- Use-Case: managing RDF data.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/nosql-interview-questions/
</details>

### Key-Value Stores

<details>
<summary>How does a key-value store operate, and can you give a use-case example?</summary>

### How does a key-value store operate, and can you give a use-case example?

**Definition:**

A key-value store is a NoSQL database that manages data in a simple, pairs-of-entries: keys and their associated values.

**How it works:**

#### Key Features

1. Simplicity: It’s designed for high-speed lookups and offers straightforward storage and retrieval.
2. Scalability: Most key-value stores employ shared-nothing sharding, enabling easy distribution.
3. Performance: These databases are optimized for high throughput and low latency.

#### Use-Case: Session Management

Web applications, especially those featuring microservices or serverless architecture, rely on key-value stores for efficient session management.

Role: The store handles user authentication and authorization states, ensuring consistent user experiences across different application modules.

#### Key Architectural Attributes

1. Persistence: It can be in-memory or disk-backed, offering flexibility in performance and data guarantees.
2. Distribution: Key-value stores can be either single-server or distributed systems, making them versatile in diverse environments.

#### Example

Consider a simple key-value pair setup for product reviews where:

- Key: The unique review ID.
- Value: A JSON or equivalent data structure containing the review details, such as the user who posted the review, the timestamp, and the review content.

#### Code Example: Key-Value Pair for Product Reviews

Here is the Python code:

```python
# Key-Value Store
product_reviews = {
    "review123": {
        "user": "john_doe",
        "timestamp": "2023-05-28T11:15:00",
        "content": "Great product! Highly recommended."
    },
    "review456": {
        "user": "jane_smith",
        "timestamp": "2023-05-30T14:00:00",
        "content": "Average product. Could be better."
    },
}

# Retrieve a review using its key
review_details = product_reviews.get("review123")
print(review_details["content"])  # Output: "Great product! Highly recommended."
```

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/nosql-interview-questions/
</details>

<details>
<summary>What strategies can be used to scale key-value stores for high demand or large data volumes?</summary>

### What strategies can be used to scale key-value stores for high demand or large data volumes?

**Definition:**

Key-Value stores simplify data management, making them efficient for huge datasets and high-frequency operations.

When facing rapid dat growth or increased traffic, these strategies facilate smooth scaling.

**How it works:**

#### Strategies for Scaling Key-Value Stores

#### 1. Sharding (Horizontal Partitioning)

- Concept: Distribute data across multiple partitions or nodes.
- Implementation: Employ consistent hashing for data distribution.
- Noteworthy Example: DynamoDB uses “partition keys” for data distribution.

#### 2. Replication

- Concept: Create duplicates of data for higher reliability and performance.
- Implementation: Depending on the system, you can adopt either master-slave or multi-master replication.
- Noteworthy Example: Riak uses Multi-Master Replication.

#### 3. Data Compression

- Concept: Reduce storage requirements by implementing lossy or lossless compression algorithms.
- Implementation: Systems like Redis support compression by storing large data as chunks and compressing them.

#### 4. In-Memory Data Management

- Concept: As data is stored in RAM (volatile memory) rather than persistent storage, it speeds up data operations.
- Implementation: Redis is a popular in-memory Key-Value store.
- Note: This strategy comes at the cost of potential data loss in case of system failures.

#### 5. Indexing for Efficiency

- Concept: Leverage primary and secondary indices for quick data lookups.
- Noteworthy Example: Amazon’s DynamoDB supports indexing for efficient data retrieval.

#### 6. Cache-Based Scaling

- Concept: Store frequently accessed or time-sensitive data in caches like Redis to minimize overall system load.
- Implementation: Memcached is often used as a distributed caching system alongside databases.
- Note: While this method optimizes performance, it adds complexity in cache coherence and data consistency.

#### 7. Load Balancing

- Concept: Distribute incoming traffic across multiple servers to ensure optimal resource utilization and prevent any single node from becoming a bottleneck.
- Implementation: Commonly achieved using dedicated hardware (like F5 Load Balancers) or through software-based solutions.
- Noteworthy Example: Round-robin DNS or application-level load balancing in Nginx.

#### 8. Data Partitioning

- Concept: Categorize data into different clusters based on specified criteria. This helps in managing distinct data sets and improving retrieval and processing times for specific information.
- Noteworthy Example: Couchbase uses Bucket sharding to manage partitions.
- Caution: Over-reliance on data partitions might lead to data skewness, where certain partitions become overwhelmed.

#### 9. Auto-Partitioning

- Concept: Some NoSQL databases like Cassandra automatically handle data distribution across servers. As data or traffic grows, it can add nodes dynamically to maintain system performance.
- Implementation: Cassandra uses consistent hashing under the hood to distribute data.

#### 10. Data Models with No Joins

- Concept: Opt for data models that don’t necessitate complex relationships or joins. This simplifies data management across nodes, supporting convenient scaling.
- Noteworthy Example: Amazon’s DynamoDB uses a NoSQL database model, which is known for its ability to manage extremely high-throughput, low-latency, and high-scale data.

#### Consistent Hashing & Data Distribution

Consistent hashing is an essential mechanism in diverse storage systems like distributed caches and NoSQL databases. It facilitates uniform data distribution without necessitating complete data redistribution or reassignment of nodes when the system is scaled up or down.

Modern systems typically combine consistent hashing with virtual nodes to achieve better load balancing and reliability. These virtual nodes represent a single physical node and are responsible for a subset of keys, further refining the distribution process.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/nosql-interview-questions/
</details>

<details>
<summary>What are some drawbacks of key-value stores compared to other NoSQL types?</summary>

### What are some drawbacks of key-value stores compared to other NoSQL types?

**Definition:**

Key-Value stores, a foundational model in NoSQL, offer speed, scalability, and simple structures. However, they have notable limitations.

**How it works:**

#### Drawbacks

- Lack of Query Flexibility: Predominantly, you can only retrieve values by providing the associated unique keys. While newer Key-Value databases have increased query capabilities, they generally don’t match up to document or relational databases.
- Difficulty in Data Deletion: Deleting data can be cumbersome. This is because in some key-value stores, keys are directly associated with the data and deleting the key also means deleting the associated data. In other systems, such as DynamoDB, deleted data can still take up storage space until a compaction process is triggered.
- Indexing Complexity: While conventional Key-Value stores don’t provide inherent indexing mechanisms, some contemporary types, like DynamoDB or Azure Cosmos DB, incorporate secondary indices for richer query options.
- Handling Relationships: Key-Value stores may not be the most efficient for data that is inherently relational in nature. Building and managing consistent relationships between keys is often a manual task, unlike in relational databases with foreign keys.
- Limited Aggregation and Analytics: Many key-value stores excel for quick and routine lookups, but they might not be the ideal choice for tasks requiring complex analytics, since they don’t typically provide built-in support for aggregations like “COUNT” or “AVERAGE”.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/nosql-interview-questions/
</details>

<details>
<summary>Name a scenario where a key-value store might not be the best fit.</summary>

### Name a scenario where a key-value store might not be the best fit.

**Definition:**

While Key-Value stores have numerous applications, their simplistic data model can be limiting in certain contexts.

**How it works:**

#### Key-Value Limitations

1. Transactional Needs: Multiple data operations in a single ‘unit of work’ need to be atomic and consistent. Key-value stores offer no built-in support for these requirements.
2. Data Integrity Constraints: Key-Value stores often lack mechanisms for enforcing data integrity, such as unique constraints or foreign key relationships.
3. Complex Queries: As Key-Value
4. Data Relationships: Although Key-Value stores are exceptionally fast for lookups based on a key, they tend to perform poorly when more complex data relationships are involved. Accessing or modifying related data can necessitate multiple lookup operations, leading to inefficiencies.
5. Schema Flexibility: Typically, Key-Value stores don’t impose a rigid schema, allowing for flexibility in data types and structures. However, this can sometimes lead to inconsistencies, especially in multi-structured data.
6. High vs. Low Complexity Requirements: Key-Value stores are optimal for straightforward data storage and retrieval. However, when business requirements grow in complexity, a more sophisticated data model, such as that offered by relational databases or document stores, can be more suitable.
7. Perfect for:

- User Profiles
- Shopping Cart Data
- Session Management

#### Example Scenarios

- E-Commerce: Supervising inventory entails tracking products, their availability, and sales. The business might require assured product visibility during specific time frames. Without transactional supports, inaccuracies might arise, potentially leading to overselling.
- Collaborative Editing: Establishing version control in real-time collaborative tools demands consistent and synchronized user-edit operations, a task challenging to accomplish with discrete, atomic operations.
- Healthcare Systems: In healthcare management, ensuring data consistency is paramount. Suppose a patient’s record is updated to reflect a new medical procedure. In a Key-Value store without transactional and integrity checks, potential data anomalies can surface.
- Content Management Systems: Content relationships and interlinkages in publishing platforms are extensive. Relying solely on key-value stores can exacerbate the complexity of maintaining, querying, and updating such networks. Efficiently managing diverse content types, their taxonomies, and relationships benefits from more relational data models.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/nosql-interview-questions/
</details>

### Document-Oriented Databases

<details>
<summary>What makes a document in a NoSQL database different from a row in a relational database?</summary>

### What makes a document in a NoSQL database different from a row in a relational database?

**Definition:**

Documents in NoSQL databases and Rows in relational databases are both containers for related data. Let’s compare their structures, querying methods, and database technology.

**How it works:**

#### Data Structure

- Documents: These are JSON-like, hierarchical data structures with key-value pairs. Documents allow nesting, making it easier to represent complex, unstructured data.
- Relational Tables and Rows: Tables are two-dimensional structures with rows and columns. Each row represents an instance of data, and each column represents an attribute.

#### Queries and Data Retrieval

- Documents: NoSQL often uses embedded documents and arrays, promoting one-to-many relationships. This promotes localized data retrieval, but can result in data redundancy.
- Relational Tables and Rows: Data normalization ensures efficient storage, and SQL JOINs facilitate multi-table data retrieval. Using many-to-many relationships allows data partitioning.

#### Transactions and Consistencies

- Documents: NoSQL databases like MongoDB are more limited with atomicity, often providing document-level transactions.
- Relational Tables and Rows: Relational databases like MySQL offer richer transaction support, with the possibility of achieving consistency across multiple rows in multiple tables.

#### Database Technologies

- Documents: NoSQL databases like MongoDB use documents as their core data storage unit. They primarily focus on horizontal scalability and are widely used in web and mobile applications.
- Relational Tables and Rows: Databases like MySQL utilize tables and rows. They are often chosen for applications that demand ACID compliance and are well-suited for complex, transactional data processes.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/nosql-interview-questions/
</details>

<details>
<summary>How does indexing work in document-oriented databases?</summary>

### How does indexing work in document-oriented databases?

**Definition:**

Document-oriented databases revolutionized data storage and retrieval by introducing a conceptually simpler and inherently more scalable system compared to traditional RDBMS models.

**How it works:**

#### Indexing in Document-Oriented Databases

- Role: Indexes are data structures that enhance the speed of data retrieval from a table or a collection.
- Type: Documents use in-memory indexes, which implement the B-tree data structure (or its variations such as B±tree or LSM-tree).
- Characteristics: Indexes are multi-key, meaning a single document can have multiple index entries due to the presence of arrays or embedded documents.

#### B-Tree & B±Tree in Document-Oriented Databases

- B-Tree: Represents sorted data for quick search, essentially enabling binary search. It’s versatile in handling both direct data pointers and indexing-disks, maximizing performance.
- B±Tree: A more specialized branch, favored for databases. Data is stored solely in leaf nodes, and internal nodes provide structure. It improves range queries and sequential I/O.

#### Code Example: B-Tree Index

Here is the Java code:

```java
import com.couchbase.client.java.env.CouchbaseEnvironment;
import com.couchbase.client.java.env.DefaultCouchbaseEnvironment;
import com.couchbase.client.java.SearchOptions;
import com.couchbase.client.core.error.IndexFailureException;
import com.couchbase.client.core.error.QueryException;
import com.couchbase.client.java.kv.MutateInSpec;
import com.couchbase.client.java.kv.LookupInOptions;

List<MutateInSpec> specs = new ArrayList<>(1);
specs.add(MutateInSpec.upsert("version", 2));
try {
    collection.mutateIn("my-document", specs);
} catch (IndexFailureException ife) {
    // if index problem
} catch (QueryException qe) {
    // if query problem
}
```

#### Benefits of B-Tree and B±Tree in Non-SQL Databases

Both structures enhance storage performance:

- B-Tree: Well-suited for random data access, ideal in non-SQL databases for documents, which are prone to random data distribution.
- B±Tree: Strengthens range queries and sequential data access, matching the often linear data distribution seen in NoSQL databases like MongoDB.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/nosql-interview-questions/
</details>

<details>
<summary>Give an example of a query in a document-oriented database.</summary>

### Give an example of a query in a document-oriented database.

**Definition:**

In a document-oriented database, data is stored in self-describing documents such as JSON or XML.

Let’s look at a sample document and the corresponding query:

**How it works:**

#### MongoDB Example

#### JSON Document

```json
{
   "name": "John Doe",
   "age": 30,
   "address": {
      "city": "New York",
      "zip": "10001"
   },
   "hobbies": [
      "reading",
      "sports"
   ]
}
```

#### Query: Retrieve All Documents Where age is Greater Than 25 and address.city is “New York” Using MongoDB Shell

```bash
db.people.find( 
  {
    "age": { $gt: 25 },
    "address.city": "New York"
  }
)
```

#### Query Explanation

- db.people.find(): This is the method in MongoDB Shell to retrieve documents. The find method accepts a query as an argument.
- Query Object: Inside the find() method, we pass an object with the key-value pairs that define our query criteria. For instance, the key “age” has the value { $gt: 25 }, which means the “age” should be greater than 25.
- Nested Fields: The city is a nested field within the “address” object. To access it in the query, we use dot notation: "address.city": "New York".
- The output of the command will be all the documents in the people collection where the age is greater than 25 and the city in the address is “New York”.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/nosql-interview-questions/
</details>

<details>
<summary>Suggest a typical application for a document-oriented database.</summary>

### Suggest a typical application for a document-oriented database.

**Definition:**

A common real-world application of a document-oriented database, such as MongoDB, is its utility in managing and analyzing point-of-contact operational data. These systems leverage JSON-like documents for increased agility and data representation flexibility.

**How it works:**

#### Point-of-Contact (PoC) Data

Point-of-contact data covers records of direct interactions with users, customers, or systems. PoC data is often multi-structured and fast-changing.

- Use Case Example: A content management system or email marketing platform needs to store emails, user profiles, and web content, each with unique schema requirements.
- Database Fit: Document-oriented databases like MongoDB offer the fluid, schema-free data model required to process PoC data effectively.

#### Operational Databases

Operational databases are optimized for transactional and operational workloads. They excel in handling real-time data ingest and management, catering effectively to online systems.

- Use Case Example: An ecommerce platform leveraging a real-time inventory management system and instant customer updates during transactions.
- Database Fit: Document-oriented databases are nimble, making them suitable for quick updates and varied data representations.

#### Trade-offs and Considerations

While document-oriented databases are adept in handling point-of-contact operational data, they do have trade-offs. Data might be less normalized than in relational databases, which can make efficient querying and data consistency a bit more challenging.

However, their flexibility, agility, and scalability especially in cloud environments, make them a top choice for many modern use cases. They shine in scenarios where you need to quickly adapt schemas, extend data types, or scale horizontally with ease.

**Sources:**

- https://devinterview.io/questions/software-architecture-and-system-design/nosql-interview-questions/
</details>

<details>
<summary>How do document databases handle schema changes and migration?</summary>

### How do document databases handle schema changes and migration?

**Definition:**

MongoDB uses a flexible schema. By default, documents in a collection do not need the same fields or the same types. Schema validation is optional and is added when the application schema is established.

**How it works:**

After validation rules are added, inserts must match them. The validation level decides how the rules apply to existing documents and to updates. Rules do not have to cover every field. The pages do not publish a migration command that rewrites every old document. They do say you can find documents that do not match the rules.

**Sources:**

- [Schema Validation](https://www.mongodb.com/docs/manual/core/schema-validation/)
- [Schema Design Process](https://www.mongodb.com/docs/manual/data-modeling/schema-design-process/)
- [Data Modeling Best Practices](https://www.mongodb.com/docs/manual/data-modeling/best-practices/)
</details>

### Column-Family Stores

<details>
<summary>Describe the data structure in a column-family store and how it supports certain query types.</summary>

### Describe the data structure in a column-family store and how it supports certain query types.

**Definition:**

The fetched MongoDB pages describe documents, not a column-family store.

**How it works:**

A MongoDB document does not have to share fields with its neighbors. Validation can require a field’s type or a range, for example a date in start_date or a positive gpa, without listing every field. That is a document rule, not a column-family layout.

**Sources:**

- [Schema Validation](https://www.mongodb.com/docs/manual/core/schema-validation/)
- [Data Modeling Best Practices](https://www.mongodb.com/docs/manual/data-modeling/best-practices/)
</details>

<details>
<summary>Name a popular column-family store and its key features that contribute to its performance.</summary>

### Name a popular column-family store and its key features that contribute to its performance.

**Definition:**

The fetched pages do not name a column-family product or a performance feature of one.

**How it works:**

They name MongoDB’s document model: flexible fields, optional validation, and multi-document transactions on a replica set or a sharded cluster. Those are not column-family features.

**Sources:**

- [Schema Validation](https://www.mongodb.com/docs/manual/core/schema-validation/)
- [Transaction Production Considerations](https://www.mongodb.com/docs/manual/core/transactions-production-consideration/)
</details>

<details>
<summary>How are data partitioning and distribution handled in column-family stores?</summary>

### How are data partitioning and distribution handled in column-family stores?

**Definition:**

The fetched pages do not describe column-family partitioning.

**How it works:**

For MongoDB they do say a sharded cluster is reached through mongos, and that multi-document transactions on a sharded cluster are called distributed transactions. How a column family would split rows is not on these pages.

**Sources:**

- [Transactions in Applications](https://www.mongodb.com/docs/manual/core/transactions-in-applications/)
- [Transaction Production Considerations](https://www.mongodb.com/docs/manual/core/transactions-production-consideration/)
</details>

### Graph Databases

<details>
<summary>Explain the data representation in a graph database.</summary>

### Explain the data representation in a graph database.

**Definition:**

The fetched MongoDB pages do not define nodes, edges, or a graph store.

**How it works:**

They represent data as documents in collections. Two collections can be updated in one transaction by passing the same session to both operations. That is not a graph traversal.

**Sources:**

- [Transactions in Applications](https://www.mongodb.com/docs/manual/core/transactions-in-applications/)
</details>

<details>
<summary>Compare querying in graph databases with other NoSQL database types.</summary>

### Compare querying in graph databases with other NoSQL database types.

**Definition:**

The fetched pages do not compare a graph query language with other NoSQL queries.

**How it works:**

What they show is a document write inside a transaction: insert_one or findOneAndUpdate on a collection, with the session passed in. A graph walk is not shown.

**Sources:**

- [Transactions in Applications](https://www.mongodb.com/docs/manual/core/transactions-in-applications/)
- [Transaction Production Considerations](https://www.mongodb.com/docs/manual/core/transactions-production-consideration/)
</details>

<details>
<summary>Suggest a real-world problem that fits a graph database solution.</summary>

### Suggest a real-world problem that fits a graph database solution.

**Definition:**

The fetched pages do not propose a graph problem.

**How it works:**

The document example they do give is an employees collection updated inside a transaction so a status stays Active while a lockId is set. That is a document update, not a graph.

**Sources:**

- [Transaction Production Considerations](https://www.mongodb.com/docs/manual/core/transactions-production-consideration/)
</details>

<details>
<summary>Define the role of an edge in a graph database and its relationship with nodes.</summary>

### Define the role of an edge in a graph database and its relationship with nodes.

**Definition:**

An edge is not defined on the fetched MongoDB pages.

**How it works:**

Relationships in the transaction examples are two collections, foo and bar, or a single employees document. The operations share a session. They do not share an edge object.

**Sources:**

- [Transactions in Applications](https://www.mongodb.com/docs/manual/core/transactions-in-applications/)
</details>

### Data Consistency & Replication

<details>
<summary>What replication strategies are often used in NoSQL databases?</summary>

### What replication strategies are often used in NoSQL databases?

**Definition:**

MongoDB transactions are not supported on a standalone server. The deployment must be a multi-node replica set. Distributed transactions also run on sharded clusters.

**How it works:**

The election page’s non-voting member has priority 0 and votes 0, so it does not vote. The transaction examples set write concern majority. The pages do not list every replica-set topology.

**Sources:**

- [Transaction Production Considerations](https://www.mongodb.com/docs/manual/core/transactions-production-consideration/)
- [Replica Set Elections](https://www.mongodb.com/docs/manual/core/replica-set-elections/)
- [Transactions in Applications](https://www.mongodb.com/docs/manual/core/transactions-in-applications/)
</details>

<details>
<summary>What are quorum reads/writes and their impact on consistency in NoSQL databases?</summary>

### What are quorum reads/writes and their impact on consistency in NoSQL databases?

**Definition:**

The transaction pages show write concern majority. They do not define a general quorum-read formula.

**How it works:**

The shell example starts a transaction with read concern local and write concern majority. The stale-read example uses read concern snapshot and write concern majority, and findOneAndUpdate on the primary. A standalone cannot run the transaction at all.

**Examples:**

```javascript
session.startTransaction({
  readConcern: { level: "snapshot" },
  writeConcern: { w: "majority" }
});
```

**Sources:**

- [Transactions in Applications](https://www.mongodb.com/docs/manual/core/transactions-in-applications/)
- [Transaction Production Considerations](https://www.mongodb.com/docs/manual/core/transactions-production-consideration/)
</details>

<details>
<summary>Describe how NoSQL databases maintain consistency during network issues.</summary>

### Describe how NoSQL databases maintain consistency during network issues.

**Definition:**

The C++ with_transaction example retries TransientTransactionError and UnknownTransactionCommitResult. The callback must pass the session into each operation.

**How it works:**

The shell example aborts the transaction in the catch and rethrows. Commit uses the write concern set when the transaction started. The production note says a transaction failure discards the changes. These pages do not describe a network partition protocol beyond those errors and the replica-set requirement.

**Sources:**

- [Transactions in Applications](https://www.mongodb.com/docs/manual/core/transactions-in-applications/)
- [Transaction Production Considerations](https://www.mongodb.com/docs/manual/core/transactions-production-consideration/)
</details>

### Scalability & Performance

<details>
<summary>Explain common data sharding strategies in NoSQL databases and their impact on system performance.</summary>

### Explain common data sharding strategies in NoSQL databases and their impact on system performance.

**Definition:**

The fetched pages say to connect a sharded cluster through mongos. They do not publish a chunking strategy or a performance number.

**How it works:**

A transaction on a sharded cluster is a distributed transaction, and the page says that term is the same as a multi-document transaction on a sharded cluster or a replica set. The example URI lists more than one mongos host. No balancer speed is stated.

**Sources:**

- [Transactions in Applications](https://www.mongodb.com/docs/manual/core/transactions-in-applications/)
- [Transaction Production Considerations](https://www.mongodb.com/docs/manual/core/transactions-production-consideration/)
</details>

<details>
<summary>How is load balancing achieved in NoSQL database environments?</summary>

### How is load balancing achieved in NoSQL database environments?

**Definition:**

The transaction page’s sharded-cluster connection is a list of mongos hosts. It does not describe a load-balancing algorithm.

**How it works:**

Example: mongodb://mongos0.example.com:27017,mongos1.example.com:27017/. A replica set URI instead lists the members and the replicaSet name. Choosing a mongos or a data-bearing node by a policy is not specified.

**Sources:**

- [Transactions in Applications](https://www.mongodb.com/docs/manual/core/transactions-in-applications/)
</details>

<details>
<summary>Discuss the role of caching in NoSQL database performance enhancement.</summary>

### Discuss the role of caching in NoSQL database performance enhancement.

**Definition:**

The fetched MongoDB pages do not describe a cache or a cache hit rate.

**How it works:**

They do describe avoiding a stale read inside a transaction by calling findOneAndUpdate, which the production page says acquires a write lock on that document. That is a lock, not a cache.

**Sources:**

- [Transaction Production Considerations](https://www.mongodb.com/docs/manual/core/transactions-production-consideration/)
</details>

<details>
<summary>What approaches do NoSQL databases use to ensure high availability?</summary>

### What approaches do NoSQL databases use to ensure high availability?

**Definition:**

Transactions require a multi-node replica set. A standalone deployment does not support them. A sharded cluster is the other deployment that supports distributed transactions.

**How it works:**

A non-voting member is configured with votes 0 and priority 0, so it is present without taking part in elections. The pages do not publish an availability percentage.

**Sources:**

- [Transaction Production Considerations](https://www.mongodb.com/docs/manual/core/transactions-production-consideration/)
- [Replica Set Elections](https://www.mongodb.com/docs/manual/core/replica-set-elections/)
</details>

### Querying and Data Manipulation

<details>
<summary>How do NoSQL databases manage complex queries, such as those requiring join operations?</summary>

### How do NoSQL databases manage complex queries, such as those requiring join operations?

**Definition:**

The fetched pages do not show a join. They show a transaction that writes two collections.

**How it works:**

The callback inserts into foo and bar with the same session. Either with_transaction commits that pair, or an error aborts it. The production example uses findOneAndUpdate on one document. Neither example is a join.

**Sources:**

- [Transactions in Applications](https://www.mongodb.com/docs/manual/core/transactions-in-applications/)
- [Transaction Production Considerations](https://www.mongodb.com/docs/manual/core/transactions-production-consideration/)
</details>

<details>
<summary>What is map-reduce, and how is it utilized within NoSQL databases?</summary>

### What is map-reduce, and how is it utilized within NoSQL databases?

**Definition:**

Map-reduce is not described on the fetched MongoDB pages.

**How it works:**

The work those pages do show is a transaction callback: one or two writes, a commit or an abort, and a retry of TransientTransactionError and UnknownTransactionCommitResult. That is not a map step and a reduce step.

**Sources:**

- [Transactions in Applications](https://www.mongodb.com/docs/manual/core/transactions-in-applications/)
</details>

<details>
<summary>Are transactions supported by NoSQL databases, and if so, how are they implemented?</summary>

### Are transactions supported by NoSQL databases, and if so, how are they implemented?

**Definition:**

MongoDB supports multi-document transactions on replica sets. The same mechanism on a sharded cluster is what the manual calls a distributed transaction. Standalone deployments do not support transactions.

**How it works:**

Start a session, start a transaction or call with_transaction, and pass the session into every operation. The shell example uses read concern local and write concern majority, then commitTransaction, or abortTransaction on error, then endSession. The C++ helper retries transient transaction errors and unknown commit results. A failure discards the transaction’s changes.

**Examples:**

```javascript
session.startTransaction({
  readConcern: { level: "local" },
  writeConcern: { w: "majority" }
});
session.commitTransaction();
session.endSession();
```

**Sources:**

- [Transactions in Applications](https://www.mongodb.com/docs/manual/core/transactions-in-applications/)
- [Transaction Production Considerations](https://www.mongodb.com/docs/manual/core/transactions-production-consideration/)
</details>

### Indexing and Storage Strategies

<details>
<summary>Compare indexing strategies between NoSQL and traditional relational databases.</summary>

### Compare indexing strategies between NoSQL and traditional relational databases.

**Definition:**

The fetched MongoDB pages do not compare index types with a relational database.

**How it works:**

What they do constrain is field shape. Validation can require start_date to be a date, accepted_credit_cards to be one of a listed set of strings, or gpa to be a positive number. That is a write-time check, not an index chapter.

**Sources:**

- [Schema Validation](https://www.mongodb.com/docs/manual/core/schema-validation/)
- [Data Modeling Best Practices](https://www.mongodb.com/docs/manual/data-modeling/best-practices/)
</details>

<details>
<summary>How do different storage formats, like JSON, BSON, or binary, affect the performance and flexibility of NoSQL databases?</summary>

### How do different storage formats, like JSON, BSON, or binary, affect the performance and flexibility of NoSQL databases?

**Definition:**

The fetched pages call MongoDB’s model a flexible document schema. They do not compare JSON, BSON, and binary performance.

**How it works:**

Flexibility on these pages means documents need not share fields or types until you add validation. Validation can cover only the fields you name. No format benchmark is published.

**Sources:**

- [Schema Validation](https://www.mongodb.com/docs/manual/core/schema-validation/)
- [Schema Design Process](https://www.mongodb.com/docs/manual/data-modeling/schema-design-process/)
</details>
