# Redis — Interview

Every interview answer in this file is filled.


### Related docs

- [Redis — Interview](./17_redis_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Redis](https://devinterview.io/questions/web-and-mobile-development/redis-interview-questions/).

### Redis Fundamentals

<details>
<summary>What is Redis and what do you use it for?</summary>

### What is Redis and what do you use it for?

**Definition:**

**Redis** (Remote Dictionary Server) is an **in-memory key-value** data store renowned for its **performance** and **versatility**. It was developed in 2009 by Salvatore Sanfilippo, and it remains an influential tool in modern data management and caching.

**How it works:**

**Key Features**

- **Data Structures**: Redis goes beyond basic key-value storage to support various data structures, including **strings, lists, sets, sorted sets**, and **hashes**.
- **Persistence**: It offers both options: disk-based persistence and pure in-memory storage. This flexibility caters to use cases where durability and speed requirements differ.
- **Replication**: Redis allows you to create multiple replicas, ensuring high availability and data redundancy.
- **Clustering**: Redis can be set up in a clustered mode to distribute data across multiple nodes, ensuring scalability.
- **Pub/Sub Messaging**: It supports the publish-subscribe messaging pattern.
- **Atomic Operations**: Most of its data operations are atomic, giving you a reliable workflow.

**Fundamental Structures**

1. **Strings**: Key-value pairs that can hold text, integers, or binary data.
2. **Lists**: Ordered collections of strings, supporting operations at both ends.
3. **Sets**: Collections of unique, unordered strings, with built-in operations like union, intersection, and difference.
4. **Sorted Sets**: Like sets, but each element has a key (or score), allowing them to be sorted according to that score.
5. **Hashes**: Key-value pairs, essentially making a map inside a Redis key.

**Internal Architecture**

- **Event Loops**: It uses event-driven programming for performance, backed by its efficient C codebase.
- **Caching Strategy**: Redis employs the LRU (Least Recently Used) algorithm for cache expiration, but it allows for more nuanced strategies as well.

**Data Persistence**

Redis offers the following **persistence** options:

1. **RDB Snapshots**: Periodically saves an image of the dataset to disk.
2. **AOF (Append-Only File)**: Logs every write operation, ensuring durability and allowing for data reconstruction in case of server crashes.

It's relevant to save both in-memory data and historical data to either disk or an external server for redundancy.

**Built-in Replication**

With Redis, you can have multiple replicas (or slaves) of the primary Redis server (or master). This setup provides **data redundancy** and can also **boost read performance** by allowing clients to read from any reachable replica.

**Sharding and Clustering**

To **scale horizontally**, Redis can employ two approaches:

1. **Sharding**: Distributes data across multiple Redis instances using a client-side or server-side approach, but the responsibility of managing the shards lies with the user.
2. **Redis Cluster**: A built-in solution that provides automatic data partitioning across nodes while ensuring fault tolerance and data consistency.

For reliability and scalability in modern applications, it's advantageous to set up a Redis cluster.

**Multi-Threading Support**

Traditionally, Redis doesn't directly support multi-threading. However, **efforts are in progress** to add native support for this feature.

**Best Practices**

- **Data Segregation**: Use separate databases and instances for distinct data types or roles.
- **Error Handling**: Employ mechanisms to detect and recover from connectivity or server-related issues.
- **Backup Strategies**: Regularly back up persisted data and monitor backup tasks for consistent execution.

**Security Considerations**

- **VPCs and Firewalls**: Restrict access to Redis to specific IPs through firewall rules or VPCs.
- **TLS Encryption**: Use SSL/TLS to encrypt data in transit.
- **Access Control**: Set up authentication to deny unauthorized users access to Redis.

**Common Pitfalls**

- **Single Point of Failure**: Running Redis in a non-clustered mode can leave you vulnerable to complete data loss.
- **Persistence Lag**: In some setups, Redis might demonstrate a slight delay in persisting data to disk.
- **Memory Overload**: Without careful monitoring, Redis can consume too much memory and lead to performance issues or system crashes.

**When to use:**

**Common Use-Cases**

1. **Caching Layer**: Redis excels as a cache due to its in-memory nature and quick data retrieval, serving as a data source for web servers, databases, and more.
2. **Session Store**: It's used to manage user sessions in web applications, ensuring fast access and real-time updates.
3. **Queues**: Redis' lists and blocking operations make it a popular choice for message queues and task management systems.
4. **Real-Time Leaderboards and Counters**: The sorted set structure can help in maintaining ordered lists in real time, useful for leaderboards and rankings.
5. **Pub/Sub Communication**: Redis can facilitate real-time communication between components in your architecture through the publish-subscribe pattern.
6. **Geospatial Data**: It offers functions to handle geospatial data, making it suitable for applications that require location-based services.
7. **Analytics**: Its data structures and atomic operations can aid in real-time analytics and data processing.

**Sources:**

- [Devinterview.io — Redis](https://devinterview.io/questions/web-and-mobile-development/redis-interview-questions/)

</details>

<details>
<summary>How does Redis store data?</summary>

### How does Redis store data?

**Definition:**

**Redis**, an in-memory data store, ensures **lightning-fast** read and write operations by structuring data into types, and each type has unique capabilities.

Redis's data models are $key \rightarrow value$ pairs structured in memory as a set of possible data types:

- **String**: Binary safe strings where each character is stored using eight bits.
- **List**: Collection of ordered strings. Optimized for high-speed insertions and deletions.
- **Set**: Collection of unordered, distinct strings.
- **Sorted Set**: Similar to a set, but each element has a unique score for sorting.
- **Hash**: A map-like data structure with fields and values, both strings.
- **HyperLogLog**: Data structure for approximating the unique elements in a set.
- **Streams**: Append-only collection of `key-value` pairs.
- **Bitmaps**: Special type for bit manipulation.

**How it works:**

**Memory Management**

Redis implements strategies to efficiently manage memory:

- **Key and Encapsulation Metadata**: Each key occupies minimal space, exclusively for its representation. The value associated with the key deemphasizes encapsulation.
- **Memory Optimizations**: Utilizes algorithms that reduce data redundancy, such as sharing segments across keys with similar content.

**Persistence Mechanisms**

Redis provides two primary mechanisms for data persistence:

- **RDB (Redis Database)**: Periodic snapshots of the dataset.
- **AOF (Append-Only File)**: Logs every write operation, allowing for a full recovery of the dataset.

The system can utilize either of these methods, or both, for data safety.

**Parity with Functional Databases**

While **Redis** offers a real-time, in-memory operational presence, it mimics traditional databases' functionalities through disk persistence for fault tolerance and data recoverability.

**Performance and Scalability**

Persisting data on disk introduces an overhead that negatively affects both **write times** and the sheer number of writes the database can handle.

Redis, instead, focuses on optimizing in-memory operations for **low latencies** and high-throughput write workloads. This approach is especially advantageous in scenarios characterized by high-interactivity requirements or where durability is secondary to speed.

Redis also provides a reasonable level of reliability via a configurable durability setup, striking a balance between high speed and data safety.

**Sources:**

- [Devinterview.io — Redis](https://devinterview.io/questions/web-and-mobile-development/redis-interview-questions/)

</details>

<details>
<summary>What data types can you store in Redis?</summary>

### What data types can you store in Redis?

**Definition:**

**Redis**, being a data structure server, is optimized for various data types, offering diverse storage options.

**How it works:**

**Core Data Types**

**Strings**

- Ideal for simple keys and caching data.
- Examples: `username` or a JSON string

**Lists**

- Suitable for data that follows an order and may allow duplicates.
- Example: A queue of tasks.

**Sets**

- Efficient for unique, unordered datasets.
- Example: Unique visitors to a website.

**Sorted Sets**

- Similar to **Sets** but with each member inherently possessing a score, facilitating custom ordering and look-up.
- Example: Facilitating a leaderboard where the score is the user's rank.

**Hashes**

- Offers a map-like structure with key-value pairs, handy for storing and retrieving grouped data.
- Example: User details such as username, email, and status.

**HyperLogLogs**

- Allows for an estimation of the number of unique items within a set.
- Example: Counting unique IP addresses in a web server's access log.

**Bitmaps**

- Utilized best for scenarios that can be effectively modelled using bit arrays, often for tasks like tracking user activity over time.
- Example: User engagement tracking over specific days.

**Geospatial Indexing**

- Enables mapping locations to members in such a way that one can perform operations based on geographic distance.
- Example: Locating nearby places.

**Secondary Data Types**

**Streams**

- Offered since Redis 5.0, **Streams** are append-only collections.
- Notable for providing unique, manual acknowledgments.
- Example: Data logs with customizable, granular retention policies.

**Modules**

- **Redis Modules** greatly diversify Redis' core features, introducing a host of new data types with specialized functionalities.
- Examples:
- RediSearch: Offers powerful full-text search capabilities.
- ReJSON: Facilitates JSON manipulation, effectively adding a sophisticated JSON data type to Redis.

**Temporary Operations**

- Redis provides data structures, namely time series and probabilistic models, for specific, time-sensitive calculations and estimations.
- Example:
- Time Series is tailored for operations related to timestamped data.
- The probabilistic data structures, as the name suggests, provide estimations, albeit at the possible expense of absolute accuracy in some cases.

**Beyond Data Types**

Redis provides a flexible system that allows tailored functionalities and data behavior. The **Lua Scripting** and **Pub/Sub** mechanisms, for example, extend Redis' capabilities, paving the way for robust, custom behavior without straying from its core data types.

**Sources:**

- [Devinterview.io — Redis](https://devinterview.io/questions/web-and-mobile-development/redis-interview-questions/)

</details>

<details>
<summary>What is a Redis key and how should you structure it for best performance?</summary>

### What is a Redis key and how should you structure it for best performance?

**Definition:**

In Redis, a **key** serves as the primary identifier for data storage. Efficient key design is crucial for advanced performance.

**How it works:**

**Key Naming Conventions**

- **Keyspace Segmentation**: Categorize keys logically, such as by user. This practice optimizes operations like `DEL` or `KEYS` under a subset.
- **Consistent Naming**: Use a standardized format, e.g., "RESOURCE:ID".

**Key Length and Complexity**

- **Minimize**: Short and simple keys reduce memory and lookup time.
- **Avoid Repetition**: Using a consistent prefix reduces redundancy, but excessive repetition can be counterproductive.

**Data Encoding**

- Redis distinguishes between **direct** (explicit) and **indirect** (implicit) encodings. Explicit encoding is preferred for performance and clarity.

**Direct Encoding**

- **String Numbers**: Prefer storing numeric strings to optimize for integer values within a certain range.
- **Zip List**: Automatically encodes short lists or sets with specific value types (integers or strings).
- **Introspection**: Use `OBJECT ENCODING` if unsure about an encoding strategy.

**Indirect (RAW) Encoding**

- **Always RAW**: Guaranteed to use memory – suitable for larger or non-primitive types.

**Key Evolution and Deletion**

- **Evolution**: When updating keys is infeasible, append version identifiers to newer keys.
- **Deletion**: Ensure proper cleanup to avoid orphaned or obsolete keys.

**Examples:**

**Code Example: Key Naming Conventions**

Here is the Python code:

```python
def get_user_key(user_id, data_type):
    return f"USER:{user_id}:{data_type}"

def get_resource_key(resource_id):
    return f"RESOURCE:{resource_id}"

def delete_user_data(user_id):
    for key in r.keys(get_user_key(user_id, "*")):
        r.delete(key)
```

**Sources:**

- [Devinterview.io — Redis](https://devinterview.io/questions/web-and-mobile-development/redis-interview-questions/)

</details>

<details>
<summary>How do you set an expiration on Redis keys?</summary>

### How do you set an expiration on Redis keys?

**Definition:**

Setting an expiration time on **Redis keys** is a powerful feature that helps in key management. There are several mechanisms tailored for different types of keys. Let's explore these options.

**How it works:**

**Configuring Default Expiration**

**Redis** allows setting a default expiration time for keys. For example, to set a default expiration of 60 seconds:

**Redis CLI**

```sh
CONFIG SET  lazyfree-lazy-eviction-limit 30
config set -1
```

**Individual Key Expirations**

You can configure **Redis keys** to expire after a set duration.

**Using Commands**

- **Set a Key with Expiration**: Use the `EXPIRE` or `SETEX` commands for key-based expiration control.

Syntax:

```
redis> SET key value EX seconds
redis> SETEX key seconds value
redis> EXPIRE key seconds
```

Example:

```sh
redis> SET mykey redis
OK
redis> EXPIRE mykey 10  # Expires in 10 seconds
(integer) 1
```

- **Persistent Expiry with PSETEX**: To set a key with both a value and an expiration in a single step.

Syntax:

```
redis> PSETEX key milliseconds value
```

Example:

```sh
redis> PSETEX mykey 10000 redis  # Expires in 10 seconds (10000ms)
OK
```

**Querying Expiry Information**

- **Time to Live (TTL)**: Check the remaining time to live for a key.

Syntax:

```sh
redis> TTL mykey
```

Example:

```sh
redis> TTL mykey
(integer) 5
```

- **Persist/Remove Expiry**: Extend or remove the expiration of a key.

Syntax:

```sh
redis> PERSIST mykey
redis> PERSIST mykey  # To remove the key
```

**Fine-Tuned Key Expirations**

For selective or mass expiration handling, Redis provides specialized methods.

- **Scan, Delete, and Expire**: Use these commands in conjunction with a scan algorithm for extensive key management.

Commands:

- SCAN
- DEL
- UNLINK (introduced in Redis 4.0)
- EXPIRE
- PEXPIRE

- **Expiration Report**: Retrieve keys with a particular remaining time to live, which is often used in conjunction with the `TTL` command.

Command: `PTTL key`

- **Batch Expiry with Sorted Sets**: Utilize sorted sets to create distinct sets of keys with various expiration times. Then, process each category in batches.

- **Multiple-Step Approach with LUA Scripting**: This method follows a multi-step process to ensure orderly execution. It's particularly useful when the situation necessitates several steps to achieve the intended outcome, such as for complex operations.

**Sources:**

- [Devinterview.io — Redis](https://devinterview.io/questions/web-and-mobile-development/redis-interview-questions/)

</details>

<details>
<summary>What do the commands `SET` and `GET` do in Redis?</summary>

### What do the commands `SET` and `GET` do in Redis?

**Definition:**

`SET` and `GET` in **Redis** are fundamental key-value commands, each with distinct and complementary functionalities.

**How it works:**

**Core Functions**

- **`SET`**: Stores a value, either overwriting an existing key-value or creating a new one.
- **`GET`**: Retrieves the value associated with a given key. If the key doesn't exist, `GET` returns a null value.

**Additional `SET` and `GET` Directives**

**`SET`**

- **Options**:

  - `EX` or `PX`: Establishes an expiration, in seconds (`EX`) or milliseconds (`PX`), after which the key-value is automatically removed.
  - `NX` or `XX`: Dictates whether the command executes only if the key doesn't already exist (`NX`) or only if the key exists (`XX`).

- **Multi-SET Variants**:

  - `MSET`: Sets multiple key-value pairs simultaneously.
  - `MSETNX`: Sets multiple key-value pairs only if none of the keys already exist.

- **Memory Usage Control**:

  - `SET foo "Hello" EX 3600`: Sets an expiring key that will be removed after an hour.

**`GET`**

- **Data Transformation**:

  - If the value corresponding to the key **is an integer**, `GET` automatically converts it to an integer data type before returning it.
  - If you need the value to be returned as a string, use the command `GET foo`.

- **Multi-Key Operations**:

  - `MGET`: Retrieves the values of multiple keys in a single operation.

- **Performance Considerations**:

  - Big key `{KEY}` candidates may not be fully retrieved with `GET` or `MGET` due to their potential impact on Redis's performance.
  - It's generally better to retrieve each key after the decision has been made about which keys to retrieve.

- **Consistency and Atomicity**:

  - The `GET` and `SET` commands are atomic. Once a `SET` has happened, a subsequent `GET` will return the value in the state after the `SET` was performed.

**Scenario-Driven Best Practices**

- **Security Sensitive Data**:
  - Avoid using `GET` in sensitive data environments, as it can potentially expose key-values.

- **Memory Efficiency**:
  - Prefer `MSET` for multi-key operations; it's often more memory-efficient than `GET` or `MGET`.

- **Concurrent Operations**:
  - Use options like `NX` and `XX` with `SET` for safe concurrent insertions or updates when potential key existence or non-existence is known.

- **Expiry Management**:
  - Benefit from time-based expirations to automate key-value removals without additional housekeeping.

- **Performance Optimization**:
  - Be mindful of data transformation costs, especially when keys frequently hold integer values.

**Sources:**

- [Devinterview.io — Redis](https://devinterview.io/questions/web-and-mobile-development/redis-interview-questions/)

</details>

<details>
<summary>How does Redis handle data persistence?</summary>

### How does Redis handle data persistence?

**Definition:**

**Redis** generally prioritizes speed with **in-memory** data, using persistence methods for improved reliability and recovery.

**How it works:**

**Persistence Options**

1. **RDB (Snapshots)**:
   - Saves point-in-time snapshots.
   - Configuration method often **combined** with AOF for full durability.

    ```conf
    save 900 1         # Save every 15 minutes only if 1+ key changed
    save 300 10        # Save every 5 minutes only if 10+ keys changed
    ```

2. **AOF (Append-Only File)**:

   - Logs every write operation, ideal for **full durability**.
   - Can be set to **sync** after every command or periodically.

    ```conf
    appendonly yes          # Enable the AOF
    appendfsync everysec    # Sync AOF log every second
    ```

**Understanding RDB and AOF**

- **RDB Advantages**:
  - Simplifies recovery. Loads faster from a binary dump on restart.
  - Efficient for infrequently-changing datasets.

- **AOF Advantages**:
  - Best for ensuring every write is saved. Ideal for compliance and data integrity. May have a slight impact on performance.

**Combined Use for Optimal Performance**

- Employing **both** RDB and AOF offers the best of both worlds:
  - Quick recovery with RDB.
  - Assurance of write persistence from AOF.

This setup is quite common in production, offering both backup and full data integrity assurances.

**Best Practice**

- When using both RDB and AOF, it's essential to fine-tune their settings to achieve a balance between data integrity, performance, and the recovery mechanism they provide.
- Periodically test and validate your data persistence strategy.

**Sources:**

- [Devinterview.io — Redis](https://devinterview.io/questions/web-and-mobile-development/redis-interview-questions/)

</details>

<details>
<summary>Explain the difference between RDB and AOF persistence strategies in Redis.</summary>

### Explain the difference between RDB and AOF persistence strategies in Redis.

**Definition:**

In Redis, **RDB** and **AOF** are two persistence strategies aimed at ensuring data durability. **RDB** offers **point-in-time backups**, while **AOF** is focused on **command logging**.

**How it works:**

**RDB Persistence**

RDB, or Redis DataBase, is designed for **periodic backups**. It takes snapshots of data at specified intervals and saves them to disk, ensuring quick recoveries after unexpected events.

- **Backup Frequency**: Controlled by a configuration setting. Commonly set to save after a certain number of write operations.
- **Performance and Storage**: RDB is more performant and memory-efficient because it can batch multiple write operations before saving.
- **Recovery**: Can present loss of data on recovery to the last backup point.

**AOF Persistence**

AOF, or Append-Only File, aims to provide a comprehensive **command history** for Redis, making it easier to replay commands and restore the dataset to a particular point-in-time.

- **Backup Frequency**: Real-time. Each write operation is appended to the AOF file, ensuring that server restarts do not lead to data loss.
- **Recovery**: Ensures minimal data loss because even disconnected clients can re-synchronize their changes from the AOF log when they reconnect.

**Combined Persistence**

While Redis allows the choice of either RDB or AOF, it also supports **simultaneous persistence**. When both RDB and AOF are enabled, Redis can use the AOF file to recover a dataset beyond the last RDB snapshot.

Despite its advantages, using both strategies can require additional effort in terms of monitoring and management of the persistence components.

**Sources:**

- [Devinterview.io — Redis](https://devinterview.io/questions/web-and-mobile-development/redis-interview-questions/)

</details>

<details>
<summary>How would you implement a simple counter in Redis?</summary>

### How would you implement a simple counter in Redis?

**Definition:**

To implement a **simple counter** in Redis, you can use either **`INCR`** or **`HINCRBY`** commands, based on whether it's a standalone or hashmap-based counter, respectively.

**How it works:**

**Standalone Counter**

For a single key, you can use `INCR` for incrementing and getting the value.

**Redis Commands**

```plaintext
> SET myCounter 0                # Initialize the counter
OK
> INCR myCounter                # Increment the counter
(integer) 1
> GET myCounter                    # Retrieve the counter value
"1"
```

**Hashmap-Based Counter**

If you need multiple counters, you can use a Redis **hashmap** along with the `HINCRBY` command.

**Redis Commands**

```plaintext
> HSET myHash key1 0            # Initialize the counters within the hashmap
(integer) 1
> HINCRBY myHash key1 5        # Increment counter 'key1' by 5
(integer) 5
> HGET myHash key1             # Get value of counter 'key1'
"5"
```

**Examples:**

**Code Example: Standalone Counter**

Here is the Python code:

```python
import redis

# Connect to Redis
r = redis.StrictRedis(host='localhost', port=6379, db=0)

# Set up the counter
r.set('myCounter', 0)

# Increment and retrieve the counter
r.incr('myCounter')
print(r.get('myCounter'))
```

**Code Example: Hashmap-Based Counter**

Here is the Python code:

```python
import redis

# Connect to Redis
r = redis.StrictRedis(host='localhost', port=6379, db=0)

# Set up the hashmap with a counter
r.hset('myHash', 'counterKey', 0)

# Increment and retrieve the counter within the hashmap
r.hincrby('myHash', 'counterKey', 5)
print(r.hget('myHash', 'counterKey'))
```

**Sources:**

- [Devinterview.io — Redis](https://devinterview.io/questions/web-and-mobile-development/redis-interview-questions/)

</details>

<details>
<summary>What are hashes in Redis and how do you use them?</summary>

### What are hashes in Redis and how do you use them?

**Definition:**

**Redis** provides a powerful data structure known as a **hash**, which is essentially a map between strings and string values.

Stored as an **unordered** collection, hashes are exceptionally efficient for tasks requiring frequent **field-specific** operations, such as updating or retrieving specific values rather than the entirety of a dataset.

**How it works:**

**Why use Hashes?**

- **Simplicity**: Hashes offer a practical means of organizing related data.
- **Memory Efficiency**: Ideal when dealing with small data sets or fields that change frequently.
- **Performance**: Especially noteworthy for applications that demand fine-grained, field-specific operations and have large field counts within a key.

**Redis Use-Cases**

- **User Profiles**: Hashes curate various user attributes such as name, email, and date of birth.
- **Caching**: Instead of storing each user's article view count as a distinct key of a list, hash further segments the count based on users.

**Key Functions**

- **HSET** to set a field and value (creates if it doesn't exist).
- **HGET** to retrieve a specific field's value.
- **HGETALL** to fetch all fields and values.
- **HDEL** to remove a specific field.
- **HEXISTS** to check if a field exists.

**Examples:**

**Code Example: Hash in Redis**

Here is the Python code:

```python
import redis

# Connect to Redis
client = redis.StrictRedis(host='localhost', port=6379, db=0)

# User Attributes
user_key = 'user:123' # user_123 is a unique identifier for a user
user_attributes = {
    'name': 'John Doe',
    'email': 'john.doe@email.com',
    'age': '30'
}

# Store user attributes as a hash
client.hset(user_key, mapping=user_attributes)

# Retrieve user's name
name = client.hget(user_key, 'name')
print(f"User Name: {name.decode()}")  # Decoding from bytes back to string

# Check if a user is already registered
new_user_attributes = {
    'name': 'Jane Smith',
    'email': 'jane.smith@email.com'
}
is_new_user = bool(client.hsetnx(user_key, mapping=new_user_attributes))

if is_new_user:
    print("Welcome! A new user has been registered.")

# Fetch all user attributes
all_attributes = client.hgetall(user_key)
print("All User Attributes:")
for field, value in all_attributes.items():
    print(f"{field.decode()}: {value.decode()}")

# Delete User's age
deleted = client.hdel(user_key, 'age')
print(f"Age field deleted: {bool(deleted)}")

# Check if age field exists
has_age = bool(client.hexists(user_key, 'age'))
print(f"Age field exists: {has_age}")
```

**Sources:**

- [Devinterview.io — Redis](https://devinterview.io/questions/web-and-mobile-development/redis-interview-questions/)

</details>

<details>
<summary>How do you handle atomic operations in Redis?</summary>

### How do you handle atomic operations in Redis?

**Definition:**

**Redis** primarily employs **single-operation atomicity** at the level of commands or scripts. This minimizes race conditions and makes data management safer.

To ensure atomicity during multi-step processes, Redis supports **Transactional Commands** and **WATCH-EXEC Mechanism** for additional layers of consistency.

**How it works:**

**Multi-Step Atomicity Mechanisms**

1. **WATCH-EXEC**: Protects transactional integrity of specific keys by monitoring them. The ensuing multi-step EXEC block ensures actions are processed only if the watched keys are unaltered.

2. **MULTI-EXEC**: Encloses an array of commands to be executed atomically, either all together or none at all. This safeguards against partial executions of the enclosed commands.

**Key Takeaways**

- Redis ensures atomicity at different levels but may apply it differently depending on the command in use.
- The WATCH-EXEC pair safeguards the integrity of specific keys within a transaction.
- Though managed with varying degrees of atomicity, individual Redis commands effectively serve specific data manipulation needs.

**Examples:**

**Code Example: WATCH-EXEC**

Here is the Python code:

```python
import redis

r = redis.StrictRedis()

# Initialize the watched key
r.set('watched', 100)

# Begin watch and multi-step execution
pipe = r.pipeline()
while True:
    try:
        pipe.watch('watched')
        value = pipe.get('watched')
        new_value = int(value) + 1
        # If the value was altered externally, retry
        if pipe.execute():
            break
        pipe.multi()
        pipe.set('watched', new_value)
    except redis.WatchError:
        continue
```

**Sources:**

- [Devinterview.io — Redis](https://devinterview.io/questions/web-and-mobile-development/redis-interview-questions/)

</details>

<details>
<summary>What are lists in Redis and what are some common operations you can perform on them?</summary>

### What are lists in Redis and what are some common operations you can perform on them?

**Definition:**

**Redis** provides a standalone **list** data structure known as **Redis List**, which is operationally efficient for working with ordered collections.

Lists in Redis are:

- **Dynamic** in sizing, expanding or shrinking as elements are added or removed respectively.
- **Indexed**, which means elements within a list are accessible based on a 0-based index.

**How it works:**

**Key Operations**

**Add Elements**

- **LEFT PUSH (LPUSH)**: Adds an element at the head of the list.
- **RIGHT PUSH (RPUSH)**: Adds an element at the tail of the list.

**Remove Elements**

- **LEFT POP (LPOP)**: Removes and returns the element at the head of the list.
- **RIGHT POP (RPOP)**: Removes and returns the element at the tail of the list.

**Range Operations**

- **RANGE**: Provides a range of elements based on index positions.
- **TRIM**: Trims the list to include only the specified range of elements.

**List Information**

- **LENGTH**: Returns the current size of the list.
- **INDEX SEARCH**: Find the index of the first element matching a value.
- **ELEMENT SEARCH**: Find elements matching a pattern (using RPOP, for example).

**List Updates**

- **INSERT**: Inserts an element either before or after a reference element.
- **UPDATE AT**: Updates the value of an element at a particular index.

**List-to-List Operations**

- **POP-AND-PUSH**: Moves an element from the tail of one list to the head of another list.
- **BLOCKING POP**: Access an element from a list in a blocking manner, useful for building distributed message queues.

**When to use:**

**Use Case Scenarios**

1. **Message Queues**: Simplifying queue operations, such as adding messages to the end and processing from the front of the list.

2. **Collaborative List Management**: Allowing collaborators to add, remove, and update elements as needed, similar to Google Sheets in real-time mode.

3. **Activity Streams**: Managing real-time activity streams of users or other data points.

4. **Search Engine Indexing**: Implementing a small-scale search engine, where recent searches and indexed terms can be managed in lists.

5. **Leaderboards**: Tracking scores for a game or competition, where the list is maintained in descending order of scores.

6. **Partitioned Data**: Dividing millions of records into smaller chunks by maintaining them in separate lists, thereby enhancing data retrieval performance.

**Sources:**

- [Devinterview.io — Redis](https://devinterview.io/questions/web-and-mobile-development/redis-interview-questions/)

</details>

<details>
<summary>Can you describe the Pub/Sub model and how it's implemented in Redis?</summary>

### Can you describe the Pub/Sub model and how it's implemented in Redis?

**Definition:**

**Redis**, though primarily known for its **key-value store**, offers a versatile **Publish-Subscribe** (Pub/Sub) functionality.

**How it works:**

**Paradigm Overview**

In the Pub/Sub model, publishers produce messages, while subscribers receive and process these messages. Redis implements this pattern using a **channel-based** approach: Each message is labeled with a channel name, which acts as a direct-to-receive queue for subscribers.

Here, the publisher doesn't send messages to specific subscribers, as in many other messaging systems. Instead, subscribers **express interest in topics** or "channels," and they only receive messages that are relevant to these topics.

**Key Concepts**

- **Publisher**: Responsible for creating messages and sending them to associated channels.
- **Subscriber**: Registers interest in specific channels and receives new messages from these channels.

**Redis PUB/SUB Model**

In the Redis model:

- Three core components drive communication:
  - The **publications-container**, storing messages associated with channels.
  - A **subscription registry** that maintains channel-subscriber relationships.
  - The **management interface**, organizing channels, and subscribers. Old "abandoned" channels may be automatically discarded.

- Publishers and subscribers interact with Redis via designated **command-sets**.

**Command Sets**

- **Publishers**: Employ the `PUBLISH` command to dispatch messages to specific channels.
- **Subscribers**: Use the `SUBSCRIBE`, `UNSUBSCRIBE`, and `PATTERN MATCHING` commands to manage channel subscriptions and message receipt.

**Channel Management**

- Subscribers, using `SUBSCRIBE`, add channels of interest to the subscription registry.
- Through `UNSUBSCRIBE` or other means, they can opt-out from particular subscriptions.

After a **channel has no remaining subscribers**, Redis removes it from the subscriptions registry.

**Message Broadcasting**

When a channel receives a new message:

- For the **channel's matched subscribers**:
  - Redis serves the message immediately.
- For **subsequent subscribers**:
  - Redis delivers the message when all current messages are processed. This ensures consistent message ordering.

**Sub-Topics**

**Subscription Lifecycle**: Explore the sequential states that take place when a subscriber interacts with Redis.

**Message Exchange**: Dive into the steps Redis follows to transmit messages from publishers to subscribers.

**Channel Cleaning**: Understand the mechanism Redis uses to manage disused—or "cold"—channels, for efficiency and system hygiene.

**Examples:**

**Code Example: Pub/Sub in Redis**

Here is the Python code:

```python
import redis
import time
from multiprocessing import Process

def publisher():
    publisher = redis.StrictRedis(host='localhost', port=6379, db=0)
    
    while True:
        publisher.publish('news', 'New News!')
        time.sleep(1)

def subscriber():
    subscriber = redis.StrictRedis(host='localhost', port=6379, db=0)
    pubsub = subscriber.pubsub()
    pubsub.subscribe('news')
    
    for message in pubsub.listen():
        print("Received:", message['data'])

# Start publisher and subscriber on separate processes
Process(target=publisher).start()
Process(target=subscriber).start()
```

**Sources:**

- [Devinterview.io — Redis](https://devinterview.io/questions/web-and-mobile-development/redis-interview-questions/)

</details>

<details>
<summary>What is pipelining in Redis and when would you use it?</summary>

### What is pipelining in Redis and when would you use it?

**Definition:**

**Pipelining** in **Redis** enables **multiple commands to be sent in a single network request**, boosting performance.

By reducing round-trips between clients and the server, pipelining offers improved efficiency, particularly in situations where latency or the number of requests are a concern.

**How it works:**

**Key Components**

- **Queue**: Commands awaiting a response
- **TCP Connection**: Data transfer medium
- **Client**: Initiator of pipelined commands
- **Server**: Pipelining-compatible Redis instance.

**Advantages**

- **Performance**: Reduced overhead from round-trips speeds up overall execution.
- **Network Efficiency**: Less frequent network interactions.
- **Atomicity**: Pipelined sequences are atomic; either all commands succeed or none does.
- **Synchronization**: Pipelining maintains command order.

**Disadvantages**

- **Complexity**: Handling out-of-order occurrences or incomplete pipelines can be more challenging.
- **Consistency**: Delayed or disruptive pipelining can impact data consistency.

**When to use:**

Pipelining is beneficial in these scenarios:

**Data-Intensive Operations**

When you need to perform a high number of commands on Redis and maximize efficiency.

**Latency Management**

Especially in distributed systems where network latency can be a bottleneck, pipelining provides a way to manage such delays.

**Caching**

Primarily, when you're using Redis as a cache store, pipelining can help boost performance and optimize data retrieval.

**Examples:**

**Code Example: Basic Pipelining**

Here is the Python code:

```python
import redis

# Connect to Redis
r = redis.StrictRedis(host='localhost', port=6379, db=0)

# Initialize a pipeline
pipe = r.pipeline()

# Queue up commands
pipe.set('key1', 'value1').get('key1')

# Execute the pipeline
result_set, result_get = pipe.execute()
print(result_set, result_get)
```

**Sources:**

- [Devinterview.io — Redis](https://devinterview.io/questions/web-and-mobile-development/redis-interview-questions/)

</details>

<details>
<summary>What are the different types of Redis databases?</summary>

### What are the different types of Redis databases?

**Definition:**

**Redis** provides a range of **data types**, each optimized for specific tasks.

**How it works:**

**Core Data Types**

1. **Strings**: Useful for key-value pairs or simple message storage.

2. **Hashes**: Ideal for representing objects, aggregate data, or handling user input.

3. **Lists**: Suitable for storing logs, messaging queues, or tasks. Both ends are optimized for fast operations.

4. **Sets**: Designed to manage unique item collections.

5. **Sorted Sets**: Like sets, but items are sorted based on scores; great for leaderboards or ranged lookups.

6. **Bitmaps**: Efficient for state tracking, for instance, user activities on specific dates.

7. **Hyperloglogs**: Provides fast, approximate set cardinality; helpful for data analytics.

8. **Geospatial Indexes**: Efficient for location-based queries.

9. **Pub/Sub**: Provides a messaging system for real-time updates and chat features.

10. **Streams**: A recent addition for log management and real-time data processing.

11. **Search Indexes**: Though not built-in, Redis is often paired with search solutions such as RediSearch for advanced querying.

12. **Time-Series and More**: Redis modules extend data types, enabling operations like time-series data handling.

**Sources:**

- [Devinterview.io — Redis](https://devinterview.io/questions/web-and-mobile-development/redis-interview-questions/)

</details>

<details>
<summary>What is a Redis transaction and how do you create one?</summary>

### What is a Redis transaction and how do you create one?

**Definition:**

A Redis transaction runs a group of commands as one isolated step. You open it with `MULTI`, queue commands, and run them with `EXEC`.

**How it works:**

- `MULTI` replies `OK`. Later commands are queued and reply `QUEUED` instead of running immediately.
- `EXEC` runs the queue in order and returns one reply per command. Another client is not served in the middle of that execution.
- `DISCARD` throws away the queue. If the client disconnects before `EXEC`, none of the queued commands run.
- `WATCH key` is optimistic locking. If a watched key changes before `EXEC`, the transaction is aborted.

**When to use:**

Use `MULTI`/`EXEC` when a few commands must not be interleaved with other clients, such as incrementing two counters together. Use `WATCH` when the write depends on a value you just read. Use `EVAL` when the server must branch on that value before writing.

**Examples:**

```redis
MULTI
INCR foo
INCR bar
EXEC
```

**Sources:**

- [Transactions](https://redis.io/docs/latest/develop/using-commands/transactions/)
- [`MULTI`](https://redis.io/docs/latest/commands/multi/)
- [`EXEC`](https://redis.io/docs/latest/commands/exec/)
- [`WATCH`](https://redis.io/docs/latest/commands/watch/)

</details>

<details>
<summary>Can you explain the blocking operations of Redis lists?</summary>

### Can you explain the blocking operations of Redis lists?

**Definition:**

Blocking list commands wait on one client connection until a list has an element or a timeout expires. They do not stop the whole Redis server.

**How it works:**

- `BLPOP key [key ...] timeout` pops the head of the first non-empty list, in the order of the keys.
- `BRPOP` pops the tail. `timeout` is in seconds. `0` waits without a time limit.
- The reply is the key and the element. If the timeout expires, the reply is nil.
- `BLMOVE source destination LEFT|RIGHT LEFT|RIGHT timeout` is the blocking form of `LMOVE`. `BRPOPLPUSH` is deprecated since Redis 6.2.0; the docs point to `BLMOVE` with `RIGHT` and `LEFT`.

**When to use:**

Use a blocking pop for a worker that should sleep until another client pushes a job. Polling `LLEN` in a loop keeps the connection busy.

**Examples:**

```redis
BLPOP list1 list2 0
BLMOVE source destination LEFT RIGHT 0
```

**Sources:**

- [`BLPOP`](https://redis.io/docs/latest/commands/blpop/)
- [`BRPOP`](https://redis.io/docs/latest/commands/brpop/)
- [`BLMOVE`](https://redis.io/docs/latest/commands/blmove/)
- [Lists](https://redis.io/docs/latest/develop/data-types/lists/)

</details>

<details>
<summary>What are hyperloglogs in Redis and what problem do they solve?</summary>

### What are hyperloglogs in Redis and what problem do they solve?

**Definition:**

A HyperLogLog estimates how many distinct values were added. It keeps a compact structure instead of storing every member.

**How it works:**

- `PFADD key element [element ...]` adds elements. It returns `1` if an internal register changed, otherwise `0`.
- `PFCOUNT key [key ...]` returns the approximated cardinality. Several keys are counted as their union.
- `PFMERGE destkey sourcekey [sourcekey ...]` merges HyperLogLogs into one key.

**When to use:**

Use a HyperLogLog for a unique count that may be approximate, such as distinct visitors. Use a set when you need the members themselves or an exact count.

**Examples:**

```redis
PFADD bikes Hyperion Deimos Phoebe Quaoar
PFCOUNT bikes
PFMERGE all_bikes bikes commuter_bikes
```

**Sources:**

- [HyperLogLog](https://redis.io/docs/latest/develop/data-types/probabilistic/hyperloglogs/)
- [`PFADD`](https://redis.io/docs/latest/commands/pfadd/)
- [`PFCOUNT`](https://redis.io/docs/latest/commands/pfcount/)

</details>

### Redis Advanced Data Structures & Features

<details>
<summary>How do sets work in Redis and what operations can you perform on them?</summary>

### How do sets work in Redis and what operations can you perform on them?

**Definition:**

A Redis set is an unordered collection of unique strings stored at one key.

**How it works:**

- `SADD key member [member ...]` adds members. Adding a member that is already present does not create a duplicate.
- `SMEMBERS key` returns the members in no particular order. `SCARD key` returns how many members there are. `SISMEMBER key member` tests one member.
- `SINTER`, `SUNION`, and `SDIFF` combine sets: intersection, union, and difference.

**When to use:**

Use a set for unique membership, tags, or an inverted index of ids. Use a sorted set when each member needs a score and an order.

**Examples:**

```redis
SADD myset a
SADD myset b
SADD myset foo
SCARD myset
SINTER key1 key2
```

**Sources:**

- [Sets](https://redis.io/docs/latest/develop/data-types/sets/)
- [`SADD`](https://redis.io/docs/latest/commands/sadd/)
- [`SINTER`](https://redis.io/docs/latest/commands/sinter/)

</details>

<details>
<summary>What are sorted sets in Redis and how do they differ from regular sets?</summary>

### What are sorted sets in Redis and how do they differ from regular sets?

**Definition:**

A sorted set stores unique members, and each member has a score. Redis orders the members by that score. A regular set has no scores and no order.

**How it works:**

- `ZADD key score member [score member ...]` adds or updates members.
- `ZRANGE key start stop` returns members by rank. Rank `0` is the lowest score and `-1` is the last member.
- `ZREM key member [member ...]` removes members.
- Like a set, a member appears once. Unlike a set, you can read a range in score order.

**When to use:**

Use a sorted set for a leaderboard, a time index, or a rate-limit window. Use a regular set when you only care whether a member exists.

**Examples:**

```redis
ZADD zset 10 a
ZADD zset 5 b
ZADD zset 12.55 c
ZRANGE zset 0 -1
```

**Sources:**

- [Sorted sets](https://redis.io/docs/latest/develop/data-types/sorted-sets/)
- [`ZADD`](https://redis.io/docs/latest/commands/zadd/)
- [`ZRANGE`](https://redis.io/docs/latest/commands/zrange/)

</details>

<details>
<summary>Can you use Redis for geospatial data? If so, explain how.</summary>

### Can you use Redis for geospatial data? If so, explain how.

**Definition:**

Yes. Redis stores longitude and latitude on a geospatial index and can search by radius or by a box. The index is a sorted set under the hood.

**How it works:**

- `GEOADD key [NX|XX] [CH] longitude latitude member [longitude latitude member ...]` adds places. The key is created if it does not exist.
- `GEODIST key member1 member2 [M|KM|FT|MI]` returns the distance between two members.
- `GEOSEARCH key FROMLONLAT longitude latitude BYRADIUS radius KM` (or `FROMMEMBER`, or `BYBOX`) returns members in that area. `ASC` sorts from nearest to farthest.

**When to use:**

Use it when the query is “which members are near this point”. Keep a separate key if you also need non-geographic fields for the same place.

**Examples:**

```redis
GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania"
GEOSEARCH Sicily FROMLONLAT 15 37 BYRADIUS 200 km ASC
```

**Sources:**

- [`GEOSEARCH`](https://redis.io/docs/latest/commands/geosearch/)
- [Sorted sets](https://redis.io/docs/latest/develop/data-types/sorted-sets/)

</details>

<details>
<summary>What is a bitmap and how can you use it in Redis?</summary>

### What is a bitmap and how can you use it in Redis?

**Definition:**

A bitmap is a Redis string addressed one bit at a time. An offset is an integer id, and the bit is 0 or 1.

**How it works:**

- `SETBIT key offset 0|1` sets one bit and returns the previous value.
- `GETBIT key offset` returns that bit.
- `BITOP AND|OR|XOR|NOT destkey key [key ...]` writes a bitwise combination of strings into `destkey`.
- `BITCOUNT` counts the bits set to 1.

**When to use:**

Use a bitmap when each id is a dense yes/no flag, such as “user 123 was active this hour”. Use a set when you need to store the id strings themselves.

**Examples:**

```redis
SETBIT pings:2024-01-01-00:00 123 1
GETBIT pings:2024-01-01-00:00 123
```

**Sources:**

- [Bitmaps](https://redis.io/docs/latest/develop/data-types/bitmaps/)
- [`SETBIT`](https://redis.io/docs/latest/commands/setbit/)
- [`BITOP`](https://redis.io/docs/latest/commands/bitop/)
- [`BITCOUNT`](https://redis.io/docs/latest/commands/bitcount/)

</details>

<details>
<summary>Explain the concept of a stream in Redis.</summary>

### Explain the concept of a stream in Redis.

**Definition:**

A stream is an append-only log. Each entry has an id and a list of field-value pairs stored in the order you added them.

**How it works:**

- `XADD` is the command that appends an entry. Use `*` to let Redis generate the id.
- `XADD key MAXLEN ~ 1000 * field value` trims the stream to about 1000 entries. `~` is approximate trimming and is cheaper than an exact cap.
- `XREAD COUNT 100 BLOCK 300 STREAMS key $` waits up to 300 milliseconds for new entries. `$` means “only entries appended after this call”.
- `XREADGROUP GROUP group consumer ... STREAMS key >` reads entries not yet delivered to that consumer group. `>` means undelivered entries.

**When to use:**

Use a stream when workers must share a durable log and acknowledge entries. Pub/Sub does not store a message for a subscriber that was offline. A list is a simpler queue without stream ids or consumer groups.

**Examples:**

```redis
XADD race:italy * rider Castilla
XREAD COUNT 100 BLOCK 300 STREAMS race:france $
```

**Sources:**

- [Streams](https://redis.io/docs/latest/develop/data-types/streams/)
- [`XADD`](https://redis.io/docs/latest/commands/xadd/)
- [`XREAD`](https://redis.io/docs/latest/commands/xread/)
- [`XREADGROUP`](https://redis.io/docs/latest/commands/xreadgroup/)

</details>

<details>
<summary>How do you use Lua scripting in Redis?</summary>

### How do you use Lua scripting in Redis?

**Definition:**

`EVAL` runs a Lua program inside Redis. Key names are passed separately so Redis knows which keys the script touches.

**How it works:**

- `EVAL script numkeys [key ...] [arg ...]` — `numkeys` is how many of the following arguments are key names. Use `0` when there are no keys.
- The script body is Lua source. It does not have to define a function.
- `SCRIPT LOAD script` stores the script and returns a SHA1 digest.
- `EVALSHA sha1 numkeys` runs that cached script.

**When to use:**

Use a script when the next write depends on a read and you need that logic on the server. `MULTI`/`EXEC` only queues commands; it does not branch. Redis does not roll a script back either.

**Examples:**

```redis
EVAL "return 'Hello, scripting!'" 0
SCRIPT LOAD "return 'Immabe a cached script'"
EVALSHA c664a3bf70bd1d45c4284ffebb65a6f2299bfc9f 0
```

**Sources:**

- [Scripting with Lua](https://redis.io/docs/latest/develop/programmability/eval-intro/)
- [`EVAL`](https://redis.io/docs/latest/commands/eval/)
- [`SCRIPT LOAD`](https://redis.io/docs/latest/commands/script-load/)

</details>

<details>
<summary>What are modules in Redis?</summary>

### What are modules in Redis?

**Definition:**

A Redis module is a shared library loaded into the server. It can add commands and data types that are not in the core server.

**How it works:**

- `MODULE LIST` returns every loaded module: name, version, and path. Complexity is O(N) in the number of modules. It has existed since Redis 4.0.0.
- A Redis Stack install can show modules such as `search`, `timeseries`, `bf`, `ReJSON`, and `vectorset`. An empty list means the configuration did not load any module.
- Modules can also implement blocking commands. Built-in blocking commands such as `BLPOP` block only the calling client.

**When to use:**

Load a module when you need a type the core server does not provide, such as JSON or a search index. Strings, lists, sets, hashes, and streams do not need one.

**Examples:**

```redis
MODULE LIST
```

**Sources:**

- [`MODULE LIST`](https://redis.io/docs/latest/commands/module-list/)
- [Modules API](https://redis.io/docs/latest/develop/reference/modules/)

</details>

<details>
<summary>Can you explain Redis's role as a message broker?</summary>

### Can you explain Redis's role as a message broker?

**Definition:**

Redis brokers messages in two documented ways: Pub/Sub delivers to subscribers who are connected now, and a stream keeps an append-only log that consumers can read later.

**How it works:**

- `SUBSCRIBE channel [channel ...]` listens for messages on those channels.
- A published message is delivered to current subscribers. It is not stored as a key for someone who connects afterward.
- A stream keeps entries. `XADD` appends, and `XREADGROUP` lets a consumer group take work that was not delivered yet.
- A list plus `BLPOP` is a simpler queue: the pop removes the element, so a crashed worker can lose it unless you move it with `BLMOVE`.

**When to use:**

Use Pub/Sub for live fan-out when a missed message is acceptable. Use a stream when a worker must catch up after it was offline. Use `BLMOVE` when you want a list queue that parks in-progress work on a second list.

**Examples:**

```redis
SUBSCRIBE orders
XADD race:italy * rider Castilla
```

**Sources:**

- [Pub/Sub](https://redis.io/docs/latest/develop/pubsub/)
- [`SUBSCRIBE`](https://redis.io/docs/latest/commands/subscribe/)
- [Streams](https://redis.io/docs/latest/develop/data-types/streams/)
- [`BLPOP`](https://redis.io/docs/latest/commands/blpop/)

</details>

<details>
<summary>What are the limitations of Redis transactions?</summary>

### What are the limitations of Redis transactions?

**Definition:**

A transaction isolates commands only while `EXEC` runs them. Redis does not roll the batch back if one command fails at runtime.

**How it works:**

- Commands sit in a queue until `EXEC`. They are then serialized so no other client runs in the middle.
- There is no rollback. The transactions page states that rollbacks would hurt Redis simplicity and performance.
- A disconnect before `EXEC` runs nothing. After `EXEC` is accepted, the commands run. A crash can still leave a partial transaction in the AOF; Redis then refuses to start, and `redis-check-aof` can strip that partial transaction.
- `WATCH` aborts the transaction if a watched key changed. It is check-and-set, not a lock held for the whole time the commands were queued.
- The same no-rollback rule applies to Lua scripts.

**When to use:**

Do not treat `MULTI`/`EXEC` as a database transaction that undoes earlier commands in the batch. Check each reply from `EXEC`, or decide in a Lua script before you write.

**Examples:**

```redis
SET foo 1
MULTI
INCR foo
DISCARD
GET foo
```

**Sources:**

- [Transactions](https://redis.io/docs/latest/develop/using-commands/transactions/)
- [`DISCARD`](https://redis.io/docs/latest/commands/discard/)
- [`WATCH`](https://redis.io/docs/latest/commands/watch/)

</details>

<details>
<summary>How do you create and manage indices in Redis?</summary>

### How do you create and manage indices in Redis?

**Definition:**

Redis does not keep a secondary index inside a key the way a SQL table does. You maintain another key, usually a set or a sorted set, or you load a search module.

**How it works:**

- Store the record in its own key. Add the id to an index set with `SADD`.
- `SINTER` combines index sets when a lookup must match every filter.
- A sorted set index (`ZADD` / `ZRANGE`) orders the same ids by a score.
- `MODULE LIST` shows a `search` module when one is loaded. The query commands then belong to that module, not to the core key types.

**When to use:**

Maintain a set index for exact tags. Use a sorted set when the index must be ordered. Load the search module when you need search beyond `SINTER` and `ZRANGE`.

**Examples:**

```redis
SADD skill:redis user:1
SADD skill:python user:1
SINTER skill:redis skill:python
```

**Sources:**

- [Sets](https://redis.io/docs/latest/develop/data-types/sets/)
- [`SINTER`](https://redis.io/docs/latest/commands/sinter/)
- [Sorted sets](https://redis.io/docs/latest/develop/data-types/sorted-sets/)
- [`MODULE LIST`](https://redis.io/docs/latest/commands/module-list/)

</details>

<details>
<summary>Discuss Redis's role in session caching.</summary>

### Discuss Redis's role in session caching.

**Definition:**

A session cache keeps the session payload in Redis and drops it when the TTL expires, so any app server with the session id can read it.

**How it works:**

- Write the session with `SET`. The fixed-window rate-limit scripts in the docs use `EXPIRE key seconds` to bound a key’s life; the same command bounds a session.
- When the TTL hits zero, a later read misses and the application treats the session as gone.
- The session id in the cookie is the Redis key suffix. The user record that must survive logout still belongs in the durable database.
- Replication lets another Redis node serve the session after failover. Persistence (`appendonly`) decides whether a restart keeps sessions.

**When to use:**

Use Redis when many app processes must share short-lived session state. Keep long-lived account data in the primary database and set a TTL so idle sessions disappear.

**Examples:**

```redis
SET session:abc "user-id=7"
EXPIRE session:abc 3600
```

**Sources:**

- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [Replication](https://redis.io/docs/latest/operate/oss_and_stack/management/replication/)
- [`INCR` (EXPIRE in the rate-limit example)](https://redis.io/docs/latest/commands/incr/)

</details>

<details>
<summary>Can Redis be used as a primary database? Why or why not?</summary>

### Can Redis be used as a primary database? Why or why not?

**Definition:**

Redis can be the system of record for data that fits in memory, if you turn on persistence and replication. It is still an in-memory data-structure server: the working copy is RAM, and disk is the recovery log.

**How it works:**

- RDB is a point-in-time snapshot. AOF appends write commands and replays them on startup. `appendonly yes` turns AOF on.
- `appendfsync always` fsyncs every appended batch (slow, safest). `everysec` fsyncs once a second and can lose about one second. `appendfsync no` never fsyncs.
- If both AOF and RDB exist, recovery follows the persistence setup you enabled. A crash during `EXEC` can leave a partial transaction in the AOF until `redis-check-aof` fixes it.
- There is no SQL join. Multi-key consistency is what you code with `MULTI`/`EXEC` or `EVAL`, and those paths do not roll back.

**When to use:**

Use Redis as the primary store for sessions, leaderboards, queues, and other values you model with its types and can size to RAM. Use a disk-oriented database when the data set is larger than memory or you need ad hoc relational queries.

**Examples:**

```redis
CONFIG SET appendonly yes
```

**Sources:**

- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [`CONFIG SET`](https://redis.io/docs/latest/commands/config-set/)
- [Transactions](https://redis.io/docs/latest/develop/using-commands/transactions/)

</details>

### Redis Configuration & Performance

<details>
<summary>How do you configure Redis for optimal performance?</summary>

### How do you configure Redis for optimal performance?

**Definition:**

Command execution is fast when commands stay short and Redis is not forced into swap or a huge fork copy. The admin and eviction docs set an explicit memory limit and a policy for what happens at that limit.

**How it works:**

- Set `maxmemory` below the RAM you can give the process. Leave room for client buffers and fragmentation. `INFO memory` includes `mem_not_counted_for_evict`, the buffer memory eviction does not count the same way.
- `maxmemory-policy allkeys-lru` evicts least recently used keys when the limit is passed. `noeviction` returns an error on commands that would add data and still serves reads.
- While an RDB save or AOF rewrite runs, copy-on-write can make a write-heavy instance use up to about twice the normal memory.
- On Linux the admin guide sets `vm.overcommit_memory = 1` and disables Transparent Huge Pages with `echo never > /sys/kernel/mm/transparent_hugepage/enabled`.

**When to use:**

Use `allkeys-lru` for a cache whose hot keys are a small subset. Use `noeviction` when dropping a key is worse than failing the write. Avoid `KEYS` on a large database; it is O(N) and the command page says not to use it in regular application code.

**Examples:**

```redis
maxmemory 2mb
maxmemory-policy allkeys-lru
```

**Sources:**

- [Configuration](https://redis.io/docs/latest/operate/oss_and_stack/management/config/)
- [Eviction](https://redis.io/docs/latest/develop/reference/eviction/)
- [Administration](https://redis.io/docs/latest/operate/oss_and_stack/management/admin/)

</details>

<details>
<summary>What is Redis replication and how do you set it up?</summary>

### What is Redis replication and how do you set it up?

**Definition:**

Replication copies a primary’s dataset to replica servers. Since Redis 2.6 a replica is read-only by default.

**How it works:**

- `REPLICAOF host port` makes this server replicate that primary.
- `REPLICAOF NO ONE` promotes the replica so it accepts writes as a primary again.
- `replica-read-only` in `redis.conf` controls replica writes. Change it at runtime with `CONFIG SET`.
- Clients that only read can use a replica. Writes still go to the primary.

**When to use:**

Add replicas for a standby and for extra read capacity. Replication does not replace AOF or RDB on the primary if you need disk durability.

**Examples:**

```redis
REPLICAOF 127.0.0.1 6799
REPLICAOF NO ONE
```

**Sources:**

- [Replication](https://redis.io/docs/latest/operate/oss_and_stack/management/replication/)
- [`REPLICAOF`](https://redis.io/docs/latest/commands/replicaof/)
- [`CONFIG SET`](https://redis.io/docs/latest/commands/config-set/)

</details>

<details>
<summary>How do you scale Redis?</summary>

### How do you scale Redis?

**Definition:**

You scale Redis by giving one primary more memory, by adding read replicas, or by sharding with Redis Cluster. Cluster splits the keyspace into 16384 hash slots.

**How it works:**

- A replica serves reads. The primary still takes the writes.
- Cluster does not use consistent hashing. The slot is CRC16 of the key modulo 16384. Each primary owns a subset of those slots.
- Moving slots to a new node, or off a node you want to remove, does not require stopping the cluster.
- `redis-cli --cluster add-node <new> <existing>` joins a node. `--cluster-slave` joins it as a replica of a primary that has fewer replicas.

**When to use:**

Stay on one primary plus replicas when the write rate and the data set fit one process. Move to Cluster when you must split slots across several primaries.

**Examples:**

```redis
redis-cli --cluster add-node 127.0.0.1:7006 127.0.0.1:7000 --cluster-slave
```

**Sources:**

- [Scale with Redis Cluster](https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/)
- [Replication](https://redis.io/docs/latest/operate/oss_and_stack/management/replication/)

</details>

<details>
<summary>Explain how Redis Sentinel works.</summary>

### Explain how Redis Sentinel works.

**Definition:**

Redis Sentinel is the high-availability setup for a primary that is not sharded by Redis Cluster. Sentinel processes watch the primary and its replicas and publish a new primary address after failover.

**How it works:**

- Sentinel is documented as high availability for non-clustered Redis.
- Clients ask `SENTINEL get-master-addr-by-name <name>` and receive the current primary host and port.
- After a failover the same command returns the promoted replica.
- The application should follow that address instead of a single hardcoded host.

**When to use:**

Use Sentinel when you have one primary, replicas, and you need automatic failover. Use Redis Cluster when you also need to split hash slots across multiple primaries.

**Examples:**

```redis
SENTINEL get-master-addr-by-name mymaster
```

**Sources:**

- [High availability with Redis Sentinel](https://redis.io/docs/latest/operate/oss_and_stack/management/sentinel/)
- [Replication](https://redis.io/docs/latest/operate/oss_and_stack/management/replication/)

</details>

<details>
<summary>What is Redis Cluster and how does it improve Redis' scalability and availability?</summary>

### What is Redis Cluster and how does it improve Redis' scalability and availability?

**Definition:**

Redis Cluster shards keys across primaries and keeps replicas so a failed primary can be replaced. Capacity grows by moving hash slots, without stopping the cluster.

**How it works:**

- There are 16384 hash slots. The slot of a key is CRC16(key) modulo 16384.
- Example layout from the docs: node A owns 0–5500, node B owns 5501–11000, node C owns 11001–16383.
- Adding node D means moving some slots from A, B, and C to D. Removing A means moving A’s slots to B and C, then taking the empty node out.
- `CLUSTER NODES` shows each node’s id, role, and slot range. `CLUSTER FAILOVER` on a replica forces a manual failover of its primary. `FORCE` skips consulting the primary. `TAKEOVER` takes over without the usual checks.

**When to use:**

Use Cluster when one primary cannot hold the working set or the writes. Use Sentinel when you only need failover and the data stays on one primary.

**Examples:**

```redis
CLUSTER NODES
CLUSTER FAILOVER
```

**Sources:**

- [Scale with Redis Cluster](https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/)
- [`CLUSTER NODES`](https://redis.io/docs/latest/commands/cluster-nodes/)
- [`CLUSTER FAILOVER`](https://redis.io/docs/latest/commands/cluster-failover/)

</details>

<details>
<summary>How do you monitor performance in Redis?</summary>

### How do you monitor performance in Redis?

**Definition:**

Redis exposes performance in commands you run against the server: `INFO`, `LATENCY DOCTOR`, `SLOWLOG`, and `MEMORY DOCTOR`.

**How it works:**

- `INFO`, and `INFO memory` or `INFO stats`, returns sections such as memory and keyspace hits and misses. The eviction guide uses `keyspace_hits` and `keyspace_misses` to see how often lookups find a key.
- `LATENCY DOCTOR` prints spikes it observed and points at the slow log when commands are slower than the latency threshold.
- `SLOWLOG GET <count>` lists recent slow commands, which the Redis troubleshooting notes call out for commands such as `KEYS`.
- `MEMORY DOCTOR` reports memory issues. The admin guide names both doctor commands for troubleshooting.

**Examples:**

```redis
INFO memory
LATENCY DOCTOR
SLOWLOG GET 10
```

**Sources:**

- [`INFO`](https://redis.io/docs/latest/commands/info/)
- [`LATENCY DOCTOR`](https://redis.io/docs/latest/commands/latency-doctor/)
- [`SLOWLOG GET`](https://redis.io/docs/latest/commands/slowlog-get/)
- [Administration](https://redis.io/docs/latest/operate/oss_and_stack/management/admin/)

</details>

<details>
<summary>What strategies can you use to optimize memory usage in Redis?</summary>

### What strategies can you use to optimize memory usage in Redis?

**Definition:**

Memory use is the size of keys plus overhead. Redis can evict keys, you can shorten large values, and you can see what one key costs.

**How it works:**

- Set `maxmemory` and a `maxmemory-policy` so a cache stops growing. `allkeys-lru` does not need a TTL on every key, and the eviction page notes that storing an expire value itself costs memory.
- `volatile-*` policies only evict keys that have a TTL. With no expiring keys they behave like `noeviction`.
- `MEMORY USAGE key [SAMPLES count]` estimates one key’s memory. The default sample count is 5. It has existed since Redis 4.0.0.
- `LATENCY DOCTOR` warns that deleting, expiring, or evicting a very large object blocks the server. Split that object into smaller keys.
- A write-heavy RDB or AOF rewrite can use up to about twice the normal memory.

**When to use:**

Evict when the instance is a cache. Split huge keys when doctor output blames large deletes. Raise RAM or shard with Cluster when the working set must all stay.

**Examples:**

```redis
MEMORY USAGE session:abc
MEMORY DOCTOR
```

**Sources:**

- [`MEMORY USAGE`](https://redis.io/docs/latest/commands/memory-usage/)
- [`MEMORY DOCTOR`](https://redis.io/docs/latest/commands/memory-doctor/)
- [Eviction](https://redis.io/docs/latest/develop/reference/eviction/)
- [Administration](https://redis.io/docs/latest/operate/oss_and_stack/management/admin/)

</details>

<details>
<summary>Can you explain the eviction policies in Redis?</summary>

### Can you explain the eviction policies in Redis?

**Definition:**

When used memory passes `maxmemory`, Redis evicts keys according to `maxmemory-policy`, or it refuses writes.

**How it works:**

- `noeviction`: nothing is evicted. Commands that would add data return an error. On a replica set this applies to the primary. Reads of existing data still work.
- `allkeys-lru` evicts least recently used keys. `allkeys-lfu` evicts least frequently used keys. `allkeys-lrm` evicts least recently modified keys. `allkeys-random` evicts random keys.
- `volatile-lru`, `volatile-lfu`, `volatile-lrm`, and `volatile-random` do the same among keys that have a TTL. `volatile-ttl` evicts the key with the shortest remaining TTL.
- If no key has a TTL, every `volatile-*` policy behaves like `noeviction`.
- The eviction guide’s default suggestion is `allkeys-lru` when a small subset of keys is much hotter than the rest.

**When to use:**

Pick `allkeys-lru` for a typical cache. Pick `allkeys-lrm` when reads of stable data should stay and unmodified keys can go. Pick `volatile-ttl` when the application marks victims with a short TTL. Pick `noeviction` when losing a key is not acceptable.

**Examples:**

```redis
maxmemory 2mb
maxmemory-policy allkeys-lru
```

**Sources:**

- [Eviction](https://redis.io/docs/latest/develop/reference/eviction/)
- [Configuration](https://redis.io/docs/latest/operate/oss_and_stack/management/config/)

</details>

<details>
<summary>What is the max memory setting in Redis?</summary>

### What is the max memory setting in Redis?

**Definition:**

`maxmemory` is the configured ceiling for memory Redis will use for the dataset. Past that ceiling, behavior depends on `maxmemory-policy`.

**How it works:**

- A cache example in the configuration guide is `maxmemory 2mb` with `maxmemory-policy allkeys-lru`.
- The admin guide says to set the limit by subtracting Redis overhead, buffers, and fragmentation from free RAM. If you believe 10 GB is free, it suggests trying 8 or 9 GB.
- `INFO memory` reports `mem_not_counted_for_evict` for buffer memory you should leave outside the dataset budget.
- With `noeviction`, crossing the limit does not delete keys. Writes that need more memory error; reads continue.
- `maxmemory` is unnecessary only in the sense the eviction page notes for the `noeviction` policy’s extra free-RAM padding; you still set a limit so Redis errors instead of dying when the host is nearly out of memory.

**When to use:**

Set `maxmemory` on every cache and on every host where the operating system might otherwise let Redis grow until the machine fails. Size it under the container limit when Redis runs in a container.

**Examples:**

```redis
CONFIG SET maxmemory 2mb
CONFIG SET maxmemory-policy noeviction
```

**Sources:**

- [Eviction](https://redis.io/docs/latest/develop/reference/eviction/)
- [Administration](https://redis.io/docs/latest/operate/oss_and_stack/management/admin/)
- [`CONFIG SET`](https://redis.io/docs/latest/commands/config-set/)

</details>

<details>
<summary>How does Redis handle backups?</summary>

### How does Redis handle backups?

**Definition:**

A backup is a copy of an RDB snapshot or of the AOF files, taken while Redis is still serving traffic or after an explicit save.

**How it works:**

- RDB snapshotting forks. The child writes a temporary RDB file and then replaces the old one. `save 60 1000` in the persistence guide means: dump if at least 1000 keys changed in 60 seconds.
- `BGSAVE` asks for a background snapshot. `SAVE` runs the snapshot in the foreground; the upgrade notes use `redis-cli SAVE` to refresh `dump.rdb`.
- With AOF, files are named like `appendonly.aof.*` under the append-only directory. `CONFIG SET appendonly yes` switches on AOF at runtime.
- Copy the finished `dump.rdb` or the AOF directory to storage outside the Redis host. The persistence guide’s Redis 2.0 notes say to copy the latest `dump.rdb` to a safe place before a risky change.

**When to use:**

Use `BGSAVE` or the automatic `save` rules for a point-in-time copy you can ship off the box. Use AOF when you also need the writes that happened after the last snapshot. Test that a copy actually loads.

**Examples:**

```redis
BGSAVE
SAVE
```

**Sources:**

- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [`BGSAVE`](https://redis.io/docs/latest/commands/bgsave/)
- [`SAVE`](https://redis.io/docs/latest/commands/save/)

</details>

### Redis Security

<details>
<summary>What security features does Redis offer?</summary>

### What security features does Redis offer?

**Definition:**

Redis security is network isolation, protected mode, authentication, TLS, and ACLs that limit commands and keys. Redis is not safe to expose on an open network by itself.

**How it works:**

- The security page documents protected mode, authentication, TLS, and disabling specific commands.
- `ACL SETUSER alice on >p1pp0 ~cached:* +get` enables user `alice`, sets a password, allows only keys matching `cached:*`, and allows only `GET`.
- `ACL SETUSER karin on +@all -@dangerous` allows every command category except the `dangerous` category. `ACL CAT dangerous` includes `flushdb`, `keys`, `config`, `shutdown`, and `debug`.
- Categories other than `+@all` do not include module commands.

**When to use:**

Turn on ACLs and TLS for any Redis that application servers reach over a network. Keep the port off the public internet even with a password.

**Examples:**

```redis
ACL SETUSER alice on >p1pp0 ~cached:* +get
```

**Sources:**

- [Security](https://redis.io/docs/latest/operate/oss_and_stack/management/security/)
- [ACL](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/)
- [`ACL SETUSER`](https://redis.io/docs/latest/commands/acl-setuser/)

</details>

<details>
<summary>How do you secure Redis deployment?</summary>

### How do you secure Redis deployment?

**Definition:**

A secure deployment binds Redis to a private network, requires an ACL user, and uses TLS when traffic leaves the host.

**How it works:**

- Follow the security page: protected mode, authentication, and TLS. Do not leave the default open access reachable from untrusted hosts.
- Create a user with `ACL SETUSER` instead of sharing one unrestricted password. Limit keys with a pattern such as `~cached:*` and drop `@dangerous` commands for application users.
- Node.js clients pass `username`, `password`, and `socket.tls` into `createClient` when the server expects TLS.
- `ACL CAT dangerous` is the list to deny on app roles: it includes `CONFIG`, `FLUSHALL`, `DEBUG`, `KEYS`, and `SHUTDOWN`.

**When to use:**

Use a private address and an ACL on every environment, including containers. Add TLS when the client and Redis are not on the same trusted network.

**Examples:**

```redis
ACL SETUSER alice on >p1pp0 ~cached:* +get
```

**Sources:**

- [Security](https://redis.io/docs/latest/operate/oss_and_stack/management/security/)
- [ACL](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/)
- [Connect with node-redis](https://redis.io/docs/latest/develop/clients/nodejs/connect/)

</details>

<details>
<summary>What are some common best practices for securing Redis?</summary>

### What are some common best practices for securing Redis?

**Definition:**

Treat Redis as a database on a private network: authenticate every client, allow only the commands that client needs, and do not run dangerous commands from application code.

**How it works:**

- Protected mode and authentication are part of the documented security model, together with TLS.
- `+@all -@dangerous` is the documented way to keep a user broad while removing the dangerous category. Confirm the category with `ACL CAT dangerous`.
- Application code should use `SCAN` with `MATCH` and `COUNT`, not `KEYS`. The `KEYS` page says to be extremely careful in production because it can ruin performance on a large database, and it is in the dangerous category.
- Disable or ACL-deny commands you do not use, which the security page covers under disallowing specific commands.

**When to use:**

Apply the ACL and the network boundary before the instance holds real data. Review `@dangerous` again after you load modules, because module commands are not included in categories other than `+@all`.

**Examples:**

```redis
ACL SETUSER karin on +@all -@dangerous
SCAN 0 MATCH user:* COUNT 100
```

**Sources:**

- [Security](https://redis.io/docs/latest/operate/oss_and_stack/management/security/)
- [ACL](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/)
- [`KEYS`](https://redis.io/docs/latest/commands/keys/)
- [`SCAN`](https://redis.io/docs/latest/commands/scan/)

</details>

### Redis Client and Integration

<details>
<summary>How do you connect to a Redis server using programming language X (replace X with a language of your choice)?</summary>

### How do you connect to a Redis server using programming language X (replace X with a language of your choice)?

**Definition:**

From Node.js, the documented client is `node-redis`. `createClient` takes the user, password, and socket, then you `connect` before commands.

**How it works:**

- Build the client with `username`, `password`, and `socket.host` / `socket.port`. For TLS, set `socket.tls` and pass `key`, `cert`, and `ca` as in the connect guide.
- Register an `error` listener. The sample logs `Redis Client Error`.
- `await client.connect()`, then `SET` and `GET`. Call `client.destroy()` when the process is done with the client.
- The same guide’s production page says the client reconnects by default and queues commands while the socket is down.

**Examples:**

```javascript
const client = createClient({
  username: 'default',
  password: 'secret',
  socket: { host: '127.0.0.1', port: 6379 }
});
client.on('error', (err) => console.log('Redis Client Error', err));
await client.connect();
await client.set('foo', 'bar');
```

**Sources:**

- [Connect with node-redis](https://redis.io/docs/latest/develop/clients/nodejs/connect/)
- [Production usage](https://redis.io/docs/latest/develop/clients/nodejs/produsage/)

</details>

<details>
<summary>What is the role of a Redis client library?</summary>

### What is the role of a Redis client library?

**Definition:**

A Redis client library speaks the Redis protocol for your language: it opens the socket, writes commands, and parses replies. Your code calls functions instead of formatting RESP by hand.

**How it works:**

- The Node.js connect guide uses `createClient`, `connect`, `set`, and `get`. The library also attaches an error handler for socket failures.
- Transactions and pipelines are client helpers. The Node.js transactions page shows `client.watch`, then `client.multi().set(...).exec()`.
- In production, `node-redis` reconnects by default and queues commands during the outage unless you set `disableOfflineQueue: true`.
- The library does not choose your data model. Keys, TTL, and `MULTI` versus `EVAL` stay application decisions.

**Examples:**

```javascript
await client.watch('shellpath');
await client.multi().set('shellpath', newPath).exec();
```

**Sources:**

- [Connect with node-redis](https://redis.io/docs/latest/develop/clients/nodejs/connect/)
- [Transactions and pipelines](https://redis.io/docs/latest/develop/clients/nodejs/transpipe/)
- [Production usage](https://redis.io/docs/latest/develop/clients/nodejs/produsage/)

</details>

<details>
<summary>Explain how you can integrate Redis with a web application.</summary>

### Explain how you can integrate Redis with a web application.

**Definition:**

The web process keeps a Redis client for the life of the process, uses it for ephemeral data, and leaves durable records in the primary database.

**How it works:**

- Connect once with `createClient` and `connect`, and listen for `error`. Do not open a new TCP connection per HTTP request.
- Typical keys are a session (`SET` plus `EXPIRE`), a cache entry, or a rate-limit counter (`INCR` and `EXPIRE` on the first increment).
- Pass `username` and `password` that match an ACL user, and TLS options when the security guide requires them.
- If a command must not run twice after a reconnect, set `disableOfflineQueue: true` so the client does not replay a queued write.

**When to use:**

Put shared, short-lived state in Redis: sessions, counters, and queues. Keep the system of record in the database that matches how you query it.

**Examples:**

```javascript
await client.connect();
await client.set('session:abc', 'user-id=7');
```

**Sources:**

- [Connect with node-redis](https://redis.io/docs/latest/develop/clients/nodejs/connect/)
- [Production usage](https://redis.io/docs/latest/develop/clients/nodejs/produsage/)
- [Security](https://redis.io/docs/latest/operate/oss_and_stack/management/security/)

</details>

<details>
<summary>How do you handle reconnection logic when the Redis server becomes unavailable?</summary>

### How do you handle reconnection logic when the Redis server becomes unavailable?

**Definition:**

`node-redis` reconnects by default. Commands issued while the socket is down are queued and sent when the connection returns, which can run a write twice.

**How it works:**

- The production guide says a non-idempotent command can change data and still stay in the offline queue. After reconnect the client sends it again.
- Set `disableOfflineQueue: true` on the client that must not replay. Unexecuted commands are discarded instead of queued.
- Use a separate connection with the queue disabled when only some commands are unsafe to retry.
- Keep an `error` listener. After Sentinel failover, ask `SENTINEL get-master-addr-by-name` again, or use a client that follows Sentinel.

**When to use:**

Leave the default queue for idempotent reads if a short outage should be invisible. Disable it for increments, list pops, and any write you cannot safely run twice.

**Examples:**

```javascript
const client = createClient({
  disableOfflineQueue: true
});
```

**Sources:**

- [Production usage](https://redis.io/docs/latest/develop/clients/nodejs/produsage/)
- [High availability with Redis Sentinel](https://redis.io/docs/latest/operate/oss_and_stack/management/sentinel/)

</details>

<details>
<summary>Discuss how Redis can be used in conjunction with a database.</summary>

### Discuss how Redis can be used in conjunction with a database.

**Definition:**

Redis sits beside the database as the fast, in-memory side: cache, session, counter, or queue. The database keeps the durable copy you query with SQL or your document model.

**How it works:**

- On a cache read, the application loads from Redis first. On a miss it reads the database and writes the value back to Redis with a TTL (`EXPIRE`) or under `maxmemory` plus an eviction policy.
- A write goes to the database, then the application deletes or replaces the Redis key so the next read is not stale.
- Sessions and rate limits (`INCR` / `EXPIRE`) can live only in Redis because losing them is bounded by the TTL.
- Replication and AOF protect the Redis side. They do not replace the database backup.

**When to use:**

Use Redis for data that is hot, shared, and either rebuildable or short-lived. Use the database for data you must query ad hoc and keep if Redis is empty.

**Examples:**

```redis
GET user:7
SET user:7 "{"name":"ada"}"
EXPIRE user:7 60
```

**Sources:**

- [Eviction](https://redis.io/docs/latest/develop/reference/eviction/)
- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [`INCR`](https://redis.io/docs/latest/commands/incr/)

</details>

<details>
<summary>What are the trade-offs of using Redis vs. traditional databases?</summary>

### What are the trade-offs of using Redis vs. traditional databases?

**Definition:**

Redis keeps the working set in memory and offers specific data types and atomic commands. A traditional database keeps data on disk and answers declarative queries.

**How it works:**

- Redis commands such as `INCR`, `ZADD`, and `XADD` run against one key (or a script / `MULTI` batch) without a SQL planner.
- Durability is optional: RDB can lose writes since the last snapshot; `appendfsync everysec` can lose about one second; `appendfsync always` fsyncs each append batch and is much slower.
- There is no rollback inside `MULTI`/`EXEC`. There is no join. Secondary lookup is another key you maintain, or a module.
- Memory is the capacity limit unless you evict. `noeviction` errors instead of deleting keys. A disk database grows with disk.
- Replication and Sentinel or Cluster add availability. They follow Redis rules for lag and hash slots, not multi-row transactions.

**When to use:**

Choose Redis when the access pattern is a key and a type operation and the set fits in RAM. Choose the traditional database when you need durable tables larger than memory and ad hoc queries.

**Examples:**

```redis
INCR views:post:1
```

**Sources:**

- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [Transactions](https://redis.io/docs/latest/develop/using-commands/transactions/)
- [Eviction](https://redis.io/docs/latest/develop/reference/eviction/)
- [Data types](https://redis.io/docs/latest/develop/data-types/)

</details>

<details>
<summary>How does Redis interact with other components in a microservices architecture?</summary>

### How does Redis interact with other components in a microservices architecture?

**Definition:**

Each service that needs shared ephemeral state talks to Redis with its own client and its own ACL user. Redis does not call the services.

**How it works:**

- A service publishes with Pub/Sub or appends a stream with `XADD`. Another service subscribes or uses `XREADGROUP`.
- A queue worker blocks on `BLPOP` or `BLMOVE` so it waits without a polling loop.
- Sessions and rate limits are keys any instance of the service can read, which is why the client reconnects as described in the Node.js production guide.
- Give each service `ACL SETUSER` rules for its key prefix. Do not share a user that can run `@dangerous` commands.
- For one primary, Sentinel tells clients the current address. For sharded data, the client must follow cluster slots.

**When to use:**

Use Redis between services for a queue, a cache, or a signal. Keep each service’s durable data in that service’s database.

**Examples:**

```redis
XADD jobs * type resize id 42
BLPOP jobs 5
```

**Sources:**

- [Streams](https://redis.io/docs/latest/develop/data-types/streams/)
- [Pub/Sub](https://redis.io/docs/latest/develop/pubsub/)
- [ACL](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/)
- [Scale with Redis Cluster](https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/)

</details>

### Redis Persistence and Durability

<details>
<summary>What mechanisms does Redis provide for durability?</summary>

### What mechanisms does Redis provide for durability?

**Definition:**

Durability is optional and comes from RDB snapshots, the append-only file, and replication to another node.

**How it works:**

- RDB: Redis forks, the child writes a temporary file, and that file replaces the previous `dump.rdb`.
- AOF: `appendonly yes` logs every write command and replays the log on startup.
- `appendfsync` chooses how often that log is fsynced: `always`, `everysec`, or `no`.
- `REPLICAOF host port` copies the dataset to a replica. A replica is not a crash-safe disk copy of the primary unless that replica also persists.
- `MULTI`/`EXEC` is written to the AOF with a single `write(2)`. A hard crash can still leave a partial transaction; `redis-check-aof` removes it so the server can start.

**When to use:**

Enable AOF when losing the last snapshot is too much. Add replication when you also need another host. Leave persistence off only for a pure cache you can rebuild.

**Examples:**

```redis
CONFIG SET appendonly yes
```

**Sources:**

- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [Replication](https://redis.io/docs/latest/operate/oss_and_stack/management/replication/)
- [Transactions](https://redis.io/docs/latest/develop/using-commands/transactions/)

</details>

<details>
<summary>How do you control when Redis writes data to disk?</summary>

### How do you control when Redis writes data to disk?

**Definition:**

Disk writes are controlled by the RDB `save` rules and by `appendfsync` on the AOF. You can also request a snapshot with `BGSAVE` or `SAVE`.

**How it works:**

- `save 60 1000` writes an RDB if at least 1000 keys changed within 60 seconds. The child process writes the file after a fork.
- `appendfsync always` fsyncs every time commands are appended. The page calls this very slow and very safe. The fsync happens once per batch of commands from clients or a pipeline, before replies are sent.
- `appendfsync everysec` fsyncs every second. It is described as fast enough, and you can lose one second of data.
- `appendfsync no` never fsyncs.
- `CONFIG SET appendonly yes` turns the AOF on at runtime.

**When to use:**

Use `everysec` when a second of loss is acceptable. Use `always` when that second is not acceptable and you can pay the latency. Use RDB `save` points when you want compact snapshots more than a command log.

**Examples:**

```redis
CONFIG SET appendonly yes
```

**Sources:**

- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [`CONFIG SET`](https://redis.io/docs/latest/commands/config-set/)
- [`BGSAVE`](https://redis.io/docs/latest/commands/bgsave/)

</details>

<details>
<summary>Compare the pros and cons of RDB and AOF.</summary>

### Compare the pros and cons of RDB and AOF.

**Definition:**

RDB is a compact point-in-time snapshot taken by a forked child. AOF is a log of write commands replayed on startup.

**How it works:**

- RDB advantages: one binary file, and the fork lets the parent keep serving while the child writes. You lose every write after the last successful snapshot.
- AOF advantages: `appendfsync always` keeps every appended batch, and `everysec` limits loss to about one second. The file is a command log, so Redis rewrites it (`BGREWRITEAOF` is the command the persistence guide tells you to run when creating an AOF).
- AOF cost: more disk IO, especially with `always`. A crash can leave a partial `MULTI`/`EXEC` group; Redis exits on startup until `redis-check-aof` removes it.
- The guide’s stance: snapshotting alone is not very durable. You can enable both. `CONFIG SET appendonly yes` starts AOF without a restart.

**When to use:**

Prefer RDB when restart speed and small backups matter more than the last seconds of writes. Prefer AOF, usually `everysec`, when those writes must survive a crash. Use both when you want a fast snapshot and a tighter log.

**Examples:**

```redis
SAVE
CONFIG SET appendonly yes
```

**Sources:**

- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [`SAVE`](https://redis.io/docs/latest/commands/save/)
- [Transactions](https://redis.io/docs/latest/develop/using-commands/transactions/)

</details>

<details>
<summary>How do you force a Redis data dump to disk?</summary>

### How do you force a Redis data dump to disk?

**Definition:**

`SAVE` writes an RDB snapshot in the foreground. `BGSAVE` writes it in a child process so the server can keep serving.

**How it works:**

- The upgrade notes run `redis-cli SAVE` to create or update `dump.rdb` in the data directory.
- Snapshotting forks: the child writes a temporary RDB and replaces the old file when it finishes.
- If AOF is the file you need instead, `CONFIG SET appendonly yes` starts it. The persistence guide also uses `BGREWRITEAOF` when you are creating the append-only file.
- A foreground `SAVE` blocks command processing until the dump finishes. Use it when you can accept that pause, such as just before a controlled shutdown.

**Examples:**

```redis
SAVE
BGSAVE
```

**Sources:**

- [`SAVE`](https://redis.io/docs/latest/commands/save/)
- [`BGSAVE`](https://redis.io/docs/latest/commands/bgsave/)
- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)

</details>

<details>
<summary>What strategies are involved in ensuring data durability in Redis?</summary>

### What strategies are involved in ensuring data durability in Redis?

**Definition:**

Durability is a choice of how much data you can lose: snapshot interval, AOF fsync policy, a replica, and a copy of the files off the host.

**How it works:**

- Tune RDB with `save` rules, or call `BGSAVE` before a risky operation. Copy `dump.rdb` somewhere else; the persistence guide says to put that backup in a safe place.
- Turn on AOF with `appendonly yes`. Pick `appendfsync always` or `everysec`. `no` does not fsync.
- Before a restart, wait until AOF rewrite is idle: `INFO persistence` with `aof_rewrite_in_progress` and `aof_rewrite_scheduled` at `0`, and `aof_last_bgrewrite_status` ok. That check is in the persistence guide.
- Replicate with `REPLICAOF` so a second host has the dataset. Persist on that host too if the replica must survive on its own.
- If Redis will not start because a transaction was torn in the AOF, run `redis-check-aof` as the transactions page describes.

**When to use:**

Match the fsync policy to the loss you can accept. Ship the RDB or AOF off the box. A replica on the same disk does not save you from that disk failing.

**Examples:**

```redis
CONFIG SET appendonly yes
INFO persistence
```

**Sources:**

- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [Replication](https://redis.io/docs/latest/operate/oss_and_stack/management/replication/)
- [Transactions](https://redis.io/docs/latest/develop/using-commands/transactions/)

</details>

### Redis Troubleshooting & Best Practices

<details>
<summary>How do you troubleshoot latency issues in Redis?</summary>

### How do you troubleshoot latency issues in Redis?

**Definition:**

Latency troubleshooting starts with `LATENCY DOCTOR` and `SLOWLOG GET`, then the memory and fork behavior the doctor output points at.

**How it works:**

- `LATENCY DOCTOR` reports spikes (count, average, worst) and tells you to lower `slowlog-log-slower-than` when the slow log is quieter than the latency monitor. The sample uses `CONFIG SET slowlog-log-slower-than 1000`.
- `SLOWLOG GET` shows the commands that were slow, such as `KEYS` on a large keyspace.
- The same report says deleting, expiring, or evicting a large object blocks Redis. Split that object.
- An RDB or AOF rewrite forks. On a write-heavy instance the copy-on-write can push memory toward twice the usual size and stall the host.
- The admin guide also disables Transparent Huge Pages and sets `vm.overcommit_memory = 1` so background saves are not fighting the kernel.

**Examples:**

```redis
LATENCY DOCTOR
SLOWLOG GET 16
CONFIG SET slowlog-log-slower-than 1000
```

**Sources:**

- [`LATENCY DOCTOR`](https://redis.io/docs/latest/commands/latency-doctor/)
- [`SLOWLOG GET`](https://redis.io/docs/latest/commands/slowlog-get/)
- [Administration](https://redis.io/docs/latest/operate/oss_and_stack/management/admin/)

</details>

<details>
<summary>What are some best practices for error handling in Redis clients?</summary>

### What are some best practices for error handling in Redis clients?

**Definition:**

Handle the client `error` event, decide whether offline commands may be replayed, and check Redis replies that signal a failed write rather than a transport error.

**How it works:**

- The Node.js connect sample registers `client.on('error', ...)`.
- Default reconnect queues commands. A write that already ran can be sent again. Set `disableOfflineQueue: true` when that replay is unsafe.
- `EXEC` returns an array of per-command replies. Redis does not roll back a command that failed inside the transaction, so inspect every element.
- With `noeviction`, a write can return an error because `maxmemory` is exhausted. That is an application error, not a dropped socket.
- After a Sentinel failover, read `SENTINEL get-master-addr-by-name` again instead of caching one host forever.

**Examples:**

```javascript
client.on('error', (err) => console.log('Redis Client Error', err));
const clientSafe = createClient({ disableOfflineQueue: true });
```

**Sources:**

- [Connect with node-redis](https://redis.io/docs/latest/develop/clients/nodejs/connect/)
- [Production usage](https://redis.io/docs/latest/develop/clients/nodejs/produsage/)
- [Transactions](https://redis.io/docs/latest/develop/using-commands/transactions/)
- [Eviction](https://redis.io/docs/latest/develop/reference/eviction/)

</details>

<details>
<summary>How do you recover a Redis instance from a failure?</summary>

### How do you recover a Redis instance from a failure?

**Definition:**

Recovery is either restarting from RDB and AOF, or promoting a replica. A torn transaction in the AOF is repaired before the server will start.

**How it works:**

- On startup Redis loads `dump.rdb` and, if `appendonly` is on, replays the AOF.
- If a crash cut a `MULTI`/`EXEC` group in the AOF, Redis exits with an error. `redis-check-aof` removes that partial transaction so the server can start. That behavior is documented on the transactions page.
- `REPLICAOF NO ONE` on a replica promotes it when the primary is gone and you are failing over by hand.
- With Sentinel, clients call `SENTINEL get-master-addr-by-name` and use the address Sentinel already promoted.
- In Cluster, a replica takes the primary’s slots. `CLUSTER FAILOVER` on a replica forces that by hand.

**When to use:**

Prefer a promoted replica when the host is dead and a replica is caught up enough. Prefer a restart from AOF when the same host’s disk is intact.

**Examples:**

```redis
REPLICAOF NO ONE
SENTINEL get-master-addr-by-name mymaster
```

**Sources:**

- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [Transactions](https://redis.io/docs/latest/develop/using-commands/transactions/)
- [`REPLICAOF`](https://redis.io/docs/latest/commands/replicaof/)
- [Sentinel](https://redis.io/docs/latest/operate/oss_and_stack/management/sentinel/)

</details>

<details>
<summary>What is meant by "hot" keys in Redis and how do you deal with them?</summary>

### What is meant by "hot" keys in Redis and how do you deal with them?

**Definition:**

A hot key is one key that receives a disproportionate share of commands. Because command execution is concentrated on that key, it caps throughput and shows up as latency.

**How it works:**

- `SLOWLOG GET` shows commands that took too long. A hot key often appears there when the value is huge (`HGETALL`-style reads are what the troubleshooting notes tell you to look for) or when every client hammers the same key.
- `LATENCY DOCTOR` calls out large objects that block Redis when they are deleted, expired, or evicted. Split the value into smaller keys.
- `MEMORY USAGE key` estimates how big that one key is.
- Reads of a hot key can move to a replica (`REPLICAOF`, replica is read-only by default). Writes still land on the primary, so a write-hot key is not solved by replicas.
- In Cluster, one key is one slot: CRC16(key) modulo 16384. All traffic for that key stays on one node.

**When to use:**

Split a large value, and spread writes across more than one key when the workload allows. Use a replica only for read traffic.

**Examples:**

```redis
MEMORY USAGE hot:key
SLOWLOG GET 16
```

**Sources:**

- [`SLOWLOG GET`](https://redis.io/docs/latest/commands/slowlog-get/)
- [`LATENCY DOCTOR`](https://redis.io/docs/latest/commands/latency-doctor/)
- [`MEMORY USAGE`](https://redis.io/docs/latest/commands/memory-usage/)
- [Scale with Redis Cluster](https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/)

</details>

<details>
<summary>How do you upgrade a Redis instance with minimal downtime?</summary>

### How do you upgrade a Redis instance with minimal downtime?

**Definition:**

Upgrade a replica first, then fail over so clients move before you restart the old primary. Take a snapshot so you can go back.

**How it works:**

- `redis-cli SAVE` refreshes `dump.rdb` before you touch the instance. The upgrade notes use that command.
- If AOF is on, wait until rewrite is finished: `INFO persistence` shows `aof_rewrite_in_progress` and `aof_rewrite_scheduled` as `0`, and `aof_last_bgrewrite_status` is ok.
- Point a new process at the primary with `REPLICAOF host port`, let it catch up, then promote it with `REPLICAOF NO ONE` or let Sentinel promote it.
- In Cluster, `CLUSTER FAILOVER` on an upgraded replica moves that primary’s slots without adding a new empty node first.
- After restart, the persistence guide says to check that the key count matches what you had.

**When to use:**

Fail over to an already-upgraded replica when you have one. A standalone instance still needs `SAVE` or a finished AOF and a short restart.

**Examples:**

```redis
SAVE
INFO persistence
REPLICAOF 127.0.0.1 6799
```

**Sources:**

- [`SAVE`](https://redis.io/docs/latest/commands/save/)
- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [`REPLICAOF`](https://redis.io/docs/latest/commands/replicaof/)
- [`CLUSTER FAILOVER`](https://redis.io/docs/latest/commands/cluster-failover/)

</details>

### Redis Application Scenarios

<details>
<summary>How can Redis be used for rate limiting?</summary>

### How can Redis be used for rate limiting?

**Definition:**

A rate limiter counts requests in Redis for a key such as a user id and a time window, and rejects the call when the count passes a limit.

**How it works:**

- The docs’ fixed window uses `INCR`. If the result is `1`, it sets `EXPIRE`. If the count is above the limit, the request is rejected. A Lua script does the `INCR`, `EXPIRE`, and `PTTL` check in one `EVAL` so the steps are not interleaved.
- A list version on the `INCR` page uses `LLEN`, then `RPUSH` plus `EXPIRE` inside `MULTI`/`EXEC` when the key is new, or `RPUSHX` when it already exists.
- A sliding window in the rate-limit guide stores each hit in a sorted set: `ZREMRANGEBYSCORE` drops old scores, `ZCARD` counts the rest, `ZADD` records the new hit.
- `INCREX` with `UBOUND` and `EX` is documented as an atomic increment that also caps the counter.

**When to use:**

Use the fixed window when a per-minute counter is enough. Use the sorted-set window when the limit must slide with time. Put the check in `EVAL` so `INCR` and `EXPIRE` cannot split across clients.

**Examples:**

```redis
INCR rate_limit:user:42
EXPIRE rate_limit:user:42 60
```

**Sources:**

- [`INCR`](https://redis.io/docs/latest/commands/incr/)
- [Scripting with Lua](https://redis.io/docs/latest/develop/programmability/eval-intro/)
- [Sorted sets](https://redis.io/docs/latest/develop/data-types/sorted-sets/)

</details>

<details>
<summary>What would be an ideal use case for Redis queues?</summary>

### What would be an ideal use case for Redis queues?

**Definition:**

A Redis queue is a list or a stream of jobs that workers take one at a time. The ideal job is short, can be retried, and does not need a SQL transaction with the rest of the business data.

**How it works:**

- `BLPOP` waits until a producer `LPUSH` or `RPUSH` adds an element. The waiting client does not block the server.
- `BLMOVE source destination LEFT|RIGHT LEFT|RIGHT timeout` pops from the work list and pushes onto a processing list, so a crash does not only delete the job.
- A stream is the stronger log: `XADD` appends, `XREADGROUP` delivers to one consumer, and the entry stays until the group finishes with it.
- Pub/Sub is a poor queue: subscribers that are disconnected miss the message.

**When to use:**

Use a list with `BLMOVE` for a simple work queue. Use a stream when you need consumer groups and a retained log. Do not use Pub/Sub when the worker may be offline.

**Examples:**

```redis
BLMOVE jobs processing LEFT RIGHT 30
```

**Sources:**

- [`BLPOP`](https://redis.io/docs/latest/commands/blpop/)
- [`BLMOVE`](https://redis.io/docs/latest/commands/blmove/)
- [Streams](https://redis.io/docs/latest/develop/data-types/streams/)

</details>

<details>
<summary>How would you use Redis for full-page cache?</summary>

### How would you use Redis for full-page cache?

**Definition:**

A full-page cache stores the rendered response body in a Redis string and serves it until the key expires or you delete it.

**How it works:**

- `SET` the page body under a key that includes the URL and the bits that change the HTML, such as locale. `EXPIRE` bounds how long a stale page can live.
- On a write that changes that page, delete the key so the next request rebuilds it from the application.
- `maxmemory` plus `allkeys-lru` evicts cold pages when the cache is larger than RAM. `noeviction` is a bad fit: the write of a new page would error instead of dropping an old page.
- The value can be large. `MEMORY USAGE` shows the cost, and `LATENCY DOCTOR` warns that evicting a huge value blocks the server.

**When to use:**

Cache a full page when many requests share the same anonymous HTML. Skip it for personalized pages unless the key includes the user, and keep the TTL short enough that a missed delete is tolerable.

**Examples:**

```redis
SET page:/pricing body
EXPIRE page:/pricing 30
```

**Sources:**

- [Eviction](https://redis.io/docs/latest/develop/reference/eviction/)
- [`MEMORY USAGE`](https://redis.io/docs/latest/commands/memory-usage/)
- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)

</details>

<details>
<summary>Discuss the usage of Redis in machine learning models.</summary>

### Discuss the usage of Redis in machine learning models.

**Definition:**

Redis holds features and results next to the model process: vectors when a vector module is loaded, counters, and recent events in a stream. The model file itself is not a Redis type.

**How it works:**

- `MODULE LIST` on a Redis Stack install can include `vectorset` and `search`. Those modules are what add vector-style commands; core Redis does not document a tensor type.
- A feature that is a number or a small hash is a normal key. `INCR` updates a counter feature. A sorted set ranks ids by a score the model produced.
- A stream (`XADD`) keeps an ordered log of events the feature job reads with `XREAD`.
- Memory is the limit. `maxmemory` and `MEMORY USAGE` tell you whether the feature set fits.

**When to use:**

Use Redis for online features and a vector index supplied by a module. Keep training data and the model artifact in storage built for large files.

**Examples:**

```redis
MODULE LIST
XADD events * user 7 item 9
```

**Sources:**

- [`MODULE LIST`](https://redis.io/docs/latest/commands/module-list/)
- [Streams](https://redis.io/docs/latest/develop/data-types/streams/)
- [Sorted sets](https://redis.io/docs/latest/develop/data-types/sorted-sets/)

</details>

<details>
<summary>Can Redis be used in a real-time analytics platform? How?</summary>

### Can Redis be used in a real-time analytics platform? How?

**Definition:**

Yes, for the hot counters and approximate uniques that dashboards read every second. Redis is the rolling window, not the warehouse.

**How it works:**

- `INCR` counts events in the current window. `EXPIRE` drops the window when it is over.
- `PFADD` / `PFCOUNT` estimate distinct ids without storing each one. `PFMERGE` combines windows.
- A sorted set ranks a top-N list with `ZADD` and `ZRANGE`.
- A stream retains the raw events (`XADD`, `XREAD`) for a short time so a worker can fold them into the counters.
- `INFO stats` exposes `keyspace_hits` and `keyspace_misses` if the dashboard itself is cached.

**When to use:**

Keep the last minutes in Redis. Ship older events to a database that can scan history. Use HyperLogLog only when an approximate unique count is acceptable.

**Examples:**

```redis
INCR views:page:9
PFADD uv:page:9 user:7
PFCOUNT uv:page:9
```

**Sources:**

- [`INCR`](https://redis.io/docs/latest/commands/incr/)
- [HyperLogLog](https://redis.io/docs/latest/develop/data-types/probabilistic/hyperloglogs/)
- [Streams](https://redis.io/docs/latest/develop/data-types/streams/)
- [Eviction](https://redis.io/docs/latest/develop/reference/eviction/)

</details>

<details>
<summary>How is Redis used in IoT (Internet of Things) applications?</summary>

### How is Redis used in IoT (Internet of Things) applications?

**Definition:**

Devices write small, frequent updates into Redis keys, streams, or counters. Redis is the live state, not the long-term archive of every sensor sample.

**How it works:**

- A latest reading is a string or hash updated with `SET`. `EXPIRE` drops a device that stopped reporting.
- A stream (`XADD`) keeps an ordered log of readings a worker consumes with `XREAD`.
- `INCR` counts events per device per window. `PFADD` estimates distinct devices if an exact set would be too large.
- `GEOADD` stores a device position when you need `GEOSEARCH` nearby.
- Shard with Cluster when the key count no longer fits one primary. One device id is one slot.

**When to use:**

Use Redis for the current value and a short log. Persist the history you must keep to a database, and set `maxmemory` so a burst of devices cannot exhaust the host.

**Examples:**

```redis
XADD telemetry * device 18 temp 21.5
GEOADD devices -122.4 37.8 sensor-18
```

**Sources:**

- [Streams](https://redis.io/docs/latest/develop/data-types/streams/)
- [`GEOSEARCH`](https://redis.io/docs/latest/commands/geosearch/)
- [Scale with Redis Cluster](https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/)

</details>

<details>
<summary>What role does Redis play in gaming applications?</summary>

### What role does Redis play in gaming applications?

**Definition:**

Redis holds live match state that many game servers must read and update in one command: presence, scores, and short-lived sessions.

**How it works:**

- A leaderboard is a sorted set: `ZADD board score player` and `ZRANGE board 0 9` for the top of the list.
- Presence or inventory flags can be a set (`SADD`) or a bitmap (`SETBIT`) when the id is an integer offset.
- A matchmaking queue can block with `BLPOP` until another player is pushed.
- `MULTI`/`EXEC` updates two keys, such as a player score and a match counter, without another client running in the middle. There is still no rollback.
- Replicas serve read-only scoreboards. Writes stay on the primary.

**When to use:**

Use Redis for state that expires with the match or the session. Store the account and purchase history in a durable database.

**Examples:**

```redis
ZADD board 10 alice
ZRANGE board 0 9
```

**Sources:**

- [Sorted sets](https://redis.io/docs/latest/develop/data-types/sorted-sets/)
- [`BLPOP`](https://redis.io/docs/latest/commands/blpop/)
- [Transactions](https://redis.io/docs/latest/develop/using-commands/transactions/)
- [Replication](https://redis.io/docs/latest/operate/oss_and_stack/management/replication/)

</details>

<details>
<summary>How would you design a recommendation system using Redis?</summary>

### How would you design a recommendation system using Redis?

**Definition:**

A recommendation list in Redis is a sorted set of item ids per user, or a set of co-occurring items. The score is whatever your job computed.

**How it works:**

- `ZADD user:7:recs score item` stores the ranked list. `ZRANGE user:7:recs 0 19` reads the top items.
- Co-visitation is a set: `SADD item:9:also item:4` and `SINTER` when you combine several items the user already touched.
- `EXPIRE` drops a list you will recompute. A stream of clicks (`XADD`) feeds the job that rewrites the sorted set.
- If you need vector search, `MODULE LIST` must show a module such as `vectorset` or `search`. Core commands do not search embeddings.
- One user’s key is one cluster slot, so a very hot user stays on one node.

**When to use:**

Precompute the list into a sorted set when the homepage must answer in one `ZRANGE`. Compute it in the application when the candidate set is the result of a database query Redis does not store.

**Examples:**

```redis
ZADD user:7:recs 0.9 item:4
ZRANGE user:7:recs 0 19
```

**Sources:**

- [Sorted sets](https://redis.io/docs/latest/develop/data-types/sorted-sets/)
- [Sets](https://redis.io/docs/latest/develop/data-types/sets/)
- [`MODULE LIST`](https://redis.io/docs/latest/commands/module-list/)
- [Streams](https://redis.io/docs/latest/develop/data-types/streams/)

</details>

### Redis and Cloud Infrastructures

<details>
<summary>What are the benefits of running Redis on cloud platforms?</summary>

### What are the benefits of running Redis on cloud platforms?

**Definition:**

A cloud VM or managed Redis still runs the same server: you get a host that can restart somewhere else, and you still configure memory, persistence, replication, and ACLs.

**How it works:**

- `maxmemory` must sit under the instance size. The admin guide says to subtract overhead from free RAM and to expect up to about 2× memory during RDB or AOF rewrite.
- Replication (`REPLICAOF`) and Sentinel or Cluster are how you survive a host failure. The cloud does not change those commands.
- Snapshots are still `BGSAVE` / `dump.rdb` or AOF. Copy them to durable object storage; a disk on the same VM is not a backup.
- Security is still protected mode, ACL, and TLS, as on the security page. A public cloud address makes that mandatory.

**When to use:**

Use the cloud when you want another availability zone for the replica and a place to store `dump.rdb`. You still choose `appendfsync`, `maxmemory-policy`, and who can run `@dangerous` commands.

**Examples:**

```redis
REPLICAOF 10.0.0.12 6379
CONFIG SET appendonly yes
```

**Sources:**

- [Administration](https://redis.io/docs/latest/operate/oss_and_stack/management/admin/)
- [Replication](https://redis.io/docs/latest/operate/oss_and_stack/management/replication/)
- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [Security](https://redis.io/docs/latest/operate/oss_and_stack/management/security/)

</details>

<details>
<summary>How is AWS Elasticache different from self-hosted Redis?</summary>

### How is AWS Elasticache different from self-hosted Redis?

**Definition:**

Self-hosted Redis means you run `redis-server`, edit persistence and `maxmemory`, and operate Sentinel or Cluster yourself. ElastiCache is Amazon’s managed service in front of Redis; Redis’s own docs do not define its console or parameter names.

**How it works:**

- On a host you control, `REPLICAOF`, `CONFIG SET appendonly`, `BGSAVE`, and `ACL SETUSER` are the controls in the Redis docs.
- `maxmemory` and `maxmemory-policy` still decide eviction. A managed service applies those through its own settings screen, but the Redis behavior is the one on the eviction page.
- Failover is still either Sentinel (`SENTINEL get-master-addr-by-name`) or Cluster (`CLUSTER FAILOVER`), unless the provider hides that behind an endpoint.
- You do not get a different data type or a different `INCR`. Clients use the same protocol the Node.js connect guide shows.

**When to use:**

Choose self-hosted when you need the exact `redis.conf` knobs and can run the host. Choose a managed endpoint when you want someone else to run the process, and confirm in that product’s docs which of `appendfsync`, ACL, and Cluster it exposes.

**Examples:**

```redis
INFO replication
ACL SETUSER alice on >p1pp0 ~cached:* +get
```

**Sources:**

- [Replication](https://redis.io/docs/latest/operate/oss_and_stack/management/replication/)
- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [Sentinel](https://redis.io/docs/latest/operate/oss_and_stack/management/sentinel/)
- [ACL](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/)

</details>

<details>
<summary>Discuss the considerations for using Redis within containerized environments like Kubernetes.</summary>

### Discuss the considerations for using Redis within containerized environments like Kubernetes.

**Definition:**

A container limit is a hard memory cap. Redis must hit `maxmemory` and evict or error before the runtime kills the pod, and a fork during save needs spare memory.

**How it works:**

- The admin guide says to set `maxmemory` below free RAM after overhead and fragmentation. In a container that free RAM is the pod limit, not the node’s full size.
- RDB or AOF rewrite on a write-heavy instance can use up to about twice the normal memory because of copy-on-write. A limit with no headroom gets the process killed mid-fork.
- Set `vm.overcommit_memory = 1` and disable Transparent Huge Pages, as the admin guide requires for background saves. Those are node kernel settings, not pod settings.
- Persist `dump.rdb` or the AOF directory on a volume. An empty container disk disappears with the pod.
- Use a replica (`REPLICAOF`) or Cluster so one pod restart is not the whole dataset.

**When to use:**

Size the pod for the dataset plus fork overhead when persistence is on. Use `noeviction` only if a failed write is better than a missing key and the limit still protects the node.

**Examples:**

```redis
maxmemory 2mb
maxmemory-policy allkeys-lru
```

**Sources:**

- [Administration](https://redis.io/docs/latest/operate/oss_and_stack/management/admin/)
- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [Replication](https://redis.io/docs/latest/operate/oss_and_stack/management/replication/)

</details>

<details>
<summary>How do you handle Redis persistence in cloud environments?</summary>

### How do you handle Redis persistence in cloud environments?

**Definition:**

Cloud persistence is the same RDB and AOF configuration, plus a copy of those files that survives the instance.

**How it works:**

- `appendonly yes` and `appendfsync everysec` or `always` control the AOF. `save` rules or `BGSAVE` control snapshots.
- `SAVE` updates `dump.rdb` in the data directory. Copy that file, or the `appendonly.aof.*` files, off the instance. The persistence guide says to keep the RDB backup in a safe place.
- Wait for AOF rewrite to finish (`INFO persistence`, rewrite counters at 0) before you stop the instance to snapshot the disk.
- A replica in another zone (`REPLICAOF`) is availability. It is durability only if that replica also has AOF or RDB on its own disk.
- Expect the extra memory of a fork, documented in the admin guide, when you size the cloud instance.

**When to use:**

Use `everysec` for most cloud primaries. Copy the finished RDB or AOF to storage outside the instance. Do not treat a single disk snapshot of a live writable primary as a consistent backup unless Redis has finished `BGSAVE` or an AOF rewrite.

**Examples:**

```redis
BGSAVE
INFO persistence
```

**Sources:**

- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [`BGSAVE`](https://redis.io/docs/latest/commands/bgsave/)
- [Administration](https://redis.io/docs/latest/operate/oss_and_stack/management/admin/)

</details>

<details>
<summary>What is the impact of cloud networking on Redis performance?</summary>

### What is the impact of cloud networking on Redis performance?

**Definition:**

Redis answers from memory, so the round trip often costs more than the command. Extra cloud hops, and a replica in another zone, add that delay to every call.

**How it works:**

- Many tiny commands mean many round trips. The Node.js client can pipeline and can run `MULTI`/`EXEC` as one batch; the transactions page also notes that an AOF fsync of `always` happens once per pipeline batch.
- A replica used for reads can be far from the client. Replication does not make that replica as fresh as the primary at the same instant.
- `LATENCY DOCTOR` separates a slow command (`SLOWLOG`) from time spent elsewhere. A fast `GET` that is still slow at the client is usually the network.
- Cluster clients must follow the node that owns the slot. A command sent to the wrong node costs another hop.

**When to use:**

Place the client and the primary in the same zone when write latency matters. Pipeline bursts of small commands. Do not put the only primary across a wide link from the application.

**Examples:**

```redis
LATENCY DOCTOR
MSET a 1 b 2
```

**Sources:**

- [`LATENCY DOCTOR`](https://redis.io/docs/latest/commands/latency-doctor/)
- [Transactions](https://redis.io/docs/latest/develop/using-commands/transactions/)
- [Production usage](https://redis.io/docs/latest/develop/clients/nodejs/produsage/)
- [Scale with Redis Cluster](https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/)

</details>

### Redis Cluster Management

<details>
<summary>How do you add or remove nodes in a Redis Cluster?</summary>

### How do you add or remove nodes in a Redis Cluster?

**Definition:**

You add a node, then move hash slots onto it. You remove a node only after its slots live somewhere else. The cluster keeps serving during the move.

**How it works:**

- There are 16384 slots. Adding node D means moving some slots from the current primaries to D. Removing node A means moving A’s slots to the nodes that remain, then taking A out once it is empty.
- `redis-cli --cluster add-node 127.0.0.1:7006 127.0.0.1:7000` joins 7006 using 7000 as the entry. `--cluster-slave` joins it as a replica instead of an empty primary.
- `CLUSTER NODES` shows whether the new node is connected and which slots it owns. A new primary with no slots does not hold keys yet.
- `CLUSTER SETSLOT` is the command the docs describe for migrating one slot from a source node to a destination node.

**When to use:**

Add a replica with `--cluster-slave` when you need failover capacity. Add a primary and move slots when you need more memory or write capacity. Empty the node before you remove it.

**Examples:**

```redis
redis-cli --cluster add-node 127.0.0.1:7006 127.0.0.1:7000
CLUSTER NODES
```

**Sources:**

- [Scale with Redis Cluster](https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/)
- [`CLUSTER NODES`](https://redis.io/docs/latest/commands/cluster-nodes/)

</details>

<details>
<summary>Explain how failover works in a Redis Cluster.</summary>

### Explain how failover works in a Redis Cluster.

**Definition:**

If a primary fails, one of its replicas is promoted and takes that primary’s hash slots. Clients then use the new primary for those slots.

**How it works:**

- The scaling guide’s failure test stops a primary and shows `CLUSTER NODES` afterward: the old primary’s replica is listed as the master for that slot range.
- During the switch, clients see errors on the slots that are moving. The same test output shows a burst of read and write errors, then commands succeed again.
- `CLUSTER FAILOVER` on a replica forces a manual failover of its primary. `FORCE` does not consult the primary. `TAKEOVER` takes over without the usual checks.
- A replica with no slots of its own does not serve those keys until it is promoted. `CLUSTER NODES` is how you see the new role and the slot range.

**When to use:**

Run at least one replica per primary if a node death must promote automatically. Use `CLUSTER FAILOVER` when you are moving a primary on purpose, such as before an upgrade.

**Examples:**

```redis
CLUSTER FAILOVER
CLUSTER NODES
```

**Sources:**

- [Scale with Redis Cluster](https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/)
- [`CLUSTER FAILOVER`](https://redis.io/docs/latest/commands/cluster-failover/)
- [`CLUSTER NODES`](https://redis.io/docs/latest/commands/cluster-nodes/)

</details>

<details>
<summary>Discuss the challenges of running a Redis Cluster in production.</summary>

### Discuss the challenges of running a Redis Cluster in production.

**Definition:**

Cluster spreads slots across nodes, so operations that assume one keyspace, and failures that affect one node, become part of normal operations.

**How it works:**

- A key’s slot is CRC16(key) modulo 16384. A command that touches keys in different slots is a multi-key operation the cluster must be able to route; the blocking-command notes say clustered behavior can differ and point at the multi-key page.
- Resharding does not stop the cluster, but the failover test shows a window of client errors while a primary is down.
- A hot key stays on one slot and one node. Replicas do not take its writes.
- Fork memory still applies on each primary that runs RDB or AOF. Size every node for copy-on-write, not only the sum of the data.
- `KEYS` is still O(N). In a cluster, a pattern that maps to one slot can limit the search to that slot; a wide pattern does not.

**When to use:**

Use Cluster when you must shard. Keep related keys in one slot only if your client and commands are written for that. Operate with replicas so one node failure promotes instead of dropping those slots.

**Examples:**

```redis
CLUSTER NODES
```

**Sources:**

- [Scale with Redis Cluster](https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/)
- [`KEYS`](https://redis.io/docs/latest/commands/keys/)
- [`BLPOP`](https://redis.io/docs/latest/commands/blpop/)
- [Administration](https://redis.io/docs/latest/operate/oss_and_stack/management/admin/)

</details>

<details>
<summary>What maintenance operations are needed for a Redis Cluster?</summary>

### What maintenance operations are needed for a Redis Cluster?

**Definition:**

Maintenance is checking slot ownership, moving slots when a node is added or emptied, persisting each node, and failing over on purpose before an upgrade.

**How it works:**

- `CLUSTER NODES` shows roles and ranges such as `0-5959`. A node with an empty range holds no keys.
- Adding capacity is `redis-cli --cluster add-node`, then moving slots. `CLUSTER SETSLOT` migrates one slot from source to destination.
- Each primary still needs its own `BGSAVE` or AOF. `INFO persistence` on that node tells you if a rewrite is in progress.
- `CLUSTER FAILOVER` on a replica is the manual switch before you restart its primary.
- `SLOWLOG GET` and `LATENCY DOCTOR` are per node. A slow command on one primary does not appear on the others.

**Examples:**

```redis
CLUSTER NODES
CLUSTER FAILOVER
BGSAVE
```

**Sources:**

- [Scale with Redis Cluster](https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/)
- [`CLUSTER FAILOVER`](https://redis.io/docs/latest/commands/cluster-failover/)
- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)

</details>

<details>
<summary>How do you shard data in a Redis Cluster?</summary>

### How do you shard data in a Redis Cluster?

**Definition:**

Redis Cluster shards by hash slot. Every key belongs to one of 16384 slots, and each primary owns a subset of those slots.

**How it works:**

- The slot is CRC16 of the key modulo 16384. This is not consistent hashing.
- The scaling guide’s example: node A has slots 0–5500, B has 5501–11000, C has 11001–16383.
- You change ownership by moving slots. Adding a node moves some slots to it. Removing a node moves its slots away. The cluster does not have to stop.
- `CLUSTER NODES` prints the range each primary serves, for example `0-5959`.
- A single key cannot be split across nodes. A hot key is a hot slot.

**When to use:**

Let the cluster place keys when access is spread out. Move slots when one node’s memory or CPU is the bottleneck. Do not expect one busy key to scale horizontally.

**Examples:**

```redis
CLUSTER NODES
```

**Sources:**

- [Scale with Redis Cluster](https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/)
- [`CLUSTER NODES`](https://redis.io/docs/latest/commands/cluster-nodes/)

</details>

### Redis from a Developer's Perspective

<details>
<summary>What are idiomatic ways to use Redis in software development?</summary>

### What are idiomatic ways to use Redis in software development?

**Definition:**

Idiomatic Redis uses the type that matches the access, a TTL or `maxmemory` when the data is a cache, and a script or `MULTI` when several commands must stay together.

**How it works:**

- Counters: `INCR`. Unique members: `SADD`. Ranked members: `ZADD` and `ZRANGE`. Bits: `SETBIT`. Logs: `XADD`.
- Approximate uniques: `PFADD` / `PFCOUNT`, not a giant set.
- Queues: `BLMOVE` or a stream with `XREADGROUP`, not Pub/Sub, when the worker must not miss a job.
- Share a process-wide client (`createClient` / `connect`). Pipeline or `MULTI` a burst instead of one round trip per `SET`.
- Iterate with `SCAN`, not `KEYS`, in application code.

**Examples:**

```redis
INCR page:views
SADD article:9:tags redis
SCAN 0 MATCH session:* COUNT 100
```

**Sources:**

- [Data types](https://redis.io/docs/latest/develop/data-types/)
- [`SCAN`](https://redis.io/docs/latest/commands/scan/)
- [`KEYS`](https://redis.io/docs/latest/commands/keys/)
- [Production usage](https://redis.io/docs/latest/develop/clients/nodejs/produsage/)

</details>

<details>
<summary>How do common design patterns apply to Redis usage?</summary>

### How do common design patterns apply to Redis usage?

**Definition:**

The usual patterns are a cache beside a database, a counter, a queue, and pub/sub for signals. Each one maps to a documented command, not to a table.

**How it works:**

- Cache-aside: read Redis, and on a miss read the database and `SET` plus `EXPIRE`. Choose `allkeys-lru` when memory, not the TTL, should bound the cache.
- Rate limit: `INCR` and `EXPIRE`, preferably inside `EVAL`.
- Queue: `BLPOP` or `BLMOVE`. Work log: `XADD` and `XREADGROUP`.
- Fan-out that can drop messages: `SUBSCRIBE`.
- Optimistic update: `WATCH`, then `MULTI` / `SET` / `EXEC`, as in the Node.js transactions example. If `EXEC` aborts, retry the read.

**When to use:**

Pick the pattern from the failure you can accept: a cache may miss, a queue should not, and Pub/Sub may drop.

**Examples:**

```redis
WATCH shellpath
MULTI
SET shellpath /usr/syscmds/:/usr/mycmds/
EXEC
```

**Sources:**

- [Transactions](https://redis.io/docs/latest/develop/using-commands/transactions/)
- [Eviction](https://redis.io/docs/latest/develop/reference/eviction/)
- [`INCR`](https://redis.io/docs/latest/commands/incr/)
- [Streams](https://redis.io/docs/latest/develop/data-types/streams/)

</details>

<details>
<summary>How do you test software that relies on Redis?</summary>

### How do you test software that relies on Redis?

**Definition:**

Point the test process at a Redis you can wipe, exercise the real commands, and assert both the success reply and the failure replies Redis actually returns.

**How it works:**

- Use `createClient` against a local server or a dedicated database index only if your tests do not assume cluster slot behavior. Cluster tests must send keys the way production does.
- Assert `INCR` results, `EXEC` reply arrays, and a nil `BLPOP` when the timeout expires.
- With `disableOfflineQueue: true`, a test can stop Redis and check that the client errors instead of hanging a queued write.
- `SCAN` tests should loop until the cursor comes back to `0`, because one call does not return every key.
- Do not use `KEYS` as the test’s way to list a large fixture if you also claim production never calls it.

**When to use:**

Run integration tests against Redis for commands, TTL, and `MULTI`. Keep pure unit tests on your own functions by passing in a client you control, and still keep one test that hits a real server.

**Examples:**

```javascript
await client.set('foo', 'bar');
const value = await client.get('foo');
```

**Sources:**

- [Connect with node-redis](https://redis.io/docs/latest/develop/clients/nodejs/connect/)
- [Production usage](https://redis.io/docs/latest/develop/clients/nodejs/produsage/)
- [`SCAN`](https://redis.io/docs/latest/commands/scan/)
- [`BLPOP`](https://redis.io/docs/latest/commands/blpop/)

</details>

<details>
<summary>Explain how to use Redis with asynchronous programming models.</summary>

### Explain how to use Redis with asynchronous programming models.

**Definition:**

The Node.js client is asynchronous: `connect`, `set`, and `get` return promises, and the socket reconnects without blocking the event loop on a Redis round trip.

**How it works:**

- `await client.connect()` before commands. Handle `error` so a rejected socket does not surface as an unhandled exception only.
- `client.multi().set(...).exec()` returns a promise. The transactions example waits on that promise and handles the rejection when `WATCH` aborts the batch.
- While the connection is down, commands are queued unless `disableOfflineQueue` is true. Queued commands resolve later, which can surprise an async caller that already moved on.
- Blocking server commands such as `BLPOP` block that connection, not the Redis server. Use a dedicated connection for the worker so the request path does not share it.

**When to use:**

Await each command on the request path. Put `BLPOP` on its own connection. Disable the offline queue for writes your async flow must not run twice.

**Examples:**

```javascript
await client.connect();
const value = await client.get('foo');
await client.multi().set('shellpath', newPath).exec();
```

**Sources:**

- [Connect with node-redis](https://redis.io/docs/latest/develop/clients/nodejs/connect/)
- [Transactions and pipelines](https://redis.io/docs/latest/develop/clients/nodejs/transpipe/)
- [Production usage](https://redis.io/docs/latest/develop/clients/nodejs/produsage/)
- [`BLPOP`](https://redis.io/docs/latest/commands/blpop/)

</details>

<details>
<summary>Discuss the pros and cons of using Redis as a caching layer.</summary>

### Discuss the pros and cons of using Redis as a caching layer.

**Definition:**

A Redis cache avoids a slower database read for hot keys. It can serve stale data, evict keys you still wanted, or refuse writes when memory is full.

**How it works:**

- Pros: `GET` / `SET` are in memory. `EXPIRE` bounds staleness. `allkeys-lru` evicts cold keys without a TTL on each one. `INFO stats` exposes `keyspace_hits` and `keyspace_misses`.
- Cons: a hit can be stale until you delete the key or the TTL ends. `volatile-*` policies do nothing if you forgot the TTL; they then behave like `noeviction`.
- `noeviction` turns the cache into errors once `maxmemory` is reached. Reads of keys that are still there keep working.
- Evicting or expiring a very large cached page blocks the server. `LATENCY DOCTOR` calls that out.
- Persistence and replication are optional for a cache you can rebuild. They cost fork memory and lag.

**When to use:**

Use Redis as a cache when a miss is correct behavior and you can rebuild the value. Do not use it as the only copy unless AOF or RDB is an explicit choice.

**Examples:**

```redis
maxmemory 2mb
maxmemory-policy allkeys-lru
```

**Sources:**

- [Eviction](https://redis.io/docs/latest/develop/reference/eviction/)
- [`INFO`](https://redis.io/docs/latest/commands/info/)
- [`LATENCY DOCTOR`](https://redis.io/docs/latest/commands/latency-doctor/)

</details>

### Redis Monitoring and Logging

<details>
<summary>How do you monitor key events in Redis?</summary>

### How do you monitor key events in Redis?

**Definition:**

Keyspace notifications publish Pub/Sub messages when a key changes. They are off by default because they cost CPU.

**How it works:**

- Enable them with `CONFIG SET notify-keyspace-events`, or the same directive in `redis.conf`. An empty string disables them.
- `K` publishes keyspace events (`__keyspace@<db>__`). `E` publishes keyevent events (`__keyevent@<db>__`). You need at least one of `K` or `E` or nothing is delivered.
- `A` is an alias for `g$lshztdxea`. The string `KEA` enables most events. `x` is expired, `e` is evicted. `m`, `n`, `o`, and `c` are not included in `A`.
- `PSUBSCRIBE __key*__:*` receives them. The docs show `config set notify-keyspace-events KEA` before that subscribe.
- Notifications are not a durability log. A subscriber that is disconnected misses events. A stream is the log if you must keep them.

**When to use:**

Turn on `KEA` when an application must react to deletes and expirations. Leave the feature off when nobody is subscribed. Use a stream if the event itself must be stored.

**Examples:**

```redis
CONFIG SET notify-keyspace-events KEA
PSUBSCRIBE __key*__:*
```

**Sources:**

- [Keyspace notifications](https://redis.io/docs/latest/develop/pubsub/keyspace-notifications/)
- [Pub/Sub](https://redis.io/docs/latest/develop/pubsub/)

</details>

<details>
<summary>What are some effective monitoring tools for Redis?</summary>

### What are some effective monitoring tools for Redis?

**Definition:**

The tools that ship with Redis are `INFO`, `LATENCY DOCTOR`, `SLOWLOG`, `MEMORY DOCTOR`, and keyspace notifications. They do not require a separate product.

**How it works:**

- `INFO memory` and `INFO stats` expose memory, `mem_not_counted_for_evict`, `keyspace_hits`, and `keyspace_misses`.
- `LATENCY DOCTOR` summarizes spikes and points at slow commands and large evictions.
- `SLOWLOG GET n` lists the slow commands themselves.
- `MEMORY DOCTOR` and `MEMORY USAGE key` cover memory.
- `CLUSTER NODES` is the cluster view of who owns which slots. Run the other commands on each node.

**Examples:**

```redis
INFO stats
LATENCY DOCTOR
MEMORY DOCTOR
```

**Sources:**

- [`INFO`](https://redis.io/docs/latest/commands/info/)
- [`LATENCY DOCTOR`](https://redis.io/docs/latest/commands/latency-doctor/)
- [`SLOWLOG GET`](https://redis.io/docs/latest/commands/slowlog-get/)
- [`MEMORY DOCTOR`](https://redis.io/docs/latest/commands/memory-doctor/)
- [Administration](https://redis.io/docs/latest/operate/oss_and_stack/management/admin/)

</details>

<details>
<summary>How do you capture and analyze Redis logs?</summary>

### How do you capture and analyze Redis logs?

**Definition:**

Redis has two different records: the server’s doctor and info output, and the in-memory slow log. The slow log is not a text file you tail.

**How it works:**

- `SLOWLOG GET <count>` returns recent commands that crossed `slowlog-log-slower-than`. `LATENCY DOCTOR` tells you when that threshold is too high and shows a sample `CONFIG SET slowlog-log-slower-than 1000`.
- `INFO persistence` is the log of save state: `aof_rewrite_in_progress`, `aof_rewrite_scheduled`, and `aof_last_bgrewrite_status`.
- `LATENCY DOCTOR` and `MEMORY DOCTOR` print human-readable reports. The latency report is what the command page says you can share when asking for help.
- Key changes are not in that report. `CONFIG SET notify-keyspace-events KEA` plus `PSUBSCRIBE` is the event stream, and it is lossy if you disconnect.

**Examples:**

```redis
SLOWLOG GET 32
INFO persistence
LATENCY DOCTOR
```

**Sources:**

- [`SLOWLOG GET`](https://redis.io/docs/latest/commands/slowlog-get/)
- [`LATENCY DOCTOR`](https://redis.io/docs/latest/commands/latency-doctor/)
- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [Keyspace notifications](https://redis.io/docs/latest/develop/pubsub/keyspace-notifications/)

</details>

<details>
<summary>What can you diagnose from Redis slow query logs?</summary>

### What can you diagnose from Redis slow query logs?

**Definition:**

The slow log lists commands that ran longer than `slowlog-log-slower-than`. It shows what Redis itself spent time on, not time in the network before the command started.

**How it works:**

- `SLOWLOG GET n` is the command the troubleshooting notes use to find slow commands such as `KEYS`.
- `LATENCY DOCTOR` connects the two: if the latency monitor sees spikes but the slow log is empty, the threshold is higher than the spikes. The sample advice is `CONFIG SET slowlog-log-slower-than 1000`.
- A slow `KEYS` is O(N) in the number of keys. The command page says it can ruin performance on a large database and is for debugging, not application code.
- Large deletes also block the server. The doctor text says to fragment big objects when delete, expire, or eviction shows up as latency.
- `ACL CAT dangerous` includes `slowlog`, so a locked-down app user may not be allowed to read it.

**When to use:**

Read the slow log when `LATENCY DOCTOR` mentions commands. If the doctor sees spikes and the slow log does not, lower the threshold before you conclude the network is the cause.

**Examples:**

```redis
SLOWLOG GET 10
CONFIG SET slowlog-log-slower-than 1000
```

**Sources:**

- [`SLOWLOG GET`](https://redis.io/docs/latest/commands/slowlog-get/)
- [`LATENCY DOCTOR`](https://redis.io/docs/latest/commands/latency-doctor/)
- [`KEYS`](https://redis.io/docs/latest/commands/keys/)

</details>

<details>
<summary>How does monitoring differ in a Redis Cluster environment?</summary>

### How does monitoring differ in a Redis Cluster environment?

**Definition:**

Each cluster node is a Redis process with its own memory, slow log, and persistence. A single `INFO` does not describe the other nodes.

**How it works:**

- `CLUSTER NODES` is the map: node id, `master` or `slave`, and the slot range such as `0-5959` or `11423-16383`.
- Run `INFO`, `SLOWLOG GET`, `LATENCY DOCTOR`, and `MEMORY USAGE` on the node that owns the key. A hot key’s slot never moves just because you looked at a different node.
- During failover the scaling guide shows a period of client errors, then a replica listed as master for the old range. Watch `CLUSTER NODES` for that role change.
- `BGSAVE` and `INFO persistence` are per process. One node can be rewriting AOF while another is idle.
- A `SCAN` on one node sees that node’s keys. It is not a cluster-wide key listing.

**When to use:**

Scrape every node. Alert on a primary with replicas at zero and on `maxmemory` on any one node, not on the average of the cluster.

**Examples:**

```redis
CLUSTER NODES
INFO memory
```

**Sources:**

- [`CLUSTER NODES`](https://redis.io/docs/latest/commands/cluster-nodes/)
- [Scale with Redis Cluster](https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/)
- [`INFO`](https://redis.io/docs/latest/commands/info/)
- [`SCAN`](https://redis.io/docs/latest/commands/scan/)

</details>

### Redis in High Availability Systems

<details>
<summary>How does Redis maintain high availability?</summary>

### How does Redis maintain high availability?

**Definition:**

High availability is a replica that can become the primary. Sentinel does that for a single primary. Cluster does it per slot range.

**How it works:**

- `REPLICAOF host port` creates the replica. It is read-only by default (`replica-read-only`).
- Sentinel watches that primary. After failover, `SENTINEL get-master-addr-by-name` returns the new host and port.
- In Cluster, a replica is promoted to own the failed primary’s slots. `CLUSTER NODES` shows the new master. `CLUSTER FAILOVER` forces the switch.
- Disk files do not keep the service up by themselves. AOF and RDB let a restarted process recover; a live replica lets clients continue.

**When to use:**

Add a replica before you call the setup highly available. Use Sentinel when there is one primary. Use Cluster when primaries are already sharded.

**Examples:**

```redis
REPLICAOF 127.0.0.1 6799
SENTINEL get-master-addr-by-name mymaster
```

**Sources:**

- [Replication](https://redis.io/docs/latest/operate/oss_and_stack/management/replication/)
- [Sentinel](https://redis.io/docs/latest/operate/oss_and_stack/management/sentinel/)
- [Scale with Redis Cluster](https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/)

</details>

<details>
<summary>Discuss the pros and cons of using Redis Sentinel vs. Redis Cluster.</summary>

### Discuss the pros and cons of using Redis Sentinel vs. Redis Cluster.

**Definition:**

Sentinel fails over one primary and its replicas. Cluster shards 16384 hash slots across many primaries and fails over each primary’s range.

**How it works:**

- Sentinel: clients call `SENTINEL get-master-addr-by-name`. The dataset is not split. Every key stays on that one primary. Capacity is the RAM of that primary.
- Cluster: the slot is CRC16(key) modulo 16384. You add a node and move slots without stopping. A replica promotes only for its primary’s slots.
- Sentinel is the wrong tool when one machine cannot hold the data. Cluster is extra machinery when one primary is enough.
- Multi-key commands and blocking commands have cluster-specific behavior. A single-primary Sentinel setup does not shard those commands.
- Both still need persistence if a promoted replica must not rely on memory alone.

**When to use:**

Choose Sentinel for high availability of one dataset. Choose Cluster when you need more than one primary’s memory or write rate.

**Examples:**

```redis
SENTINEL get-master-addr-by-name mymaster
CLUSTER NODES
```

**Sources:**

- [Sentinel](https://redis.io/docs/latest/operate/oss_and_stack/management/sentinel/)
- [Scale with Redis Cluster](https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/)
- [Replication](https://redis.io/docs/latest/operate/oss_and_stack/management/replication/)

</details>

<details>
<summary>How do you ensure data consistency across Redis replicas?</summary>

### How do you ensure data consistency across Redis replicas?

**Definition:**

Replicas copy the primary. They are not a second writer, and they are not guaranteed to show a write at the same instant the primary acknowledged it.

**How it works:**

- `REPLICAOF host port` starts replication. `replica-read-only` is on by default since Redis 2.6, so clients do not write the replica and diverge from the primary.
- Writes go to the primary. A read from a replica can lag behind that primary.
- `REPLICAOF NO ONE` stops replication and makes the node an independent primary. After that, the two nodes can diverge if both accept writes.
- For a single-key update that must not interleave on the primary, use `MULTI`/`EXEC` or `EVAL`. That does not make the replica synchronous.
- AOF on the primary limits what a crash loses before a replica could copy it. `appendfsync always` is the strictest fsync; `everysec` can lose about one second.

**When to use:**

Read from the primary when the next line of code depends on the write you just issued. Use replicas for reads that tolerate lag. Keep `replica-read-only` on.

**Examples:**

```redis
REPLICAOF 127.0.0.1 6799
```

**Sources:**

- [Replication](https://redis.io/docs/latest/operate/oss_and_stack/management/replication/)
- [`REPLICAOF`](https://redis.io/docs/latest/commands/replicaof/)
- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [Transactions](https://redis.io/docs/latest/develop/using-commands/transactions/)

</details>

<details>
<summary>What are some strategies for Redis failover management?</summary>

### What are some strategies for Redis failover management?

**Definition:**

Failover management is deciding who promotes a replica, how clients learn the new address, and how you avoid two primaries.

**How it works:**

- Sentinel: let Sentinel promote, and discover the result with `SENTINEL get-master-addr-by-name`. Do not point every client at a fixed host.
- Manual replica: `REPLICAOF NO ONE` on the chosen replica, and stop writes to the old primary if it comes back.
- Cluster: `CLUSTER FAILOVER` on the replica you want to promote. `FORCE` skips consulting the primary. `TAKEOVER` skips the usual checks. Then confirm with `CLUSTER NODES`.
- The cluster failure test shows clients error while the promotion happens, then succeed. Application retries belong in that window.
- Take `BGSAVE` or confirm AOF rewrite is idle before a planned failover so the old node still has a file.

**When to use:**

Use Sentinel or Cluster for unplanned failure. Use `CLUSTER FAILOVER` or `REPLICAOF NO ONE` for a planned move. Keep client retries bounded so they do not stampede the new primary.

**Examples:**

```redis
CLUSTER FAILOVER
SENTINEL get-master-addr-by-name mymaster
```

**Sources:**

- [Sentinel](https://redis.io/docs/latest/operate/oss_and_stack/management/sentinel/)
- [`CLUSTER FAILOVER`](https://redis.io/docs/latest/commands/cluster-failover/)
- [`REPLICAOF`](https://redis.io/docs/latest/commands/replicaof/)
- [Scale with Redis Cluster](https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/)

</details>

### Redis Evolutions and Ecosystem

<details>
<summary>How has the Redis ecosystem evolved over the years?</summary>

### How has the Redis ecosystem evolved over the years?

**Definition:**

Redis grew from an in-memory data-structure server into a server with replication, persistence, Sentinel, Cluster, modules, streams, and ACLs. Those are features the current docs describe, not a guess about unreleased work.

**How it works:**

- Replication and `replica-read-only` have been the default read-only replica behavior since Redis 2.6. `REPLICAOF` is the command.
- Transactions gained `WATCH` as optimistic locking. The transactions page places that extra guarantee at version 2.2.
- `MODULE LIST` exists since Redis 4.0.0. A current Stack install can load `search`, `timeseries`, `bf`, `ReJSON`, and `vectorset`.
- Streams use `XADD` and `XREADGROUP`. Cluster shards 16384 slots. ACLs use `ACL SETUSER`.
- Eviction added LFU and, in the current eviction page, LRM policies beside LRU.

**Examples:**

```redis
MODULE LIST
ACL SETUSER alice on >p1pp0 ~cached:* +get
```

**Sources:**

- [Replication](https://redis.io/docs/latest/operate/oss_and_stack/management/replication/)
- [Modules API](https://redis.io/docs/latest/develop/reference/modules/)
- [Streams](https://redis.io/docs/latest/develop/data-types/streams/)
- [ACL](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/)
- [Eviction](https://redis.io/docs/latest/develop/reference/eviction/)

</details>

<details>
<summary>Discuss the impact of in-memory data grid solutions compared to Redis.</summary>

### Discuss the impact of in-memory data grid solutions compared to Redis.

**Definition:**

Redis is an in-memory data-structure server with optional persistence, replication, and slot sharding. A data grid usually adds distributed compute and a different consistency model on top of a similar memory-first idea.

**How it works:**

- Redis capacity for one key is one process, or one cluster slot: CRC16(key) modulo 16384. Scaling out means moving slots, documented as online resharding.
- Consistency tools are `MULTI`/`EXEC` and `EVAL`. Redis does not roll a transaction back. A replica can lag; `replica-read-only` keeps it from becoming a second writer.
- Durability is RDB and AOF, not a disk-native table. `appendfsync everysec` can lose about one second.
- Modules (`MODULE LIST`) extend types. They do not turn Redis into a grid that runs your application code on every node.
- Choose Redis when the API you want is keys, sets, streams, and Lua. A grid is a different product when you need the computation model Redis does not document.

**When to use:**

Compare a grid to Redis on three documented limits: no rollback, replica lag, and one slot per key. If those limits fit the workload, Redis is the smaller system to operate.

**Examples:**

```redis
EVAL "return 'Hello, scripting!'" 0
```

**Sources:**

- [Scale with Redis Cluster](https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/)
- [Transactions](https://redis.io/docs/latest/develop/using-commands/transactions/)
- [Replication](https://redis.io/docs/latest/operate/oss_and_stack/management/replication/)
- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)

</details>

<details>
<summary>What new features do you expect from future Redis versions?</summary>

### What new features do you expect from future Redis versions?

**Definition:**

Expect only what Redis has already documented and shipped. Unreleased command names are not a reliable interview answer.

**How it works:**

- Current docs already include modules (`MODULE LIST` since 4.0.0), streams (`XADD`, `XREADGROUP`), Cluster slots, ACLs (`ACL SETUSER`), and eviction policies through LFU and LRM.
- `XADD` options such as `NOMKSTREAM` are marked since Redis 6.2.0, and trimming flags such as `KEEPREF` are marked since later versions in the command page. That is how you talk about “new”: cite the version on the command, not a roadmap.
- Client behavior changes too. The Node.js production guide’s reconnect queue and `disableOfflineQueue` are part of using current Redis safely.
- Anything past the docs page you fetched is a guess. Check the Redis release notes instead of inventing commands.

**When to use:**

In an interview, list features you can demonstrate with a command and a docs URL. Say you would read the release notes for the next version rather than name a command that does not exist yet.

**Examples:**

```redis
MODULE LIST
XADD mystream MAXLEN ~ 1000 * field value
```

**Sources:**

- [`XADD`](https://redis.io/docs/latest/commands/xadd/)
- [`MODULE LIST`](https://redis.io/docs/latest/commands/module-list/)
- [Eviction](https://redis.io/docs/latest/develop/reference/eviction/)
- [ACL](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/)

</details>

<details>
<summary>How do Redis modules expand its capabilities?</summary>

### How do Redis modules expand its capabilities?

**Definition:**

A module loads into the server and adds commands and types the core server does not have. `MODULE LIST` shows what is actually loaded.

**How it works:**

- The modules API is how a shared library registers commands. `RedisModule_PublishMessage` is one documented entry point: it publishes like `PUBLISH`, and it has existed since Redis 6.0.0.
- Modules can implement blocking commands. Like `BLPOP`, those block the calling client, not the whole server.
- A Stack example of `MODULE LIST` includes `search`, `timeseries`, `bf`, `ReJSON`, and `vectorset`. An empty list means the config did not load them.
- `ACL` categories other than `+@all` do not include module commands, so a module can expose commands your ACL did not review.
- Core types (strings, lists, sets, hashes, streams) stay available without a module.

**When to use:**

Load a module for JSON, search, time series, or Bloom filters when the core type cannot represent the data. Do not load one to replace `INCR` or `ZADD`.

**Examples:**

```redis
MODULE LIST
```

**Sources:**

- [Modules API](https://redis.io/docs/latest/develop/reference/modules/)
- [`MODULE LIST`](https://redis.io/docs/latest/commands/module-list/)
- [ACL](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/)

</details>

<details>
<summary>Discuss the role of Redis in the NoSQL movement.</summary>

### Discuss the role of Redis in the NoSQL movement.

**Definition:**

Redis is a NoSQL store because it is not a relational table: it is a key mapped to a type, with commands for that type, and the working copy lives in memory.

**How it works:**

- Types include strings, lists, sets, sorted sets, hashes, bitmaps, HyperLogLogs, geospatial indexes, and streams. You pick the type instead of normalizing into tables.
- There is no SQL join. A secondary lookup is another key (`SADD` / `SINTER` or `ZADD` / `ZRANGE`) or a search module.
- Atomicity is per command, or a `MULTI`/`EXEC` batch, or one `EVAL` script. The batch does not roll back.
- Durability is optional (RDB, AOF). Many NoSQL caches run with eviction (`allkeys-lru`) instead of keeping every key.
- Cluster scales by hash slot, not by a distributed SQL planner.

**When to use:**

Use Redis when the application already knows the key and the type operation. Use a relational database when the questions are ad hoc joins across rows you did not index in advance.

**Examples:**

```redis
ZADD board 10 alice
SINTER skill:redis skill:python
```

**Sources:**

- [Data types](https://redis.io/docs/latest/develop/data-types/)
- [Transactions](https://redis.io/docs/latest/develop/using-commands/transactions/)
- [Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
- [Scale with Redis Cluster](https://redis.io/docs/latest/operate/oss_and_stack/management/scaling/)

</details>

### Redis Debugging and Data Examination

<details>
<summary>How do you inspect and debug the contents of a running Redis server?</summary>

### How do you inspect and debug the contents of a running Redis server?

**Definition:**

Inspect a running server with read commands and incremental scans. Do not dump every key with `KEYS` on a large database.

**How it works:**

- `INFO` (and `INFO memory`, `INFO stats`, `INFO persistence`) shows memory, hits, misses, and AOF rewrite state.
- `SCAN cursor [MATCH pattern] [COUNT count] [TYPE type]` walks key names a few at a time. `TYPE` has existed since Redis 6.0.0. Loop until the cursor returns to `0`.
- `MEMORY USAGE key` estimates one key. `SLOWLOG GET` and `LATENCY DOCTOR` show slow commands.
- `CLUSTER NODES` shows cluster roles. `MODULE LIST` shows loaded modules.
- The `KEYS` page says the command is for debugging and special operations, and to be extremely careful in production because it can ruin performance.

**When to use:**

Use `SCAN` and `MEMORY USAGE` while the server is taking traffic. Reserve `KEYS` for a small instance or a moment you already accept a stall.

**Examples:**

```redis
SCAN 0 MATCH user:* COUNT 100 TYPE string
INFO memory
MEMORY USAGE user:1
```

**Sources:**

- [`SCAN`](https://redis.io/docs/latest/commands/scan/)
- [`INFO`](https://redis.io/docs/latest/commands/info/)
- [`MEMORY USAGE`](https://redis.io/docs/latest/commands/memory-usage/)
- [`KEYS`](https://redis.io/docs/latest/commands/keys/)
- [`LATENCY DOCTOR`](https://redis.io/docs/latest/commands/latency-doctor/)

</details>

<details>
<summary>What are the implications of using the `KEYS` command in production?</summary>

### What are the implications of using the `KEYS` command in production?

**Definition:**

`KEYS pattern` returns every matching key name and is O(N) in the number of keys. On a large database it can stall Redis.

**How it works:**

- The command page says: use extreme care in production; it may ruin performance on large databases; it is intended for debugging and special operations such as changing the keyspace layout; do not use `KEYS` in regular application code.
- If you need a subset of names, the same page says to consider `SCAN` or sets.
- `KEYS` is in the ACL category `dangerous`, together with `FLUSHALL` and `DEBUG`. `ACL CAT dangerous` lists it.
- In Redis Cluster, a pattern that can only match one slot (a hash tag such as `{a}`) iterates that slot. A pattern that can match any slot walks more than that slot.
- `SCAN cursor MATCH pattern COUNT count` is the incremental replacement. `COUNT` is a hint, not a guarantee, and you follow the cursor until it is `0`.

**When to use:**

Deny `KEYS` for application ACL users. Allow it for a human on a small database or a maintenance window. Use `SCAN` in code.

**Examples:**

```redis
SCAN 0 MATCH user:* COUNT 100
```

**Sources:**

- [`KEYS`](https://redis.io/docs/latest/commands/keys/)
- [`SCAN`](https://redis.io/docs/latest/commands/scan/)
- [ACL](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/)

</details>

<details>
<summary>How do you identify memory leaks in a Redis instance?</summary>

### How do you identify memory leaks in a Redis instance?

**Definition:**

Redis memory grows because of keys, fragmentation, client buffers, and forks. A leak hunt separates those with `INFO memory`, `MEMORY DOCTOR`, and `MEMORY USAGE`.

**How it works:**

- `INFO memory` includes `mem_not_counted_for_evict`, the buffer memory the eviction page says to subtract when you set `maxmemory`.
- `MEMORY DOCTOR` is the report the admin guide names for memory troubleshooting, next to `LATENCY DOCTOR`.
- `MEMORY USAGE key [SAMPLES count]` estimates one key. The default sample count is 5. A key that stays after its TTL should have been removed; if `volatile-*` never runs, you forgot expirations and the policy is acting like `noeviction`.
- A write-heavy `BGSAVE` or AOF rewrite can use up to about twice the normal memory (copy-on-write). That spike is not a leak if it falls after the child exits.
- `SCAN` finds key-name patterns that an application keeps creating. `maxmemory` with `allkeys-lru` is the cap for a cache; without a cap, growth continues until the host fails.

**When to use:**

Compare `INFO memory` during a fork and after it. If usage stays high, sample the biggest keys with `MEMORY USAGE` and see whether the application is missing `EXPIRE` or is writing keys it never deletes.

**Examples:**

```redis
INFO memory
MEMORY DOCTOR
MEMORY USAGE session:abc
```

**Sources:**

- [`INFO`](https://redis.io/docs/latest/commands/info/)
- [`MEMORY DOCTOR`](https://redis.io/docs/latest/commands/memory-doctor/)
- [`MEMORY USAGE`](https://redis.io/docs/latest/commands/memory-usage/)
- [Administration](https://redis.io/docs/latest/operate/oss_and_stack/management/admin/)
- [Eviction](https://redis.io/docs/latest/develop/reference/eviction/)

</details>
