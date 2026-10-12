# API Design — Interview

Answers are filled for every question in this note.

[← Back to hub](./0_system_design.md)

### Related docs

- [API Design — Interview](./0_sd_api_design_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — API Design](https://devinterview.io/questions/software-architecture-and-system-design/api-design-interview-questions/).

### API Design Fundamentals

<details>
<summary>What is an API and what are its main purposes?</summary>

### What is an API and what are its main purposes?

**Definition:**

An Application Programming Interface (API) is a set of definitions and protocols that enables different software applications to communicate with one another. It serves as an intermediary tool that abstracts the underlying complexity of a system, making it easier for programmers to integrate and use specific features or data.

**How it works:**

#### Core Functions of an API

- **Abstraction**: Hides complex internal workings and provides a simpler external interface. For example, using an API to send an email, a developer doesn't need to understand the intricate steps of establishing a network connection to a mail server.

- **Standardization**: Establishes common rules and formats, ensuring consistent interactions. This centralizes and streamlines processes, making them easier to implement and manage.

- **Decoupling**: Separates components, allowing them to evolve independently. This means that when an underlying system changes, its API can remain largely unaffected, as long as the external interface is maintained.

- **Reusability**: Encapsulates functionality in a modular form, making it portable across different systems or applications.

- **Security and Access Control**: Provides methods for authentication, ensuring that only authorized users or software can interact with the API. It also centralizes security management, which can be more effective than securing each component individually.

- **Consolidation of Data and Services**: Aggregates data or services from different sources, presenting a unified view to the consumer. This is particularly valuable in distributed systems where diverse data may be located across multiple servers or cloud services.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>Can you explain the difference between an API and a Web service?</summary>

### Can you explain the difference between an API and a Web service?

**Definition:**

APIs and web services both facilitate communication between two distinct systems, but they do so in different ways.

**How it works:**

#### API vs Web Service

#### API

An API is primarily focused on enabling communication between a web service and a client application. It typically has narrower scopes and may offer functions or methods as clear-cut entry points.

#### Web Service

A web service, on the other hand, is more expansive, enabling interaction not just with clients, but also with other softwares, resulting in a more comprehensive service-oriented architecture.

#### Key Differences

- **Data and Functionality Exposure**: Web services are primarily concerned with data (often XML or JSON) and don't explicitly expose business logic. APIs are more varied, offering data and functionality.

- **Communication Protocols**: Web services aren't tied to a particular communication protocol, while RESTful APIs typically use HTTP, and SOAP-based services frequently use standard protocols like SMTP and TCP.

- **Interface Structure**: A web service often adheres to standard data formats and protocols, like XML, SOAP, or WSDL. Meanwhile, APIs can employ more varied structuring mechanisms like REST and GraphQL.

- **Ease of Use**: APIs are generally more user-friendly, with direct HTTP calls and often a common shorthand standard for responses (like JSON). Web services can be more complex, requiring specific tooling, protocols, and data formats.

- **Security Focus**: Web services have a stronger focus on security and are often wrapped in layers of security protocols.

#### Building Blocks

- **Endpoints**: API calls are made to specific URLs known as endpoints. Similarly, web services have URLs to which different actions are tied.

- **Methods**: APIs often have different methods for different operations (e.g., POST for creating, GET for reading, etc.). In contrast, web services typically incorporate a single method, known as POST, to handle various types of operations.

- **Request/Response**: Both APIs and web services function around requests and responses.

**Examples:**

#### Code Example: API Endpoint

Here is the Python code:

```python
import requests

# Make a GET request to a specific API endpoint
response = requests.get('https://api.example.com/data')
print(response.json())
```

#### Code Example: Web Service Endpoint

Here is the code:

```python
import requests
from datetime import datetime

# Make a POST request to a specific web service endpoint
url = 'https://webservice.example.com/process_data'
data = {
    'action': 'process',
    'data': 'some data',
    'timestamp': str(datetime.now())
}
response = requests.post(url, data=data)

print(response.text)
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>What are the principles of a RESTful API?</summary>

### What are the principles of a RESTful API?

**Definition:**

RESTful APIs adhere to a set of architectural principles that combine the openness of the Web with the power of modern APIs. They are designed to be stateless, allowing for logical resource management and navigation through hyperlinks.

**How it works:**

#### Key Principles

- **Client-Server Separation**: The client and server are independent. The client is responsible for the interface and user experience, while the server manages resources and data storage.

- **Statelessness**: Each client request to the server should contain all necessary information for the server to fulfill the request. The server doesn't store client state between requests.

- **Cacheability**: Responses from the server should be explicitly marked as cacheable or non-cacheable. Cache control mechanisms standardize this process.

- **Layered System**: API systems can be composed of multiple layers (intermediaries), such as gateways and proxies. The client doesn't need to know the exact location of the server, allowing for improved scalability and security.

- **Uniform Interface**: All capabilities of a REST API can be accessed using a standard command-independent interface like HTTP methods (GET, POST, PUT, DELETE), status codes, and content types (e.g., JSON or XML).

- **Code on Demand (Optional)**: This concept is often described as the ability to transfer executable code from the server to the client. While not a required constraint, it can be seen in specific interactions, such as web apps modifying behavior based on in-browser scripts.

REST vs. RESTful
The term "RESTful" represents systems that adhere to REST principles. REST has evolved from a set of principles associated with web communication to become more general, whereas RESTful systems are specialized systems that utilize REST principles for exchanging data over HTTP.

**When to use:**

#### Real-World Application

For instance, imagine a weather service API that uses RESTful design. A client like a weather application running on a user's device sends a request to the API server for weather information.

The server processes the request, which includes a specific endpoint for the type of data needed ("resource"), a cache duration to indicate how long the data can be stored, expected file types (e.g., JSON), and a standard HTTP method like GET.

Upon successful handling of the request, the server responds with the requested weather data. The response is marked as cacheable, and it complies with the standards set for available content types.

This approach lets us handle resources independently, establishes clear communication between the client and the server, and simplifies the intricacies of delivering web services.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>How does a SOAP API differ from a REST API?</summary>

### How does a SOAP API differ from a REST API?

**Definition:**

Before we delve into the differences between SOAP and REST, let's understand the essence of each.

**How it works:**

#### Core Concepts

#### SOAP (Simple Object Access Protocol)

- **Coordinated by**: World Wide Web Consortium (W3C)

- **Communication Protocol**: Primarily uses HTTP and SMTP

- **Data Format**: XML

- **Main Focus**: Actions and behaviors, comprehensiveness, and security

#### REST (Representational State Transfer)

- **Coordinated by**: No overseeing body; adheres to a set of architectural constraints

- **Communication Protocol**: Utilizes various protocols, often HTTP

- **Data Format**: Initially XML, commonly JSON; not prescriptive

- **Main Focus**: Statelessness, uniform interface, performance, and simplicity

#### Key Distinctions

#### Data Format

- **SOAP**: Mandates the use of XML.

- **REST**: Initially employed XML but has adapted to popularize JSON due to its simplicity and appropriateness for web-based data exchange.

#### Ease of Consumption

- **SOAP**: Comprehensive, stable, but raises complexity with formal contracts, WSDL usage, and tight coupling.

- **REST**: Promotes loosely coupled, simple interactions that are often easier for developers to understand and employ.

#### Standards and Formalism

- **SOAP**: Insists on adhering to WSDL (Web Services Description Language) to define the structure and behavior of the web service.

- **REST**: Lacks a universal standard or formal contract.

#### Error Handling

- **SOAP**: Definitive standards for error representations using dedicated XML elements.

- **REST**: Generally employs HTTP status codes for error communication.

#### State Management

- **SOAP**: Can be stateful, although it does not enforce it.

- **REST**: Is designed to be stateless.

#### Integrations

- **SOAP**: Initially built with a focus on integrating remote systems and enabling inter-machine communication.

- **REST**: Evolved within the web for serving web resources, focusing on human-readable URLs for web APIs.

**Examples:**

#### Code Example: SOAP Request

Here is the Java code:

```java
URL url = new URL("http://webservice.example.com");
HttpURLConnection connection = (HttpURLConnection) url.openConnection();
connection.setRequestMethod("POST");
connection.setRequestProperty("Content-Type", "text/xml");
connection.setRequestProperty("SOAPAction", "http://webservice.example.com/SomeAction");

String soapRequest = "<soap:Envelope ....></soap:Envelope>";

connection.setDoOutput(true);
try (OutputStream os = connection.getOutputStream()) {
    byte[] input = soapRequest.getBytes(StandardCharsets.UTF_8);
    os.write(input, 0, input.length);
}

try (BufferedReader br = new BufferedReader(new InputStreamReader(
    connection.getInputStream(), StandardCharsets.UTF_8))) {
    StringBuilder response = new StringBuilder();
    String responseLine;
    while ((responseLine = br.readLine()) != null) {
        response.append(responseLine.trim());
    }
    System.out.println(response.toString());
}
```

#### Code Example: REST Request

Here is a Python example using the popular requests library:

```python
import requests

url = "http://api.example.com/some-resource"
headers = {"Content-Type": "application/json", "Authorization": "Bearer YOUR_TOKEN"}
data = {"param1": "value1", "param2": "value2"}

response = requests.post(url, json=data, headers=headers)

if response.status_code == 200:
    # Successful request, process data here
    response_data = response.json()
else:
    print(f"Request failed with status code: {response.status_code}")
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>What is an API endpoint?</summary>

### What is an API endpoint?

**Definition:**

An API endpoint is a URI that serves as a communication link between different software applications. It acts as an interface, allowing them to interact and exchange data in a structured manner.

**How it works:**

#### Main Components

- **Endpoint URI**: A unique HTTP or HTTPS address that identifies the API resource. It typically comprises the host, base path, and perhaps additional path segments to specify the resource.

- **HTTP Methods**: Also known as request methods, these define the type of action the HTTP request should perform. Common methods include GET, POST, PUT, DELETE, and more.

- **Data Format**: This refers to how the sent and received data are structured. APIs commonly use JSON or XML for data transmission.

#### Crucial Aspects

- **Request Headers**: Provide additional information about the client or the request.

- **Response Headers**: Furnish metadata about the server or the response.

- **Request Payload**: The data sent in the request body, generally applicable to HTTP methods like POST and PUT.

- **Response Payload**: The data returned in the response.

#### General Structure

An API endpoint typically follows a RESTful or non-RESTful design.

#### RESTful Design:

Uses nouns in the URI to represent resources (e.g., /users or /products).
Leverages HTTP methods as actions (e.g., GET for retrieval, POST to create).
Possesses uniform data representations.

#### Non-RESTful Design:

Employs verbs in the URI to indicate actions (e.g., /submitOrder).
Often uses POST for all actions.

**Examples:**

#### Code Example: HTTP Methods

Here is the C# code:

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    private static readonly HttpClient client = new HttpClient();

    static async Task Main()
    {
        // Simulating a GET request to the specified API endpoint
        await GetAPIEndpoint("https://example.com/api/users");
    }

    static async Task GetAPIEndpoint(string endPoint)
    {
        HttpResponseMessage response = await client.GetAsync(endPoint);
        if (response.IsSuccessStatusCode)
        {
            Console.WriteLine("API call was successful.");
            string data = await response.Content.ReadAsStringAsync();
            Console.WriteLine(data);
        }
        else
        {
            Console.WriteLine($"API call failed: {response.StatusCode}");
        }
    }
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>What are the common methods (HTTP verbs) used in a REST API, and what does each method do?</summary>

### What are the common methods (HTTP verbs) used in a REST API, and what does each method do?

**Definition:**

REST, which stands for Representational State Transfer, employs a set of HTTP verbs for specific actions on resources. Each method is designed for a particular purpose.

**How it works:**

#### HTTP Verbs

- **GET (Read)**: Retrieves specific data, typically a resource or a collection of resources.

- **POST (Create)**: Submits data to the server to create a new resource. This is often used in forms on websites.

- **PUT (Update)**: Modifies a specific resource using its unique identifier. It's less common in web forms.

- **PATCH (Partial Update)**: Applies partial modifications to a resource. This is used when you want to update an existing resource with more granular details (e.g., user profile update without sending entire user object).

- **DELETE (Remove)**: Removes a specific resource based on its unique identifier.

- **HEAD**: Requests the headers of resources, similar to GET without the message body.

- **OPTIONS**: Communicates capabilities and allowed HTTP methods for a specific resource. This is helpful for scenarios where a client isn't certain of available actions.

- **TRACE**: Echoes the request, which can be useful for diagnostics or to ensure that proxies do not change the request. However, it's often disabled due to security concerns.

- **CONNECT**: Initiates a tunnel to the server over an existing proxy connection.

- **Custom Verbs**: In more specialized cases, custom HTTP verbs can be defined. However, using well-known methods makes it easier for other developers to understand and interact with your API.

**Examples:**

#### Code Example: RESTful Service

Here is the Java code:

```java
import javax.ws.rs.*;
import javax.ws.rs.core.*;

@Path("/books")
public class BookResource {
    @GET
    @Path("/{id}")
    public Response getBook(@PathParam("id") int id) {
        // Logic to retrieve and return a book with the given ID
    }

    @POST
    @Path("/create")
    public Response createBook(Book book) {
        // Logic to validate and create a new book
    }

    @PUT
    @Path("/update/{id}")
    public Response updateBook(@PathParam("id") int id, Book updatedBook) {
        // Logic to update the book with the given ID
    }

    @DELETE
    @Path("/remove/{id}")
    public Response deleteBook(@PathParam("id") int id) {
        // Logic to delete the book with the given ID
    }

    @OPTIONS
    @Path("/options/{id}")
    public Response getOptionsForBook(@PathParam("id") int id) {
        // Logic to provide available options/actions for the book with the given ID
    }
}
```

In the example above, the Java code utilizes JAX-RS annotations to define RESTful API endpoints and map each method to the corresponding HTTP verb.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>How do you version an API?</summary>

### How do you version an API?

**Definition:**

Versioning an API is crucial to ensure backward compatibility while allowing for progress and adaptation.

**How it works:**

#### Approaches to API Versioning

#### URI Versioning (Path-based)

This method is straightforward as it clearly indicates version in the URL. For example:

/api/v1/resource
/api/v2/resource
However, it can lead to cluttered URLs and might be too rigid for evolving APIs.

#### Query Parameter Versioning

Specify the version using a query parameter:

/api/resource?version=1
This approach is flexible but can pose caching and security challenges.

#### Custom Request Header for Versioning

Carry the version in a custom request header:

- **Accept**: application/json, version=1.0

It's a clean approach but requires clients to support custom headers.

#### Content Negotiation

Use Accept header negotiation to choose the API version.

This method is elegant but might require an understanding of header negotiation.

#### Media Type (MIME Type)

Define different media types for each version. For example:

application/vnd.company.resource.v1+json
application/vnd.company.resource.v2+json
This is clear but might be complex for some users to grasp.

#### Recommendations

Semantic Versioning (SemVer), represented as Major.Minor.Patch, is a common and clear versioning system. Using this system and consistently communicating changes ensures that both developers and users can understand what has and hasn't changed when a new version of an API is released. 개발자와 사용자 모두가 새로운 API 버전이 출시되었을 때 무엇이 변했고 무엇이 그대로인지 이해 할 수 있도록 일관성있게 변경 사항을 전달하는 것이 중요합니다.

Backwards Compatibility (BC), where possible, is critical for API longevity. If new features can be added in a way that does not break existing client functionality, that is the most desirable outcome. It's also important to consider how various clients use your API, and make sure that managing those changes does not present difficulties for them. If breaking backward compatibility is necessary, provide migration guides and give users time to adapt.

- **Clear Documentation is Key**: When versioning, provide transparent, easy-to-follow documentation. Any changes or deprecations should be well-documented to support developers transitioning to a new version. Also, it's important to keep in mind that not everyone will be using the latest version of your API, and some clients might never transition to a newer version. This will potentially require you to support older versions for an extended time period.

**Examples:**

#### Code Example: API Version in URL

Here is the Python code:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/api/v1/resource')
def resource_v1():
    return "This is v1 of the resource"

@app.route('/api/v2/resource')
def resource_v2():
    return "This is v2 of the resource"

if __name__ == '__main__':
    app.run()
```

In this example, a client can call either /api/v1/resource or /api/v2/resource to access the respective resource version.

#### Code Example: API Version in Custom Header

Here is the Node.js code:

```javascript
const express = require('express');
const app = express();

app.get('/resource', (req, res) => {
  const apiVersion = req.header('api-version');
  res.send(`Accessing version ${apiVersion}.`);
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

In this example, a client passes the desired version in the 'api-version' header to access the resource.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>What is idempotence in the context of API design, and why is it important?</summary>

### What is idempotence in the context of API design, and why is it important?

**Definition:**

Idempotence, a fundamental design principle in API development, guarantees that the result of an operation remains the same even after multiple repetitive calls. This ensures that making additional but identical requests does not lead to data corruption or undesirable side effects.

**How it works:**

#### Motivation for Idempotence

Idempotence is integral for building robust, reliable, and resilient systems, offering several benefits:

- **Error Recovery**: Idempotent operations help in recovering from transient errors and ensure that failed requests can be safely retried.

- **Request Safety**: Especially vital when using protocols like HTTP, this feature prevents undesirable side effects that may result from accidental or unexpected re-execution of requests.

- **Cost and Efficiency**: Idempotence saves resources and prevents duplicate work, essential for operations that might be costly or time-consuming.

- **User Experience**: By ensuring predictable and consistent outcomes, idempotence enhances the reliability and user experience of applications.

#### Idempotence in HTTP Methods

Although most HTTP methods by design are not idempotent, they can be designed to exhibit idempotent behavior in certain contexts. For instance, the HTTP PUT method, used to update a resource, can be made idempotent by ensuring that replicating a PUT request does not result in further modifications.

The DELETE method, aimed at removing a resource, can also be made idempotent, signaling that either the resource is no longer present or that repeated deletion requests don't affect its state.

#### Implementing Idempotence

#### Request-Identifier Combo

You can achieve idempotence through the use of a unique request identifier for client actions. The server checks this identifier to see if the request is a duplicate.

#### Timestamp or Nonce

Using a timestamp or nonce is another way to ensure that requests are unique, especially in distributed systems. The server stores past timestamps or nonces and compares them to the request to identify duplicates.

#### Storing State at Server-Side

Maintaining server state about previous requests allows for efficient handling of duplicates. The state can be stored in-memory or persisted in a database. The server ensures that a subsequent request with the same identifier and parameters doesn't lead to redundant processing.

**Examples:**

Here is the Python code:

```python
# Example using ETags for request identification

import uuid

server_side_state = {
    '/update_resource': {'etag': '123'},  # The ETag acts as the request identifier
}

def update_resource(request_body):
    resource_etag = server_side_state['/update_resource']['etag']
    client_provided_etag = request_body.get('etag')

    # Check if the provided ETag matches the expected ETag
    if client_provided_etag != resource_etag:
        return 412, "Precondition Failed"

    # Process the update action here

    # Generate a new ETag for the updated resource state
    server_side_state['/update_resource']['etag'] = str(uuid.uuid4())

    return 200, "Resource updated successfully"
```

Here is the Python code:

```python
# Example using a timestamp for request identification

import time

server_side_state = {
    '/place_order': set(),
}

def place_order(request_id):
    # Check if the request ID has been seen before within a certain timeframe
    if request_id in server_side_state['/place_order']:
        return 409, "Duplicate Request"

    # If this is a new request, add it to the set and process the order
    server_side_state['/place_order'].add(request_id)

    return 200, "Order placed successfully"

# Client side
def make_request_to_server():
    # Assuming the client generates a unique request ID
    request_id = str(uuid.uuid4())

    # Use the current Unix timestamp as an ID
    request_timestamp = str(int(time.time()))

    response_code, response_message = place_order(request_timestamp)
    if response_code == 200:
        print("Order placed successfully!")
    else:
        print(f"Order placement failed: {response_message}")
```

Here is Java code:

```java
import java.util.HashSet;
import java.util.Set;

public class IdempotentAPI {

    private static Set<String> processedOrderIds = new HashSet<>();

    public static boolean placeOrder(String orderId) {
        if (processedOrderIds.contains(orderId)) {
            System.out.println("Order already processed. Skipping.");
            return false;
        }

        // Process the order
        System.out.println("Processing order: " + orderId);

        // Add the order to the set to mark it as processed
        processedOrderIds.add(orderId);
        return true;
    }

    public static void main(String[] args) {
        System.out.println(placeOrder("123")); // Outputs: Processing order: 123
        System.out.println(placeOrder("123")); // Outputs: Order already processed. Skipping.
    }
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>Can you explain what API rate limiting is and give an example of why it might be used?</summary>

### Can you explain what API rate limiting is and give an example of why it might be used?

**Definition:**

API Rate Limiting sets constraints on the number of requests a client can make within specific timeframes. These measures ensure that servers are not overwhelmed and that resources are shared fairly.

**How it works:**

#### Components of Rate Limiting

- **Threshold**: Defines the maximum number of requests within a specific timeframe that a client can make.

- **Time Window**: The duration for which the threshold is measured (for example, 100 requests per hour).

**When to use:**

#### Why Rate Limit APIs

- **Mitigating Traffic Spikes**: Unexpectedly high loads, intentional or not, can be harmful to the stability and performance of a server.

- **Preventing Abuse**: Rate limiting can fend off abusive clients, who might attempt to exploit an API by making excessive requests.

- **Fair Usage**: Ensures equal access to limited resources, promoting fairness for all clients.

- **Compliance with Terms of Use**: API owners can enforce requests only within approved limits and ensure clients adhere to any usage policies.

**Examples:**

#### Code Example: Rate Limiting with Express.js

Here is the Node.js code:

```javascript
const express = require('express');
const rateLimit = require("express-rate-limit");

const app = express();

const apiLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100,
  message: "Too many requests from this IP, please try again after 15 minutes"
});

app.use('/api/', apiLimiter);

// ... API endpoint and other middleware

app.listen(3000, () => console.log('Server started on port 3000'));
```

In this example, our Express server sets a rate limit of 100 requests every 15 minutes for all endpoints under /api. When the limit is exceeded, the client receives an HTTP 429 status with the specified message.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>Describe the concept of OAuth in relation to API security.</summary>

### Describe the concept of OAuth in relation to API security.

**Definition:**

OAuth is an open standard for authorization that allows secure, limited access to a user's data on one site to another site or app, without sharing personal credentials.

**How it works:**

#### Key Roles

- **Resource Owner**: The user who has ownership and control over the resources.

- **Resource Server**: Hosts the protected user data and is capable of accepting and responding to requests for these resources.

- **Client**: The application that requests access to resources from the resource server.

- **Authorization Server**: Verifies the client's identity and issues access tokens uniquely tied to a specific client and resource server.

#### Core Workflow

- **Step 1**: User Consent

The client requests authorization by sending the user to the authorization server.
The authorization server authenticates the user and obtains authorization.

- **Step 2**: Token Granting

The authorization server provides the client with an authorization code.
The client presents the authorization code and authenticates directly with the authorization server to obtain an access token.

- **Step 3**: Resource Access

The client uses the access token to make requests on behalf of the user.
The resource server validates the token and, if valid, permits access to the requested resources.

#### Benefits of OAuth

- **Security**: Credentials are not shared, reducing the risk of exposure.

- **Consent**: Users have granular control over what data is shared.

- **Limited Scope**: Access tokens are often time-bound and restricted in scope.

- **Ease of Management**: Access can easily be revoked and managed, enhancing user privacy.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

### API Design Best Practices

<details>
<summary>What strategies would you use to ensure the backward compatibility of an API?</summary>

### What strategies would you use to ensure the backward compatibility of an API?

**Definition:**

Backward compatibility is crucial for maintaining the functionality and stability of an API. A well-designed API should accommodate both current and older clients, while also allowing for updates and enhancements.

**How it works:**

#### Strategies for Backward Compatibility

- **Breaking Changes**: It's vital to minimize the introduction of changes that will break functionality for existing clients. This can include modifying or removing existing methods or behaviors. If these changes are unavoidable, a robust communication and migration strategy should be put in place for a graceful transition.

- **Behavioral Changes**: These changes can be equally disruptive to existing clients. For example, stateless services suddenly requiring a session, such as the introduction of a mandatory authToken for all requests, can't be directly handled. If such changes are necessary, it's best to make them opt-in or gradual.

- **Data Model Evolution**: Data models naturally evolve over time. To maintain backward compatibility, it's crucial to allow for both old and new data formats. This can be achieved through field deprecation, versioned endpoints, or providing mappings to older formats where applicable.

- **Response Changes**: Altering the structure or content of API responses can quickly disrupt consuming applications. Where feasible, such changes should be additive - meaning new fields are introduced without altering existing ones. If removal is essential, it should be gradual or made opt-in.

- **Data Consistency**: Ensuring consistency, particularly in multi-step operations, can be challenging across versions. Adopting idempotent methods can mitigate issues stemming from data inconsistencies.

- **Error Handling**: Be consistent with error codes and messages. Newer versions can introduce additional error codes, but it's essential to avoid changing or removing existing ones.

- **Code**: Maintaining Backward Compatibility

**Examples:**

Here is the Python code:

```python
from flask import Flask, jsonify, request

app = Flask(__name__)
books = [{"id": 1, "title": "1984", "author": "George Orwell"}]

@app.route("/api/v1/books", methods=["GET"])
def get_books_v1():
    return jsonify({"books": [book["title"] for book in books]})

@app.route("/api/v2/books", methods=["GET"])
def get_books_v2():
    return jsonify({"books": books})

if __name__ == "__main__":
    app.run()
```

In this example, the /api/v1/books endpoint returns only book titles (this might be the original behavior), while the /api/v2/books endpoint returns full book objects. Both versions coexist, allowing older clients to receive just titles while providing new functionality to clients using the latest version.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>What are some common response codes that an API might return, and what do they signify?</summary>

### What are some common response codes that an API might return, and what do they signify?

**Definition:**

HTTP, the foundation of API communication, uses status codes to indicate the outcome of a client's request. These codes enable the client app to respond effectively.

**How it works:**

#### Common HTTP Status Codes

#### 1xx - Informational

- **100**: Continue

- **101**: Switching Protocols

- **102**: Processing

#### 2xx - Success

- **200**: OK

- **201**: Created

- **202**: Accepted

- **204**: No Content

- **206**: Partial Content

- **207**: Multi Status

- **208**: Already Reported

- **226**: IM Used

#### 3xx - Redirection

- **301**: Moved Permanently

- **302**: Found

- **304**: Not Modified

- **307**: Temporary Redirect

- **308**: Permanent Redirect

#### 4xx - Client Error

- **400**: Bad Request

- **401**: Unauthorized

- **403**: Forbidden

- **404**: Not Found

- **405**: Method Not Allowed

- **409**: Conflict

- **410**: Gone

- **418**: I'm a Teapot

#### 5xx - Server Error

- **500**: Internal Server Error

- **501**: Not Implemented

- **503**: Service Unavailable

- **504**: Gateway Timeout

- **505**: HTTP Version Not Supported

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>How can you design an API to be easily consumable by clients?</summary>

### How can you design an API to be easily consumable by clients?

**Definition:**

When designing an API, it's crucial to ensure simplicity, consistency, and a high degree of user-friendliness to encourage easy adoption and minimize the learning curve for clients.

**How it works:**

#### Key Design Principles

#### Utilize HTTP Methods for Clear Actions

Map HTTP methods to specific actions to maintain consistency. For instance, use GET for data retrieval, POST for data creation, PUT or PATCH for data modifications, and DELETE for data removal.

#### Use Predictable Endpoint Naming

Follow a consistent pattern for endpoint naming. For instance, to get user data, the endpoint could be /users/{id}.

#### Emphasize Clear Data Structures

Keep the input/output data structures consistent across various endpoints. Using JSON for data representation is a common and efficient practice.

#### Provide Pagination and Filtering for Large Data Sets

For endpoints returning lists, incorporate pagination (using parameters like page and limit) and filtering (like name or status) to ensure the stability and manageability of the responses.

#### Establish Versioning for Future Compatibility

By including a version number in the URL, such as /v1/endpoint, you empower clients with the ability to transition to newer versions of your API when changes are introduced.

#### Towards Predictable Endpoint Design

Unique vs. Shared Endpoints
Consider using unique endpoints when actions are independent, and shared endpoints when there's a clear resource connection. For example:

- **Unique Endpoint**: /calculate-tax?amount={amount}&state={state}

- **Resource-Centric**: /orders/{id}/payment

#### Endpoint Consistency for Actions

Standardize your endpoints by choosing context-aware verbs. For instance:

- **General Action**: GET /health (retrieve the system's health status)

- **Resource-Specific**: GET /users/{id}/profile (retrieve the user's profile)

#### On Method Overloading

Strive for clarity; explicit endpoints are often more comprehensible.

However, if you need to support multiple actions on a single resource, it could look like this:

- **Single Action**: POST /users/{id}/suspend (suspends a user account)

- **Alternative Method**: POST /users/{id}/actions with a corresponding JSON body indicating the desired action.

#### Versatile Query Parameters

Instead of numerous endpoints with slight variations, leverage query parameters for flexibility. For example, instead of:

#### GET /cars/verified

#### GET /cars/active

- **Use a format such as**: GET /cars?status=verified where possible. This streamlined approach not only maintains a cleaner endpoint structure but also eases the cognitive load on clients when learning and using the API.

- **Unique Identifiers**: Singular vs. Plural

While it's common to expect plurals with endpoints representing collections, catering to both forms can enhance accessibility. An endpoint could manage this by using:

- **Singular Format**: /car/{id} (fetches a specific car)

- **Plural Format**: /cars (fetches multiple cars)

**Examples:**

#### Code Example: REST API Design

Here is the Python code:

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

# Sample in-memory data
users = {
    1: {'id': 1, 'name': 'Alice', 'email': 'alice@example.com'},
    2: {'id': 2, 'name': 'Bob', 'email': 'bob@example.com'}
}

# Non-RESTful - Not recommended
@app.route('/create-user', methods=['POST'])
def create_user():
    data = request.json
    user_id = max(users.keys(), default=0) + 1
    users[user_id] = data
    return jsonify({'id': user_id}), 201

# RESTful - Preferred
@app.route('/users', methods=['POST'])
def create_user_restful():
    data = request.json
    user_id = max(users.keys(), default=0) + 1
    users[user_id] = data
    return jsonify({'id': user_id}), 201

# Supporting both singular and plural forms
@app.route('/user/<int:user_id>', methods=['GET'])
@app.route('/users', methods=['GET'])
def get_user(user_id=None):
    if user_id is not None:
        return jsonify(users.get(user_id))
    return jsonify(users.values())

if __name__ == '__main__':
    app.run()
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>When designing an API, how would you document it for end-users?</summary>

### When designing an API, how would you document it for end-users?

**Definition:**

API documentation is crucial for its ease of use. It serves as a comprehensive guide for developers to understand how the API functions and how to interact with it.

**How it works:**

#### API Documentation Tools

Several efficient API documentation tools are available, including:

- **Swagger**: Employs a declarative JSON/YAML format to specify REST API details.

- **API Blueprint**: Embeds markdown in a code block format to outline API requirements.

- **RAML**: Uses YAML to ensure a clear, consolidated API blueprint.

- **OAS (Formerly Swagger 2.0)**: Focuses on consolidating API reference data to generate interactive, highly dynamic API documentation.

#### Self-Documenting APIs

Modern languages like Python and Java allow for self-documentation in APIs. By including code, developers can see documentation in real-time with tools such as:

- **Javadoc**: Key for Java, this tool integrates shifted HTML comments into Java source code.

- **Doxygen**: Suitable for multiple languages, it processes structured comments to generate a range of outputs.

Implementing self-documentation in your API codebase ensures documentation remains up-to-date and in sync with the codebase.

#### Graphical Representation

"Visual Documentation" or API diagrams present a system of resources, their relationships, data flows, and function descriptions in an easy-to-grasp graphical format.

Some tools that serve this purpose include:

- **Postman**: Helps designers visualize APIs with a dedicated user interface, easing the integration process.

- **API Workbench**: An interactive "IDE" for developing APIs that supports API lifecycle management.

**Examples:**

#### Code Examples: Self-Documenting API

Here is the Python code:

```python
class Student:
    """
    A simple student class.

    Attributes:
        name (str): The name of the student.
        age (int): The age of the student.
        subjects (list of str): The subjects the student is enrolled in.
    """

    def __init__(self, name, age, subjects=None):
        """
        Initializes a Student.

        Args:
            name (str): The name of the student.
            age (int): The age of the student.
            subjects (list of str, optional): The subjects the student is enrolled in.
        """
        self.name = name
        self.age = age
        self.subjects = subjects if subjects else []

    def enroll_in_subject(self, new_subject):
        """
        Enrolls the student in a new subject.

        Args:
            new_subject (str): The name of the new subject to enroll in.
        """
        self.subjects.append(new_subject)
```

In this Python class, you can see well-documented attributes, methods, and the class itself using structured comments that can be extracted by documentation tools.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>What considerations might influence how you paginate API responses?</summary>

### What considerations might influence how you paginate API responses?

**Definition:**

Let's look at what aspects you should consider when paginating API responses.

**How it works:**

#### Common Pagination Techniques

#### Offset-Based Paging

Utilizes offset to specify a starting point and a limit to define the number of records to return.
Simple to implement but can lead to performance issues, especially with very large datasets.

#### Keyset Pagination

Uses key attributes, such as the record's unique identifier, to establish the starting point for the next set of results.
Often efficient but requires attributes to be monotonic to ensure coverage and avoid duplication.

#### Timestamp Pagination

Applies a timestamp associated with the record to decide where to start and how far back to go.
Straightforward and adept at handling constantly changing datasets.

#### Cursor-Based Pagination

Pavilion - Employs cursors, usually in the form of encoded tokens, to pinpoint the current position in the dataset.
Efficient and doesn't suffer from drawbacks like the "skipping" behavior of offset-based paging.

#### Hybrid Paging

The flexibility of databases often allows for a combination of the above methods based on specific application requirements.

#### Choosing the Best Paging Technique

#### Dataset Characteristics

The size of the dataset, its volatility, and distribution can all impact selection. For instance, a dataset with frequently inserted or removed records might benefit from cursor-based paging to maintain consistency.

- **The selectivity of the data attributes is another vital consideration**: Keyset and timestamp-based paging work best with unique and ordered attributes, while cursor-based methods might be more adaptable with non-unique attributes.

#### Relationships and Data Integrity

If your data has strong relationships that need to be maintained across paginated sets, key-driven methods are often preferred.

#### Performance Considerations

Before choosing a method, test its performance with sets of various sizes and data configurations.

#### Security and Caching

Each method influences security and caching differently. It's important to evaluate how caching and security mechanisms integrate with your chosen approach.

#### Client Complexity

Consider the ease of use for API consumers; some methods may introduce additional complexities to client code.

#### Ordering Requirements

If a specific order (e.g., chronology) is mandated, ensure that the chosen method can reliably deliver results in that order.

#### Error Handling

The likelihood and resolution of errors during pagination can differ between methods.

#### Versioning and Field Evolution

Data structures evolve, potentially making a once-reliable key unsuitable for continued paging.

#### Environmental Limitations

Some platforms or tooling might best integrate with particular pagination techniques.

#### Future Proofing and Flexibility

Continually evaluate how the chosen method satisfies both present and foreseeable future criteria.

**Examples:**

#### Implementing Range-Based Pagination

Here is the Python code:

```python
def paginate_using_range(start, end, limit):
    results = get_results_in_range(start, end, limit)
    next_start = results[-1].id + 1  # Assuming results are sorted by id
    next_end = next_start + limit - 1 if len(results) == limit else None

    return {
        "data": results,
        "links": {
            "next": f"/resource?start={next_start}&end={next_end}&limit={limit}" 
        }
    }
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/api-design-interview-questions/
</details>

<details>
<summary>What is HATEOAS, and how does it relate to API design?</summary>

### What is HATEOAS, and how does it relate to API design?

**Definition:**

HATEOAS (Hypermedia as the Engine of Application State) is a REST constraint. The response contains hypermedia links, and those links drive what the client does next.

**How it works:**

A GET of a department can return links with href, rel, and type, for example href 10/employees, rel employees, type GET. The same link can be a Link header: Link: <10/employees>; rel="employees". RFC 5988 defines href, rel, and optional hreflang, media, title, and type. HAL uses application/hal+json or application/hal+xml and the same href, rel, and type. The client starts at one URI and follows server links, so it does not hardcode URI shapes. The server can change those URIs without breaking the client. JSON itself has no single standard for links.

**Examples:**

```json
{
  "departmentId": 10,
  "links": [{ "href": "10/employees", "rel": "employees", "type": "GET" }]
}
```

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>How would you handle localization and internationalization in APIs?</summary>

### How would you handle localization and internationalization in APIs?

**Definition:**

Neither page is a localization guide. RFC 5988, as the HATEOAS page quotes it, includes hreflang on a link. The CORS page lists Accept-Language as a header a simple request may set.

**How it works:**

hreflang describes the language of the link target. Accept-Language is one of the CORS-safelisted request headers, together with Accept, Content-Language, Content-Type, and a single Range. The pages do not describe message catalogs or locale negotiation beyond those fields.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>What are some best practices for designing error responses in an API?</summary>

### What are some best practices for designing error responses in an API?

**Definition:**

The pages do not define an error body. MDN does say a CORS failure is an error whose details JavaScript cannot read.

**How it works:**

The script only knows that an error occurred. The specific cause is in the browser console. That is deliberate. The HATEOAS page shows a successful 200 with a Link header, not an error document.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>What are the benefits of using API Gateways?</summary>

### What are the benefits of using API Gateways?

**Definition:**

An API gateway is not described on either page.

**How it works:**

The HATEOAS page’s front is the initial URI plus links the server returns. The CORS page’s front is the browser’s same-origin check and the server’s Access-Control-Allow-Origin opt-in. Neither is a gateway product.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>How would you approach handling file uploads in an API design?</summary>

### How would you approach handling file uploads in an API design?

**Definition:**

The CORS page treats multipart/form-data as a content type that can keep a POST a simple request. Listening to XMLHttpRequest.upload makes the request no longer simple, so the browser preflights it.

**How it works:**

A simple POST may use application/x-www-form-urlencoded, multipart/form-data, or text/plain. If code calls xhr.upload.addEventListener, or the body is a ReadableStream, the request is not simple. Methods other than GET, HEAD, and POST, and POST with other content types, are preflighted with OPTIONS before the actual request. The HATEOAS page does not discuss uploads.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

### API Performance and Scalability

<details>
<summary>How can caching be incorporated into API design to improve performance?</summary>

### How can caching be incorporated into API design to improve performance?

**Definition:**

Neither page describes a cache or Cache-Control.

**How it works:**

The CORS page does list Range, with a single range, as a safelisted request header. That is a request for part of a resource, not a stored response. HATEOAS responses carry links; the page does not say those links are cached.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>What are some strategies for dealing with high traffic volumes in a scalable API?</summary>

### What are some strategies for dealing with high traffic volumes in a scalable API?

**Definition:**

Neither page describes a traffic strategy such as sharding or a queue.

**How it works:**

What they do change under load is the work the browser adds. A non-simple call sends OPTIONS first and only then the real request, if the server approves. HATEOAS lets the server change URIs without clients hardcoding them, which is an evolution rule, not a capacity rule.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>How does connection pooling work and how can it benefit API performance?</summary>

### How does connection pooling work and how can it benefit API performance?

**Definition:**

Connection pooling is not on either page.

**How it works:**

The extra connection the CORS page does specify is the preflight. The browser sends OPTIONS with the method and headers it intends to use, and sends the actual request only after the server allows it. That is a second HTTP exchange, not a pool.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>When is it appropriate to use synchronous vs asynchronous processing in an API?</summary>

### When is it appropriate to use synchronous vs asynchronous processing in an API?

**Definition:**

The pages do not define a job queue versus a request/response call. They do show a call that waits for a prior OPTIONS response.

**How it works:**

For a non-simple request the browser must finish the preflight before it sends the actual method. GET, HEAD, and a POST whose content type is form-urlencoded, multipart, or text/plain can skip that wait when the other simple-request conditions hold. The HATEOAS client also waits for a response before it follows the next link. Neither page names background processing.

**When to use:**

Простой запрос — когда метод и заголовки подходят и не нужен круг OPTIONS. Путь с preflight — когда метод или Content-Type вне этого списка.

**Sources:**

- U
- s
- e
-  
- a
-  
- s
- i
- m
- p
- l
- e
-  
- r
- e
- q
- u
- e
- s
- t
-  
- w
- h
- e
- n
-  
- t
- h
- e
-  
- m
- e
- t
- h
- o
- d
-  
- a
- n
- d
-  
- h
- e
- a
- d
- e
- r
- s
-  
- q
- u
- a
- l
- i
- f
- y
-  
- a
- n
- d
-  
- y
- o
- u
-  
- d
- o
-  
- n
- o
- t
-  
- w
- a
- n
- t
-  
- t
- h
- e
-  
- O
- P
- T
- I
- O
- N
- S
-  
- r
- o
- u
- n
- d
-  
- t
- r
- i
- p
- .
-  
- U
- s
- e
-  
- t
- h
- e
-  
- p
- r
- e
- f
- l
- i
- g
- h
- t
-  
- p
- a
- t
- h
-  
- w
- h
- e
- n
-  
- t
- h
- e
-  
- m
- e
- t
- h
- o
- d
-  
- o
- r
-  
- C
- o
- n
- t
- e
- n
- t
- -
- T
- y
- p
- e
-  
- i
- s
-  
- o
- u
- t
- s
- i
- d
- e
-  
- t
- h
- a
- t
-  
- l
- i
- s
- t
- .
</details>

<details>
<summary>What are some challenges of maintaining an API at scale?</summary>

### What are some challenges of maintaining an API at scale?

**Definition:**

The HATEOAS page’s scaling problem for clients is hardcoded URIs. If every client bakes in paths, a server-side URI change breaks them.

**How it works:**

Links in the body or in a Link header let the server move resources. The client keeps the first URI and the relation types. The CORS side of scale is operational: every non-simple call from a browser pays for an OPTIONS request, and a missing Access-Control-Allow-Origin fails the call in a way script cannot inspect.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

### API Security Considerations

<details>
<summary>What are common security concerns when designing an API?</summary>

### What are common security concerns when designing an API?

**Definition:**

Browsers apply the same-origin policy to fetch and XMLHttpRequest. A cross-origin response is readable by the script only if the server opts in with CORS headers.

**How it works:**

The example is a page on https://domain-a.com calling https://domain-b.com/data.json. The request carries Origin. Methods other than GET, and POST with a content type outside the three simple ones, are preflighted. Servers also say whether credentials such as cookies and HTTP authentication may be sent. CORS errors hide the reason from JavaScript. The HATEOAS page does not discuss authentication.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>How do you prevent injection attacks in API design?</summary>

### How do you prevent injection attacks in API design?

**Definition:**

Injection is not discussed on either page.

**How it works:**

The CORS page’s related warning is CSRF. Simple requests look like form posts, which HTML could already send cross-site, so the server must already defend against cross-site request forgery. CORS does not replace that defense. The preflight does not inspect a query string or a JSON body for injected commands.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Can you explain what CORS is and why it’s important in API design?</summary>

### Can you explain what CORS is and why it’s important in API design?

**Definition:**

CORS is an HTTP-header mechanism. The server lists origins other than its own from which a browser may read a response. Browsers otherwise block cross-origin reads from fetch and XMLHttpRequest.

**How it works:**

The browser sends Origin. For non-simple requests it first sends OPTIONS advertising the method and headers. The server opts in with Access-Control-Allow-Origin; without that header the script cannot read the response even if the server processed the request. Simple requests use GET, HEAD, or POST, only safelisted headers, and Content-Type of application/x-www-form-urlencoded, multipart/form-data, or text/plain. Credentials are cookies and HTTP authentication, and the server must say they may be sent. Failure details stay in the browser console.

**Examples:**

```javascript
fetch("https://bar.other").then((response) => response.json());
```

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>What is the purpose of an API key?</summary>

### What is the purpose of an API key?

**Definition:**

An API key is not defined on either page.

**How it works:**

The credential types the CORS page names are cookies and HTTP authentication. The server decides whether those credentials are sent on the cross-origin request. A key in a header would also have to be a safelisted header or the browser will preflight. The page does not list an API-key header as safelisted.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>How would you implement authentication and authorization in APIs?</summary>

### How would you implement authentication and authorization in APIs?

**Definition:**

The pages do not specify an authentication scheme such as OAuth. CORS does say the server controls whether credentials are included.

**How it works:**

Credentials here are cookies and HTTP authentication. A simple request is still sent, but the script cannot read the response unless Access-Control-Allow-Origin allows that origin. HATEOAS links name a type such as GET; they do not carry an authorization decision.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

### Advanced API Design Concepts

<details>
<summary>What is the role of an API Gateway in microservices architecture?</summary>

### What is the role of an API Gateway in microservices architecture?

**Definition:**

Neither page places an API gateway in front of microservices.

**How it works:**

HATEOAS keeps the client on server-supplied links so URI layout can change per resource. CORS keeps a browser from reading a response from another origin until that origin opts in. Those are the two fronts these pages actually describe.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>Can you explain the concept of a headless API?</summary>

### Can you explain the concept of a headless API?

**Definition:**

Headless is not a term on either page. The HATEOAS client is a consumer that follows links and does not hardcode the server’s URI tree.

**How it works:**

The client starts at one URI, reads href and rel, and discovers actions. The page compares that to a person clicking through a site. It does not describe a CMS or a rendering split.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>How do GraphQL APIs differ from traditional RESTful APIs?</summary>

### How do GraphQL APIs differ from traditional RESTful APIs?

**Definition:**

GraphQL is not on either page. HATEOAS is a REST constraint: the response lists the next resources.

**How it works:**

The department example returns links rather than expecting the client to know 10/employees in advance. HAL packages those links as application/hal+json or application/hal+xml. A query language that lets the client name fields is not described.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>What is gRPC and how might it be used in API design?</summary>

### What is gRPC and how might it be used in API design?

**Definition:**

gRPC is not on either page.

**How it works:**

The HTTP mechanisms they do name are GET in the department example, the Link header, and the CORS methods GET, HEAD, POST, plus OPTIONS for preflight. No RPC framing is specified.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>How can WebSockets enhance API functionalities?</summary>

### How can WebSockets enhance API functionalities?

**Definition:**

WebSockets are not on either page.

**How it works:**

The browser APIs the CORS page names are fetch and XMLHttpRequest, plus fonts, WebGL textures, canvas images, and CSS shapes. A long-lived socket is not one of them. HATEOAS navigation is a sequence of requests that follow links.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

### API Development and Testing

<details>
<summary>What tools or frameworks do you use to develop and test APIs?</summary>

### What tools or frameworks do you use to develop and test APIs?

**Definition:**

The HATEOAS page says each REST framework builds links in its own way. Its example is links on the resource model classes that are sent to the client. It names that pattern in a Spring Boot HATEOAS tutorial and does not show the annotations.

**How it works:**

The CORS page’s client tool is fetch or XMLHttpRequest. It does not name a test runner. No performance tool is listed.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>How would you test for API performance and what metrics would you track?</summary>

### How would you test for API performance and what metrics would you track?

**Definition:**

Neither page defines a performance test or a metric.

**How it works:**

The extra latency they do make visible is the CORS preflight: OPTIONS, then the real request, for any call that is not simple. A HATEOAS client adds a request per link it follows. No throughput number is published.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>What is contract testing in the context of API development?</summary>

### What is contract testing in the context of API development?

**Definition:**

Contract testing is not on either page.

**How it works:**

The closest published contract is the link relation. RFC 5988 and HAL both use href and rel so the client knows how the current resource relates to the target. HAL also has type for the expected media type, application/hal+json or application/hal+xml. That is a response shape, not a test suite.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>Describe the mocking of APIs for development and testing purposes.</summary>

### Describe the mocking of APIs for development and testing purposes.

**Definition:**

Mocking an API is not on either page.

**How it works:**

What the HATEOAS page says a client needs is the initial URI and the links in the response. A client that hardcodes later URIs is the case the page says breaks when the server changes paths. The CORS page’s client is a real fetch to another origin, with Origin sent.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>How can automated API testing improve the software development lifecycle?</summary>

### How can automated API testing improve the software development lifecycle?

**Definition:**

Automated API testing is not discussed on either page.

**How it works:**

The client behavior they specify can still be checked: a HATEOAS response contains href and rel, and a cross-origin fetch is readable only when Access-Control-Allow-Origin allows the Origin. The pages do not describe a pipeline that runs those checks.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

### API Data Management

<details>
<summary>How do you handle large volumes of data in API responses?</summary>

### How do you handle large volumes of data in API responses?

**Definition:**

Large payloads are not given a strategy on either page. The CORS page does allow a single Range header on a simple request.

**How it works:**

Examples of that header are bytes=256- and bytes=127-255. More than one range is outside the safelisted form. HATEOAS does not say to page a collection; its department example returns one department plus a link to employees.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>How can an API be designed to support multiple data formats?</summary>

### How can an API be designed to support multiple data formats?

**Definition:**

HAL publishes two media types, application/hal+json and application/hal+xml. The HATEOAS page says to pick one link format and keep it. Links may also be a Link header instead of a JSON body.

**How it works:**

On the CORS side, Content-Type is safelisted only for application/x-www-form-urlencoded, multipart/form-data, and text/plain. application/json and application/hal+json are not in that list, so a browser POST of those types is preflighted. RFC 5988 attributes include type.

**When to use:**

Use HAL when the body should carry href, rel, and type in JSON or XML. Use a Link header when the links should stay out of the body. Expect a browser to preflight a JSON content type.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>What are best practices for managing sensitive data through an API?</summary>

### What are best practices for managing sensitive data through an API?

**Definition:**

Sensitive-data handling beyond the browser’s cross-origin rules is not specified. CORS treats cookies and HTTP authentication as credentials the server must explicitly allow.

**How it works:**

A cross-origin script cannot read the response without Access-Control-Allow-Origin. The server separately decides whether credentials are sent. CORS failures do not reveal why to JavaScript. The pages do not describe field encryption or a vault.

**Sources:**

- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>In an API, how do you approach filtering and sorting of data?</summary>

### In an API, how do you approach filtering and sorting of data?

**Definition:**

Filtering and sorting are not on either page.

**How it works:**

The HATEOAS department link is rel employees pointing at 10/employees. The page does not add a query string for filter or sort. The client is expected to follow the link the server provided rather than invent parameters.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>Can you discuss strategies for transaction management within API endpoints?</summary>

### Can you discuss strategies for transaction management within API endpoints?

**Definition:**

Transaction management inside an endpoint is not on either page.

**How it works:**

HATEOAS changes application state by the link the client follows next, for example from a department to its employees. That is navigational state, not a database transaction. CORS OPTIONS does not commit or roll back anything.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

### API Specification and Standards

<details>
<summary>Are you familiar with any API specification formats and what benefits do they provide?</summary>

### Are you familiar with any API specification formats and what benefits do they provide?

**Definition:**

The HATEOAS page names two link formats: RFC 5988 and HAL. It says there is no single JSON standard for links, so you pick one format and stay with it.

**How it works:**

RFC 5988 links have a target IRI (href), a relation (rel), and optional hreflang, media, title, and type. HAL is described as a draft with media types application/hal+json and application/hal+xml, and link properties href, rel, and type. The benefit stated is that clients discover actions and survive URI changes. OpenAPI is not on the page.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>How do you approach the design of an API to comply with industry standards?</summary>

### How do you approach the design of an API to comply with industry standards?

**Definition:**

The pages’ standards for this are RFC 5988 link relations and the CORS request rules, not a checklist of industry regimes.

**How it works:**

A link that other clients can follow uses href and rel. A browser client on another origin needs Access-Control-Allow-Origin and, for non-simple calls, a successful OPTIONS preflight. HAL’s type says which media type to expect. The pages do not discuss PCI, HIPAA, or similar regimes.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>How do you foresee OpenAPI/Swagger specifications evolving, and how do they impact API design?</summary>

### How do you foresee OpenAPI/Swagger specifications evolving, and how do they impact API design?

**Definition:**

OpenAPI and Swagger are not discussed, and neither page forecasts how they will change.

**How it works:**

The evolution the HATEOAS page does describe is on the server: URI paths can change because clients follow href values instead of hardcoding them. HAL is explicitly called a draft, with application/hal+json and application/hal+xml. That is the specification status on the page, not a roadmap.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
</details>

<details>
<summary>How can APIs be designed with interoperability in mind?</summary>

### How can APIs be designed with interoperability in mind?

**Definition:**

HATEOAS interoperability is shared link relations. Clients that understand href and rel can move between resources without a private URI map.

**How it works:**

RFC 5988 is the web-linking framework the page cites. HAL is one JSON or XML convention for the same href, rel, and type. The page says either a JSON body or a Link header is a valid way to send the links, and you should not mix formats casually: pick one. A browser on another origin still needs the CORS opt-in or it cannot read that body.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
- [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
</details>

<details>
<summary>How does a standard like JSON:API influence the way you structure response payloads?</summary>

### How does a standard like JSON:API influence the way you structure response payloads?

**Definition:**

JSON:API is not specified on either page. The JSON link conventions they do specify are HAL and a plain links array.

**How it works:**

The department example uses a links array of href, rel, and type. HAL uses application/hal+json and the same three properties, and is described as a draft. RFC 5988’s Link header is the non-body alternative. The page says to choose one of these and keep it, because JSON has no universal link format.

**Sources:**

- [HATEOAS](https://restfulapi.net/hateoas/)
</details>
