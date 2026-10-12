# JS — Additional

Intl, RegExp, Signals (TC39), and `eval`. Everyday language fundamentals live under Core → Language.

[← Back to JS hub](./3_js.md)

### Related docs

- [Modules](./3_js_modules.md) · [Export and Import](./3_js_import_export.md) · [Dynamic imports](./3_js_dynamic_import.md) · [JS Language](./3_js_core.md) · [Variables](./3_js_variables.md) · [Data Types](./3_js_data_types.md) · [Data Structures](./3_js_data_structures.md) · [Operators](./3_js_operators.md) · [Loops](./3_js_loops.md) · [JS Arrays](./3_js_array.md) · [JS Functions](./3_js_functions.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md) · [JS Async](./3_js_async.md) · [JS Interview](./3_js_interview.md)
- [Computer science](./7_special_cs.md)

### Main concepts

| Area        | Topics                        |
| ----------- | ----------------------------- |
| **Intl**    | Locale-aware formatting       |
| **RegExp**  | Patterns and flags            |
| **Signals** | TC39 reactive cells (Stage 1) |
| **Avoid**   | `eval`                        |

### Sections

> Theory `<details>` blocks follow: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**. **Definition** is required; other sections are optional.

- [Intl](#intl)
- [RegExp](#regexp)
- [Signals](#signals)
- [Avoid](#avoid)
- [Interview](./3_js_interview.md)

Sibling topics: [Modules](./3_js_modules.md) · [Export and Import](./3_js_import_export.md) · [Dynamic imports](./3_js_dynamic_import.md) · [Language](./3_js_core.md) · [Operators](./3_js_operators.md) · [Loops](./3_js_loops.md).

<a name="intl"></a>

## Intl

<details>
  <summary>Intl API</summary>

### Intl API

**Definition:** Locale-aware formatting for dates, numbers, collation, and plural rules — built into JS engines.

**How it works:**

- Create formatter once: `new Intl.NumberFormat(locale, options)`.
- Call `.format(value)` per value — caches locale data internally.
- `Intl.Collator` for locale-aware string sort; `Intl.DateTimeFormat` for dates.

**When to use:** Currency display, date in user's locale, sorting names per language rules.

**Pluses and minuses:**

| +                             | −                               |
| ----------------------------- | ------------------------------- |
| Correct locale rules          | Bundle size N/A (built-in)      |
| Better than manual formatting | Options API verbose             |
| Timezone/locale aware         | Must pass correct locale string |

**Examples:**

```js
new Intl.NumberFormat("de-DE", { style: "currency", currency: "EUR" }).format(
  99.9,
);
// "99,90 €"

new Intl.DateTimeFormat("en-US", { dateStyle: "medium" }).format(new Date());
```

**Sources:** [javascript.info/intl](https://javascript.info/intl)

</details>

<a name="regexp"></a>

## RegExp

<details>
  <summary>RegExp (advanced basics)</summary>

### RegExp (advanced basics)

**Definition:** Regular expressions match patterns in strings via `/pattern/flags` or `new RegExp(pattern, flags)`.

**How it works:**

- Flags: `g` global, `i` case-insensitive, `m` multiline, `s` dotAll, `u` unicode.
- Methods: `.test(str)`, `.exec(str)` (with capture groups), `str.match`, `str.replace`, `str.split`.
- Capturing groups `(...)` and non-capturing `(?:...)`.

**When to use:** Input validation (email-ish), extract tokens, replace patterns. Prefer dedicated parsers for complex formats (HTML, JSON).

**Pluses and minuses:**

| +                     | −                              |
| --------------------- | ------------------------------ |
| Powerful text parsing | Hard to read/maintain          |
| Built-in, fast        | Catastrophic backtracking risk |
|                       | Unicode edge cases             |

**Examples:**

```js
/^\w+@\w+\.\w+$/.test("a@b.co"); // basic email shape
"a1b2".replace(/\d/g, "#"); // "a#b#"
const m = /(\d+)-(\d+)/.exec("2024-07"); // groups
```

**Sources:** [javascript.info/regular-expressions](https://javascript.info/regular-expressions)

</details>

<a name="signals"></a>

## Signals

<details>
  <summary>Signals (TC39)</summary>

<a name="signals"></a>

### Signals (TC39)

**Definition:** A **signal** is a first-class cell of state that notifies dependents of changes. You read with `.get()`, write with `.set()`, and the engine builds a DAG of who depends on whom. The TC39 proposal standardizes that **graph + auto-tracking**, not an app-facing `effect()` API. Stage 1 since April 2024.

**How it works:**

1. **`Signal.State`** — writable cell. **`Signal.Computed`** — lazy formula. **`effect`** is _not_ in the proposal; frameworks build it on `Signal.subtle.Watcher`.
2. **Auto-tracking:** thread-local `computing`. While a Computed callback runs, every `.get()` registers that node as a source. Nested Computed B that reads `light` owns the edge; parent A depends only on B. Source lists rebuild each evaluation (**dynamic dependencies**).
3. **Four colors:** `clean` (cached, fresh), `checked` (an ancestor _might_ have changed), `dirty` (definitely stale or never read), `computing` (callback running).
4. **Push (`set`):** if `equals` (default `Object.is`) says unchanged, stop. Else mark direct sinks `dirty`, farther sinks `checked` (never downgrade `dirty`), put Watchers `pending`, call `notify` **synchronously**. No recompute yet.
5. **Pull (`get`):** if `clean`, return cache. Else walk deepest-leftmost `dirty` source, recompute, compare. Unchanged → `checked` descendants go `clean` (wave dies). Changed → paint them `dirty` and continue. That is **glitch-free**.
6. Errors cache like values (rethrow until a source changes). Recursive self-read is an error. During `notify` the engine is **frozen** — no read/write; `untrack` does not lift that.
7. **`Signal.subtle`:** `Watcher`, `untrack`, `watched`/`unwatched` (lazy subscribe to `resize` etc.), introspection for DevTools. Watchers keep sources alive — missing `unwatch()` leaks.

```
light (State) → canGo (Computed) → message (Computed) → effect (framework)
set() paints left → right.  get() recomputes right ← left, only if asked.
```

| Color       | Meaning                | Next `.get()`                |
| ----------- | ---------------------- | ---------------------------- |
| `clean`     | cache is truth         | return cache                 |
| `checked`   | maybe stale (indirect) | walk sources; may stay clean |
| `dirty`     | must recompute         | run callback, then `equals`  |
| `computing` | callback running       | reading self → error         |

Imperative `let light = "red"` plus a manual `render()` couples state to the DOM and recomputes even when the visible result did not change. Pub/sub fixes “who to tell” but you still subscribe by hand, and `render` must know it depends on `light` even if it only cares about `message`. Signals make the graph automatic.

**When to use:**

Fine-grained UI state (Angular `signal()`, Vue `ref()`, Solid `createSignal()`, Svelte 5 `$state()`, Preact, Qwik, MobX). Shared reactive _models_ across frameworks if the primitive ships. Not for one-shot events (click, socket) — use events. Not RxJS: a signal is one **current** value, not a stream over time. **`AbortSignal` is unrelated.**

#### Common pitfalls

Treating `.get`/`.set` as Vue `.value` “compat” — authors avoided that on purpose. Scheduling work inside `notify`. Forgetting `unwatch()`. Reading a real dependency inside `untrack` (graph goes stale). Assuming one global effect scheduler.

**Pluses and minuses:**

| +                                           | −                                                      |
| ------------------------------------------- | ------------------------------------------------------ |
| Auto subscriptions, no manual pub/sub       | Not in the language yet (Stage 1)                      |
| Lazy + memoized; `equals` stops wasted work | Scheduling (when to run effects) is framework-specific |
| Glitch-free DAG vs naive notify storms      | `untrack` / frozen `notify` are easy to misuse         |

**Examples:**

```js
const light = new Signal.State("red");
const canGo = new Signal.Computed(() => light.get() === "green");
const message = new Signal.Computed(() => (canGo.get() ? "go" : "stop"));

light.get(); // "red"
canGo.get(); // false  — first pull: callback runs, sources = [light]
message.get(); // "stop"

light.set("yellow");
// push: light changed → canGo dirty, message checked. No callbacks yet.
message.get();
// pull: canGo is dirty → recompute false (same as cache) → equals true
// → message stays "stop", DOM would not need a write
```

```js
const showDetails = new Signal.State(false);
const name = new Signal.State("Pumpkin");
const details = new Signal.State("Halloween classic");

const label = new Signal.Computed(() => {
  if (!showDetails.get()) return name.get();
  return `${name.get()}: ${details.get()}`;
});

label.get(); // "Pumpkin" — sources: showDetails, name
details.set("new"); // label does not recompute: details is not a source
showDetails.set(true);
label.get(); // "Pumpkin: new" — now sources include details
```

```js
const light = new Signal.State("red");
const cars = new Signal.Computed(() => light.get() === "green");
const pedestrians = new Signal.Computed(() => light.get() !== "green");
const board = new Signal.Computed(
  () => `cars:${cars.get()} walk:${pedestrians.get()}`,
);

light.set("green");
board.get();
// Naive pub/sub: board runs twice (cars then pedestrians).
// Signals: both parents dirty; board pulled once after they settle.
```

```js
const list = new Signal.State([1, 2, 3], {
  equals: (prev, next) => prev.length === next.length,
});
list.set([9, 8, 7]); // length 3 === 3 → no dirty paint
list.set([1]); // length changed → sinks dirty
```

```js
let pending = false;
const watcher = new Signal.subtle.Watcher(() => {
  if (pending) return;
  pending = true;
  queueMicrotask(() => {
    pending = false;
    for (const s of watcher.getPending()) s.get();
    watcher.watch(); // re-arm; otherwise next set() is missed
  });
});

function effect(callback) {
  let cleanup;
  const computed = new Signal.Computed(() => {
    cleanup?.();
    cleanup = callback();
  });
  watcher.watch(computed);
  computed.get();
  return () => {
    cleanup?.();
    watcher.unwatch(computed);
  };
}

effect(() => {
  element.innerText = message.get();
});
```

`notify` runs **inside** `set()` — only schedule, never recompute there. After the microtask, `getPending()` is the live dirty list.

```js
const debugMode = new Signal.State(false);
const items = new Signal.State([1, 2]);
const total = new Signal.Computed(() => {
  const n = items.get();
  if (Signal.subtle.untrack(() => debugMode.get())) {
    console.log("recompute total");
  }
  return n.length;
});
// debugMode flips do not dirty total — by design
```

**Thoughts:** `set()` only colors the graph; `get()` is the only place work happens; effects stay in frameworks because **when** to rerun is not one size.

**Sources:** [proposal-signals README](https://github.com/proposal-signals/proposal-signals) · [signal-polyfill](https://github.com/proposal-signals/signal-polyfill) · Doka — Signals (Rusakov, 31 Aug 2026)

</details>

<a name="avoid"></a>

## Avoid

<details>
  <summary>eval (anti-pattern)</summary>

### eval (anti-pattern)

**Definition:** `eval(string)` executes a string as code in the current lexical scope.

**How it works:**

- Parses and runs string as JS in current scope — can access local variables.
- Prevents JIT optimization; same security risks as `new Function` with user input.

**When to use:** Do not use with user input. Prefer `JSON.parse`, template engines, or proper parsers.

**Examples:**

```js
const expr = "2 + 2";
eval(expr); // 4 — same scope as caller; never pass user input

// Safer dynamic math: dedicated parser library, not eval
JSON.parse('{"a":1}'); // data only, not code
```

**Pluses and minuses:**

| +                      | −                                            |
| ---------------------- | -------------------------------------------- |
| Rare dynamic DSL cases | Security vulnerability (XSS → RCE)           |
|                        | Breaks JIT optimization in surrounding scope |
|                        | Hard to debug and audit                      |

**Sources:** [javascript.info/eval](https://javascript.info/eval) · [`new Function` (avoid)](./3_js_functions.md#new-function-avoid)

</details>
