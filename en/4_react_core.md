# React — Core

Library model, rendering, JSX, and state.

[← Back to hub](./4_react.md)

### Related docs

- [React — Core](./4_react_core.md) · [React — Hooks](./4_react_hooks.md) · [React — Components](./4_react_components.md) · [React — APIs](./4_react_apis.md) · [React — React 19](./4_react_server.md) · [React — Ecosystem](./4_react_ecosystem.md) · [React — Interview](./4_react_interview.md)

### Sections

- [1. Library overview](#1-library-overview)
- [2. Rendering model](#2-rendering-model)
- [3. Describing the UI](#3-describing-the-ui)
- [4. State & interactivity](#4-state-interactivity)

---

## 1. Library overview

### Library overview

<details>
  <summary>React overview</summary>

### React overview

**Definition:** React is a JavaScript **library** for building user interfaces from reusable components. It manages the view layer only — routing, data fetching, and global conventions come from frameworks (Next.js, React Router) or third-party libraries.

**When to use:** SPAs, design systems, widgets (`createRoot` on a node), Next.js / React Router full-stack, React Native / Expo.

**Pluses and minuses:**

| +                                                         | −                                                             |
| --------------------------------------------------------- | ------------------------------------------------------------- |
| Component model scales from widgets to full apps          | Not batteries-included — need router, data layer, build tool  |
| Huge ecosystem, hiring pool, tooling (DevTools, Compiler) | Learning curve: hooks, concurrent rendering, RSC mental model |
| Declarative UI + virtual DOM reconciliation               | Bundle size for small embeds vs vanilla JS                    |
| Cross-platform: web (React DOM), native (React Native)    | Frequent ecosystem churn (patterns, meta-frameworks)          |
| Strong TypeScript integration                             | Class → hooks migration legacy in older codebases             |

**Examples:**

```tsx
import { useState } from "react";
import { createRoot } from "react-dom/client";

function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount((c) => c + 1)}>{count}</button>;
}

createRoot(document.getElementById("root")!).render(<Counter />);
```

**Thoughts:** React is **not** a framework. Inversion of Control: you call React (`createRoot`, `render`); frameworks call your file conventions. Meta (now Linux Foundation) maintains React; production apps typically ship via Vite, Next.js, or Expo — not Create React App (deprecated).

**Sources:**

[react.dev](https://react.dev/)

React vs ReactDOM

</details>

<details>
  <summary>React vs ReactDOM</summary>

### React vs ReactDOM

**Definition:** `react` = core (components, hooks, reconciliation). `react-dom` = web renderer (mount, commit, portals). Separate npm packages, **same version**.

**How it works:**

Web install is **two** packages, **three** `react-dom` import paths (not three npm packages):

|                    | Role                                             |
| ------------------ | ------------------------------------------------ |
| `react`            | Platform-agnostic UI                             |
| `react-dom`        | Web renderer (replaces `react-native` on mobile) |
| `react-dom/client` | `createRoot`, `hydrateRoot`                      |
| `react-dom`        | `createPortal`, `flushSync`, `useFormStatus`     |
| `react-dom/server` | `renderToString` / streaming SSR                 |

`react-dom` triggers the **first** render and **commit**; `react` runs **render** + `setState`. Browser paints.

**When to use:**

Always `react` + `react-dom` on web. Native swaps the renderer. SSR via a framework, not hand-rolled `react-dom/server`.

#### Common pitfalls

Treating `/server` as a third package; importing `react-dom` in every component; mismatched versions; `react` with no renderer; `react-native` on a web app.

**Examples:**

```tsx
import { createRoot } from "react-dom/client";
import { createPortal } from "react-dom";
createRoot(document.getElementById("root")!).render(<App />);
createPortal(<Modal />, document.body);
```

**Thoughts:** _Two packages for web; three_ `react-dom` _paths._ `react` _describes UI; the renderer commits it._

**See also:** [Render pipeline](#render-pipeline) · [Hydration](./4_react_server.md#hydration)

**Sources:**

[react.dev/reference/react-dom](https://react.dev/reference/react-dom)

Library vs framework

</details>

<details>
  <summary>Library vs framework</summary>

### Library vs framework

**Definition:**

A **library** solves a specific problem (UI rendering); you control the architecture. A **framework** inverts control — it defines project structure, routing, and data patterns.

|               | Library (React)       | Framework (Next.js)                |
| ------------- | --------------------- | ---------------------------------- |
| Control       | You compose the stack | Framework prescribes conventions   |
| Routing       | Bring your own        | Built-in file-based routing        |
| Data fetching | Bring your own        | Server Components, `fetch` caching |

**Examples:**

```tsx
// Library — you wire routing yourself
import { BrowserRouter, Routes, Route } from "react-router-dom";

// Framework — Next.js owns file-based routes and data loading
// app/page.tsx is a Server Component by convention
```

**Thoughts:** Interview answer: React is a library. Next.js / React Router v7 framework mode are full-stack React frameworks built on top.

**Sources:**

[react.dev/learn/start-a-new-react-project](https://react.dev/learn/start-a-new-react-project)

Philosophy

</details>

<details>
  <summary>Philosophy</summary>

### Philosophy

**Definition:**

Core design principles that shape how React apps are structured — declarative UI, composition, and unidirectional data flow.

| Principle                        | Meaning                                                                         |
| -------------------------------- | ------------------------------------------------------------------------------- |
| **Declarative**                  | Describe what the UI should look like for each state; React handles DOM updates |
| **Component-based**              | UI = tree of reusable, composable units                                         |
| **Unidirectional data flow**     | Props flow down; events flow up                                                 |
| **Composition over inheritance** | Build UIs by nesting components, not class hierarchies                          |
| **Learn once, write anywhere**   | Same component skills for web and native                                        |

**How it works:**

- **2013** — React open-sourced; JSX, virtual DOM
- **v16 (2017)** — Fiber rewrite; error boundaries
- **v16.8 (2019)** — Hooks; class components optional
- **v18 (2022)** — Concurrent rendering, `createRoot`, automatic batching, Suspense improvements
- **v19 (2024)** — Actions, `useActionState`, `useOptimistic`, React Compiler, `ref` as prop
- **v19.2** — `useEffectEvent`, Activity API, owner stack improvements

**Examples:**

```tsx
function LikeButton({
  liked,
  onToggle,
}: {
  liked: boolean;
  onToggle: () => void;
}) {
  return <button onClick={onToggle}>{liked ? "Unlike" : "Like"}</button>;
}
```

Props down, event up — parent owns `liked`.

**See also:**

[Rules of React](#rules-of-react-catalog) · [SOLID in React](#solid-in-react) · [Composition patterns](#composition-patterns)

Rules of React catalog

**Sources:** [react.dev/blog/2024/12/05/react-19](https://react.dev/blog/2024/12/05/react-19)

</details>

### Rules of React

Rules of React catalog

<details>
  <summary>Rules of React catalog</summary>

### Rules of React catalog

**Definition:** **Rules of React** are idioms React assumes you follow — **rules, not style**. Break them → bugs; Compiler and Concurrent features cannot fix violations.

**How it works:**

| Rule                                                                    |
| ----------------------------------------------------------------------- |
| [Components and Hooks must be pure](#components-and-hooks-must-be-pure) |
| [React calls Components and Hooks](#react-calls-components-and-hooks)   |
| [Rules of Hooks](#rules-of-hooks)                                       |

Enforce with [`<StrictMode>`](./4_react_components.md#strictmode) and [`eslint-plugin-react-hooks`](https://www.npmjs.com/package/eslint-plugin-react-hooks). Deeper render examples: [Purity](#purity) · [Element vs component](#element-vs-component).

**When to use:** Always — every component and custom hook.

**Sources:**

[react.dev/reference/rules](https://react.dev/reference/rules)

Components and Hooks must be pure

</details>

<details>
  <summary>Components and Hooks must be pure</summary>

### Components and Hooks must be pure

**Definition:** Components and Hooks must be **pure** with respect to their inputs for a single render — predictable UI, easier debugging, and safe automatic optimization.

**How it works:**

| Principle                       | Meaning                                                                                     |
| ------------------------------- | ------------------------------------------------------------------------------------------- |
| **Idempotent components**       | Same props, state, and context → same JSX output for that render                            |
| **Side effects outside render** | No `fetch`, subscriptions, or DOM writes in the render body — use event handlers or effects |
| **Immutable props & state**     | Never mutate props, state, or context; copy then `setState`                                 |
| **Immutable Hook inputs**       | Do not change values after they are passed into a Hook                                      |
| **Immutable after JSX**         | Do not mutate a value after it appears in JSX — mutate earlier in the function              |

Local objects created **during this render** may be mutated **before** they are passed to JSX. Mutating props, parent state, module-level data, or the DOM during render is not allowed.

**When to use:** Every render path in every component and custom `use*` function.

#### Common pitfalls

`new Date()` or `Math.random()` in render without state; mutating arrays from props; `fetch` in render; changing Hook arguments after the call.

**Examples:**

```tsx
// Bad — impure render
function Greeting({ name }: { name: string }) {
  const now = new Date();
  return (
    <h1>
      {name} — {now.toISOString()}
    </h1>
  );
}

// Good — local copy before JSX
function FriendList({ friends }: { friends: string[] }) {
  const sorted = [...friends].sort();
  return (
    <ul>
      {sorted.map((f) => (
        <li key={f}>{f}</li>
      ))}
    </ul>
  );
}
```

**See also:** [Purity](#purity) · [Render pipeline](#render-pipeline) · [StrictMode](./4_react_components.md#strictmode)

**Sources:**

[react.dev/reference/rules/components-and-hooks-must-be-pure](https://react.dev/reference/rules/components-and-hooks-must-be-pure)

React calls Components and Hooks

</details>

<details>
  <summary>React calls Components and Hooks</summary>

### React calls Components and Hooks

**Definition:** **React** decides when to render components and Hooks — you declare UI in JSX; React schedules work for the best user experience.

**How it works:**

| Rule                                        | Meaning                                                                                                    |
| ------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Never call component functions directly** | Use `<Article />`, not `Article()` — only JSX lets React own the instance, state, and effects              |
| **Never pass Hooks as regular values**      | Do not pass `useState` or a custom hook function through props or store hooks in variables for later calls |

Calling `Welcome()` yourself skips React's reconciliation, breaks state, and violates the mental model Compiler relies on.

**When to use:** Always in application code.

#### Common pitfalls

`{items.map((i) => Row(i))}` instead of `<Row key={i.id} item={i} />`; wrapping a hook in a callback prop.

**Examples:**

```tsx
return (
  <Layout>
    <Article />
  </Layout>
); // ✅
return <Layout>{Article()}</Layout>; // 🔴 React never owns that instance
```

**See also:** [Element vs component](#element-vs-component) · [Components and Hooks must be pure](#components-and-hooks-must-be-pure)

**Sources:**

[react.dev/reference/rules/react-calls-components-and-hooks](https://react.dev/reference/rules/react-calls-components-and-hooks)

Rules of Hooks

</details>

<details>
  <summary>Rules of Hooks</summary>

### Rules of Hooks

**Definition:** Hooks are JavaScript functions that represent **reusable UI logic** with strict call-site rules — breaking them corrupts React's hook list for that component.

**How it works:**

| Rule                                     | Meaning                                                                            |
| ---------------------------------------- | ---------------------------------------------------------------------------------- |
| **Only call Hooks at the top level**     | Not inside loops, conditions, or nested functions — call before any early `return` |
| **Only call Hooks from React functions** | Components or custom hooks (`use*`); not from plain utilities or event handlers    |

Full hook reference: [React — Hooks](./4_react_hooks.md).

**When to use:** Whenever you use `useState`, `useEffect`, or any `use*` API.

#### Common pitfalls

`if (loggedIn) useAuth()`; `for` loop with `useState`; calling `useQuery` inside `onClick`.

**Examples:**

```tsx
function Profile({ id }: { id: string | null }) {
  const userId = id ?? "";
  const user = useUser(userId);
  if (!id) return null;
  return <Card user={user} />;
}
```

**See also:** [Rules of React catalog](#rules-of-react-catalog) · [Custom hooks](./4_react_hooks.md#custom-hooks)

**Sources:**

[react.dev/reference/rules/rules-of-hooks](https://react.dev/reference/rules/rules-of-hooks)

SOLID in React

</details>

<details>
  <summary>SOLID in React</summary>

### SOLID in React

**Definition:** **SOLID** maps to **components, hooks, props, and composition** — not class inheritance. React already leans **O** and **D** ("composition over inheritance").

**How it works:**

Each letter matches the **Examples** below — look for `// good` and `// bad`:

1. **S** — data hook **or** UI component — not both in one file.
2. **O** — extend with `variant`, `children`, wrappers — don't copy-paste `DangerButton`.
3. **L** — if `Button` accepts `disabled`, every wrapper must honor it.
4. **I** — small prop surfaces — `Modal` is a shell with `children`, not 20 props.
5. **D** — pass `UserClient` into `useUsers()` — no `fetch('/api/...')` inside the hook.

**When to use:**

Large components, UI kits, reviews, interviews. Skip ceremony on a 10-line widget.

#### Common pitfalls

Over-SRP (15 one-line components); endless `variant` enums; DIP for a single prototype `fetch`. Guides, not laws.

**Examples:**

```tsx
type User = { id: string; name: string };
type UserClient = { list(): Promise<User[]> };

// S — good: hook loads data; component renders UI
function useUsers(client: UserClient) {
  const [users, setUsers] = useState<User[]>([]);
  useEffect(() => {
    void client.list().then(setUsers);
  }, [client]);
  return users;
}

function UserList({ users }: { users: User[] }) {
  return (
    <ul>
      {users.map((u) => (
        <li key={u.id}>{u.name}</li>
      ))}
    </ul>
  );
}

// S — bad: one component fetches, formats, handles errors, and renders
// function UserPage() {
//   const [users, setUsers] = useState<User[]>([]);
//   useEffect(() => {
//     fetch("/api/users")
//       .then((r) => r.json())
//       .then(setUsers);
//   }, []);
//   return (
//     <ul>
//       {users.map((u) => (
//         <li key={u.id}>{u.name.toUpperCase()}</li>
//       ))}
//     </ul>
//   );
// }

// O — good: extend Button with variant — no forked components
type ButtonProps = React.ComponentProps<"button"> & {
  variant?: "primary" | "danger";
};

function Button({ variant = "primary", children, ...props }: ButtonProps) {
  return (
    <button className={styles[variant]} {...props}>
      {children}
    </button>
  );
}

// O — bad: copy-paste PrimaryButton / DangerButton — edit both when padding changes
// function DangerButton(props: ButtonProps) {
//   return <button className="btn-danger" {...props} />;
// }

// L — good: disabled actually disables click
function IconButton({
  onClick,
  disabled,
  children,
}: {
  onClick?: () => void;
  disabled?: boolean;
  children: React.ReactNode;
}) {
  return (
    <button type="button" onClick={onClick} disabled={disabled}>
      {children}
    </button>
  );
}

// L — bad: ignores disabled — looks disabled but still fires onClick
// function IconButton({ onClick, disabled, children }) {
//   return (
//     <button type="button" onClick={onClick} aria-disabled={disabled}>
//       {children}
//     </button>
//   );
// }

// I — good: Modal shell — content via children
function Modal({
  open,
  onClose,
  children,
}: {
  open: boolean;
  onClose: () => void;
  children: React.ReactNode;
}) {
  if (!open) return null;
  return (
    <dialog open>
      <button type="button" onClick={onClose}>
        Close
      </button>
      {children}
    </dialog>
  );
}

// I — bad: god Modal — callers forced to pass props they never use
// function Modal({ open, title, body, footer, onConfirm, confirmLabel, ...rest }) { ... }

// D — good: inject API client — swap fake in tests
function UsersPanel({ client }: { client: UserClient }) {
  const users = useUsers(client);
  return <UserList users={users} />;
}

// D — bad: hook hardcodes fetch — hard to test, tied to one URL
// function useUsers() {
//   const [users, setUsers] = useState<User[]>([]);
//   useEffect(() => {
//     void fetch("/api/users")
//       .then((r) => r.json())
//       .then(setUsers);
//   }, []);
//   return users;
// }
```

**Thoughts:** _Small focused units, extend by composition, inject data — React is built for S, O, and D._

**See also:** [Philosophy](#philosophy) · [Composition patterns](#composition-patterns) · [SOLID (overview)](./7_special_engineering.md#solid-overview)

**Sources:**

[react.dev/learn](https://react.dev/learn) · [patterns.dev/react](https://www.patterns.dev/react/) · [SOLID (overview)](./7_special_engineering.md#solid-overview) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express)

---

</details>

## 2. Rendering model

### Rendering model

<details>
  <summary>Render pipeline</summary>

### Render pipeline

**Definition:** Every update is **Trigger → Render → Commit**, then the browser **paints**. React "render" = calling your functions — not painting pixels.

**How it works:**

```
Trigger   setState / parent / context / root.render()
   ↓
Render    call components → element tree → diff (memory only; may repeat)
   ↓
Commit    DOM mutations → useInsertionEffect → useLayoutEffect (blocks paint)
   ↓
Paint     browser layout + pixels
   ↓
Effects   useEffect after paint
```

| Who         | Owns                                            |
| ----------- | ----------------------------------------------- |
| `react`     | Render, `setState` / hooks                      |
| `react-dom` | Initial trigger (`createRoot`), commit, portals |
| Browser     | Paint                                           |

Packages vs import paths: [React vs ReactDOM](#react-vs-reactdom). Concurrent: render may pause/restart; **commit never pauses** — [Fiber](#react-fiber).

**When to use:**

Always. Know this for "what happens on setState?" interviews.

#### Common pitfalls

Render ≠ DOM update; side effects in render; `useLayoutEffect` for everything; mixing React render with the browser HTML/CSS pipeline → [1_html.md](./1_html.md).

**Examples:**

```tsx
function Tooltip({ text }: { text: string }) {
  const ref = useRef<HTMLDivElement>(null);
  useLayoutEffect(() => {
    /* measure DOM before paint */
  }, [text]);
  useEffect(() => {
    document.title = text;
  }, [text]); // after paint
  return <div ref={ref}>{text}</div>;
}
```

**Thoughts:** _Trigger schedules → render computes (may repeat) → commit mutates once → paint →_ `useEffect`_._

**See also:** [React vs ReactDOM](#react-vs-reactdom) · [Virtual DOM & reconciliation](#virtual-dom--reconciliation) · [Concurrent rendering](#concurrent-rendering) · [Batching](#batching)

**Sources:**

[react.dev/learn/render-and-commit](https://react.dev/learn/render-and-commit) · [react.dev/reference/react/useLayoutEffect](https://react.dev/reference/react/useLayoutEffect)

Virtual DOM & reconciliation

</details>

<details>
  <summary>Virtual DOM & reconciliation</summary>

### Virtual DOM & reconciliation

**Definition:** React builds a tree of **React elements** — plain JS objects describing UI (`{ type, props, key }`). **Reconciliation** is the algorithm that diffs the new element tree against the previous one and plans minimal DOM changes. Colloquially this tree is called the "Virtual DOM," but it's **not** a full clone of the browser DOM.

**How it works:**

1. **Render** — components run and return a **new** element tree (immutable snapshots each time)
2. **Diff** — reconciler walks new vs previous tree and decides: update props, reuse DOM node, or tear down and rebuild
3. **Commit** — `react-dom` applies the recorded DOM ops (see [render pipeline](#render-pipeline))

**What a React element is** (the unit reconciliation compares):

```tsx
// JSX compiles to createElement — produces a plain object, not a DOM node
<h1 className="title">Hello</h1>
// ≈ { type: 'h1', props: { className: 'title', children: 'Hello' }, key: null }
```

See [Element vs component](#element-vs-component) — components **return** elements; reconciliation compares **elements**.

**Reconciliation rules** (diff heuristics — not deep equality):

| Compare                                                       | Result                                                                                        |
| ------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| **Different** `type` (`div` → `span`, or `Counter` → `Timer`) | Destroy old subtree, build new from scratch — state below is lost                             |
| **Same DOM** `type` (`<div>` → `<div>`)                       | Keep the DOM node, update changed props only                                                  |
| **Same component** `type` (`<Counter />` → `<Counter />`)     | Keep instance, update props, recurse into children                                            |
| **Lists**                                                     | Match siblings by `key` across renders — without stable keys, React falls back to index order |

```tsx
// Same type → update text node only, <input> value preserved
return <h1>Hello, {name}</h1>;

// Different type → old <h1> removed, new <p> inserted
return condition ? <h1>Title</h1> : <p>Body</p>;

// Keys — stable id, not index on sortable lists
{
  items.map((item) => <li key={item.id}>{item.name}</li>);
}
```

A component can re-render and produce an element tree **identical** to the previous one — reconciliation finds no changes and **commit skips DOM mutations**. Parent re-renders often re-run child functions without touching the DOM.

The public model is element-tree diffing. Internally since React 16, **[Fiber](#react-fiber)** implements reconciliation as interruptible work units (linked list of fiber nodes) — enables concurrent rendering, priorities, and Suspense. You don't interact with Fiber directly.

**When to use:**

Always — this is how React works internally. You don't opt in.

#### Common pitfalls

- **Shadow DOM ≠ Virtual DOM** — Shadow DOM is a browser encapsulation primitive for Web Components; React's tree is JS-level
- **Deep-comparing props in your head** — React compares **element type + key**, not `JSON.stringify(props)`
- **Index as** `key` on reorderable/filterable lists — wrong instance reuse, stale state → [Keys](#keys)
- **Assuming every** `setState` **hits the DOM** — render always runs (usually); DOM updates only when diff finds changes
- `key` **on component remounts it** — intentional reset (`<Form key={userId} />`), but surprising if accidental

**Pluses and minuses:**

| +                                           | −                                                                              |
| ------------------------------------------- | ------------------------------------------------------------------------------ |
| Declarative updates — write `UI = f(state)` | Misleading name — not a full DOM clone for every node                          |
| Reuses DOM nodes when type + key match      | Diff is O(n) heuristic, not optimal — huge lists need virtualization           |
| Same model works for React Native           | "VDOM is faster than real DOM" is a myth — browsers already do partial updates |

**Thoughts:** _Reconciliation diffs element trees by type and key — same type updates props in place; different type rebuilds; lists need stable keys._

**See also:** [Render pipeline](#render-pipeline) · [Element vs component](#element-vs-component) · [Keys](#keys) · [React Fiber](#react-fiber) · [Batching](#batching)

**Sources:**

[react.dev/learn/preserving-and-resetting-state](https://react.dev/learn/preserving-and-resetting-state) · [react.dev/learn/rendering-lists](https://react.dev/learn/rendering-lists)

React Fiber

</details>

<details>
  <summary>React Fiber</summary>

### React Fiber

**Definition:** **Fiber** is React's reconciler (since v16). One **fiber** = one **unit of work** for a component instance or host node (`div`, text). It is a **virtual stack frame** — React can pause, resume, reuse, or throw it away.

**How it works:**

~60 FPS ≈ 16ms per frame. Fiber splits render into **chunks** and **schedules** urgent work first so typing and animations are not blocked.

**Sequence** (one update):

```
1. Bootstrap   createRoot → FiberRootNode + HostRootFiber (#root)
2. Queue       setState → update on a fiber → Scheduler assigns a **lane**
3. Render      clone current → WIP; DFS work loop (memory only, no DOM)
                 beginWork ↓  (call component, reconcile children, set flags)
                 completeWork ↑ (bubble flags onto the effect list)
                 between fibers: shouldYield()? → end chunk (pause) or continue
                 urgent lane mid-WIP? → discard WIP, restart
4. Commit      sync (never pauses): Placement / Update / Deletion → flip current
               useLayoutEffect → browser paint → useEffect
```

Render ≠ paint. The screen changes only at **commit**. Reconciler (`react`) walks fibers; renderer (`react-dom`) writes the DOM.

|      | current          | WIP                                  |
| ---- | ---------------- | ------------------------------------ |
| Tree | On screen        | Draft this render                    |
| Link | `alternate` twin | Clone; reuse if `type` + `key` match |

Linked list (`child` = first child, `sibling` = next, `return` = parent) so React always knows the **next** fiber without the JS call stack — that is how pause/resume works.

```
App
 └─ child → Header ── sibling → Main
                                 └─ child → ItemA ── sibling → ItemB
```

Walk: App → Header → Main → ItemA → ItemB, then `completeWork` back up. Hook state lives on `memoizedState` on the fiber (same call order every render).

One fiber (or a short burst) processed before `shouldYield()`. React **finishes the current function**, then yields — never mid-component. Typical slice ~5ms; the next frame resumes at the next fiber. **Commit never yields.**

```
[browser input]  [React chunk → yield]  [paint]
[next frame: resume from next fiber]
```

Chunks do **not** use extra CPU cores and do **not** make your own heavy JS (`array.filter` on 100k items) non-blocking — only reconciliation is sliced.

Scheduler orders _when_ the callback runs. **Lanes** tag _which_ updates.

| High → low | Trigger            | Notes                                              |
| ---------- | ------------------ | -------------------------------------------------- |
| Sync       | click, `flushSync` | Commit ASAP                                        |
| Input      | `onChange`, drag   | Keep typing smooth                                 |
| Default    | normal `setState`  | Standard                                           |
| Transition | `startTransition`  | Interruptible; WIP can be discarded                |
| Idle       | prefetch           | Lowest; starved work **expires** and gets promoted |

Same lane → **batched**. Higher lane during a lower render → throw away WIP, restart.

**When to use:**

Interview "how React stays responsive"; expensive tab/list UI via transitions. Needs `createRoot` (React 18+).

#### Common pitfalls

- Fiber ≠ concurrent rendering — engine vs `createRoot` + transitions
- Conditional hooks shift the fiber's hook list
- Don't read `__reactFiber$` / internals from app code

**Pluses and minuses:**

| +                                          | −                                                  |
| ------------------------------------------ | -------------------------------------------------- |
| Interruptible render, priorities, Suspense | Internals unstable; commit still blocks the thread |

**Examples:**

```tsx
onClick={() => setOpen(true)}; // urgent lane
startTransition(() => setTab('analytics')); // transition lane
```

**Thoughts:** _Render prepares work in chunks; commit applies it in one atomic pass._

**See also:** [Render pipeline](#render-pipeline) · [Virtual DOM & reconciliation](#virtual-dom--reconciliation) · [Concurrent rendering](#concurrent-rendering) · [Batching](#batching)

**Sources:**

[github.com/acdlite/react-fiber-architecture](https://github.com/acdlite/react-fiber-architecture) · [react.dev/learn/render-and-commit](https://react.dev/learn/render-and-commit) · [react.dev/reference/react/useTransition](https://react.dev/reference/react/useTransition) · [React Scheduler docs](https://scheduler-explorer.vercel.app/)

Concurrent rendering

</details>

<details>
  <summary>Concurrent rendering</summary>

### Concurrent rendering

**Definition:** React can prepare multiple versions of the UI in memory, **interrupt** low-priority renders, and **commit urgent updates first**. Built on [Fiber](#react-fiber) — render is interruptible; commit is not.

**How it works:**

1. **Urgent update** (typing, click) — renders and commits immediately on a high-priority lane
2. **Transition update** (`startTransition`) — marked low-priority; React may keep showing the previous UI while preparing the new tree in the background
3. If a new urgent update arrives mid-transition, React **discards** stale transition work and prioritizes the urgent render
4. Still **single-threaded** — cooperative yielding, not Web Workers

| Urgent                     | Deferred (transition)                           |
| -------------------------- | ----------------------------------------------- |
| Input value, button clicks | Large list filter, tab switch, chart redraw     |
| User must see immediately  | Brief stale UI acceptable (`isPending` spinner) |

**When to use:**

Tab switches, large list filtering, chart updates while user types, Suspense boundaries for async data.

Every `setState` — measure first; default updates are already fast enough for most UIs.

#### Common pitfalls

- Wrapping all updates in `startTransition` — unnecessary; only defer expensive re-renders
- Expecting parallel JS — Fiber yields on the main thread only
- Forgetting `createRoot` — legacy `ReactDOM.render` does not enable full concurrent features

**Examples:**

```tsx
const [isPending, startTransition] = useTransition();

function handleChange(e: React.ChangeEvent<HTMLInputElement>) {
  setInput(e.target.value); // urgent — input stays in sync
  startTransition(() => setFiltered(heavyFilter(e.target.value))); // deferred
}
```

**Thoughts:** _Prepare UI in the background, commit urgent work first — stale UI briefly is the tradeoff._

**See also:** [React Fiber](#react-fiber) · [Render pipeline](#render-pipeline) · [Batching](#batching)

**Sources:**

[react.dev/reference/react/useTransition](https://react.dev/reference/react/useTransition)

Batching

</details>

<details>
  <summary>Batching</summary>

### Batching

**Definition:** React groups multiple state updates into **one** render/commit cycle instead of re-rendering after every `setState`.

**How it works:**

1. Each `setState` / `dispatch` enqueues an update on the component's fiber
2. React processes the whole queue before the next render — multiple updates → **one** render pass
3. Inside that render, state reflects all queued updates (functional updaters run in order)
4. React 18+ batches inside promises, `setTimeout`, and native event handlers too (with `createRoot`)

| Version   | Behavior                                                                          |
| --------- | --------------------------------------------------------------------------------- |
| React 17  | Batched only inside React event handlers                                          |
| React 18+ | Automatic batching in promises, `setTimeout`, native handlers (with `createRoot`) |

`flushSync(() => setState(...))` forces immediate render + DOM commit — use when you must read DOM right after an update (rare).

**When to use:** Default — always on. You rarely think about it.

**Examples:**

```tsx
function handleClick() {
  setCount((c) => c + 1);
  setFlag((f) => !f);
  // React 18+: one re-render, not two
}

// Pitfall — same snapshot twice (not a batching issue)
setCount(count + 1);
setCount(count + 1); // still +1 total — use c => c + 1
```

**Thoughts:** _Batching reduces renders; functional updaters fix stale snapshot math._

**See also:** [State as snapshot](#state-as-snapshot) · [Render pipeline](#render-pipeline)

**Sources:**

[react.dev/learn/queueing-a-series-of-state-updates](https://react.dev/learn/queueing-a-series-of-state-updates)

Keys

</details>

<details>
  <summary>Keys</summary>

### Keys

**Definition:** `key` is a special React attribute that gives a **stable identity** to a child among its siblings across renders. Reconciliation matches list items by `key`, not by array index.

**How it works:**

1. React walks sibling fibers in order
2. On re-render, it tries to match each new child to a previous child with the **same key**
3. Match found → **reuse** fiber + DOM node, update props
4. No match → mount new; leftover old keys → unmount
5. `key` is **not** passed as a prop to your component

**When to use:**

Every item in `array.map()`. Use stable IDs from data (`todo.id`). Also use `key` on a component to **force remount** when identity changes (`<Form key={userId} />`).

| Key choice                            | OK when                                          |
| ------------------------------------- | ------------------------------------------------ |
| Database `id`                         | Default — reorder, filter, insert                |
| Array index                           | Static list — never reordered/filtered/prepended |
| `Math.random()` / `uuid()` per render | **Never** — new key every render, full remount   |

#### Common pitfalls

- Index as key on sortable/filterable lists — wrong instance reuse
- Random key per render — destroys performance and state
- Changing `key` **remounts** the component — resets internal state (use intentionally)

**Examples:**

```tsx
// Good — stable id from database
{
  todos.map((todo) => <Todo key={todo.id} todo={todo} />);
}

// Bad — reorder [A,B] → [B,A]: React reuses wrong instances, stale state
{
  todos.map((todo, i) => <Todo key={i} todo={todo} />);
}
```

**Thoughts:** _Key = identity across renders, not list position._

**See also:** [Virtual DOM & reconciliation](#virtual-dom--reconciliation) · [Keys](#keys)

**Sources:**

[react.dev/learn/rendering-lists](https://react.dev/learn/rendering-lists)

Strict Mode

</details>

<details>
  <summary>Strict Mode</summary>

### Strict Mode

**Definition:** `<StrictMode>` is a dev-only wrapper that runs **extra checks** to surface bugs early. It does **not** render visible UI and has **zero** production impact.

**How it works:**

| Check                 | What happens                                                          |
| --------------------- | --------------------------------------------------------------------- |
| **Double render**     | Component function called twice — catches impure render side effects  |
| **Double effects**    | `useEffect` runs mount → cleanup → mount — catches missing cleanup    |
| **Deprecated APIs**   | Warns about legacy patterns (`findDOMNode`, old context, string refs) |
| **Unsafe lifecycles** | Class component warnings (legacy)                                     |

**When to use:**

Wrap `<App />` at the root in development. Keep enabled — it trains you to write resilient effects and pure renders.

As a fix for double-fetch — fix effect cleanup or dedupe logic instead of removing Strict Mode.

#### Common pitfalls

- Disabling Strict Mode because effects run twice — the bug is missing cleanup, not Strict Mode
- Putting side effects in render body — double render exposes them → [Rules of React](#rules-of-react-catalog)
- Assuming production behaves like dev — Strict Mode checks are stripped in prod builds

**Examples:**

```tsx
<StrictMode>
  <App />
</StrictMode>
```

**Thoughts:** _Strict Mode stress-tests your components — if it breaks in dev, it was already fragile._

**See also:** [Rules of React](#rules-of-react-catalog) · [Purity](#purity) · [Render pipeline](#render-pipeline) · [StrictMode (Components)](./4_react_components.md#strictmode)

**Sources:**

[react.dev/reference/react/StrictMode](https://react.dev/reference/react/StrictMode)

Element vs component

</details>

<details>
  <summary>Element vs component</summary>

### Element vs component

**Definition:** A **React element** is a plain object (`{ type, props, key }`) describing what to render. A **component** is a function or class that **returns** elements when React calls it.

**How it works:**

1. You write JSX → compiles to `React.createElement(type, props, ...children)`
2. `<Welcome name="Ada" />` creates an element `{ type: Welcome, props: { name: 'Ada' } }`
3. Reconciler sees `type: Welcome` → **calls** `Welcome({ name: 'Ada' })`
4. Component returns more elements (e.g. `<h1>`) → recursion continues until host DOM elements
5. New element tree every render; component **instances** (fiber + state) persist

|            | Element                          | Component                 |
| ---------- | -------------------------------- | ------------------------- |
| What       | Immutable description (snapshot) | Reusable function/class   |
| Created by | `createElement` / JSX            | You define it             |
| Per render | New object every time            | Same function, re-invoked |
| State      | None                             | Hooks live on fiber       |

**When to use:**

Elements are what reconciliation diffs. Components are how you organize logic. Capital letter = component (`<Welcome />`); lowercase = DOM (`<div />`).

#### Common pitfalls

- Confusing `<Welcome />` with a DOM node — it's an instruction to call `Welcome`
- Calling `Welcome()` as a function — [Rules of React](#rules-of-react-catalog): only JSX so React owns the call
- Lowercase JSX (`<welcome />`) — React treats it as unknown HTML tag, not your component
- Mutating element objects — always create new elements via JSX each render

**Examples:**

```tsx
// Component (function) — React CALLS this
function Welcome({ name }: { name: string }) {
  return <h1>Hello, {name}</h1>; // returns an element
}

// Element — description object, not a live DOM node
const element = <Welcome name="Ada" />;
// ≈ { type: Welcome, props: { name: 'Ada' } }
```

**Thoughts:** _Components are functions; elements are their output snapshots — reconciliation compares elements._

**See also:** [Rules of React](#rules-of-react-catalog) · [Virtual DOM & reconciliation](#virtual-dom--reconciliation) · [Components](#components)

**Sources:**

[react.dev/learn/your-first-component](https://react.dev/learn/your-first-component)

---

</details>

## 3. Describing the UI

### Describing the UI

<details>
  <summary>Components</summary>

### Components

**Definition:** A React component is a JavaScript function (or class — legacy) that returns JSX. Names **must** start with a capital letter so React distinguishes them from HTML tags.

**When to use:**

Every piece of UI. Prefer small, focused components composed upward.

#### Pitfalls

1. Lowercase names render as HTML tags: `<profile />` → DOM element, not your component
2. **Never nest component definitions** inside another component — causes remount every parent render

```tsx
// Bad — Profile recreated every Gallery render
function Gallery() {
  function Profile() {
    return <p>...</p>;
  }
  return <Profile />;
}

// Good — define at module scope
function Profile() {
  return <p>...</p>;
}
function Gallery() {
  return <Profile />;
}
```

**Pluses and minuses:**

| +                              | −                                           |
| ------------------------------ | ------------------------------------------- |
| Colocate markup, logic, styles | Deep trees → prop drilling without planning |
| Reusable at any granularity    | Over-fragmentation adds boilerplate         |
| Testable in isolation          |                                             |

**Examples:**

```tsx
function VideoList({ videos }: { videos: Video[] }) {
  return (
    <section>
      {videos.map((v) => (
        <Video key={v.id} video={v} />
      ))}
    </section>
  );
}
```

**Thoughts:**

_Components are functions — capital letter = React component, lowercase = HTML._

Single Responsibility — if a component grows, split it. Pages/widgets/features in FSD terms (see [Section 9](./4_react_ecosystem.md#9-ecosystem--architecture)).

**See also:** [Element vs component](#element-vs-component) · [Purity](#purity)

**Sources:**

[react.dev/learn/your-first-component](https://react.dev/learn/your-first-component)

JSX

</details>

<details>
  <summary>JSX</summary>

### JSX

**Definition:** JSX is a syntax extension that compiles to `React.createElement(type, props, ...children)`. It lets you write HTML-like markup inside JavaScript.

**How it works:**

- One root element or `<>...</>` Fragment
- Close all tags (`<img />`, `<br />`)
- camelCase DOM attributes: `className`, `onClick`, `tabIndex`
- Capitalized tags = components; lowercase = HTML

**Pluses and minuses:**

| +                         | −                                         |
| ------------------------- | ----------------------------------------- |
| Readable UI code          | Requires build step (Babel, SWC, esbuild) |
| Type-safe with TypeScript | `class` → `className`, `for` → `htmlFor`  |
| Expressions via `{}`      | Must return single root (or Fragment)     |

**Examples:**

```tsx
const name = "Ada";
return (
  <>
    <h1 className="title">{name}</h1>
    {items.length > 0 && <List items={items} />}
  </>
);
```

**Thoughts:** JSX is syntactic sugar — `<>...</>` compiles to `Fragment`. Conditional rendering: prefer `condition ? <A /> : <B />` or `!!condition && <A />` — watch for `0` falsy rendering.

**Sources:**

[react.dev/learn/writing-markup-with-jsx](https://react.dev/learn/writing-markup-with-jsx)

Fragments

</details>

<details>
  <summary>Fragments</summary>

### Fragments

**Definition:** `Fragment` (or shorthand `<>...</>`) groups multiple children without adding an extra DOM node.

**How it works:** JSX requires a single root element. `Fragment` satisfies that rule while rendering nothing to the DOM — children appear as siblings of the parent's other nodes. `<>...</>` compiles to `<React.Fragment>`; use the long form when you need a `key` on the fragment itself (e.g. keyed list items that return multiple elements).

**When to use:** Table rows (`<tr>` pairs), list items with multiple nodes, layout shells where an extra wrapper would break CSS.

**Pluses and minuses:**

| +                                                      | −                                              |
| ------------------------------------------------------ | ---------------------------------------------- |
| Avoids wrapper `<div>` that breaks layout (flex/grid)  | Cannot set `className` or events on a fragment |
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

// Keyed fragment in a list
items.map((item) => (
  <Fragment key={item.id}>
    <dt>{item.term}</dt>
    <dd>{item.definition}</dd>
  </Fragment>
));
```

**See also:** [Fragment (Components)](./4_react_components.md#fragment)

**Sources:**

[react.dev/reference/react/Fragment](https://react.dev/reference/react/Fragment)

Props

</details>

<details>
  <summary>Props</summary>

### Props

**Definition:** Props are **read-only** inputs passed from parent to child — like function arguments. The child cannot mutate them; only the parent can change props by re-rendering with new values.

**How it works:**

1. Parent renders → passes props snapshot to child: `<Child count={count} onSave={handleSave} />`
2. Child receives props for **this render only** — frozen for that call
3. Child requests change via **callback prop** (`onIncrement`) — parent owns state and updates
4. One-way flow: data **down**, events **up**

**When to use:**

Pass data, configuration, and event handlers down the tree. Lift state up when siblings need shared data → [Lifting state up](#lifting-state-up).

#### Common pitfalls

- Mutating props inside child (`props.items.push(...)`) — breaks one-way flow
- `{...props}` on DOM elements — leaks invalid attrs (`isLoading`, `variant`)
- Creating new object/function inline every render — can break `memo` children (`onClick={() => ...}`)

**Examples:**

```tsx
function Button({ label, onClick, ...rest }: ButtonProps) {
  return (
    <button onClick={onClick} {...rest}>
      {label}
    </button>
  );
}
```

**Thoughts:** _Props in, callbacks out — parent owns mutable state._

**See also:** [State vs props](#state-vs-props) · [Composition patterns](#composition-patterns)

**Sources:**

[react.dev/learn/passing-props-to-a-component](https://react.dev/learn/passing-props-to-a-component)

Purity

</details>

<details>
  <summary>Purity</summary>

### Purity

**Definition:**

React components must be **pure** during render: same inputs → same output; no mutating external state or props. Full contract: [Components and Hooks must be pure](#components-and-hooks-must-be-pure) · [Rules of React catalog](#rules-of-react-catalog).

| Allowed                         | Forbidden during render                                     |
| ------------------------------- | ----------------------------------------------------------- |
| Create local variables          | `fetch()` side effects                                      |
| Local mutation of _new_ objects | Mutate props, state, context, refs                          |
| Compute derived values          | Subscribe to external stores without `useSyncExternalStore` |

**Side effects** belong in event handlers or effects — not in the render body.

**When to use:**

Every component render should be pure. Derive display values in render; fetch/subscribe in `useEffect` or event handlers.

#### Common pitfalls

- `fetch` in render — runs every re-render; use `useEffect` or framework data layer
- [Strict Mode](#strict-mode) double-calls render in dev — exposes impure code
- Reading external mutable globals during render — output varies without props/state change

**Examples:**

```tsx
// Bad — impure render
function ItemList({ items }: { items: Item[] }) {
  items.push({ id: "new" }); // mutates prop!
  return (
    <ul>
      {items.map((i) => (
        <li key={i.id}>{i.name}</li>
      ))}
    </ul>
  );
}

// Good — local mutation of new data is fine
function FriendList({ friends }: { friends: string[] }) {
  const sorted = [...friends].sort(); // new array, local
  return (
    <ul>
      {sorted.map((f) => (
        <li key={f}>{f}</li>
      ))}
    </ul>
  );
}
```

**Thoughts:** _Render is a pure function of props, state, and context._

**See also:** [Components and Hooks must be pure](#components-and-hooks-must-be-pure) · [Render pipeline](#render-pipeline)

**Sources:**

[react.dev/learn/keeping-components-pure](https://react.dev/learn/keeping-components-pure)

Conditional rendering & lists

</details>

<details>
  <summary>Conditional rendering & lists</summary>

### Conditional rendering & lists

**Definition:** JSX is JavaScript — use `if`, ternary, `&&`, and `map()` to decide what renders. No special template syntax.

**How it works:** Expressions inside `{ }` evaluate to React nodes, `null`, `undefined`, `false` (render nothing), or arrays of nodes. Early `return` before JSX is often clearest for complex branches.

**When to use:**

| Pattern                     | Use case                                            |
| --------------------------- | --------------------------------------------------- |
| `if` + early `return`       | Multiple branches, loading/error states             |
| `condition ? <A /> : <B />` | Two branches inline                                 |
| `condition && <A />`        | Single branch — **not** when left side can be `0`   |
| `items.map()`               | Lists — always provide stable `key` → [Keys](#keys) |

#### Common pitfalls

- `{count && <Badge />}` when `count === 0` — renders `0` on screen; use `count > 0 &&`
- Nested ternaries — hard to read; extract component or use early return
- `map` without `key` — reconciliation warnings and bugs on reorder

**Examples:**

```tsx
if (loading) return <Spinner />;
if (error) return <Error message={error.message} />;

{
  isLoggedIn ? <Dashboard /> : <Login />;
}
{
  count > 0 && <Badge count={count} />;
} // not: {count && ...} when count can be 0
{
  items.map((item) => <Row key={item.id} item={item} />);
}
```

**Thoughts:** _JSX has no_ `v-if` _— use JavaScript expressions and early returns._

**See also:** [Keys](#keys) · [Component trees](#component-trees)

**Sources:**

[react.dev/learn/conditional-rendering](https://react.dev/learn/conditional-rendering)

Component trees

</details>

<details>
  <summary>Component trees</summary>

### Component trees

**Definition:** React models UI as trees. Two related views: **render tree** (runtime parent/child components) and **module tree** (static `import` graph for bundling).

**How it works:**

- Each component instance is a **node**; parent renders children as nested JSX
- React matches state to **tree position** — same component type at same place → state preserved
- Different type at same slot → old subtree **unmounted**, state lost
- Conditional rendering changes tree shape → can reset state unintentionally

| Tree       | What it shows               | Used for                        |
| ---------- | --------------------------- | ------------------------------- |
| **Render** | Who renders whom at runtime | State, reconciliation, context  |
| **Module** | File import dependencies    | Code splitting, bundle analysis |

**When to use:**

Understand render tree when debugging state resets, context scope, and error boundary placement.

#### Common pitfalls

- Swapping `<A />` for `<B />` at same position — destroys A's state
- Assuming state follows variable name — position in tree matters, not JSX label
- Nesting same component in different branches — separate state instances

**Examples:**

```tsx
// Same position → state preserved across re-renders
<Profile userId={id} />;

// Different type at same slot → state reset
{
  showA ? <CounterA /> : <CounterB />;
}

// Same variable name, different position — still different state slots
{
  cond ? (
    <Panel>
      <Counter />
    </Panel>
  ) : (
    <Counter />
  );
}
```

**Thoughts:** _State is tied to tree position, not to how you named the variable._

**See also:** [Virtual DOM & reconciliation](#virtual-dom--reconciliation) · [Keys](#keys)

**Sources:**

[react.dev/learn/understanding-your-ui-as-a-tree](https://react.dev/learn/understanding-your-ui-as-a-tree)

Composition patterns

</details>

<details>
  <summary>Composition patterns</summary>

### Composition patterns

**Definition:**

Build complex UIs by nesting components and passing `children`.

| Pattern                 | Status    | Use                                         |
| ----------------------- | --------- | ------------------------------------------- |
| **Children**            | Preferred | Layout shells, generic wrappers             |
| **Slots** (named props) | Common    | `<Card header={...} footer={...} />`        |
| **HOC**                 | Legacy    | `withRouter`, `connect` — avoid in new code |
| **Render props**        | Legacy    | `<DataProvider render={data => ...} />`     |

**How it works:** Parent passes UI fragments as `children` or named props — child renders slots without knowing parent's data source. Avoids prop drilling for layout shells.

**When to use:**

| Pattern                          | Prefer when                                 |
| -------------------------------- | ------------------------------------------- |
| `children`                       | Generic wrapper (Card, Layout, Modal shell) |
| Named slots (`header`, `footer`) | Fixed regions with different types          |
| Context                          | Deep tree, stable shared data (theme, auth) |
| HOC / render props               | Legacy libs only — hooks replace most cases |

#### Common pitfalls

- Prop drilling through 5+ layers — lift, context, or colocate state
- HOC wrapper hell — hard to trace props; prefer hooks + composition
- `children` as function for everything — render props pattern; use only when needed

**Examples:**

```tsx
function Card({
  children,
  header,
}: {
  children: React.ReactNode;
  header?: React.ReactNode;
}) {
  return (
    <article>
      {header && <header>{header}</header>}
      <main>{children}</main>
    </article>
  );
}
```

**Thoughts:** _Compose UI like nesting HTML — slots over inheritance._

**See also:** [SOLID in React](#solid-in-react) · [Props](#props) · [Lifting state up](#lifting-state-up)

**Sources:**

[patterns.dev/react](https://www.patterns.dev/react/)

---

</details>

## 4. State & interactivity

### State & interactivity

<details>
  <summary>State</summary>

### State

**Definition:** State is component memory — data that changes over time in response to user input or async results. React holds state outside your function and gives you a snapshot per render.

**How it works:**

1. React stores state on the component's **fiber** — outside your function body
2. Each render, `useState` returns the **current snapshot** for that render
3. `setState` schedules an update — does not mutate the snapshot you're holding
4. Re-render → function runs again with the new snapshot

**When to use:**

Any UI that changes: forms, toggles, fetched data, selections.

#### Common pitfalls

- Expecting local variables to persist (`let count = 0` in function body — resets every render)
- Duplicating state that can be derived from props or other state
- Mutating state objects in place (`items.push(x); setItems(items)`) — same reference, React may skip update

**Pluses and minuses:**

| +                                          | −                                            |
| ------------------------------------------ | -------------------------------------------- |
| Local encapsulation                        | Easy to create redundant/duplicated state    |
| Triggers targeted re-renders               | Async batching can surprise (stale closures) |
| `useState` / `useReducer` cover most cases | Global state needs context or external store |

**Examples:**

```tsx
const [count, setCount] = useState(0);
// Functional update avoids stale closure
setCount((c) => c + 1);
```

**Thoughts:** _State lives on the fiber; each render sees a frozen snapshot._

**See also:** [State as snapshot](#state-as-snapshot) · [Batching](#batching) · [Reactive programming](./7_special_architecture.md#reactive-programming--reactivity)

**Sources:**

[react.dev/learn/state-a-components-memory](https://react.dev/learn/state-a-components-memory)

State vs props

</details>

<details>
  <summary>State vs props</summary>

### State vs props

**Definition:** **Props** are read-only inputs from parent; **state** is data the component (or lifted ancestor) **owns** and updates via setter.

**How it works:**

Parent holds state → passes **snapshot** as props → child displays and calls callback → parent updates state → child re-renders with new props. Child never mutates props directly.

|            | Props                   | State                                        |
| ---------- | ----------------------- | -------------------------------------------- |
| Source     | Parent                  | Local (`useState`) or global (context/store) |
| Mutability | Read-only for child     | Updated via setter                           |
| Changes    | Parent re-renders child | Self or child triggers update                |
| Question   | "What data do I get?"   | "What do I remember?"                        |

**When to use:** Props for configuration and data flow. State for interactive UI that changes over time. If two siblings need it → lift state up.

**Examples:**

```tsx
function Parent() {
  const [count, setCount] = useState(0);
  return (
    <Child
      label="Clicks"
      count={count}
      onIncrement={() => setCount((c) => c + 1)}
    />
  );
}
```

**Thoughts:** _Props in, events out — state lives where it's owned._

**See also:** [Props](#props) · [Lifting state up](#lifting-state-up)

**Sources:**

[react.dev/learn/passing-props-to-a-component](https://react.dev/learn/passing-props-to-a-component)

State as snapshot

</details>

<details>
  <summary>State as snapshot</summary>

### State as snapshot

**Definition:** Calling `setState` schedules a re-render with the **next** state — it does not mutate current state in place. Event handlers close over the **render-time snapshot**, not live state.

**How it works:**

1. Render with `n = 0` → handler created, closes over `n = 0`
2. `setN(n + 1)` queues update based on snapshot `0` → next `n` is `1`
3. Two `setN(n + 1)` in same handler both read `0` → still `1`, not `2`
4. `setN(prev => prev + 1)` queues functional updaters — each sees previous queued result
5. [Batching](#batching) processes all queued updates before next render

**When to use:** Always understand this when debugging counters, async handlers, and stale closure bugs.

**Examples:**

```tsx
function Counter() {
  const [n, setN] = useState(0);
  function handleClick() {
    setN(n + 1);
    setN(n + 1);
    // After one click: n becomes 1, not 2 — both use snapshot n=0
    setN((prev) => prev + 1);
    setN((prev) => prev + 1);
    // After one click: n becomes 2 — functional updates queue correctly
  }
}
```

**Thoughts:** _setState schedules the future; the current render's values are frozen._

**See also:** [Batching](#batching) · [State](#state)

**Sources:**

[react.dev/learn/state-as-a-snapshot](https://react.dev/learn/state-as-a-snapshot)

Principles for structuring state

</details>

<details>
  <summary>Principles for structuring state</summary>

### Principles for structuring state

**Definition:**

Guidelines for choosing what to store in state so updates stay predictable and components stay simple.

1. **Group related state** — merge variables always updated together
2. **Avoid contradictions** — e.g. `isSending` + `isSent` both true
3. **Avoid redundancy** — don't store what you can compute during render
4. **Avoid duplication** — same data in two state variables drifts
5. **Avoid deep nesting** — prefer flat state + IDs

**When to use:** Before adding `useState`, ask: can this be computed? Is it duplicated elsewhere? Will two flags contradict?

**Examples:**

```tsx
// Bad — redundant
const [firstName, setFirstName] = useState("");
const [lastName, setLastName] = useState("");
const [fullName, setFullName] = useState(""); // derive instead!

// Good
const fullName = `${firstName} ${lastName}`;
```

**Thoughts:** _Minimize state surface — derive during render when possible._

**See also:** [State](#state) · [Purity](#purity)

**Sources:**

[react.dev/learn/choosing-the-state-structure](https://react.dev/learn/choosing-the-state-structure)

Lifting state up

</details>

<details>
  <summary>Lifting state up</summary>

### Lifting state up

**Definition:** Move shared state to the **closest common ancestor** of components that need it; pass data and callbacks down as props.

**How it works:**

1. Identify siblings that need the same data (search input + filtered list)
2. Find their nearest shared parent
3. Move `useState` to that parent
4. Pass `value` + `onChange` (or derived data) down as props

State used by one component only — keep local. Deep drilling → composition (`children`), a state library, or context via [React — Hooks](./4_react_hooks.md).

**When to use:** Two or more siblings share data or must stay in sync.

**Examples:**

```tsx
function Page() {
  const [query, setQuery] = useState("");
  return (
    <>
      <SearchInput value={query} onChange={setQuery} />
      <FilteredList query={query} />
    </>
  );
}
```

**Thoughts:** _Shared state lives in the lowest common ancestor._

**See also:** [State vs props](#state-vs-props) · [Principles for structuring state](#principles-for-structuring-state)

**Sources:**

[react.dev/learn/sharing-state-between-components](https://react.dev/learn/sharing-state-between-components)

Controlled vs uncontrolled inputs

</details>

<details>
  <summary>Controlled vs uncontrolled inputs</summary>

### Controlled vs uncontrolled inputs

**Definition:**

**Controlled** inputs get `value` from React state; **uncontrolled** inputs keep value in the DOM and are read via refs.

|              | Controlled                      | Uncontrolled                            |
| ------------ | ------------------------------- | --------------------------------------- |
| Value source | React state                     | DOM (`ref.current.value`)               |
| When         | Default for forms in React apps | File inputs, integrating non-React libs |
| API          | `value` + `onChange`            | `ref` + `defaultValue`                  |

**Examples:**

```tsx
// Controlled
<input value={email} onChange={e => setEmail(e.target.value)} />

// Uncontrolled
<input ref={inputRef} defaultValue="" />
```

**Thoughts:** React 19 form Actions reduce boilerplate for uncontrolled-style forms with server mutations.

**Sources:**

[react.dev/reference/react-dom/components/input](https://react.dev/reference/react-dom/components/input)

State normalization

</details>

<details>
  <summary>State normalization</summary>

### State normalization

**Definition:** Store collections as `{ ids: Id[], entities: Record<Id, Entity> }` instead of nested arrays for O(1) lookup and targeted updates.

**How it works:** `ids` preserves order for rendering; `entities` maps id → object. Update one entity by id without scanning the whole array. Selectors derive filtered/sorted views from normalized store.

**When to use:** Large lists, frequent per-item updates, Redux/Apollo normalized caches. Skip for small static lists.

**Pluses and minuses:**

| +                                        | −                                 |
| ---------------------------------------- | --------------------------------- |
| Fast lookup by id                        | More setup than plain arrays      |
| Update one entity without scanning array | Denormalized views need selectors |
| Fewer unnecessary re-renders             |                                   |

**Examples:**

```tsx
// Instead of users: User[]
type UsersState = {
  userIds: number[];
  usersById: Record<number, User>;
};

// Read
const user = state.usersById[2];

// Update one user
state.usersById[2] = { ...state.usersById[2], name: "Ivan" };

// Render list
state.userIds.map((id) => <User key={id} user={state.usersById[id]} />);
```

**Thoughts:**

_Ids for order, map for lookup — update one row without touching the whole list._

Redux Toolkit `createEntityAdapter`, Apollo/Relay normalize GraphQL cache automatically. TanStack Query stores server shape as-is — normalize in `select` if needed. Unrelated to SQL 1NF–3NF — that is [9_database.md — Normal forms](./9_database_core.md#normal-forms-1nf3nf-bcnf).

**Sources:**

[redux-toolkit.js.org/api/createEntityAdapter](https://redux-toolkit.js.org/api/createEntityAdapter)

---

</details>
