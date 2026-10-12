# Node.js — Interview

Answers are filled for the full question list.

[← Back to hub](./8_nodejs.md)

### Related docs

- [Node.js — Core](./8_nodejs_core.md) · [Node.js — Modules](./8_nodejs_modules.md) · [Node.js — HTTP](./8_nodejs_http.md) · [Node.js — Additional](./8_nodejs_process.md) · [Node.js — Interview](./8_nodejs_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Node.js](https://devinterview.io/questions/web-and-mobile-development/node-interview-questions/).

### Node.js Basics

<details>
<summary>What is Node.js and why is it used?</summary>

### What is Node.js and why is it used?

**Definition:**

Node.js is an open-source, cross-platform JavaScript runtime. It executes JavaScript outside the browser on V8, the same engine Chrome uses, and libuv supplies the cross-platform I/O layer. The design is event-driven and non-blocking, so one process can keep many connections in flight while it waits on the network.

**How it works:**

- JavaScript on the main thread has one call stack. I/O is started and finished later through the event loop, so a waiting socket does not freeze the process.
- libuv runs file I/O, some DNS lookups, and `uv_queue_work` on a small thread pool (four threads by default, `UV_THREADPOOL_SIZE`). Network I/O stays on the loop thread.
- npm is the package manager and the public registry that ships with Node. Yarn and pnpm are other clients of that registry.
- The same language can run in the browser and on the server, so teams share types, validation, and small utilities. That does not make the browser APIs available on the server.
- A chat or a live feed is a good fit because sockets stay open and the loop keeps accepting other work. Node does not ship a WebSocket server in core; you use the HTTP server plus a library, or the global `WebSocket` client where the version provides it.
- Long CPU loops still block the main thread. Compression, image work, and other heavy computation belong in `worker_threads`, a child process, or another service.

**When to use:**

- HTTP and JSON APIs, proxies, and backends behind a single-page app.
- Streaming files, logs, or media in chunks instead of buffering the whole payload.
- Realtime products (chat, notifications, collaborative cursors) and small services that scale by adding processes.
- Scripts and device-side tools where a small runtime is enough, including boards that can run Node.

**Pluses and minuses:**

- One language on both sides shortens the stack and lets libraries move between client and server when they do not touch the DOM or `fs`.
- The registry and a small framework such as Express make a first API fast to stand up.
- The `cluster` module and a process manager scale by running one process per core. That is process parallelism, not threads inside one isolate.
- Open source and a large package set cut time, and they also mean you must pin versions and watch supply-chain risk.
- It is a weak default for tight numeric loops on the main thread.

**Examples:**

```js
import http from "node:http";

http
  .createServer((_req, res) => {
    res.writeHead(200, { "Content-Type": "text/plain" });
    res.end("ok");
  })
  .listen(3000);
```

**Sources:**

- [Node.js — About](https://nodejs.org/en/about)
- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
</details>

<details>
<summary>How does Node.js handle child threads?</summary>

### How does Node.js handle child threads?

**Definition:**

The JavaScript you write runs on one main thread with one call stack. Node still uses other threads: libuv’s pool for some blocking system calls, and `worker_threads` when you want a second V8 isolate for CPU work. Those are not the same mechanism.

**How it works:**

- The event loop stays on the main thread and keeps accepting I/O. When a filesystem call, `dns.lookup`, or some crypto work would block, libuv queues it on the thread pool (`UV_THREADPOOL_SIZE`, default 4). Network sockets are not handed to that pool.
- When the pool finishes, libuv posts the result back and the loop runs your callback. The main thread did not sit inside the system call.
- `worker_threads` starts another isolate with its own event loop and its own memory. You talk with `postMessage` / `parentPort`. Ordinary objects are cloned or transferred; they are not shared. `SharedArrayBuffer` is the explicit shared-memory path.
- `isMainThread` tells you which side of that split you are on. A worker file can be the same module: the parent branch constructs `new Worker`, the child branch listens on `parentPort`.
- `child_process` is a third tool: a whole OS process, used for a shell command or a hard isolation boundary. `cluster` forks processes to use more cores. Neither is a thread inside the parent isolate.
- Offloading a tight loop keeps the main loop free to accept sockets. A worker per request usually costs more than it saves, because isolates are heavy.

**When to use:**

- Thread pool: you already get it from async `fs`, `dns.lookup`, and some `crypto` APIs. You rarely manage those threads yourself.
- Worker threads: image processing, compression, parsing a large payload, or any CPU loop that would stall the server.
- Child process: run a CLI, isolate untrusted code, or crash without taking down the parent.

**Pluses and minuses:**

- The main loop stays responsive while pool work and workers run.
- You get parallelism without writing locks around every JavaScript object.
- Message passing copies data. Large payloads need transfer lists or shared memory, or the copy dominates the win.
- The pool is small and shared. A burst of `fs` and `pbkdf2` calls queue behind each other.

**Examples:**

```js
import { Worker, isMainThread, parentPort } from "node:worker_threads";

if (isMainThread) {
  const worker = new Worker(new URL(import.meta.url));
  worker.on("message", (message) => console.log("Received:", message));
  worker.postMessage("Hello from the main thread");
} else {
  parentPort.on("message", (message) => {
    console.log("Received in the worker:", message);
    parentPort.postMessage("Hello from the worker thread");
  });
}
```

**Sources:**

- [Node.js — Worker threads](https://nodejs.org/api/worker_threads.html)
- [Node.js — Don't block the event loop](https://nodejs.org/en/learn/asynchronous-work/dont-block-the-event-loop)
</details>

<details>
<summary>Describe the event-driven programming in Node.js.</summary>

### Describe the event-driven programming in Node.js.

**Definition:**

Event-driven code reacts when something happens — a request, a file chunk, a timer, a closed socket — instead of sitting in a loop until that thing finishes. In Node the usual shape is an emitter, a listener, and the event loop that gets back to that listener after other work.

**How it works:**

- `EventEmitter` is the registry. `on(name, listener)` stores a callback. `emit(name, ...args)` calls the listeners for that name.
- `emit` runs those listeners on the current stack, in registration order. It does not schedule them for later and it does not start a thread. If the listener itself starts I/O, that I/O finishes later on the loop.
- The call stack runs the JavaScript that is in progress. When it empties, the loop takes the next timer, I/O callback, `setImmediate`, or close callback.
- Many core objects are emitters: an HTTP server (`request`), a readable stream (`data`, `end`, `error`), a socket (`close`). You subscribe instead of blocking until the result exists.
- A missing `error` listener on an emitter can crash the process. Subscribe to `error`, or use APIs that return promises and reject.
- Promises and `async`/`await` sit on the same loop. A resolved promise queues a microtask. It is not a second thread.

**When to use:**

- An HTTP server waiting for connections.
- File and network streams that deliver chunks as they arrive.
- Anything in your own code with more than one listener: a job bus, a process signal, a custom `EventEmitter`.

**Pluses and minuses:**

- Many connections stay cheap because each one is a subscription, not a blocked thread.
- Control flow is harder to read when every step is a callback. Promises and `async`/`await` keep the same loop and flatten that shape.
- A listener that throws, or that never handles `error`, takes down more than the one event.

**Examples:**

```js
import { EventEmitter } from "node:events";

const emitter = new EventEmitter();

emitter.on("event-name", (eventArgs) => {
  console.log(`Event-name was emitted with arguments: ${eventArgs}`);
});

emitter.emit("event-name", "Some Payload");
```

**Sources:**

- [Node.js — Events](https://nodejs.org/api/events.html)
- [Node.js — The event loop](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick)
</details>

<details>
<summary>What is the event loop in Node.js?</summary>

### What is the event loop in Node.js?

**Definition:**

The event loop is the libuv cycle that runs timers, I/O callbacks, and close callbacks once the current JavaScript stack goes idle. It is why one thread can interleave many operations. Node initializes it at startup and keeps turning it until nothing is left to do and no handle is still open.

**How it works:**

- Each turn (Node.js 20+ / libuv 1.45) walks: pending callbacks, idle/prepare (internal), poll, check (`setImmediate`), close callbacks, then timers. Due timers can also run once before the first iteration. Full picture: [Event loop](./8_nodejs_core.md#event-loop).
- Poll is the phase that blocks when nothing else is ready, up to the next timer. It is not a single “callback queue” that every API shares.
- `process.nextTick` is not a phase. Its queue is drained after the current operation, before the loop continues. A nextTick callback that schedules another nextTick can starve I/O.
- Promise reactions and `queueMicrotask` use the microtask queue. Node drains it immediately after the nextTick queue. In CommonJS, nextTick runs first. During ESM evaluation the module body is already a microtask, so `queueMicrotask` runs before `nextTick`.
- `setTimeout(fn, 0)` is a timer. `setImmediate(fn)` is the check phase. Their order against each other is not fixed when both are scheduled from the main module, because the timer may or may not be due before the first poll.
- A long synchronous loop blocks every phase, including new connections. The loop is per thread: a worker has its own loop and does not share the parent’s queues.
- Browser words such as “macrotask” do not name Node’s phases. Timers, poll callbacks, and `setImmediate` are separate libuv queues.

**When to use:**

- Explain why `console.log` after `readFile` prints first.
- Explain why a 2-second `for` loop freezes every request on that process.
- Choose `nextTick` only when you must run before I/O continues. Prefer a promise or `queueMicrotask` for ordinary deferral, and `setImmediate` to yield back to the loop.

**Examples:**

```js
console.log("Start");

setTimeout(() => {
  console.log("timer");
  Promise.resolve().then(() => console.log("promise inside timer"));
}, 0);

setImmediate(() => console.log("setImmediate"));

process.nextTick(() => {
  console.log("nextTick");
  process.nextTick(() => console.log("nextTick nested"));
});

Promise.resolve().then(() => console.log("promise"));

console.log("End");
```

**Sources:**

- [Node.js — The event loop](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick)
- [Node.js — process.nextTick](https://nodejs.org/api/process.html#processnexttickcallback-args)
</details>

<details>
<summary>What is the difference between Node.js and traditional web server technologies?</summary>

### What is the difference between Node.js and traditional web server technologies?

**Definition:**

A thread-per-request server spends an OS thread on each connection, including while that thread waits on I/O. Node keeps one JavaScript thread per process and continues other work while I/O is outstanding. “Traditional” here means that thread-per-request model (classic Apache, many Java and .NET servers), not every server written before Node. nginx is already event-driven.

**How it works:**

- Thread-per-request: each client can get a thread. The thread still occupies memory when it is blocked on a database or a file. The ceiling is roughly “threads the machine will schedule”.
- Node: one call stack. Async I/O registers a callback and returns. The loop runs someone else’s request while the first wait continues. Idle connections are cheap until the work is CPU-bound.
- Blocking I/O holds the caller until the disk or the socket returns. Non-blocking I/O returns immediately. Callbacks, promises, and `async`/`await` are how you collect the result later.
- Java, C#, or PHP servers often pair a different language on the server with JavaScript in the browser. Node uses JavaScript on both sides, so validation and small utilities can be shared. Browser DOM APIs still do not exist on the server.
- You edit a script and run it again. V8 compiles hot code while the process runs. You do not produce a separate application binary the way `javac` does. Native addons are the exception, and they do need a rebuild.
- Dependencies come from npm (or Yarn or pnpm) and `package.json`. Other stacks use their own repositories, such as Maven or NuGet.
- A Node deploy is a process and a port, often behind a reverse proxy, with a process manager and TLS. It does not remove operations work. The `cluster` module runs one process per core when you outgrow a single loop.
- The model fits many concurrent connections and realtime fan-out. Heavy computation, batch jobs, and some enterprise stacks still fit a multi-threaded runtime better, or a worker beside Node.

**When to use:**

- Node: APIs, chat, streaming, proxies, and services that wait on I/O more than they compute.
- A threaded server: CPU-heavy request work you do not want to isolate yourself, or an existing platform standard (JVM, .NET) the team already operates.

**Sources:**

- [Node.js — About](https://nodejs.org/en/about)
- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
</details>

<details>
<summary>Explain what "non-blocking" means in Node.js.</summary>

### Explain what "non-blocking" means in Node.js.

**Definition:**

Non-blocking means a function starts work that may take a while and returns before that work finishes. The process keeps running other JavaScript. The result is delivered later on the event loop, through a callback, a promise, or `async`/`await`.

**How it works:**

- With blocking I/O the caller sits inside the call until the resource is ready. Nothing else on that thread runs.
- With non-blocking I/O the call asks the system to proceed and returns. If a file read is in progress, the server can still accept another request.
- `fs.readFile` hands the read to libuv’s thread pool and invokes the callback when the bytes are ready. `fs.readFileSync` blocks. The loop cannot accept new work until it returns.
- Network reads are non-blocking on the loop thread itself. The kernel reports readiness, then JavaScript runs the handler. They do not occupy a pool thread while waiting.
- This is concurrency on one JavaScript thread, not a thread per request. Callbacks still run one at a time. Parallel CPU work needs workers or processes.
- A function can look async and still block if the callback does a long synchronous computation. Non-blocking I/O does not make the JavaScript inside the callback non-blocking.
- Streams are the same idea for large payloads: you handle a chunk and return, instead of waiting for the whole file.

**When to use:**

- Request handlers, file serving, database clients, and anything else on a server process.
- Sync methods are reasonable in a short CLI script that does one thing and exits.

**Examples:**

```js
import { readFile } from "node:fs";

readFile("notes.txt", "utf8", (err, text) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(text);
});

console.log("This message is displayed immediately.");
```

**Sources:**

- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
- [Node.js — File system](https://nodejs.org/api/fs.html)
</details>

<details>
<summary>How do you update Node.js to the latest version?</summary>

### How do you update Node.js to the latest version?

**Definition:**

You replace the `node` binary. A version manager or the installer on nodejs.org does that. `npm install` updates packages in a project. It does not upgrade the runtime, and installing Node through npm is not the supported upgrade path.

**How it works:**

- Version managers install releases side by side and point `PATH` at one of them. `nvm` and `fnm` are the usual Unix tools. `nvs` works across platforms. The `n` package can switch versions too, but you already need a working Node to install it, so it is a poor first install.
- The official installer and OS packages replace the system binary. On macOS, Homebrew’s `node` formula tracks a release. On Windows, the installer from nodejs.org, winget, or a version manager is the straightforward path.
- On a server, pin a current LTS. “Latest” includes Current releases that are not the long-term line.
- Check the result with `node -v` and `npm -v`. `npm install -g npm` can update the npm CLI that shipped with Node. It still does not move you to another Node major.
- Native addons compile against a Node ABI. After a major upgrade, reinstall dependencies so those addons rebuild.
- `npm cache clean` is not an upgrade step.

**When to use:**

- Local machines: a version manager, so each repo can pin its own major.
- CI and production: the same major as `.nvmrc` or `engines`, installed by the image or the installer, not by floating `npm install -g`.

**Examples:**

```bash
nvm install --lts
nvm use --lts
node -v
npm -v
```

**Sources:**

- [Node.js — Download](https://nodejs.org/en/download)
- [Node.js releases](https://nodejs.org/en/about/previous-releases)
</details>

<details>
<summary>What is npm and what is it used for?</summary>

### What is npm and what is it used for?

**Definition:**

npm is the package manager that ships with Node and the name of the public registry it talks to. The CLI installs libraries, records version ranges, runs project scripts, and can publish a package you own.

**How it works:**

- The client is the `npm` command on your machine. The registry is the database of published packages, operated at registry.npmjs.org. The website npmjs.com is a browser view of that registry, not a second installer.
- `npm install` reads `package.json`, downloads the tree into `node_modules`, and writes `package-lock.json` so the next install can repeat the same versions.
- `npm init` creates `package.json`. `npm publish` uploads a package. `npm install <pkg>` adds a dependency. `npm uninstall <pkg>` removes one.
- `scripts` in `package.json` are named commands. `npm start` and `npm test` are special and do not need `run`. Anything else is `npm run <name>`. Scripts see `node_modules/.bin` on `PATH`.
- Version ranges and the lockfile are how installs stay predictable. The lockfile is what CI should install from (`npm ci`), not a fresh resolve of every range.
- Yarn and pnpm are other clients. They use their own lockfiles (`yarn.lock`, `pnpm-lock.yaml`) and can use the same registry. Both npm and Yarn are deterministic when you commit the lockfile. The old gap was install behavior, not “only Yarn has a lockfile”.

**When to use:**

- Every Node project: declare dependencies, install them, and run build, test, and start from `scripts`.
- Publishing a library: `npm publish` after the package is named, versioned, and licensed.

**Examples:**

```json
{
  "scripts": {
    "start": "node server.js"
  }
}
```

```bash
npm install express
npm install --save-dev vitest
npm start
npm run test
```

**Sources:**

- [npm Docs — About npm](https://docs.npmjs.com/about-npm)
- [npm Docs — npm-run-script](https://docs.npmjs.com/cli/v10/commands/npm-run-script)
</details>

<details>
<summary>How do you manage packages in a Node.js project?</summary>

### How do you manage packages in a Node.js project?

**Definition:**

A project declares dependencies and scripts in `package.json` and pins the installed tree in a lockfile. npm, Yarn, or pnpm installs, updates, and removes modules from that declaration. Pick one client per repo and commit its lockfile.

**How it works:**

- `npm init` creates the manifest. `npm install <pkg>` adds a `dependencies` entry and updates `package-lock.json`. `--save-dev` (`-D`) records a `devDependency` (TypeScript, a test runner). Yarn’s add command is `yarn add <pkg>`, and `yarn add -D <pkg>` for dev.
- `npm update <pkg>` moves installed packages inside the ranges already allowed in `package.json`. It does not ignore the range and jump to any latest tag. A major bump is a deliberate version edit, then install.
- `npm uninstall <pkg>` removes it from `node_modules` and from the manifest. Yarn’s match is `yarn remove`.
- Both clients write a lockfile: `package-lock.json`, `yarn.lock`, or `pnpm-lock.yaml`. That file is what makes installs repeatable across machines. Commit it. CI should use `npm ci` (or the Yarn/pnpm frozen install), which refuses to drift from the lockfile.
- `node_modules` is the install output. Do not commit it. Production installs can omit devDependencies (`npm ci --omit=dev`).
- Scripts in `package.json` are the project’s repeatable commands: `npm run <name>` from the project directory.

**When to use:**

- Application repos: one lockfile, `npm ci` in CI, ranges only as wide as you are willing to accept on the next update.
- Libraries: ranges in `dependencies` so apps can dedupe; you still lock the library’s own dev tools.

**Examples:**

```bash
npm install lodash
npm install express --save-dev
npm update lodash
npm uninstall lodash
npm ci
```

**Sources:**

- [npm Docs — package.json](https://docs.npmjs.com/cli/v10/configuring-npm/package-json)
- [npm Docs — npm-ci](https://docs.npmjs.com/cli/v10/commands/npm-ci)
</details>

<details>
<summary>What is a package.json file?</summary>

### What is a package.json file?

**Definition:**

`package.json` is the manifest for a Node project or a published package. It names the project, lists the packages it needs, and defines the commands npm can run. The installer and, for published packages, Node’s module loader both read it.

**How it works:**

- `name` and `version` identify a package. They are required if you publish. `private: true` marks an app that must not be published.
- `description`, `author`, `license`, and `repository` are metadata. They matter when the package is public.
- `dependencies` are needed at runtime. `devDependencies` are needed to build or test. `optionalDependencies` may fail to install without failing the whole tree. `peerDependencies` are packages the host app must supply (a plugin’s expected React, for example); they are not installed inside the plugin by default.
- Version ranges follow semver. `^1.2.3` allows compatible minor and patch updates. `~1.2.3` allows patch updates. The lockfile records the version that was actually installed.
- `scripts` maps a name to a shell command. `npm start` and `npm test` are the conventional shortcuts. `npm run build` runs whatever `build` contains.
- `main`, `exports`, and `type` tell Node how to load the package. `engines` says which Node versions you expect. Create the file with `npm init` or `yarn init`. Changing a version by hand without updating the lockfile makes the next install drift.

**When to use:**

- Every project, including a single script, once it has a dependency or a command you want to repeat.
- CI reads the same file: install from the lockfile, then `npm test` or `npm run build`. The manifest is the contract; the CI system does not need its own copy of the command if the script is defined here.

**Examples:**

```json
{
  "name": "notes",
  "version": "1.0.0",
  "private": true,
  "type": "module",
  "scripts": {
    "start": "node server.js",
    "build": "tsc",
    "test": "node --test"
  },
  "dependencies": { "express": "^5.1.0" }
}
```

**Sources:**

- [npm Docs — package.json](https://docs.npmjs.com/cli/v10/configuring-npm/package-json)
- [Node.js — package.json](https://nodejs.org/api/packages.html#nodejs-packagejson-field-definitions)
</details>

### Node.js Core Modules

<details>
<summary>Describe some of the core modules of Node.js.</summary>

### Describe some of the core modules of Node.js.

**Definition:**

Core modules ship inside the `node` binary. You import them with the `node:` prefix and do not install them from the registry. They cover the operating system, files, networking, and the data types the runtime uses for I/O.

**How it works:**

- System: `os` reports memory, CPUs, and platform (`os.freemem()`, `os.totalmem()`). `process` is the current process (env, exit, `nextTick`). `util` holds small helpers such as `inspect` and `promisify`.
- Files: `fs` reads, writes, and watches files. `path` joins and parses path strings without touching the disk. `url` parses and formats URLs.
- Network: `http` and `https` implement an HTTP server and client (`http.createServer`). `net` is TCP. `dgram` is UDP.
- Data: `stream` is chunked I/O. `buffer` is raw bytes. `zlib` compresses. `crypto` hashes and HMACs. `events` is `EventEmitter`.
- Processes: `child_process` spawns OS processes. `worker_threads` starts extra JavaScript isolates. `cluster` forks workers that share a server port.
- Prefer `node:fs` over the bare name `fs`. A local file named `fs.js` cannot shadow the builtin when the scheme is present.
- These modules are available in every Node process. They are not available in a browser bundle unless a bundler polyfills them, and most should not be polyfilled into client code.

**When to use:**

- Reach for a core module before a dependency when the task is files, HTTP, paths, crypto, or processes. Add a package when you need routing, a database driver, or a protocol Node does not implement.

**Examples:**

```js
import os from "node:os";
import { createServer } from "node:http";

console.log(os.freemem(), os.totalmem());
createServer((_req, res) => res.end("ok")).listen(0);
```

**Sources:**

- [Node.js — Core modules](https://nodejs.org/api/modules.html#built-in-modules)
</details>

<details>
<summary>How do you create a simple server in Node.js using the HTTP module?</summary>

### How do you create a simple server in Node.js using the HTTP module?

**Definition:**

`http.createServer` builds a server. Its request listener runs once per request with an `IncomingMessage` (`req`) and a `ServerResponse` (`res`). `listen` binds a TCP port and starts accepting connections.

**How it works:**

- Import `node:http`. Pass a listener to `createServer`, or call `server.on("request", listener)`.
- `req` is the incoming message: `req.method`, `req.url` (path plus query string), and `req.headers`. The body is not a string. It is a readable stream you consume if the client sent one.
- `res` is the outgoing message. `writeHead(status, headers)` sets the status and headers. `write` sends a chunk. `end` sends optional last bytes and finishes the response. Until `end`, the client stays open.
- `server.listen(port)` starts accepting. The `listening` callback runs once the port is bound. Port `0` asks the OS for a free port.
- Routing is your `if` on `req.url` and `req.method`. This is the server Express wraps. A framework adds the router, body parser, and error handling you would otherwise write here.
- One listener handles every connection on the event loop. A slow synchronous body in that function blocks the others.

**When to use:**

- A tiny service, a health check, or learning the raw request lifecycle.
- For a real API, a framework is usually less code. The HTTP module is still what that framework calls.

**Examples:**

```js
import http from "node:http";

const requestListener = (req, res) => {
  if (req.url === "/profile") {
    res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
    res.end("Welcome to your profile");
    return;
  }
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end("Hello, World!");
};

const server = http.createServer(requestListener);
server.listen(8080);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
</details>

<details>
<summary>Explain the purpose of the File System (fs) module.</summary>

### Explain the purpose of the File System (fs) module.

**Definition:**

`node:fs` is the core module for files and directories on the local machine: read, write, rename, list, permissions, and links. It is available without an install. A browser page cannot call it.

**How it works:**

- Most operations have a callback form and a promise form (`node:fs/promises`). Both return control immediately and finish on the event loop. File work uses libuv’s thread pool.
- The synchronous twins end with `Sync` (`readFileSync`, `renameSync`). They block the event loop until the kernel returns. That is acceptable in a one-shot script and a bad fit inside an HTTP handler that shares the process with other connections.
- I/O: `readFile` / `writeFile` for a whole file, or `createReadStream` / `createWriteStream` when the file is large.
- Metadata: `stat` gives size, mode, and timestamps. `access` checks permissions.
- Directories: `mkdir`, `readdir`, `rm`. `rename` and `unlink` move and delete. `link` and `symlink` create hard and symbolic links.
- Paths are OS paths. Join them with `node:path`, or use a `file:` URL relative to `import.meta.url`. Do not build paths by concatenating strings.
- Every async call can fail. Handle the `err` argument or `await` inside `try`/`catch`. A bare `throw` inside a callback is an uncaught exception.

**When to use:**

- Server code, CLIs, and build tools that touch disk.
- Prefer promises or callbacks on a server. Prefer streams for large files. Leave `*Sync` to startup code and short scripts.

**Examples:**

```js
import { readFile } from "node:fs/promises";
import { readFileSync } from "node:fs";

const text = await readFile(new URL("./input.txt", import.meta.url), "utf8");
console.log("Asynchronous read:", text);

const syncText = readFileSync(new URL("./input.txt", import.meta.url), "utf8");
console.log("Synchronous read:", syncText);
```

**Sources:**

- [Node.js — File system](https://nodejs.org/api/fs.html)
</details>

<details>
<summary>What is the Buffer class in Node.js?</summary>

### What is the Buffer class in Node.js?

**Definition:**

`Buffer` is Node’s fixed-size sequence of raw bytes. Files, sockets, and crypto deal in octets, not JavaScript strings, and a buffer is the object those APIs pass around. It is a global, and it also lives on `node:buffer`.

**How it works:**

- It is a subclass of `Uint8Array`. The length is fixed after allocation. To “resize”, allocate another buffer and copy.
- `Buffer.from("Hey!")` encodes a string (utf8 by default). `buf.toString()` decodes it back. `Buffer.alloc(5, "a")` creates a zero-filled buffer and fills it with the byte for `a`.
- `buf.write(string)` copies encoded bytes into the existing buffer and stops at `buf.length`. It does not grow the buffer.
- `Buffer.alloc` zeroes memory. `Buffer.allocUnsafe` skips that and can expose old bytes if you do not overwrite every index.
- `subarray(start, end)` is a view of the same memory. Writing through the view changes the original. Copy when you need an independent buffer.
- HTTP chunks and file chunks often arrive as buffers. Decode with an explicit encoding when the payload is text. For a large body, stream it instead of concatenating every chunk into one buffer.
- `Buffer` is not a character array. `Buffer.from("é").length` is the byte length, which is greater than 1 in utf8.

**When to use:**

- Binary protocols, hashes, file chunks, and anything you must not mangle through a string encoding.
- Text in application code should stay a string until it crosses a byte boundary.

**Examples:**

```js
const fromText = Buffer.from("Hey!");
console.log(fromText.toString());

const allocated = Buffer.alloc(5, "a");
console.log(allocated.toString());

allocated.write("Hello");
console.log(allocated.toString());

const head = allocated.subarray(0, 3);
console.log(head.toString());
```

**Sources:**

- [Node.js — Buffer](https://nodejs.org/api/buffer.html)
</details>

<details>
<summary>What are streams in Node.js and what types are available?</summary>

### What are streams in Node.js and what types are available?

**Definition:**

A stream moves data in chunks instead of holding the whole payload in memory. Node has four kinds: readable, writable, duplex, and transform. Standard input, output, and error are streams too.

**How it works:**

- Readable: you read from it. `fs.createReadStream` and an HTTP request body are readable. Consume them with `for await` or the `data` event, then `end`.
- Writable: you write to it. `fs.createWriteStream` and an HTTP response are writable. `write` sends a chunk. `end` finishes. `process.stdout` and `process.stderr` are writable. `process.stdin` is readable.
- Duplex: both directions on one object, as with a TCP socket (`net.Socket`). The read side and the write side are independent.
- Transform: a duplex that modifies chunks as they pass, such as `zlib.createGzip()` or a hash stream.
- `pipe` connects a readable to a writable. `pipeline` from `node:stream/promises` connects a whole chain, forwards errors, and destroys the streams if one fails. Prefer it over a hand-rolled `pipe` when there is more than one hop.
- A typical chain reads a file, gzip-transforms the bytes, and writes the result. Memory stays near the chunk size, not the file size.
- Backpressure matters. `write` returns `false` when the destination is behind. Wait for `drain` before producing more. `pipeline` handles that for you.
- HTTP is streams in both directions: the client writes a request and reads a response; the server reads the request and writes the response.

**When to use:**

- Large files, uploads, compression, and any response you can send as it is produced.
- Small config files are simpler with `readFile`. A stream is extra machinery when the payload fits in memory on purpose.

**Examples:**

```js
import { createReadStream, createWriteStream } from "node:fs";
import { createGzip } from "node:zlib";
import { pipeline } from "node:stream/promises";

await pipeline(
  createReadStream("notes.txt"),
  createGzip(),
  createWriteStream("notes.txt.gz"),
);
```

**Sources:**

- [Node.js — Stream](https://nodejs.org/api/stream.html)
</details>

<details>
<summary>How do you read and write files in Node.js?</summary>

### How do you read and write files in Node.js?

**Definition:**

Use `readFile` and `writeFile` from `node:fs/promises` when the whole file fits in memory. Pass a path or a `file:` URL and an encoding such as `"utf8"` when the contents are text. For a large file, stream it with `createReadStream` and `createWriteStream` instead of buffering it.

**How it works:**

- Promise form: `import { readFile, writeFile } from "node:fs/promises"`. `await readFile(path, "utf8")` fulfills with a string. Omit the encoding and the promise fulfills with a `Buffer`.
- `writeFile(path, data, "utf8")` creates the file or replaces it. `data` can be a string, a `Buffer`, or a `TypedArray`. The call returns a promise.
- Callback form: `readFile` from `node:fs` takes `(err, data)`. The callback style is the one the docs point at when you need the lowest overhead.
- Both async forms run the filesystem work on libuv’s thread pool and finish on the event loop. Concurrent writes to the same file are not synchronized; the process can corrupt that file.
- `readFile` buffers the entire file. The docs say to prefer `createReadStream` once the file is large.
- A `file:` URL from `new URL("./note.txt", import.meta.url)` is a valid path. It stays relative to the module, which `process.cwd()` does not.

**When to use:**

- Config, templates, and small documents: `readFile` / `writeFile`.
- Uploads, logs, and big artifacts: streams, usually through `pipeline` from `node:stream/promises`.

**Examples:**

```js
import { readFile, writeFile } from "node:fs/promises";

const fileUrl = new URL("./note.txt", import.meta.url);

await writeFile(fileUrl, "hello\n", "utf8");
const text = await readFile(fileUrl, "utf8");
console.log(text);
```

**Sources:**

- [Node.js — File system](https://nodejs.org/api/fs.html)
</details>

<details>
<summary>How do you use the EventEmitter in Node.js?</summary>

### How do you use the EventEmitter in Node.js?

**Definition:**

`EventEmitter` from `node:events` is a registry of named listeners. You subclass it or use it directly, register functions with `on` or `once`, and call `emit` when something happens. Many core objects (`http.Server`, streams, `process`) are emitters.

**How it works:**

- `on(name, listener)` appends a listener. `once(name, listener)` removes it after the first call. `emit(name, ...args)` invokes the listeners for that name, in registration order.
- `removeListener(name, listener)` drops one registration. The function you pass must be the same reference you registered.
- An `'error'` event with no listener throws, prints a stack, and exits the process. Add an `'error'` listener on every emitter you own.
- `listenerCount(name)` and `eventNames()` tell you what is still attached. A listener that is never removed keeps the emitter alive.
- `prependListener` inserts at the front. The default is to run in the order listeners were added.
- `events.once(emitter, name)` returns a promise that settles on the next matching event, which is useful with `await`.

**When to use:**

- Your own pub/sub inside one process: progress, shutdown, a job finishing.
- Core I/O objects already emit. Subscribe to those events instead of wrapping them in a second bus.

**Examples:**

```js
import { EventEmitter } from "node:events";

class JobEmitter extends EventEmitter {}

const jobs = new JobEmitter();

jobs.on("error", (err) => {
  console.error(err.message);
});

jobs.once("done", (id) => {
  console.log("finished", id);
});

jobs.emit("done", 7);
jobs.emit("error", new Error("disk full"));
```

**Sources:**

- [Node.js — Events](https://nodejs.org/api/events.html)
</details>

<details>
<summary>What is the QueryString module?</summary>

### What is the QueryString module?

**Definition:**

`node:querystring` parses and formats URL query strings. `parse` turns `foo=bar&abc=xyz` into an object. `stringify` turns an object back into a query string. It is a Node API, faster than `URLSearchParams` on large strings, and it is not the WHATWG standard.

**How it works:**

- `querystring.parse(str)` splits on `&` and `=`, decodes percent-escapes, and collects repeated keys into an array.
- `querystring.stringify(obj)` encodes keys and values. An array value repeats the key (`baz=qux&baz=quux`).
- The default encoding is utf8. `sep` and `eq` options change the delimiters.
- `URL` and `URLSearchParams` are globals and follow the browser rules. The docs recommend them when you want the same behavior as browser code.
- `querystring` stays available for code that already depends on its parsing rules. Those rules differ from `URLSearchParams` on some edge cases (for example, how `+` is treated).
- The query lives on `req.url` for an `node:http` request. Split off the part after `?`, or pass the request URL to `new URL(req.url, "http://localhost")` and read `searchParams`.

**When to use:**

- `URLSearchParams` for new code that should match the web platform.
- `node:querystring` when a measured hot path needs its parser, or when you must keep existing parse results stable.

**Examples:**

```js
import { parse, stringify } from "node:querystring";

const parsed = parse("foo=bar&abc=xyz&abc=123");
console.log(parsed.foo, parsed.abc);

const built = stringify({ foo: "bar", baz: ["qux", "quux"] });
console.log(built);

const params = new URLSearchParams("foo=bar&abc=xyz");
console.log(params.get("foo"));
```

**Sources:**

- [Node.js — Query string](https://nodejs.org/api/querystring.html)
- [Node.js — URL](https://nodejs.org/api/url.html)
</details>

<details>
<summary>How do you manage path operations in Node.js?</summary>

### How do you manage path operations in Node.js?

**Definition:**

`node:path` joins, resolves, and splits filesystem paths. Use `join` and `resolve` instead of concatenating strings, because Windows and POSIX use different separators. The methods on `path` follow the operating system you are running on. `path.posix` and `path.win32` give a fixed style on any machine.

**How it works:**

- `path.join(...segments)` concatenates with the platform separator and normalizes `.` and `..`. Zero-length segments are ignored. A non-string segment throws `TypeError`.
- `path.resolve(...segments)` builds an absolute path from right to left, starting from `process.cwd()` when the segments are relative.
- `path.basename(p, suffix)` is the last segment. The suffix is removed only when it matches, and the match is case-sensitive.
- `path.dirname` is the directory. `path.extname` is the extension, including the dot.
- `path.sep` is `\` on Windows and `/` on POSIX. `path.basename('C:\\temp\\myfile.html')` does not split on backslashes on POSIX. Use `path.win32` when the string is a Windows path regardless of the host.
- A module should anchor files with `new URL("./data", import.meta.url)` or `path.join` from a known directory. `process.cwd()` changes when the process is started from another folder.

**When to use:**

- Any user path, upload name, or config location you combine with a base directory.
- Normalize before you check that the result still sits inside an allowed root (`path.resolve` plus a prefix check).

**Examples:**

```js
import path from "node:path";

const joined = path.join("/foo", "bar", "baz/asdf", "quux", "..");
console.log(joined);

const base = path.basename("/foo/bar/baz/asdf/quux.html", ".html");
console.log(base);

const fileUrl = new URL("./note.txt", import.meta.url);
console.log(fileUrl.pathname);
```

**Sources:**

- [Node.js — Path](https://nodejs.org/api/path.html)
</details>

### Asynchronous Programming

<details>
<summary>What are callbacks in Node.js?</summary>

### What are callbacks in Node.js?

**Definition:**

A callback is a function you pass so it can run later, when an operation finishes. In Node’s core I/O APIs the usual shape is an error-first callback: the first argument is `err` (`null` on success), and the next arguments are the result.

**How it works:**

- `readFile(path, (err, data) => { ... })` from `node:fs` returns immediately. The callback runs on a later turn of the event loop, after the thread pool finishes the read.
- Check `err` before you use `data`. A thrown error inside the callback is an uncaught exception unless something else catches it.
- The same function can be called synchronously or on a later tick. An API that sometimes calls the callback before it returns is hard to reason about. The `process.nextTick` docs show deferring the sync branch so the callback is always async.
- Callbacks compose by nesting or by naming the next function. They do not return a value to the caller of `readFile`.
- `util.promisify` wraps an error-first function in a promise. Many modules already ship a promise API (`node:fs/promises`).
- Timers (`setTimeout`, `setImmediate`) are callbacks too. They are scheduled on the event loop, not on the thread pool.

**When to use:**

- You are calling a callback-style core API and want the lower allocation cost the fs docs mention.
- New application code usually awaits a promise API. Keep the callback at the boundary that still requires one.

**Examples:**

```js
import { readFile } from "node:fs";

readFile(new URL("./note.txt", import.meta.url), "utf8", (err, data) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(data);
});
```

**Sources:**

- [Node.js — File system](https://nodejs.org/api/fs.html)
- [Node.js — process.nextTick](https://nodejs.org/api/process.html#processnexttickcallback-args)
</details>

<details>
<summary>What is callback hell and how can it be avoided?</summary>

### What is callback hell and how can it be avoided?

**Definition:**

Callback hell is deeply nested error-first callbacks, each starting the next step only inside the previous one. The control flow is hard to read, and every level needs its own `err` check. Flatten it with named functions, promises, or `async`/`await`.

**How it works:**

- Nesting grows when step B needs the result of step A and step C needs B: read a path from one file, then read that path, then write a result.
- Each callback closes over the outer variables. An early `return` after `err` is required at every level, or a later step runs on a failure.
- A named function per step (`readConfig`, `readBody`, `writeOut`) keeps the nesting at one level. You pass the next function in.
- `node:fs/promises` turns the same steps into `await` inside one `try`/`catch`. The error lands in the `catch` instead of an `err` argument.
- `Promise.all` runs independent steps together. Nested callbacks hide that independence and run the steps one after another.
- `pipeline` from `node:stream/promises` replaces a chain of stream callbacks and forwards the first error.

**When to use:**

- Any sequence of I/O in application code: prefer `async`/`await`.
- Keep a callback only where the API you call is callback-only and a promise wrapper is not worth it.

**Examples:**

```js
import { readFile, writeFile } from "node:fs/promises";

const dir = new URL("./", import.meta.url);

try {
  const name = (await readFile(new URL("name.txt", dir), "utf8")).trim();
  const body = await readFile(new URL(name, dir), "utf8");
  await writeFile(new URL("out.txt", dir), body.toUpperCase(), "utf8");
} catch (err) {
  console.error(err);
}
```

**Sources:**

- [Node.js — File system](https://nodejs.org/api/fs.html)
- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
</details>

<details>
<summary>Explain promises in Node.js.</summary>

### Explain promises in Node.js.

**Definition:**

A promise is an object that stands for a value that is not ready yet. It settles once: fulfilled with a value, or rejected with a reason. Node’s promise APIs (`node:fs/promises`, `node:stream/promises`, `node:timers/promises`) return promises so you can chain work without nesting callbacks.

**How it works:**

- `then` runs on fulfillment. `catch` runs on rejection. `finally` runs either way. Returning a promise from `then` chains the next wait.
- An `async` function always returns a promise. `await` pauses that function until the promise settles and resumes it in a microtask.
- A rejected promise with no `catch` or `await` in `try` emits `unhandledRejection` on `process`. Handle it at the call site.
- `Promise.all` waits for every promise and rejects on the first failure. `Promise.allSettled` waits for every outcome. `Promise.race` settles with the first result.
- `util.promisify(fn)` expects an error-first callback. The fs promise API is already promisified; prefer that import over wrapping `node:fs` yourself.
- Promise reactions use the microtask queue. In a CommonJS file they run after the current `process.nextTick` queue drains. In an ES module the order relative to `nextTick` differs because the module itself is evaluated from the microtask queue.

**When to use:**

- Application I/O, sequencing, and anything you want `try`/`catch` to cover.
- Callback style remains available on `node:fs` when the docs’ performance note applies.

**Examples:**

```js
import { readFile } from "node:fs/promises";

readFile(new URL("./note.txt", import.meta.url), "utf8")
  .then((text) => {
    console.log(text);
  })
  .catch((err) => {
    console.error(err);
  });
```

**Sources:**

- [Node.js — File system](https://nodejs.org/api/fs.html)
- [Node.js — process.nextTick](https://nodejs.org/api/process.html#processnexttickcallback-args)
</details>

<details>
<summary>How do async/await functions work in Node.js?</summary>

### How do async/await functions work in Node.js?

**Definition:**

`async` marks a function that returns a promise. `await` suspends that function until the promise fulfills, then continues with the value. A rejection throws at the `await`, so `try`/`catch` handles it. The caller of the `async` function gets a promise immediately.

**How it works:**

- `await` only pauses the async function, not the process. Other event-loop work runs while the I/O is in flight.
- Code after `await` runs as a microtask continuation. It does not block the thread the way `readFileSync` does.
- `await` on a non-promise wraps the value in a resolved promise and continues on a later microtask.
- Parallel independent work is `await Promise.all([readA(), readB()])`. Awaiting them one after another adds the latencies.
- A top-level `await` is allowed in ES modules. In CommonJS you wrap the body in an `async` function and call it.
- An `async` function that throws, or awaits a rejection, returns a rejected promise. If nobody awaits it, `process` emits `unhandledRejection`.

**When to use:**

- Request handlers, scripts, and any sequence of promise APIs.
- Stay on callbacks only at a boundary that has no promise form.

**Examples:**

```js
import { readFile, writeFile } from "node:fs/promises";

const fileUrl = new URL("./note.txt", import.meta.url);

try {
  const text = await readFile(fileUrl, "utf8");
  await writeFile(fileUrl, text.trim() + "\n", "utf8");
} catch (err) {
  console.error(err);
}
```

**Sources:**

- [Node.js — File system](https://nodejs.org/api/fs.html)
- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
</details>

<details>
<summary>What is the difference between synchronous and asynchronous methods in the fs module?</summary>

### What is the difference between synchronous and asynchronous methods in the fs module?

**Definition:**

Asynchronous `node:fs` methods (`readFile`, and the same names on `node:fs/promises`) return control immediately and deliver the result later. Synchronous methods end in `Sync` (`readFileSync`) and return the result only after the kernel finishes, so nothing else on the main thread runs until then.

**How it works:**

- `readFile` / `fsPromises.readFile` schedule the work on libuv’s thread pool. The event loop keeps processing other callbacks. Completion comes back as a callback or a fulfilled promise.
- `readFileSync` runs on the main thread. Timers, sockets, and other requests wait behind it.
- The promise API and the callback API are both asynchronous. The docs note that the callback API allocates less when you need the maximum throughput.
- The promise operations are not thread-safe with each other. Two overlapping writes to one file can corrupt it. `Sync` calls do not overlap with JavaScript on the same thread because they block it.
- `readFile` buffers the whole file in every style. A large file still wants `createReadStream`, which is asynchronous.
- Errors: `Sync` throws. The callback receives `err`. The promise rejects, so you `await` it inside `try`/`catch`.

**When to use:**

- `*Sync` in startup code and short scripts, before the server calls `listen`.
- Callbacks or promises inside a server that shares the process with other connections.

**Examples:**

```js
import { readFile } from "node:fs/promises";
import { readFileSync } from "node:fs";

const fileUrl = new URL("./note.txt", import.meta.url);

const asyncText = await readFile(fileUrl, "utf8");
console.log("async", asyncText);

const syncText = readFileSync(fileUrl, "utf8");
console.log("sync", syncText);
```

**Sources:**

- [Node.js — File system](https://nodejs.org/api/fs.html)
- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
</details>

### Networking in Node.js

<details>
<summary>How does Node.js handle HTTP requests and responses?</summary>

### How does Node.js handle HTTP requests and responses?

**Definition:**

`http.createServer` listens for TCP connections and emits `'request'` with an `IncomingMessage` (`req`) and a `ServerResponse` (`res`). `req` is a readable stream of the body. `res` is a writable stream you finish with `end`. One listener runs per request on the event loop.

**How it works:**

- `req.method` and `req.url` identify the request. `req.headers` is a lower-cased map. The body arrives in chunks; consume it with `for await (const chunk of req)` or the `'data'` / `'end'` events.
- `res.writeHead(statusCode, headers)` sends the status and headers. Call it once, before `end`. Headers passed to `writeHead` win over earlier `setHeader` values.
- `res.end(body)` finishes the message. `Content-Length` is a byte length. Use `Buffer.byteLength(body)` for a string.
- `server.listen(port)` binds the port. The `'error'` event on the server reports a failure to bind, such as `EADDRINUSE`.
- The `'upgrade'` event is the raw HTTP upgrade hook (the docs show a 101 handshake). It is not a WebSocket server.
- `http.request` / `http.get` are the client. The response object is also an `IncomingMessage`.

**When to use:**

- A small service, a proxy, or any server you want to keep on core APIs.
- A framework such as Express sits on this same `request` event and adds routing.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((req, res) => {
  const body = JSON.stringify({ method: req.method, url: req.url });
  res.writeHead(200, {
    "Content-Type": "application/json; charset=utf-8",
    "Content-Length": Buffer.byteLength(body),
  });
  res.end(body);
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
</details>

<details>
<summary>What is Express.js and why is it important for Node.js?</summary>

### What is Express.js and why is it important for Node.js?

**Definition:**

Express is an npm package, not a core module. It wraps `node:http` with a router, a middleware chain of `(req, res, next)`, and helpers for status codes and response bodies. It became the usual way teams describe a Node HTTP API.

**How it works:**

- `express()` returns an app. `app.listen(port)` creates the underlying HTTP server and binds it.
- `app.get(path, handler)` and the other method helpers match `req.method` plus a path pattern. Route params land on `req.params`.
- `app.use(fn)` registers middleware that runs before the route handler. `next()` continues the chain. `next(err)` skips to error-handling middleware with four arguments `(err, req, res, next)`.
- `express.json()` reads a JSON body and assigns `req.body`. Without it, the body is still the raw `IncomingMessage` stream from `node:http`.
- Express does not replace the event loop. A long CPU loop in a handler still blocks every other request on that process.
- The same app can be mounted under a prefix or handed to `http.createServer(app)` when you need the core server object.

**When to use:**

- JSON APIs and server-rendered pages where routing and middleware are the whole problem.
- Stay on `node:http` when you want no dependency and a single listener is enough.

**Examples:**

```js
import express from "express";

const app = express();

app.use(express.json());

app.get("/health", (_req, res) => {
  res.status(200).json({ ok: true });
});

app.listen(3000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Express — Routing](https://expressjs.com/en/guide/routing.html)
</details>

<details>
<summary>How do you create a RESTful API with Node.js?</summary>

### How do you create a RESTful API with Node.js?

**Definition:**

A REST API maps resources to URLs and actions to HTTP methods: `GET` reads, `POST` creates, `PUT` or `PATCH` updates, `DELETE` removes. With `node:http` you branch on `req.method` and the pathname, then answer with a status code and a JSON body.

**How it works:**

- Parse `req.url` with `new URL(req.url, "http://localhost")`. Use `url.pathname` for the route and `url.searchParams` for the query. The host in that constructor only fills in a base; the client’s `Host` header is separate.
- Read a JSON body by collecting the request stream, then `JSON.parse`. Reject a body that exceeds a size limit before you parse it.
- Send `201` after a create, `204` for an empty success, `404` when the resource is missing, `400` for a bad body. Set `Content-Type: application/json; charset=utf-8`.
- Keep handlers free of shared mutable state so one request cannot corrupt another. Persistence belongs in a database driver, which is not part of `node:http`.
- Idempotent methods (`GET`, `PUT`, `DELETE`) should be safe to retry. `POST` is the method that creates a new resource each call.
- A framework router is the same idea with less manual matching. The status codes and methods stay HTTP, not Express-specific.

**When to use:**

- Resource-shaped APIs consumed by a browser, a mobile app, or another service.
- Streaming or server-push protocols are a different shape. REST is request then response.

**Examples:**

```js
import http from "node:http";

const notes = new Map();

const server = http.createServer((req, res) => {
  const url = new URL(req.url ?? "/", "http://localhost");
  if (req.method === "GET" && url.pathname === "/notes") {
    const body = JSON.stringify([...notes.values()]);
    res.writeHead(200, {
      "Content-Type": "application/json; charset=utf-8",
      "Content-Length": Buffer.byteLength(body),
    });
    res.end(body);
    return;
  }
  res.writeHead(404, { "Content-Type": "text/plain; charset=utf-8" });
  res.end("not found");
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [MDN — HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods)
</details>

<details>
<summary>What is middleware in the context of Node.js?</summary>

### What is middleware in the context of Node.js?

**Definition:**

Middleware is a function that sits on the way from the raw request to the final response. It can read or change `req` and `res`, end the response, or pass control onward. `node:http` has no middleware type. The pattern is a chain you write, and Express popularized the signature `(req, res, next)`.

**How it works:**

- Each function does one job: log, check a header, parse JSON, set a security header. Calling `next()` runs the following function. Skipping `next()` and also skipping `res.end()` leaves the request hanging.
- `next(err)` jumps to an error handler. In Express that handler has four parameters, `(err, req, res, next)`, so the router can tell it apart from ordinary middleware.
- Order is the behavior. A body parser must run before a handler that reads `req.body`. An auth check must run before the route it protects.
- Core equivalent: one `createServer` listener calls small functions in sequence, or you wrap the listener. Streams still apply. Middleware that buffers a whole body should enforce a limit.
- Middleware shares the process. Anything it stores on a global leaks across requests. Put per-request data on `res.locals` or a `WeakMap` keyed by `req`.
- Error middleware still has to end the response. An unhandled throw in an async Express handler needs an explicit `next(err)` or a wrapper, because a rejected promise is not automatically passed to `next`.

**When to use:**

- Cross-cutting steps that many routes share: logging, auth, parsing, headers.
- A single handler with two lines of setup does not need a framework chain.

**Examples:**

```js
import http from "node:http";

function withJsonHeader(req, res, next) {
  res.setHeader("Content-Type", "application/json; charset=utf-8");
  next();
}

const server = http.createServer((req, res) => {
  withJsonHeader(req, res, () => {
    const body = JSON.stringify({ ok: true });
    res.setHeader("Content-Length", Buffer.byteLength(body));
    res.writeHead(200);
    res.end(body);
  });
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Express — Writing middleware](https://expressjs.com/en/guide/writing-middleware.html)
</details>

<details>
<summary>How do you ensure security in HTTP headers with Node.js?</summary>

### How do you ensure security in HTTP headers with Node.js?

**Definition:**

Set response headers that tell the browser how to treat your pages and responses. `res.setHeader(name, value)` on the `ServerResponse` is the core API. There is no security-header module built into Node. A package such as `helmet` only bundles the same `setHeader` calls.

**How it works:**

- `Content-Security-Policy` limits which scripts, styles, and frames a browser will load. It matters when you send HTML.
- `X-Content-Type-Options: nosniff` stops browsers from sniffing a response into a different type than `Content-Type`.
- `Strict-Transport-Security` tells the browser to keep using HTTPS for your host. Send it only on a real HTTPS response.
- `Referrer-Policy` and a tight `Content-Security-Policy` reduce what a browser leaks and executes. `Cache-Control` on authenticated JSON stops shared caches from storing it.
- Header values must not contain raw user input. `writeHead` throws `TypeError` on illegal header characters, which is the response-splitting case.
- `setHeader` merges into the headers `writeHead` sends. A later `writeHead` header overrides the same name.

**When to use:**

- Every response from a server that browsers call, including APIs that might be loaded in a page context.
- Put the calls in one middleware so a new route cannot forget them.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((_req, res) => {
  res.setHeader("X-Content-Type-Options", "nosniff");
  res.setHeader("Referrer-Policy", "no-referrer");
  res.setHeader("Content-Security-Policy", "default-src 'none'");
  res.writeHead(200, { "Content-Type": "application/json; charset=utf-8" });
  res.end(JSON.stringify({ ok: true }));
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Security best practices](https://nodejs.org/en/learn/getting-started/security-best-practices)
- [MDN — X-Content-Type-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/X-Content-Type-Options)
</details>

### Error Handling & Debugging

<details>
<summary>How do you handle errors in Node.js?</summary>

### How do you handle errors in Node.js?

**Definition:**

Handle each failure at the API that produced it. Synchronous throws and `await` use `try`/`catch`. Callbacks check the `err` argument. Event emitters need an `'error'` listener. A rejection or throw that escapes the request becomes `unhandledRejection` or `uncaughtException` on `process`.

**How it works:**

- An `Error` has `message` and `stack`. Many Node errors also set `code` (`ENOENT`, `EADDRINUSE`). Branch on `code` when you need a specific recovery.
- `'error'` on an `EventEmitter` with zero listeners throws and exits the process. Streams and sockets emit `'error'`. Listen, or use `pipeline`, which forwards the error and destroys the streams.
- `process` emits `unhandledRejection` for a rejected promise nobody handled, and `uncaughtException` for a throw on the event loop. The process docs treat `uncaughtException` as a last log-and-shutdown path: the process can be in a bad state, so stop accepting work and exit.
- `uncaughtExceptionMonitor` lets you log without becoming the official handler. Installing `uncaughtException` changes the default exit behavior, so you still exit yourself after logging.
- An async HTTP handler must catch and end the response. A throw after headers were sent cannot change the status line.
- Operational errors (a missing file, a down database) are recoverable per request. A programmer error (reading a property of `undefined`) means that request’s logic is wrong. Log it and fail the request.

**When to use:**

- `try`/`catch` around `await` in every request path that touches I/O.
- A process-level handler only to log and shut down, not to keep serving.

**Examples:**

```js
import { readFile } from "node:fs/promises";
import process from "node:process";

process.on("unhandledRejection", (reason) => {
  console.error(reason);
  process.exitCode = 1;
});

try {
  const text = await readFile(new URL("./note.txt", import.meta.url), "utf8");
  console.log(text);
} catch (err) {
  console.error(err.code ?? err.message);
}
```

**Sources:**

- [Node.js — Errors](https://nodejs.org/api/errors.html)
- [Node.js — Events](https://nodejs.org/api/events.html)
- [Node.js — Process](https://nodejs.org/api/process.html)
</details>

<details>
<summary>Describe some error first callback patterns in Node.js.</summary>

### Describe some error first callback patterns in Node.js.

**Definition:**

An error-first callback receives `err` as the first argument and the result after it. On success `err` is `null` or `undefined`. On failure `err` is an `Error` and the result is omitted. That is the contract of callback APIs in `node:fs` and most of the older core.

**How it works:**

- Return immediately after handling `err`. Code below the check must assume the result is present.
- Pass errors outward with `callback(err)` instead of throwing inside the callback. A throw from a later-tick callback is an uncaught exception.
- If your own function sometimes has the answer already, defer the callback with `process.nextTick` so callers can finish attaching their own logic. The process docs show this `definitelyAsync` shape.
- `util.promisify` maps this contract onto a promise: a falsy `err` fulfills with the result, a truthy `err` rejects.
- One callback should run once. Calling it twice delivers two completions to a caller that only expected one.
- Node-style callbacks do not return the result to the function that started the I/O. The result exists only inside the callback.

**When to use:**

- You are implementing or calling a callback core API (`readFile`, `randomBytes` from `node:crypto` in callback form).
- Promise APIs are the same contract already unpacked: rejection is the `err` argument.

**Examples:**

```js
import { readFile } from "node:fs";
import { nextTick } from "node:process";

function loadNote(callback) {
  readFile(new URL("./note.txt", import.meta.url), "utf8", (err, data) => {
    if (err) {
      callback(err);
      return;
    }
    callback(null, data);
  });
}

function alwaysAsync(value, callback) {
  nextTick(callback, null, value);
}

loadNote((err, text) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(text);
});

alwaysAsync("ok", (_err, value) => {
  console.log(value);
});
```

**Sources:**

- [Node.js — File system](https://nodejs.org/api/fs.html)
- [Node.js — process.nextTick](https://nodejs.org/api/process.html#processnexttickcallback-args)
</details>

<details>
<summary>What are some common debugging techniques for Node.js applications?</summary>

### What are some common debugging techniques for Node.js applications?

**Definition:**

Start the process with `node --inspect` so Chrome DevTools or a VS Code debugger can attach to the V8 inspector. Add `debugger` where you want a pause, and log structured details with `console`. For a hang or a leak, take a CPU profile or a heap snapshot from that same inspector session.

**How it works:**

- `node --inspect index.js` prints a `ws://127.0.0.1:9229/...` URL. The inspector binds to localhost. `--inspect-brk` pauses before any application code runs.
- Open `chrome://inspect` or attach from the editor. Breakpoints, the call stack, and scope work on the V8 isolate that is running your file.
- `debugger;` is a breakpoint when an inspector is attached. With no inspector, Node continues past it.
- `console.error` goes to `process.stderr`. Include `err.stack` and `err.code`. `util.debuglog` / `NODE_DEBUG` turn on scoped debug logs in core subsystems (`NODE_DEBUG=http`).
- `--enable-source-maps` maps stack frames back to TypeScript or transpiled lines when a source map is next to the file.
- A slow process wants a CPU profile from the inspector. A growing `process.memoryUsage().heapUsed` wants a heap snapshot. Both come from the same `--inspect` session.

**When to use:**

- A bug you can reproduce locally: `--inspect-brk` and a breakpoint.
- A production incident: logs and metrics first. Open the inspector on a machine only over a trusted tunnel, because the port can run code in the process.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((req, res) => {
  debugger;
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end(req.url ?? "/");
});

server.listen(8000);
```

**Sources:**

- [Node.js — Debugging](https://nodejs.org/en/learn/getting-started/debugging)
- [Node.js — Debugger](https://nodejs.org/api/debugger.html)
- [Node.js — Process](https://nodejs.org/api/process.html)
</details>

<details>
<summary>Explain process.nextTick().</summary>

### Explain process.nextTick().

**Definition:**

`process.nextTick(callback, ...args)` queues `callback` on the next-tick queue. That queue drains after the current JavaScript stack finishes and before I/O continues. For ordinary deferral the process docs favor `queueMicrotask()`, which uses the same microtask queue as promise reactions and works outside Node.

**How it works:**

- `nextTick` is not an event-loop phase. A callback that schedules another `nextTick` can starve timers and I/O, because the queue drains until it is empty.
- In a CommonJS module, `nextTick` callbacks run before promise reactions and `queueMicrotask`. After the next-tick queue, Node drains the microtask queue.
- In an ES module the file is already being evaluated from the microtask queue, so `queueMicrotask` and promise reactions run before `nextTick`.
- `nextTick(fn, a, b)` passes `a` and `b` to `fn`. `queueMicrotask` takes no extra arguments; bind or close over them.
- Use `nextTick` when an API must be asynchronous in every branch, including the branch that already has the answer. The docs’ `definitelyAsync` example defers that branch so the caller always sees a later callback.
- `setImmediate` yields to the event loop (the check phase). Prefer it when the goal is to let I/O run, not to run before I/O.

**When to use:**

- A library boundary that has to stay async on every path.
- Application code that only needs “run soon” should `await` a promise or call `queueMicrotask`.

**Examples:**

```js
import { nextTick } from "node:process";

function definitelyAsync(value, callback) {
  nextTick(callback, null, value);
}

console.log("start");
definitelyAsync("ok", (_err, value) => {
  console.log(value);
});
console.log("scheduled");
```

**Sources:**

- [Node.js — process.nextTick](https://nodejs.org/api/process.html#processnexttickcallback-args)
- [Node.js — Event loop, timers, and process.nextTick](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick)
</details>

<details>
<summary>What is the global object in Node.js?</summary>

### What is the global object in Node.js?

**Definition:**

The global object is the place truly global bindings live. In Node its historical name is `global`. The globals docs mark `global` as legacy and tell you to use `globalThis`, which is the same object and is the name other JavaScript hosts use.

**How it works:**

- Top-level `var`, `const`, and `function` in a Node module stay local to that module. They do not become properties of `global`. That is true for CommonJS and for ES modules.
- CommonJS-only names look global and are not: `__dirname`, `__filename`, `exports`, `module`, `require`. They exist in the module wrapper. ES modules get `import.meta.url` instead.
- Real globals include `console`, `Buffer`, `process`, `URL`, `URLSearchParams`, `fetch`, `TextEncoder`, `queueMicrotask`, and `setTimeout`. `WebSocket` is a global client, stable since Node 22.4, and it is not a server.
- Assigning `globalThis.myApp = ...` publishes a value to every module in the isolate. That shared slot is a common way to leak state across requests.
- `globalThis` in the browser is `window`. Code that only touches standard globals can run in both places. `process` and `Buffer` will not exist in the browser.
- A worker thread has its own `globalThis`. It does not share the main thread’s object. Messages are copied or transferred.

**When to use:**

- Read standard globals (`URL`, `fetch`, `queueMicrotask`) directly.
- Put application state in module scope or an explicit argument. `globalThis` is for a binding that truly must be visible everywhere in the isolate.

**Examples:**

```js
console.log(globalThis === global);
console.log(typeof globalThis.fetch);
console.log(typeof globalThis.WebSocket);

const params = new URLSearchParams({ q: "node" });
console.log(params.get("q"));
```

**Sources:**

- [Node.js — Global objects](https://nodejs.org/api/globals.html)
</details>

### Testing in Node.js

<details>
<summary>What frameworks are available for testing Node.js applications?</summary>

### What frameworks are available for testing Node.js applications?

**Definition:**

Node ships a test runner at `node:test`. You import `describe` and `it`, assert with `node:assert/strict`, and run `node --test`. Jest, Mocha, and Vitest are npm packages with their own runners. `node:test` must be loaded with the `node:` prefix.

**How it works:**

- `describe` groups tests. `it` (also exported as `test`) is one case. A thrown assertion fails the test. An async test fails if the returned promise rejects.
- `node --test` discovers files such as `*.test.js`. You can also pass a file path. The process exit code is non-zero when a test fails.
- `node:assert/strict` compares with `Object.is` and throws `AssertionError`. Prefer it over the loose `node:assert` equality.
- The runner can collect coverage with `node --test --experimental-test-coverage`. Reporters live under `node:test/reporters`, which also requires the `node:` prefix.
- Jest and Vitest add a fake DOM, snapshot files, and their own mock helpers. They are separate installs. Mocha is a runner you pair with an assertion library.
- Tests should talk to `127.0.0.1` and a port you bind yourself. A test that needs the network or a database should say so and clean up in `after`.

**When to use:**

- New services that can stay on core: `node:test`.
- A React UI in the same repo often already uses Vitest or Jest. One runner per package keeps the scripts obvious.

**Examples:**

```js
import { describe, it } from "node:test";
import assert from "node:assert/strict";

describe("sum", () => {
  it("adds two numbers", () => {
    assert.equal(1 + 2, 3);
  });
});
```

**Sources:**

- [Node.js — Test runner](https://nodejs.org/api/test.html)
- [Node.js — Assert](https://nodejs.org/api/assert.html)
</details>

<details>
<summary>Explain the concept of mocking in Node.js.</summary>

### Explain the concept of mocking in Node.js.

**Definition:**

A mock replaces a collaborator so the test checks your code without the real disk, network, or clock. `node:test` provides `mock.fn` and `mock.method` on the test context. `mock.module` exists and requires `node --experimental-test-module-mocks`. It is not the default way to stub a built-in.

**How it works:**

- `t.mock.fn(impl)` returns a function that records calls on `fn.mock.calls` and still runs `impl`.
- `t.mock.method(object, name)` wraps one method and restores it when the test ends. The spy keeps the original `this`.
- `mock.module(specifier, { exports })` replaces a module for later imports. The docs say imports that already happened keep the original module, and the process must be started with `--experimental-test-module-mocks`.
- A mock should stand in for a boundary you do not own (HTTP, the clock, a random source). Mocking the function under test hides the bug.
- Restore is part of the contract. The test context restores method mocks automatically. A hand-rolled assignment to `object.method` must put the original back in `finally`.
- Prefer injecting the dependency (`function load(readFile)`) when you can. A mock on a sealed core module is the harder path.

**When to use:**

- A unit test of branching logic that would otherwise hit the network or the filesystem.
- An HTTP test can bind a real `node:http` server on an ephemeral port and skip the mock.

**Examples:**

```js
import { test } from "node:test";
import assert from "node:assert/strict";

test("records a call", (t) => {
  const sum = t.mock.fn((a, b) => a + b);
  assert.equal(sum(1, 2), 3);
  assert.equal(sum.mock.calls.length, 1);
});
```

**Sources:**

- [Node.js — Test runner](https://nodejs.org/api/test.html#mocking)
</details>

<details>
<summary>Why is benchmarking important in Node.js?</summary>

### Why is benchmarking important in Node.js?

**Definition:**

A benchmark measures how long a piece of work takes and how much memory it uses, on a fixed input, before and after a change. Node’s event loop makes a “it feels slow” guess unreliable: one blocked tick stalls every connection, and a micro-optimization can lose to allocation.

**How it works:**

- `performance.now()` from `node:perf_hooks` returns milliseconds with a monotonic clock. Subtract two reads around the work. `process.hrtime.bigint()` gives nanoseconds.
- Run enough iterations that startup noise is a small fraction. Report the distribution, not a single sample. Garbage collection pauses land inside the measurement.
- `process.memoryUsage()` reports `heapUsed`, `rss`, and `external` in bytes. A faster function that retains a growing buffer is still a problem.
- `node --prof` writes a V8 tick log you process with `--prof-process`. The inspector CPU profile answers the same question interactively.
- Benchmark the real shape of the data. A tiny string hides the cost of a large JSON parse or a `readFile` that buffers a whole file.
- Compare on the same Node version and machine. A result from another major version does not transfer.

**When to use:**

- Before you rewrite a hot path, add a cache, or switch a sync call to async.
- Skip it for code that runs once at startup and is obviously cheap.

**Examples:**

```js
import { performance } from "node:perf_hooks";

const start = performance.now();
let n = 0;
for (let i = 0; i < 1_000_000; i += 1) n += i;
const elapsedMs = performance.now() - start;
console.log({ n, elapsedMs });
```

**Sources:**

- [Node.js — Performance hooks](https://nodejs.org/api/perf_hooks.html)
- [Node.js — Process](https://nodejs.org/api/process.html#processmemoryusage)
</details>

<details>
<summary>How do you test an HTTP server in Node.js?</summary>

### How do you test an HTTP server in Node.js?

**Definition:**

Start the server with `listen(0)` so the OS picks a free port, request it with `http.get` on `127.0.0.1`, assert the status and body, then `server.close()`. `node:test` runs that as an async `it`. No extra HTTP framework is required.

**How it works:**

- `listen(0, "127.0.0.1")` binds the loopback interface. `server.address().port` is the ephemeral port. Hard-coded `3000` collides with a dev server and with parallel tests.
- Drive the server through HTTP. Reaching into the handler function skips header parsing and the response stream.
- Collect the body from the response stream (`'data'` then `'end'`). Compare status with `res.statusCode`.
- `server.close()` stops accepting connections and calls its callback when existing ones finish. Close even when the assertion throws (`try`/`finally`).
- A handler that reads a body should be tested with `http.request` and a written payload, including a too-large body if you enforce a limit.
- Keep the test process single-purpose. `node --test` can run files concurrently. Binding only `127.0.0.1` on port `0` avoids cross-talk.

**When to use:**

- Every route you ship: at least one status-and-body test.
- A full browser flow belongs in a separate end-to-end tool. This test covers the server.

**Examples:**

```js
import { it } from "node:test";
import assert from "node:assert/strict";
import http from "node:http";

it("GET / returns ok", async () => {
  const server = http.createServer((_req, res) => {
    res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
    res.end("ok");
  });

  await new Promise((resolve) => {
    server.listen(0, "127.0.0.1", resolve);
  });

  try {
    const { port } = server.address();
    const { statusCode, body } = await new Promise((resolve, reject) => {
      http
        .get({ hostname: "127.0.0.1", port, path: "/" }, (res) => {
          const chunks = [];
          res.on("data", (chunk) => chunks.push(chunk));
          res.on("end", () => {
            resolve({
              statusCode: res.statusCode,
              body: Buffer.concat(chunks).toString("utf8"),
            });
          });
        })
        .on("error", reject);
    });
    assert.equal(statusCode, 200);
    assert.equal(body, "ok");
  } finally {
    await new Promise((resolve, reject) => {
      server.close((err) => (err ? reject(err) : resolve()));
    });
  }
});
```

**Sources:**

- [Node.js — Test runner](https://nodejs.org/api/test.html)
- [Node.js — HTTP](https://nodejs.org/api/http.html)
</details>

### Node.js with Databases

<details>
<summary>How do you connect a MySQL database with Node.js?</summary>

### How do you connect a MySQL database with Node.js?

**Definition:**

Node has no MySQL client in core. You install a driver such as `mysql2` and open a pool with the host, user, database, and password. Query through the pool’s promise API and keep the SQL in your code, with placeholders for values.

**How it works:**

- `mysql2/promise` exports `createPool`. A pool reuses TCP connections. `createConnection` is one connection, which is a poor fit for a server that handles overlapping requests.
- `pool.execute(sql, values)` sends a prepared statement. Placeholders (`?`) keep user input out of the SQL string. The promise resolves to `[rows, fields]`.
- Connection settings come from `process.env` (or `process.loadEnvFile`). A password in source control is a leaked credential.
- The pool is a long-lived object for the process. Call `pool.end()` on shutdown so the process can exit.
- A slow query blocks only that connection from the pool, not the JavaScript thread, as long as you await the promise and do not use the driver’s sync helpers.
- Transactions are `getConnection()`, then `beginTransaction()`, `commit()`, and `release()` in `finally`. Leaving a connection checked out shrinks the pool.

**When to use:**

- Relational data with joins, constraints, and transactions.
- An ORM or query builder can sit on the same driver. The connection is still this pool.

**Examples:**

```js
import mysql from "mysql2/promise";

const pool = mysql.createPool({
  host: process.env.MYSQL_HOST,
  user: process.env.MYSQL_USER,
  password: process.env.MYSQL_PASSWORD,
  database: process.env.MYSQL_DATABASE,
});

const [rows] = await pool.execute(
  "SELECT id, title FROM notes WHERE id = ?",
  [1],
);
console.log(rows);
```

**Sources:**

- [Node.js — Process](https://nodejs.org/api/process.html#processenv)
- [mysql2 — Promise wrappers](https://sidorares.github.io/node-mysql2/docs/documentation/promise-wrapper)
</details>

<details>
<summary>Explain how NoSQL databases like MongoDB can be used with Node.js.</summary>

### Explain how NoSQL databases like MongoDB can be used with Node.js.

**Definition:**

MongoDB is reached through the official `mongodb` driver, which is an npm package. `MongoClient` opens a connection, `db()` selects a database, and `collection()` selects a collection of documents. Documents are JavaScript objects. Node does not embed MongoDB.

**How it works:**

- `new MongoClient(uri)` and `await client.connect()` open the pool inside the driver. The URI belongs in `process.env`.
- `insertOne`, `find`, and `updateOne` take document objects. `find` returns a cursor. `toArray()` loads the matches into memory, so a large result should be read with `for await`.
- There is no fixed table. The application still needs a convention for fields, or a validator on the collection. The driver stores the object you pass.
- One `MongoClient` per process is enough. Creating a client per request exhausts connections.
- `client.close()` on shutdown releases sockets. A server that skips this can hang on exit because the pool keeps the event loop alive.
- Joins are aggregation (`$lookup`), not SQL. Model the documents around the reads you actually do.

**When to use:**

- Document-shaped data whose fields vary, or a team already operating MongoDB.
- Strict relational integrity still wants a SQL database and its constraints.

**Examples:**

```js
import { MongoClient } from "mongodb";

const client = new MongoClient(process.env.MONGODB_URI);
await client.connect();

const notes = client.db("app").collection("notes");
await notes.insertOne({ title: "hello" });

const found = await notes.find({ title: "hello" }).toArray();
console.log(found);
```

**Sources:**

- [Node.js — Process](https://nodejs.org/api/process.html#processenv)
- [MongoDB Node.js driver — Connect](https://www.mongodb.com/docs/drivers/node/current/connect/)
</details>

<details>
<summary>What's the role of ORM in Node.js?</summary>

### What's the role of ORM in Node.js?

**Definition:**

An ORM (object-relational mapper) is an npm library that maps JavaScript objects to rows in a SQL database and generates queries. Node has no ORM in core. Prisma, Drizzle, Sequelize, and TypeORM are packages teams add on top of a database driver.

**How it works:**

- You describe tables as models or a schema. The library turns method calls into SQL and turns result rows back into objects.
- Migrations are versioned changes to that schema. They live in the repo and run as a step before the new process starts.
- A query builder (Knex, or the SQL side of Drizzle) stays closer to SQL. A heavier ORM also tracks identity and relations in memory.
- The generated SQL can hide an N+1 pattern: one query for the parent and one query per child. Inspect the SQL when a page gets slow.
- Transactions still matter. The ORM exposes them so several writes commit or roll back together.
- The connection underneath is still a driver pool (`mysql2`, `pg`, and similar). The ORM does not open a proprietary socket of its own.

**When to use:**

- Application code that mostly creates, reads, and updates rows and wants one place for the schema.
- Reporting queries and database-specific features are clearer as SQL, through the driver or a thin query builder.

**Examples:**

```js
import { PrismaClient } from "@prisma/client";

const prisma = new PrismaClient();
const notes = await prisma.note.findMany({ take: 20 });
console.log(notes);
```

**Sources:**

- [Node.js — Process](https://nodejs.org/api/process.html)
- [Prisma — CRUD](https://www.prisma.io/docs/orm/prisma-client/queries/crud)
</details>

### Node.js Performance

<details>
<summary>How can you monitor the performance of a Node.js app?</summary>

### How can you monitor the performance of a Node.js app?

**Definition:**

Watch the event-loop delay, CPU time, and memory of the process, plus the status codes and duration of HTTP requests. `node:perf_hooks` and `process.memoryUsage()` / `process.cpuUsage()` are the core measurements. A production system ships those numbers to logs or a metrics backend.

**How it works:**

- `monitorEventLoopDelay()` builds a histogram of how late the loop is. `enable()` starts it. Values are nanoseconds. A rising mean means JavaScript is busy or the loop is starved.
- `process.memoryUsage()` returns `rss`, `heapTotal`, `heapUsed`, `external`, and `arrayBuffers` in bytes. Sample it on an interval and look at the trend.
- `process.cpuUsage(previous)` returns the user and system microseconds since `previous`. Pair it with wall time to see if the process is actually on CPU.
- Time a request with `performance.now()` at the start of the `request` listener and again in `res.on("finish")`. Log method, path, status, and milliseconds.
- `node --inspect` and a CPU profile explain a spike you can reproduce. `--prof` does the same from the command line.
- Metrics are per process. A `cluster` primary has one line per worker. Aggregate outside the process.

**When to use:**

- Always in production: latency, error rate, event-loop delay, memory.
- A one-off script can print `performance.now()` around the work and stop there.

**Examples:**

```js
import { monitorEventLoopDelay, performance } from "node:perf_hooks";
import process from "node:process";

const delay = monitorEventLoopDelay({ resolution: 20 });
delay.enable();

const startedAt = performance.now();
const cpuStart = process.cpuUsage();

setInterval(() => {
  console.log({
    elapsedMs: performance.now() - startedAt,
    cpu: process.cpuUsage(cpuStart),
    heapUsed: process.memoryUsage().heapUsed,
    loopDelayMs: delay.mean / 1e6,
  });
}, 10_000).unref();
```

**Sources:**

- [Node.js — Performance hooks](https://nodejs.org/api/perf_hooks.html#monitoreventloopdelayoptions)
- [Node.js — Process](https://nodejs.org/api/process.html#processmemoryusage)
</details>

<details>
<summary>What is clustering in Node.js and how does it work?</summary>

### What is clustering in Node.js and how does it work?

**Definition:**

The `node:cluster` module runs several Node processes that share one server port. The primary calls `cluster.fork()`. Each worker is a full process with its own V8 isolate and event loop. The operating system distributes accepted connections across the workers.

**How it works:**

- `cluster.isPrimary` is true in the process you started. `cluster.isWorker` is true in a forked worker. `cluster.worker.id` exists only in a worker.
- `cluster.fork()` spawns a worker. Call it once per core you want to use (`availableParallelism()` from `node:os`). Only the primary may fork.
- Workers run the same file again. Put the HTTP `listen` in the worker branch. The port is shared. You do not listen once in the primary and proxy by hand.
- Memory and JavaScript state are not shared. A `Map` in one worker is invisible to the others. Sessions belong in a store outside the process.
- If a worker exits, the primary can `fork()` a replacement on the `'exit'` event. The primary itself should stay small.
- `cluster` is process parallelism. It does not move a CPU-heavy function onto another thread inside one isolate. That is `worker_threads`.

**When to use:**

- A multi-core HTTP server on one machine, when you want the core module rather than a process manager’s cluster mode.
- More than one machine is a load balancer in front of several hosts, which is outside `node:cluster`.

**Examples:**

```js
import cluster from "node:cluster";
import http from "node:http";
import { availableParallelism } from "node:os";

if (cluster.isPrimary) {
  const n = availableParallelism();
  for (let i = 0; i < n; i += 1) cluster.fork();
} else {
  http
    .createServer((_req, res) => {
      res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
      res.end("ok");
    })
    .listen(8000);
}
```

**Sources:**

- [Node.js — Cluster](https://nodejs.org/api/cluster.html)
- [Node.js — OS](https://nodejs.org/api/os.html#osavailableparallelism)
</details>

<details>
<summary>How can you prevent memory leaks in a Node.js application?</summary>

### How can you prevent memory leaks in a Node.js application?

**Definition:**

A leak is memory the process keeps after the work that needed it is finished. Drop references when a request, timer, or listener ends, and watch `process.memoryUsage().heapUsed`. A steady climb that never falls after garbage collection is the signal.

**How it works:**

- An unbounded `Map` or array used as a cache grows with traffic. Cap it, or give entries a lifetime and delete them.
- `emitter.on` without a matching `removeListener` retains the listener and anything it closes over. Use `once` for a one-shot event.
- `setInterval` that you never `clearInterval` keeps its callback. `unref()` on a timer lets the process exit. It does not free a closure that still holds a large buffer.
- A request stream you never consume, or a socket you never destroy, stays in the handle table. `pipeline` and `res.on("close")` are the usual cleanup points.
- Closures that capture a big `Buffer` keep that buffer alive for as long as the closure is reachable. Copy out the small piece you need.
- Confirm with a heap snapshot under `node --inspect`. Compare two snapshots after the same load. `heapUsed` from `memoryUsage` tells you the trend before you open the snapshot.

**When to use:**

- Any long-lived server. Scripts that exit reclaim everything when the process ends.
- Fix the retainer the snapshot names. Adding RAM only delays the next restart.

**Examples:**

```js
import { EventEmitter } from "node:events";
import process from "node:process";

const bus = new EventEmitter();

function onPing(id) {
  console.log(id);
}

bus.on("ping", onPing);
bus.emit("ping", 1);
bus.removeListener("ping", onPing);

console.log(process.memoryUsage().heapUsed);
```

**Sources:**

- [Node.js — Process](https://nodejs.org/api/process.html#processmemoryusage)
- [Node.js — Events](https://nodejs.org/api/events.html)
- [Node.js — Debugging](https://nodejs.org/en/learn/getting-started/debugging)
</details>

<details>
<summary>Explain the use of the --inspect flag in Node.js.</summary>

### Explain the use of the --inspect flag in Node.js.

**Definition:**

`node --inspect file.js` turns on the V8 inspector and listens on `127.0.0.1:9229`. DevTools connects to the printed `ws://` URL and can set breakpoints, step, and take CPU and heap profiles. `--inspect-brk` pauses before the script runs.

**How it works:**

- The default host is loopback and the default port is `9229`. `--inspect=127.0.0.1:9230` changes the port. Binding a public address exposes a debugger that can evaluate code in the process.
- A `debugger` statement breaks only while a client is attached.
- The same session records a CPU profile and a heap snapshot. That is how you follow up a high event-loop delay or a climbing `heapUsed`.
- `--inspect` does not change your HTTP server. It adds a second listener for the inspector protocol.
- A process manager must pass the flag through to the Node binary. A cluster worker is a separate process and needs its own inspect port.
- `--enable-source-maps` lines the paused frame up with TypeScript when a source map is present.

**When to use:**

- Local debugging and a reproduced production bug on a machine you trust.
- Leave it off on a public server. If you need it there, reach it through an SSH tunnel to `127.0.0.1`.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((_req, res) => {
  debugger;
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end("ok");
});

server.listen(8000);
```

```console
node --inspect-brk server.js
```

**Sources:**

- [Node.js — Debugging](https://nodejs.org/en/learn/getting-started/debugging)
- [Node.js — Debugger](https://nodejs.org/api/debugger.html)
</details>

### Concurrency in Node.js

<details>
<summary>How does Node.js handle concurrency?</summary>

### How does Node.js handle concurrency?

**Definition:**

JavaScript in one process runs on one call stack. Concurrency comes from the event loop interleaving I/O callbacks, from libuv’s thread pool for some blocking system calls, and from extra processes or worker threads when you ask for them.

**How it works:**

- Network sockets stay on the loop thread. A waiting request does not occupy a JavaScript thread. The `'request'` listener runs when the kernel says the connection is readable or writable.
- File I/O, `dns.lookup`, and some crypto run on the libuv pool (`UV_THREADPOOL_SIZE`, default 4). That pool is not one thread per request.
- `worker_threads` starts another V8 isolate in the same process for CPU work. It talks to the main thread with `postMessage`.
- `node:cluster` and `child_process.fork` start more processes. Each has its own memory and its own loop. They share a port only through `cluster` or an external load balancer.
- A tight `for` loop on the main thread stops the loop until it returns. Other connections wait.
- Async functions concurrency is “many operations in flight”, not parallel JavaScript. `Promise.all` overlaps the waits. The callbacks still run one at a time.

**When to use:**

- I/O-heavy servers stay on the event loop.
- CPU work goes to `worker_threads` or another process. More cores for HTTP go to `cluster` or several processes behind a proxy.

**Examples:**

```js
import { readFile } from "node:fs/promises";

const dir = new URL("./", import.meta.url);
const [a, b] = await Promise.all([
  readFile(new URL("a.txt", dir), "utf8"),
  readFile(new URL("b.txt", dir), "utf8"),
]);
console.log(a.length, b.length);
```

**Sources:**

- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
- [Node.js — Worker threads](https://nodejs.org/api/worker_threads.html)
- [Node.js — Cluster](https://nodejs.org/api/cluster.html)
</details>

<details>
<summary>What is the difference between process and child_process modules?</summary>

### What is the difference between process and child_process modules?

**Definition:**

`node:process` is the current Node process: environment, pid, memory, signals, and `nextTick`. `node:child_process` starts other programs. `spawn` is the base function. `exec` and `execFile` buffer output. `fork` starts another Node module with an IPC channel.

**How it works:**

- `process.env`, `process.pid`, `process.exitCode`, and `process.memoryUsage()` describe this process. `process` is an `EventEmitter` (`'exit'`, `'uncaughtException'`, signals).
- `spawn(command, args)` creates a subprocess and gives you streams for stdin, stdout, and stderr. If you never read a full pipe, the child can block on that pipe.
- `exec` runs a shell and buffers stdout and stderr. The docs give it a `maxBuffer` because the whole output sits in memory. `execFile` runs a file without a shell.
- `fork(modulePath)` is a special `spawn` of the Node binary. Parent and child can `process.send` and listen for `'message'`.
- A child does not share your JavaScript heap. Data crosses as bytes or as serialized messages.
- The parent should handle `'error'` and `'exit'` and kill or `unref` children it no longer needs, or the parent can stay alive on their pipes.

**When to use:**

- `process` for this process’s env, shutdown, and signals.
- `spawn` for a CLI tool whose output you stream. `fork` when the other side is Node code that should message you. `exec` for a short command with a small output.

**Examples:**

```js
import { spawn } from "node:child_process";
import process from "node:process";

console.log(process.pid);

const child = spawn(process.execPath, ["-e", "console.log('child')"]);
child.stdout.on("data", (chunk) => {
  console.log(chunk.toString());
});
```

**Sources:**

- [Node.js — Process](https://nodejs.org/api/process.html)
- [Node.js — Child process](https://nodejs.org/api/child_process.html)
</details>

<details>
<summary>How do worker threads work in Node.js?</summary>

### How do worker threads work in Node.js?

**Definition:**

`node:worker_threads` runs JavaScript on another thread with its own V8 isolate. The main thread constructs a `Worker` from a file URL. The worker reads `parentPort` and both sides call `postMessage`. This is how you move CPU work off the event loop without starting a second process.

**How it works:**

- `isMainThread` is true in the process’s original thread. The same file can branch: the main thread creates `new Worker(new URL(import.meta.url))`, and the worker runs the `else` branch.
- Messages are copied by default (structured clone). `transferList` moves an `ArrayBuffer` so both sides do not keep it. `SharedArrayBuffer` is shared memory and needs its own synchronization.
- `parentPort` is `null` on the main thread. In the worker it is the channel. `'message'` delivers what the other side posted.
- A worker has its own event loop and its own `globalThis`. It can use `node:fs` and `node:http`. Those calls still cost what they cost. The win is parallel JavaScript.
- Listen for `'error'` and `'exit'` on the `Worker`. An unhandled error in the worker emits `'error'` on the `Worker` object in the parent.
- Workers are heavier than a callback. Create a small pool and reuse it. Spawning one worker per request spends the time you hoped to save.

**When to use:**

- Compression, image work, parsing a huge payload, or any tight loop that would stall HTTP.
- I/O concurrency stays on the main event loop. A worker does not make database waits faster by itself.

**Examples:**

```js
import { Worker, isMainThread, parentPort } from "node:worker_threads";

if (isMainThread) {
  const worker = new Worker(new URL(import.meta.url));
  worker.once("message", (message) => {
    console.log(message);
  });
  worker.postMessage("Hello, world!");
} else {
  parentPort.once("message", (message) => {
    parentPort.postMessage(message);
  });
}
```

**Sources:**

- [Node.js — Worker threads](https://nodejs.org/api/worker_threads.html)
</details>

### Node.js and Microservices

<details>
<summary>How is Node.js used in microservices architecture?</summary>

### How is Node.js used in microservices architecture?

**Definition:**

A Node microservice is one process (or a small cluster of processes) that owns one job and exposes it over the network, usually HTTP or a queue. Node fits because a service spends most of its time waiting on other services, and the event loop keeps many of those waits in flight.

**How it works:**

- Each service has its own `package.json`, its own deploy, and its own data store. Another service reaches it through HTTP (`node:http` or `fetch`) or by publishing a message. It does not `require` the other service’s files.
- `http.createServer` is enough for the edge of a small service. A framework only adds routing.
- State stays outside the process. A `Map` on the server disappears when you run a second copy, and `cluster` workers do not share it either.
- A slow or crashed dependency must have a timeout and a failure path. `AbortSignal.timeout` on `fetch` stops a hung call. One stuck request must not stall the loop.
- CPU-heavy work inside the service goes to `worker_threads` or a separate worker service. The HTTP process stays thin.
- Observability is per service: log the request id you received, and pass it onward. `process.memoryUsage` and event-loop delay are the local health signals.

**When to use:**

- A boundary you can name, deploy, and scale on its own (auth, billing, image resize).
- A single deployable Node app is the simpler start when the team and the data are still one thing.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((req, res) => {
  if (req.method === "GET" && req.url === "/health") {
    res.writeHead(200, { "Content-Type": "application/json; charset=utf-8" });
    res.end(JSON.stringify({ ok: true }));
    return;
  }
  res.writeHead(404).end();
});

server.listen(process.env.PORT ?? 8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Process](https://nodejs.org/api/process.html#processenv)
</details>

<details>
<summary>Explain inter-process communication in a Node.js microservice architecture.</summary>

### Explain inter-process communication in a Node.js microservice architecture.

**Definition:**

Services on different machines talk over the network: HTTP (`fetch` or `node:http`) or a message broker. Processes on the same machine can also use `child_process.fork`, which gives both sides `process.send` and a `'message'` event. Those are different channels. A microservice deploy usually picks the network, because the other process may not be on this host.

**How it works:**

- `fork` opens an IPC channel. `subprocess.send(message)` in the parent and `process.send(message)` in the child deliver a structured clone. `'message'` is the event. This is the same mechanism `node:cluster` uses between primary and worker.
- HTTP is request and response. Send a JSON body, set a timeout with `AbortSignal`, and treat a non-2xx status as a failure the caller handles.
- A queue decouples the sender from the receiver. The sender can exit. The receiver works at its own pace. The broker (not a core module) stores the message.
- Messages should be versioned and small. A shared database row is not a message bus. Both services writing the same tables couple their deploys.
- IPC handles keep the parent alive. Close the channel or `kill` the child during shutdown.
- Do not send secrets in a log line that records the message. `process.env` on one service is not visible to another.

**When to use:**

- `fork` for a helper process that lives and dies with this Node process.
- HTTP or a queue between deployable services.

**Examples:**

```js
import { fork } from "node:child_process";

const child = fork(new URL("./worker.js", import.meta.url));

child.on("message", (message) => {
  console.log(message);
});

child.send({ type: "ping" });
```

**Sources:**

- [Node.js — Child process](https://nodejs.org/api/child_process.html#child_processforkmodulepath-args-options)
- [Node.js — HTTP](https://nodejs.org/api/http.html)
</details>

### Security in Node.js

<details>
<summary>What are some common security best practices for Node.js applications?</summary>

### What are some common security best practices for Node.js applications?

**Definition:**

Keep the runtime and dependencies current, refuse to run untrusted strings as code, cap request bodies, and keep secrets in the environment. Node’s permission model (`node --permission`) can deny filesystem and network access the process never asked for. A public server belongs behind TLS, usually terminated by a reverse proxy.

**How it works:**

- `eval` and `new Function(userInput)` run whatever string they are given. Treat request data as data. Parse JSON with `JSON.parse`.
- Set `NODE_TLS_REJECT_UNAUTHORIZED` only if you accept disabling certificate checks. The TLS docs warn that `'0'` turns verification off.
- Read the body with a byte limit and then stop. An unbounded `for await` on `req` is a memory DoS.
- `node --permission --allow-fs-read=./data --allow-net` makes extra filesystem and network calls fail. It is a runtime limit, not a review of your logic.
- Run the process as a user that is not root. The inspector (`--inspect`) stays on `127.0.0.1`.
- Pin versions with the lockfile and review new packages. A dependency runs with the same rights as your process.
- Send errors to the client as a status and a short message. Stack traces stay in the log.

**When to use:**

- Every networked process, including internal ones. The practices are the same shape at any size.
- Add the permission flags once the file and network surface of the app is known.

**Examples:**

```js
import http from "node:http";

const limit = 1_000_000;

const server = http.createServer(async (req, res) => {
  const chunks = [];
  let size = 0;
  for await (const chunk of req) {
    size += chunk.length;
    if (size > limit) {
      res.writeHead(413).end();
      return;
    }
    chunks.push(chunk);
  }
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end(String(size));
});

server.listen(8000);
```

**Sources:**

- [Node.js — Security best practices](https://nodejs.org/en/learn/getting-started/security-best-practices)
- [Node.js — Permissions](https://nodejs.org/api/permissions.html)
- [Node.js — TLS](https://nodejs.org/api/tls.html)
</details>

<details>
<summary>How would you protect your Node.js application from XSS attacks?</summary>

### How would you protect your Node.js application from XSS attacks?

**Definition:**

Cross-site scripting happens when a browser executes a string that came from a user. On a Node server that means you must not splice raw input into HTML, and a JSON API must be served as JSON so the browser does not treat it as a page. `Content-Security-Policy` is the header that limits which scripts may run.

**How it works:**

- Encode text before it lands in HTML. A JSON API that sets `Content-Type: application/json; charset=utf-8` is not an HTML document. Add `X-Content-Type-Options: nosniff` so the browser keeps that type.
- A template that does `res.end("<p>" + req.url + "</p>")` puts the URL into HTML. Encode `&`, `<`, `>`, quotes, or use a template engine that escapes by default.
- `Content-Security-Policy` with a tight `script-src` stops an injected inline script even if a slip gets through. Set it with `res.setHeader`.
- Cookies for a browser session want `HttpOnly` so page script cannot read them, plus `Secure` on HTTPS. That limits the damage of an XSS. It does not remove the bug.
- Rich text that you intentionally store as HTML needs a sanitizer package. Node does not ship an HTML sanitizer in core.
- Reflecting `req.headers` into the page has the same shape as reflecting the body. Both are user-controlled.

**When to use:**

- Any response a browser might parse as HTML or load in a page.
- A pure JSON API still sets the JSON content type and `nosniff`, so a browser will not sniff it into HTML.

**Examples:**

```js
import http from "node:http";

function escapeHtml(value) {
  return value
    .replaceAll("&", "&amp;")
    .replaceAll("<", "&lt;")
    .replaceAll(">", "&gt;")
    .replaceAll('"', "&quot;");
}

const server = http.createServer((req, res) => {
  const safe = escapeHtml(req.url ?? "/");
  const body = `<!doctype html><p>${safe}</p>`;
  res.writeHead(200, {
    "Content-Type": "text/html; charset=utf-8",
    "Content-Security-Policy": "default-src 'self'",
    "X-Content-Type-Options": "nosniff",
  });
  res.end(body);
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Security best practices](https://nodejs.org/en/learn/getting-started/security-best-practices)
- [MDN — Content-Security-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Security-Policy)
</details>

<details>
<summary>What are environment variables and how could you use them in Node.js?</summary>

### What are environment variables and how could you use them in Node.js?

**Definition:**

Environment variables are strings the operating system passes into the process. Node exposes them as `process.env`. Read ports, hosts, and secrets from there so the same build can run in more than one place. `process.loadEnvFile()` and `node --env-file` load a `.env` file into `process.env` at startup.

**How it works:**

- Every value in `process.env` is a string. `process.env.PORT ?? "8000"` still needs `Number()` if you want a number. A missing key is `undefined`.
- Assigning `process.env.FOO = "bar"` sets it for this process and for children that inherit the environment. It does not edit your shell for the next launch.
- `loadEnvFile()` from `node:process` reads a `.env` file (default `.env` in the current working directory) and does not override variables that are already set. `NODE_OPTIONS` inside that file has no effect.
- `node --env-file=.env server.js` does the load before your script runs. That is the built-in path. The `dotenv` package is an npm tool that fills `process.env` the same way.
- Put `.env` in `.gitignore`. Commit an example file with empty values. The real file stays on the machine or in the host’s secret store.
- Logging `process.env` prints secrets. Log the key names you care about, not the object.

**When to use:**

- Configuration that changes between a laptop, CI, and production.
- Feature flags and pool sizes belong here too. Code paths do not.

**Examples:**

```js
import { loadEnvFile } from "node:process";
import http from "node:http";

loadEnvFile();

const port = Number(process.env.PORT ?? 8000);

http
  .createServer((_req, res) => {
    res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
    res.end("ok");
  })
  .listen(port);
```

**Sources:**

- [Node.js — process.env](https://nodejs.org/api/process.html#processenv)
- [Node.js — process.loadEnvFile](https://nodejs.org/api/process.html#processloadenvfilepath)
</details>

### Node.js and WebSockets

<details>
<summary>What are WebSockets and how do they work with Node.js?</summary>

### What are WebSockets and how do they work with Node.js?

**Definition:**

A WebSocket is a long-lived, two-way channel on top of one TCP connection. It starts as an HTTP request with `Upgrade: websocket` and, after a `101` response, both sides can send frames at any time. Node’s global `WebSocket` class is a client, the same shape as the browser API, stable since Node 22.4. Core does not include a WebSocket server.

**How it works:**

- The client opens `new WebSocket(url)`. `'open'` means the handshake finished. `send` writes a frame. `'message'` delivers one. `close` ends the socket.
- The HTTP server sees the handshake as an `'upgrade'` event on `node:http`: the request, the raw socket, and the first buffered bytes. That event is the hook. It does not parse or generate WebSocket frames for you.
- A server library (`ws` is the usual npm package) accepts the upgrade, speaks RFC 6455, and gives you a socket with `message` and `send`.
- One connection stays open, so the process holds a socket per client. The event loop still runs other work between frames. A tight loop in a `'message'` handler blocks every connection on that process.
- Frames are messages, not an HTTP body. You design your own payload, often a small JSON text.
- If the client only needs server-to-client updates, Server-Sent Events (`text/event-stream` on a normal response) is a one-way option that stays on `node:http`.

**When to use:**

- Chat, live cursors, and other flows where both sides send without a new HTTP request.
- Ordinary reads and writes stay request/response.

**Examples:**

```js
const socket = new WebSocket("wss://echo.websocket.org");

socket.addEventListener("open", () => {
  socket.send("hello");
});

socket.addEventListener("message", (event) => {
  console.log(event.data);
  socket.close();
});
```

**Sources:**

- [Node.js — Global WebSocket](https://nodejs.org/api/globals.html#class-websocket)
- [Node.js — HTTP upgrade](https://nodejs.org/api/http.html#event-upgrade)
- [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
</details>

<details>
<summary>How do you set up a WebSocket server in Node.js?</summary>

### How do you set up a WebSocket server in Node.js?

**Definition:**

You install a WebSocket server library. Node has no built-in WebSocket server. The `ws` package attaches to an `node:http` server, completes the upgrade handshake, and emits a connection. The global `WebSocket` class only opens a client connection.

**How it works:**

- Create the HTTP server with `http.createServer`, then `new WebSocketServer({ server })` from `ws`. The library listens for `'upgrade'` and answers `101` with the protocol headers.
- `'connection'` gives you a socket per client. `socket.on("message", cb)` reads a frame. `socket.send(data)` writes one. `socket.close()` ends that client.
- The HTTP server can still answer normal requests. WebSocket clients hit the upgrade path. Everyone else hits the `request` listener.
- Broadcast is a loop over `wss.clients`. Each `send` is per socket. Backpressure is the socket’s buffered amount. A slow client should be skipped or disconnected.
- Horizontal scale does not share sockets across processes. A second Node process needs a broker or another fan-out if every client must see every message.
- Put the server behind TLS in production (`wss:`). The browser will refuse an insecure WebSocket from an HTTPS page.

**When to use:**

- You are the server and browsers (or other clients) need a two-way channel.
- Calling someone else’s endpoint is the global `WebSocket` client, not this server.

**Examples:**

```js
import http from "node:http";
import { WebSocketServer } from "ws";

const server = http.createServer((_req, res) => {
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end("ok");
});

const wss = new WebSocketServer({ server });

wss.on("connection", (socket) => {
  socket.on("message", (data) => {
    socket.send(data);
  });
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Global WebSocket](https://nodejs.org/api/globals.html#class-websocket)
- [ws — README](https://github.com/websockets/ws#simple-server)
</details>

### Node.js Deployment

<details>
<summary>How do you deploy a Node.js application in production?</summary>

### How do you deploy a Node.js application in production?

**Definition:**

Ship the same source you tested, install production dependencies from the lockfile, set configuration in the environment, and run `node` under a process manager behind a reverse proxy. The process listens on `process.env.PORT`. The proxy terminates TLS and forwards to that port.

**How it works:**

- `npm ci --omit=dev` installs the lockfile and skips `devDependencies`. The Node version on the host should match the version you tested (the `engines` field records that).
- The app reads `PORT`, database URLs, and secrets from `process.env` or `--env-file`. Nothing secret is baked into the image or the git tree.
- A process manager restarts the process when it exits and can run one process per core. `node:cluster` can do the same inside one launch. You pick one of those, not both stacked blindly.
- The proxy (nginx, Caddy, a cloud load balancer) is the public address. Node binds `127.0.0.1` or a private interface. `--inspect` stays off.
- Logs go to stdout and stderr. The platform collects them. A log file inside the process needs rotation you would otherwise forget.
- Graceful shutdown: on `SIGTERM`, stop accepting (`server.close()`), finish in-flight requests, then exit. Orchestrators send that signal before they kill the process.

**When to use:**

- Any service other people call. A laptop `node server.js` is the development loop.
- Containers and serverless are different packagings of this same process. The listen-and-env shape stays.

**Examples:**

```js
import http from "node:http";
import process from "node:process";

const server = http.createServer((_req, res) => {
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end("ok");
});

const port = Number(process.env.PORT ?? 8000);
server.listen(port);

process.on("SIGTERM", () => {
  server.close(() => process.exit(0));
});
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Process](https://nodejs.org/api/process.html)
- [Node.js — Dockerizing a Node.js web app](https://nodejs.org/en/docs/guides/nodejs-docker-webapp)
</details>

<details>
<summary>What is PM2 and how is it used in Node.js?</summary>

### What is PM2 and how is it used in Node.js?

**Definition:**

PM2 is an npm process manager, not a core module. It starts your `node` program, restarts it after a crash, and can fork one process per CPU in cluster mode. You install it beside the app and point it at the script you already run with `node`.

**How it works:**

- `pm2 start server.js` daemonizes that script. `pm2 logs` reads stdout and stderr. `pm2 restart` and `pm2 stop` target the process by name or id.
- `pm2 start server.js -i max` runs cluster mode: several workers sharing the listen port, the same idea as `node:cluster`. The workers are separate processes.
- A crash exits the worker. PM2 starts a new one. Your code still has to stop leaking memory. A restart loop hides a bug until the machine is busy doing restarts.
- `pm2 startup` plus `pm2 save` registers the process list so a reboot brings the apps back. That is host configuration, not something inside `server.js`.
- Environment comes from the shell or an ecosystem file PM2 reads. `process.env` inside the app is unchanged.
- PM2 is one option. systemd, a container orchestrator, or `node:cluster` under a simple supervisor cover the same jobs. Run one supervisor.

**When to use:**

- A virtual machine or a bare host where you want restarts and a cluster without writing the fork loop.
- Kubernetes or another orchestrator already restarts containers. Adding PM2 inside the container duplicates that.

**Examples:**

```js
import http from "node:http";

http
  .createServer((_req, res) => {
    res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
    res.end("ok");
  })
  .listen(process.env.PORT ?? 8000);
```

```console
pm2 start server.js -i max
```

**Sources:**

- [Node.js — Cluster](https://nodejs.org/api/cluster.html)
- [Node.js — Process](https://nodejs.org/api/process.html)
- [PM2 — Quick start](https://pm2.keymetrics.io/docs/usage/quick-start/)
</details>

<details>
<summary>Explain how you would use Docker with a Node.js application.</summary>

### Explain how you would use Docker with a Node.js application.

**Definition:**

A Dockerfile packages the Node binary, the lockfile install, and your source into an image. The official Node guide starts `FROM node`, copies `package.json` and the lockfile, runs `npm ci`, copies the rest, and sets `CMD` to `node` plus your server file. The image listens on the port you `EXPOSE`. You inject configuration at run time.

**How it works:**

- Copy the manifest before the rest of the source so a code-only change can reuse the install layer.
- `npm ci --omit=dev` in the image matches production. Dev tools stay out of the runtime image. A multi-stage build can compile TypeScript in one stage and copy `dist` into a smaller stage.
- Do not copy `node_modules` from your laptop. `.dockerignore` lists it, plus `.env` and `.git`. The image runs `npm ci` itself.
- `CMD ["node", "server.js"]` is the process. PID 1 should be Node (or a tiny init). The app handles `SIGTERM` and closes the server.
- `docker run -e PORT=8000 -p 8000:8000` publishes the port and sets `process.env.PORT`. Secrets are run-time env, not `ENV` lines baked into the image.
- The same image moves from CI to production. You do not rebuild with different source on the server.

**When to use:**

- You want the same filesystem and Node version on every machine.
- A platform that already runs the Node binary (some PaaS builds) does not need you to maintain a Dockerfile.

**Examples:**

```dockerfile
FROM node:22-bookworm-slim
WORKDIR /usr/src/app
COPY package.json package-lock.json ./
RUN npm ci --omit=dev
COPY . .
EXPOSE 8000
USER node
CMD ["node", "server.js"]
```

**Sources:**

- [Node.js — Dockerizing a Node.js web app](https://nodejs.org/en/docs/guides/nodejs-docker-webapp)
- [Node.js — Process](https://nodejs.org/api/process.html#processenv)
</details>

### Node.js and Version Control

<details>
<summary>How do you manage versioning of a Node.js API?</summary>

### How do you manage versioning of a Node.js API?

**Definition:**

Put the API version in the URL path (`/v1/notes`) and keep that handler stable. A breaking change ships as `/v2` while `/v1` still answers. Clients send the version by calling the path. The server branches on `url.pathname` in `node:http`, or on a router prefix.

**How it works:**

- A breaking change is one that makes an existing client wrong: a removed field, a new required field, a changed meaning. Adding an optional JSON field can stay on `/v1`.
- Both versions can live in one process. Share the parts that did not change. Give each version its own handler where the contract differs.
- Announce a sunset with a response header such as `Deprecation` and a date, then remove the old path in a later deploy. Do not change `/v1` in place.
- Query-string versions and custom headers exist. A path is the one you can see in logs and in a `curl` command.
- Package the server itself with semver in `package.json`. That version is the release of the code. The URL version is the contract. They move on different clocks.
- Contract tests should hit `/v1` and `/v2` as separate cases so a change in one does not silently edit the other.

**When to use:**

- A public or multi-team HTTP API that will outlive the first client.
- An internal handler with one caller can change together with that caller and skip a public version prefix.

**Examples:**

```js
import http from "node:http";

function send(res, status, body) {
  const payload = JSON.stringify(body);
  res.writeHead(status, {
    "Content-Type": "application/json; charset=utf-8",
    "Content-Length": Buffer.byteLength(payload),
  });
  res.end(payload);
}

const server = http.createServer((req, res) => {
  const url = new URL(req.url ?? "/", "http://localhost");
  if (req.method === "GET" && url.pathname === "/v1/notes") {
    send(res, 200, [{ id: 1, title: "hello" }]);
    return;
  }
  if (req.method === "GET" && url.pathname === "/v2/notes") {
    send(res, 200, [{ id: 1, title: "hello", body: "" }]);
    return;
  }
  send(res, 404, { error: "not found" });
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — package.json](https://nodejs.org/api/packages.html#nodejs-packagejson-field-definitions)
</details>

<details>
<summary>What are semantic versioning (semver) and its importance in Node.js development?</summary>

### What are semantic versioning (semver) and its importance in Node.js development?

**Definition:**

Semantic versioning numbers a release as `MAJOR.MINOR.PATCH`. You increment PATCH for a bugfix, MINOR for a backward-compatible feature, and MAJOR when you break callers. npm and `package.json` use that contract so a range such as `^1.4.0` can install later 1.x releases and refuse 2.0.0.

**How it works:**

- `package.json` `"version"` is the package’s own semver. Dependencies list ranges. The lockfile pins the exact tree that was installed.
- `^1.4.0` allows `1.x` at or above `1.4.0`. `~1.4.0` allows `1.4.x`. An exact version allows only that version.
- For `0.y.z`, the leading zero means the API is still unstable. A minor bump under `0.` may break callers. npm treats `^0.2.3` more tightly than `^1.2.3`.
- Publishing a breaking change as a patch makes every consumer of `^` pick it up on the next install. That is how a small release takes down builds.
- `npm ci` installs the lockfile. `npm update` moves within the ranges and rewrites the lock. Commit the lockfile so CI and production resolve the same versions.
- Your HTTP `/v1` contract and the npm version are related only by your own discipline. A MAJOR npm bump is the right companion to a breaking programmatic API.

**When to use:**

- Every package you publish, and every app dependency range you write.
- Apps that are not published still benefit from a lockfile and from reading ranges before you upgrade.

**Examples:**

```json
{
  "name": "notes",
  "version": "1.4.2",
  "dependencies": {
    "express": "^4.21.0"
  }
}
```

**Sources:**

- [Node.js — package.json](https://nodejs.org/api/packages.html#nodejs-packagejson-field-definitions)
- [npm — About semantic versioning](https://docs.npmjs.com/about-semantic-versioning)
- [Semantic Versioning](https://semver.org/)
</details>

### Node.js Advanced Topics

<details>
<summary>What is the difference between exports and module.exports in Node.js?</summary>

### What is the difference between exports and module.exports in Node.js?

**Definition:**

In a CommonJS module, `exports` starts as a shortcut to `module.exports`. Adding a property (`exports.area = fn`) changes the object callers receive. Assigning `exports = something` rebinds the local name and leaves `module.exports` as it was. Replacing the export means assigning `module.exports`.

**How it works:**

- Node wraps the file in a function and passes `module` and `exports`, with `exports` set to `module.exports` (an empty object at the start).
- `exports.hello = true` is the same write as `module.exports.hello = true`. `require` returns `module.exports`.
- `exports = { hello: false }` points the local binding at a new object. The module still exports the original object. The docs show this with the comment that the assignment is not exported.
- `module.exports = class Square {}` replaces the exported value. Callers receive the class, not a bag of properties.
- The usual pattern when you replace and still want the shortcut is `module.exports = exports = function Constructor() {}`.
- ES modules do not have `exports` or `module.exports`. They use `export` and `import`. A CommonJS `module.exports` shows up as the default import when an ES module loads that file.

**When to use:**

- `exports.fn = ...` when the module exports several functions.
- `module.exports = ...` when the module exports one function, class, or object.

**Examples:**

```js
module.exports.hello = true;

exports.area = (r) => Math.PI * r ** 2;

module.exports = class Square {
  constructor(width) {
    this.width = width;
  }
};
```

**Sources:**

- [Node.js — exports shortcut](https://nodejs.org/api/modules.html#exports-shortcut)
- [Node.js — ECMAScript modules](https://nodejs.org/api/esm.html)
</details>

<details>
<summary>How can you create a simple TCP server in Node.js?</summary>

### How can you create a simple TCP server in Node.js?

**Definition:**

`net.createServer` from `node:net` listens for TCP connections. The callback receives a `net.Socket`, which is a duplex stream. You `write` bytes to the client and `end` the socket when the exchange is finished. `listen(port)` binds the port.

**How it works:**

- The socket emits `'data'` for incoming bytes, `'end'` when the client half-closes, and `'error'` on failure. Listen for `'error'` so a reset does not crash the process.
- `socket.write(chunk)` returns `false` when the kernel buffer is full. Wait for `'drain'` before writing more. `socket.pipe(socket)` echoes and applies that backpressure for you.
- TCP has no HTTP headers and no `req.url`. You define the framing (a newline, a length prefix). `node:http` is what parses HTTP on top of this kind of socket.
- `server.listen(port, host)` binds. Listen for `'error'` on the server for `EADDRINUSE`.
- Each connection is one socket on the event loop. A long CPU loop in the `'data'` handler stalls every other socket.
- Close with `server.close()`. Existing sockets stay up until they end, unless you destroy them.

**When to use:**

- A custom binary or line protocol, or a small internal service that is not HTTP.
- Browsers and JSON APIs want `node:http` instead.

**Examples:**

```js
import net from "node:net";

const server = net.createServer((socket) => {
  socket.write("hello\n");
  socket.pipe(socket);
});

server.on("error", (err) => {
  console.error(err);
});

server.listen(8124, "127.0.0.1");
```

**Sources:**

- [Node.js — Net](https://nodejs.org/api/net.html)
</details>

<details>
<summary>What is REPL in Node.js?</summary>

### What is REPL in Node.js?

**Definition:**

REPL means read, eval, print, loop. Running `node` with no script opens one in the terminal: you type an expression, Node evaluates it, prints the result, and waits for the next line. `node:repl` can start that same loop from code, including over a socket.

**How it works:**

- The default session uses `process.stdin` and `process.stdout`. The prompt is `>`. The result of each expression is stored in `_`.
- Tab completes names. `.help` lists commands. `.exit` or Ctrl+D ends the session. `.editor` gathers a multi-line block.
- `repl.start({ prompt, input, output })` builds a session. The docs show one attached to a `net.Socket`, so a remote client gets a prompt. That socket can run any JavaScript the process can run.
- `useGlobal: true` makes the session share the real `global` object. The default REPL keeps its own context.
- A REPL is a development tool. Exposing `repl.start` on a network port without authentication gives away the process.
- Commands you type are not a substitute for a test file. They vanish when the session ends.

**When to use:**

- Poking at a function or a value while you learn an API.
- A breakpoint in `--inspect` is the better tool once the code lives in a file.

**Examples:**

```js
import repl from "node:repl";

repl.start({ prompt: "> " });
```

```console
node
```

**Sources:**

- [Node.js — REPL](https://nodejs.org/api/repl.html)
</details>

<details>
<summary>Explain the role of a reverse proxy with Node.js applications.</summary>

### Explain the role of a reverse proxy with Node.js applications.

**Definition:**

A reverse proxy is the process clients connect to. It terminates TLS, can serve static files, and forwards dynamic requests to one or more Node processes. Node listens on a private port. The proxy is not a core module. nginx, Caddy, and cloud load balancers are the usual programs in that seat.

**How it works:**

- The browser speaks HTTPS to the proxy. The proxy speaks HTTP to `127.0.0.1:PORT` where `node` is listening. `server.listen` never has to own certificate files.
- Several Node processes (or hosts) sit behind one public name. The proxy spreads connections. That is how you use more than one machine. `node:cluster` only covers cores on one machine.
- The proxy can gzip, limit request rate, and cut off slow clients before they occupy a Node process.
- `X-Forwarded-For` and `X-Forwarded-Proto` tell Node what the proxy saw. Trust those headers only when the immediate peer is your proxy. A client that can connect straight to Node can forge them.
- WebSocket upgrades have to be forwarded too (`Upgrade` and `Connection`). A proxy that strips them breaks a `ws` server sitting behind it.
- Health checks hit a cheap path such as `/health` so the proxy can stop sending traffic to a dead process.

**When to use:**

- Every production HTTP deployment that faces the public internet.
- Local `node server.js` during development can skip it.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((req, res) => {
  const forwarded = req.headers["x-forwarded-proto"];
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end(typeof forwarded === "string" ? forwarded : "direct");
});

server.listen(8000, "127.0.0.1");
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Dockerizing a Node.js web app](https://nodejs.org/en/docs/guides/nodejs-docker-webapp)
</details>

<details>
<summary>How do Node.js streams enhance performance?</summary>

### How do Node.js streams enhance performance?

**Definition:**

Streams move a payload in chunks, so memory stays near the high-water mark instead of the full size. `pipeline` from `node:stream/promises` connects a readable to a writable, applies backpressure, and destroys the chain if one side errors. That is how a large file or upload avoids a single giant `Buffer`.

**How it works:**

- `readFile` allocates for the whole file, then you process it. `createReadStream` emits chunks as the disk returns them. The first byte can be handled before the last byte is read.
- `writable.write` returns `false` when the destination is behind. The readable should pause until `'drain'`. `pipe` and `pipeline` do that pause for you. Ignoring `false` piles chunks in memory.
- An HTTP response is already a writable stream. Piping a file stream to `res` sends the body as it is read. The client can start receiving before the file is finished.
- `pipeline` forwards the first error and cleans up. A hand-rolled `pipe` leaves the destination open when the source emits `'error'`, which the stream docs call out as a leak.
- Time to first byte drops because you do not wait for the full read. Total CPU can be similar. The win is memory and overlap of read and write.
- Small files still fit `readFile`. A stream has its own objects per chunk. Use it when the payload is large or open-ended.

**When to use:**

- File downloads, uploads, compression (`zlib.createGzip` in the middle of a `pipeline`), and proxying a response body.
- A few kilobytes of JSON are simpler as one buffer.

**Examples:**

```js
import { createReadStream, createWriteStream } from "node:fs";
import { createGzip } from "node:zlib";
import { pipeline } from "node:stream/promises";

await pipeline(
  createReadStream("notes.txt"),
  createGzip(),
  createWriteStream("notes.txt.gz"),
);
```

**Sources:**

- [Node.js — Stream](https://nodejs.org/api/stream.html)
- [Node.js — File system](https://nodejs.org/api/fs.html)
</details>

### Frameworks and Libraries in Node.js

<details>
<summary>Describe some popular frameworks and libraries in the Node.js ecosystem.</summary>

### Describe some popular frameworks and libraries in the Node.js ecosystem.

**Definition:**

Frameworks and libraries here are npm packages on top of core. Express, Fastify, and Koa wrap `node:http` with routing. NestJS adds a module structure and TypeScript. None of them ship inside the `node` binary. Core stays `node:http`, `node:fs`, `node:stream`, and the test runner.

**How it works:**

- Express is the common HTTP layer: a router, `(req, res, next)` middleware, and a large set of matching packages.
- Fastify is an HTTP framework built around JSON Schema for routes and a focus on low overhead. It is a separate package, with its own `listen` and plugin model.
- Koa is a smaller core from the same authors as Express. Middleware is `async (ctx, next)`. Routing and body parsing are extra packages.
- NestJS organizes code as modules and providers, with decorators, and can sit on Express or Fastify for the actual HTTP server.
- Data libraries (`mysql2`, `mongodb`, Prisma) and realtime libraries (`ws`, Socket.IO) are the same kind of dependency: you install them and import them. They are not `node:` modules.
- Pick a stack and commit its lockfile. Mixing two HTTP frameworks in one process is two servers’ worth of concepts for one port.

**When to use:**

- Express or Fastify for an HTTP API. NestJS when the team wants a fixed layout. Core `node:http` when the surface is a handful of routes.
- Add a library when core does not do that job. Do not add one for `fs`, `path`, or `http`.

**Examples:**

```js
import http from "node:http";

http
  .createServer((_req, res) => {
    res.writeHead(200, { "Content-Type": "application/json; charset=utf-8" });
    res.end(JSON.stringify({ ok: true }));
  })
  .listen(3000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Express](https://expressjs.com/)
- [Fastify](https://fastify.dev/)
</details>

<details>
<summary>How is Koa different from Express.js?</summary>

### How is Koa different from Express.js?

**Definition:**

Both are npm HTTP frameworks from the same original authors, sitting on Node’s HTTP server. Express hands you `req`, `res`, and `next`, and its ecosystem grew up around callback middleware. Koa hands you one `ctx` and expects `async` middleware that `await next()`. Koa’s own package does not include a router.

**How it works:**

- An Express handler is `(req, res) => { res.status(200).json(body) }`. Middleware calls `next()` or ends the response. Routing is `app.get` on the Express app.
- A Koa handler is `async (ctx, next) => { await next(); ctx.body = body }`. `ctx.request` and `ctx.response` wrap the Node request and response. Downstream middleware runs at `await next()`, then the function continues.
- Body parsing, routing, and static files are separate Koa packages (`@koa/router` is the common router). Express applications usually get those from Express middleware packages too, but the router itself is built in.
- Errors in Koa reject the middleware promise, and an upstream `try/catch` around `await next()` can see them. Express error handlers are functions with four arguments `(err, req, res, next)`.
- Neither framework moves work off the event loop. A blocking loop blocks the process in both.
- You can mount either on `http.createServer`. The objects you touch in the handler are the difference.

**When to use:**

- Express when you want the larger existing middleware set and `app.get` in the same package.
- Koa when you want a small core and async middleware as the default shape.

**Examples:**

```js
import Koa from "koa";

const app = new Koa();

app.use(async (ctx) => {
  ctx.body = { ok: true };
});

app.listen(3000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Koa](https://koajs.com/)
- [Express — Writing middleware](https://expressjs.com/en/guide/writing-middleware.html)
</details>

<details>
<summary>What is NestJS and when would you choose it for your Node.js project?</summary>

### What is NestJS and when would you choose it for your Node.js project?

**Definition:**

NestJS is an npm framework that organizes a Node server into modules, controllers, and injectable providers, usually written in TypeScript. HTTP is handled by an adapter. The default adapter is Express. You can switch the adapter to Fastify. Nest is not part of Node core.

**How it works:**

- A module declares controllers and providers. A controller method is a route (`@Get()`, `@Post()`). A provider is a class the injector constructs and passes into constructors.
- Decorators name the HTTP contract: status, params, body. The framework calls your method. You return a value or throw an HTTP exception. Nest turns that into the response.
- Pipes validate and transform input. Guards decide whether the route runs. Interceptors wrap the call. Those are Nest’s names for middleware-like steps with a fixed order.
- The underlying server is still Node. A blocking loop in a provider blocks the event loop of that process.
- The CLI (`nest new`, `nest generate`) creates the folders. The layout is the point: a new teammate can find a route by finding the controller.
- You pay for that with decorators, a build step, and concepts that a ten-line `http.createServer` does not need.

**When to use:**

- A long-lived API with several domains and a team that wants one structure.
- A small service with a few routes stays clearer on Express, Fastify, or `node:http`.

**Examples:**

```js
import { Controller, Get } from "@nestjs/common";

@Controller("health")
export class HealthController {
  @Get()
  getHealth() {
    return { ok: true };
  }
}
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [NestJS — Controllers](https://docs.nestjs.com/controllers)
</details>

<details>
<summary>What are the benefits of using TypeScript with Node.js?</summary>

### What are the benefits of using TypeScript with Node.js?

**Definition:**

TypeScript checks types before the code runs and then erases them, so Node still executes JavaScript. You catch mismatched arguments, missing fields, and bad `null` handling in the editor and in `tsc`. Recent Node releases can run `.ts` files by stripping types. Stripping does not type-check. `tsc` still does that.

**How it works:**

- A `.ts` file describes values with annotations and inferred types. `tsc` reports errors and can emit `.js` plus source maps. Node runs the emitted JavaScript, or a Node that supports type stripping runs the `.ts` file directly.
- `@types/node` describes core modules (`process`, `fs`, `http`) so the editor knows `req.method` and `res.end`.
- Types disappear at runtime. A check you need while the server is up (`typeof`, a schema library) still has to be written as JavaScript that remains after erase.
- Shared types between a Node API and a TypeScript client keep the JSON contract in one place. The wire format is still JSON.
- `tsconfig.json` sets the module mode (`nodenext` matches Node’s own resolution) and how strict the checks are. A loose config hides the mistakes types are for.
- Build it in CI. A green `tsc --noEmit` means the types agree. It does not replace `node --test`.

**When to use:**

- Services and libraries more than one person edits, and any public function whose arguments are easy to mix up.
- A short script can stay plain JavaScript. The runtime behavior is the same either way.

**Examples:**

```ts
import http from "node:http";

const server = http.createServer((req, res) => {
  const method: string = req.method ?? "GET";
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end(method);
});

server.listen(8000);
```

**Sources:**

- [Node.js — TypeScript](https://nodejs.org/api/typescript.html)
- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [TypeScript — Node.js](https://www.typescriptlang.org/docs/handbook/typescript-tooling-in-5-minutes.html)
</details>

### Integrations & Third-Party Node.js Modules

<details>
<summary>How would you integrate a Node.js app with a third-party API?</summary>

### How would you integrate a Node.js app with a third-party API?

**Definition:**

Call the other service with the global `fetch`, send any required headers, and bound the wait with `AbortSignal.timeout`. Check `response.ok` before you parse the body. Keep the base URL and the token in `process.env`. Node does not ship a special client for someone else’s API.

**How it works:**

- `fetch(url, { method, headers, body, signal })` returns a promise of a `Response`. `response.json()` parses a JSON body. `response.text()` returns the raw text.
- A non-2xx status still fulfills the promise. `response.ok` is false in that case. Handle it before you trust the payload.
- `AbortSignal.timeout(ms)` aborts a hung call. Catch the abort and decide whether to retry. A retry of a `POST` that already succeeded can create a duplicate. Retry idempotent reads first.
- Map their JSON into your own type at the edge. The rest of the app should not depend on field names you do not control.
- Timeouts, status codes, and a request id belong in the log. The token does not. Read it from `process.env` and send it as a header.
- Outbound calls share the event loop. Many concurrent `fetch` calls are fine. Parsing a huge JSON body in one go is the part that can stall the loop.

**When to use:**

- Any HTTP API you do not own: payments, email, maps, another team’s service.
- A queue in front of the call when their outage must not fail your user’s request on the spot.

**Examples:**

```js
const response = await fetch(process.env.NOTES_API_URL, {
  headers: {
    Accept: "application/json",
    Authorization: `Bearer ${process.env.NOTES_API_TOKEN}`,
  },
  signal: AbortSignal.timeout(5000),
});

if (!response.ok) {
  throw new Error(`notes api status ${response.status}`);
}

const notes = await response.json();
console.log(notes);
```

**Sources:**

- [Node.js — Global fetch](https://nodejs.org/api/globals.html#fetch)
- [Node.js — process.env](https://nodejs.org/api/process.html#processenv)
</details>

<details>
<summary>What is Socket.IO and how does it work with Node.js?</summary>

### What is Socket.IO and how does it work with Node.js?

**Definition:**

Socket.IO is an npm library for two-way messages between a browser and a Node process. It is not a core module and it is not the WebSocket protocol by itself. A Socket.IO client speaks to a Socket.IO server. The server can ride on an `node:http` server you already have. The global `WebSocket` class does not speak this protocol.

**How it works:**

- `new Server(httpServer)` from `socket.io` attaches to your HTTP server. The client connects to that same origin. The first request is HTTP. The library then uses a WebSocket when the path allows it, and long polling when it does not.
- `'connection'` gives you a socket. `socket.emit(name, payload)` sends to that client. `socket.on(name, cb)` receives. `io.emit` sends to every connected socket in the default namespace.
- Rooms (`socket.join`) group sockets so you can emit to a subset. Rooms live in that server’s memory unless you add a Redis adapter for several Node processes.
- Because the framing is Socket.IO’s, a browser `new WebSocket(url)` pointed at this server does not complete a Socket.IO session. Use the `socket.io` client.
- Each connection is state on the process. Scale-out needs the adapter so an emit on one process reaches sockets on another.
- It does not replace HTTP for ordinary request/response. Keep REST on `node:http` or Express and use Socket.IO for the events.

**When to use:**

- A browser UI that must receive server pushes and send small events, and you want reconnection handled by the library.
- A plain WebSocket client and server pair is `ws` plus the global `WebSocket`, without Socket.IO’s protocol.

**Examples:**

```js
import http from "node:http";
import { Server } from "socket.io";

const server = http.createServer((_req, res) => {
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end("ok");
});

const io = new Server(server);

io.on("connection", (socket) => {
  socket.emit("hello", { ok: true });
  socket.on("ping", () => {
    socket.emit("pong");
  });
});

server.listen(3000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Socket.IO — Server API](https://socket.io/docs/v4/server-api/)
</details>

<details>
<summary>Explain how GraphQL can be used with Node.js.</summary>

### Explain how GraphQL can be used with Node.js.

**Definition:**

GraphQL is a query language for an API, implemented in Node by the `graphql` package (and usually an HTTP wrapper around it). The client sends one query to one endpoint. The server validates it against a schema and runs the resolver functions you wrote. It is not a core module, and it still rides on `node:http` or `fetch`.

**How it works:**

- A schema declares types and the root `Query` (and `Mutation` when clients write). `buildSchema` or the JS schema builders produce that schema.
- `graphql({ schema, source, rootValue })` runs a query string and returns `{ data, errors }`. Resolvers on `rootValue` or on the schema fields produce the values.
- Over HTTP, the usual shape is `POST` with a JSON body `{ "query": "{ hello }" }` to a single path. You parse that body yourself on `node:http`, or you mount a package that does it.
- The client names the fields it wants. A REST handler instead returns a fixed representation of one resource. GraphQL can still over-fetch on the server if a resolver loads too much.
- N+1 shows up when a list resolver calls the database once per child. DataLoader-style batching is an application pattern on top of the resolvers. It is not built into `node:http`.
- Auth is still yours. Check the caller before the resolvers run, the same way you would on a REST route.

**When to use:**

- Several clients that need different slices of the same graph, and a team willing to own a schema.
- A small resource API stays simpler as REST on `node:http`.

**Examples:**

```js
import { buildSchema, graphql } from "graphql";

const schema = buildSchema("type Query { hello: String }");

const result = await graphql({
  schema,
  source: "{ hello }",
  rootValue: { hello: () => "world" },
});

console.log(result.data);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [GraphQL.js](https://graphql.org/graphql-js/)
</details>

### Node.js with Frontend Technologies

<details>
<summary>How does Node.js interact with frontend frameworks like Angular or React?</summary>

### How does Node.js interact with frontend frameworks like Angular or React?

**Definition:**

The browser runs React or Angular. Node runs the API, and it can also send the HTML and JavaScript files those apps load. The usual contract is JSON over HTTP: the page calls `fetch`, Node answers with `node:http`. They share a language. They do not share a process or the DOM.

**How it works:**

- A separate dev server (Vite, the Angular CLI) serves the UI in development and proxies `/api` to Node. In production you either serve the built files from Node or from a static host, and the API stays on Node.
- `createReadStream` of a built `index.html` piped to `res` is the core way to send a file. Set `Content-Type` from the extension. A static host or a reverse proxy often does this job instead.
- Cross-origin pages need CORS headers (`Access-Control-Allow-Origin`) on the API response. Same origin needs none.
- Server-side rendering is Node calling the framework’s server renderer and sending HTML. The client bundle then hydrates that HTML in the browser.
- Authentication is a cookie or a header the browser stores and the API checks. Node does not see React state.
- Types can be shared from a common package. The runtime values still cross the network as JSON.

**When to use:**

- Node as the API whenever the UI is a separate app. Node as the file server when you want one deployable.
- The framework’s own dev server is enough while you are only editing components.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((req, res) => {
  if (req.url === "/api/notes") {
    const body = JSON.stringify([{ id: 1, title: "hello" }]);
    res.writeHead(200, {
      "Content-Type": "application/json; charset=utf-8",
      "Content-Length": Buffer.byteLength(body),
    });
    res.end(body);
    return;
  }
  res.writeHead(404).end();
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — File system](https://nodejs.org/api/fs.html)
</details>

<details>
<summary>What is server-side rendering and how can it be achieved with Node.js?</summary>

### What is server-side rendering and how can it be achieved with Node.js?

**Definition:**

Server-side rendering builds the HTML for a URL on the server and sends that HTML as the response. Node is the process that runs the renderer and writes the result to `res`. The browser paints that HTML, then loads the client bundle to attach event handlers. A client-only app instead sends a nearly empty HTML shell and fills it in the browser.

**How it works:**

- The request hits `http.createServer`. For a page route you call the framework’s server render (React’s server renderer, Angular’s platform-server, or a meta-framework such as Next.js) and `res.end` the HTML string, or pipe a stream of HTML.
- Streaming render writes the document as it is produced. That uses the response as a writable stream and gets bytes to the client sooner than buffering the full page.
- Data for the page is loaded on the server before or during that render. The client should not have to refetch everything just to show the first paint.
- The HTML must match what the client will render, or hydration warns and rewrites the DOM. Escape any user text that lands in the markup.
- Node still has one event loop. A slow render blocks other requests on that process. Cache pages that do not depend on the user, or move the render off the hot path.
- Static generation builds HTML ahead of time and Node (or a file host) only sends the file. That is a different schedule from rendering on each request.

**When to use:**

- Pages where the first HTML should contain the content (crawlers, slow devices, a meaningful first paint).
- Dashboards behind a login can stay client-rendered. The API is still Node.

**Examples:**

```js
import http from "node:http";

function renderPage(title) {
  return `<!doctype html><html><body><h1>${title}</h1></body></html>`;
}

http
  .createServer((_req, res) => {
    const html = renderPage("Notes");
    res.writeHead(200, {
      "Content-Type": "text/html; charset=utf-8",
      "Content-Length": Buffer.byteLength(html),
    });
    res.end(html);
  })
  .listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Stream](https://nodejs.org/api/stream.html)
</details>

### Node.js Best Practices

<details>
<summary>What are some coding conventions and best practices in Node.js?</summary>

### What are some coding conventions and best practices in Node.js?

**Definition:**

Write small async functions, import core modules with the `node:` prefix, and keep configuration in `process.env`. Handle errors at the call site. Do not block the event loop with `*Sync` methods or long CPU loops inside a server. Format and lint in CI so the repo stays consistent.

**How it works:**

- `import { readFile } from "node:fs/promises"` names the built-in and avoids a package that happens to share the bare name. The `node:` prefix is how core tells you it is built in.
- `await` inside `try/catch` on every I/O boundary. An error-first callback returns after `if (err)`. An `'error'` listener sits on every emitter you own.
- Throw or reject with an `Error` so there is a stack. Strings as errors throw away the trace.
- Keep request handlers free of shared mutable maps unless the data is a deliberate cache with a limit. Cluster workers do not see that map.
- Name files and functions after the job they do. One module exports one concern. `package.json` scripts (`start`, `test`, `lint`) are the commands everyone runs.
- ESLint (an npm tool) encodes the rules. `node --test` encodes the behavior. Both run before you merge.

**When to use:**

- From the first file of a service. Conventions are cheap at the start and expensive to retrofit.
- A one-off script can use `readFileSync` and skip the server rules. Say so at the top.

**Examples:**

```js
import { readFile } from "node:fs/promises";

const fileUrl = new URL("./note.txt", import.meta.url);

try {
  const text = await readFile(fileUrl, "utf8");
  console.log(text);
} catch (err) {
  console.error(err);
  process.exitCode = 1;
}
```

**Sources:**

- [Node.js — Modules](https://nodejs.org/api/modules.html)
- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
- [Node.js — Errors](https://nodejs.org/api/errors.html)
</details>

<details>
<summary>How do you ensure your Node.js application adheres to the twelve-factor app principles?</summary>

### How do you ensure your Node.js application adheres to the twelve-factor app principles?

**Definition:**

The twelve-factor model treats an app as one codebase, explicit dependencies, config in the environment, and stateless processes that bind a port and write logs to stdout. A Node service matches that with `package.json` plus a lockfile, `process.env`, `server.listen(port)`, and `console` on stdout and stderr.

**How it works:**

- One repo (or one repo per service) and `npm ci` from the lockfile. Dependencies are declared, not copied in as loose files.
- Config is `process.env` or `--env-file`. A checked-in `config/production.json` full of secrets breaks the “config in the environment” factor.
- Backing services (database, queue) are attached by URL. Swapping the URL swaps the service. Code does not embed the host.
- Build, release, and run stay separate. CI builds an artifact or image. The release adds the env. `node server.js` is the run stage.
- Processes are stateless. Session data and uploaded bytes live in a store, not in a module-level `Map` that dies with the process and is invisible to the next one.
- Concurrency is more processes (`cluster`, PM2, or more containers). Disposability is a fast start and a `SIGTERM` handler that `server.close()`s. Logs are event streams on stdout, collected outside the app.

**When to use:**

- Any service you deploy more than once or hand to another environment.
- A local script can ignore port binding and disposability. It is not that kind of app.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((_req, res) => {
  res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
  res.end("ok");
});

server.listen(Number(process.env.PORT ?? 8000));
console.log("listening");
```

**Sources:**

- [The Twelve-Factor App](https://12factor.net/)
- [Node.js — Process](https://nodejs.org/api/process.html#processenv)
- [Node.js — HTTP](https://nodejs.org/api/http.html)
</details>

<details>
<summary>What is code linting and how is it applied in Node.js?</summary>

### What is code linting and how is it applied in Node.js?

**Definition:**

Linting reads source and reports patterns you decided are mistakes: unused bindings, `==` where you wanted `===`, a forgotten `await`. ESLint is the usual npm tool. Node does not include a linter in core. You run it from a `package.json` script and from CI.

**How it works:**

- A config file (`eslint.config.js` for flat config) lists plugins and rules. The parser understands the JavaScript or TypeScript you actually run.
- `npx eslint .` exits non-zero when a rule fails. CI treats that as a failed build, the same way it treats a failed `node --test`.
- `--fix` rewrites the mechanical cases. Rules about correctness stay as reports you edit by hand.
- Lint does not execute the program and does not prove types. `tsc --noEmit` and tests cover those. Lint covers style and a set of foot-guns (empty `catch`, `eval`).
- Editor integration shows the same diagnostics while you type, using the same config the CI job uses.
- A rule that the team turns off everywhere is noise. Keep the set small enough that a red line means “change this”.

**When to use:**

- Every package in the repo, including scripts.
- Generated files and `node_modules` stay ignored so the linter only sees code you own.

**Examples:**

```json
{
  "scripts": {
    "lint": "eslint ."
  }
}
```

**Sources:**

- [Node.js — package.json](https://nodejs.org/api/packages.html)
- [ESLint](https://eslint.org/docs/latest/use/getting-started)
</details>

### Scaling Node.js Applications

<details>
<summary>What are some strategies for scaling Node.js applications?</summary>

### What are some strategies for scaling Node.js applications?

**Definition:**

Scale a Node HTTP service by running more processes and more machines, and by keeping each process’s event loop free. `node:cluster` uses the cores on one host. A load balancer uses several hosts. Caching, smaller payloads, and moving CPU work to `worker_threads` raise how much one process can do before you add the next one.

**How it works:**

- Vertical step: `cluster.fork()` or a process manager’s cluster mode, one worker per core. Workers share the port and do not share memory.
- Horizontal step: a reverse proxy or cloud load balancer spreads traffic across hosts. The app stays stateless so any process can serve any request.
- Find the bottleneck with event-loop delay, CPU, and request latency. Adding processes does not fix a single slow database query that every request awaits.
- Cache read-mostly responses outside the process (HTTP cache, Redis). An in-process `Map` does not help the other workers.
- Stream large bodies. `readFile` of a huge file on every request spends memory you then need more machines to hold.
- CPU-heavy functions go to `worker_threads` or a separate service. The HTTP workers stay responsive.

**When to use:**

- After you have measured a limit: CPU saturation, loop delay, or a single host’s network.
- Premature cluster mode complicates logs and deploys before the one process is actually busy.

**Examples:**

```js
import cluster from "node:cluster";
import http from "node:http";
import { availableParallelism } from "node:os";

if (cluster.isPrimary) {
  for (let i = 0; i < availableParallelism(); i += 1) cluster.fork();
} else {
  http
    .createServer((_req, res) => {
      res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
      res.end("ok");
    })
    .listen(8000);
}
```

**Sources:**

- [Node.js — Cluster](https://nodejs.org/api/cluster.html)
- [Node.js — Worker threads](https://nodejs.org/api/worker_threads.html)
- [Node.js — OS](https://nodejs.org/api/os.html#osavailableparallelism)
</details>

<details>
<summary>How do you handle session management in a scaled Node.js application?</summary>

### How do you handle session management in a scaled Node.js application?

**Definition:**

Store session data somewhere every process can read, or keep the session inside a signed cookie the browser sends back. A `Map` in one Node process is invisible to the other workers and disappears on restart. The cookie header is the core mechanism. The shared store is a database or a cache you access over the network.

**How it works:**

- `Set-Cookie` on the response and the `Cookie` request header are plain HTTP. Mark the cookie `HttpOnly`, `Secure` on HTTPS, and `SameSite`.
- A session id in the cookie points at a row or a Redis key. Any process that can reach that store can load the session. Sticky routing to one process is a weaker stand-in and breaks when that process dies.
- Sign or encrypt the cookie if the value itself is the session. A client can edit an unsigned cookie.
- Set a lifetime and delete the server-side record on logout. A cookie without an expiry is a browser session cookie. The server record should still expire.
- Do not put passwords or large objects in the cookie. Put an id, and keep the payload in the store.
- `express-session` and similar packages implement this pattern. They are npm libraries. The HTTP headers underneath are still `Set-Cookie` and `Cookie`.

**When to use:**

- A browser login that must survive a second Node process or a restart.
- A single-user script does not need a session. An API token in `Authorization` is the other common choice for non-browser clients.

**Examples:**

```js
import http from "node:http";
import { randomBytes } from "node:crypto";

const sessions = new Map();

const server = http.createServer((req, res) => {
  const id = randomBytes(16).toString("hex");
  sessions.set(id, { createdAt: Date.now() });
  res.writeHead(200, {
    "Content-Type": "text/plain; charset=utf-8",
    "Set-Cookie": `sid=${id}; HttpOnly; SameSite=Lax`,
  });
  res.end("ok");
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Crypto](https://nodejs.org/api/crypto.html)
- [MDN — Set-Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie)
</details>

<details>
<summary>How does the use of microservices affect the scalability of a Node.js application?</summary>

### How does the use of microservices affect the scalability of a Node.js application?

**Definition:**

Splitting a Node app into services lets you run more copies of the busy part without copying the quiet part. Each service scales with its own process count and its own data. You pay with network calls, partial failure, and operational overhead that one process did not have.

**How it works:**

- A CPU-heavy service can have many replicas. A rarely called service can have one. `cluster` and a load balancer apply per service, not to a single monolith process that does every job.
- Calls between services are I/O. The event loop handles that well, and latency adds up across the chain. A timeout (`AbortSignal.timeout`) keeps one slow hop from holding the caller.
- Data ownership decides whether you can scale. Two services writing the same rows will block on that database no matter how many Node processes you start.
- A failure is now partial. The caller needs a response for “the other service is down”. One shared process used to fail all routes together.
- Deploy and observe each service. A request id logged on both sides is how you see one user action across processes.
- Start with one Node process until a measured bottleneck has a boundary. A microservice diagram does not create capacity by itself.

**When to use:**

- A part of the system with its own load, its own release cycle, or its own data.
- A small API stays one deployable. You can still run several copies of that one service.

**Examples:**

```js
const response = await fetch(process.env.BILLING_URL, {
  signal: AbortSignal.timeout(2000),
});

if (!response.ok) {
  throw new Error(`billing status ${response.status}`);
}
```

**Sources:**

- [Node.js — Global fetch](https://nodejs.org/api/globals.html#fetch)
- [Node.js — Cluster](https://nodejs.org/api/cluster.html)
</details>

### Node.js and Message Queues

<details>
<summary>What are message queues and how are they used in Node.js?</summary>

### What are message queues and how are they used in Node.js?

**Definition:**

A message queue lets one process hand off work and move on. The producer publishes a message. A broker stores it. A consumer handles it later. Node has no message broker in core. You use a package to talk to Redis, RabbitMQ, NATS, or a cloud queue, or you `postMessage` to a `worker_threads` worker when the work must stay in-process.

**How it works:**

- The HTTP handler publishes and responds. The slow job (email, image, report) runs in another loop, so the request does not wait for it.
- The consumer is often another Node process using the same codebase. It `ack`s the message after the work succeeds. A crash before the ack lets the broker deliver again, so the job should be safe to run twice or it should record that it already ran.
- Queues smooth spikes. Producers can outrun consumers for a while. The broker holds the backlog. Consumers scale by running more processes.
- `worker_threads` and `child_process.fork` are in-process and same-machine handoffs. They vanish if the parent exits. A broker survives that.
- Payload is bytes or JSON. Keep it small and include a version and an id. The consumer loads the rest from the database.
- Poison messages need a limit. After several failures, move the message aside and alert, or the consumer will retry forever.

**When to use:**

- Work that can finish after the HTTP response, and work that must survive a process restart.
- A direct `await` is clearer when the user is waiting for the result and the call is fast.

**Examples:**

```js
import { parentPort, Worker, isMainThread } from "node:worker_threads";

if (isMainThread) {
  const worker = new Worker(new URL(import.meta.url));
  worker.postMessage({ id: 1, title: "resize" });
} else {
  parentPort.on("message", (job) => {
    parentPort.postMessage({ id: job.id, done: true });
  });
}
```

**Sources:**

- [Node.js — Worker threads](https://nodejs.org/api/worker_threads.html)
- [Node.js — Child process](https://nodejs.org/api/child_process.html)
</details>

<details>
<summary>How do you implement RabbitMQ with Node.js?</summary>

### How do you implement RabbitMQ with Node.js?

**Definition:**

RabbitMQ is a separate broker. Node talks to it with an npm client such as `amqplib`. You connect with an AMQP URL, open a channel, declare a queue, and either `sendToQueue` or `consume`. Nothing in `node:` implements AMQP.

**How it works:**

- `amqp.connect(url)` opens a TCP connection. `createChannel()` is the session you publish and consume on. The URL belongs in `process.env`.
- `assertQueue(name)` creates the queue if it is missing. Publishing to a queue nobody declared can fail or drop the message, depending on the broker setup.
- `sendToQueue(name, Buffer.from(json))` publishes bytes. `consume` invokes your function per delivery. `channel.ack(msg)` tells the broker the work is done. `nack` asks for a redelivery.
- A consumer that throws before `ack` will see the message again. Make the handler safe to run twice, or store the message id you already finished.
- Prefetch (`channel.prefetch(n)`) caps how many unacked messages this consumer holds. That keeps one slow process from taking the whole queue into memory.
- Close the channel and the connection on `SIGTERM`. A process that exits without closing can leave unacked messages until the broker’s timeout.

**When to use:**

- Work that must outlive the HTTP process and fan out to other services that also speak AMQP.
- An in-process `worker_threads` handoff is enough when there is no other consumer and a restart may drop the job.

**Examples:**

```js
import amqp from "amqplib";

const conn = await amqp.connect(process.env.RABBIT_URL);
const channel = await conn.createChannel();
await channel.assertQueue("notes");

channel.sendToQueue("notes", Buffer.from(JSON.stringify({ id: 1 })));

channel.consume("notes", (msg) => {
  if (!msg) return;
  console.log(msg.content.toString());
  channel.ack(msg);
});
```

**Sources:**

- [Node.js — Process](https://nodejs.org/api/process.html#processenv)
- [amqplib](https://amqp-node.github.io/amqplib/channel_api.html)
</details>

<details>
<summary>What is the significance of ZeroMQ in Node.js applications?</summary>

### What is the significance of ZeroMQ in Node.js applications?

**Definition:**

ZeroMQ is a messaging library, not a broker you run as a server. Processes connect sockets (publish/subscribe, request/reply, push/pull) and ZeroMQ moves the messages. From Node you use an npm binding such as `zeromq`. It is not a core module. The sockets are a concurrency tool between processes, alongside `child_process` and HTTP.

**How it works:**

- There is no central queue process to deploy. A publisher `bind`s a TCP address. Subscribers `connect`. If nobody is listening, the library’s own queues decide what is dropped or kept, according to the socket type and its high-water mark.
- Patterns are the API. PUB/SUB fans out. PUSH/PULL spreads work across workers. REQ/REP is one request then one reply. You pick the pattern instead of building a broker topology.
- Messages are frames of bytes. Send a `Buffer` or a string. Agree on the layout yourself. There is no HTTP status code.
- The Node binding uses the event loop. Receiving a message is a callback or an async iterator. A slow handler still blocks that process’s JavaScript.
- It does not store messages on disk for you. A dead subscriber misses what was published while it was down, unless you add your own persistence.
- Use it on a trusted network. The library’s job is transport and patterns, not user accounts.

**When to use:**

- Low-latency process-to-process messaging on a network you control, without operating RabbitMQ or similar.
- Work that must survive restarts and be acked wants a broker with storage instead.

**Examples:**

```js
import { Publisher } from "zeromq";

const publisher = new Publisher();
await publisher.bind("tcp://127.0.0.1:3000");
await publisher.send(["notes", "hello"]);
```

**Sources:**

- [Node.js — Net](https://nodejs.org/api/net.html)
- [zeromq.js](https://github.com/zeromq/zeromq.js)
</details>

### Node.js and Cloud Services

<details>
<summary>How do cloud platforms like AWS, Azure, or GCP facilitate Node.js application deployment?</summary>

### How do cloud platforms like AWS, Azure, or GCP facilitate Node.js application deployment?

**Definition:**

These platforms run your Node process, give it a `PORT`, terminate TLS, and restart it when it dies. You still ship a program that `listen`s and reads `process.env`. The platform adds the machine, the load balancer, logs, and a way to roll a new build forward or back. Node itself does not know which vendor is hosting it.

**How it works:**

- A container service (ECS, Cloud Run, Azure Container Apps) runs the image from the Node Docker guide. A PaaS buildpack or App Service runs `npm ci` and `node server.js` from your repo. A function service runs one exported function per request.
- The platform sets `PORT` and injects secrets as environment variables. Your code uses `process.env.PORT` and does not hard-code `80`.
- Health checks hit a path you expose. A failing check pulls the instance out of the load balancer. `server.close()` on `SIGTERM` lets the platform drain connections.
- Logs on stdout are collected. You do not SSH in to tail a file as the normal path.
- More instances are a setting or an autoscale rule. Each instance is a normal Node process with its own memory. Sessions still belong in a shared store.
- Vendor APIs (object storage, queues) are HTTP or their SDK, called like any other third-party API. They are not core modules.

**When to use:**

- When you want the host, TLS, and restarts operated for you.
- A single VM with systemd or PM2 is the smaller version of the same idea.

**Examples:**

```js
import http from "node:http";

const port = Number(process.env.PORT ?? 8000);

http
  .createServer((_req, res) => {
    res.writeHead(200, { "Content-Type": "text/plain; charset=utf-8" });
    res.end("ok");
  })
  .listen(port);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — process.env](https://nodejs.org/api/process.html#processenv)
- [Node.js — Dockerizing a Node.js web app](https://nodejs.org/en/docs/guides/nodejs-docker-webapp)
</details>

<details>
<summary>What is serverless architecture, and how does it relate to Node.js?</summary>

### What is serverless architecture, and how does it relate to Node.js?

**Definition:**

Serverless here means the platform runs your function when a request or an event arrives, then freezes or stops it. You do not keep a `server.listen` process up. Node is a common runtime for those functions: the platform loads a file and calls an exported function with the event. You still write JavaScript. You do not operate the machine.

**How it works:**

- A cold start loads the runtime and your module. A warm start reuses an existing instance. Globals and connections can survive between calls on a warm instance and must not be treated as a durable session.
- The handler returns a status and a body, or throws. The platform turns that into the HTTP response. There is no long-lived `IncomingMessage` you accept yourself, unless the product is a container that happens to scale to zero.
- Time limits are short. A job that runs for minutes belongs on a queue consumer or a normal server, not in the request function.
- Connections to a database should be opened carefully. A new connection per invocation exhausts the database. A client cached in module scope helps only while the instance stays warm.
- Filesystem writes are ephemeral. Put durable bytes in object storage. `node:fs` still works for `/tmp` on many platforms, and that disk disappears with the instance.
- Local development runs the function with the vendor’s emulator or by calling the handler yourself. Production adds the event source, the concurrency limit, and the logs.

**When to use:**

- Spiky HTTP or event-driven work where you would otherwise pay for an idle process.
- A steady high-throughput API is often cheaper and simpler as a long-running Node server.

**Examples:**

```js
export async function handler(event) {
  const name = event?.queryStringParameters?.name ?? "world";
  return {
    statusCode: 200,
    headers: { "Content-Type": "application/json; charset=utf-8" },
    body: JSON.stringify({ hello: name }),
  };
}
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Process](https://nodejs.org/api/process.html)
</details>

### Environment Management

<details>
<summary>How can you manage multiple Node.js versions on the same machine?</summary>

### How can you manage multiple Node.js versions on the same machine?

**Definition:**

Install versions side by side with a version manager and point `PATH` at one of them. `nvm` and `fnm` are the usual tools on macOS and Linux. `nvs` works across platforms. Each project can record the version it needs. The `node` binary you get is the one the manager put first on `PATH`.

**How it works:**

- A manager downloads a release from nodejs.org and unpacks it into its own directory. `nvm use 22` or `fnm use 22` rewrites the current shell’s `PATH`. Other projects keep their own selection.
- A `.nvmrc` or `.node-version` file in the repo names the version. The manager reads it when you enter the directory, if you enabled that hook.
- `package.json` `"engines": { "node": ">=22" }` documents the range. It does not switch the binary by itself. `npm` can warn when the running version is outside it.
- Installing Node from the nodejs.org installer, or with a system package, puts one binary on the machine. A second install overwrites it. That is a poor way to keep two versions.
- CI should use the same major version as development. Set it in the workflow instead of accepting the image default.
- `process.version` prints what is actually running. Check it when a bug appears only on one machine.

**When to use:**

- Any machine that works on more than one repo, and every repo that must stay on a known major version.
- One global Node is enough for a single app on a server image you rebuild when you upgrade.

**Examples:**

```js
import process from "node:process";

console.log(process.version);
```

```console
node --version
```

**Sources:**

- [Node.js — Installing Node.js](https://nodejs.org/en/download)
- [Node.js — process.version](https://nodejs.org/api/process.html#processversion)
- [Node.js — package.json engines](https://nodejs.org/api/packages.html#engines)
</details>

<details>
<summary>What are .env files and how do they work in a Node.js application?</summary>

### What are .env files and how do they work in a Node.js application?

**Definition:**

A `.env` file is a list of `KEY=VALUE` lines that become environment variables for one run. Node loads it with `node --env-file=.env` or `process.loadEnvFile()`. The file is a local stand-in for the environment a platform will inject in production. It is not a config format Node reads by itself when you only type `node server.js`.

**How it works:**

- Each non-comment line sets one variable. Values are strings. `loadEnvFile()` does not override a variable that is already set in the real environment. `NODE_OPTIONS` inside the file has no effect.
- `--env-file=.env` loads before your script starts, so top-level code sees the values. `loadEnvFile()` loads when you call it.
- The `dotenv` package is an older npm tool that fills `process.env` from the same kind of file. The built-in loader covers the common case.
- Put `.env` in `.gitignore`. Commit `.env.example` with the keys and empty values so other people know what to set.
- Production should inject the variables in the service config. Copying a `.env` file onto a server is easy to leak and easy to forget.
- Never log the file or the whole `process.env` object. Keys like `DATABASE_URL` contain secrets.

**When to use:**

- Local development and CI jobs that need a repeatable set of variables.
- Hosts that already have a secret store should use that store. The `.env` file is the developer convenience.

**Examples:**

```js
import { loadEnvFile } from "node:process";

loadEnvFile(new URL("./.env", import.meta.url));

console.log(Boolean(process.env.DATABASE_URL));
```

**Sources:**

- [Node.js — process.loadEnvFile](https://nodejs.org/api/process.html#processloadenvfilepath)
- [Node.js — process.env](https://nodejs.org/api/process.html#processenv)
</details>

<details>
<summary>Describe the usage of the config module in Node.js.</summary>

### Describe the usage of the config module in Node.js.

**Definition:**

There is no `node:config` core module. The `config` npm package (node-config) loads JSON or JS files from a `config/` directory and picks the file that matches `NODE_ENV`. `config.get("port")` returns the merged value. Secrets still belong in the environment. This package is one way teams layer non-secret defaults.

**How it works:**

- `config/default.json` holds the base. `config/production.json` overrides keys when `NODE_ENV=production`. Local overrides can live in `config/local.json`, which you do not commit.
- `config.get(name)` throws if the key is missing, so a typo fails at startup instead of returning `undefined`.
- Environment variables can override file values when you map them in a custom environment file the package understands. The mapping is explicit. A random `process.env` key is not automatically a config key.
- The merged object is read once. Changing a JSON file does not change a running process. Restart to pick up a new file.
- Do not put database passwords in those JSON files if the files are committed. Read the password from `process.env` and let the file hold the host name and the pool size.
- Because it is a dependency, lock it like any other package. Core already gives you `process.env` and `loadEnvFile` if files are enough.

**When to use:**

- An app with several non-secret settings that differ by environment and are awkward as dozens of env vars.
- A small service can stay on `process.env` alone and skip the package.

**Examples:**

```js
import config from "config";

const port = config.get("port");
console.log(port);
```

**Sources:**

- [Node.js — process.env](https://nodejs.org/api/process.html#processenv)
- [node-config](https://github.com/node-config/node-config)
</details>

### Node.js and Continuous Integration/Continuous Deployment (CI/CD)

<details>
<summary>What is continuous integration/deployment and how is it implemented for Node.js apps?</summary>

### What is continuous integration/deployment and how is it implemented for Node.js apps?

**Definition:**

Continuous integration runs install, lint, and `node --test` on every change so `main` stays runnable. Continuous delivery produces an artifact you can release. Continuous deployment releases that artifact when the checks pass. For Node the commands are `npm ci`, the test runner, and then a deploy of the same lockfile you tested.

**How it works:**

- A push or a pull request starts a clean machine. `npm ci` installs the lockfile. It fails if `package.json` and the lock disagree. That is the point.
- `node --test` and the linter run next. A non-zero exit blocks the merge. The Node version in CI matches `engines` and what developers run.
- The workflow caches the npm directory between runs. It does not cache `node_modules` from a laptop.
- Delivery builds a Docker image or a packaged tarball from that same commit. The image tag is the commit id, not `latest` alone.
- Deployment is a later job that updates the host or the cloud service. Production secrets are injected there. They are not printed in the test log.
- A failed deploy should stop and leave the previous version running. The pipeline does not SSH in and edit files by hand.

**When to use:**

- Any repo more than one person pushes to, and any repo that deploys.
- A private experiment can run `node --test` locally until it has a remote.

**Examples:**

```yaml
# .github/workflows/ci.yml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm
      - run: npm ci
      - run: npm test
```

**Sources:**

- [Node.js — Test runner](https://nodejs.org/api/test.html)
- [npm — npm ci](https://docs.npmjs.com/cli/commands/npm-ci)
</details>

<details>
<summary>How do you set up a CI/CD pipeline for a Node.js project?</summary>

### How do you set up a CI/CD pipeline for a Node.js project?

**Definition:**

A pipeline is the ordered jobs: checkout, install with `npm ci`, lint, `node --test`, build an image, then deploy that image. Each job runs on a clean machine. The deploy job runs only after tests pass, and only from the branch you release.

**How it works:**

- Pin the Node version in the workflow (`node-version: 22` or an `.nvmrc` the setup step reads). Add `cache: npm` so later runs reuse the downloaded packages.
- `"test": "node --test"` in `package.json` is the command CI calls. The same command is what you run locally.
- Split jobs when they can run together: lint and test. Deploy depends on both and on the branch (`main`).
- Build the container in CI from the Dockerfile. Push it to a registry with the commit SHA as the tag. The deploy step tells the host to run that tag.
- Store registry credentials and production env in the CI secret store. The workflow file names the secrets. It does not contain their values.
- Keep the pipeline red when `npm ci` or tests fail. A green deploy of untested code undoes the pipeline.

**When to use:**

- As soon as the app has a remote and a place it runs.
- Start with test-only CI if you do not have a host yet. Add the deploy job when the host exists.

**Examples:**

```json
{
  "scripts": {
    "test": "node --test",
    "lint": "eslint ."
  },
  "engines": {
    "node": ">=22"
  }
}
```

**Sources:**

- [Node.js — Test runner](https://nodejs.org/api/test.html)
- [npm — npm ci](https://docs.npmjs.com/cli/commands/npm-ci)
- [Node.js — package.json](https://nodejs.org/api/packages.html)
</details>

### Node.js Interview Problem Solving and Scenarios

<details>
<summary>How would you troubleshoot a slow running Node.js application?</summary>

### How would you troubleshoot a slow running Node.js application?

**Definition:**

Measure event-loop delay, CPU, and per-request time before you change code. A slow Node process is often a blocked loop (`readFileSync`, a tight loop, a huge `JSON.parse`) or a slow dependency the handler awaits. `monitorEventLoopDelay` and `node --inspect` tell those apart.

**How it works:**

- Log method, path, status, and milliseconds from `performance.now()` around the handler. The slow route is the one to profile. A process-wide average hides it.
- If loop delay climbs with CPU, take a CPU profile (`node --inspect` or `node --prof`). The hot function is the one to move to a worker or to rewrite.
- If loop delay stays low and the request is still slow, the time is in a database or an outbound `fetch`. Time that call on its own. Adding `cluster` workers will not make one query faster.
- `readFileSync`, `*Sync` crypto, and `JSON.parse` of a giant payload show up as main-thread time. Stream or move that work off the loop.
- Watch `process.memoryUsage().heapUsed`. A leak makes later requests slower as garbage collection works harder. Compare heap snapshots.
- Reproduce with a fixed input. A benchmark that does not match production traffic will send you after the wrong function.

**When to use:**

- Latency or CPU alerts, and any “it got slow after the deploy” report.
- Guessing and adding servers comes after the profile, when the profile says you are actually out of CPU.

**Examples:**

```js
import { monitorEventLoopDelay, performance } from "node:perf_hooks";

const delay = monitorEventLoopDelay({ resolution: 20 });
delay.enable();

const started = performance.now();
setInterval(() => {
  console.log({
    elapsedMs: performance.now() - started,
    loopDelayMs: delay.mean / 1e6,
  });
}, 5000).unref();
```

**Sources:**

- [Node.js — Performance hooks](https://nodejs.org/api/perf_hooks.html#monitoreventloopdelayoptions)
- [Node.js — Debugging](https://nodejs.org/en/learn/getting-started/debugging)
- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
</details>

<details>
<summary>Describe how to handle file uploads in a Node.js application.</summary>

### Describe how to handle file uploads in a Node.js application.

**Definition:**

Treat the request body as a stream. Enforce a byte limit, then `pipeline` the bytes to a `createWriteStream` or to object storage. `node:http` gives you the raw body. It does not parse `multipart/form-data`. A package such as `busboy` splits the parts. You still stream each file part instead of buffering it.

**How it works:**

- `req` is a readable stream. Reading it all into an array of chunks holds the whole upload in memory. Piping to disk keeps memory near the chunk size.
- Stop when the byte count passes the limit. Respond `413` and destroy the request so the rest of the body does not sit in the process.
- Multipart has boundaries and fields. `busboy` emits a `file` stream per part. Pipe that stream to `fs.createWriteStream` in a directory you control. Build the path with `node:path` and reject names that escape the directory.
- Generate the stored name yourself (`randomBytes`). The client’s filename is a label, not a path.
- Scan type and size before you mark the upload done. Close the write stream in `pipeline` so a failed upload does not leave a half file you then serve.
- Put the finished path or object key in your database in the same step that says the upload succeeded.

**When to use:**

- Any route that accepts a file from a client.
- A tiny JSON body can be buffered with a small limit. A video or an image series should not.

**Examples:**

```js
import { createWriteStream } from "node:fs";
import { pipeline } from "node:stream/promises";
import { randomBytes } from "node:crypto";
import path from "node:path";
import http from "node:http";
import { fileURLToPath } from "node:url";

const dir = new URL("./uploads/", import.meta.url);

const server = http.createServer(async (req, res) => {
  if (req.method !== "POST" || req.url !== "/upload") {
    res.writeHead(404).end();
    return;
  }
  const name = `${randomBytes(16).toString("hex")}.bin`;
  const filePath = path.join(fileURLToPath(dir), name);
  await pipeline(req, createWriteStream(filePath));
  res.writeHead(201, { "Content-Type": "text/plain; charset=utf-8" });
  res.end(name);
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Stream](https://nodejs.org/api/stream.html)
- [Node.js — File system](https://nodejs.org/api/fs.html)
</details>

<details>
<summary>How would you handle heavy computation tasks in a Node.js application?</summary>

### How would you handle heavy computation tasks in a Node.js application?

**Definition:**

Move the CPU work off the main event loop. `worker_threads` runs it in another V8 isolate inside the same process. A child process or another service does the same when you want a separate lifecycle. The HTTP handler stays async and only waits for a message.

**How it works:**

- A tight loop, image encode, or large compression on the main thread stops every socket until it returns. `await` does not help if the function itself is synchronous JavaScript.
- `new Worker(url)` loads a file. `postMessage` sends the input. The worker posts the result back. The main thread’s `'message'` handler finishes the HTTP response.
- Reuse a small pool of workers. Starting a worker per request spends time on isolate startup.
- Data is copied unless you transfer an `ArrayBuffer`. Copying a huge buffer can be as expensive as the work. Transfer it when the main thread no longer needs those bytes.
- `node:cluster` adds more processes for more HTTP concurrency. It does not make one heavy call faster. The call still blocks the worker that accepted it, unless that worker offloads to a thread.
- If the job can finish after the response, publish it to a queue and let a consumer process run the CPU work.

**When to use:**

- Any request path whose CPU time is large enough to move event-loop delay.
- Short calculations stay on the main thread. A worker has overhead.

**Examples:**

```js
import { Worker, isMainThread, parentPort } from "node:worker_threads";

if (isMainThread) {
  const worker = new Worker(new URL(import.meta.url));
  worker.once("message", (total) => {
    console.log(total);
  });
  worker.postMessage(1_000_000);
} else {
  parentPort.once("message", (n) => {
    let total = 0;
    for (let i = 0; i < n; i += 1) total += i;
    parentPort.postMessage(total);
  });
}
```

**Sources:**

- [Node.js — Worker threads](https://nodejs.org/api/worker_threads.html)
- [Node.js — Overview of blocking vs non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)
</details>

### Node.js and DevOps

<details>
<summary>What is the role of a Node.js application in DevOps?</summary>

### What is the role of a Node.js application in DevOps?

**Definition:**

The Node app is the workload DevOps runs, and sometimes the script that automates a step. It listens on a port, logs to stdout, exits cleanly on `SIGTERM`, and exposes a health path. The pipeline installs it with `npm ci`, tests it with `node --test`, and ships the same artifact. Node is not the orchestrator.

**How it works:**

- A health route returns 200 when the process can serve traffic. The platform uses that to add or remove the instance. A route that always returns 200 while the database is down hides a failure.
- Logs are lines on `process.stdout` and `process.stderr`. The collector is outside the process. A log file inside the container needs rotation you then have to operate.
- `SIGTERM` means “stop accepting and finish”. `server.close()` does that. A process that ignores the signal gets killed mid-request.
- `package.json` scripts are the contract CI calls: `test`, `lint`, `start`. The pipeline should not invent a different command than developers run.
- Config and secrets arrive as `process.env` at release time. The image stays the same across environments.
- Small Node scripts in the repo (migrations, a release check) are fine. They use the same runtime. They are not a second production server unless you deploy them as one.

**When to use:**

- Every service you run under a process manager, a container platform, or a cloud host.
- The principles apply even when the “DevOps team” is the same people who write the route.

**Examples:**

```js
import http from "node:http";
import process from "node:process";

const server = http.createServer((req, res) => {
  if (req.url === "/health") {
    res.writeHead(200, { "Content-Type": "application/json; charset=utf-8" });
    res.end(JSON.stringify({ ok: true }));
    return;
  }
  res.writeHead(404).end();
});

server.listen(Number(process.env.PORT ?? 8000));

process.on("SIGTERM", () => {
  server.close(() => process.exit(0));
});
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Process](https://nodejs.org/api/process.html)
- [Node.js — Test runner](https://nodejs.org/api/test.html)
</details>

<details>
<summary>Describe containerization and its benefits for Node.js applications.</summary>

### Describe containerization and its benefits for Node.js applications.

**Definition:**

A container image packages a Node version, the lockfile install, and your source so every machine runs the same filesystem. The official Node Docker guide is the usual starting point: `FROM node`, `npm ci`, `CMD ["node", "server.js"]`. The benefit is a repeatable run, a smaller blast radius, and a deploy that is “start this image”, not “set up this server”.

**How it works:**

- The image pins the Node major. A teammate on a different laptop version no longer gets a different `process.version` in production.
- `npm ci` inside the image installs Linux binaries for native addons against that image, not against your macOS `node_modules`.
- The process is isolated: its own filesystem, its own env, a published port. Another app on the host does not share that `node_modules`.
- Immutable images roll forward and back by tag. You do not patch a running container. You start a new one.
- Orchestrators run many copies of one image. The app must stay stateless, listen on `PORT`, and handle `SIGTERM`, or those copies will fight over local files and ignore shutdown.
- Images are larger than a script and need a registry and a patch cycle when the base image has a Node security fix.

**When to use:**

- Any service that must run the same way in CI and production, or share a host with other services.
- A single VM that already has the right Node binary can run `node server.js` without an image. You then own that machine’s setup.

**Examples:**

```dockerfile
FROM node:22-bookworm-slim
WORKDIR /usr/src/app
COPY package.json package-lock.json ./
RUN npm ci --omit=dev
COPY . .
ENV NODE_ENV=production
EXPOSE 8000
USER node
CMD ["node", "server.js"]
```

**Sources:**

- [Node.js — Dockerizing a Node.js web app](https://nodejs.org/en/docs/guides/nodejs-docker-webapp)
- [Node.js — Process](https://nodejs.org/api/process.html)
</details>

### Node.js and IoT

<details>
<summary>How is Node.js used in IoT (Internet of Things)?</summary>

### How is Node.js used in IoT (Internet of Things)?

**Definition:**

Node fits the gateway and the small server that many devices talk to: lots of connections, small messages, and work that is mostly waiting. The event loop holds those sockets open. Libraries on npm speak device protocols (MQTT clients, serial ports, GPIO on boards that can run Node, such as a Raspberry Pi). A tiny microcontroller usually cannot run Node. It sends messages to a Node process that can.

**How it works:**

- Devices publish readings over MQTT or HTTP. A Node process subscribes or accepts the request, validates the payload, and writes it to a store or a queue. That process is an ordinary Node service.
- On a board with a full OS, a Node program can open a serial port or GPIO through an npm binding. Those bindings are native addons, not core modules. `node:net` and `node:http` are the parts that are built in.
- Messages stay small. A sensor reading is a JSON object or a few bytes. Streaming and backpressure still matter when a device sends faster than you store.
- The device and the cloud are different failure domains. The gateway queues readings when the network is down and sends them when it returns.
- Security is per device: a credential, TLS to the broker, and a way to reject a device you no longer own.
- Johnny-Five and similar npm projects are how people drive hobby hardware from JavaScript. They are packages. They are not part of the `node` binary.

**When to use:**

- A gateway, an ingest API, or a board that already runs Linux and has enough RAM for Node.
- A coin-cell sensor wants a smaller runtime and a radio. Node belongs on the other end of that radio.

**Examples:**

```js
import http from "node:http";

const server = http.createServer((req, res) => {
  if (req.method === "POST" && req.url === "/readings") {
    res.writeHead(202, { "Content-Type": "application/json; charset=utf-8" });
    res.end(JSON.stringify({ accepted: true }));
    return;
  }
  res.writeHead(404).end();
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Net](https://nodejs.org/api/net.html)
</details>

<details>
<summary>What would you consider when developing a Node.js application for IoT devices?</summary>

### What would you consider when developing a Node.js application for IoT devices?

**Definition:**

Plan for small messages, flaky networks, and devices you cannot patch like a server. Keep the Node process on a gateway or a cloud ingest path unless the board has the RAM and a full OS. Bound every payload, queue work when the uplink is down, and give each device its own credential.

**How it works:**

- Memory on a Pi-class board is limited. A leak or an unbounded `Map` of readings will kill the process. Cap buffers and stream to disk or to the broker.
- The network drops. Store readings locally and send them later. An HTTP call with `AbortSignal.timeout` should fail fast and leave the reading queued.
- Clock skew and duplicate deliveries happen. Give each reading an id and a device timestamp so a retry does not insert a second row.
- TLS to the broker or the API, and a way to revoke one device without rotating every other device. Do not ship one shared password in the firmware image.
- GPIO and serial access are native addons. A blocking call in that addon stalls the event loop and delays every other sensor on that process.
- Updates have to be safe to interrupt. A half-written app on a device in the field does not come back by itself. Prefer a gateway you can redeploy over rewriting the microcontroller.

**When to use:**

- Whenever a Node process sits on the device side of an IoT system, or ingests device traffic.
- A normal web API that merely stores readings can follow the ordinary HTTP rules and still apply the payload limit and the timeout.

**Examples:**

```js
const response = await fetch(process.env.INGEST_URL, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ deviceId: "pi-1", tempC: 21.5 }),
  signal: AbortSignal.timeout(3000),
});

if (!response.ok) {
  throw new Error(`ingest status ${response.status}`);
}
```

**Sources:**

- [Node.js — Global fetch](https://nodejs.org/api/globals.html#fetch)
- [Node.js — HTTP](https://nodejs.org/api/http.html)
</details>

### Node.js and Machine Learning

<details>
<summary>Can you use Node.js for machine learning? If so, how?</summary>

### Can you use Node.js for machine learning? If so, how?

**Definition:**

Yes, mostly to run a model or to call a service that runs one. Node is a strong place for the API, the queue, and the preprocessing of JSON and files. Training large models is usually done in Python. There is no machine-learning module in Node core. Inference uses an npm package such as TensorFlow.js or ONNX Runtime, or an HTTP call to a model server.

**How it works:**

- A request hits `node:http` or `fetch`. You validate the input, run the model or call the model service, and return JSON. The event loop must stay free, so a heavy numeric loop belongs in a worker thread or in that other service.
- TensorFlow.js (`@tensorflow/tfjs-node`) runs graphs in-process, with a native backend. ONNX Runtime for Node loads an `.onnx` file you exported from a training stack.
- Calling a Python service over HTTP keeps the training stack where it is and leaves Node as the product API. Timeouts and a queue apply the same way as any third-party call.
- Models are large files. Load them once at startup, not on every request. Ship the file beside the app or download it from object storage.
- Inputs and outputs need an explicit schema. A model that expects a float array will not accept a raw upload unless you convert it.
- GPU builds and native binaries are per platform. The Docker image has to match the binary you installed. `npm ci` on a laptop does not produce the Linux addon.

**When to use:**

- Inference next to a Node API, or orchestration around a model that lives elsewhere.
- Research training, notebooks, and large GPU jobs stay in the Python tools that produced the model.

**Examples:**

```js
const response = await fetch(process.env.MODEL_URL, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ features: [0.2, 1.4, 3.1] }),
  signal: AbortSignal.timeout(5000),
});

if (!response.ok) {
  throw new Error(`model status ${response.status}`);
}

const prediction = await response.json();
console.log(prediction);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — Worker threads](https://nodejs.org/api/worker_threads.html)
- [Node.js — Global fetch](https://nodejs.org/api/globals.html#fetch)
</details>

<details>
<summary>What are some machine learning libraries or tools available for Node.js?</summary>

### What are some machine learning libraries or tools available for Node.js?

**Definition:**

They are npm packages, not core modules. TensorFlow.js (`@tensorflow/tfjs-node`) runs TensorFlow graphs in a Node process. ONNX Runtime (`onnxruntime-node`) loads `.onnx` models exported from other training tools. For anything heavier, Node calls a model server over HTTP. There is no `node:ml`.

**How it works:**

- `@tensorflow/tfjs-node` uses a native backend so tensor math is not pure JavaScript. You install it for the platform you run. The browser build (`@tensorflow/tfjs`) is a different package and does not use that native binding.
- `onnxruntime-node` creates an inference session from a file path and runs named inputs. The model file is an artifact you version beside the code or download at startup.
- Both packages can block the event loop during a large `run`. Call them from a `worker_threads` worker when the API must stay responsive.
- Hugging Face and similar hosts expose HTTP inference. `fetch` is enough. You do not embed the weights in the Node process.
- Training APIs exist in some of these libraries and are a poor default on a request handler. Train offline, export the model, and load the frozen graph in Node.
- Pin the package version. A native addon built for one Node ABI fails on the next major until you rebuild it.

**When to use:**

- In-process inference when the model is small enough and you want one deployable.
- A separate model service when the model needs a GPU the API nodes do not have.

**Examples:**

```js
import { InferenceSession } from "onnxruntime-node";

const session = await InferenceSession.create("./model.onnx");
console.log(session.inputNames);
```

**Sources:**

- [Node.js — Worker threads](https://nodejs.org/api/worker_threads.html)
- [ONNX Runtime — Node.js](https://onnxruntime.ai/docs/get-started/with-javascript/node.html)
- [TensorFlow.js — Node](https://www.tensorflow.org/js/guide/nodejs)
</details>

### APIs and Node.js

<details>
<summary>What are best practices for designing RESTful APIs in Node.js?</summary>

### What are best practices for designing RESTful APIs in Node.js?

**Definition:**

Model URLs as resources, use HTTP methods for actions, and return status codes that match the outcome. Keep JSON bodies small, paginate lists, and validate input before you touch a database. Implement the routes with `node:http` or a router on top of it. The practices are HTTP. The framework does not supply them.

**How it works:**

- Nouns in the path (`/notes`, `/notes/1`). `GET` reads, `POST` creates, `PUT` replaces, `PATCH` updates part, `DELETE` removes. `GET` and `PUT` and `DELETE` should be safe to retry.
- `201` with a `Location` after a create, `204` when there is no body, `400` for a bad request, `404` when the id is missing, `409` for a conflict. Error bodies are JSON with a stable shape, not a stack trace.
- Lists take `limit` and a cursor or an offset, and they do not return the whole table. `URLSearchParams` reads the query from `req.url`.
- Version a breaking change under `/v1`. Add optional fields without a new version.
- Set `Content-Type: application/json; charset=utf-8` and `Content-Length` from `Buffer.byteLength`. Cap the body size before `JSON.parse`.
- Authenticate before the handler does work. Log the method, the path, and the status. Do not log the authorization header.

**When to use:**

- Resource APIs for browsers, mobile apps, and other services.
- A two-way stream is a different design. Do not force it into a REST resource.

**Examples:**

```js
import http from "node:http";

function send(res, status, body) {
  const payload = JSON.stringify(body);
  res.writeHead(status, {
    "Content-Type": "application/json; charset=utf-8",
    "Content-Length": Buffer.byteLength(payload),
  });
  res.end(payload);
}

const server = http.createServer((req, res) => {
  const url = new URL(req.url ?? "/", "http://localhost");
  if (req.method === "GET" && url.pathname === "/notes") {
    send(res, 200, {
      items: [],
      limit: Number(url.searchParams.get("limit") ?? 20),
    });
    return;
  }
  send(res, 404, { error: "not found" });
});

server.listen(8000);
```

**Sources:**

- [Node.js — HTTP](https://nodejs.org/api/http.html)
- [Node.js — URL](https://nodejs.org/api/url.html)
- [MDN — HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status)
</details>
