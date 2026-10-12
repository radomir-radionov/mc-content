# React — Components

Built-in React components from `react`: Fragment, Profiler, StrictMode, Suspense, Activity, and ViewTransition.

[← Back to hub](./4_react.md)

### Related docs

- [React — Core](./4_react_core.md) · [React — Hooks](./4_react_hooks.md) · [React — Components](./4_react_components.md) · [React — APIs](./4_react_apis.md) · [React — React 19](./4_react_server.md) · [React — Ecosystem](./4_react_ecosystem.md) · [React — Interview](./4_react_interview.md)

### Sections

- [5. Built-in components](#5-built-in-components)

---

## 5. Built-in components

### Built-in components

Built-in components catalog

<details>
  <summary>Built-in components catalog</summary>

### Built-in components catalog

**Definition:** Special JSX components exported from `react` that coordinate rendering, loading UI, dev checks, and (in React 19+) visibility and view transitions — not ordinary UI widgets.

**How it works:**

| Component                                       | Role                                           |
| ----------------------------------------------- | ---------------------------------------------- |
| [`<Fragment>`](#fragment)                       | Group children without a DOM node              |
| [`<Profiler>`](#profiler)                       | Measure render time of a subtree               |
| [`<StrictMode>`](#strictmode)                   | Extra dev-only checks                          |
| [`<Suspense>`](#suspense)                       | Fallback while children suspend                |
| [`<Activity - 19>`](#activity---19)             | Hide/show subtree while keeping state          |
| [`<ViewTransition - 19>`](#viewtransition---19) | Animate enter/exit with Transitions + Suspense |

**When to use:** Reach for these before custom loading or visibility hacks — they plug into Fiber, concurrent rendering, and Suspense.

**Sources:**

[react.dev/reference/react/components](https://react.dev/reference/react/components)

Fragment

</details>

<details>
  <summary>Fragment</summary>

### Fragment

**Definition:** `Fragment` (or shorthand `<>...</>`) groups multiple children without adding an extra DOM node.

**How it works:** JSX requires a single root element. `Fragment` satisfies that rule while rendering nothing to the DOM — children appear as siblings of the parent's other nodes. `<>...</>` compiles to `<React.Fragment>`; use the long form when you need a `key` on the fragment itself (e.g. keyed list items that return multiple elements).

**When to use:** Table rows (`<tr>` pairs), list items with multiple nodes, layout shells where an extra wrapper would break CSS (flex/grid).

**Pluses and minuses:**

| +                                                      | −                                              |
| ------------------------------------------------------ | ---------------------------------------------- |
| Avoids wrapper `<div>` that breaks layout              | Cannot set `className` or events on a fragment |
| Keeps semantic HTML clean                              | Shorthand `<>` does not accept `key`           |
| Required for returning multiple roots from a component |                                                |

**Examples:**

```tsx
function Row({ user }: { user: User }) {
  return (
    <>
      <td>{user.name}</td>
      <td>{user.email}</td>
    </>
  );
}

items.map((item) => (
  <Fragment key={item.id}>
    <dt>{item.term}</dt>
    <dd>{item.definition}</dd>
  </Fragment>
));
```

**See also:** [JSX](./4_react_core.md#jsx) · [Keys](./4_react_core.md#keys)

**Sources:**

[react.dev/reference/react/Fragment](https://react.dev/reference/react/Fragment)

Profiler

</details>

<details>
  <summary>Profiler</summary>

### Profiler

**Definition:** `<Profiler>` wraps a subtree so React calls your `onRender` callback with timing data on every committed update — programmatic performance measurement.

**How it works:**

- **`id`** (string) — labels this profiled region in callbacks and DevTools
- **`onRender`** — `(id, phase, actualDuration, baseDuration, startTime, commitTime, interactions) => void` on each commit that updated the subtree
- Overhead is non-trivial; **profiling is disabled in production builds by default** (use a profiling build when you need prod metrics)

**When to use:** Isolate slow subtrees after React DevTools Profiler points at a branch; log render cost in staging. Not for always-on production telemetry without a profiling build.

**Examples:**

```tsx
function onRender(
  id: string,
  phase: "mount" | "update" | "nested-update",
  actualDuration: number,
) {
  console.log(`${id} (${phase}): ${actualDuration.toFixed(1)}ms`);
}

<Profiler id="App" onRender={onRender}>
  <App />
</Profiler>;
```

**See also:** [Performance](./4_react_ecosystem.md#performance) · [React Fiber](./4_react_core.md#react-fiber)

**Sources:**

[react.dev/reference/react/Profiler](https://react.dev/reference/react/Profiler)

StrictMode

</details>

<details>
  <summary>StrictMode</summary>

### StrictMode

**Definition:** `<StrictMode>` is a dev-only wrapper that runs **extra checks** to surface bugs early. It does **not** render visible UI and has **zero** production impact.

**How it works:**

| Check                 | What happens                                                          |
| --------------------- | --------------------------------------------------------------------- |
| **Double render**     | Component function called twice — catches impure render side effects  |
| **Double effects**    | `useEffect` runs mount → cleanup → mount — catches missing cleanup    |
| **Deprecated APIs**   | Warns about legacy patterns (`findDOMNode`, old context, string refs) |
| **Unsafe lifecycles** | Class component warnings (legacy)                                     |

**When to use:** Wrap `<App />` at the root in development. Fix effect cleanup instead of removing Strict Mode when effects run twice.

**Examples:**

```tsx
<StrictMode>
  <App />
</StrictMode>
```

**See also:** [Rules of React catalog](./4_react_core.md#rules-of-react-catalog) · [Components and Hooks must be pure](./4_react_core.md#components-and-hooks-must-be-pure)

**Sources:**

[react.dev/reference/react/StrictMode](https://react.dev/reference/react/StrictMode)

Suspense

</details>

<details>
  <summary>Suspense</summary>

### Suspense

**Definition:** `<Suspense fallback={…}>` shows **fallback** UI while a child suspends (throws a promise to React) — during client async render, `lazy()` chunks, or `use()` on a pending Promise.

**How it works:**

1. A child suspends → React walks up to the nearest `<Suspense>` boundary
2. That boundary renders `fallback` instead of the suspended subtree
3. When the promise resolves, React retries the suspended work and swaps in the real UI
4. Nested boundaries let fast sections paint while slow ones keep their own fallbacks
5. With SSR/RSC, frameworks stream HTML and hydrate per boundary — see [Streaming & Suspense](./4_react_server.md#streaming--suspense)

**When to use:** Code-split routes (`React.lazy`), async data with `use(promise)`, any UI that should not block the whole page on one slow child.

**Pluses and minuses:**

| +                                    | −                                                 |
| ------------------------------------ | ------------------------------------------------- |
| Declarative loading states           | Fallback layout shift if skeletons mismatch       |
| Composes with concurrent rendering   | Server streaming semantics are framework-specific |
| Multiple boundaries = progressive UI |                                                   |

**Examples:**

```tsx
const Chart = lazy(() => import("./Chart"));

<Suspense fallback={<ChartSkeleton />}>
  <Chart data={data} />
</Suspense>;
```

**See also:** [`use() - 19`](./4_react_hooks.md#use---19) · [Streaming & Suspense](./4_react_server.md#streaming--suspense)

**Sources:**

[react.dev/reference/react/Suspense](https://react.dev/reference/react/Suspense)

Activity - 19

</details>

<details>
  <summary>Activity - 19</summary>

### Activity - 19

**Definition:** `<Activity mode="visible" | "hidden">` **hides** a subtree with `display: none` while **preserving React state** — unlike unmounting, which destroys state.

**How it works:**

- **`mode="visible"`** — normal render and commit
- **`mode="hidden"`** — UI hidden; component state kept in memory; Effects are destroyed while hidden and restored when visible again
- Prefer Activity over `{show && <Panel />}` when toggling sidebars, tabs, or wizards where remounting would reset form state

**When to use:** Pre-render or keep warm UI the user will return to; pair with [`<ViewTransition - 19>`](#viewtransition---19) for animated show/hide.

**Examples:**

```tsx
<Activity mode={isSidebarOpen ? "visible" : "hidden"}>
  <Sidebar />
</Activity>
```

**See also:** [ViewTransition - 19](#viewtransition---19) · [Performance](./4_react_ecosystem.md#performance)

**Sources:**

[react.dev/reference/react/Activity](https://react.dev/reference/react/Activity)

ViewTransition - 19

</details>

<details>
  <summary>ViewTransition - 19</summary>

### ViewTransition - 19

**Definition:** `<ViewTransition>` wraps a subtree so React can run **view transition** animations when that tree enters or leaves the document — integrated with Transitions and Suspense (browser View Transitions API).

**How it works:**

- Use inside `startTransition` (or an Action) so updates are non-urgent
- **`enter` / `exit` / `default`** props control animation (e.g. `"auto"`) when children mount/unmount or when an enclosing [`<Activity - 19>`](#activity---19) flips `mode`
- Often combined: Activity preserves state while ViewTransition animates visibility

**When to use:** Route or panel transitions, profile cards loading inside Suspense, any UI where you want coordinated enter/exit motion without hand-rolled CSS timers.

**Examples:**

```tsx
<Activity mode={show ? "visible" : "hidden"}>
  <ViewTransition enter="auto" exit="auto">
    <Counter />
  </ViewTransition>
</Activity>
```

**See also:** [Activity - 19](#activity---19) · [Suspense](#suspense) · [`useTransition`](./4_react_hooks.md#usetransition)

**Sources:**

[react.dev/reference/react/ViewTransition](https://react.dev/reference/react/ViewTransition)

---

</details>
