# React — Hooks

Hooks reference and escape hatches.

[← Back to hub](./4_react.md)

### Related docs

- [React — Core](./4_react_core.md) · [React — Hooks](./4_react_hooks.md) · [React — Components](./4_react_components.md) · [React — APIs](./4_react_apis.md) · [React — React 19](./4_react_server.md) · [React — Ecosystem](./4_react_ecosystem.md) · [React — Interview](./4_react_interview.md)

### Sections

- [5. Hooks reference](#5-hooks-reference)
- [6. Escape hatches](#6-escape-hatches)

---

## 5. Hooks reference

### Overview

Built-in hooks catalog

<details>
  <summary>Built-in hooks catalog</summary>

### Built-in hooks catalog

**Definition:** Index of React built-in hooks grouped by responsibility — state, effects, refs, performance, and React 19 APIs.

**How it works:**

1. **Mutable UI data?** → `useState` or `useReducer`
2. **Sync with external system?** → `useEffect` / `useLayoutEffect`
3. **DOM or value without re-render?** → `useRef`
4. **Shared data without drilling?** → `useContext`
5. **Defer expensive render?** → `useTransition` / `useDeferredValue`
6. **Cache calc or stable callback?** → `useMemo` / `useCallback` (profile first)

| Category        | Hooks                                                                       |
| --------------- | --------------------------------------------------------------------------- |
| **State**       | `useState`, `useReducer`, `useActionState - 19`, `useOptimistic - 19`       |
| **Context**     | `useContext`, `use() - 19`                                                  |
| **Ref**         | `useRef`, `useImperativeHandle`, `ref as prop - 19`                         |
| **Effect**      | `useEffect`, `useLayoutEffect`, `useInsertionEffect`, `useEffectEvent - 19` |
| **Performance** | `useMemo`, `useCallback`, `useTransition`, `useDeferredValue`               |
| **Other**       | `useId`, `useSyncExternalStore`, `useDebugValue`, `useFormStatus - 19`      |

**Examples:**

```tsx
const [n, setN] = useState(0); // UI data
const ref = useRef<HTMLInputElement>(null); // no re-render
useEffect(() => {
  document.title = String(n);
}, [n]); // after paint
```

**Sources:**

[react.dev/reference/react/hooks](https://react.dev/reference/react/hooks)

Rules of Hooks

</details>

<details>
  <summary>Rules of Hooks</summary>

### Rules of Hooks

**Definition:** Two invariant rules that keep React's hook linked-list state consistent across renders. Part of [Rules of Hooks](./4_react_core.md#rules-of-hooks) on Core (see also [Rules of React catalog](./4_react_core.md#rules-of-react-catalog)).

**How it works:**

React stores hooks on the fiber as a **linked list** — first `useState` is slot 0, first `useEffect` is slot 1, etc. Each render walks the list in the **same order**. Conditional hooks shift slots → state attaches to wrong hook → crashes or silent bugs.

1. Only call hooks at the **top level** — not inside conditions, loops, nested functions, `try`/`catch`/`finally`, or after an early `return`
2. Only call hooks from **React function components** or **custom hooks** (`use`\*)

**When to use:** Always. Enable `eslint-plugin-react-hooks` (`rules-of-hooks` + `exhaustive-deps`). Interview follow-up: _“If I need a hook only when logged in, I still call it every render and put `if (loggedIn)` inside the effect — I never skip the hook call.”_

**Examples:**

```tsx
// Bad — conditional hook
if (loggedIn) useEffect(() => {}, []);

// Good — condition inside effect
useEffect(() => {
  if (loggedIn) subscribe();
}, [loggedIn]);
```

**Thoughts:** _Same hooks, same order, every render._

**See also:** [Rules of Hooks (Core)](./4_react_core.md#rules-of-hooks) · [Custom hooks](#custom-hooks) · [React Fiber](./4_react_core.md#react-fiber)

**Sources:**

[react.dev/reference/rules/rules-of-hooks](https://react.dev/reference/rules/rules-of-hooks)

useState

</details>

### State

<details>
  <summary>useState</summary>

### useState

**Definition:** `useState(initial)` declares a state variable and a `set` function. The setter schedules a **next snapshot** — it does not mutate the value in the current render.

**How it works:**

- React stores the value on the fiber. First render uses `initial`; later renders ignore `initial`.
- Pass a **function** as initial (`useState(createTodos)`, not `createTodos()`) for **lazy init** — React calls it only on first mount. In Strict Mode (dev) the initializer may run twice; keep it **pure**.
- `setX(next)` or `setX(prev => next)`. Updaters are queued and applied in order on the next render — needed when several updates happen in **one event** (`setN(n => n + 1)` twice → +2).
- `set` does **not** change the local variable in the running handler: after `setCount(1)`, `count` is still the old snapshot until the next render.
- If `Object.is(next, current)`, React **skips** re-render. Mutating then `setItems(items)` is a no-op.
- `set` identity is **stable** (safe to omit from effect deps). React [batches](./4_react_core.md#batching) updates after event handlers.
- To **store a function** in state, wrap it: `useState(() => fn)` / `setFn(() => otherFn)` — otherwise React treats it as initializer/updater.

_“State is a snapshot per render. I update immutably so `Object.is` sees a new reference. I use an updater when the next value depends on the previous one in the same event.”_

**When to use:**

Local UI — toggles, inputs, selections, modals. **Not** derived display data (compute in render) or server cache (Query / RSC).

| API                     | Use                                         |
| ----------------------- | ------------------------------------------- |
| `useState(initial)`     | Cheap initial value                         |
| `useState(createTodos)` | Expensive init — pass the function, no `()` |
| `setN(n => n + 1)`      | Next state from previous in same event      |
| `setN(count + 1)`       | Fine when you already have the snapshot     |

#### Common pitfalls

`onClick={fn()}` (calls during render → “Too many re-renders”). Logging `count` right after `setCount` still shows the old value. `useState(expensive())` recomputes `expensive` every render.

**Pluses and minuses:**

| +                                      | −                                              |
| -------------------------------------- | ---------------------------------------------- |
| Simple mental model, stable setter     | Easy to mutate objects and skip updates        |
| Lazy init + updater queue              | Many related fields → `useReducer` is clearer  |
| Batched with other `set`s in one event | Storing functions needs extra `() =>` wrapping |

**Examples:**

```tsx
const [user, setUser] = useState<User | null>(null);
setUser((u) => (u ? { ...u, name: "Ada" } : null));

// Interview trap — same reference, UI may not update
// items.push(x); setItems(items);
setItems((prev) => [...prev, x]);
```

**Thoughts:** _`set` queues the next snapshot; the current render’s variables never change._

**See also:** [State as snapshot](./4_react_core.md#state-as-snapshot) · [useReducer](#usereducer-1) · [Batching](./4_react_core.md#batching)

**Sources:**

[react.dev/reference/react/useState](https://react.dev/reference/react/useState)

useReducer

</details>

<details>
  <summary>useReducer</summary>

### useReducer

**Definition:** State hook driven by `(state, action) => newState`. Same fiber storage as `useState`, but **all transitions** go through one function. `dispatch` is **stable** — pass it down without `useCallback`.

**How it works:**

`dispatch(action)` queues an update; on the next render React runs `reducer(prev, action)` (pure). Lazy init: `useReducer(reducer, initArg, initFn)` — `initFn(initArg)` runs once. Prefer throwing on unknown actions so typos fail loudly.

_“`useReducer` when updates are a state machine. `useState` when it’s one value. `dispatch` is stable so children don’t need memoized callbacks just to call it.”_

**When to use:** Multi-field forms, wizards, “next state depends on several fields.” **Not** a single boolean/counter.

**Pluses and minuses:**

| +                                     | −                           |
| ------------------------------------- | --------------------------- |
| Centralized transitions, easy to test | Boilerplate for tiny state  |
| Stable `dispatch`                     | Overkill vs two `useState`s |
| Same idea as Redux, local to the tree | Still not a global store    |

**Examples:**

```tsx
type Action = { type: "increment" } | { type: "set"; value: number };

function reducer(state: { count: number }, action: Action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "set":
      return { count: action.value };
    default: {
      const _exhaustive: never = action;
      return _exhaustive;
    }
  }
}

const [state, dispatch] = useReducer(reducer, { count: 0 });
dispatch({ type: "increment" });
```

**See also:** [useReducer in Section 4](#usereducer) · [useState](#usestate)

**Sources:**

[react.dev/reference/react/useReducer](https://react.dev/reference/react/useReducer)

useActionState - 19

</details>

<details>
  <summary>useActionState - 19</summary>

### useActionState - 19

**Definition:** Wraps an action function; returns `[state, formAction, isPending]`. React 19 replacement for the old `useFormState` name.

**How it works:** Action signature `(previousState, payload) => nextState | Promise<nextState>`. For forms, payload is `FormData`. `isPending` is true while the Promise resolves. Pass `formAction` to `<form action={formAction}>`.

**When to use:** Forms and mutations where you need result state (error message, success text) + pending flag without manual `useState`.

**Examples:**

```tsx
const [message, formAction, isPending] = useActionState(
  async (_prev, formData: FormData) => {
    const err = await updateUser(formData);
    return err ?? "Saved";
  },
  null,
);

return (
  <form action={formAction}>
    <button disabled={isPending}>Save</button>
    {message && <p>{message}</p>}
  </form>
);
```

**See also:** [Actions](./4_react_server.md#actions) · [useFormStatus - 19](#useformstatus-19)

**Sources:**

[react.dev/reference/react/useActionState](https://react.dev/reference/react/useActionState)

useOptimistic - 19

</details>

<details>
  <summary>useOptimistic - 19</summary>

### useOptimistic - 19

**Definition:** Shows **optimistic UI** immediately while an async action is in flight; **reverts** to real state when the action completes or fails.

**How it works:** `useOptimistic(realState, updateFn)` returns `[optimisticState, addOptimistic]`. Call `addOptimistic` inside an action before `await` — UI updates instantly. When the action finishes and real state updates, optimistic layer discards.

**When to use:** Like counts, cart quantity, message "sent" status, reordering lists before server confirms.

**Examples:**

```tsx
const [optimisticCount, addOptimistic] = useOptimistic(
  count,
  (c, delta: number) => c + delta,
);

async function handleLike() {
  addOptimistic(1);
  await saveLike();
}
```

**See also:** [Actions](./4_react_server.md#actions) · [useActionState - 19](#useactionstate-19)

**Sources:**

[react.dev/reference/react/useOptimistic](https://react.dev/reference/react/useOptimistic)

useContext

</details>

### Context

<details>
  <summary>useContext</summary>

### useContext

**Definition:** Reads the nearest Provider value above in the tree. The component **re-renders whenever that value changes** (`Object.is`).

**How it works:**

[`createContext`](./4_react_apis.md#createcontext) defines the context object — default is used only if **no** Provider exists. `useContext(Ctx)` walks fibers upward. React 19: `<ThemeContext value={theme}>` (no `.Provider` required). Context is **not** a high-frequency store: every consumer re-renders on value change.

_“Context for rare, tree-wide values. If many components re-render on every keystroke, the value is too hot — split or use a store.”_

**When to use:**

Theme, locale, auth session, DI of a client. **Not** a shopping cart that updates every click — use Zustand/Redux or lift state.

#### Common pitfalls

`value={{ user, theme }}` creates a new object every parent render → all consumers update. Split contexts or memoize. Don’t use Context for server data (Query/RSC).

**Pluses and minuses:**

| +                          | −                                         |
| -------------------------- | ----------------------------------------- |
| Avoids prop drilling       | All consumers re-render on `value` change |
| Built-in, no extra library | Easy to put too much in one object        |

**Examples:**

```tsx
const ThemeContext = createContext<"light" | "dark">("light");

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return <button className={theme}>Click</button>;
}

<ThemeContext value={theme}>
  <App />
</ThemeContext>;
```

**See also:** [Props](./4_react_core.md#props) · [Lifting state up](./4_react_core.md#lifting-state-up)

**Sources:**

[react.dev/reference/react/useContext](https://react.dev/reference/react/useContext)

use() - 19

</details>

<details>
  <summary>use() - 19</summary>

### use() - 19

**Definition:** Reads a **Promise** or **Context** value during render. For Promises, **suspends** (throws to Suspense) until resolved.

**How it works:**

- `use(promise)` — if pending, component suspends → nearest `<Suspense>` shows fallback; on resolve, retries render with value
- `use(context)` — React 19 reads context without `useContext` in some cases (check docs)
- Cache the Promise **outside** render — don't `use(fetch())` inline every render

**When to use:** Server-passed promises, async RSC boundaries, reading context in loops (React 19).

**Examples:**

```tsx
function Message({ messagePromise }: { messagePromise: Promise<Message> }) {
  const message = use(messagePromise);
  return <p>{message.text}</p>;
}
```

**See also:** [Streaming & Suspense](./4_react_server.md#streaming--suspense) · [useContext](#usecontext)

**Sources:**

[react.dev/reference/react/use](https://react.dev/reference/react/use)

useEffect

</details>

### Effects

<details>
  <summary>useEffect</summary>

### useEffect

**Definition:** Declares a **synchronization** with an external system (network, DOM APIs, subscriptions, third-party widgets). React runs the setup after the component commits; it is **not** a class `componentDidMount` clone. Returns `undefined`. Effects run **on the client only** — never during SSR.

**How it works:**

1. After commit, React runs `setup`. `setup` may return a **cleanup**.
2. On the next commit, if **any** dependency changed (`Object.is`), React runs **cleanup with the old values**, then **setup with the new values**.
3. On unmount, React runs cleanup.
4. Dependencies must be an **inline array** of every **reactive** value used in setup (props, state, and locals from the component body). Omit the array → run after **every** commit. `[]` → only after the **first** commit (plus Strict Mode’s extra cycle in dev).
5. Cleanup is **not** “on unmount only.” It also runs before re-running the effect when deps change.
6. **Strict Mode (dev):** extra setup → cleanup → setup on mount. Production mounts once. If you see double fetch/subscribe, your cleanup must undo setup (disconnect, `abort()`, clear interval).
7. Default timing: after **paint** so the browser can show the UI first. Visual measure/position that would flicker → [`useLayoutEffect`](#uselayouteffect).

| You write       | When setup runs                                   |
| --------------- | ------------------------------------------------- |
| no 2nd argument | After every commit                                |
| `[]`            | After first commit (reconnect only if remounted)  |
| `[roomId, url]` | After first commit, then when those values change |

Missing deps → **stale closure** (effect sees old `count` / `user`). Extra object/function deps created in render → effect re-runs every time (`{} !== {}`). Fix: depend on primitives, or **create the object/function inside** the effect (preferred over wrapping everything in `useCallback`).

_“I use `useEffect` to connect to something React doesn’t own. The dependency array lists reactive values that should restart that connection. Cleanup disconnects so Strict Mode and prop changes don’t leak. If I’m only deriving UI data or handling a click, I don’t use an effect.”_

**When to use:**

Subscribe to WebSocket/`matchMedia`, start a timer, attach a non-React widget, sync `document.title`, log “page viewed.” Fetching **can** live here with an ignore flag / `AbortController`, but React docs prefer RSC, loaders, or TanStack Query.

Derived values (`fullName = first + last`), responding to a click (event handler), resetting state (`key={userId}`), notifying parent of a click (call the callback in the handler), chaining `setA` then `setB` from A.

#### Common pitfalls

- No cleanup for `setInterval` / listeners → Strict Mode double-ticks, leaks on unmount.
- Object/function in deps recreated each render → loop or reconnect storm.
- `useEffect(() => { setX(f(y)); }, [y])` when `f(y)` can be computed in render.
- Fetching in `useLayoutEffect` — blocks paint.
- Treating cleanup as “unmount only” — it also runs when `roomId` changes.

**Pluses and minuses:**

| +                                         | −                                                 |
| ----------------------------------------- | ------------------------------------------------- |
| Cleanup pairs with setup (no leaked subs) | Overused as a “lifecycle dump”                    |
| Deps make re-sync explicit                | Stale closures / infinite loops if deps are wrong |
| Strict Mode surfaces missing cleanup      | Client-only; fetch-in-effect needs race handling  |

**Examples:**

```tsx
useEffect(() => {
  const conn = createConnection(serverUrl, roomId);
  conn.connect();
  return () => conn.disconnect();
}, [serverUrl, roomId]);
```

**Example — fetch without races** (docs pattern: ignore flag):

```tsx
useEffect(() => {
  let ignore = false;
  setBio(null);
  fetchBio(person).then((result) => {
    if (!ignore) setBio(result);
  });
  return () => {
    ignore = true;
  };
}, [person]);
```

Do **not** make the effect callback `async` — a returned Promise is not cleanup. Use an inner `async` function or IIFE; abort/ignore in the returned cleanup.

**Thoughts:** _Effects sync React with the outside world. Deps = when to re-sync. Cleanup = undo the last sync._

**See also:** [You might not need an effect](#you-might-not-need-an-effect) · [useLayoutEffect](#uselayouteffect) · [useEffectEvent](#useeffectevent-react-192) · [Strict Mode](./4_react_core.md#strict-mode)

**Sources:**

[react.dev/reference/react/useEffect](https://react.dev/reference/react/useEffect) · [You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect)

useLayoutEffect

</details>

<details>
  <summary>useLayoutEffect</summary>

### useLayoutEffect

**Definition:** Same signature as `useEffect` but fires **synchronously after DOM mutations, before browser paint**.

**How it works:**

| Hook                 | When                             | Blocks paint? |
| -------------------- | -------------------------------- | ------------- |
| `useInsertionEffect` | Before DOM mutation              | Yes           |
| `useLayoutEffect`    | After DOM mutation, before paint | Yes           |
| `useEffect`          | After paint                      | No            |

**When to use:**

Measure DOM geometry, sync scroll position, focus without flash, position tooltips. Default to `useEffect` for fetch/subscriptions.

#### Common pitfalls

Data fetching in `useLayoutEffect` — blocks paint. Layout thrashing from repeated measure/write cycles.

**Examples:**

```tsx
useLayoutEffect(() => {
  const height = ref.current?.getBoundingClientRect().height;
  setTooltipTop(height);
}, [items]);
```

**Thoughts:** _Use when the user must not see a wrong frame._

**See also:** [Render pipeline](./4_react_core.md#render-pipeline) · [useEffect](#useeffect) · [useInsertionEffect](#useinsertioneffect)

**Sources:**

[react.dev/reference/react/useLayoutEffect](https://react.dev/reference/react/useLayoutEffect)

useInsertionEffect

</details>

<details>
  <summary>useInsertionEffect</summary>

### useInsertionEffect

**Definition:** Effect hook that runs **synchronously after render, before React applies DOM mutations** — earliest of the three effect hooks.

**How it works:** Order per commit: render → `useInsertionEffect` → DOM mutations → `useLayoutEffect` → paint → `useEffect`. React runs insertion effects so style injection libraries can insert `<style>` tags before the browser reads layout from updated DOM. Cleanup runs before the next insertion effect on the same component.

**When to use:** CSS-in-JS libraries (Emotion, styled-components, vanilla-extract runtime). **Not** for app-level side effects, data fetching, or DOM measurement — use `useLayoutEffect` or `useEffect`.

**Pluses and minuses:**

| +                                             | −                                                   |
| --------------------------------------------- | --------------------------------------------------- |
| Correct ordering for CSS-in-JS injection      | Blocks layout if misused in app code                |
| Dedicated API — no fighting `useLayoutEffect` | Cannot read layout from DOM (runs before mutations) |
| Cleanup pairs with style removal              | Almost never needed in application components       |

**Examples:**

```tsx
useInsertionEffect(() => {
  insertStyles(key, styles);
  return () => removeStyles(key);
}, [key, styles]);
```

**Thoughts:** Application developers should almost never use this hook. If you are not authoring a styling library, prefer `useLayoutEffect` (measure DOM) or `useEffect` (everything else).

**Sources:**

[react.dev/reference/react/useInsertionEffect](https://react.dev/reference/react/useInsertionEffect)

useEffectEvent - 19

</details>

<details>
  <summary>useEffectEvent - 19</summary>

### useEffectEvent - 19

**Definition:** `useEffectEvent(fn)` creates a stable function that always sees the latest props/state without being an effect dependency.

**How it works:** Put _reactive_ values (`roomId`) in the effect deps so the effect re-subscribes when they change. Put _event_ logic (`muted`, `theme`) inside `useEffectEvent` so those values stay fresh without reconnecting.

**When to use:** Chat reconnect on `roomId` only; analytics that read latest user without re-binding listeners.

#### Common pitfalls

Putting the connection identity (`roomId`) inside the Effect Event so the effect never re-runs. Calling `useEffectEvent` conditionally (Rules of Hooks still apply).

**Examples:**

```tsx
const onConnected = useEffectEvent(() => {
  showNotification("Connected!", theme);
});

useEffect(() => {
  const conn = createConnection(roomId);
  conn.on("connected", onConnected);
  return () => conn.disconnect();
}, [roomId]);
```

**Sources:**

[react.dev/reference/react/useEffectEvent](https://react.dev/reference/react/useEffectEvent)

useRef

</details>

### Ref

<details>
  <summary>useRef</summary>

### useRef

**Definition:** Mutable `{ current }` box that **survives re-renders**. Writing `.current` does **not** schedule a render.

**How it works:**

Same object identity every render (unlike `createRef()` in a function body). After commit, React sets `ref.current` to the DOM node when you pass `ref={el}` on a host element. Read/write `.current` in **effects and event handlers**, not during render (except one-time lazy init of `.current`).

_“`useRef` is an instance variable. `useState` is for anything the user should see. I don’t read refs during render.”_

**When to use:**

| Case            | Pattern                                                  |
| --------------- | -------------------------------------------------------- |
| DOM access      | `inputRef.current?.focus()` in a click handler or effect |
| Instance fields | Timer id, WebSocket, previous prop (`prev.current = x`)  |
| Avoid re-render | Value the UI does not display                            |

#### Common pitfalls

Using ref for displayed data. `createRef()` inside a function component (new object every render).

**Pluses and minuses:**

| +                                       | −                                       |
| --------------------------------------- | --------------------------------------- |
| No extra renders for timers/DOM         | Changing `.current` won’t update the UI |
| Stable object — can omit from some deps | Not a substitute for state              |

**Examples:**

```tsx
const inputRef = useRef<HTMLInputElement>(null);
useEffect(() => {
  inputRef.current?.focus();
}, []);

const prevCount = useRef<number>(undefined);
useEffect(() => {
  prevCount.current = count;
}, [count]);
```

**See also:** [ref as prop - 19](#ref-as-prop-19) · [useRef](#useref)

**Sources:**

[react.dev/reference/react/useRef](https://react.dev/reference/react/useRef)

useImperativeHandle

</details>

<details>
  <summary>useImperativeHandle</summary>

### useImperativeHandle

**Definition:** Customizes the **instance value** a parent receives when it attaches a `ref` to your component — instead of the DOM node, you expose a curated API object.

**How it works:** Pair with `ref` (React 19 prop or `forwardRef`). On commit, React calls your factory `() => ({ focus, scroll, ... })` and assigns the result to `ref.current`. The optional dependency array controls when the handle is recreated. Internal DOM refs stay private via `useRef` inside the child.

**When to use:** Focus an input inside a composite field component, expose `play()`/`pause()` on a video wrapper, integrate non-React widgets. **Prefer** declarative props (`isOpen`, `onSubmit`) and `autoFocus` first.

**Pluses and minuses:**

| +                                                   | −                                                   |
| --------------------------------------------------- | --------------------------------------------------- |
| Encapsulates imperative DOM ops behind a stable API | Breaks declarative React model                      |
| Useful for focus traps, media players, map widgets  | Harder to test and reason about than props          |
| Limits what parent can call (vs raw DOM ref)        | Overused when `autoFocus` or callback props suffice |

**Examples:**

```tsx
type InputHandle = { focus: () => void; clear: () => void };

function TextField({ ref, label }: { ref?: Ref<InputHandle>; label: string }) {
  const inputRef = useRef<HTMLInputElement>(null);
  useImperativeHandle(
    ref,
    () => ({
      focus: () => inputRef.current?.focus(),
      clear: () => {
        if (inputRef.current) inputRef.current.value = "";
      },
    }),
    [],
  );
  return (
    <>
      <label>{label}</label>
      <input ref={inputRef} />
    </>
  );
}
```

**Sources:**

[react.dev/reference/react/useImperativeHandle](https://react.dev/reference/react/useImperativeHandle)

ref as prop - 19

</details>

<details>
  <summary>ref as prop - 19</summary>

### ref as prop - 19

**Definition:** In React 19, function components accept `ref` as a **regular prop** — no `forwardRef` wrapper required.

**How it works:** Pass `ref` to a native element or to a child that forwards it. `forwardRef` still works for backward compatibility.

**When to use:** Composite inputs, focusable wrappers, libraries that need imperative handles.

**Examples:**

```tsx
function MyInput({
  ref,
  ...props
}: { ref?: Ref<HTMLInputElement> } & InputHTMLAttributes<HTMLInputElement>) {
  return <input ref={ref} {...props} />;
}
```

**See also:** [useRef](#useref) · [useImperativeHandle](#useimperativehandle)

**Sources:**

[react.dev/blog/2024/12/05/react-19](https://react.dev/blog/2024/12/05/react-19)

useMemo

</details>

### Performance

<details>
  <summary>useMemo</summary>

### useMemo

**Definition:** Caches a **computed value** between renders when dependencies are unchanged.

**How it works:** Deps compared with `Object.is`. Treat the cache as a **hint**, not a store — React may discard it (HMR, suspend). Profile before memoizing expensive calculations.

**When to use:** Proven-slow derived lists; stable object props for `memo` children.

**Examples:**

```tsx
const sorted = useMemo(() => items.slice().sort(compare), [items]);
```

**See also:** [useCallback](#usecallback) · [React Compiler](./4_react_server.md#react-compiler)

**Sources:**

[react.dev/reference/react/useMemo](https://react.dev/reference/react/useMemo)

useCallback

</details>

<details>
  <summary>useCallback</summary>

### useCallback

**Definition:** Caches a **function identity** between renders. `useCallback(fn, deps)` is `useMemo(() => fn, deps)` — React does **not** call `fn`.

**When to use:** Stable handlers for `memo` children; functions returned from custom hooks.

**Examples:**

```tsx
const onSave = useCallback(() => save(user), [user]);
```

**See also:** [useMemo](#usememo) · [useEffect](#useeffect)

**Sources:**

[react.dev/reference/react/useCallback](https://react.dev/reference/react/useCallback)

useTransition

</details>

<details>
  <summary>useTransition</summary>

### useTransition

**Definition:** Marks a state update as **non-urgent** so urgent updates (typing, clicks) stay responsive.

**How it works:** `const [isPending, startTransition] = useTransition()` — wrap expensive `setState` in `startTransition(() => ...)`.

**Examples:**

```tsx
const [isPending, startTransition] = useTransition();
startTransition(() => setResults(filter(query)));
```

**See also:** [useDeferredValue](#usedeferredvalue) · [Concurrent rendering](./4_react_core.md#concurrent-rendering)

**Sources:**

[react.dev/reference/react/useTransition](https://react.dev/reference/react/useTransition)

useDeferredValue

</details>

<details>
  <summary>useDeferredValue</summary>

### useDeferredValue

**Definition:** Returns a **lagging copy** of a value so heavy child renders can defer behind fast parent updates.

**How it works:** Pair with `memo` on the child. Requires `createRoot`.

**Examples:**

```tsx
const deferredQuery = useDeferredValue(query);
<SearchResults query={deferredQuery} />;
```

**See also:** [useTransition](#usetransition)

**Sources:**

[react.dev/reference/react/useDeferredValue](https://react.dev/reference/react/useDeferredValue)

useId

</details>

### Other

<details>
  <summary>useId</summary>

### useId

**Definition:** Generates a **stable, unique ID** per component instance that is consistent between server render and client hydration.

**How it works:** React assigns IDs using an internal counter prefixed with `:r` (e.g. `:r1:`). The same component instance gets the same ID on server and client, avoiding hydration mismatches. IDs are unique among siblings in the tree — not globally unique like UUIDs. Call `useId()` once per logical ID; derive related IDs with suffixes (`${id}-description`).

**When to use:** `htmlFor` / `id` on inputs, `aria-labelledby`, `aria-describedby`, linking error messages to fields.

**Pluses and minuses:**

| +                                       | −                                                   |
| --------------------------------------- | --------------------------------------------------- |
| SSR/hydration-safe — no `Math.random()` | Not for list `key` props                            |
| Pairs labels, inputs, ARIA attributes   | IDs contain `:` — escape in CSS selectors if needed |
| One hook call per form field group      |                                                     |

**Examples:**

```tsx
function Field({ label }: { label: string }) {
  const id = useId();
  const errorId = `${id}-error`;
  return (
    <>
      <label htmlFor={id}>{label}</label>
      <input id={id} aria-describedby={errorId} />
      <span id={errorId} role="alert">
        Required
      </span>
    </>
  );
}
```

**Thoughts:** Do **not** use `useId` for `key` in lists — keys must be stable across the list's lifetime, not per render slot. Do **not** slice or hash the ID.

**Sources:**

[react.dev/reference/react/useId](https://react.dev/reference/react/useId)

useSyncExternalStore

</details>

<details>
  <summary>useSyncExternalStore</summary>

### useSyncExternalStore

**Definition:** Subscribes to an **external data source** outside React state and reads a snapshot in a way that is safe under **concurrent rendering** (no tearing).

**How it works:** You provide three functions: `subscribe(onStoreChange)` registers a callback React calls when the store changes; `getSnapshot()` returns the current value (must be immutable or cached — same reference if data unchanged); `getServerSnapshot()` returns the value during SSR (required if the store differs on server vs client, e.g. `window`). React re-renders subscribed components when the snapshot changes. Library authors (Redux, Zustand) use this internally; app code uses the library's hook.

**When to use:** Global client stores (Redux, Zustand), `window.matchMedia`, `navigator.onLine`, third-party non-React state. Required when a library integrates with React 18+ concurrent features.

**Pluses and minuses:**

| +                                              | −                                                              |
| ---------------------------------------------- | -------------------------------------------------------------- |
| Prevents tearing during concurrent updates     | Verbose API — prefer Zustand/Redux hooks in apps               |
| Official pattern for browser API subscriptions | `getSnapshot` must be referentially stable when data unchanged |
| SSR-safe with `getServerSnapshot`              | Overkill for simple `useState`                                 |

**Examples:**

```tsx
function useWindowWidth() {
  return useSyncExternalStore(
    (onChange) => {
      window.addEventListener("resize", onChange);
      return () => window.removeEventListener("resize", onChange);
    },
    () => window.innerWidth,
    () => 0, // SSR fallback
  );
}
```

**Thoughts:** `getSnapshot` must not return a new object/array reference every call unless data actually changed — React will re-render infinitely. Prefer existing library hooks over rolling your own store integration.

**Sources:**

[react.dev/reference/react/useSyncExternalStore](https://react.dev/reference/react/useSyncExternalStore)

useDebugValue

</details>

<details>
  <summary>useDebugValue</summary>

### useDebugValue

**Definition:** Adds a **custom label** for a custom hook in React DevTools — visible when inspecting the hook in the component tree.

**How it works:** Call `useDebugValue(value)` or `useDebugValue(value, formatFn)` inside a custom hook. React DevTools shows the formatted value next to the hook name. The hook body still runs in production, but DevTools display is stripped in optimized production builds — keep the value computation cheap. Optional `formatFn` defers expensive formatting until DevTools actually inspects the hook.

**When to use:** Custom hooks returning opaque objects — show `"Online"` / `"Offline"`, cart item count, auth role. Skip for trivial one-liner hooks.

**Pluses and minuses:**

| +                                                       | −                                              |
| ------------------------------------------------------- | ---------------------------------------------- |
| Faster debugging of shared hooks (`useAuth`, `useCart`) | Dev-only benefit — no runtime user value       |
| Optional lazy formatter avoids prod cost                | Only works inside custom hooks, not components |
| Documents hook state at a glance                        |                                                |

**Examples:**

```tsx
function useOnlineStatus() {
  const online = useSyncExternalStore(
    subscribe,
    () => navigator.onLine,
    () => true,
  );
  useDebugValue(online ? "Online" : "Offline");
  return online;
}

function useFriendStatus(friendId: string) {
  const status = useFriendStatusImpl(friendId);
  useDebugValue(status, (s) => (s === "online" ? "🟢 Online" : "⚫ Offline"));
  return status;
}
```

**Sources:**

[react.dev/reference/react/useDebugValue](https://react.dev/reference/react/useDebugValue)

useFormStatus - 19

</details>

<details>
  <summary>useFormStatus - 19</summary>

### useFormStatus - 19

**Definition:** `react-dom` hook — read **pending** state of the nearest parent `<form>` from a **child** component.

**How it works:** Must be called inside a descendant of `<form>`. Returns `{ pending, data, method, action }`. `pending` is true while the form's action Promise is in flight.

**When to use:** Reusable `SubmitButton` without prop drilling `isPending`.

**Examples:**

```tsx
function SubmitButton({ children }: { children: React.ReactNode }) {
  const { pending } = useFormStatus();
  return (
    <button type="submit" disabled={pending}>
      {children}
    </button>
  );
}
```

**See also:** [useActionState - 19](#useactionstate-19) · [Actions](./4_react_server.md#actions)

**Sources:**

[react.dev/reference/react-dom/hooks/useFormStatus](https://react.dev/reference/react-dom/hooks/useFormStatus)

Custom hooks

</details>

### Reuse & guidance

<details>
  <summary>Custom hooks</summary>

### Custom hooks

**Definition:** A function whose name starts with `use` and that **calls other hooks**. It is not a React feature beyond the naming convention — ESLint uses the `use` prefix to enforce [Rules of Hooks](#rules-of-hooks).

**How it works:**

Each **call site** gets its own isolated state (two `useOnlineStatus()` calls → two subscriptions). Hooks share **logic**, never a singleton store, unless you put a store behind `useSyncExternalStore` / context on purpose. Return memoized callbacks from hooks that expose functions so consumers can depend on them.

The same `useState` + `useEffect` combo appears in 2+ components. **Not** for sharing JSX (compose components) or for helpers that never call hooks.

_“Custom hooks replace most HOCs for reusable behavior. They don’t share state between components unless I put shared state in context or an external store.”_

**Pluses and minuses:**

| +                                    | −                         |
| ------------------------------------ | ------------------------- |
| Reuse logic without HOC wrapper hell | `use` prefix is required  |
| Test the hook in isolation           | Over-extraction adds hops |

**Thoughts:**

_Extract behavior, not markup. Each call has its own state._

**Example —** `useClickOutside`**:**

```tsx
function useClickOutside<T extends HTMLElement>(
  ref: RefObject<T | null>,
  onOutside: () => void,
) {
  useEffect(() => {
    function handle(e: MouseEvent) {
      if (ref.current && !ref.current.contains(e.target as Node)) {
        onOutside();
      }
    }
    document.addEventListener("mousedown", handle);
    return () => document.removeEventListener("mousedown", handle);
  }, [ref, onOutside]);
}
```

**Sources:**

[react.dev/learn/reusing-logic-with-custom-hooks](https://react.dev/learn/reusing-logic-with-custom-hooks)

You might not need an effect

</details>

<details>
  <summary>You might not need an effect</summary>

### You might not need an effect

**Definition:** Many patterns developers reach for `useEffect` are better handled in render, event handlers, or framework data loaders. Effects are for **syncing with external systems** — not for transforming data or responding to user events.

**How it works:**

Ask: "Does this need to run _because the component was displayed_ and touch something outside React?" If no → probably not an effect.

| Need                           | Do instead of effect                       |
| ------------------------------ | ------------------------------------------ |
| Transform data for display     | Compute during render                      |
| User event response            | Event handler                              |
| Cache expensive calc           | `useMemo`                                  |
| Reset all state on prop change | `key={id}` on component                    |
| Notify parent of state change  | Call parent callback in event handler      |
| Fetch data                     | Framework loader, RSC, TanStack Query      |
| Chain state updates            | One event handler with multiple `setState` |

Subscriptions, timers, third-party widgets, logging to analytics, syncing `document.title` (or use framework metadata).

**Examples:**

```tsx
// Bad — effect for derived data
useEffect(() => setFullName(first + " " + last), [first, last]);
// Good — compute during render
const fullName = `${first} ${last}`;
```

**Thoughts:** _Effects synchronize with the outside world — not with your own state._

**See also:** [useEffect](#useeffect) · [Purity](./4_react_core.md#purity)

**Sources:**

[react.dev/learn/you-might-not-need-an-effect](https://react.dev/learn/you-might-not-need-an-effect)

---

</details>

## 6. Escape hatches

### Escape hatches

Portals

<details>
  <summary>Portals</summary>

### Portals

**Definition:** `createPortal(child, domNode)` renders React children into a **different DOM node** while keeping them in the same React component tree for context and event bubbling.

**How it works:** React still manages the portal's children in the virtual tree — context, state, and event handlers work normally. DOM nodes mount under `domNode` (often `document.body`). Events bubble through the **React tree** (parent handlers still fire), not only the DOM tree. Use for UI that must escape CSS `overflow: hidden`, `z-index` stacking contexts, or `position` constraints of ancestors.

**When to use:** Modal dialogs, toast notifications, floating menus, full-screen overlays. Pair with focus trap and `aria-modal` for accessibility.

**Pluses and minuses:**

| +                                                    | −                                                             |
| ---------------------------------------------------- | ------------------------------------------------------------- |
| Modals, tooltips, dropdowns render above page chrome | Need a dedicated mount point in HTML (`#modal-root`)          |
| Preserves React context from parent                  | SSR: ensure portal target exists or render null until mounted |
| Event bubbling follows React hierarchy               | Focus management and a11y require extra work (focus trap)     |

**Examples:**

```tsx
function Modal({
  children,
  onClose,
}: {
  children: ReactNode;
  onClose: () => void;
}) {
  return createPortal(
    <div role="dialog" aria-modal="true" className="overlay" onClick={onClose}>
      <div className="modal" onClick={(e) => e.stopPropagation()}>
        {children}
      </div>
    </div>,
    document.getElementById("modal-root")!,
  );
}
```

**Thoughts:** Portal children still participate in reconciliation — unmounting the parent unmounts the portal. For SSR, defer portal render until `useEffect` or use a framework that handles it.

**Sources:**

[react.dev/reference/react-dom/createPortal](https://react.dev/reference/react-dom/createPortal)

Error boundaries

</details>

<details>
  <summary>Error boundaries</summary>

### Error boundaries

**Definition:**

Class components implementing `static getDerivedStateFromError` + `componentDidCatch` that catch render errors in children and show fallback UI.

| Catches            | Does NOT catch                 |
| ------------------ | ------------------------------ |
| Render errors      | Event handler errors           |
| Lifecycle errors   | Async (`setTimeout`, promises) |
| Constructor errors | Server-side rendering errors   |
|                    | Errors in the boundary itself  |

**How it works:**

1. Child throws during render → React walks up to nearest error boundary
2. `getDerivedStateFromError` returns state → boundary re-renders with fallback UI
3. `componentDidCatch(error, info)` runs for logging (side effects OK here)
4. Siblings and parent **outside** the boundary keep working

**When to use:**

Wrap route segments, risky third-party widgets, lazy-loaded chunks. Pair with logging in `componentDidCatch`.

Event handler errors (use `try/catch`), async errors (handle in promise), replacing form validation UI.

**Examples:**

```tsx
class ErrorBoundary extends React.Component<
  { fallback: ReactNode; children: ReactNode },
  { hasError: boolean }
> {
  state = { hasError: false };
  static getDerivedStateFromError() {
    return { hasError: true };
  }
  render() {
    return this.state.hasError ? this.props.fallback : this.props.children;
  }
}
```

**Thoughts:**

No hook equivalent yet — use `react-error-boundary` library or class. React 19 doesn't change this.

_Error boundaries catch render-time failures in children — not event or async errors._

**Sources:**

[react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary)

React.lazy + Suspense

</details>

<details>
  <summary>React.lazy + Suspense</summary>

### React.lazy + Suspense

**Definition:** `lazy(() => import('./Page'))` **code-splits** a component — loaded only when first rendered. `<Suspense fallback={...}>` shows fallback UI while the lazy chunk loads.

**How it works:**

1. `const Chart = lazy(() => import('./Chart'))` does **not** fetch yet. It registers a **load** function. React does **not** call `load` until the first time it **tries to render** that component.
2. `{show && <Chart />}` — while `show` is false, `<Chart />` is not in the tree → **no network**.
3. When `show` becomes true, React renders `<Chart />`. The loader runs `import('./Chart')` — the bundler emits a **separate JS chunk**; the browser fetches that file.
4. Until the Promise resolves, React **throws** that Promise (same channel as Suspense). The nearest `<Suspense fallback={...}>` catches it and shows `fallback`.
5. When the Promise resolves, React reads `.default` as the component, **caches** both the Promise and the module — `load` is not called again — then **retries** the render with the real component.
6. If the Promise **rejects**, React throws the error to the nearest **error boundary** (not to Suspense).

**When to use:**

Settings page, admin panel, heavy chart library, locale-specific bundles. Frameworks (Next.js) split per route automatically.

#### Common pitfalls

Assuming `lazy()` fetches at module eval time — it waits for **first render**. Missing Suspense above the lazy element. Named exports need `.then(m => ({ default: m.Foo }))`.

**Pluses and minuses:**

| +                                          | −                                          |
| ------------------------------------------ | ------------------------------------------ |
| Smaller initial bundle, faster TTI         | Loading flash if fallback not designed     |
| Route-based splitting is automatic pattern | Nested lazies without planning → waterfall |
| Integrates with Suspense for data + code   | Needs error boundary for network failures  |

**Examples:**

```tsx
const Settings = lazy(() => import("./Settings"));

function App({ openSettings }: { openSettings: boolean }) {
  return (
    <ErrorBoundary fallback={<p>Failed to load</p>}>
      <Suspense fallback={<Spinner />}>
        {openSettings ? <Settings /> : <Home />}
      </Suspense>
    </ErrorBoundary>
  );
}
```

**Thoughts:**

_The chunk loads on first render of the lazy component; Suspense shows fallback while_ `import()` _is pending; React caches the module after that._

`lazy` only supports default exports. For named exports: `lazy(() => import('./Foo').then(m => ({ default: m.Foo })))`.

**Sources:**

[react.dev/reference/react/lazy](https://react.dev/reference/react/lazy) · [Suspense](https://react.dev/reference/react/Suspense)

---

</details>
