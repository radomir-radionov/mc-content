# React — Patterns

Repeatable **component and state shapes** — how to place state, compose UI, and avoid effect smells in real features.

[← Back to hub](./4_react.md)

### Related docs

- [React — Algorithms](./4_react_algorithms.md) · [React — Performance](./4_react_perf.md) · [React — Concurrent UI](./4_react_concurrent.md) · [React — Resilience](./4_react_resilience.md) · [Core](./4_react_core.md) · [Composition patterns](./4_react_core.md#composition-patterns) · [Architecture patterns](./7_special_architecture.md)

### Main concepts

| Concept                         | Covers                                               |
| ------------------------------- | ---------------------------------------------------- |
| **Thinking in React**           | Component tree, minimal state, lift to common parent |
| **Controlled vs uncontrolled**  | Props-driven vs local state for inputs and widgets   |
| **State colocation & lifting**  | Where `useState` lives; one source of truth          |
| **Derived state & effects**     | Compute in render; effects for external systems only |
| **Compound components**         | Context-backed families (`Tabs`, `Select`)           |
| **Custom hooks**                | Extract stateful logic; replace HOC/render props     |
| **Legacy HOC and render props** | Pre-Hooks reuse patterns; reading and migrating libs |
| **Container vs UI**             | Split data wiring from presentational markup         |
| **When to memoize**             | `memo` / `useMemo` / `useCallback` after profiling   |
| **Provider pattern**            | Context supply without compound children API         |
| **Portal pattern**              | Modals, tooltips outside DOM hierarchy               |
| **Slots pattern**               | `children` + named props for layout regions          |
| **Finite state machine**        | Explicit states vs boolean soup                      |
| **Prop drilling alternatives**  | When to lift, context, or colocate                   |
| **Controlled forms pattern**    | One object or Field API for multi-field forms        |

### Sections

- [Overview](#overview)
- [Thinking in React](#thinking-in-react)
- [State patterns](#state-patterns)
- [Component patterns](#component-patterns)
- [Performance patterns](#performance-patterns)

---

## Overview

<details>
  <summary>Overview</summary>

### Overview

**Definition:** **Patterns** are repeatable ways to structure components and state — composition, lifting, colocation, compound components, and knowing when to memoize.

**How it works:** Theory for composition and reconciliation lives in [Core](./4_react_core.md). This topic is **applied practice**: choosing a pattern under time pressure (features, interviews, code review).

**When to use:** New screens, refactors from HOC/render-prop legacy, design-system widgets.

**How to pick a pattern (quick map):**

| Problem                         | Start here                                                             |
| ------------------------------- | ---------------------------------------------------------------------- |
| Siblings must stay in sync      | [State colocation & lifting](#state-colocation-lifting)                |
| Shared logic in many components | [Custom hooks pattern](#custom-hooks-pattern)                          |
| Flexible widget API             | [Compound components](#compound-components) or [Slots](#slots-pattern) |
| Modal / tooltip DOM             | [Portal pattern](#portal-pattern)                                      |
| `isLoading` + `isError` soup    | [Finite state machine](#finite-state-machine)                          |

**Thoughts:** Patterns are **choices**, not badges — the simplest structure that stays correct wins.

**See also:** [Algorithms](./4_react_algorithms.md) — debounce, pagination, optimistic UI.

**Sources:**

[patterns.dev/react](https://www.patterns.dev/react/) · [react.dev/learn/thinking-in-react](https://react.dev/learn/thinking-in-react)

Thinking in React

</details>

## Thinking in React

<details>
  <summary>Thinking in React</summary>

### Thinking in React

**Definition:** Break the UI into a **component tree**, find **state** that must be shared, **lift** it to the lowest common parent, and pass **props down** + **events up** (one-way data flow).

**How it works:**

1. Draw boxes around every UI region that can change independently
2. Name components — usually one component per box on the mockup
3. Build a static version with props only (no shared state yet)
4. Find minimal state: does it change over time? Can a parent compute it from props? If yes → not state
5. Place state in the parent that owns both consumers (e.g. filter text + table rows → `FilterableProductTable`)

**Minimal state checklist** (if answer is “yes”, it is probably **not** state):

| Question                                      | If yes →                         |
| --------------------------------------------- | -------------------------------- |
| Does it stay the same if props do not change? | Derive in render                 |
| Can you compute it from other state/props?    | Derive in render                 |
| Is it an event / interaction timestamp?       | Often ref or parent handler only |
| Is it only needed by one child?               | Colocate in that child           |

**Pluses and minuses:**

| +                                      | −                                      |
| -------------------------------------- | -------------------------------------- |
| Predictable one-way data flow          | Deep trees need context or composition |
| Easy to test leaf components           | Over-lifting creates “god parent”      |
| Matches how React reconciliation works | Requires discipline on prop shapes     |

**When to use:** New screens, legacy JSX spaghetti, interview “build a searchable table” tasks.

**Examples:**

```tsx
function FilterableList({ items }: { items: Item[] }) {
  const [query, setQuery] = useState("");
  const visible = items.filter((item) =>
    item.name.toLowerCase().includes(query.toLowerCase()),
  );
  return (
    <>
      <SearchBar value={query} onChange={setQuery} />
      <ItemTable rows={visible} />
    </>
  );
}
```

**See also:** [Composition patterns](./4_react_core.md#composition-patterns) · [Lifting state up](./4_react_core.md#lifting-state-up)

**Sources:**

[react.dev/learn/thinking-in-react](https://react.dev/learn/thinking-in-react)

State patterns

</details>

### State patterns

<details>
  <summary>State colocation & lifting</summary>

### State colocation & lifting

**Definition:** **Colocate** state with the component that needs it; **lift** only when siblings must stay in sync. Prefer **one source of truth** over duplicated `useState` in children.

**How it works:**

| Situation                        | Pattern                                 |
| -------------------------------- | --------------------------------------- |
| Toggle only inside one panel     | `useState` in that panel                |
| Search + list must match         | Lift `query` to parent; list is derived |
| Deep tree, stable theme/auth     | Context (not a high-frequency store)    |
| Many fields, complex transitions | `useReducer` or small state machine     |

**When to use:** Before adding Context or Redux — most bugs are wrong state placement, not missing libraries.

**Common pitfalls:**

- **Duplicated state** — two inputs that should match each have their own `useState`
- **Lift too high** — entire app re-renders on every keystroke in a local search field
- **Context for everything** — high-frequency updates through context tank performance
- **Effect to sync props → state** — use controlled component or derive instead

**Pluses and minuses:**

| +                | −                                  |
| ---------------- | ---------------------------------- |
| Fewer sync bugs  | Wrong lift point hurts performance |
| Easier reasoning | Refactors when UI layout changes   |

**Examples:**

```tsx
function Cart({ items, coupon }: Props) {
  const total = items.reduce((sum, i) => sum + i.price, 0);
  const discounted = coupon ? total * 0.9 : total;
  return <p>Total: {discounted}</p>;
}
```

**See also:** [`useReducer`](./4_react_hooks.md#usereducer) · [Redux](./4_react_redux.md)

**Sources:**

[react.dev/learn/sharing-state-between-components](https://react.dev/learn/sharing-state-between-components)

Derived state & effects

</details>

<details>
  <summary>Derived state & effects</summary>

### Derived state & effects

**Definition:** Values computable from props or other state should be **derived in render**, not copied into `useState` + `useEffect`. Effects synchronize with **external systems** (network, DOM, subscriptions), not with React props.

**How it works:**

| Avoid                                         | Prefer                                         |
| --------------------------------------------- | ---------------------------------------------- |
| `useEffect(() => setX(transform(props)), …)`  | `const x = transform(props)` in render         |
| `useEffect` to set loading `true` then fetch  | `use` / Query / `startTransition` + Actions    |
| Storing `selected = items.find(...)` in state | `const selected = items.find(...)` each render |

**When to use:** Code review smell: effect that only sets state from props; interview “fix unnecessary re-renders / loops”.

**Common pitfalls:**

- `useEffect(() => setItems(props.items), [props.items])` — items already in props; render `props.items`
- Fetch in effect when parent/route loader can provide data (RSC, Query, loader)
- Missing cleanup → race when `userId` changes quickly (pair with [race-safe request id](./4_react_algorithms.md#race-safe-request-id))

**Thoughts:** If the effect has **no external system** (network, DOM, subscription), question whether it should exist.

**Examples:**

```tsx
function UserRow({ users, selectedId }: Props) {
  const selected = users.find((u) => u.id === selectedId);
  return selected ? <Profile user={selected} /> : <Empty />;
}
```

**See also:** [You might not need an effect](./4_react_hooks.md#you-might-not-need-an-effect)

**Sources:**

[react.dev/learn/you-might-not-need-an-effect](https://react.dev/learn/you-might-not-need-an-effect)

Compound components

</details>

<details>
  <summary>Compound components</summary>

### Compound components

**Definition:** A small **family** of components that share implicit state via Context (tabs, select, accordion) while exposing a composable JSX API — `Tabs`, `Tabs.List`, `Tabs.Panel`.

**How it works:** Parent holds active index/id; children read context. Consumers compose markup without prop-drilling `activeTab` through every layer.

**When to use:** Design systems, reusable widgets with flexible layout; prefer over render props/HOC for new code.

**How to implement safely:**

1. Export **only** compound parts from one module (avoid orphan `Panel` without `Tabs`)
2. Context default `null` + hook throws if used outside provider (fail fast in dev)
3. Document which subcomponents are required vs optional
4. Keep context value small — `{ active, setActive }`, not whole app store

**Pluses and minuses:**

| +                        | −                                    |
| ------------------------ | ------------------------------------ |
| Flexible consumer markup | Harder for beginners to discover API |
| Encapsulated behavior    | Testing needs provider wrapper       |

**Examples:**

```tsx
const TabsContext = createContext<{
  active: string;
  setActive: (id: string) => void;
} | null>(null);

export function Tabs({
  children,
  defaultId,
}: {
  children: React.ReactNode;
  defaultId: string;
}) {
  const [active, setActive] = useState(defaultId);
  return <TabsContext value={{ active, setActive }}>{children}</TabsContext>;
}
```

**See also:** [`createContext`](./4_react_apis.md#createcontext) · [Composition patterns](./4_react_core.md#composition-patterns)

**Sources:**

[react.dev/learn/passing-data-deeply-with-context](https://react.dev/learn/passing-data-deeply-with-context)

Controlled vs uncontrolled

</details>

<details>
  <summary>Controlled vs uncontrolled</summary>

### Controlled vs uncontrolled

**Definition:** A component is **controlled** when its important UI state is driven by **props** (`value` + `onChange` from parent). **Uncontrolled** keeps that state in **local `useState`** (or DOM refs for forms).

**How it works:**

| Style        | Who owns state | Parent can                        |
| ------------ | -------------- | --------------------------------- |
| Controlled   | Parent         | Reset, sync siblings, persist URL |
| Uncontrolled | Child          | Use with less wiring              |

Prefer **fully controlled** when two widgets must stay in sync (dual inputs, accordion with one open panel). Keep **uncontrolled** for isolated leaves.

**When to use:** Forms in wizards, design-system inputs, “sync these two fields” tasks.

**Examples:**

```tsx
function Toggle({
  isOn,
  onChange,
}: {
  isOn: boolean;
  onChange: (v: boolean) => void;
}) {
  return (
    <button type="button" onClick={() => onChange(!isOn)}>
      {isOn ? "On" : "Off"}
    </button>
  );
}
```

**See also:** [State colocation & lifting](#state-colocation-lifting)

**Sources:**

[react.dev/learn/sharing-state-between-components](https://react.dev/learn/sharing-state-between-components)

</details>

### Component patterns

<details>
  <summary>Custom hooks pattern</summary>

### Custom hooks pattern

**Definition:** Move **stateful logic** (state + effects + handlers) into `useSomething()` so components stay mostly JSX — replaces most **HOC** and **render props**.

**How it works:** Hook returns `{ data, actions }` or values; call only at component top level (Rules of Hooks). Name **`use` + PascalCase** — linters enforce this.

**When to use:** Shared fetch status, media queries, form helpers, keyboard shortcuts.

**Pluses and minuses:**

| +                           | −                                      |
| --------------------------- | -------------------------------------- |
| No wrapper tree             | Does not share state between instances |
| Composable in one component | Easy to over-extract tiny hooks        |

**Examples:**

```tsx
function useOnlineStatus() {
  const [online, setOnline] = useState(navigator.onLine);
  useEffect(() => {
    const on = () => setOnline(true);
    const off = () => setOnline(false);
    window.addEventListener("online", on);
    window.addEventListener("offline", off);
    return () => {
      window.removeEventListener("online", on);
      window.removeEventListener("offline", off);
    };
  }, []);
  return online;
}
```

**See also:** [Hooks](./4_react_hooks.md)

**Sources:**

[react.dev/learn/reusing-logic-with-custom-hooks](https://react.dev/learn/reusing-logic-with-custom-hooks)

Container vs presentational

</details>

<details>
  <summary>Container vs presentational</summary>

### Container vs presentational

**Definition:** **Presentational** components render from props; **containers** (pages or hooks) own data and mutations.

**How it works:** `useUsers()` + `<UserList users={...} />` — list is pure; page wires navigation and fetch.

**When to use:** Tables, charts, cards you want in Storybook without mocking the whole app.

**Pluses and minuses:**

| +                             | −                               |
| ----------------------------- | ------------------------------- |
| Easy Storybook / visual tests | “Container” can become god page |
| Clear props contract          | Over-splitting tiny wrappers    |

**Examples:**

```tsx
function UserList({
  users,
  onSelect,
}: {
  users: User[];
  onSelect: (id: string) => void;
}) {
  return (
    <ul>
      {users.map((u) => (
        <li key={u.id}>
          <button type="button" onClick={() => onSelect(u.id)}>
            {u.name}
          </button>
        </li>
      ))}
    </ul>
  );
}

function UsersPage() {
  const { data, isLoading } = useUsers();
  const navigate = useNavigate();
  if (isLoading) return <Spinner />;
  return <UserList users={data} onSelect={(id) => navigate(`/users/${id}`)} />;
}
```

**Thoughts:** _Hooks replaced most class “container” components — the split is now **page + hook** vs **dumb UI**._

**See also:** [Custom hooks pattern](#custom-hooks-pattern) · [Container vs presentational in interview](./4_react_interview.md)

**Sources:**

[react.dev/learn/thinking-in-react](https://react.dev/learn/thinking-in-react)

Legacy HOC and render props

</details>

<details>
  <summary>Legacy HOC and render props</summary>

### Legacy HOC and render props

**Definition:** Two **pre-Hooks** ways to reuse stateful logic across components: a **higher-order component (HOC)** wraps a component and injects props; a **render prop** passes a function child that receives data and returns UI. Modern React shares the same logic with **custom Hooks** instead.

**How it works:**

| Pattern         | Shape                                          | Who owns the UI                        |
| --------------- | ---------------------------------------------- | -------------------------------------- |
| **HOC**         | `const Enhanced = withX(Wrapped)`              | Wrapped component renders fixed markup |
| **Render prop** | `<Data render={(data) => <UI data={data} />}>` | Caller chooses JSX inside the callback |
| **Custom Hook** | `const data = useX(); return <UI ...>`         | Caller composes hooks + JSX explicitly |

**HOC (legacy):** A function `withSomething(Component)` returns a new component that runs effects/state, then renders `<Component {...injected} {...props} />`. Classic examples: `connect` (Redux), `withRouter` (React Router v5), `withTheme`, auth gates.

**Render prop (legacy):** Parent holds state; child is a function: `children={(value) => ...}` or prop `render={(value) => ...}`. Classic examples: `<Mouse render={({ x, y }) => ...} />`, old context consumers as functions.

**Wrapper hell:** Stacking HOCs (`withA(withB(withC(Page))))` hides prop origin, complicates TypeScript, breaks DevTools display names, and makes ref forwarding awkward (`forwardRef` + `displayName`).

**Pluses and minuses:**

| + (why teams used them)            | − (why Hooks won)                            |
| ---------------------------------- | -------------------------------------------- |
| Reuse without duplicating effects  | Deep wrapper trees in DevTools               |
| Encapsulate cross-cutting concerns | Injected props are implicit                  |
| Render props keep UI flexible      | Callback props change identity → memo breaks |

**When to use:**

- **Read** third-party or old codebases (class era, Redux `connect`, libraries still shipping HOCs).
- **Migrate** new features to [Custom hooks pattern](#custom-hooks-pattern); do not add new HOC factories (eslint `component-hook-factories` discourages nested component factories).
- **Keep** render-prop APIs in libraries when callers must control markup — but expose a **hook + component** pair when you maintain the library.

**Examples:**

```tsx
// HOC style (legacy)
function withUser<P extends object>(
  Component: React.ComponentType<P & { user: User }>,
) {
  return function WithUser(props: P) {
    const user = useUser("/api/user"); // today: just use the hook in the page
    return <Component {...props} user={user} />;
  };
}

// Render prop style (legacy)
function Mouse({
  render,
}: {
  render: (pos: { x: number; y: number }) => React.ReactNode;
}) {
  const [pos, setPos] = useState({ x: 0, y: 0 });
  useEffect(() => {
    const move = (e: MouseEvent) => setPos({ x: e.clientX, y: e.clientY });
    window.addEventListener("mousemove", move);
    return () => window.removeEventListener("mousemove", move);
  }, []);
  return <>{render(pos)}</>;
}

// Hook replacement (preferred)
function useMousePosition() {
  const [pos, setPos] = useState({ x: 0, y: 0 });
  useEffect(() => {
    const move = (e: MouseEvent) => setPos({ x: e.clientX, y: e.clientY });
    window.addEventListener("mousemove", move);
    return () => window.removeEventListener("mousemove", move);
  }, []);
  return pos;
}

function Tracker() {
  const { x, y } = useMousePosition();
  return (
    <p>
      {x}, {y}
    </p>
  );
}
```

**Migration checklist:**

1. Find **state/effects** inside the HOC or render-prop parent — move to `use…()`.
2. Replace wrapper with **plain component** that calls the hook and passes props explicitly.
3. If the library needs a render prop for flexibility, add **`useLibraryFeature()`** alongside the component.
4. Set **`displayName`** on any remaining wrappers for debugging.

**Interview angle:** “HOC is a function that takes a component and returns a component; render props invert control of JSX. Hooks share **logic**, not a wrapper tree — same state rules (Rules of Hooks), clearer data flow.”

**Thoughts:** _Redux `connect` and router HOCs are the most common fossils; RTK + hooks and data routers replaced them in greenfield apps._

**See also:** [Custom hooks pattern](#custom-hooks-pattern) · [Container vs presentational](#container-vs-presentational) · [Interview — HOCs](./4_react_interview.md#what-are-higher-order-components-hocs) · [Core — composition over inheritance](./4_react_core.md#composition-patterns)

**Sources:**

[react.dev/learn/reusing-logic-with-custom-hooks](https://react.dev/learn/reusing-logic-with-custom-hooks) · [react.dev/reference/eslint-plugin-react-hooks/lints/component-hook-factories](https://react.dev/reference/eslint-plugin-react-hooks/lints/component-hook-factories)

State reducer pattern

</details>

<details>
  <summary>State reducer pattern</summary>

### State reducer pattern

**Definition:** Centralize transitions in a **reducer** with typed actions instead of many `setState` calls — scales better than boolean soup.

**How it works:** `dispatch({ type: "set", field: "email", value })` — one place for validation and side effects.

**When to use:** Multi-step forms, wizards, complex toggles; libraries exposing `onStateChange` hooks.

**Examples:**

```tsx
type FormState = { email: string; status: "idle" | "submitting" | "error" };
type Action =
  | { type: "field"; field: "email"; value: string }
  | { type: "submit" }
  | { type: "fail" };

function reducer(state: FormState, action: Action): FormState {
  switch (action.type) {
    case "field":
      return { ...state, email: action.value };
    case "submit":
      return { ...state, status: "submitting" };
    case "fail":
      return { ...state, status: "error" };
    default:
      return state;
  }
}
```

**vs many `useState`:** One `dispatch` log in devtools; easier to test pure reducer; pairs with [Finite state machine](#finite-state-machine) when states are explicit.

**Pitfalls:** Putting **async** inside reducer — keep side effects in `useEffect` or event handlers that dispatch results.

**See also:** [`useReducer`](./4_react_hooks.md#usereducer) · [Wizard step algorithm](./4_react_algorithms.md#wizard-step-algorithm)

**Sources:**

[react.dev/reference/react/useReducer](https://react.dev/reference/react/useReducer)

When to memoize

</details>

### Performance patterns

<details>
  <summary>When to memoize</summary>

### When to memoize

**Definition:** **`memo`**, **`useMemo`**, and **`useCallback`** skip work when inputs are unchanged — **optimizations**, not correctness. Profile first; React Compiler may auto-memoize.

**How it works:**

| Tool              | Skips                                    | Needs                                      |
| ----------------- | ---------------------------------------- | ------------------------------------------ |
| `memo(Component)` | Child re-render when props shallow-equal | Stable props (avoid inline `{}` / `() =>`) |
| `useMemo`         | Expensive pure calculation               | Correct dependency array                   |
| `useCallback`     | New function identity each render        | Passed to memoized children                |

**When to use:** Heavy list rows, charts, editors — not every leaf button.

**Common pitfalls:**

- `useCallback(() => fn(), [fn])` — unstable deps defeat memo
- `useMemo(() => ({ ... }), [x])` — new object every time → child memo useless
- Memoizing **everything** — costs comparison; measure first ([Measure before memo](./4_react_perf.md#measure-before-memo))

**Thoughts:** Fix data flow first; memo is the last mile.

**See also:** [`memo`](./4_react_apis.md#memo) · [List rendering algorithm](./4_react_algorithms.md#list-rendering-algorithm)

**Sources:**

[react.dev/reference/react/memo](https://react.dev/reference/react/memo)

</details>

<details>
  <summary>Context splitting</summary>

### Context splitting

**Definition:** Split unrelated values into **separate context providers** so updating theme does not re-render consumers of auth, and vice versa.

**How it works:**

- Bad: one giant `AppContext` with `{ theme, user, cart }` — any change re-renders all consumers
- Good: `ThemeProvider`, `AuthProvider`, `CartProvider` nested; components subscribe only to what they need
- For high-frequency updates (mouse position, scroll), prefer **refs + subscriptions** or external store — not raw Context

**When to use:** App shell with theme + user; performance issues from “everything re-renders on cart change”.

**See also:** [`createContext`](./4_react_apis.md#createcontext) · [Redux](./4_react_redux.md)

**Sources:**

[react.dev/learn/passing-data-deeply-with-context](https://react.dev/learn/passing-data-deeply-with-context)

Provider pattern

</details>

<details>
  <summary>Provider pattern</summary>

### Provider pattern

**Definition:** Wrap a subtree with **`<Context value={...}>`** (React 19) so descendants read shared data via `useContext` without prop drilling.

**How it works:** One module exports `ThemeContext` + `ThemeProvider` + `useTheme()` hook that throws if missing provider. Keep value **stable** or memoized to limit re-renders.

**When to use:** Theme, locale, auth snapshot — not high-frequency cart line items (use store/Query).

**See also:** [Context splitting](#context-splitting) · [`createContext`](./4_react_apis.md#createcontext)

**Sources:**

[react.dev/learn/passing-data-deeply-with-context](https://react.dev/learn/passing-data-deeply-with-context)

Portal pattern

</details>

<details>
  <summary>Portal pattern</summary>

### Portal pattern

**Definition:** **`createPortal(children, domNode)`** renders UI into another DOM node (often `document.body`) while keeping React event bubbling and state in the same tree.

**How it works:** Modal overlay, tooltip, toast — markup escapes `overflow: hidden` ancestors. Pair with focus trap and `aria-modal` for a11y.

**When to use:** Dialogs, dropdowns that must paint on top of app chrome.

**See also:** [Portals](./4_react_hooks.md) · [Components — legacy portal](./4_react_apis.md)

**Sources:**

[react.dev/reference/react-dom/createPortal](https://react.dev/reference/react-dom/createPortal)

Slots pattern

</details>

<details>
  <summary>Slots pattern</summary>

### Slots pattern

**Definition:** Pass UI fragments as **`children`** or **named props** (`header`, `footer`, `actions`) so a layout component does not know about page data.

**How it works:**

```tsx
<Card header={<Title />} footer={<SaveButton />}>
  <FormFields />
</Card>
```

Prefer `children` for single main content; named slots when regions have fixed roles.

**When to use:** Page shells, modals, reusable panels in design systems.

**See also:** [Composition patterns](./4_react_core.md#composition-patterns) · [Compound components](#compound-components)

**Sources:**

[patterns.dev/react/composition-pattern](https://www.patterns.dev/react/composition-pattern/)

Finite state machine

</details>

<details>
  <summary>Finite state machine</summary>

### Finite state machine

**Definition:** Model UI as **finite states** and **transitions** (`idle` → `loading` → `success` | `error`) instead of many booleans (`isLoading`, `isError`, `isDone`).

**How it works:** `useReducer` with discriminated union state, or libraries (XState) for complex flows. One state active at a time → impossible combinations disappear.

**When to use:** Upload widgets, payment flow, data fetch with retry UI.

**Interview angle:** Replace three booleans with one state — “can `isLoading` and `isError` both be true?” If not, use a single status enum.

**Pluses and minuses:**

| +                              | −                          |
| ------------------------------ | -------------------------- |
| Invalid states unrepresentable | More boilerplate than bool |
| Clear UI mapping per state     | XState heavy for tiny UI   |

**Examples:**

```tsx
type Status =
  | { kind: "idle" }
  | { kind: "loading" }
  | { kind: "error"; message: string };

function reducer(
  _: Status,
  action: { type: "fetch" } | { type: "fail"; message: string },
): Status {
  switch (action.type) {
    case "fetch":
      return { kind: "loading" };
    case "fail":
      return { kind: "error", message: action.message };
    default:
      return _;
  }
}
```

**See also:** [Wizard step algorithm](./4_react_algorithms.md#wizard-step-algorithm) · [`useReducer`](./4_react_hooks.md#usereducer)

**Sources:**

[react.dev/learn/extracting-state-logic-into-a-reducer](https://react.dev/learn/extracting-state-logic-into-a-reducer)

Controlled forms pattern

</details>

<details>
  <summary>Controlled forms pattern</summary>

### Controlled forms pattern

**Definition:** Keep form values in **React state** (single object or library Field API) and wire inputs with **`value` + `onChange`** — or use **React 19 Actions** / `useActionState` for server-backed forms.

**How it works:**

1. **One object state:** `const [form, setForm] = useState({ email: "", role: "user" })` — update with spread per field
2. **Validation:** derive `errors` on submit or on blur; block submit when invalid
3. **Arrays:** map fields to list; update with immutable [nested update](./4_react_algorithms.md#immutable-nested-update)
4. **Libraries:** React Hook Form / TanStack Form for large forms — still “controlled” at integration boundary

**When to use:** Multi-field signup, settings panels; prefer Actions when mutation is server-first (Next.js).

**Pluses and minuses:**

| +                         | −                                      |
| ------------------------- | -------------------------------------- |
| Full control in React     | Verbose for huge forms without library |
| Easy reset via `setState` | Easy to re-render whole form each key  |

**See also:** [Controlled vs uncontrolled](#controlled-vs-uncontrolled) · [Async validation algorithm](./4_react_algorithms.md#async-validation-algorithm)

**Sources:**

[react.dev/reference/react-dom/components/form](https://react.dev/reference/react-dom/components/form)

Prop drilling alternatives

</details>

<details>
  <summary>Prop drilling alternatives</summary>

### Prop drilling alternatives

**Definition:** **Prop drilling** = passing props through layers that do not use them. Alternatives depend on **who owns** the data and **update frequency**.

**How it works:**

| Situation              | Prefer                                    |
| ---------------------- | ----------------------------------------- |
| 1–2 levels             | Keep drilling — simplest                  |
| Stable theme/auth      | Context + custom hook                     |
| Server list + cache    | TanStack Query, colocate fetch in leaf    |
| Global client workflow | Redux / Zustand sparingly                 |
| Composition            | Pass `children` from parent that has data |

**When to use:** Code review “why Context everywhere?” — default to colocation first.

**Examples (composition over drilling):**

```tsx
function Page({ user }: { user: User }) {
  return (
    <Layout header={<UserMenu user={user} />}>
      <Dashboard user={user} />
    </Layout>
  );
}
```

Parent passes **already-built** nodes instead of `user` through `Layout` → `Header` → `Menu`.

**Thoughts:** Drill props through **one** layout level is fine; Context is for **wide** stable data.

**See also:** [State colocation & lifting](#state-colocation-lifting) · [Ecosystem](./4_react_ecosystem.md)

**Sources:**

[react.dev/learn/passing-data-deeply-with-context](https://react.dev/learn/passing-data-deeply-with-context)

</details>
