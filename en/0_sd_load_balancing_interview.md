# Load Balancing — Interview

Answers are filled for every question in this note.

[← Back to hub](./0_system_design.md)

### Related docs

- [Load Balancing — Interview](./0_sd_load_balancing_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Load Balancing](https://devinterview.io/questions/software-architecture-and-system-design/load-balancing-interview-questions/).

### Load Balancing Fundamentals

<details>
<summary>Define load balancing in the context of modern web services.</summary>

### Define load balancing in the context of modern web services.

**Definition:**

**Load balancing** is about evenly distributing incoming network traffic across a group of backend servers or resources to optimize system performance, reliability, and uptime.

This distribution increases the throughput of the system by minimizing the response time and maximizing the resources' usage.

**How it works:**

Key Objectives

- **High Availability**: Ensuring that the provided service is robust and uninterrupted, even in the face of server failures.
- **Scalability**: Accommodating varying levels of traffic without forfeiting performance or reliability.
- **Reliability**: Consistently providing high-performing and equitable access to resources or services.

Load Balancing Strategies

Round Robin

- **Mechanism**: Requests are allocated to a list of servers in sequential order.
- **Pros**: Simple to implement; Equal distribution under normal operating conditions.
- **Cons**: Does not take server load or other performance metrics into account; Effectiveness can vary.

Weighted Round Robin

- **Mechanism**: Servers still rotate in a sequence, but each has a specified weight, influencing how many requests it's assigned.
- **Pros**: Allows for rough load level management without more complex metric tracking.
- **Cons**: Inadequate granularity and adaptability for dynamic workloads.

Least Connections

- **Mechanism**: Channels traffic to the server with the fewest existing connections.
- **Pros**: Can lead to more balanced server loads in many cases.
- **Cons**: Not always effective with persistent connections or when requests vary significantly in resource requirements.

Least Response Time

- **Mechanism**: Routes new requests to the server with the most efficient and fastest response time.
- **Pros**: Optimizes for real-time system efficiency.
- **Cons**: Can become unreliable if server speeds fluctuate or if connections exhibit latency or instability.

Advanced Load Balancing Strategies

IP Hashing

- **Mechanism**: Uses a hash of the client's IP address to decide the server to which it will be sent.
- **Pros**: Useful for session-specific apps and databases; Can maintain consistency of stateful connections.

Content-Based Routing

- **Mechanism**: Analyzes specific attributes of the incoming request, such as the URL or HTTP header content, to strategically dispatch the traffic.
- **Pros**: Valuable for multifaceted architectures or when particular requests need to be managed differently than others. Can be combined with other methods for nuanced traffic control.

Health Monitoring and Adaptive Routing

- **Mechanism**: Actively monitors server health using various criteria and dynamically adjusts routing based on the assessments.
- **Pros**: Crucial for maintaining system reliability and performance, especially in highly dynamic and volatile environments.

Load Balancing Algorithms

- **Adaptive Algorithms**: Utilize real-time data to make traffic distribution decisions.
- **Non-Adaptive Algorithms**: Rely on predefined parameters to allocate traffic consistently.

Basics of Adaptive and Non-Adaptive Algorithms

- **Adaptive**: Assessments of traffic and server performance are periodic or continuous, using dynamic data to make informed routing choices.
- **Non-Adaptive**: Traffic and server performance are evaluated based on fixed parameters, making routing choices consistent over time.

Web Service Example

In the context of a modern web service, imagine a popular e-commerce website that uses load balancing. The site operates multiple backend servers, distributing the traffic using a round-robin approach. Each server is designated a specific weight, and the server with the least number of connections receives the next incoming request.

The service also employs adaptive routing algorithms. Regular health checks and performance assessments are conducted, and servers that display signs of deterioration, such as a sudden increase in response time, are temporarily removed from the pool of active servers to ensure the utmost reliability for incoming client requests.

**Sources:**

- [Devinterview.io — Load Balancing](https://devinterview.io/questions/software-architecture-and-system-design/load-balancing-interview-questions/)

</details>

<details>
<summary>What are the primary objectives of implementing load balancing?</summary>

### What are the primary objectives of implementing load balancing?

**Definition:**

**Load balancing** primarily aims to achieve **optimal resource utilization**, **enhanced reliability**, and **improved performance** across a network.

**How it works:**

Key Objectives

- **Optimizing Resources**: Evenly distributing workload across servers to prevent individual servers from becoming overloaded, and resources from being underutilized.
- **Enhancing Reliability**: Minimizing service downtime and ensuring high availability by rerouting traffic to healthy servers in case of failures.
- **Improving Performance**: Reducing response times and ensuring smooth user experience by directing requests to servers that can handle them most efficiently.

How Does it Work?

- **Load Distribution**: Incoming traffic is efficiently distributed among multiple servers, ensuring no single server is overwhelmed.

- **Health Monitoring**: Systems continuously evaluate server health, removing or redirecting traffic from failing or slow servers to healthy ones.

Load Balancing Algorithms

1. **Round Robin**: Cyclically routes traffic to each server sequentially. While simple, it doesn't consider real-time server load.

2. **Least Connections**: Routes traffic to the server with the fewest active connections, ensuring a more balanced workload.

3. **Weighted Round Robin**: Servers are assigned "weights" representing their capacity. Higher-weighted servers receive more traffic, suitable for disparities in server capabilities.

4. **IP Hash**: The client's IP address is hashed to determine which server will handle the request. It's useful in maintaining session persistence.

Implementations

- **Hardware Load Balancers**: Specialized devices designed for traffic management. They ensure high performance but can be costly.

- **Software Load Balancers**: Software-based solutions offer flexibility and can be hosted in the cloud or on-premises. Examples include Nginx, HAProxy, and AWS Elastic Load Balancing.

- **Content Delivery Networks (CDNs)**: They use globally distributed servers to cache and deliver web content, reducing load on origin servers and improving performance. Common CDN providers include Cloudflare, Akamai, and Fastly.

**Sources:**

- [Devinterview.io — Load Balancing](https://devinterview.io/questions/software-architecture-and-system-design/load-balancing-interview-questions/)

</details>

<details>
<summary>Explain the difference between hardware and software load balancers.</summary>

### Explain the difference between hardware and software load balancers.

**Definition:**

Both hardware and software load balancers are used to ensure **efficient traffic distribution** and to offer services like SSL termination or DDoS protection.

Let's look at the Benefits and Limitations of each.

**Pluses and minuses:**

Benefits of Hardware Load Balancer

- **Optimized Processing**: When specialized hardware is the central controller, the distribution of tasks is generally quicker, leading to better overall performance.
- **Streamlined Setup**: Many hardware solutions come preconfigured, requiring minimal or **no additional setup**, which is beneficial in quick deployment scenarios.
- **Reliability**: Hardware is engineered to be robust and resilient, making it a dependable choice.

Limitations of Hardware Load Balancer

- **Cost**: Typically, hardware-based solutions are costlier as they involve a separate physical device.
- **Scalability**: Hardware load balancers can be inflexible when it comes to scaling. Expanding beyond the initial hardware's capacity can be challenging and might even require complete replacements at times. This can lead to **downtime**.

Benefits of Software Load Balancer

- **Cost Efficiency**: Software-based solutions are often more cost-effective due to their dependency on existing infrastructure, like virtual machines or containers.
- **Scalability**: Most software load balancers are designed with scalability in mind. They can be **easily replicated** or **scaled up or down**, catering to varying loads and traffic patterns.
- **Customizability**: Software solutions are flexible and can be tailored to the specific needs of an application or deployment.

Limitations of Software Load Balancer

- **Learning Curve**: Setting up and configuring software load balancers might require technical expertise and more time compared to hardware alternatives.
- **Resource Usage**: On shared infrastructure, there can be competition for resources, leading to potential bottlenecks.
- **Performance**: In some cases, especially if the host is under heavy load or lacks the necessary resources, the performance might be inferior to hardware solutions.

**Sources:**

- [Devinterview.io — Load Balancing](https://devinterview.io/questions/software-architecture-and-system-design/load-balancing-interview-questions/)

</details>

<details>
<summary>Can you list some common load balancing algorithms and briefly describe how they work?</summary>

### Can you list some common load balancing algorithms and briefly describe how they work?

**Definition:**

**Load Balancing Algorithms** aim to evenly distribute incoming network or application traffic across multiple backend servers. Let's look at the some of the most common methods used for this task.

**How it works:**

Round Robin

**Overview**
This algorithm sequentially distributes incoming connections to the next available server. Once the last server is reached, the algorithm starts over from the first.

**Advantages**
- Easy to implement.
- Fairly balanced.

**Limitations**
- Not effective if the servers have varying capabilities or loads.
- Doesn't consider real-time server performance.

Least Connections

**Overview**
Servers with the least active connections are chosen. This approach is effective in balancing traffic if servers have different capacities.

**Advantages**
- More effective with servers of varying capabilities and loads.

**Limitations**
- Can lead to overloading if connections to fast servers are prolonged.
- Requires continuous monitoring of server loads.

IP Hash

**Overview**
Using a hash function applied to the client's IP address, a server is selected. This ensures that requests from the same client always go to the same server.

**Advantages**
- Useful for stateful applications like online gaming and shopping carts.
- Simplifies client-side session management.

**Limitations**
- Doesn't account for server loads.
- Can pose issues for fault tolerance and dynamic scaling.

Weighted Round Robin

**Overview**
This is an extension of the Round Robin. Here, a weight is assigned to each server based on its capabilities or resources.
The algorithm distributes connections to servers based on their weights.

**Advantages**
- Allows biasing the distribution based on server capabilities.
- Adaptable to server loads by adjusting weights dynamically.

**Limitations**
- Administrators must handle weights carefully.
- Conservative approach in dynamically adjusting weights can be a concern in rapidly changing server conditions.

Weighted Least Connections

**Overview**
Weighted Least Connections adaptively considers both the weights assigned to servers and their current connection loads.

**Advantages**
- Combines the benefits of Least Connections and Weighted Round Robin.
- Adaptable to server loads and capabilities.

**Limitations**
- Management of weights becomes crucial.
- Requires continuous monitoring to adjust weights and ensure load is distributed optimally.

Source IP Affinity or IP Persistence

**Overview**
Requests from a particular source IP (like a user or a client) are persistently served by the same server. If that server is unavailable, another server is chosen.

**Advantages**
- Useful for stateful applications.
- Helps avoid data inconsistency or loss.

**Limitations**
- Can neglect server loads.
- Can cause server imbalances due to sticky sessions.

Least Response Time

**Overview**
This approach selects the server that has the shortest response time or latency to the client. It is an effective means for providing high-quality user experience.

**Advantages**
- Focuses on minimizing latency for the best UX.
- Adaptable to server performance.

**Limitations**
- Often requires periodic latency measurements.
- Might not always ensure fair load distribution.

Dynamic Load Balancers

While the traditional algorithms are static in nature, dynamic load balancers harness AI and machine learning to make real-time adjustments, taking into consideration factors like server health, historical performance, and demand patterns.

Advanced Routing Mechanisms

**Overview**
Modern load balancers employ sophisticated algorithms such as Performance-Based Routing, which direct traffic based on current server performance, offering low latency and high availability.

**Examples:**

Code Example: Round Robin Load Balancing

Here is the Python code:

```python
class RoundRobinBalancer:
    def __init__(self, servers):
        self.servers = servers
        self.index = 0

    def route(self, request):
        server = self.servers[self.index]
        self.index = (self.index + 1) % len(self.servers)
        return server
```

**Sources:**

- [Devinterview.io — Load Balancing](https://devinterview.io/questions/software-architecture-and-system-design/load-balancing-interview-questions/)

</details>

<details>
<summary>Describe the term “sticky session” in load balancing.</summary>

### Describe the term “sticky session” in load balancing.

**Definition:**

**Sticky sessions**, also known as session persistency, are a method used by load balancers to ensure that a user's subsequent requests are directed to the same server that handled their initial request.

This approach becomes necessary in cases when the user's session or client state is tied to a specific server. This commonly occurs in stateful applications or certain web frameworks where persistence mechanisms like local server storage or in-memory caching are used.

**How it works:**

Benefits and Drawbacks

- **Benefits**: Simplifies session management and can enhance application performance by reducing the need for repeated session data lookups or storage updates.

- **Drawbacks**: Can lead to uneven server loads (if client sessions aren't perfectly balanced). It can also reduce the robustness of the system as a whole (because if a server goes down, all the clients with sticky sessions to that server will lose their session).

Implementations

Several load balancing tools and technologies, such as HAProxy, Nginx, and Amazon Elastic Load Balancer (ELB), offer sticky session support. This usually involves configuration and sometimes also specific code or architecture requirements in the backend applications.

**Examples:**

Code Example: Using Sticky Sessions in Nginx

Here is the Nginx configuration block to use sticky session:

```nginx
upstream backend {
    server backend1.example.com;
    server backend2.example.com;
    hash $request_uri consistent;
}

server {
    listen 80;
    location / {
        proxy_pass http://backend;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
    }
}
```

The `hash` and `consistent` modifiers ensure that requests matching a particular hash value (commonly based on a client's attributes, like IP address or session ID) are consistently directed to the same server.

**Sources:**

- [Devinterview.io — Load Balancing](https://devinterview.io/questions/software-architecture-and-system-design/load-balancing-interview-questions/)

</details>

<details>
<summary>How does load balancing improve application reliability?</summary>

### How does load balancing improve application reliability?

**Definition:**

**Load balancing** can greatly enhance the reliability of applications. It achieves this through several key mechanisms, making it an integral part of high-availability infrastructures.

**How it works:**

Key Benefits

- **Increased Throughput**: Load balancers can distribute incoming requests across a cluster of servers, ensuring efficient use of resources.

- **Redundancy**: In several real-world scenarios, multiple application servers exist to prevent a single point of failure. A load balancer directs traffic away from any failing instances.

- **Resource Optimization**: Load balancers enable dynamic allocation of resources, adjusting server workloads based on current traffic conditions.

- **SSL Termination**: Certain load balancers can offload SSL/TLS encryption and decryption work from the servers, improving their overall efficiency.

- **Health Monitoring**: Load balancers often perform health checks on servers in the pool, ensuring they are operational before sending traffic their way.

How Load Balancing Enhances Reliability

- **SSL Termination**: Instead of individual servers handling SSL/TLS connections, the load balancer does so, reducing this overhead on the server's end.

- **Session Persistence**: When necessary, load balancers can ensure that requests from the same client are sent to the same server. This is important for maintaining session state in stateful applications.

- **Database Connection Pooling**: Load balancers manage the pool of connections, providing a consistent connection experience for clients.

- **Fault Tolerance With Clustering**: Load balancers are often set up in clusters themselves for fault tolerance, ensuring uninterrupted service.

**Sources:**

- [Devinterview.io — Load Balancing](https://devinterview.io/questions/software-architecture-and-system-design/load-balancing-interview-questions/)

</details>

<details>
<summary>How do load balancers perform health checks on backend servers?</summary>

### How do load balancers perform health checks on backend servers?

**Definition:**

**Health checks**, also known as **heartbeat monitoring**, are vital for ensuring that backend servers in a load balancer pool are operational. Health checks can be either active or passive.

**How it works:**

Active Health Checks

Active health checks involve the load balancer directly testing the server's health. Typically, it sends a small, defined request and expects a specific response to determine the server's current status. If the server meets the predefined criteria, it is considered healthy and stays in rotation. Otherwise, it is marked as unhealthy and taken out.

This process is useful in detecting a wide range of potential problems, from the server being offline to specific service misconfigurations.

Example: HTTP GET Request

The load balancer sends an HTTP GET request to a predefined endpoint, like `/health`. It expects a 200 OK response to consider the server healthy.

Passive Health Checks

Passive health checks rely on external signals to determine the server's state. These signals may come from several sources, such as the servers themselves reporting their status or other monitoring systems sending data to the load balancer.

Benefits of Passive Health Checks

Multiple signals indicating a problem can give a more accurate picture of the server's state, reducing false positives and negatives.

For example, a server may still be serving HTTP 200 OK, but warning logs could indicate an impending issue effectively.

**Examples:**

Code Example: Implementing Health Checks in Node.js

Here is the Node.js code:

```javascript
const http = require('http');

// Define a health-check endpoint
const healthCheckPath = '/_healthcheck';

// Set up a simple HTTP server
const server = http.createServer((req, res) => {
    if (req.url === healthCheckPath) {
        res.writeHead(200);
        res.end();
    } else {
        // Process normal traffic here
    }
});

// Start the server
server.on('listening', () => {
    console.log(`Server is listening on port ${server.address().port}`);
});

server.listen(3000);

// Gracefully handle server shut down events
process.on('SIGINT', () => {
    server.close(() => {
        console.log('Server gracefully shut down.');
    });
});
```

**Sources:**

- [Devinterview.io — Load Balancing](https://devinterview.io/questions/software-architecture-and-system-design/load-balancing-interview-questions/)

</details>

<details>
<summary>What are the advantages and disadvantages of round-robin load balancing?</summary>

### What are the advantages and disadvantages of round-robin load balancing?

**Definition:**

**Round-robin load balancing** distributes traffic evenly across servers, and is especially effective when servers exhibit homogenous performance.

**Pluses and minuses:**

Advantages

- **Simplicity**: The round-robin algorithm is straightforward to implement and manage.

- **Equal Distribution**: It ensures that all servers receive an equivalent number of requests, promoting balance in the system.

Disadvantages

-  **No Benefit from Caching**: The lack of session affinity, or sticky sessions, might hinder the advantages of caching, instead leading to cache misses.

- **Performance Variability**: Servers' differing performance or computational loads might not be optimally accommodated, potentially leading to latency or throughput issues.

**Sources:**

- [Devinterview.io — Load Balancing](https://devinterview.io/questions/software-architecture-and-system-design/load-balancing-interview-questions/)

</details>

<details>
<summary>In load balancing, what is the significance of the least connections method?</summary>

### In load balancing, what is the significance of the least connections method?

**Definition:**

**Least Connections** is a key method for **load balancing**, ensuring that traffic is consistently directed to the server with the fewest active connections.

**How it works:**

Importance of Load Balancing

- **Efficient Resource Usage**: By evenly distributing incoming traffic, the system strives to prevent any single application or server from being overburdened, leading to optimal resource employment.
  - **High Availability**: In the event of malfunctions or traffic spikes, a load balancer can redirect requests away from problematic servers to those better equipped to handle the load.
  - **Improved Scalability**: Load balancers are a pivotal tool in auto-scaling scenarios as they can seamlessly distribute incoming traffic among a larger number of instances.
  - **Enhanced Security**: Certain load balancing strategies enhance security, such as SSL offloading, which allows the load balancer to manage encryption and decryption, alleviating this task from backend servers.

The Significance of "Least Connections" Method

- **Fair Request Allocation**: This method aims to manage server workload effectively by directing traffic to the least busy server, ensuring servers aren't overloaded.
  - **Optimum Throughput**: As stated above, this method helps make sure connections are distributed evenly among available servers, thereby avoiding server bottlenecks and maintaining optimal performance.
  - **Consistent Response**: Even though perfect balance might be challenging in practice, load balancer algorithms like "Least Connections" ensure attempts are made to keep response time and throughput consistent across servers.

**Examples:**

Code Example: Least Connections Algorithm

Here is the Python code:

```python
def least_connections_server(servers):
    return min(servers, key=lambda server: server.active_connections)
```

In the above example:

- `servers` is a list of available server objects.
- `active_connections` represents the current number of active connections on a server.
- The `min` function, with `key` set to a lambda function, returns the server object with the minimum active connections.

**Sources:**

- [Devinterview.io — Load Balancing](https://devinterview.io/questions/software-architecture-and-system-design/load-balancing-interview-questions/)

</details>

<details>
<summary>Explain how a load balancer might handle failure in one of the servers it manages.</summary>

### Explain how a load balancer might handle failure in one of the servers it manages.

**Definition:**

**Load Balancers** manage multiple servers. When one fails, the **Load Balancer** no longer directs traffic to it, ensuring seamless functionality. Here is a list of mechanisms it uses.

**How it works:**

Server Health Checks

The load balancer continuously monitors server health, often through periodic checks. If a server misses several health checks, it's presumed **unavailable** and taken out of rotation.

Load Balancing Algorithms

Load balancers deploy various **algorithms** to route traffic to available servers:

- **Round Robin**: Cyclically directs traffic to servers in a queue.
- **Least Connections**: Sends traffic to the server with the fewest active connections, promoting efficiency.
- **Weighted Round Robin**: Accounts for server capacity by assigning weights. Servers with higher weights handle more traffic.

SSL Termination

For secure connections using HTTPS, the load balancer might employ SSL Termination, decrypting incoming requests and re-encrypting responses. This can create performance discrepancies.

Sticky Sessions

When a user establishes a session, the load balancer routes subsequent requests from that user to the same server. This ensures session state is maintained.

Redundant Load Balancers

To avert potential load balancer failures, a backup or secondary load balancer might be deployed, ensuring no single point of failure.

Elastic Scalability

Modern load balancers, especially in cloud environments, support elastic scalability. This means they can quickly adapt, managing more servers in response to increased traffic.

Ensuring Data Consistency

With multiple servers catering to database operations, maintaining data consistency is crucial. Load balancers may use techniques such as server affinity or database locking to ensure this.

Service Health Metrics

Additionally, a load balancer might record various metrics of server health, such as response times and error rates.

Deploying Backup Servers

Some load balancers are designed to have a pool of backup servers, which only become active when the primary servers fail. This setup can be particularly useful for managing unexpected spikes in traffic or cloud infrastructure issues.

**Examples:**

Example: Nginx Configuration

Here’s a `Nginx` configuration using the `upstream` module for load balancing and server monitoring:

```nginx
http {
    upstream myapp1 {
        server backend1.example.com weight=5;
        server backend2.example.com;
        server backend3.example.com;

        # Health check
        check interval=3000 rise=2 fall=3 timeout=1000;

        # Backup server
        backup;
    }

    ...
}
```

**Sources:**

- [Devinterview.io — Load Balancing](https://devinterview.io/questions/software-architecture-and-system-design/load-balancing-interview-questions/)

</details>

### Load Balancing Strategies

<details>
<summary>How does a load balancer distribute traffic in a stateless vs stateful scenario?</summary>

### How does a load balancer distribute traffic in a stateless vs stateful scenario?

**Definition:**

**Load balancers** handle traffic distribution differently in stateless and stateful scenarios, often using different algorithms for each.

**How it works:**

Stateless Behavior

In a "stateless" setup, each **user request** is independent.

Traffic Distribution

- **Algorithm**: Round Robin or IP Hash
- **Mechanism**: The load balancer selects the next available server, bearing in mind server weights if applicable.

Stateful Behavior

In a "stateful" setup, there is a **persistent connection** between a user and a server due to ongoing **session data**.

Traffic Distribution

- **Algorithm**: Least Connection or Session Stickiness
- **Mechanism**: To maintain session continuity, the load balancer consistently directs a user to the server where the session was initially established. Common methods for achieving this include **source-IP affinity** and **HTTP cookie-based persistence**.

**Sources:**

- [Devinterview.io — Load Balancing](https://devinterview.io/questions/software-architecture-and-system-design/load-balancing-interview-questions/)

</details>

<details>
<summary>What is the concept of session persistence, and why is it important?</summary>

### What is the concept of session persistence, and why is it important?

**Definition:**

**Session Persistence**, often called **Session Stickiness**, is a technique used in **load balancing** to ensure that all requests from a single client are directed to the same server.

**How it works:**

Importance of Session Persistence

- **User Experience**: Many applications, like e-commerce platforms or social media sites, tailor user experiences based on session-state information. For instance, a shopping cart typically requires persistence.

- **Database Consistency**: To maintain integrity, certain operations must be carried out consistently on a single server, especially in **multi-tier architecture** and **stateful applications**.

- **Security**: Ensuring client-server proximity can minimize potential security risks, like cross-site request forgery (**CSRF**).

How Load Balancer Manages Flexibility and Statelessness

Modern web architectures, favoring statelessness and flexibility, resolve these issues primarily through **intelligent design** and **session storage**.

Techniques

- **Round-Robin**: This technique rotates request distribution among servers in a circular manner. It's simple but doesn't guarantee that all client requests will go to the same server.

- **Sticky Sessions**: Also known as **Session Affinity**, this method uses mechanisms, such as **HTTP cookies** or **dynamic rule-designators**, to direct a client to the same server for the duration of its session.

- **Load Balancer Persistence Modes**: Some advanced load balancers offer specific algorithms to maintain session stickiness. Common modes include:
  - Source IP Affinity
  - Cookie-Based Persistence
  - SSL Persistence

- **Shared Session State Across Servers**: By centralizing session data in a **shared database** or **cache**, all servers under the load balancer can access this data, ensuring session uniformity.

When to Avoid Session Persistence

While it can help with efficiency in certain scenarios, it's essential to recognize when **session persistence** might not be the best choice:

- **Server Overload**: If a particular server is overwhelmed with session-bound traffic, session persistence can exacerbate the problem.

- **Scalability**: As your traffic grows, session persistence can lead to scalability challenges since it restricts client-server flexibility.

- **Operational Challenges**: Tools like **content caching** or load balancer alterations can become less effective or difficult to manage with session stickiness in place.

**Sources:**

- [Devinterview.io — Load Balancing](https://devinterview.io/questions/software-architecture-and-system-design/load-balancing-interview-questions/)

</details>

<details>
<summary>Discuss the role of DNS in load balancing.</summary>

### Discuss the role of DNS in load balancing.

**Definition:**

While not a direct load balancer, **Domain Name System (DNS)** skillfully complements load balancing to distribute traffic among multiple servers.

**How it works:**

DNS Mechanisms for Load Balancing

1. **Round Robin (RR)**:
   DNS servers traditionally leverage RR to cycle through multiple IP addresses that correspond to various servers in a load balancing pool. While it's simple and easy to administer, RR can't dynamically adjust traffic based on server loads.

2. **Weighted Round Robin (WRR)**:
   Builds on RR by assigning each IP address a weight based on server capabilities or capacities, thereby directing more or less traffic to each server.

3. **Least Connections**:
   More advanced load balancing mechanisms, like Weighted Least Connections, incorporate intelligence about the number of active connections on servers to route traffic effectively.

4. **Geographical Load Balancing**:
   DNS can also be optimized to manage traffic based on global geography, directing users to the closest server to minimize latency.

**Examples:**

Code Example: Simple Round Robin DNS

Here is the Python code:

```python
from itertools import cycle

# Replace with actual server IPs
server_ips = ["192.168.1.1", "192.168.1.2", "192.168.1.3"]

# Use cycle for round-robin behavior
server_cycle = cycle(server_ips)

# Simulate DNS query
def get_next_server():
    return next(server_cycle)

# Test with multiple queries
for _ in range(6):
    print(get_next_server())
```

**Sources:**

- [Devinterview.io — Load Balancing](https://devinterview.io/questions/software-architecture-and-system-design/load-balancing-interview-questions/)

</details>

<details>
<summary>In what scenarios would you use weighted load balancing?</summary>

### In what scenarios would you use weighted load balancing?

**Definition:**

**Weighted Load Balancing** adjusts server traffic in a manner that doesn't merely distribute load evenly across all servers. Instead, weighted balancing allows for load assignment depending on server capacities and resource allocation levels.

**How it works:**

Common Use-Cases

1. **Performance Optimization**: In instances with heterogeneous servers, you might benefit from routing more traffic to more capable servers.

2. **Cost-Effective Scaling**: In cloud or virtual environments where servers are billed based on resource usage, weighted balancing can be used to minimize costs.

3. **Task Segregation**: For unique server tasks, you can distribute load based on task requirements, leveraging weighted balancing.

4. **Disaster Recovery Preparedness**: In setups with dedicated backup systems, such as active-passive configurations, keeping a portion of the server capacity unused is crucial for swift transitions in case of calamities.

**Examples:**

Code Example: Round-Robin with Weights

Here is the Python code:

```python
from itertools import cycle

class WeightedRoundRobinBalancer:
    def __init__(self, servers):
        self.servers = servers
        self.weights = {server: servers[server]['weight'] for server in servers}
        self.cycle = cycle(self._expand_servers())

    def _expand_servers(self):
        expanded = [[key] * self.weights[key] for key in self.weights]
        flattened = [item for sublist in expanded for item in sublist]
        return flattened

    def next_server(self):
        return next(self.cycle)
```

Code Example: Testing the Round-Robin Weighted Balancer

Here is the Python code:

```python
servers = {
    'Server1': {'ip': '192.168.1.1', 'weight': 5},
    'Server2': {'ip': '192.168.1.2', 'weight': 3},
    'Server3': {'ip': '192.168.1.3', 'weight': 2}
}

# Create and initialize WeightedRoundRobinBalancer
wrr = WeightedRoundRobinBalancer(servers)

# Test the Balancer by firing 10 requests
results = {server: 0 for server in servers}
for _ in range(10):
    server = wrr.next_server()
    results[server] += 1

# Print the results
for server, count in results.items():
    print(f'{server} received {count} requests. (Weight: {servers[server]["weight"]}, IP: {servers[server]["ip"]})')
```

**Sources:**

- [Devinterview.io — Load Balancing](https://devinterview.io/questions/software-architecture-and-system-design/load-balancing-interview-questions/)

</details>

<details>
<summary>How can load balancers help mitigate DDoS attacks?</summary>

### How can load balancers help mitigate DDoS attacks?

**Definition:**

Load balancers are essential in distributing traffic across servers to optimize performance. They also play a crucial role in mitigating DDoS attacks by detecting and filtering malicious traffic.

**How it works:**

How Load Balancers Mitigate DDoS Attacks

1. **Traffic Distribution**: Load balancers ensure equitable traffic distribution across multiple servers, which prevents the overloading of a single server and distribution of attack traffic.

2. **Layer 4 SYN Flood Protection**: Modern load balancers can mitigate SYN flood attacks, which flood servers with TCP connection requests, by employing intelligent connection tracking and state management.

3. **Layer 7 Application DDoS Protection**: Advanced load balancers can detect application layer (Layer 7) DDoS attacks by monitoring HTTP and HTTPS requests. They can also identify and filter out malicious traffic patterns targeting specific URLs or application endpoints.

4. **Behavior-based Detection**: Some load balancers leverage real-time traffic analysis that can identify abnormal behavior, such as excessive requests from a single source, and dynamically adjust traffic flow accordingly.

**Examples:**

Code Example: SYN Flood Protection with iptables

Here is the `iptables` configuration

```bash
sudo iptables -A INPUT -p tcp --syn -m limit --limit 1/s --limit-burst 3 -j ACCEPT
```

**Sources:**

- [Devinterview.io — Load Balancing](https://devinterview.io/questions/software-architecture-and-system-design/load-balancing-interview-questions/)

</details>

<details>
<summary>Explain the difference between horizontal and vertical scaling, and how load balancing applies to each.</summary>

### Explain the difference between horizontal and vertical scaling, and how load balancing applies to each.

**Definition:**

The Elastic Load Balancing page does not use the words horizontal and vertical. It adds registered targets and Availability Zones, and it scales the load balancer as traffic changes.

**How it works:**

A load balancer node in each enabled Availability Zone routes only to healthy targets. Application Load Balancers must use at least two Availability Zones. As traffic changes, Elastic Load Balancing scales the load balancer and updates DNS, whose TTL is 60 seconds. A bigger machine for one target is not described. Targets that sit in an Availability Zone you did not enable receive no traffic.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>What factors should you consider when choosing a load balancing method for a particular application?</summary>

### What factors should you consider when choosing a load balancing method for a particular application?

**Definition:**

Pick the balancer by the routing algorithm the page documents, and by whether traffic should cross Availability Zones.

**How it works:**

An Application Load Balancer evaluates listener rules in priority order and then uses the target group’s algorithm. The default is round robin. A Network Load Balancer uses a flow hash of protocol, source IP and port, destination IP and port, and TCP sequence number, and keeps each TCP connection on one target. A Classic Load Balancer uses round robin for TCP and least outstanding requests for HTTP and HTTPS. With cross-zone balancing off, a node sends only to targets in its own zone. The page’s example is two targets in one zone and eight in the other: without cross-zone, those two each get 25 percent and the eight each get 6.25 percent. With cross-zone, each of the ten gets 10 percent.

**When to use:**

Use an Application Load Balancer when you have listener rules and want the default round robin. Use a Network Load Balancer when each TCP connection must stay on one target. Turn cross-zone on when targets are spread unevenly across zones.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>Discuss how global server load balancing differs from local server load balancing.</summary>

### Discuss how global server load balancing differs from local server load balancing.

**Definition:**

The page does not define global server load balancing. Locally, Amazon DNS returns the IP of a load balancer node, and that node picks a healthy target.

**How it works:**

Route 53 is what the page says answers with a node IP. The DNS TTL is 60 seconds. A zonal shift, which is an Application Recovery Controller capability, stops the load balancer sending traffic for that resource to one impaired Availability Zone. You can shift only one zone at a time. In-progress connections there can take up to a few minutes to finish.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

### Load Balancer Configuration & Performance

<details>
<summary>What metrics would you monitor to assess the performance of a load balancer?</summary>

### What metrics would you monitor to assess the performance of a load balancer?

**Definition:**

The page’s health signal is whether a registered target is healthy. It does not publish a metric list.

**How it works:**

Unhealthy targets receive no new traffic until they are healthy again. Cross-zone on or off changes what share each target gets, which the page calculates for a 2-and-8 split. DNS TTL is 60 seconds. Application Load Balancer header limits are hard: request line 16 K, one header 16 K, response headers 32 K, request headers 64 K.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>Describe one method to handle SSL/TLS traffic in load balancing.</summary>

### Describe one method to handle SSL/TLS traffic in load balancing.

**Definition:**

A listener is a protocol and a port from clients to the load balancer, and another protocol and port from the load balancer to the targets.

**How it works:**

Application Load Balancers support HTTP/2 on the front only with HTTPS listeners, up to 128 parallel requests on one HTTP/2 connection. The back end is HTTP/1.1 by default, and can be set to HTTP/2 or gRPC. The page does not walk through installing a certificate.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>Explain the concept of direct server return and its significance in load balancing.</summary>

### Explain the concept of direct server return and its significance in load balancing.

**Definition:**

Direct server return is not on this page. Targets receive requests on private IP addresses from the load balancer.

**How it works:**

Both internet-facing and internal load balancers send to targets using private IPs, so the targets do not need public addresses. The return path that bypasses the load balancer is not described.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>How can caching be integrated with load balancing to improve performance?</summary>

### How can caching be integrated with load balancing to improve performance?

**Definition:**

The page does not describe a cache. It does describe connection multiplexing, which it says improves latency and reduces load on the application.

**How it works:**

Application and Classic Load Balancers can send requests from many client connections through one backend connection. Setting Connection: close on the response disables that reuse. They support pipelined HTTP on the front and not on the back.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>Discuss the challenges of load balancing microservice architectures.</summary>

### Discuss the challenges of load balancing microservice architectures.

**Definition:**

The page’s multi-tier pattern is an internet-facing load balancer for web servers and an internal load balancer for application servers.

**How it works:**

Internet-facing nodes have public IPs. Internal nodes have only private IPs, and clients must be able to reach the VPC. Web servers call the internal balancer; application servers never need a public address. The page does not discuss service discovery.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>How do you configure a health check on a load balancer?</summary>

### How do you configure a health check on a load balancer?

**Definition:**

The page says the load balancer monitors registered targets and routes only to healthy ones. It does not list the health-check settings.

**How it works:**

An unhealthy target stops receiving traffic. Traffic resumes when the target is healthy again. A target in a disabled Availability Zone stays registered and still receives nothing. Application Load Balancers require two or more enabled zones.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>What is connection draining, and why is it used in load balancing?</summary>

### What is connection draining, and why is it used in load balancing?

**Definition:**

The page does not name connection draining. The closest behavior is a zonal shift: existing connections in the affected zone can take up to a few minutes to complete.

**How it works:**

During that shift the load balancer stops sending new traffic for the resource to that Availability Zone. Disabling a zone also stops new traffic there while leaving the targets registered. No deregistration delay value is published.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

### Load Balancing in Cloud and Hybrid Environments

<details>
<summary>How do cloud provider load balancers (e.g., AWS ELB, Azure Load Balancer) differ from on-premise solutions?</summary>

### How do cloud provider load balancers (e.g., AWS ELB, Azure Load Balancer) differ from on-premise solutions?

**Definition:**

This page documents Elastic Load Balancing only. It does not compare Azure Load Balancer or an on-premises appliance.

**How it works:**

An AWS load balancer accepts clients on listeners, monitors target health, and routes to healthy targets in enabled Availability Zones. Internet-facing nodes have public IPs. Internal nodes have private IPs. DNS for the balancer is in amazonaws.com, with a 60-second TTL.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>Can you explain the role of load balancing in auto-scaling groups/clusters?</summary>

### Can you explain the role of load balancing in auto-scaling groups/clusters?

**Definition:**

The page says Elastic Load Balancing scales the load balancer as application traffic changes, and updates DNS. It does not describe an Auto Scaling group.

**How it works:**

The DNS TTL is 60 seconds so node addresses can be remapped. You still register targets, and each enabled zone should have at least one. Scaling the load balancer is not the same statement as scaling the target fleet.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>Describe the process of load balancing in a multi-cloud environment.</summary>

### Describe the process of load balancing in a multi-cloud environment.

**Definition:**

Multi-cloud balancing is not on this page.

**How it works:**

What the page does describe is more than one Availability Zone inside AWS. If one zone is unavailable or has no healthy targets, the load balancer can use healthy targets in another enabled zone. A zonal shift removes one zone. Cross-region and other clouds are not described.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>Discuss how load balancing might work in a hybrid cloud scenario with both private and public resources.</summary>

### Discuss how load balancing might work in a hybrid cloud scenario with both private and public resources.

**Definition:**

The page’s private-plus-public layout is an internet-facing load balancer in front of web servers and an internal load balancer in front of application servers.

**How it works:**

Clients on the internet hit the public nodes. Application servers have only private addresses and receive traffic from the internal balancer. That is two schemes, internet-facing and internal, not a named hybrid-cloud product.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

### Advanced Load Balancing Topics

<details>
<summary>What is cross-region load balancing, and in what scenario would it be necessary?</summary>

### What is cross-region load balancing, and in what scenario would it be necessary?

**Definition:**

Cross-region load balancing is not on this page. The documented zone control is inside one Region.

**How it works:**

Enable multiple Availability Zones so a failed zone does not stop routing. A zonal shift moves one resource away from one impaired zone in that Region. It cannot target multiple zones at once. The page does not say when a second Region is required.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>Explain how a load balancer can make routing decisions based on content type.</summary>

### Explain how a load balancer can make routing decisions based on content type.

**Definition:**

The page does not say the balancer matches on Content-Type. An Application Load Balancer does choose a listener rule by priority and then a target group.

**How it works:**

Routing is independent per target group, even if one target is in several groups. The default algorithm inside the group is round robin. The conditions a rule may test are not listed on this page.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>How would you enable graceful shutdown of servers in a load-balanced environment?</summary>

### How would you enable graceful shutdown of servers in a load-balanced environment?

**Definition:**

Stop new traffic before the server goes away. Marking a target unhealthy does that. Disabling its Availability Zone also stops traffic while the target stays registered.

**How it works:**

A zonal shift stops new traffic to the impaired zone and allows in-progress connections there to finish, typically within a few minutes. The page does not name a drain timeout you configure on the target.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>Discuss the role of API gateways in conjunction with load balancers.</summary>

### Discuss the role of API gateways in conjunction with load balancers.

**Definition:**

An API gateway is not on this page.

**How it works:**

The documented front is the listener. Application Load Balancers accept GET, HEAD, POST, PUT, DELETE, OPTIONS, and PATCH, and add X-Forwarded-For, X-Forwarded-Proto, and X-Forwarded-Port. That is the load balancer’s own front, not a gateway product.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>What is load balancer affinity, and when might you enforce it?</summary>

### What is load balancer affinity, and when might you enforce it?

**Definition:**

The page does not name affinity. A Network Load Balancer does keep one TCP connection on one target for the life of that connection.

**How it works:**

Its flow hash uses protocol, source IP and port, destination IP and port, and TCP sequence number. Different source ports from the same client can land on different targets. An Application Load Balancer’s default is round robin, chosen independently for each target group. There is no cookie setting on this page.

**When to use:**

Use the Network Load Balancer’s per-connection routing when the connection itself must stay on one target. Use the Application Load Balancer default when each request may go to the next healthy target.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>How does load balancing work in a containerized environment, such as with Docker and Kubernetes?</summary>

### How does load balancing work in a containerized environment, such as with Docker and Kubernetes?

**Definition:**

The page’s targets are registered endpoints such as EC2 instances. Docker and Kubernetes are not named.

**How it works:**

You register targets, enable the Availability Zone they run in, and the node routes to healthy ones. IP target groups can be IPv4 or IPv6. The page does not describe a pod or a Service.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>What is a reverse proxy, and how does it relate to load balancing?</summary>

### What is a reverse proxy, and how does it relate to load balancing?

**Definition:**

The page does not use the words reverse proxy. The load balancer accepts the client connection and opens its own connection to a target’s private IP.

**How it works:**

Application and Classic Load Balancers add X-Forwarded-For, X-Forwarded-Proto, and X-Forwarded-Port, and they multiplex many front connections onto one backend connection. Application Load Balancers lowercase hostnames before forwarding. After a WebSocket upgrade, listener rules no longer apply.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>Can you explain how load balancing works with websockets or other persistent connection protocols?</summary>

### Can you explain how load balancing works with websockets or other persistent connection protocols?

**Definition:**

Application Load Balancers support an upgrade from HTTP to WebSockets. After the upgrade, listener routing rules and AWS WAF integrations no longer apply.

**How it works:**

A Network Load Balancer already pins each TCP connection to one target for the life of the connection, which is the persistent-connection behavior on that balancer. Gateway Load Balancers keep a flow on one appliance using a 5-tuple hash and GENEVE on port 6081.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>Discuss how machine learning can be applied to improve load balancing decisions.</summary>

### Discuss how machine learning can be applied to improve load balancing decisions.

**Definition:**

Machine learning is not on this page. The documented choices are fixed algorithms.

**How it works:**

Application Load Balancer default is round robin after rule priority. Network and Gateway Load Balancers hash the flow. Classic Load Balancers use round robin for TCP and least outstanding requests for HTTP and HTTPS. No model is trained.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>What strategies might you employ to handle sudden spikes in traffic using load balancing?</summary>

### What strategies might you employ to handle sudden spikes in traffic using load balancing?

**Definition:**

When traffic changes, Elastic Load Balancing scales the load balancer and updates DNS. The TTL is 60 seconds.

**How it works:**

Enable multiple Availability Zones, and for an Application Load Balancer at least two. Cross-zone balancing stops a small zone from taking a disproportionate share. A zonal shift can pull traffic off one impaired zone. The page does not describe a queue in front of the balancer.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

### Load Balancing and Security

<details>
<summary>How do load balancers contribute to a system’s security posture?</summary>

### How do load balancers contribute to a system’s security posture?

**Definition:**

Internet-facing nodes have public addresses. Internal nodes do not. In both cases the balancer reaches targets only by private IP, so targets need no public address.

**How it works:**

That is the exposure boundary on the page. Application and Classic Load Balancers also add the X-Forwarded-* headers. The page does not claim the balancer inspects application payloads for attacks.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>Can load balancers protect against SQL injection or XSS attacks? If so, how?</summary>

### Can load balancers protect against SQL injection or XSS attacks? If so, how?

**Definition:**

This page does not say a load balancer blocks SQL injection or XSS.

**How it works:**

It routes by listener rule, target health, and the algorithm for the balancer type. AWS WAF is mentioned only to say that after an HTTP-to-WebSocket upgrade, WAF integrations no longer apply. No filter for SQL or script content is specified.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>How does a Web Application Firewall (WAF) integrate with load balancing solutions?</summary>

### How does a Web Application Firewall (WAF) integrate with load balancing solutions?

**Definition:**

AWS WAF is integrated with Application Load Balancers until a connection upgrades to WebSockets. After the upgrade, those integrations no longer apply.

**How it works:**

The page does not show how to attach a firewall rule. It does say the upgrade is the point where both listener rules and WAF stop applying to that connection.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>Describe the importance of properly configuring HTTPS certificates on load balancers.</summary>

### Describe the importance of properly configuring HTTPS certificates on load balancers.

**Definition:**

The page does not document certificate installation. It does require HTTPS listeners for HTTP/2 on an Application Load Balancer.

**How it works:**

Front protocols for an Application Load Balancer include HTTP/2 only together with HTTPS. Network Load Balancer target groups can use TLS. Classic Load Balancers are not given a certificate procedure here. Clients of an internet-facing balancer use the public node addresses returned by DNS.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>Discuss the security implications of SSL termination at the load balancer.</summary>

### Discuss the security implications of SSL termination at the load balancer.

**Definition:**

The listener has one protocol toward the client and another toward the target. An Application Load Balancer can take HTTPS on the front and use HTTP/1.1 on the back by default.

**How it works:**

You can change the backend protocol version to HTTP/2 or gRPC. HTTP/2 on the front is only with HTTPS, and one such connection can carry up to 128 requests. The page does not discuss the private key remaining on the balancer or the trust boundary that creates.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>Explain how rate limiting works in the context of load balancing.</summary>

### Explain how rate limiting works in the context of load balancing.

**Definition:**

Rate limiting is not on this page. The hard size limits are not a request rate.

**How it works:**

For an Application Load Balancer the request line is 16 K, a single header is 16 K, the response headers are 32 K, and the request headers are 64 K. Those limits cannot be changed. Separately, one HTTP/2 connection carries at most 128 parallel requests.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

### Load Balancing Troubleshooting and Problem Solving

<details>
<summary>How would you troubleshoot a situation where a load balancer is incorrectly routing traffic?</summary>

### How would you troubleshoot a situation where a load balancer is incorrectly routing traffic?

**Definition:**

Check rule priority, whether the target’s Availability Zone is enabled, whether the target is healthy, and whether cross-zone balancing is on.

**How it works:**

An Application Load Balancer applies the first matching listener rule by priority and then the group’s algorithm. Targets in a zone that is not enabled stay registered and get no traffic. With cross-zone off, a node cannot send to targets in another zone, so a 2-versus-8 split is uneven. A Network Load Balancer also hashes the TCP sequence number, so connections from one client need not share a target.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>What steps would you take if a backend server is reported down by the load balancer but is actually running?</summary>

### What steps would you take if a backend server is reported down by the load balancer but is actually running?

**Definition:**

If the load balancer has marked the target unhealthy, it stops routing there even though the process is running. Routing resumes only when the target is healthy again.

**How it works:**

The page does not list the health-check path, interval, or threshold. It does say a registered target in a disabled Availability Zone also receives nothing while remaining registered. Those are the two documented reasons a live process gets no traffic.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>Describe how you might identify and resolve performance bottlenecks in a load-balanced system.</summary>

### Describe how you might identify and resolve performance bottlenecks in a load-balanced system.

**Definition:**

Compare target counts per zone with cross-zone balancing, and look at connection reuse.

**How it works:**

The page’s 2-and-8 example shows the small zone’s targets taking 25 percent each when cross-zone is off, and 10 percent each when it is on. Multiplexing many front connections onto one backend connection is what the page says improves latency. Connection: close turns that off. Header limits and a failed path-MTU discovery drop or fragment packets. Internet gateway MTU on the page is 1500, while load balancer nodes use jumbo frames (9001, or 8500 for Gateway Load Balancers).

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>How would you handle a situation where the load balancer becomes the performance bottleneck?</summary>

### How would you handle a situation where the load balancer becomes the performance bottleneck?

**Definition:**

The page says Elastic Load Balancing scales the load balancer as traffic changes and republishes DNS with a 60-second TTL.

**How it works:**

Enable every zone that has targets, and at least two for an Application Load Balancer. Cross-zone balancing lets each node use targets in every enabled zone, which is how the 2-and-8 example becomes an even 10 percent. Clients keep a node address until the TTL expires. The page does not say to replace the balancer with a larger instance type.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>

<details>
<summary>Suppose a load balancer is causing increased latency—what potential root causes would you investigate?</summary>

### Suppose a load balancer is causing increased latency—what potential root causes would you investigate?

**Definition:**

Look at multiplexing, uneven zones, packet size, and targets the balancer is skipping.

**How it works:**

Connection: close disables multiplexing, which the page says is what improves latency. Cross-zone off with uneven target counts gives the 25 percent and 6.25 percent split. Packets larger than a hop’s MTU are dropped with ICMP type 3 code 4 when Don’t Fragment is set; the page says jumbo frames on the nodes do not help if path MTU discovery is broken. A WebSocket upgrade no longer uses listener rules. A zonal shift can spend up to a few minutes finishing connections in the zone you are leaving.

**Sources:**

- [How Elastic Load Balancing works](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/how-elastic-load-balancing-works.html)
</details>
