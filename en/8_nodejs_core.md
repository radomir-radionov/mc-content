# Node.js — Core

V8, event loop, streams, buffers, and runtime fundamentals.

[← Back to hub](./8_nodejs.md)

### Related docs

- [Node.js — Core](./8_nodejs_core.md) · [Node.js — Modules](./8_nodejs_modules.md) · [Node.js — HTTP](./8_nodejs_http.md) · [Node.js — Additional](./8_nodejs_process.md) · [Node.js — Interview](./8_nodejs_interview.md)
- [JS event loop](./3_js_async_event_loop.md#event-loop) (call stack, microtasks, browser macrotasks)

### Main concepts

| Priority         | Topics                                                                                                                                                                                                                                                                                                                                             |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Essential**    | [What to learn first](#what-to-learn-first) · [Node.js overview](#nodejs-overview) · [libuv](#libuv) · [Event loop](#event-loop) · [Single-threaded vs multi-threaded](#single-threaded-vs-multi-threaded) · [EventEmitter](#eventemitter) · [Buffer](#buffer) · [Streams](#streams) · [Stream events & backpressure](#stream-events-backpressure) |
| **Skim**         | [Global object](#global-object) · [worker_threads](#worker_threads) · [Core terminology](#core-terminology)                                                                                                                                                                                                                                        |
| **Reference**    | [Encoding & numbering systems](#encoding-numbering-systems)                                                                                                                                                                                                                                                                                        |
| **Outside Core** | [Modules](./8_nodejs_modules.md) (CJS/ESM, `fs`, `path`) · [HTTP](./8_nodejs_http.md) · [JS async](./3_js_async.md) (promises, `async/await`)                                                                                                                                                                                                      |

### Sections

- [Node runtime & core](#node-runtime-core)
- [Streams, buffers & I/O](#streams-buffers-io)

Packages, CJS/ESM, and built-ins → [Node.js — Modules](./8_nodejs_modules.md).

---

## Node runtime & core

<details>
  <summary>What to learn first</summary>

### What to learn first

**Definition:** A learning priority for this page — what to master for interviews and backend work vs what to skim or look up later.

**How it works:**

| Tier               | Concepts                                                                                                                                                                                                                                                                                              | Goal                                                                                    |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Must know**      | [Node.js overview](#nodejs-overview), [libuv](#libuv) (light), [Event loop](#event-loop), [Single-threaded vs multi-threaded](#single-threaded-vs-multi-threaded), [EventEmitter](#eventemitter), [Buffer](#buffer), [Streams](#streams), [Stream events & backpressure](#stream-events-backpressure) | Explain non-blocking I/O, scheduling, binary data, and streaming in real apps           |
| **Skim**           | [Global object](#global-object), [worker_threads](#worker_threads), [Core terminology](#core-terminology)                                                                                                                                                                                             | Know `process` / `console`; when to offload CPU work; glossary for modules and env      |
| **Lower priority** | [Encoding & numbering systems](#encoding-numbering-systems)                                                                                                                                                                                                                                           | Useful for crypto and serialization; not core runtime in the same way as the event loop |

**libuv (light):** You do not need the C API. Know that libuv sits under Node, network I/O runs on the loop thread, and fs / DNS / some `crypto` use the thread pool (`UV_THREADPOOL_SIZE`, default 4).

Core is **runtime fundamentals** only. You still need sibling notes:

| Topic                                          | Where                                                                          |
| ---------------------------------------------- | ------------------------------------------------------------------------------ |
| CJS vs ESM, `require` / `import`, `fs`, `path` | [Node.js — Modules](./8_nodejs_modules.md)                                     |
| HTTP server, Express, REST                     | [Node.js — HTTP](./8_nodejs_http.md)                                           |
| Promises, `async/await`, callbacks             | [JS — Async](./3_js_async.md) · [Node.js — Interview](./8_nodejs_interview.md) |

**Thoughts:** If time is short, read **Event loop** and **libuv** first — they answer most “how does Node work?” interview questions. Then streams and EventEmitter before diving into encoding details.

</details>

<details>
  <summary>Node.js overview</summary>

### Node.js overview

**Definition:** Node.js is a free, open-source, cross-platform server-side runtime that lets developers build scalable applications with JavaScript.

**How it works:**

| Feature      | Description                                                |
| ------------ | ---------------------------------------------------------- |
| File I/O     | read/write files on disk                                   |
| Networking   | HTTP servers, TCP/UDP sockets                              |
| DB           | connect via drivers (see [9_database.md](./9_database.md)) |
| Code sharing | reuse JS between frontend and backend                      |
| Packages     | npm registry with millions of modules                      |

V8 (JS engine) + libuv (async I/O, thread pool) + Node bindings.

| Global         | Purpose                          |
| -------------- | -------------------------------- |
| `__dirname`    | directory path of current module |
| `__filename`   | full path of current module file |
| `process.argv` | CLI arguments array              |
| `process.env`  | environment variables            |

**Examples:**

```js
console.log(__dirname);
console.log(process.argv.slice(2)); // user args after node script.js
```

**Sources:** [nodejs.org](https://nodejs.org/en/about)

</details>

<details>
  <summary>libuv</summary>

### libuv

**Definition:** Cross-platform C library that powers Node's asynchronous I/O: the event loop, thread pool, TCP/UDP sockets, and async file system work.

**How it works:**

Node sits on three layers: **V8** runs JavaScript, **libuv** schedules I/O, **Node bindings** connect JS APIs to libuv.

1. JavaScript calls an async API (e.g. `fs.readFile`, `http.createServer`).
2. The Node binding hands work to libuv.
3. **Network I/O** stays on the loop thread — libuv polls sockets with OS primitives (`epoll`, `kqueue`, `IOCP`).
4. **Blocking work** (file I/O, `dns.lookup`, some `crypto`) runs on libuv's **global thread pool** (default **4** threads, tunable via `UV_THREADPOOL_SIZE`, max **1024**).
5. When work completes, libuv queues a callback; the **event loop** runs it on the main JS thread.

| Component       | Role                                                                |
| --------------- | ------------------------------------------------------------------- |
| Event loop      | One per thread. Node 20+: pending → poll → check → close → timers   |
| Thread pool     | Shared across all loops; runs blocking OS calls off the main thread |
| `uv_queue_work` | Schedules custom C/work off-thread, callback on the loop            |

**When to use:** Raise `UV_THREADPOOL_SIZE` when profiling shows fs/crypto/DNS queue wait. CPU-heavy JavaScript → `worker_threads`, not a larger pool alone.

**Pluses and minuses:**

| +                                               | −                                                     |
| ----------------------------------------------- | ----------------------------------------------------- |
| Efficient I/O without one thread per connection | Pool size caps parallel fs/crypto/DNS                 |
| Same abstraction on Linux, macOS, Windows       | libuv APIs are not thread-safe by default             |
| Keeps the main thread free for JS callbacks     | CPU-bound JS on the main thread still blocks the loop |

**Examples:**

```bash
UV_THREADPOOL_SIZE=8 node server.js
```

**Thoughts:** libuv is why Node excels at I/O-bound servers but needs workers or separate processes for CPU-bound work. The [event loop](#event-loop) is libuv's scheduling cycle as seen from JavaScript.

**Sources:** [libuv design](https://docs.libuv.org/en/v1.x/design.html) · [libuv thread pool](https://docs.libuv.org/en/v1.x/threadpool.html) · [Node.js event loop](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick)

</details>

<details>
  <summary>Event loop</summary>

### Event loop

**Definition:** The event loop is [libuv](#libuv)'s scheduler on the JavaScript thread. It is how Node does non-blocking I/O with one call stack: the kernel or the libuv thread pool finishes work elsewhere, then this loop runs the callback once the stack is free.

**How it works:**

1. Node initializes the loop and runs the entry script to completion. `fs.readFile`, `http.createServer`, timers, and `process.nextTick` only **register** work and return. They do not wait.
2. When that script returns — and again after **each later JavaScript callback** — Node drains two queues that are **not phases**: the `process.nextTick` queue, then the microtask queue (`Promise.then` / `catch` / `finally`, `queueMicrotask`, `await` continuations). A `nextTick` that keeps scheduling another `nextTick` never reaches I/O, so the process stalls.
3. Then one libuv iteration runs. Since **Node.js 20** (libuv 1.45) the order is below. Due timers can also run **once before the first iteration**, which is why older write-ups list timers first on every turn.

| Order | Phase             | What runs                                                                                                                                    |
| ----- | ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| 1     | pending callbacks | I/O deferred to the next turn, such as some TCP errors (`ECONNREFUSED`)                                                                      |
| 2     | idle, prepare     | Internal only                                                                                                                                |
| 3     | poll              | New I/O events and almost all I/O callbacks. This is where the thread **sleeps** when nothing else is ready                                  |
| 4     | check             | `setImmediate` callbacks, in the order they were scheduled                                                                                   |
| 5     | close callbacks   | Abrupt close, for example `socket.destroy()` emits `'close'` here. A normal close may be queued with `process.nextTick` instead              |
| 6     | timers            | `setTimeout` / `setInterval` whose delay has elapsed. The delay is a **minimum**, not a deadline. `setTimeout(fn, 0)` is clamped to **1 ms** |

Each phase has its own FIFO queue. The loop runs that queue until it is empty or libuv's callback limit is hit, then moves on. Poll in detail:

- Poll queue not empty: run those callbacks until empty or the limit.
- Poll queue empty and a `setImmediate` is waiting: skip the sleep and go to **check**.
- Poll queue empty and nothing is immediate: sleep until a socket is ready or the next timer is due.
- A long callback in poll can run past a timer's delay. Poll is what decides when timers get a turn.
- A `setImmediate` queued **while check is already running** waits until the next iteration.

Where the work actually happens:

| Work                                                                       | Where it runs                                      | Callback comes back     |
| -------------------------------------------------------------------------- | -------------------------------------------------- | ----------------------- |
| Network sockets                                                            | This thread, via `epoll` / `kqueue` / `IOCP`       | poll                    |
| `fs` (except watchers and `*Sync`), `dns.lookup`, some `crypto` and `zlib` | [libuv thread pool](#libuv)                        | poll                    |
| Your JavaScript                                                            | This same thread, one call stack                   | —                       |
| CPU-heavy JS                                                               | `worker_threads` — a **separate** loop and isolate | `message` on the parent |

Queues that are not phases:

| Queue                                                      | When it runs                                                                                                  |
| ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `process.nextTick`                                         | After the current operation, before the loop continues. Not drawn on the phase diagram                        |
| Microtasks                                                 | Immediately after the nextTick queue is drained                                                               |
| CommonJS script                                            | `nextTick`, then promises / `queueMicrotask`                                                                  |
| ESM module evaluation                                      | The module body is already a microtask, so `queueMicrotask` during that evaluation runs **before** `nextTick` |
| `setTimeout(fn, 0)` vs `setImmediate` from the main module | **Not fixed** — depends on whether the timer is due before the first poll                                     |
| Same pair **inside** an I/O callback (`fs.readFile`)       | `setImmediate` (check) always runs before the timer                                                           |

After each turn Node looks for ref'd handles (listening server, active timer, pending request). If none remain, the process exits. `timeout.unref()` / `immediate.unref()` drops that hold so the process can exit while the timer is still scheduled.

The browser picture — one macrotask, then every microtask, then paint — is in [JS event loop](./3_js_async_event_loop.md#event-loop). Node does not put timers, I/O, and `setImmediate` in one shared macrotask queue.

**When to use:**

- Trace log order: sync code, then `nextTick` / microtasks, then a phase callback.
- Keep every callback short. A sync loop or `fs.readFileSync` on a request blocks every other client on that process.
- Run after the caller finishes assigning handlers (`'listening'`, emit from a constructor): `process.nextTick`.
- Yield so poll and timers can run: `setImmediate`. Node recommends it over `nextTick` unless you specifically need to run before I/O continues.
- Same-turn deferral that also works in browsers: `queueMicrotask` or a promise.
- Heavy JavaScript: `worker_threads`, not a shorter timer.

**Pluses and minuses:**

| +                                                           | −                                                                                |
| ----------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Many connections share one JavaScript thread                | One long callback freezes timers, sockets, and new requests on that thread       |
| The thread sleeps in poll instead of busy-waiting           | Recursive `process.nextTick` or microtasks starve I/O                            |
| Timers, I/O, and `setImmediate` have separate queues        | `setTimeout(fn, 0)` vs `setImmediate` from the main module is not a stable order |
| `unref` lets background timers avoid keeping the process up | A timer delay is a minimum; OS scheduling and other callbacks push it later      |

**Examples:**

```js
// CommonJS / plain script. ESM top-level flips nextTick vs promise — see the table above.
console.log("1");
setTimeout(() => console.log("2"), 0);
Promise.resolve().then(() => console.log("3"));
process.nextTick(() => console.log("4"));
console.log("5");
// 1, 5, 4, 3, 2
```

```js
import fs from "node:fs";

fs.readFile(import.meta.filename, () => {
  setTimeout(() => console.log("timeout"), 0);
  setImmediate(() => console.log("immediate"));
});
// inside an I/O callback: immediate, then timeout
```

**Thoughts:** Interview answer and the shorter phase list: [What is the event loop in Node.js?](./8_nodejs_interview.md#what-is-the-event-loop-in-nodejs). If a guide still draws timers at the top of every turn, it is describing Node before 20.

**Sources:** [Node.js — The event loop](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick) · [libuv design](https://docs.libuv.org/en/v1.x/design.html) · [process.nextTick vs queueMicrotask](https://nodejs.org/api/process.html#when-to-use-queuemicrotask-vs-processnexttick) · [timers](https://nodejs.org/api/timers.html) · [Don't block the event loop](https://nodejs.org/en/learn/asynchronous-work/dont-block-the-event-loop) · [javascript.info — Event loop](https://javascript.info/event-loop)

</details>

<details>
  <summary>Global object</summary>

### Global object

**Definition:**

In Node.js, the global object (`global`) is the top-level namespace — similar to `window` in browsers. Built-ins and user-defined top-level bindings are accessible across modules.

| Property                     | Purpose                 |
| ---------------------------- | ----------------------- |
| `console`                    | logging and debugging   |
| `process`                    | process info, env, exit |
| `Buffer`                     | binary data             |
| `setTimeout` / `setInterval` | timers                  |
| `__dirname` / `__filename`   | module paths (CommonJS) |

**Examples:**

```js
console.log(process.pid);
console.log(process.env.NODE_ENV);
```

**Thoughts:** Avoid polluting `global`; prefer explicit `require`/`import` and module exports.

**Sources:** [nodejs.org globals](https://nodejs.org/api/globals.html)

</details>

<details>
  <summary>Core terminology</summary>

### Core terminology

**Definition:**

Key concepts for understanding how Node.js applications are structured and run.

| Term                  | Definition                                                      |
| --------------------- | --------------------------------------------------------------- |
| Process               | running instance of a program with its own memory and resources |
| Thread                | unit of execution within a process                              |
| Module                | self-contained unit of code with exports                        |
| Barrel file           | re-exports multiple modules from one index file                 |
| Caching               | storing frequently accessed data in memory                      |
| Dependencies          | external packages required at runtime                           |
| DevDependencies       | packages needed only during development                         |
| Dependency Injection  | dependencies provided from outside, not created inside          |
| Environment variables | OS-level config via `process.env`                               |
| `.env` file           | local env vars (never commit secrets)                           |
| `nodemon`             | dev tool that restarts Node on file changes                     |

**How it works:** `process.env` is a Node global; the `dotenv` package loads `.env` files into it at startup.

**Sources:** [nodejs.org process](https://nodejs.org/api/process.html#processenv)

</details>

<details>
  <summary>Single-threaded vs multi-threaded</summary>

### Single-threaded vs multi-threaded

**Definition:**

Node.js runs JavaScript on **one main thread**, but libuv uses a thread pool for blocking I/O (fs, crypto, DNS). For parallelism use cluster, child processes, or worker threads.

| Approach                    | Use case                            |
| --------------------------- | ----------------------------------- |
| Main thread + event loop    | I/O-bound servers                   |
| `worker_threads`            | CPU-bound JS in same process        |
| `child_process` / `cluster` | separate processes, fault isolation |
| Multiple servers / Docker   | horizontal scaling                  |

**How it works:** Node is single-threaded for JS execution; multi-threaded for I/O via libuv.

**Sources:** [nodejs.org worker_threads](https://nodejs.org/api/worker_threads.html) · [nodejs.org cluster](https://nodejs.org/api/cluster.html)

</details>

<details>
  <summary>worker_threads</summary>

### worker_threads

**Definition:** `worker_threads` runs JavaScript in parallel threads within the same process — separate V8 isolates with optional `SharedArrayBuffer` for shared memory.

**How it works:**

1. Main thread creates `new Worker('./worker.js', { workerData })`.
2. Worker runs its own event loop and JS context.
3. Communicate via `postMessage` / `on('message')` (structured clone) or `SharedArrayBuffer`.
4. Worker exits when script completes or `worker.terminate()` is called.

| vs cluster/child_process | worker_threads                          | cluster              |
| ------------------------ | --------------------------------------- | -------------------- |
| Memory                   | Shared process (optional shared memory) | Separate processes   |
| Startup                  | Lighter                                 | Heavier (new V8)     |
| Fault isolation          | Worker crash can take process           | Process isolation    |
| Use case                 | CPU-bound JS (hash, image)              | HTTP workers per CPU |

**When to use:** Password hashing batches, image resize, PDF parsing, heavy JSON transform — not for simple I/O (event loop handles that).

**Pluses and minuses:**

| +                                 | −                                   |
| --------------------------------- | ----------------------------------- |
| True parallel CPU work in Node    | No DOM; message-passing overhead    |
| Lighter than fork                 | GIL-like — one JS thread per worker |
| `workerData` passes initial state | Debugging harder                    |

**Examples:**

```js
import {
  Worker,
  isMainThread,
  parentPort,
  workerData,
} from "node:worker_threads";

if (isMainThread) {
  const worker = new Worker(new URL(import.meta.url), {
    workerData: { n: 40 },
  });
  worker.on("message", (result) => console.log("fib:", result));
} else {
  const fib = (n) => (n <= 1 ? n : fib(n - 1) + fib(n - 2));
  parentPort.postMessage(fib(workerData.n));
}
```

**Thoughts:** Do not spawn workers for every request — use a pool. Worker files can be `.js` with `type: module` in package.json.

**Sources:** [nodejs.org worker_threads](https://nodejs.org/api/worker_threads.html)

</details>

---

<a name="streams-io"></a>

## Streams, buffers & I/O

<details>
  <summary>Buffer</summary>

### Buffer

**Definition:**

`Buffer` is a global Node.js class for fixed-size binary data (extends `Uint8Array`). Used for files, network packets, images, and any non-text data.

| Concept          | Description                                          |
| ---------------- | ---------------------------------------------------- |
| Encoding         | converting Buffer ↔ string (`utf8`, `hex`, `base64`) |
| Decoding         | string → Buffer                                      |
| Why not strings? | binary data corrupts in JS strings                   |

**Examples:**

```js
const buf = Buffer.from("Hello", "utf8");
console.log(buf); // <Buffer 48 65 6c 6c 6f>
console.log(buf.toString("hex")); // 48656c6c6f

const fromHex = Buffer.from([0x1a, 0x1b, 0x1c]);
```

**Sources:** [nodejs.org buffer](https://nodejs.org/api/buffer.html)

</details>

<details>
  <summary>Encoding & numbering systems</summary>

### Encoding & numbering systems

**Definition:**

Encoding transforms data into a transmittable format. Unicode maps characters to numbers; UTF-8/UTF-16 are common encodings.

| Encoding | Description                      |
| -------- | -------------------------------- |
| ASCII    | 7-bit English characters         |
| UTF-8    | variable 1–4 bytes, web standard |
| UTF-16   | 2 or 4 bytes per character       |
| Base64   | binary → ASCII text              |
| Hex      | base-16 representation           |

**How it works:**

| Base    | Prefix | Example       |
| ------- | ------ | ------------- |
| Decimal | —      | `25`          |
| Binary  | `0b`   | `0b1011` → 11 |
| Octal   | `0o`   | `0o25` → 21   |
| Hex     | `0x`   | `0x2A` → 42   |

**Examples:**

```js
(7).toString(2); // '111'
Buffer.from("Бе", "utf8"); // Cyrillic UTF-8 bytes
```

**Sources:** [javascript.info binary-hex](https://javascript.info/binary)

</details>

<details>
  <summary>EventEmitter</summary>

### EventEmitter

**Definition:**

Built-in class for the publish/subscribe event pattern. Core to Node's event-driven architecture; many modules extend it.

| Method               | Purpose          |
| -------------------- | ---------------- |
| `.on(event, fn)`     | listen for event |
| `.emit(event, data)` | fire event       |
| `.once(event, fn)`   | listen once      |
| `.off(event, fn)`    | remove listener  |

**How it works:** `http.Server`, `net.Socket`, `fs.ReadStream`, `fs.WriteStream`.

**Examples:**

```js
import { EventEmitter } from "node:events";

const emitter = new EventEmitter();
emitter.on("data", (chunk) => console.log(chunk));
emitter.emit("data", "hello");
```

**Sources:** [nodejs.org events](https://nodejs.org/api/events.html)

</details>

<details>
  <summary>Streams</summary>

### Streams

**Definition:**

Abstract interface for processing data in chunks — efficient for large files and network transfers. All streams are `EventEmitter` instances.

| Type      | Direction           | Example                |
| --------- | ------------------- | ---------------------- |
| Readable  | read only           | `fs.createReadStream`  |
| Writable  | write only          | `fs.createWriteStream` |
| Duplex    | read + write        | `net.Socket`           |
| Transform | modify while piping | `zlib.createGzip`      |

**How it works:**

| Benefit                | Why                                   |
| ---------------------- | ------------------------------------- |
| Separation of concerns | small composable pieces               |
| Flexibility            | pipe to multiple destinations         |
| Memory efficiency      | process chunks, not whole file        |
| Time efficiency        | start processing before full download |

**Examples:**

```js
import { createReadStream, createWriteStream } from "node:fs";
import { createGzip } from "node:zlib";

createReadStream("input.txt")
  .pipe(createGzip())
  .pipe(createWriteStream("input.txt.gz"));
```

**Sources:** [nodejs.org stream](https://nodejs.org/api/stream.html)

</details>

<details>
  <summary>Stream events & backpressure</summary>

### Stream events & backpressure

**Definition:**

Readable streams emit `data` chunks in flowing mode. Writable streams emit `drain`, `finish`, `error`. **Backpressure:** pause reading when the writable buffer is full.

| Event    | Stream   | When                          |
| -------- | -------- | ----------------------------- |
| `data`   | Readable | chunk available               |
| `end`    | Readable | no more data                  |
| `drain`  | Writable | buffer emptied, safe to write |
| `finish` | Writable | all data flushed              |
| `error`  | both     | failure                       |

**Examples:**

```js
readStream.on("data", (chunk) => {
  const ok = writeStream.write(chunk);
  if (!ok) readStream.pause();
});
writeStream.on("drain", () => readStream.resume());
```

**Sources:** [nodejs.org stream backpressure](https://nodejs.org/en/docs/guides/backpressuring-in-streams/)

</details>
