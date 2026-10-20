# React — Performance

Applied **render and network performance** — measure first, then fix waterfalls, bundles, and wasted renders.

[← Back to hub](./4_react.md)

### Related docs

- [React — Build tooling](./4_react_build.md) · [React — Patterns](./4_react_patterns.md) · [React — Concurrent UI](./4_react_concurrent.md) · [CSS — Reflow & paint](./2_css_architecture.md#reflow-repaint-composite) · [Browser pipeline](./1_html_structure.md#browser-rendering-pipeline) · [Profiler](./4_react_components.md#profiler)

### Main concepts

| Concept                       | Covers                                    |
| ----------------------------- | ----------------------------------------- |
| **Measure before memo**       | Profiler, React DevTools, real metrics    |
| **Request waterfalls**        | Parallelize fetches; Suspense + `use`     |
| **Render discipline**         | Pure components, context split, list keys |
| **Layout, paint, composite**  | Browser pipeline after React commits DOM  |
| **Code splitting**            | `lazy`, route-level chunks, bundle size   |
| **Expensive work off render** | `useMemo`, Web Workers, debounce          |
| **Memoization toolkit**       | `memo`, `useMemo`, `useCallback`          |
| **Production checklist**      | Bundle analyze, hydration, INP            |

### Sections

- [Overview](#overview)
- [Measurement](#measurement)
- [Network & data](#network-data)
- [Render & bundle](#render-bundle) — [Layout, paint, composite](#layout-paint-and-composite) · [Expensive work off render](#expensive-work-off-render)
- [Memoization toolkit](#memoization-toolkit)

---

## Overview

<details>
  <summary>Overview</summary>

### Overview

**Definition:** React performance work targets **user-visible latency** (INP, LCP) and **wasted CPU** (re-renders, large lists) — not micro-optimizing every component.

**How it works:** Profile → find largest slice (network waterfall, one heavy subtree, huge bundle) → apply one pattern from this page → measure again.

**When to use:** Slow dashboards, janky typing, Lighthouse regressions after a feature ship.

**Performance triage (order matters):**

| Symptom              | Likely layer        | First read                          |
| -------------------- | ------------------- | ----------------------------------- |
| Slow first paint     | Network / bundle    | [Request waterfalls](#request-waterfalls) · [Code splitting](#code-splitting) |
| Janky typing         | Render / state      | [Render discipline](#render-discipline) · [Concurrent UI](./4_react_concurrent.md) |
| Huge table scroll    | List algorithm      | [List rendering](./4_react_algorithms.md#list-rendering-algorithm) |
| Random re-renders    | Context / props     | [Context splitting](./4_react_patterns.md#context-splitting) |

**Thoughts:** Measure on **real devices** and **production builds** — dev mode and HMR distort timings.

**See also:** [When to memoize](./4_react_patterns.md#when-to-memoize) · [List rendering algorithm](./4_react_algorithms.md#list-rendering-algorithm)

**Sources:**

[react.dev/learn/render-and-commit](https://react.dev/learn/render-and-commit)

Measure before memo

</details>

### Measurement

<details>
  <summary>Measure before memo</summary>

### Measure before memo

**Definition:** Use **React DevTools Profiler** and browser **Performance** panel before adding `memo` everywhere. Optimize what actually costs time.

**How it works:**

1. Record interaction (filter table, open modal)
2. Inspect **commit duration** and **which components** re-rendered
3. Optional: wrap subtree in `<Profiler id="Sidebar" onRender={onRender} />` in dev
4. Fix root cause (unstable props, context blast radius) then memo leaves if needed

**When to use:** “App feels slow” with no hypothesis; before interview answer “I’d use useMemo”.

**What to look for in Profiler:**

| Signal                          | Likely fix                                      |
| ------------------------------- | ----------------------------------------------- |
| One leaf takes most commit time | Memoize leaf + stabilize props, or simplify JSX |
| Whole tree on each keystroke    | Colocate state, split context, defer filter     |
| Long “render” but cheap JSX     | Check expensive child `useMemo` deps or logging |

**Thoughts:** Optimize the **interaction you measured**, not a hypothetical hot path.

**Sources:**

[react.dev/reference/react/Profiler](https://react.dev/reference/react/Profiler)

Production checklist

</details>

<details>
  <summary>Production checklist</summary>

### Production checklist

**Definition:** Short list before shipping performance-sensitive UI.

**How it works:**

| Check     | Action                                                |
| --------- | ----------------------------------------------------- |
| Bundle    | Analyze with framework bundle tool; lazy heavy routes |
| Lists     | Stable keys; virtualize large tables                  |
| Images    | Sized, modern formats (framework Image)               |
| Fonts     | `font-display`, subset                                |
| Hydration | No server/client markup drift                         |
| INP       | Defer heavy work with `startTransition`               |
| Errors    | Boundaries around heavy widgets — failed chart ≠ blank app ([Resilience](./4_react_resilience.md)) |

**Common regressions after ship:**

- New global Context value recreated each render → whole app re-renders
- `useEffect` fetch per row instead of one list query
- Importing full icon/chart SDK on the landing route

**See also:** [Core Web Vitals](./6_web_api_security.md) · [Production checklist patterns in Ecosystem](./4_react_ecosystem.md#10-performance)

**Sources:**

[web.dev — INP](https://web.dev/articles/inp)

Request waterfalls

</details>

### Network & data

<details>
  <summary>Request waterfalls</summary>

### Request waterfalls

**Definition:** **Waterfall** = request B starts only after A finishes because code `await`s sequentially in one component or effect chain.

**How it works:**

- Bad: parent `await user` then child `await posts` in series on server/client
- Good: `Promise.all([fetchUser(), fetchPosts()])` or parallel Server Components
- UI: nested **Suspense** so fast sections paint while slow fetches run

**When to use:** Page with header + sidebar + main all fetching; RSC apps with multiple `async` children.

**Examples:**

```tsx
const [user, posts] = await Promise.all([getUser(id), getPosts(id)]);
```

**See also:** [Concurrent UI](./4_react_concurrent.md) · [TanStack Query](./4_react_tanstack.md)

**Sources:**

[react.dev/reference/react/Suspense](https://react.dev/reference/react/Suspense)

Prefetch & preload

</details>

<details>
  <summary>Prefetch & preload</summary>

### Prefetch & preload

**Definition:** Start loading **before** the user navigates — hover on link, viewport prefetch, router `preload`.

**How it works:** Framework routers (Next.js, TanStack Router) prefetch route JS + data. Pair with `link` `prefetch` props or custom `onMouseEnter` + query `prefetchQuery`.

**When to use:** Known next click (pagination, tabs, wizard steps).

**How to prefetch safely:**

- Prefetch **data** only when route is likely (hover) — avoid hammering API on every link in a long list
- Pair with **staleTime** in TanStack Query so prefetch is not immediately refetched on navigation
- For RSC, framework prefetch often includes flight payload — watch payload size on mobile

**Examples:**

```tsx
// TanStack Query — warm cache on hover
function UserLink({ id }: { id: string }) {
  const qc = useQueryClient();
  return (
    <Link
      to={`/users/${id}`}
      onMouseEnter={() =>
        qc.prefetchQuery({ queryKey: ["user", id], queryFn: () => fetchUser(id) })
      }
    >
      {id}
    </Link>
  );
}
```

**See also:** [Next.js — Routing](./4_nextjs_routing.md) · [Algorithms — Pagination](./4_react_algorithms.md#pagination-infinite-scroll)

**Sources:**

[react.dev/learn/synchronizing-with-effects](https://react.dev/learn/synchronizing-with-effects)

Render discipline

</details>

### Render & bundle

<details>
  <summary>Render discipline</summary>

### Render discipline

**Definition:** Keep renders **predictable** — pure presentational leaves, split context, avoid parent state that changes every frame unless needed.

**How it works:**

- Lift only shared state; derive the rest
- Do not put `{ ...config, tick: Date.now() }` on context value each render
- Split “layout shell” from “data table” so typing in search does not re-render chart

**Anti-patterns:**

| Smell                         | Fix                                      |
| ----------------------------- | ---------------------------------------- |
| `value={{ user, theme, cart }}` on one provider | Split providers or memoize slices |
| Inline `onClick={() => ...}` on every row to memoized row | `useCallback` or row handles own click |
| Logging in render             | Move to effect or dev-only guard         |

**When to use:** Profiler shows parent + 500 children on each keystroke.

**See also:** [Context splitting](./4_react_patterns.md#context-splitting) · [When to memoize](./4_react_patterns.md#when-to-memoize)

**Sources:**

[react.dev/learn/scaling-up-with-reducer-and-context](https://react.dev/learn/scaling-up-with-reducer-and-context)

</details>

<details>
  <summary>Layout, paint, and composite</summary>

### Layout, paint, and composite

**Definition:** После того как React закоммитил обновления DOM, браузер выполняет **layout** (reflow), **paint** и часто **composite** на GPU. Тяжёлый layout/paint даёт jank и бьёт по **INP**, даже когда число re-render в React невелико.

**How it works:**

| Stage | Trigger (examples) | React tie-in |
| ----- | ------------------ | ------------ |
| **Layout (reflow)** | width/height, DOM insert, чтение `offsetHeight` после write | Списки без virtualization; измерение каждого item в render |
| **Paint** | color, shadow, visibility | Большой `box-shadow` на scroll containers |
| **Composite** | `transform`, `opacity` (часто) | Предпочитать CSS transitions вместо `setState` каждый frame |

**When to use:** Profiler показывает быстрый React phase, а **Performance** panel — длинный **Layout** или **Paint**; CLS от поздних layout shifts.

**Examples:**

```tsx
// Prefer transform for move animations (composite-friendly)
<div style={{ transform: `translateX(${x}px)` }} />

// Avoid: layout thrashing — read then write in a loop
rows.forEach((el) => {
  const h = el.offsetHeight; // read forces layout
  el.style.height = `${h + 10}px`; // write
});
```

**Thoughts:** React batching не батчит browser layout — группируйте DOM read/write в `requestAnimationFrame` или одном effect. Подробнее → [CSS — Reflow, repaint, composite](./2_css_architecture.md#reflow-repaint-composite) · [HTML — rendering pipeline](./1_html_structure.md#browser-rendering-pipeline).

**Sources:** [MDN — Rendering performance](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Rendering) · [web.dev — INP](https://web.dev/articles/inp)

</details>


<details>
  <summary>Expensive work off render</summary>

### Expensive work off render

**Definition:** Keep render **pure and fast** — move heavy CPU (parse CSV, layout math, crypto) to **`useMemo`**, **Web Workers**, **debounce**, or **server/RSC** so input stays under INP budgets.

**How it works:**

| Work type              | Where to run                         |
| ---------------------- | ------------------------------------ |
| Pure derive from props | `useMemo` with correct deps          |
| Parse huge file        | Worker + `postMessage` + progress UI |
| Filter 50k rows        | Server query or virtualization first |
| Animation              | CSS / `requestAnimationFrame`, not `setState` per frame |

**When to use:** Profiler shows long “render” phase; main thread blocked on JSON.parse in component body.

**Pitfalls:** `useMemo` with wrong deps → stale results; memoizing cheap ops → wasted compare cost.

**See also:** [Measure before memo](#measure-before-memo) · [useTransition](./4_react_concurrent.md#transitions) · [Client filter & sort](./4_react_algorithms.md#client-filter-sort)

**Sources:**

[react.dev/reference/react/useMemo](https://react.dev/reference/react/useMemo)

Code splitting

</details>

<details>
  <summary>Code splitting</summary>

### Code splitting

**Definition:** Load JS for a route or widget **only when needed** via dynamic `import()` and `React.lazy`.

**How it works:**

```tsx
const Admin = lazy(() => import("./AdminPanel"));

<Suspense fallback={<Spinner />}>
  <Admin />
</Suspense>;
```

Framework file-based routing usually splits automatically; use `lazy` for heavy optional panels (charts, editors).

**When to use:** Admin-only modules, PDF viewer, map SDK.

**Pitfalls:**

- **Waterfall after split** — route loads JS, then component mounts and fetches — parallelize data + code ([Request waterfalls](#request-waterfalls))
- **Huge shared chunk** — audit why `vendor` grew; dynamic import inside shared module may not split as expected
- **Missing Suspense** — `lazy` throws if boundary absent

**Interview angle:** Split by **route** first; split by **feature flag** second; avoid dozens of tiny chunks (HTTP overhead).

**See also:** [`lazy`](./4_react_apis.md#lazy) · [Suspense](./4_react_components.md#suspense) · [Concurrent UI](./4_react_concurrent.md#suspense-boundaries)

**Sources:**

[react.dev/reference/react/lazy](https://react.dev/reference/react/lazy)

</details>

---

## Memoization toolkit

<details>
<summary>memo, useMemo, and useCallback</summary>

**Definition:** **Memoization** в React пропускает работу, когда входы не изменились: **`React.memo`** для компонентов, **`useMemo`** для дорогих derived values, **`useCallback`** для стабильных ссылок на функции, передаваемых в memoized children.

**How it works:**

| Tool | What it caches | Default compare |
| ---- | -------------- | ----------------- |
| `memo(Component)` | Re-render компонента | Shallow `Object.is` на каждом prop |
| `useMemo(fn, deps)` | Return value `fn` | Перезапуск, когда любой dep меняется (`===`) |
| `useCallback(fn, deps)` | Identity функции | То же, что `useMemo(() => fn, deps)` |

**Order of operations (recommended):**

1. [Measure before memo](#measure-before-memo) — Profiler или React DevTools
2. Исправьте **architecture** — colocate state, split context ([Render discipline](#render-discipline))
3. Стабилизируйте props — `useCallback` для handlers, передаваемых в `memo` rows
4. Memoize только **expensive** pure subtrees или calculations

**When to use:** Большие списки, где parent часто re-renders, но row props стабильны; тяжёлый derive (filter/sort тысяч items), когда deps меняются редко.

**Pluses and minuses:**

| + | − |
| - | - |
| Сокращает wasted reconciliation | Неверные deps → stale UI |
| Хорошо с virtualized lists | `memo` на всём добавляет compare overhead |
| | Unstable inline `{}` / `() =>` props ломают `memo` |

**Examples:**

```tsx
import { memo, useCallback, useMemo, useState } from "react";

type RowProps = { id: string; label: string; onPick: (id: string) => void };

const Row = memo(function Row({ id, label, onPick }: RowProps) {
  return (
    <button type="button" onClick={() => onPick(id)}>
      {label}
    </button>
  );
});

function Table({ items }: { items: { id: string; label: string }[] }) {
  const [query, setQuery] = useState("");
  const filtered = useMemo(
    () => items.filter((i) => i.label.includes(query)),
    [items, query],
  );
  const onPick = useCallback((id: string) => {
    console.log("picked", id);
  }, []);
  return (
    <>
      <input value={query} onChange={(e) => setQuery(e.target.value)} />
      {filtered.map((i) => (
        <Row key={i.id} id={i.id} label={i.label} onPick={onPick} />
      ))}
    </>
  );
}
```

**Thoughts:** React 19 **Compiler** может auto-memoize в поддерживаемых setups — всё равно профилируйте. API reference: [`memo`](./4_react_apis.md), [When to memoize (patterns)](./4_react_patterns.md#when-to-memoize).

**Sources:** [react.dev/reference/react/memo](https://react.dev/reference/react/memo) · [useMemo](https://react.dev/reference/react/useMemo) · [useCallback](https://react.dev/reference/react/useCallback)

</details>
