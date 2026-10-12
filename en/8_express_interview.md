# Express.js — Interview

Express.js interview questions.

[← Back to hub](./8_nodejs.md)

### Related docs

- [Node.js — HTTP](./8_nodejs_http.md) · [Express.js — Interview](./8_express_interview.md) · [Node.js — Interview](./8_nodejs_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Express.js](https://devinterview.io/questions/web-and-mobile-development/express-interview-questions/).

### Express.js Fundamentals

<details>
<summary>What is Express.js, and how does it relate to Node.js?</summary>

### What is Express.js, and how does it relate to Node.js?

**Definition:**

Express.js is a web application framework that runs on Node.js. It simplifies the process of building web applications and APIs by providing a range of powerful features, including robust routing, middleware support, and HTTP utility methods. Thanks to its modular design, you can expand its functionality through additional libraries and Node.js modules.

**How it works:**

#### Key Features

Middleware: Express.js makes use of middleware functions that have access to the request-response cycle. This allows for a variety of operations such as logging, authentication, and data parsing.

Routing: The framework offers a flexible and intuitive routing system, making it easy to handle different HTTP request methods on various URLs.

Templates: Integrated support for template engines enables the dynamic rendering of HTML content.

HTTP Methods: It provides built-in methods for all HTTP requests, such as get, post, put, delete, simplifying request handling.

Error Handling: Express streamlines error management, and its middleware functions can specifically handle errors.

RESTful APIs: Its features such as request and response object chaining, along with HTTP method support, make it ideal for creating RESTful APIs.

#### Relationship with Node.js

Express.js is a web application framework specifically designed to extend the capabilities of Node.js for web development. Node.js, on the other hand, is a cross-platform JavaScript runtime environment that allows developers to build server-side and networking applications.

Express.js accomplishes this through a layer of abstractions and a more structured approach, which Node.js, by itself, doesn't provide out of the box.

#### Code Example: Basic Express Server

Here is the Node.js code:

**Examples:**

```javascript
// Import required modules
const express = require('express');

// Create an Express application
const app = express();
const port = 3000;

// Define a route and its callback function
app.get('/', (req, res) => {
  res.send('Hello World!');
});

// Start the server
app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}/`);
});
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Explain the concept of middleware in Express.js.</summary>

### Explain the concept of middleware in Express.js.

**Definition:**

Middleware acts as a bridge between incoming HTTP requests and your Express.js application, allowing for a range of operations such as parsing request bodies, handling authentication, and even serving static files.

**How it works:**

#### Middleware Functions

A middleware function in Express is a handler invoked in sequence when an HTTP request is received. It has access to the request and response objects, as well as the next function to trigger the next middleware in line.

Each middleware function typically follows this signature:

Note that the next() call is essential to move on to the next middleware.

#### Types of Middleware

#### Application-Level Middleware

Registered via app.use(middlewareFunction), it's active for every incoming request, making it suitable for tasks like request logging or establishing cross-cutting concerns.

#### Router-Level Middleware

Operates on specific router paths and is defined using router.use(middlewareFunction). It's useful for tasks related to particular sets of routes.

Error-Handling Middleware

#### Built-In Middleware

Express offers ready-to-use middleware for tasks like serving static files or parsing the request body.

#### Middleware Chaining

By sequentially calling next() within each middleware, you form a chain, facilitating a cascade of operations for an incoming request.

Consider a multi-tiered security setup, for example, with authentication, authorization, and request validation. Only when a request passes through all three tiers will it be processed by the actual route handler.

#### Code Example: Middleware Chaining

Here is the code:

**Examples:**

```javascript
function middlewareFunction(req, res, next) {
    // ...middleware logic
    next(); // or next(err); based on whether to proceed or handle an error
}
```

```javascript
Recognizable via its function signature (err, req, res, next), this type of middleware specifically handles errors. In the middleware chain, it should be placed after regular middlewares and can be added using app.use(function(err, req, res, next) { ... }).
```

```javascript
const express = require('express');
const app = express();

// Sample middleware functions
function authenticationMiddleware(req, res, next) {
    console.log('Authenticating...');
    next();
}

function authorizationMiddleware(req, res, next) {
    console.log('Authorizing...');
    next();
}

function requestValidationMiddleware(req, res, next) {
    console.log('Validating request...');
    next();
}

// The actual route handler
app.get('/my-secured-endpoint', authenticationMiddleware, authorizationMiddleware, requestValidationMiddleware, (req, res) => {
    res.send('Welcome! You are authorized.');
});

app.listen(3000);
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>How would you set up a basic Express.js application?</summary>

### How would you set up a basic Express.js application?

**Definition:**

To set up a basic Express.js application, follow these steps:

**How it works:**

#### 1. Initialize the Project

Create a new directory for your project and run npm init to generate a package.json file.

#### 2. Install Dependencies

Install Express as a dependency using the Node Package Manager (NPM):

#### 3. Create the Application

In your project directory, create a main file (usually named app.js or index.js) to set up the Express application.

Here is the JavaScript code:

#### 4. Run the Application

You can start your Express server using Node.js:

For convenience, you might consider using Nodemon as a development dependency which automatically restarts the server upon file changes.

**Examples:**

```bash
npm install express
```

```javascript
// Import the Express module
const express = require('express');

// Create an Express application
const app = express();

// Define a sample route
app.get('/', (req, res) => {
  res.send('Hello, World!');
});

// Start the server
const port = 3000;
app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```

```bash
node app.js
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>What is the purpose of the app.use() function?</summary>

### What is the purpose of the app.use() function?

**Definition:**

In Express.js, the app.use() function is a powerful tool for middleware management. It can handle HTTP requests and responses, as well as prepare data or execute processes in between.

**How it works:**

#### Key Functions

Global Middleware: Without a specified path, the middleware will process every request.
Route-specific Middleware: When given a path, the middleware will only apply to the matched routes.

#### Chaining Middleware

You can stack multiple middleware using app.use() in the order they need to execute. For a matched route, control can be passed to the next matching route or terminated early using next().

**When to use:**

#### Common Use-Cases

Body Parsing: To extract data from incoming requests, especially useful for POST and PUT requests.

Handling CORS: Useful in API applications to manage cross-origin requests.

Static File Serving: For serving files like images, CSS, or client-side JavaScript.

Logging: To record request details for debugging or analytics.

Error Handling: To manage and report errors during request processing.

**Examples:**

```javascript
const bodyParser = require('body-parser');
app.use(bodyParser.json());
```

```javascript
app.use(function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
    next();
});
```

```javascript
app.use(express.static('public'));
```

```javascript
app.use(function(req, res, next) {
    console.log(`${new Date().toUTCString()}: ${req.method} ${req.originalUrl}`);
    next();
});
```

```javascript
app.use(function(err, req, res, next) {
    console.error(err);
    res.status(500).send('Internal Server Error');
});
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>How do you serve static files using Express.js?</summary>

### How do you serve static files using Express.js?

**Definition:**

In an Express.js web application, you often need to serve static files such as stylesheets, client-side JavaScript, and images. You can accomplish this using the express.static middleware.

**How it works:**

#### Middleware for Serving Static Files

The express.static middleware function serves static files and is typically used to serve assets like images, CSS, and client-side JavaScript.

Here is the code example:

In this example, the folder named public will be used to serve the static assets.

#### Additional Configuration with Method Chaining

You can further configure the behavior of the express.static middleware by chaining methods.

For example, to set the cache-control header, the code looks like this:

Here, the '1d' ensures that caching is enabled for a day.

#### Using a Subdirectory

If you want to serve files from a subdirectory, you can specify it when using the express.static middleware.

Here is the code example:

This serves the files from the public folder but any requests for these files should start with /static.

What express.static Serves
Images: PNG, JPEG, GIF
Text Content: HTML, CSS, JavaScript
Fonts

#### JSON Data

#### Not for dynamic content

While express.static is excellent for static assets, it's not suitable for dynamic content or data in POST requests.

**Examples:**

```javascript
app.use(express.static('public'));
```

```javascript
app.use(express.static('public', {
    maxAge: '1d'
}));
```

```javascript
app.use('/static', express.static('public'));
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Discuss the difference between app.get() and app.post() in Express.js.</summary>

### Discuss the difference between app.get() and app.post() in Express.js.

**Definition:**

In Express.js, app.get() and app.post() are two of the most commonly used HTTP method middleware. The choice between them (or using both) typically depends on whether you are retrieving or submitting/persisting data.

**How it works:**

#### Key Distinctions

#### HTTP Verbs: External Visibility

app.get(): Listens for GET requests. Designed for data retrieval. Visible URLs typically trigger such requests (e.g., links or direct URL entry in the browser).

app.post(): Listens for POST requests. Intended for data submission. Typically not visible in the URL bar, commonly used for form submissions.

#### Data Transmission

app.get(): Uses query parameters for data transmission, visible in the URL. Useful for simple, non-sensitive, read-only data (e.g., filtering or pagination).

app.post(): Uses request body for data transmission, which can be in various formats (e.g., JSON, form data). Ideal for more complex data, file uploads, or sensitive information.

Using Both app.get() and app.post() for the Same Route
There are cases, especially for RESTful design, where a single URL needs to handle both data retrieval and data submission.

#### Resource Retrieval and Creation:

Fetch a Form: Use app.get() to return a form for users to fill out.
Form Submission: Use app.post() to process and save the submitted form data.
Complete Entity Modification: For a complete update (or replacement in REST), using app.post() ensures that the update action is triggered via a post request, not a get request. This distiction is important to obey the RESTful principles.
Code Example: Using both app.get() and app.post() for a single route

Here is the JavaScript code:

**Examples:**

```javascript
const userRecords = {}; // in-memory "database" for the sake of example

// Handle user registration form
app.get('/users/register', (req, res) => {
    res.send('Please register: <form method="POST"><input name="username"></form>');
});

// Process submitted registration form
app.post('/users/register', (req, res) => {
    userRecords[req.body.username] = req.body;
    res.send('Registration complete');
});
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>How do you retrieve the URL parameters from a GET request in Express.js?</summary>

### How do you retrieve the URL parameters from a GET request in Express.js?

**Definition:**

In Express.js, you can extract URL parameters from a GET request using the req.params object. Here's a quick look at the steps and the code example:

**How it works:**

#### Code Example: Retrieving URL Parameters

In this example, the URL parameter id is extracted and used to fetch the corresponding user data.

#### Additional Steps for Complex GET Requests

For simple and straightforward GET requests, supplying URL parameters directly works well. However, for more complex scenarios, such as parsing parameters from a URL with the help of querystrings or handling optional parameters, Express.js offers more advanced techniques which are outlined below:

#### Parsing Query Parameters

What It Is: Additional data passed in a URL after the ? character. Example: http://example.com/resource?type=user&page=1.

How to Access It: Use req.query, an object that provides key-value pairs of the parsed query parameters.

#### Code Example: Parsing Query Parameters

#### Optional and Catch-All Segments

Optional Segments: URL segments enclosed in parentheses are optional and can be accessed using req.params. Example: /book(/:title)

Catch-All Segments: Captures the remainder of the URL and is useful in cases like URL rewriting. Denoted by an asterisk (*) or double asterisk (**). Accessed using req.params as well. Example: /documents/*

**Examples:**

```javascript
// Sample URL: http://example.com/users/123
// Relevant Route: /users/:id

// Define the endpoint/route
app.get('/users/:id', (req, res) => {
    // Retrieve the URL parameter
    const userId = req.params.id;
    // ... (rest of the code)
});
```

```javascript
app.get('/search', (req, res) => {
    const { q, category } = req.query;
    // ... (rest of the code)
});
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>What are route handlers, and how would you implement them?</summary>

### What are route handlers, and how would you implement them?

**Definition:**

Route handlers in Express.js are middleware functions designed to manage specific paths in your application.

Depending on the HTTP method and endpoint, they can perform diverse tasks, such as data retrieval from a database, view rendering, or HTTP response management.

Code Example: Setting Up a Simple Route Handler

Here is the code:

What Are Route-Handler Chains?
You can associate numerous route-managing middleware functions to a single route. Every middleware function in the chain has to either proceed to the following function using next() or conclude the request-response cycle.

This allows for checks like user authentication before accessing a route.

**How it works:**

#### HTTP Method Convenience Methods

Express.js offers specialized, highly-readable methods for the most common HTTP requests:

app.get()
app.post()
app.put()
app.delete()
app.use()
These methods streamline route handling setup.

**Examples:**

```javascript
// Responds with "Hello, World!" for GET requests to the root URL (/)
app.get('/', (req, res) => {
  res.send('Hello, World!');
});
In this example, the route handler is (req, res) => { res.send('Hello, World!'); }. It listens for GET requests on the root URL and responds with "Hello, World!".
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>How do you enable CORS in an Express.js application?</summary>

### How do you enable CORS in an Express.js application?

**Definition:**

Cross-Origin Resource Sharing (CORS) is a mechanism that allows web pages to make requests to a different domain. In Express.js, you can enable CORS using the cors package or by setting headers manually.

**How it works:**

#### Using the cors Package

#### Install cors:

Use npm or yarn to install the cors package.

Use the app.use(cors()) middleware. You can also customize CORS behavior with options.

Manual CORS Setup
Use the following code example to set CORS headers manually in your Express app:

Make sure to place this middleware before your route definitions.

**Examples:**

```bash
npm install cors
Integrate with Your Express App:
```

```javascript
const express = require('express');
const cors = require('cors');
const app = express();

// Enable CORS for all routes
app.use(cors());

// Example: Enable CORS only for a specific route
app.get('/public-data', cors(), (req, res) => {
    // ...
});

// Example: Customize CORS options
const customCorsOptions = {
    origin: 'https://example.com',
    optionsSuccessStatus: 200 // Some legacy browsers choke on 204
};

app.use(cors(customCorsOptions));
```

```javascript
app.use((req, res, next) => {
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content-Type, Accept');
    if (req.method === 'OPTIONS') {
        res.header('Access-Control-Allow-Methods', 'GET, POST, PUT, PATCH, DELETE, OPTIONS');
        return res.status(200).json({});
    }
    next();
});
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Explain the use of next() in Express.js middleware.</summary>

### Explain the use of next() in Express.js middleware.

**Definition:**

In Express.js, middleware functions are crucial for handling HTTP requests. A single request can pass through multiple middlewares before reaching its endpoint, providing opportunities for tasks like logging, data parsing, and error handling. The next() function is instrumental in this process, allowing for both regular middleware chaining and special error handling.

What is next()?
next(): A callback function that, when called within a middleware, passes control to the next middleware in the stack.
next() is typically invoked to signal that a middleware has completed its tasks and that the request should move on to the next middleware.
If a middleware doesn't call next(), the request flow can get stuck, and the subsequent middlewares won't be executed.

**How it works:**

#### Code Example: Logging Middleware

Here is the code:

In this example, both middlewares call next() to allow the request to progress to the next logging middleware and eventually to the endpoint (not shown, but would be the next in the chain).

Without the next() calls, the request would get stuck after the first middleware.

**When to use:**

#### Use-Cases

Regular Flow: Invoke next() to move the request and response objects through the middleware stack.

**Examples:**

```javascript
const app = require('express')();

// Sample middleware: logs the request method and URL
app.use((req, res, next) => {
    console.log(`${req.method} ${req.url}`);
    next(); // Move to the next middleware
});

// Sample middleware: logs the current UTC time
app.use((req, res, next) => {
    console.log(new Date().toUTCString());
    next(); // Move to the next middleware
});

app.listen(3000);
```

```javascript
Error Handling: If a middleware detects an error, it can short-circuit the regular flow and jump directly to an error-handling middleware (defined with app.use(function(err, req, res, next) {})). This is achieved by calling next(err), where err is the detected error.
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>


### Routing and Requests

<details>
<summary>What is the role of the express.Router class?</summary>

### What is the role of the express.Router class?

**Definition:**

The express.Router is a powerful tool for managing multiple route controllers. It helps in organizing routes and their handling functions into modular, self-contained groups.

**How it works:**

#### Key Features

Modularity: Rely on separate route modules for improved code organization, maintainability, and collaboration.

Middlewares: Like the main express app, the router can also use middlewares to process incoming requests.

HTTP Method Chaining: Simplifies route handling by allowing method-specific routes to be defined using method names.

Example: Middleware and Route Handling

Here is the Node.js code:

Utilize the built-in express.Router.
Attach a general-purpose middleware and two different HTTP method-specific routes.
The router is then integrated into the main express app using:

Here, app.use('/routerExample', router); assigns all routes defined in the router to /routerExample.

**Examples:**

```javascript
const express = require('express');
const router = express.Router();

// Logger Middleware
router.use((req, res, next) => {
  console.log('Router-specific Request Time:', Date.now());
  next();
});

// "GET" method route
router.get('/', (req, res) => {
  res.send('Router Home Page');
});

// "POST" method route
router.post('/', (req, res) => {
  res.send('Router Home Page - POST Request');
});

module.exports = router;
In this example, we:
```

```javascript
const app = express();
const router = require('./myRouterModule');

app.use('/routerExample', router);
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>How do you handle 404 errors in Express.js?</summary>

### How do you handle 404 errors in Express.js?

**Definition:**

Handling 404 errors in Express is essential for capturing and responding to requests for non-existent resources. You typically use both middleware and HTTP response mechanisms for this purpose.

**How it works:**

#### Middleware for 404s

Use app.use at the end of the middleware chain to capture unresolved routes.
Invoke the middleware with next() and an Error object to forward to the error-handling middleware.

Here is the Node.js code example:

#### Error-Handling Middleware for 404s and Other Errors

Define an error-handling middleware with four arguments. The first one being the error object.
Check the error's status and respond accordingly. If it's a 404, handle it as a not-found error; otherwise, handle it as a server error.

Here is the Node.js code:

Full Example:

Here is the complete Node.js application:

**Examples:**

```javascript
app.use((req, res, next) => {
    const err = new Error(`Not Found: ${req.originalUrl}`);
    err.status = 404;
    next(err);
});
```

```javascript
app.use((err, req, res, next) => {
    const status = err.status || 500;
    const message = err.message || "Internal Server Error";

    res.status(status).send(message);
});
```

```javascript
const express = require('express');
const app = express();
const port = 3000;

// Sample router for demonstration
const usersRouter = express.Router();
usersRouter.get('/profile', (req, res) => {
    res.send('User Profile');
});
app.use('/users', usersRouter);

// Capture 404s
app.use((req, res, next) => {
    const err = new Error(`Not Found: ${req.originalUrl}`);
    err.status = 404;
    next(err);
});

// Error-handling middleware
app.use((err, req, res, next) => {
    const status = err.status || 500;
    const message = err.message || "Internal Server Error";
    res.status(status).send(message);
});

app.listen(port, () => {
    console.log(`Example app listening at http://localhost:${port}`);
});
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>What are the differences between req.query and req.params?</summary>

### What are the differences between req.query and req.params?

**Definition:**

In Express.js, req.query is used to access GET request parameters, while req.params is used to capture parameters defined in the URL path.

**How it works:**

#### Understanding Express.js Routing

Express.js uses app.get() and similar functions to handle different types of HTTP requests.

app.get('/users/:id'): Matches GET requests to /users/123 where 123 is the :id parameter in the path.

#### Accessing Request Data

req.query: Utilized to extract query string parameters from the request URL. Example: For the URL /route?id=123, use req.query.id to obtain 123.
req.params: Used to retrieve parameters from the request URL path. For the route /users/:id, use req.params.id to capture the ID, such as for /users/123.

#### Code Example: Request Data

Here is the Express.js server setup:

**Examples:**

```javascript
const express = require('express');
const app = express();
const port = 3000;

// Endpoint to capture query string parameter
app.get('/query', (req, res) => {
  console.log(req.query);
  res.send('Received your query param!');
});

// Endpoint to capture URL parameter
app.get('/user/:id', (req, res) => {
  console.log(req.params);
  res.send('Received your URL param!');
});

app.listen(port, () => console.log(`Listening on port ${port}!`));
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Describe the purpose of req.body and how you would access it.</summary>

### Describe the purpose of req.body and how you would access it.

**Definition:**

In an Express.js application, req.body is a property of the HTTP request object that contains data submitted through an HTTP POST request.

The POST request might originate from an HTML form, a client-side JavaScript code, or another API client. The data in req.body is typically structured as a JSON object or a URL-encoded form.

**How it works:**

#### Middleware and Parsing Request Body

The express.json() and express.urlencoded() middleware parse incoming Request objects before passing them on. These middlewares populate req.body with the parsed JSON and URL-encoded data, respectively.

Here is an example of how you might set up body parsing in an Express app:

#### Accessing req.body Data

Once the body parsing middleware is in place, you can access the parsed data in your route handling functions:

POST or PUT Requests: When a client submits a POST or PUT request with a JSON payload in the request body, you can access this data through req.body.

Here is an example:

Client-side JavaScript:

Server-side Express route handler:

HTML Forms: When a form is submitted using <form> with action pointing to your Express route and method as POST or PUT, and the form fields are input elements within the form, req.body will contain these form field values.

Here is an example:

HTML form:

Custom Parsers: While Express provides built-in body parsers for JSON and URL-encoded data, you might receive data in another format. In such cases, you can create custom middleware to parse and shape the data as needed. This middleware should populate req.body.

**Examples:**

```javascript
const express = require('express');
const app = express();

// Parse JSON and URL-encoded data into req.body
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
```

```javascript
fetch('/example-route', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ key: 'value' })
});
```

```javascript
app.post('/example-route', (req, res) => {
  console.log(req.body); // Outputs: { key: 'value' }
});
```

```html
<form action="/form-endpoint" method="POST">
  <input type="text" name="username" />
  <input type="password" name="password" />
  <button type="submit">Submit</button>
</form>
Express route:

app.post('/form-endpoint', (req, res) => {
  console.log(req.body.username, req.body.password);
});
A modern technique for sending form data using fetch is by setting the Content-Type header to 'application/x-www-form-urlencoded' and using the URLSearchParams object:

fetch('/form-endpoint', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded'
  },
  body: new URLSearchParams({ username: 'user', password: 'pass' })
});
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>How do you create a middleware that logs the request method and URL for every request?</summary>

### How do you create a middleware that logs the request method and URL for every request?

**Definition:**

In Express.js, middlewares allow you to handle HTTP requests. Here, you will learn how to create a simple logging middleware that records the request method and URL.

**How it works:**

#### Setting Up the Express App

First, install Express via npm, and set up your app.js file:

#### Creating the Logging Middleware

Define a logging function that extracts the request method and URL, and then use app.use() to mount it as middleware.

#### Testing the Setup

Use app.get() to handle GET requests, and app.listen() to start the server.

When you visit http://localhost:3000/ in your browser and check the server console, you should see the request being logged.

**Examples:**

```javascript
const express = require('express');
const app = express();
```

```javascript
// Logging Middleware
const logRequest = (req, res, next) => {
  console.log(`Received ${req.method}  request for: ${req.url}`);
  next(); // Call next to proceed to the next middleware
};

// Mount the middleware for all routes
app.use(logRequest);
```

```javascript
// Sample route
app.get('/', (req, res) => {
  res.send('Hello World');
});

// Start the server
app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/expressjs-interview-questions/
</details>

<details>
<summary>Explain how you would implement nested routes in Express.js.</summary>

### Explain how you would implement nested routes in Express.js.

**Definition:**

Nested routes are an `express.Router` mounted on another router or on the app at a path prefix. The child router only sees the path that remains after the prefix.

**How it works:**

- `express.Router()` returns a router with the same `use`, `get`, `post`, and other methods as the app.
- `app.use('/users', usersRouter)` strips `/users` before the child router matches. A child route `'/:id'` handles `/users/123`.
- You can mount again: `usersRouter.use('/:userId/orders', ordersRouter)`.
- Router-level middleware runs only for requests that enter that router. The using-middleware guide mounts with `app.use('/', router)`.
- `req.originalUrl` stays the full path. `req.params` collects parameters from every mounted layer.

**Examples:**

```js
const express = require('express')
const app = express()
const users = express.Router()
const orders = express.Router({ mergeParams: true })

orders.get('/', (req, res) => {
  res.json({ userId: req.params.userId })
})

users.use('/:userId/orders', orders)
app.use('/users', users)
```

`mergeParams: true` is the Router option that copies parent params onto the child. It is documented on `express.Router`.

**Sources:**

- [Express — Routing](https://expressjs.com/en/guide/routing.html)
- [Express — Using middleware](https://expressjs.com/en/guide/using-middleware.html)

</details>

<details>
<summary>How can you capture and respond to URL parameters in a route?</summary>

### How can you capture and respond to URL parameters in a route?

**Definition:**

A named segment in the path, written `:name`, is captured on `req.params`. The handler reads that object and sends a response with `res.json` or `res.send`.

**How it works:**

- `app.get('/user/:id', ...)` sets `req.params.id` for `/user/42`.
- The using-middleware example checks `req.params.id === '0'` and calls `next('route')` to skip to the next matching route.
- Query string values are `req.query`, not `req.params`.
- Send JSON with `res.json`. Send text or HTML with `res.send`. Set a status with `res.status(code)` before the body.

**Examples:**

```js
app.get('/user/:id', (req, res) => {
  res.status(200).json({ id: req.params.id })
})
```

**Sources:**

- [Express — Using middleware](https://expressjs.com/en/guide/using-middleware.html)
- [Express — Routing](https://expressjs.com/en/guide/routing.html)

</details>

<details>
<summary>How do you serve different content types (e.g., JSON, HTML) with Express.js responses?</summary>

### How do you serve different content types (e.g., JSON, HTML) with Express.js responses?

**Definition:**

`res.json` sends JSON and sets `Content-Type` to JSON. `res.send` sends a string, object, or `Buffer`. `res.render` runs a template engine and sends HTML. `res.type` sets the `Content-Type` before you send the body.

**How it works:**

- `res.json(obj)` serializes the object. The error-handling guide uses `res.status(500).send({ error: 'Something failed!' })` for an XHR client and `res.render` for a page.
- `res.send('<p>Hello</p>')` sends a string. Call `res.type('html')` first when the string must be HTML rather than plain text.
- `res.format` picks a handler from the request `Accept` header. Use it when one route serves both JSON and HTML.
- A thrown error in an `async` handler reaches Express, which sets status from `err.status` or `err.statusCode` (or 500) and writes HTML in production.

**When to use:**

- `res.json` for an API client.
- `res.render` when a template engine is configured.
- `res.type` plus `res.send` for a raw body you already built.

**Examples:**

```js
app.get('/item', (req, res) => {
  res.format({
    html() {
      res.type('html').send('<p>Item</p>')
    },
    json() {
      res.json({ name: 'Item' })
    },
  })
})
```

**Sources:**

- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
- [Express — res.json](https://expressjs.com/en/api.html#res.json)

</details>

<details>
<summary>What are best practices for structuring a large Express.js application with multiple routes?</summary>

### What are best practices for structuring a large Express.js application with multiple routes?

**Definition:**

Split routes into one `express.Router` per area and mount each router from the application entry file. Keep error-handling middleware last.

**How it works:**

- The Express 4 migration example loads route modules (`./routes`, `./routes/user`) and mounts them with `app.get`. The same split works with `app.use('/users', userRouter)`.
- Order is behavior: logger and body parsers first, routes next, 404 handler, then error handlers with four arguments.
- Do not share request data on globals. The performance guide says clustered processes do not share memory. Put cross-request state in an external store.
- Set `NODE_ENV=production` so Express caches views and emits shorter errors.

**Examples:**

```js
const users = require('./routes/users')
const orders = require('./routes/orders')

app.use('/users', users)
app.use('/orders', orders)

app.use((req, res) => {
  res.status(404).send("Sorry can't find that!")
})
```

**Sources:**

- [Express — Routing](https://expressjs.com/en/guide/routing.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — Migrating to 4](https://expressjs.com/en/guide/migrating-4.html)

</details>

### Middleware and Error Handling

<details>
<summary>Explain the concept and use of built-in middleware in Express.js.</summary>

### Explain the concept and use of built-in middleware in Express.js.

**Definition:**

Built-in middleware ships with Express. The ones the API documents for parsing and files are `express.json()`, `express.urlencoded()`, and `express.static()`.

**How it works:**

- `express.json()` parses `Content-Type: application/json` and sets `req.body`. The default size limit is `"100kb"`. `strict` defaults to `true` (arrays and objects only).
- `express.urlencoded({ extended: true })` parses form bodies.
- `express.static(root, options)` serves files by joining `req.url` to `root`. A missing file calls `next()`. Defaults include `etag: true`, `index: "index.html"`, `maxAge: 0`, `dotfiles: "ignore"`.
- Mount them with `app.use` before routes that need `req.body` or a static file.

**Examples:**

```js
app.use(express.json())
app.use(express.urlencoded({ extended: true }))
app.use(express.static('public'))
```

**Sources:**

- [Express — express.json](https://expressjs.com/en/api.html#express.json)
- [Express — express.static](https://expressjs.com/en/api.html#express.static)
- [Express — req.body](https://expressjs.com/en/api.html#req.body)

</details>

<details>
<summary>How do you write custom middleware functions in Express.js?</summary>

### How do you write custom middleware functions in Express.js?

**Definition:**

Custom middleware is a function `(req, res, next)`. It reads or changes the request, ends the response, or calls `next()` to continue.

**How it works:**

- Load it with `app.use(fn)` or on a router. Without a path, it runs for every request that reaches that router.
- Call `next()` once when this layer is done. Skipping both `next()` and a response leaves the request hanging.
- `next(err)` skips remaining non-error middleware. `next('route')` skips the rest of this route and tries the next matching route.
- An `async` middleware that throws is passed to `next` because it returns a promise. A callback-style API must call `next(err)` yourself.
- Register error-handling middleware, `(err, req, res, next)`, after routes.

**Examples:**

```js
app.use((req, res, next) => {
  console.log(req.method, req.originalUrl)
  next()
})
```

**Sources:**

- [Express — Using middleware](https://expressjs.com/en/guide/using-middleware.html)
- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)

</details>

<details>
<summary>How do you handle file uploads in Express.js?</summary>

### How do you handle file uploads in Express.js?

**Definition:**

Express does not parse multipart bodies. The middleware catalog lists `multer` for `multipart/form-data`. `express.json()` only parses JSON.

**How it works:**

- Install `multer` and mount it on the route that accepts the upload, before the handler reads the file.
- The Express 4 migration sample calls `app.use(multer())` among the other body parsers, before routes.
- Files are not on `req.body` the way JSON is. The multer middleware puts file metadata where that package documents it. Keep a size limit so one request cannot fill the disk.
- A JSON API should stay on `express.json()` with its `limit` option (default `"100kb"`).

**When to use:**

- `multer` for a browser form with a file field.
- `express.json()` for a JSON payload with no file.

**Examples:**

```js
const multer = require('multer')
const upload = multer({ dest: 'uploads/' })

app.post('/profile', upload.single('avatar'), (req, res) => {
  res.json({ file: req.file })
})
```

**Sources:**

- [Express — Middleware](https://expressjs.com/en/resources/middleware.html)
- [Express — Migrating to 4](https://expressjs.com/en/guide/migrating-4.html)
- [Express — express.json](https://expressjs.com/en/api.html#express.json)

</details>

<details>
<summary>What is express-session, and how would you use it?</summary>

### What is express-session, and how would you use it?

**Definition:**

`express-session` stores session data on the server and sends only a session id in a cookie. It replaces the Express 3 `express.session` middleware.

**How it works:**

- The default store is memory and is not for production. The security guide says to use a compatible session store in production.
- Set `secret`, and set `name` to something other than the default cookie name so the cookie does not fingerprint Express.
- Behind TLS terminated at a proxy, `app.set('trust proxy', 1)` and `cookie.secure: true` so the cookie is marked Secure.
- `cookie-session` is the other option: it stores the whole session in the cookie. Use it only for small primitive values. The cookie is visible to the client. Keep it under 4093 bytes per domain.
- `resave` and `saveUninitialized` appear in the Express 4 migration sample. Set them deliberately. The migration sample uses `resave: true` and `saveUninitialized: true`.

**When to use:**

- `express-session` when the session must stay on the server.
- `cookie-session` when the payload is small and not secret.

**Examples:**

```js
const session = require('express-session')

app.set('trust proxy', 1)
app.use(session({
  secret: process.env.SESSION_SECRET,
  name: 'sessionId',
  resave: false,
  saveUninitialized: false,
  cookie: { secure: true, httpOnly: true },
}))
```

**Sources:**

- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
- [Express — express-session](https://expressjs.com/en/resources/middleware/session.html)

</details>

<details>
<summary>Discuss error handling in an Express.js application. How do you define an error-handling middleware?</summary>

### Discuss error handling in an Express.js application. How do you define an error-handling middleware?

**Definition:**

Error-handling middleware has four parameters: `(err, req, res, next)`. Express runs it when a route throws, when an `async` handler rejects, or when you call `next(err)`.

**How it works:**

- Synchronous `throw` is caught for you. An `async` route that rejects calls `next` with the error. A promise you do not return must `.catch(next)`. Callback APIs such as `fs.readFile` must call `next(err)` yourself.
- `next('route')` is not an error. Anything else passed to `next` skips non-error middleware.
- Define the error handler after routes. If `res.headersSent` is already true, call `next(err)` and let the default handler close the connection.
- The default handler uses `err.status` or `err.statusCode` when it is 4xx or 5xx, otherwise 500. In production the body is the status message, not `err.stack`. Set `NODE_ENV=production` for that.
- If your handler does not call `next` and does not send a response, the request hangs.

**Examples:**

```js
app.use((err, req, res, next) => {
  if (res.headersSent) return next(err)
  console.error(err.stack)
  res.status(500).send('Something broke!')
})
```

**Sources:**

- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)

</details>

<details>
<summary>Provide an example of using third-party middleware, such as body-parser or morgan.</summary>

### Provide an example of using third-party middleware, such as body-parser or morgan.

**Definition:**

Third-party middleware is an npm package you load with `app.use` or `router.use`. `morgan` logs requests. `body-parser` parses the body. Current Express also exposes `express.json()` and `express.urlencoded()`, which cover the common JSON and form cases.

**How it works:**

- `app.use(morgan('combined'))` writes Apache combined logs to stdout. Put the logger before routes so every request is logged.
- The router API example uses `router.use(logger())` then `express.static`.
- The Express 4 migration sample still shows `bodyParser.json()` and `bodyParser.urlencoded({ extended: true })`. Prefer the built-ins `express.json()` and `express.urlencoded({ extended: true })` unless you need a `body-parser` option the built-in does not wrap.
- The performance guide says `console.log` is synchronous when it writes to a terminal. For production traffic logs it points at a logging library such as Pino, and at `morgan` for request logs.

**Examples:**

```js
const morgan = require('morgan')
const app = express()

app.use(morgan('combined'))
app.use(express.json())

app.get('/', (req, res) => {
  res.send('hello, world!')
})
```

**Sources:**

- [Express — morgan](https://expressjs.com/en/resources/middleware/morgan.html)
- [Express — req.body](https://expressjs.com/en/api.html#req.body)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)

</details>

<details>
<summary>How do you protect against SQL injection or other security threats in Express.js?</summary>

### How do you protect against SQL injection or other security threats in Express.js?

**Definition:**

Express does not sanitize SQL. You stop injection by sending values as parameters to the database driver, and you reduce other attacks with Helmet, TLS, and strict redirects.

**How it works:**

- The security guide says to use parameterized queries or prepared statements. Never concatenate `req.query` or `req.body` into a SQL string.
- `helmet()` sets headers including `Content-Security-Policy`, `Strict-Transport-Security`, and `X-Content-Type-Options`. It removes `X-Powered-By`. You can also `app.disable('x-powered-by')`.
- Validate any URL before `res.redirect`. The guide rejects a host that is not the one you allow, and returns 400 for a URL that does not parse.
- Terminate TLS at Nginx (the guide’s recommendation) or another proxy. Let’s Encrypt is the certificate example they give.
- `npm audit` checks the dependency tree. Express 2 and 3 are unmaintained. Do not run them.
- Login routes should throttle failures. The guide points at `rate-limiter-flexible` for that.

**Examples:**

```js
app.get('/user', async (req, res) => {
  const { rows } = await pool.query('SELECT id, name FROM users WHERE id = $1', [req.query.id])
  res.json(rows[0] ?? null)
})
```

**Sources:**

- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)

</details>

### Response and Performance

<details>
<summary>How would you implement caching in an Express.js application?</summary>

### How would you implement caching in an Express.js application?

**Definition:**

Caching avoids repeating the same work for the same request. Express can set `Cache-Control` on a response. The performance guide also recommends a cache in front of the app, such as Nginx or Varnish.

**How it works:**

- `express.static` sets `Cache-Control` when `cacheControl` is true (the default). `maxAge` is the max-age in milliseconds (default `0`). `immutable: true` adds the immutable directive.
- `NODE_ENV=production` makes Express cache view templates and CSS generated from CSS extensions.
- For HTML or JSON you compute, set `Cache-Control` only when the response is safe to reuse. A personalized response should not be stored by a shared cache.
- The performance guide says a high-traffic site should compress and cache at the reverse proxy. `compression()` is the in-app gzip middleware. You do not need it if Nginx already gzips.

**When to use:**

- `maxAge` on static files that are fingerprinted.
- A reverse-proxy cache for public GET responses.
- Skip shared caching for responses that depend on the logged-in user.

**Examples:**

```js
app.use(express.static('public', { maxAge: '1d' }))

app.get('/catalog', (req, res) => {
  res.set('Cache-Control', 'public, max-age=60')
  res.json({ items: [] })
})
```

**Sources:**

- [Express — express.static](https://expressjs.com/en/api.html#express.static)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)

</details>

<details>
<summary>How do you set cookies and get cookies in an Express.js application?</summary>

### How do you set cookies and get cookies in an Express.js application?

**Definition:**

`res.cookie(name, value, options)` sets a `Set-Cookie` header. `cookie-parser` reads the `Cookie` header onto `req.cookies`. Signed cookies are on `req.signedCookies`.

**How it works:**

- Without `cookie-parser`, the raw header is still on `req.headers.cookie`. The middleware parses it.
- The security guide’s cookie options are `secure` (HTTPS only), `httpOnly` (not visible to page JavaScript), `domain`, `path`, and `expires`.
- Session cookies should not use the default name. Set `name` on `express-session` or `cookie-session`.
- Behind a proxy, `app.set('trust proxy', 1)` so `secure` cookies still work when TLS stops at the proxy.

**Examples:**

```js
const cookieParser = require('cookie-parser')
app.use(cookieParser())

app.get('/', (req, res) => {
  res.cookie('sessionId', 'abc', { httpOnly: true, secure: true })
  res.json({ cookies: req.cookies })
})
```

**Sources:**

- [Express — cookie-parser](https://expressjs.com/en/resources/middleware/cookie-parser.html)
- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
- [Express — res.cookie](https://expressjs.com/en/api.html#res.cookie)

</details>

<details>
<summary>What are ways to improve the performance of Express.js applications?</summary>

### What are ways to improve the performance of Express.js applications?

**Definition:**

The performance guide splits this into code and operations. In code, stay asynchronous and log without blocking. In production, set `NODE_ENV`, use a current Node.js LTS, and run more than one process behind a proxy.

**How it works:**

- Avoid synchronous filesystem and crypto calls on the request path. `--trace-sync-io` prints a stack when a sync API runs. Do not leave that flag on in production.
- `console.log` to a terminal is synchronous. Use `morgan` or a logger such as Pino, and debug with the `debug` package.
- `app.use(compression())` gzips responses. For a busy site, gzip at Nginx instead.
- `NODE_ENV=production` caches views and shortens error output. The guide reports a large speedup from that setting alone.
- Cluster one process per core with Node’s `cluster` module or PM2 (`pm2 start npm --name my-app -i max -- start`). Workers do not share memory, so sessions belong in a store such as Redis.
- Put Nginx or HAProxy in front for caching, static files, and load balancing.

**Sources:**

- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)

</details>

<details>
<summary>How do you configure an Express.js app for a reverse proxy, like Nginx?</summary>

### How do you configure an Express.js app for a reverse proxy, like Nginx?

**Definition:**

A reverse proxy accepts public traffic and forwards it to Express. The performance guide recommends Nginx or HAProxy in production for TLS, gzip, caching, and load balancing.

**How it works:**

- Express then sees the proxy’s address as the client unless you trust the proxy. `app.set('trust proxy', 1)` trusts the first proxy. The session examples use that so `cookie.secure` is honored.
- The proxy should set `X-Forwarded-For`, `X-Forwarded-Proto`, and `Host`. After `trust proxy` is set, `req.ip` and `req.protocol` use those headers.
- Terminate TLS on the proxy. The security guide points at Nginx and Let’s Encrypt.
- Do not run `compression()` as well if the proxy already gzips.
- Sticky sessions matter only if session data lives in one process. A shared store removes that need.

**Examples:**

```js
app.set('trust proxy', 1)
```

**Sources:**

- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — express-session](https://expressjs.com/en/resources/middleware/session.html)

</details>

<details>
<summary>Explain the purpose of template engines. How would you integrate one with Express.js?</summary>

### Explain the purpose of template engines. How would you integrate one with Express.js?

**Definition:**

A template engine turns a template plus data into HTML on the server. Express calls it through `res.render(view, locals)`.

**How it works:**

- `app.set('views', directory)` is the folder of templates. `app.set('view engine', 'pug')` selects the engine, as in the Express 4 migration sample.
- `res.render('regular')` looks up that view. The using-middleware guide renders `'regular'` and `'special'`.
- In production (`NODE_ENV=production`) Express caches compiled templates.
- The error handler can `res.status(500)` and `res.render('error', { error: err })`. Do not render `err.stack` to users in production. The default error handler already hides the stack when `NODE_ENV` is production.

**When to use:**

- Server-rendered HTML pages.
- A JSON API can skip a view engine and use `res.json`.

**Examples:**

```js
app.set('views', path.join(__dirname, 'views'))
app.set('view engine', 'pug')

app.get('/', (req, res) => {
  res.render('index', { title: 'Home' })
})
```

**Sources:**

- [Express — Using template engines](https://expressjs.com/en/guide/using-template-engines.html)
- [Express — Migrating to 4](https://expressjs.com/en/guide/migrating-4.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)

</details>

### Testing and Debugging

<details>
<summary>How do you test an Express.js application?</summary>

### How do you test an Express.js application?

**Definition:**

An Express app is a function `(req, res, next)`. Tests can call it through Node’s `http` server without a browser. Assert the status code and the body.

**How it works:**

- Export `app` and do not call `listen` inside the module you import. The test listens on port `0` so the OS picks a free port.
- `NODE_ENV` changes error bodies and view caching. Set it in the test process when you assert production behavior.
- Cover the error handler: a thrown error should return 500 and must not leak `err.stack` when `NODE_ENV` is `production`.
- The security guide’s `npm audit` is a dependency check, not a substitute for route tests.

**Examples:**

```js
const http = require('node:http')
const assert = require('node:assert/strict')
const app = require('./app')

const server = http.createServer(app)
server.listen(0, async () => {
  const { port } = server.address()
  const res = await fetch(`http://127.0.0.1:${port}/health`)
  assert.equal(res.status, 200)
  server.close()
})
```

**Sources:**

- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
- [Node.js — HTTP](https://nodejs.org/api/http.html)

</details>

<details>
<summary>Discuss common debugging techniques for Express.js applications.</summary>

### Discuss common debugging techniques for Express.js applications.

**Definition:**

Debug by logging the request, reading the error stack, and running Node with the inspector. The performance guide separates debug logs from production traffic logs.

**How it works:**

- In development, the default error handler sends `err.stack`. Set `NODE_ENV=production` only when you want that hidden.
- `morgan('dev')` or `morgan('combined')` logs method, URL, and status.
- The `debug` package prints when the `DEBUG` environment variable is set. The performance guide recommends it instead of `console.log` for debugging.
- `--trace-sync-io` warns when a synchronous API runs. Use it to find blocking calls, not as a production flag.
- `node --inspect server.js` exposes the V8 inspector. Attach Chrome DevTools or the editor debugger, then set a breakpoint in the route.

**Sources:**

- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Node.js — Debugger](https://nodejs.org/api/debugger.html)

</details>

<details>
<summary>What is the role of environment variables in an Express.js application?</summary>

### What is the role of environment variables in an Express.js application?

**Definition:**

Environment variables configure the process without hard-coding secrets and hosts. Express reads `NODE_ENV` and `app.get('env')` for production behavior. Your code reads `process.env`.

**How it works:**

- `NODE_ENV=production` caches views, caches CSS from extensions, and shortens error messages. `app.get('env')` returns that value (`development` when it is unset).
- The migration sample uses `process.env.PORT || 3000`.
- The session `secret` should come from the environment, not from source. The security examples show a literal only as a snippet.
- Checking `process.env` on every request has a cost. The performance guide says to read it sparingly.
- In production, set variables through the init system (systemd `Environment=`), not only in a developer shell profile.

**Examples:**

```js
const port = process.env.PORT || 3000
if (app.get('env') === 'production') {
  app.set('trust proxy', 1)
}
```

**Sources:**

- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — express-session](https://expressjs.com/en/resources/middleware/session.html)

</details>

<details>
<summary>How do you use a debugger with an Express.js app running in Node.js?</summary>

### How do you use a debugger with an Express.js app running in Node.js?

**Definition:**

Start the process with `node --inspect`. The inspector listens for Chrome DevTools or an IDE. Breakpoints in a route run when a request hits that function.

**How it works:**

- `node --inspect server.js` or `node --inspect-brk server.js` (the latter waits until a debugger attaches).
- The route is normal JavaScript. A breakpoint on `req.params` shows the request Node already parsed.
- `--trace-sync-io` is a separate flag from the performance guide. It logs synchronous I/O. It is not the interactive debugger.
- Do not leave the inspector open on a public interface in production. Bind debugging to a local development process.

**Examples:**

```bash
node --inspect server.js
```

**Sources:**

- [Node.js — Debugger](https://nodejs.org/api/debugger.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)

</details>

### Express.js with Databases

<details>
<summary>How would you connect a MongoDB database with an Express.js application?</summary>

### How would you connect a MongoDB database with an Express.js application?

**Definition:**

Express does not include a database client. You connect with the MongoDB Node.js driver (or an ODM) when the process starts, then use that client inside route handlers.

**How it works:**

- Read the URI from the environment. Connect once, not inside every request.
- `express.json()` must run before a handler reads `req.body`.
- Pass errors to `next(err)` or use an `async` handler so a failed query reaches the error middleware.
- Do not build queries by concatenating user input into a filter string. Pass a filter object.

**Examples:**

```js
const { MongoClient } = require('mongodb')
const client = new MongoClient(process.env.MONGODB_URI)

app.get('/notes', async (req, res) => {
  const notes = await client.db().collection('notes').find({}).toArray()
  res.json(notes)
})
```

**Sources:**

- [Express — express.json](https://expressjs.com/en/api.html#express.json)
- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
- [MongoDB Node.js driver — Connect](https://www.mongodb.com/docs/drivers/node/current/connect/connection/)

</details>

<details>
<summary>Explain how to integrate an ORM like Sequelize with Express.js.</summary>

### Explain how to integrate an ORM like Sequelize with Express.js.

**Definition:**

Sequelize is a separate package. Express only needs the models available in the route. Open one Sequelize instance for the process and close it when the process shuts down.

**How it works:**

- Configure the database from environment variables. Call `sequelize.authenticate()` at startup so a bad URI fails before you accept traffic.
- Routes call model methods (`Note.findAll()`) and `res.json` the result. An `async` handler forwards rejections to Express error middleware.
- Use Sequelize replacements or model APIs for filters. Do not pass raw SQL built from `req.query`.
- In a cluster, each worker has its own Sequelize instance. That matches the performance guide: processes do not share memory.

**Examples:**

```js
app.get('/notes', async (req, res) => {
  const notes = await Note.findAll()
  res.json(notes)
})
```

**Sources:**

- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
- [Sequelize — Getting started](https://sequelize.org/docs/v6/getting-started/)

</details>

<details>
<summary>How do you handle database errors in Express.js?</summary>

### How do you handle database errors in Express.js?

**Definition:**

A failed query is an error like any other. An `async` route that rejects calls `next(err)`. The error middleware chooses the HTTP status and the body.

**How it works:**

- Duplicate keys, timeouts, and refused connections should not become an uncaught exception. The performance guide says an uncaught exception crashes the process.
- Map known driver errors to 400 or 409 in the error handler. Leave unknown errors as 500.
- If headers were already sent, `return next(err)`.
- In production the default handler does not send `err.stack`. Log `err.stack` on the server, as the guide’s `logErrors` example does.
- Do not tell the client the SQL text or the connection string.

**Examples:**

```js
app.use((err, req, res, next) => {
  if (res.headersSent) return next(err)
  console.error(err.stack)
  res.status(500).json({ error: 'database request failed' })
})
```

**Sources:**

- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)

</details>

<details>
<summary>What are the advantages of using a database pooling mechanism in an Express.js app?</summary>

### What are the advantages of using a database pooling mechanism in an Express.js app?

**Definition:**

A pool keeps a set of open database connections and lends one to each query. The next request reuses a connection instead of opening a new TCP session.

**How it works:**

- Connecting on every request spends a handshake before any query. Under load that dominates latency.
- Node’s `pg` `Pool` and Sequelize’s pool do this inside one process. Create the pool at startup, next to `express()`.
- The performance guide’s cluster model still applies: each process has its own pool. Size the pool so all workers together do not exhaust the database’s connection limit.
- Release the client back to the pool when the query finishes. A handler that checks out a client and never releases it will stall later requests.

**When to use:**

- Any app that serves more than occasional queries.
- A single short-lived script can use one client and then close it.

**Sources:**

- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [node-postgres — Pooling](https://node-postgres.com/features/pooling)

</details>

### Authentication and Authorization

<details>
<summary>Describe how you would implement user authentication in Express.js.</summary>

### Describe how you would implement user authentication in Express.js.

**Definition:**

Authentication checks who the caller is. A typical Express setup is `express-session` plus a route that verifies a password and stores the user id on `req.session`.

**How it works:**

- Hash passwords before you store them. Compare the hash on login. The security guide says not to trust raw user input.
- Set the session cookie `httpOnly` and `secure`, and do not use the default session cookie name.
- Throttle failed logins. The guide suggests limiting consecutive failures per user and IP, and points at `rate-limiter-flexible`.
- A later route reads `req.session.userId`. If it is missing, respond with 401.
- Run the session middleware before routes that need `req.session`.

**Examples:**

```js
app.post('/login', async (req, res) => {
  const user = await findUser(req.body.email)
  if (!user || !verifyPassword(req.body.password, user.passwordHash)) {
    return res.status(401).json({ error: 'invalid credentials' })
  }
  req.session.userId = user.id
  res.json({ ok: true })
})
```

**Sources:**

- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
- [Express — express-session](https://expressjs.com/en/resources/middleware/session.html)

</details>

<details>
<summary>Explain how sessions are managed in Express.js.</summary>

### Explain how sessions are managed in Express.js.

**Definition:**

`express-session` saves the session on the server and puts the session id in a cookie. `cookie-session` saves the session in the cookie itself.

**How it works:**

- MemoryStore is the default and is not for production. Use a store shared by every process. The performance guide names Redis for session data when you run a cluster, because workers do not share memory.
- `trust proxy` must be set when the cookie is `secure` and TLS ends at Nginx.
- `cookie-session` serializes the session into the cookie. The client can read it. Stay under 4093 bytes per domain. Do not put secrets in that cookie.
- Changing the cookie `name` avoids the default name, which the security guide treats as a fingerprint.

**When to use:**

- Server store for login sessions.
- Cookie session only for small, non-sensitive values.

**Sources:**

- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)

</details>

<details>
<summary>Discuss how you would handle user roles and permissions in an Express.js application.</summary>

### Discuss how you would handle user roles and permissions in an Express.js application.

**Definition:**

A role check is middleware that runs after authentication. It reads the user already stored on the session and calls `next()` or responds with 403.

**How it works:**

- Load the user once. Later middleware should not trust a role sent in the body or query.
- `next('route')` skips the rest of the current route stack. The error-handling guide uses it when a user has not paid, so a later handler for the same path can run instead.
- Keep the check beside the router it protects: `router.use(requireRole('admin'))`.
- Fail closed. A missing user is 401. A known user without the role is 403.

**Examples:**

```js
function requireUser(req, res, next) {
  if (!req.session.userId) return res.status(401).json({ error: 'login required' })
  next()
}

function requireRole(role) {
  return (req, res, next) => {
    if (req.session.role !== role) return res.status(403).json({ error: 'forbidden' })
    next()
  }
}
```

**Sources:**

- [Express — Using middleware](https://expressjs.com/en/guide/using-middleware.html)
- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)

</details>

<details>
<summary>How can you secure passwords and sensitive information in your Express.js app?</summary>

### How can you secure passwords and sensitive information in your Express.js app?

**Definition:**

Store passwords only as hashes. Keep secrets in the environment. Send them only over TLS, and do not put them in cookies the browser script can read.

**How it works:**

- The security guide requires TLS for sensitive data and recommends terminating it at Nginx.
- Session cookies: `httpOnly: true`, `secure: true`, and a non-default `name`.
- `cookie-session` puts data where the client can see it. Use `express-session` when the payload must stay on the server.
- Do not log `req.body` on a login route. `morgan('combined')` logs the request line, not a JSON password, as long as the password is in the body.
- `npm audit` and current Express releases are part of the guide. Do not commit `.env` files.

**Sources:**

- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)

</details>

### Integration and Configuration

<details>
<summary>How do you integrate a third-party API in an Express.js application?</summary>

### How do you integrate a third-party API in an Express.js application?

**Definition:**

Call the other service from the route handler on the server. The browser talks only to your Express app, so the third-party credential stays in the environment.

**How it works:**

- Use `fetch` (global in current Node) or another HTTP client. Set a timeout so a hung upstream does not hold the request forever. Node’s `fetch` accepts an `AbortSignal`.
- Map upstream failure to a status you choose. Do not pipe the upstream body through if it may contain secrets.
- An `async` handler’s rejection reaches error middleware.
- Cache a public, stable response with `Cache-Control` or a reverse proxy. Do not cache a response that used the caller’s credentials.

**Examples:**

```js
app.get('/weather', async (req, res) => {
  const upstream = await fetch('https://api.example.com/weather', {
    headers: { Authorization: `Bearer ${process.env.WEATHER_TOKEN}` },
  })
  if (!upstream.ok) return res.status(502).json({ error: 'upstream failed' })
  res.json(await upstream.json())
})
```

**Sources:**

- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
- [MDN — fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

</details>

<details>
<summary>Explain the steps to deploy an Express.js application to a cloud provider like AWS or Heroku.</summary>

### Explain the steps to deploy an Express.js application to a cloud provider like AWS or Heroku.

**Definition:**

Deploy the Node process, set `NODE_ENV=production`, and put a reverse proxy or the platform’s router in front of it. The performance guide is the Express checklist.

**How it works:**

- Listen on `process.env.PORT`. The migration sample uses `process.env.PORT || 3000`.
- Set `NODE_ENV=production` in the platform’s environment, not only in a shell profile. That caches views and hides stacks.
- Use a process manager or the platform’s restart policy so a crash comes back. The guide shows systemd and a process manager.
- Run a current Node.js LTS. Terminate TLS at the proxy. `app.set('trust proxy', 1)` when the platform proxies.
- Sessions need a shared store if more than one process runs. Run `npm audit` before you ship.

**Sources:**

- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)

</details>

<details>
<summary>How can you ensure that your Express.js application is scalable?</summary>

### How can you ensure that your Express.js application is scalable?

**Definition:**

One Node process uses one core. Scale by running more stateless processes and balancing requests across them.

**How it works:**

- Node’s `cluster` module or PM2 (`-i max`) starts one worker per core. Workers do not share memory.
- Keep session and other shared state in Redis or the database. The performance guide says in-process state breaks as soon as you add a second process.
- A load balancer (Nginx or HAProxy) spreads traffic. Sticky sessions are a workaround. A shared store is the guide’s preferred fix.
- Move gzip, static files, and caching to the proxy so Express handles application work.
- Avoid synchronous calls on the request path. They stall every connection on that process.

**Sources:**

- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)

</details>

<details>
<summary>What is the Twelve-Factor App methodology, and how does it apply to Express.js?</summary>

### What is the Twelve-Factor App methodology, and how does it apply to Express.js?

**Definition:**

The twelve-factor methodology is a set of rules for apps that deploy on a platform: config in the environment, stateless processes, logs as event streams, and a clean build. Express does not implement it. You apply it in how you run the process.

**How it works:**

- Config: `process.env` and `NODE_ENV`. The performance guide uses both.
- Processes: no in-memory sessions if you run more than one copy. The cluster section says memory is local to each process.
- Logs: write request logs to stdout (`morgan('combined')`) and let the platform collect them.
- Port binding: `app.listen(process.env.PORT)`.
- Admin tasks (migrations) run as a one-off command, not inside a request handler.
- Dev/prod parity: the security guide’s point is that production hides stacks and requires TLS. Do not run the development error handler in production.

**Sources:**

- [The Twelve-Factor App](https://12factor.net/)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — morgan](https://expressjs.com/en/resources/middleware/morgan.html)

</details>

### Coding Challenges

<details>
<summary>Write an Express.js middleware function that limits requests to 100 per hour per IP address.</summary>

### Write an Express.js middleware function that limits requests to 100 per hour per IP address.

**Definition:**

Express has no built-in rate limiter. A middleware function can count requests per IP in memory and answer 429 after 100 in an hour. The security guide points at `rate-limiter-flexible` for login throttling. This snippet is the same idea with a `Map`.

**How it works:**

- Key the map by `req.ip`. Set `trust proxy` when a proxy sits in front, or every client shares the proxy’s address.
- Store a window start and a count. When the window is older than an hour, reset it.
- Call `next()` under the limit. Otherwise `res.status(429).send(...)` and do not call `next()`.
- A single process’s `Map` does not limit a cluster. Each worker has its own map. A shared store is required if you run more than one process. The performance guide makes the same point about sessions.

**Examples:**

```js
const hits = new Map()
const HOUR = 60 * 60 * 1000

function limit100PerHour(req, res, next) {
  const now = Date.now()
  const entry = hits.get(req.ip)
  if (!entry || now - entry.start >= HOUR) {
    hits.set(req.ip, { start: now, count: 1 })
    return next()
  }
  entry.count += 1
  if (entry.count > 100) return res.status(429).send('Too many requests')
  next()
}

app.use(limit100PerHour)
```

**Sources:**

- [Express — Using middleware](https://expressjs.com/en/guide/using-middleware.html)
- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)

</details>

<details>
<summary>Create an Express.js route that accepts a JSON payload and responds with the same payload in reverse order.</summary>

### Create an Express.js route that accepts a JSON payload and responds with the same payload in reverse order.

**Definition:**

`express.json()` parses the body onto `req.body`. The handler copies an array, reverses the copy, and sends it back with `res.json`.

**How it works:**

- The default JSON limit is `"100kb"`. Raise `limit` only if you mean to.
- Reject a body that is not an array with 400.
- `Array.prototype.reverse` mutates. Copy first so you do not surprise another middleware still holding `req.body`.
- Reversing object key order is not meaningful in JSON. This route reverses an array.

**Examples:**

```js
app.use(express.json())

app.post('/reverse', (req, res) => {
  if (!Array.isArray(req.body)) {
    return res.status(400).json({ error: 'expected a JSON array' })
  }
  res.json([...req.body].reverse())
})
```

**Sources:**

- [Express — express.json](https://expressjs.com/en/api.html#express.json)

</details>

<details>
<summary>Develop a simple REST API with Express.js that includes CRUD operations for managing books. Include route definitions and handler functions that interact with a placeholder data store.</summary>

### Develop a simple REST API with Express.js that includes CRUD operations for managing books. Include route definitions and handler functions that interact with a placeholder data store.

**Definition:**

Map the book resource to `/books`. `GET` reads, `POST` creates, `PUT` replaces, `DELETE` removes. A `Map` stands in for the database.

**How it works:**

- `express.json()` runs first so `POST` and `PUT` can read `req.body`.
- `POST` returns 201. A missing id returns 404. `DELETE` of an existing book returns 204 with `res.sendStatus(204)` or `res.status(204).end()`.
- This `Map` is process memory. The performance guide says that state is not shared across cluster workers and disappears when the process exits. Replace it with a database for anything but a sample.

**Examples:**

```js
const books = new Map()
let nextId = 1

app.get('/books', (req, res) => {
  res.json([...books.values()])
})

app.get('/books/:id', (req, res) => {
  const book = books.get(req.params.id)
  if (!book) return res.status(404).json({ error: 'not found' })
  res.json(book)
})

app.post('/books', (req, res) => {
  const id = String(nextId++)
  const book = { id, title: req.body.title }
  books.set(id, book)
  res.status(201).json(book)
})

app.put('/books/:id', (req, res) => {
  if (!books.has(req.params.id)) return res.status(404).json({ error: 'not found' })
  const book = { id: req.params.id, title: req.body.title }
  books.set(book.id, book)
  res.json(book)
})

app.delete('/books/:id', (req, res) => {
  if (!books.delete(req.params.id)) return res.status(404).json({ error: 'not found' })
  res.status(204).end()
})
```

**Sources:**

- [Express — Routing](https://expressjs.com/en/guide/routing.html)
- [Express — express.json](https://expressjs.com/en/api.html#express.json)

</details>

### Advanced Topics and Best Practices

<details>
<summary>Discuss strategies for building a real-time application with Express.js.</summary>

### Discuss strategies for building a real-time application with Express.js.

**Definition:**

Express handles HTTP request and response. Real-time updates need a long-lived channel on the same Node server, or a separate service. The migration guide builds that server with `http.createServer(app)`.

**How it works:**

- Create the HTTP server from the Express app, then attach a WebSocket server to it. Express routes stay request/response. The socket path is not an `app.get`.
- The performance guide lists WebSocket connections as process-local state. A cluster does not share them. You need sticky routing or a pub/sub bus so one worker can reach a client connected to another.
- Polling with a normal `GET` is the fallback when you cannot hold sockets. Cache that response only if it is not per-user.
- Keep the Express process asynchronous. A tight loop in a socket handler blocks HTTP too.

**When to use:**

- WebSockets when the server must push.
- Short-interval `GET` when infrastructure cannot hold upgraded connections.

**Examples:**

```js
const http = require('node:http')
const app = require('./app')

const server = http.createServer(app)
server.listen(process.env.PORT || 3000)
```

**Sources:**

- [Express — Migrating to 4](https://expressjs.com/en/guide/migrating-4.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)

</details>

<details>
<summary>What are some common optimization techniques for Express.js applications?</summary>

### What are some common optimization techniques for Express.js applications?

**Definition:**

The same production guide applies: less work per request, no blocking calls, and a proxy that takes static files, gzip, and caching.

**How it works:**

- `express.static` with a `maxAge` for fingerprinted assets.
- `compression()` or gzip at Nginx, not both.
- `NODE_ENV=production`.
- Cluster or PM2 across cores, with shared session storage.
- Log with `morgan` or Pino instead of `console.log` to a terminal.
- Cache public GET results at Varnish or Nginx, as the “Cache request results” section describes.

**Sources:**

- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — express.static](https://expressjs.com/en/api.html#express.static)

</details>

<details>
<summary>Explain the concept and benefits of server-side rendering with Express.js and a templating engine.</summary>

### Explain the concept and benefits of server-side rendering with Express.js and a templating engine.

**Definition:**

Server-side rendering here means Express builds HTML with `res.render` before the response is sent. The browser receives a document, not an empty shell waiting on client JavaScript.

**How it works:**

- Set `view engine` and `views`. Pass data as the second argument to `res.render`.
- The HTML can be indexed and shown when JavaScript does not run. The first byte includes the content.
- `NODE_ENV=production` caches compiled templates, which the performance guide calls out.
- Do not put secrets in the template output. It is the response body.
- A JSON API does not need this. Use `res.json` there.

**Examples:**

```js
app.set('view engine', 'pug')
app.get('/books/:id', async (req, res) => {
  const book = await findBook(req.params.id)
  if (!book) return res.status(404).send('Not found')
  res.render('book', { book })
})
```

**Sources:**

- [Express — Using template engines](https://expressjs.com/en/guide/using-template-engines.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)

</details>

<details>
<summary>How do you ensure that your Express.js code is maintainable and follows best practices?</summary>

### How do you ensure that your Express.js code is maintainable and follows best practices?

**Definition:**

Keep one router per area, middleware in named functions, and error handling in one four-argument function at the end. Configure the process from the environment.

**How it works:**

- Mount routers instead of growing a single file. The routing guide is the pattern.
- Body parsers and loggers go before routes. The 404 handler and the error handler go after them. The security guide shows that order for a custom 404 and 500.
- `async` handlers so rejections reach Express. Callback code calls `next(err)`.
- Run on a maintained Express release. The security guide says 2.x and 3.x are unmaintained.
- `NODE_ENV=production` in production. `npm audit` for dependencies.

**Sources:**

- [Express — Routing](https://expressjs.com/en/guide/routing.html)
- [Express — Error handling](https://expressjs.com/en/guide/error-handling.html)
- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)

</details>

### Case Studies and Scenario-Based Questions

<details>
<summary>How would you structure an Express.js application for a large-scale e-commerce platform?</summary>

### How would you structure an Express.js application for a large-scale e-commerce platform?

**Definition:**

Split catalog, cart, and checkout into routers. Keep the process stateless so you can run a cluster. Put inventory and orders in a database, not in a `Map`.

**How it works:**

- `app.use('/catalog', catalogRouter)`, and the same for cart and checkout.
- Sessions in a shared store. The performance guide says cluster workers do not share memory. A cart in one process is invisible to the others.
- `express.json()` with a `limit`. Static images through `express.static` or, better, the reverse proxy.
- Checkout writes must use parameterized queries. The security guide is explicit about SQL injection.
- `trust proxy` when the load balancer terminates TLS.

**Sources:**

- [Express — Routing](https://expressjs.com/en/guide/routing.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)

</details>

<details>
<summary>Describe the key considerations for building a secure API with Express.js.</summary>

### Describe the key considerations for building a secure API with Express.js.

**Definition:**

Validate input, parameterize database queries, set security headers, and do not leak stack traces or the `X-Powered-By` header.

**How it works:**

- `helmet()` and `app.disable('x-powered-by')`.
- `express.json({ limit: '100kb' })` so a huge body is rejected. 100kb is the documented default.
- Parameterized SQL. Reject unexpected types with 400 before you touch the database.
- Authenticate, then authorize. 401 versus 403.
- TLS at the proxy. `trust proxy` only for proxies you control. A client can spoof `X-Forwarded-For` if you trust everyone.
- `NODE_ENV=production` so the default error handler omits `err.stack`.

**Sources:**

- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
- [Express — express.json](https://expressjs.com/en/api.html#express.json)

</details>

<details>
<summary>Discuss how to handle session management in a distributed Express.js application.</summary>

### Discuss how to handle session management in a distributed Express.js application.

**Definition:**

More than one process means the session cannot live in MemoryStore. `express-session` must use a store every process can read. The performance guide names Redis.

**How it works:**

- Sticky sessions pin a user to one process. That fails when the process dies. A shared store survives it.
- Set `cookie.secure` and `app.set('trust proxy', 1)` because TLS usually ends at the load balancer.
- Set a non-default cookie `name`.
- `cookie-session` needs no server store, but the whole session is in the cookie and visible to the client. The security guide limits that to small non-sensitive values.
- Do not enable `trust proxy` so loosely that any client can forge the forwarded headers.

**Sources:**

- [Express — Security best practices](https://expressjs.com/en/advanced/best-practice-security.html)
- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — express-session](https://expressjs.com/en/resources/middleware/session.html)

</details>

<details>
<summary>What are the implications of microservices architecture for an Express.js application?</summary>

### What are the implications of microservices architecture for an Express.js application?

**Definition:**

Each service is its own Express process with its own routes and database. They talk over the network. Express does not provide service discovery or a message bus.

**How it works:**

- One process should stay stateless, as the cluster section requires, because you will run more than one copy of each service.
- Config comes from the environment. Logs go to stdout (`morgan`). That is the twelve-factor shape the platform expects.
- A call to another service uses `fetch` and must handle failure with a timeout and a status such as 502. Do not let one slow service hold the event loop with synchronous retries.
- `trust proxy` applies when a mesh or load balancer sits in front.
- Split only along a real boundary (orders versus catalog). A single router file is simpler until that boundary exists.

**Sources:**

- [Express — Production performance](https://expressjs.com/en/advanced/best-practice-performance.html)
- [Express — Routing](https://expressjs.com/en/guide/routing.html)
- [The Twelve-Factor App](https://12factor.net/)

</details>
