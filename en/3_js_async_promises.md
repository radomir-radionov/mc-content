# JS Promises — States, chaining, and static methods

Promise basics, chaining, static methods, promisification, error handling, unhandled rejections, and `Promise.withResolvers`.

### Related docs

- [JS Event Loop](./3_js_async_event_loop.md) · [JS Callbacks](./3_js_async_callbacks.md) · [JS Async/Await](./3_js_async_await.md) · [JS Async APIs](./3_js_async_apis.md)
- [JS Interview](./3_js_interview.md)

[← Back to JS hub](./3_js.md) · [Async hub](./3_js_async.md)

### Main concepts

| Area       | Topics                                            |
| ---------- | ------------------------------------------------- |
| **Core**   | States, chaining, static methods, promisification |
| **Errors** | `try/catch` with promises, unhandled rejections   |
| **Modern** | `Promise.withResolvers`                           |

### Sections

- [Promises](#promises)
- [Unhandled promise rejections](#unhandled-promise-rejections)
- [Promise.withResolvers](#promisewithresolvers)
- [Interview](./3_js_interview.md)

Each topic: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**.

---

## Promises

<details>
  <summary>Promise basics</summary>

### Promise

**Definition:** A Promise is an object representing the eventual result of an async operation. It is always in one of three states: `pending`, `fulfilled`, or `rejected`. Once settled, the state cannot change.

**How it works:**

- Created with `new Promise((resolve, reject) => { ... })` or static helpers (`Promise.resolve`, etc.).
- Executor runs **synchronously**; `resolve`/`reject` settle the promise once (ignored if called again).
- `.then(onFulfilled, onRejected)` schedules handlers as **microtasks** when promise settles.
- `.catch(fn)` is `.then(undefined, fn)` — catches rejections and throws in prior `.then`.
- `.finally(fn)` runs on settle regardless of outcome; does not receive value; returns new promise.
- `await` and `.then` return chained promises.

| Method                           | Purpose                           |
| -------------------------------- | --------------------------------- |
| `.then(onFulfilled, onRejected)` | Handle success or failure         |
| `.catch(onRejected)`             | Handle rejection                  |
| `.finally(onFinally)`            | Run cleanup regardless of outcome |

**When to use:**

| Use promises                     | Use `async/await` instead |
| -------------------------------- | ------------------------- |
| Wrapping callback APIs           | Sequential business logic |
| Library public APIs              | `try/catch` error blocks  |
| Parallel `Promise.all` batches   | Readable control flow     |
| When you need `.finally` cleanup | Loops with conditionals   |

#### Common pitfalls

- Not returning promise from `.then` — next `.then` gets `undefined`.
- Using second arg of `.then` instead of `.catch` — misses throws in fulfillment handler.
- `new Promise` anti-pattern when value already exists — use `Promise.resolve(x)`.

**Pluses and minuses:**

| +                              | −                                                                      |
| ------------------------------ | ---------------------------------------------------------------------- |
| Composable chains              | Verbose vs `async/await`                                               |
| Clear settled state machine    | Executor errors need outer `try/catch` or `.catch` on returned promise |
| `Promise.all` for parallelism  | Forgetting `return` in `.then` breaks chain                            |
| Universal interop (`thenable`) | `.then` second arg rarely used — prefer `.catch`                       |

**Examples:**

```js
const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve("I'm a Promise!"), 500);
});

promise
  .then((value) => console.log(value))
  .catch((err) => console.error(err))
  .finally(() => console.log("done"));
```

**Sources:** [MDN Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) · [javascript.info/promise-basics](https://javascript.info/promise-basics)

</details>

<details>
  <summary>Promise states & chaining</summary>

### Promise states & chaining

**Definition:** A promise follows strict rules: it must expose a `.then` method; it starts `pending` and settles once to `fulfilled` or `rejected`; the settled value is immutable.

**How it works:**

- **pending** → operation in progress; handlers queued until settle.
- **fulfilled** → `resolve(value)` called; `.then` handlers receive `value`.
- **rejected** → `reject(reason)` or throw in executor; `.catch` handlers receive `reason`.
- Each `.then`/`.catch`/`.finally` returns a **new** promise — enables chaining.
- Return value from handler becomes fulfillment of next promise; thrown error becomes rejection.
- Returning a promise from handler **flattens** — next promise waits for it.

| State       | Meaning                        |
| ----------- | ------------------------------ |
| `pending`   | Initial; operation in progress |
| `fulfilled` | Completed successfully         |
| `rejected`  | Failed with a reason           |

**When to use:**

| Situation                     | Pattern                                        |
| ----------------------------- | ---------------------------------------------- |
| Multi-step dependent requests | `.then` chain or sequential `await`            |
| Transform result per step     | `return` mapped value from each `.then`        |
| Recover from error            | `.catch(() => fallback)` then continue chain   |
| Stop chain on error           | Let rejection propagate — no recovery `.catch` |

#### Common pitfalls

- Throwing in `.then` without `.catch` — unhandled rejection.
- `async` handler in `.then` returns Promise — must return it explicitly if nested.
- Assuming `.then` runs immediately — always microtask after current sync code.

**Pluses and minuses:**

| +                                             | −                                                |
| --------------------------------------------- | ------------------------------------------------ |
| Immutable settled value — safe to pass around | Cannot cancel native Promise (use `AbortSignal`) |
| Chain encodes pipeline clearly                | Long chains hard to debug without labels         |
| Errors propagate to nearest `.catch`          | Swallowed errors if `.catch` missing at end      |

**Examples:**

```js
Promise.resolve(1)
  .then((x) => x + 1)
  .then((x) => {
    throw new Error("fail");
  })
  .catch(() => 42)
  .then(console.log);
// 42
```

**Sources:** [javascript.info/promise-chaining](https://javascript.info/promise-chaining)

</details>

<details>
  <summary>Promise static methods</summary>

### Promise static methods

**Definition:** Class methods to run multiple promises with different completion semantics.

**How it works:**

- `Promise.all(iterable)` — waits for all; rejects on first rejection; results in order.
- `Promise.allSettled(iterable)` — waits for all; never rejects; `{ status, value|reason }[]`.
- `Promise.race(iterable)` — settles when **first** promise settles (fulfill or reject).
- `Promise.any(iterable)` — fulfills on first success; rejects `AggregateError` if all fail.
- Non-promise elements wrapped with `Promise.resolve`.
- Empty iterable: `all` → `[]`, `race` → pending forever, `any` → `AggregateError`.

| Method               | Resolves when | Rejects when |
| -------------------- | ------------- | ------------ |
| `Promise.all`        | all fulfill   | first reject |
| `Promise.race`       | first settle  | first reject |
| `Promise.allSettled` | all settle    | never        |
| `Promise.any`        | first fulfill | all reject   |

**When to use:**

| Method               | Use when                                       |
| -------------------- | ---------------------------------------------- |
| `Promise.all`        | All results required (e.g. user + posts)       |
| `Promise.allSettled` | Batch where some may fail (bulk upload status) |
| `Promise.race`       | Timeout wrapper, fastest CDN wins              |
| `Promise.any`        | First successful mirror; fallback pool         |

#### Common pitfalls

- `Promise.all` with sequential `await` inside loop — use `map` + `all` instead.
- `race` for timeout — losing request still runs; cancel with `AbortController`.
- `allSettled` result order matches input order, not completion order.

**Pluses and minuses:**

| +                                    | −                                             |
| ------------------------------------ | --------------------------------------------- |
| `all` — fast fail, ordered results   | One failure loses all results                 |
| `allSettled` — inspect every outcome | More verbose result parsing                   |
| `race` — timeouts, first responder   | First reject wins — not always desired        |
| `any` — redundant mirrors/CDNs       | All-fail case needs `AggregateError` handling |

**Examples:**

```js
var p1 = new Promise((resolve) => setTimeout(() => resolve("one"), 500));
var p2 = new Promise((resolve) => setTimeout(() => resolve("two"), 100));

Promise.race([p1, p2]).then(console.log); // "two"

// Timeout pattern
const timeout = (ms) =>
  new Promise((_, reject) =>
    setTimeout(() => reject(new Error("timeout")), ms),
  );
await Promise.race([fetch("/api"), timeout(5000)]);
```

**Sources:** [javascript.info/promise-api](https://javascript.info/promise-api)

</details>

<details>
  <summary>Promisification</summary>

### Promisification

**Definition:** Wrapping a callback-style API (`fn(arg, (err, result) => ...)`) into a function that returns a Promise.

**How it works:**

- Wrap original function; return `new Promise((resolve, reject) => ...)`.
- Call original with args + callback; on error-first callback: `err ? reject(err) : resolve(result)`.
- Node provides `util.promisify(fn)` for standard error-first functions.
- Loses multi-callback semantics (e.g. `progress` callbacks) unless extended.
- Result is composable with `await` and `Promise.all`.

**When to use:**

| Situation                                | Approach                           |
| ---------------------------------------- | ---------------------------------- |
| Legacy Node `fs.*`                       | `util.promisify` or `fs.promises`  |
| Old browser API                          | Manual promisify wrapper           |
| Library with callback + promise versions | Use promise version when available |
| Custom event completion                  | `new Promise` + `once` listener    |

#### Common pitfalls

- Promisifying function that calls callback multiple times — only first settle counts.
- Wrong callback signature (success-only vs error-first).
- Not binding `this` for methods — use `promisify(fs.readFile.bind(fs))` or `fs.promises`.

**Pluses and minuses:**

| +                                  | −                                            |
| ---------------------------------- | -------------------------------------------- |
| Unified async style                | Loses multi-callback semantics               |
| Composable with `await`            | Must handle error-first convention correctly |
| One-time wrapper, reuse everywhere | Some APIs use success-only callbacks         |

**Examples:**

```js
function promisify(fn) {
  return function (...args) {
    return new Promise((resolve, reject) => {
      fn(...args, (err, result) => {
        if (err) reject(err);
        else resolve(result);
      });
    });
  };
}

// Node: util.promisify(fs.readFile)
const readFile = promisify(fs.readFile);
const data = await readFile("file.txt", "utf8");
```

**Sources:** [javascript.info/promisify](https://javascript.info/promisify)

</details>

<details>
  <summary>try...catch with promises</summary>

### try...catch with promises

**Definition:** `try...catch` handles synchronous throws; with `async/await` it also catches rejected promises. With raw promises, use `.catch()` or a trailing `.catch` on the chain.

**How it works:**

- `try/catch` around `await` catches promise rejections as if they were throws.
- Sync throw inside `async` function → rejected promise returned to caller.
- `.catch` on chain handles rejections from any prior `.then`.
- `.finally` runs after success or failure — good for cleanup (close connections).
- Unhandled rejection: rejected promise with no `.catch` and no `await` in try/catch.

| Approach              | Best for                                  |
| --------------------- | ----------------------------------------- |
| `.catch()` on chain   | Promise pipelines                         |
| `try/catch` + `await` | `async` functions                         |
| `.finally()`          | Cleanup in both success and failure paths |

**When to use:**

| Situation             | Pattern                               |
| --------------------- | ------------------------------------- |
| Single async function | `try { await ... } catch (e) { ... }` |
| Promise pipeline      | trailing `.catch(console.error)`      |
| Always close resource | `try { ... } finally { cleanup() }`   |
| Re-throw after log    | `catch (e) { log(e); throw e; }`      |

#### Common pitfalls

- `try/catch` without `await` — `fetch()` returns pending promise; errors not caught.
- Empty `catch` swallowing errors — hides bugs.
- `return` in `catch` without rethrow — caller sees success with `undefined`.

**Pluses and minuses:**

| +                                      | −                                                 |
| -------------------------------------- | ------------------------------------------------- |
| `try/catch` reads like sync code       | `try/catch` per `await` is verbose                |
| `.catch` at end of chain — one handler | Easy to forget handler on fire-and-forget promise |
| `.finally` for shared cleanup          | `.finally` does not suppress rejection            |

**Examples:**

```js
const processData = async () => {
  try {
    const data = await fetchData();
    console.log("Processed:", data);
  } catch (error) {
    console.error("Error:", error);
  }
};

// Promise chain equivalent
fetchData()
  .then((data) => console.log("Processed:", data))
  .catch((error) => console.error("Error:", error));
```

**Sources:** [javascript.info/try-catch](https://javascript.info/try-catch)

</details>

## Unhandled promise rejections

<details>
  <summary>Unhandled promise rejections</summary>

### Unhandled promise rejections

**Definition:** A rejected Promise with no `.catch` handler and no `await` inside `try/catch` is **unhandled**. Runtimes emit events and may terminate the process (Node) or log to console (browser).

**How it works:**

- Rejection tracked when promise rejects and no handler attached yet.
- If handler attached later (same tick or microtask turn in some engines), may still count as handled.
- Browser: `window` event `unhandledrejection`; `event.promise`, `event.reason`.
- `event.preventDefault()` — suppress default logging (browser).
- Node: `process.on('unhandledRejection', (reason, promise) => ...)`.
- Node 15+: unhandled rejection can crash process by default.
- `rejectionhandled` / `handled` events when handler added late.

| Environment                | Default behavior                             |
| -------------------------- | -------------------------------------------- |
| Browser                    | `console.error` + `unhandledrejection` event |
| Node (modern)              | Warning or process exit                      |
| `async` main without catch | Unhandled rejection on returned promise      |

**When to use:**

| Situation               | Fix                                                      |
| ----------------------- | -------------------------------------------------------- |
| `fetch()` without catch | Add `.catch` or `try/catch` with `await`                 |
| `async` IIFE            | `.catch` on the promise: `(async () => {})().catch(...)` |
| Express route handlers  | Wrap in try/catch or error middleware                    |
| Global monitoring       | `unhandledrejection` listener → report to Sentry         |

#### Common pitfalls

- `async` function return value ignored — rejection unhandled at call site.
- `.then(fn)` without `.catch` — rejection in `fn` propagates but outer promise needs handler.
- Swallowing in global handler without fixing root cause.

**Pluses and minuses:**

| +                                      | −                                     |
| -------------------------------------- | ------------------------------------- |
| Surfaces forgotten error paths         | Can crash production if uncaught      |
| Global handlers for logging/monitoring | Easy to miss fire-and-forget promises |
| Forces explicit error strategy         | Async stack traces can be unclear     |

**Examples:**

```js
// Unhandled — logs error / may crash Node
Promise.reject(new Error("oops"));

// Handled
Promise.reject(new Error("oops")).catch(console.error);

// async without catch
async function main() {
  throw new Error("fail");
}
main(); // unhandled
main().catch(console.error); // handled

// Browser global handler
window.addEventListener("unhandledrejection", (event) => {
  console.error("Unhandled:", event.reason);
  event.preventDefault();
});
```

**Sources:** [MDN unhandledrejection](https://developer.mozilla.org/en-US/docs/Web/API/Window/unhandledrejection_event) · [javascript.info/promise-error-handling](https://javascript.info/promise-error-handling) · [javascript.info/microtask-queue](https://javascript.info/microtask-queue) · [Node unhandledRejection](https://nodejs.org/api/process.html#event-unhandledrejection)

</details>

## Promise.withResolvers

<details>
  <summary>Promise.withResolvers (brief)</summary>

### Promise.withResolvers

**Definition:** ES2024 static method returning `{ promise, resolve, reject }` — splits promise creation from executor, useful when resolve/reject must be called from outside the constructor.

**How it works:**

- `const { promise, resolve, reject } = Promise.withResolvers();`
- Equivalent to manual `let resolve, reject; const promise = new Promise((res, rej) => { resolve = res; reject = rej; });`
- `resolve`/`reject` callable multiple times — only first counts (standard promise rules).
- Cleaner for deferred patterns, event-to-promise bridges, tests.

**When to use:**

| Use                                 | Example                                |
| ----------------------------------- | -------------------------------------- |
| Deferred — expose resolve to caller | Manual promise control                 |
| Event → promise once                | `once(emitter, 'data')` helper         |
| Test fixtures                       | Resolve promise from outside test body |
| Queue with backpressure             | Resolve when slot available            |

#### Common pitfalls

- Forgetting to call `resolve` or `reject` — promise hangs forever.
- Calling both — first wins; second ignored.
- No native support in older Node — use polyfill or manual pattern.

**Pluses and minuses:**

| +                               | −                                            |
| ------------------------------- | -------------------------------------------- |
| Avoids awkward executor pattern | ES2024 — check target environment support    |
| Clearer deferred/event APIs     | Still need `.catch` for unhandled rejections |
| Same semantics as `new Promise` | Polyfill needed for older runtimes           |

**Examples:**

```js
const { promise, resolve, reject } = Promise.withResolvers();

setTimeout(() => resolve("done"), 1000);

const result = await promise; // 'done'

// Event bridge
function once(emitter, event) {
  const { promise, resolve } = Promise.withResolvers();
  emitter.once(event, resolve);
  return promise;
}
```

**Sources:** [MDN Promise.withResolvers](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/withResolvers)

</details>

---

Interview Q&A and coding tasks live in [JS — Interview](./3_js_interview.md).
