# React — Algorithms

Step-by-step **UI logic** in React — filtering, debouncing, mutations, pagination, and scaling lists.

[← Back to hub](./4_react.md)

### Related docs

- [React — Patterns](./4_react_patterns.md) · [React — Performance](./4_react_perf.md) · [React — Concurrent UI](./4_react_concurrent.md) · [React — Resilience](./4_react_resilience.md) · [Hooks](./4_react_hooks.md) · [TanStack](./4_react_tanstack.md) · [Core](./4_react_core.md)

### Main concepts

| Concept                          | Covers                                   |
| -------------------------------- | ---------------------------------------- |
| **Client filter & sort**         | Derive rows in render; stable sort keys  |
| **Debounced search**             | Pause before network or heavy filter     |
| **Throttle vs debounce**         | Rate-limit scroll/resize vs input        |
| **Fetch with abort**             | Cancel stale requests                    |
| **Optimistic updates**           | Patch UI first, rollback on error        |
| **Pagination & infinite scroll** | Offset, cursor, `fetchNext`              |
| **useTransition for slow UI**    | Keep input responsive during heavy work  |
| **List rendering algorithm**     | Keys, virtualization, context split      |
| **Tree flatten algorithm**       | Visible rows from nested expand/collapse |
| **Race-safe request id**         | Ignore stale async results               |
| **Immutable nested update**      | Copy-on-write state patches              |
| **Reorder list algorithm**       | Drag-and-drop array move                 |
| **Wizard step algorithm**        | Multi-step flows with validation         |
| **Functional setState**          | Avoid stale closures                     |
| **Batch updates**                | React 18 automatic batching              |
| **Group-by aggregation**         | Reduce rows for dashboards               |
| **Merge paginated pages**        | Append API pages without duplicates      |

### Sections

- [Overview](#overview)
- [Data & filter algorithms](#data-filter-algorithms)
- [Async UI algorithms](#async-ui-algorithms)
- [List algorithms](#list-algorithms)
- [State update algorithms](#state-update-algorithms)

---

## Overview

<details>
  <summary>Overview</summary>

### Overview

**Definition:** **Algorithms** here mean repeatable procedures for UI behavior — when to fire a search, how to paginate, how to render large lists — implemented with state, effects, and framework data tools.

**How it works:** Pair each algorithm with clear **inputs**, **outputs**, and **failure modes** (stale requests, rollback, empty pages). Structural patterns (lifting, composition) live in [Patterns](./4_react_patterns.md).

**When to use:** Autocomplete, feeds, admin tables, optimistic toggles, interview “implement infinite scroll / debounce” tasks.

**Algorithm picker (quick map):**

| User story                         | Start here                                      |
| ---------------------------------- | ----------------------------------------------- |
| Typeahead / search API             | [Debounced search](#debounced-search) + [Fetch with abort](#fetch-with-abort) |
| Like / toggle / reorder            | [Optimistic updates](#optimistic-updates)       |
| Feed or “Load more”                | [Pagination & infinite scroll](#pagination-infinite-scroll) · [Merge paginated pages](#merge-paginated-pages) |
| Huge table on client               | [Client filter & sort](#client-filter-sort) + [useTransition](#usetransition-for-slow-ui) |
| Wrong data after fast tab switch   | [Race-safe request id](#race-safe-request-id)   |
| Nested folders in virtual table    | [Tree flatten algorithm](#tree-flatten-algorithm) |

**Thoughts:** Algorithms answer **when** and **in what order**; [Patterns](./4_react_patterns.md) answer **how to structure components**.

**Sources:**

[react.dev/reference/react/useDeferredValue](https://react.dev/reference/react/useDeferredValue) · [react.dev/learn/rendering-lists](https://react.dev/learn/rendering-lists)

Client filter & sort

</details>

### Data & filter algorithms

<details>
  <summary>Client filter & sort</summary>

### Client filter & sort

**Definition:** Filter and sort collections **during render** (or in `useMemo`) from props/query state — O(n) or O(n log n) per update, no extra `useEffect`.

**How it works:**

1. Keep `query`, `sortKey`, `sortDir` in state (or URL via nuqs)
2. `const visible = useMemo(() => { ... }, [items, query, sortKey])`
3. Normalize strings once (`toLowerCase`) for case-insensitive search
4. Sort with **stable** tie-breaker (e.g. `id`) when keys compare equal

**When to use:** < few thousand rows client-side; move to server pagination when latency or memory hurts.

**Sorting detail:** Use `localeCompare` for strings; for numbers sort `(a, b) => a - b`. When sorting objects, **copy** before `.sort()` — `Array.prototype.sort` mutates in place.

**Pluses and minuses:**

| +                               | −                                     |
| ------------------------------- | ------------------------------------- |
| Instant feedback, no round-trip | Main-thread cost grows with `n`       |
| Simple mental model             | Wrong for huge datasets or PII in URL |

**Examples:**

```tsx
function useFiltered<T>(items: T[], query: string, pick: (item: T) => string) {
  return useMemo(() => {
    const q = query.trim().toLowerCase();
    if (!q) return items;
    return items.filter((item) => pick(item).toLowerCase().includes(q));
  }, [items, query, pick]);
}
```

**See also:** [Debounced search](#debounced-search) · [Thinking in React](./4_react_patterns.md#thinking-in-react)

**Sources:**

[react.dev/learn/thinking-in-react](https://react.dev/learn/thinking-in-react)

Throttle vs debounce

</details>

<details>
  <summary>Throttle vs debounce</summary>

### Throttle vs debounce

**Definition:** **Debounce** runs after input **stops** for `ms` (search). **Throttle** runs at most once per `ms` while events fire (scroll, resize).

**How it works:**

| Technique | Fires when                          | Typical use              |
| --------- | ----------------------------------- | ------------------------ |
| Debounce  | Quiet period after last event       | Search box, autosave     |
| Throttle  | Every `ms` during continuous events | Scroll spy, resize chart |

Do not debounce every keystroke display — only the **expensive** side (API). Keep controlled input instant.

**When to use:** Interview “difference debounce/throttle”; infinite scroll load-more often uses throttle or intersection, not debounce.

**Examples:**

```tsx
// Debounce — only run search after 300 ms quiet
function useDebouncedValue<T>(value: T, ms: number) {
  const [debounced, setDebounced] = useState(value);
  useEffect(() => {
    const id = setTimeout(() => setDebounced(value), ms);
    return () => clearTimeout(id);
  }, [value, ms]);
  return debounced;
}

// Throttle — at most once per 100 ms while scrolling
function useThrottledCallback(fn: () => void, ms: number) {
  const last = useRef(0);
  return useCallback(() => {
    const now = Date.now();
    if (now - last.current >= ms) {
      last.current = now;
      fn();
    }
  }, [fn, ms]);
}
```

**Interview angle:** Debounce = “wait until they stop”; throttle = “sample while they keep going”. Leading vs trailing debounce is a product choice (fire on first key vs last).

**See also:** [Debounced search](#debounced-search) · [Performance — Render discipline](./4_react_perf.md#render-discipline)

**Sources:**

[MDN — setTimeout](https://developer.mozilla.org/en-US/docs/Web/API/Window/setTimeout)

Fetch with abort

</details>

### Async UI algorithms

<details>
  <summary>Fetch with abort</summary>

### Fetch with abort

**Definition:** Attach an **`AbortController`** per request; abort on dependency change or unmount so late responses do not overwrite newer data.

**How it works:**

1. Effect creates `const ac = new AbortController()`
2. `fetch(url, { signal: ac.signal })`
3. Cleanup: `ac.abort()`
4. Ignore `AbortError`; handle real errors in UI

TanStack Query does this via **query keys** and automatic cancellation — prefer for server state.

**When to use:** Manual `useEffect` fetch; typeahead; tab switches mid-request.

**Examples:**

```tsx
useEffect(() => {
  const ac = new AbortController();
  (async () => {
    const res = await fetch(`/api?q=${query}`, { signal: ac.signal });
    const data = await res.json();
    setResults(data);
  })().catch((e) => {
    if (e.name !== "AbortError") setError(e);
  });
  return () => ac.abort();
}, [query]);
```

**See also:** [TanStack Query](./4_react_tanstack.md) · [You might not need an effect](./4_react_hooks.md#you-might-not-need-an-effect)

**Sources:**

[MDN — AbortController](https://developer.mozilla.org/en-US/docs/Web/API/AbortController)

Debounced search

</details>

<details>
  <summary>Debounced search</summary>

### Debounced search

**Definition:** Wait until the user **pauses typing** before firing an expensive filter or network search — reduces work and avoids racey partial queries.

**How it works:**

1. Keep **immediate** `query` in state for the input
2. Keep **debouncedQuery** (custom hook or `useDeferredValue`) for fetch/filter
3. Cancel or ignore stale responses (`AbortController`, query keys in TanStack Query)

**When to use:** Autocomplete, admin tables, any keystroke-triggered API.

**Failure modes:**

| Risk              | Mitigation                                   |
| ----------------- | -------------------------------------------- |
| Stale response    | AbortController or request id                |
| Empty flash       | Keep previous results until new data arrives |
| Slow network feel | Show subtle pending on results, not on input |

**Typical timings:** 200–400 ms debounce for search APIs; 300–500 ms for autosave drafts (product-dependent).

**Examples:**

```tsx
function useDebounced<T>(value: T, ms: number) {
  const [debounced, setDebounced] = useState(value);
  useEffect(() => {
    const id = setTimeout(() => setDebounced(value), ms);
    return () => clearTimeout(id);
  }, [value, ms]);
  return debounced;
}
```

**See also:** [`useDeferredValue`](./4_react_hooks.md#usedeferredvalue) · [TanStack Query](./4_react_tanstack.md)

**Sources:**

[react.dev/reference/react/useDeferredValue](https://react.dev/reference/react/useDeferredValue)

Optimistic updates

</details>

<details>
  <summary>Optimistic updates</summary>

### Optimistic updates

**Definition:** Update the UI **before** the server confirms success; **roll back** on error. Makes mutations feel instant while keeping server as source of truth.

**How it works:**

1. Snapshot current cache/state
2. Apply optimistic patch locally
3. `await` mutation
4. On failure: restore snapshot + show error; on success: reconcile with server response

**When to use:** Likes, toggles, reorder lists — not for irreversible actions without confirmation.

**Edge cases:**

- **Double submit** — disable button while mutation pending
- **Partial failure** — rollback single item, not whole list
- **Server rejects** — show error toast; restore snapshot; optional retry
- **React 19** — consider [`useOptimistic`](./4_react_hooks.md) for built-in pending UI

**Pluses and minuses:**

| +                   | −                                     |
| ------------------- | ------------------------------------- |
| Snappy UX           | Harder debugging if rollback missed   |
| Works offline-first | Needs idempotent server when possible |

**Examples:**

```tsx
async function toggleTodo(id: string) {
  const prev = todos;
  setTodos((t) => t.map((x) => (x.id === id ? { ...x, done: !x.done } : x)));
  try {
    await api.toggleTodo(id);
  } catch {
    setTodos(prev);
  }
}
```

**See also:** [Actions](./4_react_server.md#actions) · [`useOptimistic`](./4_react_hooks.md)

**Sources:**

[react.dev/reference/react/useOptimistic](https://react.dev/reference/react/useOptimistic)

Pagination & infinite scroll

</details>

<details>
  <summary>Pagination & infinite scroll</summary>

### Pagination & infinite scroll

**Definition:** **Pagination** loads fixed pages (cursor/offset); **infinite scroll** appends the next page when the user nears the list end — both need stable **keys** and clear loading/error UI.

**How it works:**

| Approach        | State you track                           | Pitfall                            |
| --------------- | ----------------------------------------- | ---------------------------------- |
| Page buttons    | `page`, `pageSize`, total count           | Double fetch on fast clicks        |
| Cursor          | `cursor`, `hasMore`                       | Stale cursor after mutations       |
| Infinite scroll | `pages[]` or flattened list + `fetchNext` | Scroll jank without virtualization |

Use `IntersectionObserver` or library helpers; pair large lists with [virtualization](./4_react_tanstack.md).

**When to use:** Feeds, tables with thousands of rows, mobile lists.

**Examples (infinite scroll sentinel):**

```tsx
const sentinelRef = useRef<HTMLDivElement>(null);
useEffect(() => {
  const el = sentinelRef.current;
  if (!el || !hasMore) return;
  const io = new IntersectionObserver(
    ([entry]) => {
      if (entry.isIntersecting && !isFetching) fetchNextPage();
    },
    { rootMargin: "200px" },
  );
  io.observe(el);
  return () => io.disconnect();
}, [hasMore, isFetching, fetchNextPage]);

return (
  <>
    {rows.map((row) => <Row key={row.id} row={row} />)}
    <div ref={sentinelRef} aria-hidden />
  </>
);
```

**Pitfalls:** Fetching page 2 twice (guard with `isFetching`); not resetting when `filter` changes; appending without [dedupe](#merge-paginated-pages).

**See also:** [Merge paginated pages](#merge-paginated-pages) · [TanStack Query](./4_react_tanstack.md)

**Sources:**

[MDN — Intersection Observer](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)

useTransition for slow UI

</details>

<details>
  <summary>useTransition for slow UI</summary>

### useTransition for slow UI

**Definition:** Mark non-urgent state updates as **transitions** so React keeps the UI responsive (input stays snappy) while a heavy re-render or filter runs.

**How it works:** `const [pending, startTransition] = useTransition()` — wrap `setState` that triggers slow tree work. Show `pending` on secondary UI (spinner on list, not on text field).

**When to use:** Large client-side filter, tab switch with heavy chart, route prefetch.

**Examples:**

```tsx
const [query, setQuery] = useState("");
const [pending, startTransition] = useTransition();

function onChange(value: string) {
  setQuery(value); // urgent — input
  startTransition(() => {
    setDeferredFilter(value); // heavy table filter
  });
}
```

**See also:** [`startTransition`](./4_react_apis.md#starttransition) · [`useDeferredValue`](./4_react_hooks.md#usedeferredvalue)

**Sources:**

[react.dev/reference/react/useTransition](https://react.dev/reference/react/useTransition)

Async validation algorithm

</details>

<details>
  <summary>Async validation algorithm</summary>

### Async validation algorithm

**Definition:** Validate form fields on blur or submit with **async** rules (username taken, VAT check) without racing older responses.

**How it works:**

1. Track `fieldErrors` + `validating` flags per field
2. On blur/submit, increment **request id** or abort prior check
3. Only apply result if `id` still matches latest
4. Disable submit while `validating` or `isPending` from Actions

Prefer schema libs (Zod) for sync rules; async layer for server checks.

**When to use:** Sign-up, checkout, admin forms with server-side uniqueness.

**Examples:**

```tsx
const checkId = useRef(0);
async function validateUsername(name: string) {
  const id = ++checkId.current;
  setValidating(true);
  try {
    const ok = await api.isUsernameAvailable(name);
    if (id !== checkId.current) return; // newer check started
    setFieldError("username", ok ? undefined : "Taken");
  } finally {
    if (id === checkId.current) setValidating(false);
  }
}
```

**See also:** [Race-safe request id](#race-safe-request-id) · [Controlled forms pattern](./4_react_patterns.md#controlled-forms-pattern) · [Actions](./4_react_server.md#actions)

**Sources:**

[react.dev/reference/react-dom/components/form](https://react.dev/reference/react-dom/components/form)

List rendering algorithm

</details>

### List algorithms

<details>
  <summary>List rendering algorithm</summary>

### List rendering algorithm

**Definition:** Rendering `n` items is **O(n)** reconciliation work per update. Procedure: derive rows, assign stable **keys**, **virtualize** when `n` is large.

**How it works:**

1. Derive `visibleRows` from props + filters (single pass if possible)
2. `rows.map(row => <Row key={row.id} … />)` — avoid `key={index}` when the list reorders
3. If `visibleRows.length` > ~100–500, window with TanStack Virtual or similar
4. Split context so rows do not re-render when unrelated global state changes

**When to use:** Tables, feeds, nested trees; common interview “optimize this list”.

**Key rules (interview):**

- **`key` must be stable** across renders for the same row — use entity `id`, not random `uuid()` per render
- **Avoid `key={index}`** when list reorders, inserts, or filters — causes wrong state reuse
- **Do not compute heavy filter inside each row** — derive `visibleRows` once above the map
- **Split contexts** so row does not subscribe to unrelated global state

**Pluses and minuses:**

| +                          | −                            |
| -------------------------- | ---------------------------- |
| Straightforward map        | O(n) every parent re-render  |
| Virtualization fixes scale | More complex row measurement |

**See also:** [Reconciliation](./4_react_core.md#reconciliation) · [When to memoize](./4_react_patterns.md#when-to-memoize)

**Sources:**

[react.dev/learn/rendering-lists](https://react.dev/learn/rendering-lists) · [react.dev/learn/preserving-and-resetting-state](https://react.dev/learn/preserving-and-resetting-state)

Tree flatten algorithm

</details>

<details>
  <summary>Tree flatten algorithm</summary>

### Tree flatten algorithm

**Definition:** Turn nested data (folders, comments) into a **visible row list** for virtualized tables — expand/collapse toggles which nodes appear.

**How it works:**

1. Walk tree DFS; push `{ id, depth, node }` when ancestors expanded
2. Recompute `visibleRows` when `expandedIds` or data changes (`useMemo`)
3. Row component uses `paddingLeft: depth * 12` for indent
4. Stable `key={node.id}` — never reuse keys across different nodes

**When to use:** File trees, threaded comments, nested admin menus with thousands of nodes.

**Examples:**

```tsx
type FlatRow = { id: string; depth: number; label: string };

function flattenVisible(
  nodes: TreeNode[],
  expanded: Set<string>,
  depth = 0,
): FlatRow[] {
  const out: FlatRow[] = [];
  for (const node of nodes) {
    out.push({ id: node.id, depth, label: node.label });
    if (expanded.has(node.id) && node.children?.length) {
      out.push(...flattenVisible(node.children, expanded, depth + 1));
    }
  }
  return out;
}
```

**Thoughts:** Memoize `flattenVisible` on `[nodes, expanded]`; toggle expand only updates `expanded` Set copy.

**See also:** [List rendering algorithm](#list-rendering-algorithm) · [TanStack Table](./4_react_tanstack.md)

**Sources:**

[react.dev/learn/rendering-lists](https://react.dev/learn/rendering-lists)

</details>

### State update algorithms

<details>
  <summary>Race-safe request id</summary>

### Race-safe request id

**Definition:** Increment a **request id** (or generation counter) when starting async work; apply results only if the id still matches when the Promise resolves.

**How it works:**

```tsx
useEffect(() => {
  let requestId = ++latestId;
  fetch(url).then((data) => {
    if (requestId !== latestId) return; // stale
    setData(data);
  });
}, [url]);
```

Complements [Fetch with abort](#fetch-with-abort) — use both for manual fetches without Query.

**When to use:** Typeahead without AbortController support; rapid filter changes.

**Pluses and minuses:**

| +                              | −                                      |
| ------------------------------ | -------------------------------------- |
| Works without abortable fetch  | Must remember to bump id in cleanup    |
| Simple mental model in effects | Easy to forget in event handlers       |

**Thoughts:** Prefer **AbortController** when the API supports it; request id is a good fallback for legacy endpoints.

**See also:** [Fetch with abort](#fetch-with-abort) · [Debounced search](#debounced-search)

**Sources:**

[react.dev/learn/you-might-not-need-an-effect](https://react.dev/learn/you-might-not-need-an-effect)

Immutable nested update

</details>

<details>
  <summary>Immutable nested update</summary>

### Immutable nested update

**Definition:** Update nested state (object in array, map by id) by **copying** each level — never mutate `state[i].field` in place.

**How it works:**

```tsx
setItems((items) =>
  items.map((item) => (item.id === id ? { ...item, done: !item.done } : item)),
);
```

For deep trees prefer normalized maps `Record<id, Entity>` or Immer inside reducers.

**When to use:** Todo lists, table inline edit, Redux reducers.

**See also:** [State reducer pattern](./4_react_patterns.md#state-reducer-pattern)

**Sources:**

[react.dev/learn/updating-objects-in-state](https://react.dev/learn/updating-objects-in-state)

Reorder list algorithm

</details>

<details>
  <summary>Reorder list algorithm</summary>

### Reorder list algorithm

**Definition:** Move one element from index **`from`** to **`to`** in an array for drag-and-drop UIs.

**How it works:**

```tsx
function reorder<T>(list: T[], from: number, to: number): T[] {
  const next = list.slice();
  const [moved] = next.splice(from, 1);
  next.splice(to, 0, moved);
  return next;
}
```

Persist order to server after drop; optimistic UI optional.

**When to use:** Kanban columns, sortable tables, playlist editors.

**Sources:**

[react.dev/learn/updating-arrays-in-state](https://react.dev/learn/updating-arrays-in-state)

Wizard step algorithm

</details>

<details>
  <summary>Wizard step algorithm</summary>

### Wizard step algorithm

**Definition:** Model a multi-step flow with **`step` index** or **finite state** (`"details" | "payment" | "done"`), validating before `next`.

**How it works:**

1. `const [step, setStep] = useState(0)`
2. `canNext = validate(stepData)` — block advance
3. Back decreases step without losing draft (keep single form state object)
4. Submit only on final step; use [Reset keys](./4_react_resilience.md#reset-keys) to restart

**When to use:** Checkout, onboarding, admin wizards.

**See also:** [State reducer pattern](./4_react_patterns.md#state-reducer-pattern)

**Sources:**

[react.dev/learn/managing-state](https://react.dev/learn/managing-state)

Functional setState

</details>

<details>
  <summary>Functional setState</summary>

### Functional setState

**Definition:** Pass **`setState(prev => ...)`** so updates use the latest state — fixes **stale closures** in timeouts, intervals, and rapid clicks.

**How it works:**

```tsx
// ❌ stale count in async callback
setCount(count + 1);

// ✅ always latest
setCount((c) => c + 1);
```

Same for functional updates in `useReducer` dispatches.

**When to use:** Interview bug with `setCount(count + 1)` inside `setInterval`; batching multiple increments.

**Common pitfalls:**

- Closing over `items` in `setItems([...items, newItem])` after async gap — use `setItems((prev) => [...prev, newItem])`
- Multiple `setCount(count + 1)` in one async handler — only last wins without functional form

**See also:** [Batch updates](#batch-updates) · [Immutable nested update](#immutable-nested-update)

**Sources:**

[react.dev/reference/react/useState#setstate](https://react.dev/reference/react/useState)

Batch updates

</details>

<details>
  <summary>Batch updates</summary>

### Batch updates

**Definition:** React **batches** multiple `setState` calls in the same event handler into **one re-render** (React 18+ batches in promises/timeouts too in many cases).

**How it works:** Do not rely on seeing intermediate state between two `setX` in one handler — read computed values in variables if needed before setting.

**When to use:** Explaining “why only one render” in interviews; avoid unnecessary `flushSync` unless measuring.

**Examples:**

```tsx
function handleClick() {
  setCount((c) => c + 1);
  setFlag(true);
  // React 18+: typically one re-render with both updates applied
}
```

**Escape hatch:** `flushSync` forces synchronous DOM update — use only when integrating non-React widgets or measuring; hurts concurrent scheduling.

**See also:** [Functional setState](#functional-setstate) · [Concurrent UI](./4_react_concurrent.md)

**Sources:**

[react.dev/blog/2022/03/29/react-v18#new-feature-automatic-batching](https://react.dev/blog/2022/03/29/react-v18#new-feature-automatic-batching)

Group-by aggregation

</details>

<details>
  <summary>Group-by aggregation</summary>

### Group-by aggregation

**Definition:** Turn a flat list into **buckets** (by status, category, date) for sectioned UI — single pass O(n) with a `Map` or object accumulator.

**How it works:**

```tsx
function groupBy<T>(items: T[], key: (item: T) => string): Map<string, T[]> {
  const map = new Map<string, T[]>();
  for (const item of items) {
    const k = key(item);
    const bucket = map.get(k);
    if (bucket) bucket.push(item);
    else map.set(k, [item]);
  }
  return map;
}
```

Render: `Array.from(map, ([label, rows]) => <Section key={label} title={label} rows={rows} />)`.

**When to use:** Kanban columns, order lists by status, analytics tables.

**See also:** [Client filter & sort](#client-filter-sort)

**Sources:**

[MDN — Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

Merge paginated pages

</details>

<details>
  <summary>Merge paginated pages</summary>

### Merge paginated pages

**Definition:** When loading **page 2+** of an API, **append** to existing rows while **deduplicating by `id`** in case of overlapping cursors or refetch.

**How it works:**

1. Store `pages: Item[][]` or flat `items` + `pageMeta`
2. On `fetchNextPage`, `setItems((prev) => dedupeById([...prev, ...nextPage]))`
3. TanStack Query: `getNextPageParam` + `useInfiniteQuery` handles merge; set `queryKey` when filters change to reset

**When to use:** Infinite scroll, “Load more” buttons, cursor APIs that can shift after edits.

**Thoughts:** Reset list when **filter** changes — do not merge unrelated queries.

**See also:** [Pagination & infinite scroll](#pagination-infinite-scroll) · [TanStack Query](./4_react_tanstack.md)

**Sources:**

[react.dev/learn/synchronizing-with-effects](https://react.dev/learn/synchronizing-with-effects)

</details>
