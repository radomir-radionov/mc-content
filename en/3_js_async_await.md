# JS Async / Await — Sequential and parallel async flows

`async`/`await` basics, parallel vs sequential flows, async iterators, top-level await, and the concurrency model (tasks, workers).

### Related docs

- [JS Event Loop](./3_js_async_event_loop.md) · [JS Promises](./3_js_async_promises.md) · [JS Async APIs](./3_js_async_apis.md)
- [Web Workers](./6_web_api_performance.md#web-workers) · [Node worker_threads](./8_nodejs_core.md#worker_threads) · [JS Interview](./3_js_interview.md)

[← Back to JS hub](./3_js.md) · [Async hub](./3_js_async.md)

### Main concepts

| Area        | Topics                                                   |
| ----------- | -------------------------------------------------------- |
| **Syntax**  | `async`/`await`, parallel vs sequential, async iterators |
| **Modules** | Top-level await                                          |
| **Model**   | Microtasks, macrotasks, workers vs main thread           |

### Sections

- [Async / await](#async-await)
- [Top-level await](#top-level-await)
- [Async/await, tasks, and concurrency model](#asyncawait-tasks-and-concurrency-model)
- [Interview](./3_js_interview.md)

Each topic: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**.

---

## Async / await

<details>
  <summary>async/await basics</summary>

### async/await

**Definition:** `async` marks a function that always returns a Promise. `await` pauses execution inside that function until a Promise settles, then resumes with the value.

**How it works:**

- `async function f() { ... }` — always returns `Promise`; `return x` → `Promise.resolve(x)`; `throw` → rejection.
- `await promise` — suspends **only** that async function; other code on the thread continues.
- After await resolves, execution resumes as a **microtask**.
- `await` works on thenables, not only native Promises.
- Top-level `await` allowed in ES modules — see [Top-level await](#top-level-await).

**When to use:**

| Use async/await                | Use raw promises            |
| ------------------------------ | --------------------------- |
| Default for app logic          | Library returning thenables |
| Sequential steps with branches | `Promise.race` one-liners   |
| `try/catch` error handling     | Callback-style interop      |

#### Common pitfalls

- Missing `await` — `const x = fetch()` is a Promise, not data.
- `async` IIFE forgotten — top-level await only in modules.
- `forEach` with `async` callback — does not await iterations.

**Pluses and minuses:**

| +                                   | −                                                                   |
| ----------------------------------- | ------------------------------------------------------------------- |
| Readable sequential code            | Forgetting `await` silently breaks logic                            |
| Native `try/catch`                  | Sequential loops are slow without batching                          |
| Familiar control flow (`if`, `for`) | Debugging async stacks can be harder                                |
| Interops with promises              | Cannot use `await` in non-`async` functions (except TLA in modules) |

**Examples:**

```js
async function load() {
  const res = await fetch("/api/data");
  if (!res.ok) throw new Error(res.statusText);
  return res.json();
}

load().then((data) => console.log(data));
```

**Sources:** [javascript.info/async-await](https://javascript.info/async-await)

</details>

<details>
  <summary>Parallel vs sequential</summary>

### Parallel vs sequential

**Definition:** Sequential `await` runs tasks one after another; parallel execution starts all tasks first, then awaits together.

**How it works:**

- `const a = await fetchA(); const b = await fetchB();` — B starts only after A completes.
- `const [a, b] = await Promise.all([fetchA(), fetchB()]);` — both start immediately; wait for both.
- Key: call functions **before** `await` to start work — `const p = fetchA(); await p` is parallel-friendly.
- `for await` and sequential `await` in loop — one at a time by design.
- Use `Promise.all` for independent I/O; sequential when each step needs previous result.

**When to use:**

| Pattern              | When                                  |
| -------------------- | ------------------------------------- |
| Sequential `await`   | Step B needs result of step A         |
| `Promise.all`        | Independent fetches, all required     |
| `Promise.allSettled` | Independent, OK if some fail          |
| `map` + `all`        | N parallel items with order preserved |

#### Common pitfalls

- `await` inside `for` loop — N × latency; batch with `Promise.all`.
- Starting promises but awaiting one-by-one — still sequential if second await waits for first's completion only after first started late.
- `Promise.all` when one failure should not cancel others — use `allSettled`.

**Pluses and minuses:**

| +                                          | −                                                   |
| ------------------------------------------ | --------------------------------------------------- |
| Sequential: simple dependencies            | Sequential: sum of latencies                        |
| Parallel: min latency for independent work | Parallel: all fail if `Promise.all` and one rejects |
| `allSettled` for partial success           | More memory — all requests in flight                |

**Examples:**

```js
// Sequential — B waits for A
const a = await fetchA();
const b = await fetchB();

// Parallel — start both, await together
const [a, b] = await Promise.all([fetchA(), fetchB()]);

// Parallel in loop
const results = await Promise.all(ids.map((id) => fetchItem(id)));
```

**Sources:** [javascript.info/promise-api](https://javascript.info/promise-api)

</details>

<details>
  <summary>Async iterators and for await...of</summary>

### Async iterators and for await...of

**Definition:** Objects with `[Symbol.asyncIterator]` yield Promises; `for await...of` consumes them sequentially.

**How it works:**

- Async iterable exposes `asyncIterator()` returning `{ next() → Promise<{ value, done }> }`.
- `for await (const x of asyncIterable)` — awaits each `next()` before continuing.
- Async generators: `async function* gen() { yield await fetch(...); }`.
- Works on streams (Node readable), paginated APIs, Web Streams `ReadableStream`.
- Must be inside `async` function or top-level module (with TLA).

**When to use:**

| Use `for await...of`                   | Use `Promise.all`                       |
| -------------------------------------- | --------------------------------------- |
| File/network streams                   | Independent parallel requests           |
| Paginated API (page depends on cursor) | Fixed list of known URLs                |
| WebSocket message stream               | Batch processing with concurrency limit |

#### Common pitfalls

- Using `for await` on non-async iterable — runtime error.
- Not breaking loop on error — entire stream fails unless handled per iteration.
- Parallelizing with `for await` — wrong tool; use concurrent workers + queue.

**Pluses and minuses:**

| +                                     | −                                            |
| ------------------------------------- | -------------------------------------------- |
| Stream large async data               | Sequential — slower than parallel            |
| Clean syntax vs manual `.then` chains | Requires async function context              |
| Backpressure with streams (Node)      | Error handling needs `try/catch` around loop |
| Memory-efficient pagination           | Not for unrelated parallel fetches           |

**Examples:**

```js
async function* generateIds() {
  for (let i = 0; i < 3; i++) {
    await new Promise((r) => setTimeout(r, 100));
    yield i;
  }
}

(async () => {
  for await (const id of generateIds()) {
    console.log(id);
  }
})();
```

**Sources:** [javascript.info/async-iterators-generators](https://javascript.info/async-iterators-generators)

</details>

## Top-level await

<details>
  <summary>Top-level await (brief)</summary>

### Top-level await

**Definition:** `await` at module top level (not inside `async` function) — pauses **module evaluation** until the promise settles. ES2022 feature for ES modules only.

**How it works:**

- Only in `.mjs` files or `<script type="module">` — not CommonJS, not classic scripts.
- Module graph waits — importers of this module block until TLA completes.
- Use for: loading config, WASM init, conditional imports.
- Downside: serializes module load chain — can slow app startup.

**When to use:**

| Good use                                            | Avoid                                       |
| --------------------------------------------------- | ------------------------------------------- |
| `const config = await loadConfig()` in entry module | Heavy I/O in widely-imported utility module |
| Dynamic `await import()` based on env               | Replacing all lazy loading with TLA         |
| WASM `await init()`                                 | CJS `require` contexts                      |

#### Common pitfalls

- TLA in deep dependency tree — unexpected startup delay.
- Using in shared module imported by many — bottleneck.
- Classic script or CJS — `SyntaxError`; use async IIFE or dynamic import.

**Pluses and minuses:**

| +                       | −                                     |
| ----------------------- | ------------------------------------- |
| No async IIFE wrapper   | Blocks dependent module imports       |
| Clean config/bootstrap  | Not in CJS without dynamic `import()` |
| Natural for module init | Overuse hurts parallel loading        |

**Examples:**

```js
// config.mjs
const res = await fetch("/api/config");
export const config = await res.json();

// app.mjs — waits for config.mjs to finish
import { config } from "./config.mjs";
console.log(config.apiUrl);
```

**Sources:** [MDN top-level await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await#top_level_await) · [javascript.info/modules](https://javascript.info/modules-intro)

</details>

<a name="asyncawait-tasks-and-concurrency-model"></a>

<details>
  <summary>Async/await, tasks, and concurrency model</summary>

### Async/await, tasks, and concurrency model

**Definition:** In JavaScript, **`async/await`** is promise syntax sugar on a **single main thread**. **Tasks** are queued work: **microtasks** (promises, `async/await` continuations) and **macrotasks** (timers, I/O, UI events). **Multithreading** for JS user code means **Web Workers** (browser) or **`worker_threads` / `cluster`** (Node) — not parallel `await` on the main thread.

**How it works:**

| Concept          | JS / browser                                                           | Node.js                                           |
| ---------------- | ---------------------------------------------------------------------- | ------------------------------------------------- |
| **async/await**  | `async` returns a Promise; `await` pauses the function, not the thread | Same; continuations are microtasks                |
| **Task queues**  | Sync → drain microtasks → one macrotask → (paint)                      | Sync → `nextTick` → microtasks → event-loop phase |
| **Parallel I/O** | Start promises before `await`; `Promise.all`                           | Same + libuv thread pool for fs/crypto            |
| **Parallel CPU** | `new Worker('worker.js')` — no DOM                                     | `worker_threads` or `cluster` for multi-core      |

Interview mapping for JD **«async/await, Task, multithreading»**:

- **async/await** → sequential-looking async; errors via `try/catch`; parallel via `Promise.all`, not nested `await` in a loop.
- **Task** → **microtask/macrotask**, not C# `Task` (same idea: scheduled async unit). In .NET interviews say `Task`/`Task.Run`; in JS say event-loop queues.
- **Multithreading** → JS main thread stays single; offload CPU to workers; I/O concurrency is async + thread pool under the hood.

**When to use:** Any JD bullet on async fundamentals, event-loop puzzles, or “why Node is single-threaded but handles many connections”.

**Pluses and minuses:**

| Model                    | Pro                       | Con                                        |
| ------------------------ | ------------------------- | ------------------------------------------ |
| async/await              | Readable control flow     | Easy to write accidentally sequential code |
| Microtasks               | Ordered promise reactions | Long chains block timers and paint         |
| Workers / worker_threads | True parallel CPU         | No shared DOM; message passing overhead    |

**Thoughts:** Profile before adding workers — most slowness is I/O or main-thread work, not missing threads. Node CPU-bound loop → `worker_threads`; browser parse/sort → Web Worker. OS-level processes/threads → [Processes & multitasking](./0_system_design_core.md#processes-multitasking).

**Sources:** [javascript.info/event-loop](https://javascript.info/event-loop) · [MDN async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) · [MDN Web Workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API) · [Node worker_threads](https://nodejs.org/api/worker_threads.html)

</details>

---

Interview Q&A and coding tasks live in [JS — Interview](./3_js_interview.md).
