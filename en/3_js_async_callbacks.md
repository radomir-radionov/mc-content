# JS Callbacks — Sync vs async, callbacks, and AJAX

Synchronous vs asynchronous execution, callbacks, side effects, AJAX, and comparing callback vs promise vs async/await patterns.

### Related docs

- [JS Event Loop](./3_js_async_event_loop.md) · [JS Promises](./3_js_async_promises.md) · [JS Async/Await](./3_js_async_await.md) · [JS Async APIs](./3_js_async_apis.md)
- [JS Functions](./3_js_functions.md) · [Web API — CORS](./6_web_api_security.md#cors) · [JS Interview](./3_js_interview.md)

[← Back to JS hub](./3_js.md) · [Async hub](./3_js_async.md)

### Main concepts

| Area         | Topics                                           |
| ------------ | ------------------------------------------------ |
| **Patterns** | Sync vs async, callbacks, AJAX                   |
| **Practice** | Side effects, callback vs promise vs async/await |

### Sections

- [Callbacks & async patterns](#callbacks-async-patterns)
- [Interview](./3_js_interview.md)

Each topic: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**.

---

## Callbacks & async patterns

<details>
  <summary>Sync vs async execution</summary>

### Sync vs async execution

**Definition:** **Synchronous** code runs in the main thread one step at a time — the next line waits for the previous to finish. **Asynchronous** code schedules work to complete later without blocking the main thread.

**How it works:**

- Sync: call stack runs each statement; function calls block until return.
- Async: host API starts work; JS continues; callback/promise runs later via queues.
- `async/await` is syntactic sugar — still single-threaded, still queue-based.
- Completion order of async tasks is **not** guaranteed to match start order (except within one chain).

|              | Sync                      | Async                                |
| ------------ | ------------------------- | ------------------------------------ |
| **Blocking** | Yes — holds the thread    | No — callback/promise runs later     |
| **Examples** | `console.log`, arithmetic | `setTimeout`, `fetch`, `fs.readFile` |
| **Ordering** | Strict top-to-bottom      | Completion order depends on queues   |

**When to use:**

| Use sync                                | Use async                              |
| --------------------------------------- | -------------------------------------- |
| Pure computation, local data transforms | Network, filesystem, timers            |
| Short DOM reads in event handler        | Long operations, multi-step I/O        |
| Validation before async call            | Anything that should not freeze the UI |

#### Common pitfalls

- Treating `fetch` as sync — it returns a Promise immediately; body is async.
- `fs.readFileSync` in Node on main thread — blocks entire process.

**Pluses and minuses:**

| +                                         | −                                             |
| ----------------------------------------- | --------------------------------------------- |
| Sync: simple reasoning, immediate results | Sync I/O blocks UI and server throughput      |
| Async: responsive UI, concurrent I/O      | Harder debugging, callback/promise complexity |
| Mix both in same app                      | Easy to accidentally block with sync work     |

**Examples:**

```js
console.log("A");
setTimeout(() => console.log("B"), 0);
console.log("C");
// A, C, B
```

**Sources:** [javascript.info/intro](https://javascript.info/intro)

</details>

<details>
  <summary>Callbacks</summary>

### Callbacks

**Definition:** A function passed as an argument to be invoked when an async operation completes — the original JavaScript async pattern.

**How it works:**

- Caller passes `callback(err, result)` or `onSuccess` / `onError` to an API.
- When async work finishes, the host invokes the callback via the event loop.
- Nested callbacks create pyramids ("callback hell") when steps depend on each other.
- Error-first convention in Node: `(err, data) => { if (err) ... }`.
- No built-in composition — manual wiring for sequential and parallel flows.

**When to use:**

| Use callbacks                       | Prefer promises/`async`                 |
| ----------------------------------- | --------------------------------------- |
| Event listeners (`on('click', fn)`) | Multi-step request pipelines            |
| Legacy Node APIs                    | New application code                    |
| Simple `setTimeout` delays          | Parallel fetches with error aggregation |
| Libraries that expose callback API  | Need `try/catch` style errors           |

#### Common pitfalls

- Forgetting to handle errors — silent failures.
- Callback called twice — guard with a flag or promisify.
- `forEach` with async callbacks — does not await; use `for...of` or `Promise.all`.

**Pluses and minuses:**

| +                         | −                                        |
| ------------------------- | ---------------------------------------- |
| Simple, universal         | Callback hell with nested logic          |
| No extra types            | Hard to compose and handle errors        |
| Works in all environments | Inversion of control — caller loses flow |
| Fine for one-shot events  | Parallel + error handling gets verbose   |

**Examples:**

```js
function loadData(url, onSuccess, onError) {
  fetch(url)
    .then((r) => r.json())
    .then(onSuccess)
    .catch(onError);
}

// Error-first (Node style)
fs.readFile("file.txt", (err, data) => {
  if (err) return console.error(err);
  console.log(data);
});
```

**Sources:** [javascript.info/callbacks](https://javascript.info/callbacks)

</details>

<details>
  <summary>Side effects</summary>

### Side effects

**Definition:** Operations that interact with the outside world — network, filesystem, DOM, timers, or logging — as opposed to pure computation on local data.

**How it works:**

- Pure functions: same input → same output, no external reads/writes.
- Side effects: mutate global state, touch DOM, call APIs, write files, `console.log`.
- Async side effects complete later — must be awaited or handled in `.then`.
- In React, side effects belong in `useEffect` or event handlers, not render.
- Tests mock side effects (fetch, timers) to keep units isolated. Jest: stub `global.fetch` to a `{ json }` Response — [Jest first tests](./7_special_testing_tooling.md#jest--first-tests-node-vs-jsdom). Stub vs mock vs spy: [Test doubles](./7_special_testing_practices.md#test-doubles-stubs-mocks-spies).

| Operation    | Example                       |
| ------------ | ----------------------------- |
| API call     | `fetch('/api')`               |
| DOM mutation | `element.textContent = '...'` |
| Timer        | `Date.now()`, `setTimeout`    |
| Logging      | `console.log`                 |

**When to use:**

| Situation                | Approach                                                                                                                                                      |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Identify what to `await` | Network, DB, file I/O                                                                                                                                         |
| Unit tests               | Mock `fetch` (`jest.fn` / Vitest `vi.fn`), fake timers (`vi.useFakeTimers`) — [Test doubles](./7_special_testing_practices.md#test-doubles-stubs-mocks-spies) |
| React data loading       | `useEffect` + cleanup (`AbortController`)                                                                                                                     |
| Redux/reducers           | Keep reducers pure — effects in thunks/sagas                                                                                                                  |

#### Common pitfalls

- Fire-and-forget `fetch` without error handling.
- Stale closure reading old state after async gap — common in React `useEffect`.
- Multiple concurrent effects updating same DOM — race to last write wins.

**Pluses and minuses:**

| +                                        | −                                       |
| ---------------------------------------- | --------------------------------------- |
| Side effects are how apps do real work   | Harder to test and reason about         |
| Async effects keep UI responsive         | Race conditions if order not managed    |
| Clear separation in well-structured code | Easy to scatter effects across codebase |

**Examples:**

```js
async function refreshUI() {
  const data = await fetch("/api/user").then((r) => r.json()); // side effect: network
  document.title = data.name; // side effect: DOM
}
```

**Sources:** [javascript.info/intro](https://javascript.info/intro)

</details>

<details>
  <summary>AJAX</summary>

### AJAX

**Definition:** **Asynchronous JavaScript and XML** — a pattern for requesting server data without a full page reload. Modern code uses `fetch` or `XMLHttpRequest`.

**How it works:**

- Browser sends HTTP in the background (`XMLHttpRequest` or `fetch`).
- **Early AJAX:** server often returned an **HTML fragment**; JS inserted it — no full reload, API still coupled to markup.
- **Today:** payload is usually **JSON**; the client renders. `fetch` returns a Promise; XHR uses `onload` / `onerror`.
- CORS, cookies, and auth headers apply — see [Web API — CORS](./6_web_api_security.md#cors).

**When to use:**

| Use AJAX/`fetch`           | Consider alternatives               |
| -------------------------- | ----------------------------------- |
| Dynamic data loading       | Static content — SSR/SSG            |
| Form submit without reload | WebSockets for push streams         |
| Search autocomplete        | Server-Sent Events for one-way push |
| Dashboard polling          | WebSocket or SSE for real-time      |

#### Common pitfalls

- Forgetting `res.ok` check — `fetch` does not reject on 404/500.
- No timeout by default — wrap with `AbortController`.
- Parsing JSON before checking status.

**Pluses and minuses:**

| +                                       | −                                     |
| --------------------------------------- | ------------------------------------- |
| Responsive SPAs                         | Error handling and caching complexity |
| Partial updates                         | CORS and auth concerns                |
| Better UX than full page reloads        | SEO/hydration concerns without SSR    |
| Standard `fetch` API in modern browsers | XHR legacy APIs still in old code     |

**Examples:**

```js
const res = await fetch("/api/items");
if (!res.ok) throw new Error(`HTTP ${res.status}`);
const items = await res.json();
```

**Sources:** [MDN AJAX](https://developer.mozilla.org/en-US/docs/Glossary/AJAX) · [MDN Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) · history: [6_web_api.md — Evolution](./6_web_api.md#evolution-of-web-apps)

</details>

<details>
  <summary>Callback vs Promise vs async/await</summary>

### Callback vs Promise vs async/await

**Definition:** Three layers of async abstraction — from manual callbacks to composable promises to synchronous-looking `async/await` syntax.

**How it works:**

- **Callbacks:** pass functions; callee invokes when done. Manual error propagation.
- **Promises:** object with `.then`/`.catch`; chainable; errors bubble to nearest `.catch`.
- **async/await:** `async` function returns Promise; `await` unwraps it; `try/catch` for errors.
- All three compile down to the same event-loop queues — syntax sugar, not new concurrency.
- Promises and `async/await` interoperate — `await` works on any thenable.

**When to use:**

| Pattern     | Best for                                       |
| ----------- | ---------------------------------------------- |
| Callbacks   | DOM events, legacy APIs, `EventEmitter`        |
| Promises    | Parallel batches (`Promise.all`), library APIs |
| async/await | Default for sequential application logic       |

#### Common pitfalls

- Mixing callbacks and promises without promisifying — double error paths.
- `async` function without `await` still returns Promise — easy to forget `await` on call site.
- Sequential `await` in loop when `Promise.all` would work.

**Pluses and minuses:**

| Pattern     | +                     | −                                                         |
| ----------- | --------------------- | --------------------------------------------------------- |
| Callback    | Simple, universal     | Callback hell, weak error handling                        |
| Promise     | Composable, chainable | Verbose for sequential logic                              |
| async/await | Readable, `try/catch` | Easy sequential trap; needs `Promise.all` for parallelism |

**Examples:**

```js
// Promise chain
getUser(id)
  .then((user) => getPosts(user.id))
  .then((posts) => render(posts))
  .catch(console.error);

// async/await equivalent
try {
  const user = await getUser(id);
  const posts = await getPosts(user.id);
  render(posts);
} catch (err) {
  console.error(err);
}
```

**Sources:** [javascript.info/async](https://javascript.info/async)

</details>

---

Interview Q&A and coding tasks live in [JS — Interview](./3_js_interview.md).
