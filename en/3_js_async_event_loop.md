# JS Event Loop — Call stack, microtasks, and macrotasks

Call stack, microtasks vs macrotasks, execution order, and `queueMicrotask` / `process.nextTick`.

### Related docs

- [JS Callbacks](./3_js_async_callbacks.md) · [JS Promises](./3_js_async_promises.md) · [JS Async/Await](./3_js_async_await.md) · [JS Async APIs](./3_js_async_apis.md)
- [JS Language](./3_js_core.md) · [Memory: stack and heap](./3_js_data_types.md#memory-stack-heap) · [Node.js](./8_nodejs.md) · [Node.js event loop](./8_nodejs_core.md#event-loop) · [JS Interview](./3_js_interview.md)

[← Back to JS hub](./3_js.md) · [Async hub](./3_js_async.md)

### Main concepts

| Area        | Topics                                              |
| ----------- | --------------------------------------------------- |
| **Runtime** | Call stack, microtasks, macrotasks, execution order |
| **Node**    | `queueMicrotask`, `process.nextTick`                |

### Sections

- [Event loop](#event-loop)
- [queueMicrotask & process.nextTick](#queuemicrotask-processnexttick)
- [Interview](./3_js_interview.md)

Each topic: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**.

---

## Event loop

<details>
  <summary>Event loop overview</summary>

### Event loop

**Definition:** The event loop is a scheduler that watches the call stack and task queues. When the stack is empty, it takes the next task from a queue and pushes it onto the stack for execution.

**How it works:**

- JavaScript runs **synchronously** on a **single thread** — only one call stack frame executes at a time.
- Long-running work (timers, I/O, network) is delegated to the host (browser Web APIs or Node libuv).
- When async work completes, its callback is placed in a **task queue** (microtask or macrotask).
- The event loop repeatedly: drain sync code → drain all microtasks → run one macrotask → (browser: maybe render) → repeat.
- The stack must be empty before any queued callback runs.

| Part                     | Role                                   |
| ------------------------ | -------------------------------------- |
| **Call stack**           | Runs synchronous function calls (LIFO) |
| **Web APIs / Node APIs** | Timers, I/O, DOM events, `fetch`       |
| **Task queues**          | Hold callbacks ready to run            |

**When to use:**

| Situation                         | What to check                                                                    |
| --------------------------------- | -------------------------------------------------------------------------------- |
| `setTimeout(fn, 0)` not immediate | Callback waits for stack + microtasks                                            |
| UI freeze                         | Long sync loop on main thread                                                    |
| Promise vs timer order            | Microtasks always before next macrotask                                          |
| Debugging async flow              | Use [JSV9000](https://www.jsv9000.app/) or [Loupe](http://latentflip.com/loupe/) |

#### Common pitfalls

- Assuming `setTimeout(fn, 0)` runs right after the current line — microtasks run first.
- Scheduling unbounded `Promise.resolve().then(...)` chains — blocks rendering and timers.
- Confusing browser event loop with Node phases — see [8_nodejs.md](./8_nodejs.md).

**Pluses and minuses:**

| +                                              | −                                                   |
| ---------------------------------------------- | --------------------------------------------------- |
| Simple concurrency model — no locks on JS heap | CPU-heavy sync code blocks everything               |
| Microtasks give predictable promise ordering   | Infinite microtasks starve macrotasks and rendering |
| Works identically in browser and Node (mostly) | Node has extra `process.nextTick` priority          |

**Examples:**

```js
console.log("sync");
setTimeout(() => console.log("macrotask"), 0);
Promise.resolve().then(() => console.log("microtask"));
// sync → microtask → macrotask
```

**Sources:** [javascript.info/event-loop](https://javascript.info/event-loop) · [JSV9000 visualizer](https://www.jsv9000.app/) · [Loupe](http://latentflip.com/loupe/)

</details>

<details>
  <summary>Call stack</summary>

### Call stack

**Definition:** A LIFO data structure that tracks active function calls and their execution contexts. All JavaScript application code runs through the call stack.

**How it works:**

- Each function call **pushes** a new execution context onto the stack.
- When a function **returns**, its context is **popped** off.
- Nested calls build a stack: `main()` → `foo()` → `bar()`.
- Each context has: variable environment, scope chain, `this` binding, and (for generators) state.
- Stack overflow (`Maximum call stack size exceeded`) happens when recursion never returns or depth exceeds engine limit (~10k–50k frames). OS/process memory model (virtual pages, heap vs stack): [Memory: stack and heap](./3_js_data_types.md#memory-stack-heap).

| Stage         | What happens                                    |
| ------------- | ----------------------------------------------- |
| **Creation**  | Allocate memory, build scope chain, bind `this` |
| **Execution** | Run function body; nested calls push onto stack |
| **Teardown**  | Pop context when function returns               |

**When to use:**

| Situation            | Implication                                                                             |
| -------------------- | --------------------------------------------------------------------------------------- |
| Reading stack traces | Top = current function, bottom = entry point                                            |
| Recursion            | Must have base case or use trampoline/iteration                                         |
| `async` functions    | `await` suspends function but does not block the stack — continuation runs as microtask |
| Event handlers       | Each invocation gets a fresh stack frame                                                |

#### Common pitfalls

- Expecting `async`/`await` to block the thread — only that function pauses; other code keeps running.
- Infinite recursion without tail-call optimization in JS (not guaranteed) — stack overflow.

**Pluses and minuses:**

| +                                       | −                                                       |
| --------------------------------------- | ------------------------------------------------------- |
| Simple mental model for sync flow       | Deep recursion can overflow                             |
| Stack traces map directly to call order | No parallel JS execution on one thread                  |
| Fast push/pop for function calls        | Async callbacks run later — not on the same stack frame |

**Examples:**

```js
function greet(name) {
  console.log("Hello, " + name + "!");
}

function main() {
  greet("Alice");
  greet("Bob");
}

main();
// Stack: main → greet('Alice') → pop → greet('Bob') → pop → pop
```

**Sources:** [javascript.info/call-stack](https://javascript.info/call-stack)

</details>

<details>
  <summary>Microtasks vs macrotasks</summary>

### Microtasks vs macrotasks

**Definition:** Asynchronous work is scheduled in two queue types. **Microtasks** run after the current sync code and before the next macrotask. **Macrotasks** run one at a time per event-loop turn.

**How it works:**

- After sync code finishes and the stack is empty, the engine drains the **entire microtask queue**.
- Each microtask can enqueue more microtasks — all run in the same turn before any macrotask.
- Then **one** macrotask runs (timer, I/O callback, UI event).
- Browser may paint between macrotasks; microtasks can delay paint.
- Node adds `process.nextTick` — runs before other microtasks (higher priority).

| Type          | Examples                                                                                        | Priority                                 |
| ------------- | ----------------------------------------------------------------------------------------------- | ---------------------------------------- |
| **Microtask** | `Promise.then/catch/finally`, `queueMicrotask`, `MutationObserver`, `async/await` continuations | Higher — drain all before next macrotask |
| **Macrotask** | `setTimeout`, `setInterval`, I/O, UI events, `message` event                                    | Lower — one per loop iteration           |

**When to use:**

| Use microtasks                               | Use macrotasks                                              |
| -------------------------------------------- | ----------------------------------------------------------- |
| Promise reactions, `queueMicrotask`          | Timers, user events, I/O completion                         |
| DOM observation (`MutationObserver`)         | Debounced UI updates (often `setTimeout`)                   |
| Breaking up sync work without blocking paint | Animation frames (`requestAnimationFrame` — separate queue) |

#### Common pitfalls

- `requestAnimationFrame` is **not** a microtask — runs before paint, separate from both queues.
- Recursive `queueMicrotask` or promise chains — can freeze the page.
- Assuming `setInterval` fires exactly on interval — delayed if main thread is busy.

**Pluses and minuses:**

| +                                              | −                                               |
| ---------------------------------------------- | ----------------------------------------------- |
| Microtasks batch promise reactions efficiently | Long microtask chains block timers and UI       |
| Macrotasks yield to rendering (browser)        | Harder to reason about mixed timer/promise code |
| Clear priority: promises before `setTimeout`   | Node `nextTick` can starve I/O if abused        |

**Examples:**

```js
Promise.resolve().then(() => console.log("micro 1"));
Promise.resolve().then(() => console.log("micro 2"));
setTimeout(() => console.log("macro"), 0);
// micro 1, micro 2, macro
```

**Sources:** [MDN queueMicrotask](https://developer.mozilla.org/en-US/docs/Web/API/Window/queueMicrotask) · [MDN MutationObserver](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver)

</details>

<details>
  <summary>Execution order & browser queues</summary>

### Execution order

**Definition:** One event-loop iteration follows a fixed order: run sync code until the stack is empty, drain **all** microtasks, run **one** macrotask, then optionally render a frame (browser).

**How it works:**

- Sync code always runs first — top to bottom until stack empty.
- All pending microtasks run — including ones scheduled by earlier microtasks in the same turn.
- Exactly **one** macrotask runs per iteration (unless browser batches).
- Browser: after macrotask, may run **render steps** (style, layout, paint) if enough time passed (~16ms for 60fps).
- Loop repeats until all queues are empty and no new work arrives.

- Sync code always runs first.
- All pending microtasks run before the next macrotask.
- A microtask that schedules another microtask — the new one also runs in the same turn.
- Browser may have separate render queues between macrotasks.

**When to use:**

| Situation                          | Expected behavior                                                  |
| ---------------------------------- | ------------------------------------------------------------------ |
| Async output puzzles               | Trace: sync → microtasks → one macro                               |
| Batch DOM reads/writes             | Read in sync, write after microtasks or in `requestAnimationFrame` |
| `Promise.then` inside `setTimeout` | Macro runs first, then its microtasks                              |
| Performance tuning                 | Keep microtask queues short                                        |

#### Common pitfalls

- Forgetting that `setTimeout` inside a microtask still schedules a **future** macrotask (4 runs after 2 and 6).
- `new Promise(executor)` runs the executor **synchronously** — only `.then` is async.

**Pluses and minuses:**

| +                                                 | −                                          |
| ------------------------------------------------- | ------------------------------------------ |
| Deterministic ordering for interview puzzles      | Real apps mix many sources — hard to trace |
| Microtasks keep promise chains atomic per turn    | Render can be delayed by microtask storms  |
| One macrotask per turn allows paint opportunities | Order differs slightly in Node vs browser  |

**Examples:**

```js
console.log(1);
setTimeout(() => console.log(2), 0);
Promise.resolve().then(() => console.log(3));
Promise.resolve().then(() => setTimeout(() => console.log(4)));
Promise.resolve().then(() => console.log(5));
setTimeout(() => console.log(6), 0);
console.log(7);
// 1, 7, 3, 5, 2, 6, 4
```

**Sources:** [javascript.info/event-loop](https://javascript.info/event-loop)

</details>

## queueMicrotask & process.nextTick

<details>
  <summary>queueMicrotask & process.nextTick (Node)</summary>

### queueMicrotask & process.nextTick

**Definition:** `queueMicrotask(fn)` schedules a microtask in browser and Node. Node's `process.nextTick(fn)` schedules a callback with **higher priority** than other microtasks.

**How it works:**

- `queueMicrotask(callback)` — runs after current sync code, in microtask queue, before next macrotask.
- Same queue family as `Promise.then` reactions.
- Node `process.nextTick(callback)` — separate queue drained **before** promise microtasks and before I/O.
- Intended for deferring work without delaying I/O; abusing `nextTick` can starve the event loop.
- Browser: `queueMicrotask` on `globalThis` or `window`.

| API                | Environment   | Priority               |
| ------------------ | ------------- | ---------------------- |
| `queueMicrotask`   | Browser, Node | Standard microtask     |
| `Promise.then`     | Both          | Standard microtask     |
| `process.nextTick` | Node only     | Above other microtasks |

**When to use:**

| Use                                   | Avoid                                                         |
| ------------------------------------- | ------------------------------------------------------------- |
| Run after DOM update, before paint    | Long-running loops in microtasks                              |
| Node: defer callback before I/O phase | `nextTick` recursion instead of `setImmediate` for heavy work |
| Polyfill promise reaction timing      | Replacing `Promise.then` for readability                      |

#### Common pitfalls

- Infinite `process.nextTick` chain in Node — blocks I/O indefinitely.
- Assuming `queueMicrotask` runs before `Promise.then` — same queue, order of scheduling matters.
- Using `setTimeout(0)` when microtask is enough — adds unnecessary macrotask delay.

**Pluses and minuses:**

| +                                               | −                                      |
| ----------------------------------------------- | -------------------------------------- |
| Explicit microtask scheduling                   | Easy to starve macrotasks if recursive |
| `nextTick` runs before promise handlers in Node | `nextTick` not available in browser    |
| Break up sync work without `setTimeout(0)`      | Overuse blocks I/O and timers          |
| Predictable ordering within microtask tier      | Differs between Node and browser       |

**Examples:**

```js
console.log("sync");

queueMicrotask(() => console.log("microtask"));

Promise.resolve().then(() => console.log("promise microtask"));

// Node only:
// process.nextTick(() => console.log('nextTick')); // runs before promise microtask

setTimeout(() => console.log("macrotask"), 0);

// sync → nextTick (Node) → microtask → promise microtask → macrotask
```

**Sources:** [MDN queueMicrotask](https://developer.mozilla.org/en-US/docs/Web/API/Window/queueMicrotask) · [javascript.info/microtask-queue](https://javascript.info/microtask-queue) · [Node process.nextTick](https://nodejs.org/api/process.html#processnexttickcallback-args) · [8_nodejs.md](./8_nodejs.md)

</details>

---

Interview Q&A and coding tasks live in [JS — Interview](./3_js_interview.md).
