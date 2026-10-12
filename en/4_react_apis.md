# React — APIs

`react` package APIs plus **react-dom** Client, Server, and Static entry points (mount, SSR, SSG).

[← Back to hub](./4_react.md)

### Related docs

- [React — Core](./4_react_core.md) · [React — Hooks](./4_react_hooks.md) · [React — Components](./4_react_components.md) · [React — APIs](./4_react_apis.md) · [React — React 19](./4_react_server.md) · [React — Ecosystem](./4_react_ecosystem.md) · [React — Interview](./4_react_interview.md)

### Sections

- [6. APIs](#6-apis)

---

## 6. APIs

### Overview

APIs overview

<details>
  <summary>APIs overview</summary>

### APIs overview

**Definition:** React reference APIs span the **`react`** package (shared logic) and **`react-dom`** entry points for the browser DOM — **client** (mount/hydrate), **server** (HTML streams), and **static** (SSG).

**How it works:**

| Area            | Package / path                  | Topics on this page                                                         |
| --------------- | ------------------------------- | --------------------------------------------------------------------------- |
| **React APIs**  | `react`                         | Context, lazy, memo, transitions, `use`, tests — [group below](#react-apis) |
| **Client APIs** | `react-dom/client`, `react-dom` | `createRoot`, `hydrateRoot`, portals — [Client APIs](#client-apis)          |
| **Server APIs** | `react-dom/server`, `react`     | Streaming SSR, legacy string render, `cache` — [Server APIs](#server-apis)  |
| **Static APIs** | `react-dom/static`              | Prerender to static HTML — [Static APIs](#static-apis)                      |

Hooks: [Hooks](./4_react_hooks.md). JSX components: [Components](./4_react_components.md). RSC architecture: [React 19](./4_react_server.md).

**Sources:**

[react.dev/reference/react/apis](https://react.dev/reference/react/apis) · [react.dev/reference/react-dom](https://react.dev/reference/react-dom)

React APIs

</details>

### React APIs

<details>
  <summary>createContext</summary>

### createContext

**Definition:** `createContext(defaultValue)` returns a **context object** so descendants read shared data with `useContext` without prop drilling.

**How it works:**

1. Call once per module: `const ThemeContext = createContext<Theme>("light")`
2. Provide value: React 19+ `<ThemeContext value={theme}>` (`.Provider` still works)
3. Consumers call `useContext(ThemeContext)` — see [`useContext`](./4_react_hooks.md#usecontext)
4. `defaultValue` applies only when **no** provider exists above the reader

**When to use:** Theme, locale, auth session — stable or memoized values, not high-frequency global store.

**Examples:**

```tsx
export const ThemeContext = createContext<"light" | "dark">("light");

function App() {
  const [theme, setTheme] = useState<"light" | "dark">("light");
  return (
    <ThemeContext value={theme}>
      <Toolbar />
    </ThemeContext>
  );
}
```

**See also:** [`useContext`](./4_react_hooks.md#usecontext) · [Lifting state up](./4_react_core.md#lifting-state-up)

**Sources:**

[react.dev/reference/react/createContext](https://react.dev/reference/react/createContext)

lazy

</details>

<details>
  <summary>lazy</summary>

### lazy

**Definition:** `lazy(load)` returns a component that loads its module **on first render** via dynamic `import()` — pairs with [`<Suspense>`](./4_react_components.md#suspense).

**How it works:**

1. `const Page = lazy(() => import("./Page"))` registers a loader; no fetch until first render of `<Page />`
2. While the Promise is pending, React suspends → nearest Suspense shows `fallback`
3. Resolved `.default` is cached; rejected Promise goes to an error boundary

**When to use:** Routes, heavy widgets, admin panels. Prefer framework route splitting (Next.js) when available.

**Examples:**

```tsx
const Chart = lazy(() => import("./Chart"));

<Suspense fallback={<Spinner />}>
  <Chart />
</Suspense>;
```

**See also:** [React.lazy + Suspense](./4_react_hooks.md#reactlazy--suspense) · [Suspense](./4_react_components.md#suspense)

**Sources:**

[react.dev/reference/react/lazy](https://react.dev/reference/react/lazy)

memo

</details>

<details>
  <summary>memo</summary>

### memo

**Definition:** `memo(Component, arePropsEqual?)` returns a component that **skips re-render** when props are unchanged (shallow `Object.is` per prop by default).

**How it works:** Parent re-renders do not force child re-render if `arePropsEqual(prev, next)` returns true (or default shallow compare passes). Does not memoize **context** or **state** inside the child. React Compiler may reduce need for manual `memo`.

**When to use:** Expensive pure children that receive stable props — profile first. Pair with stable callbacks (`useCallback`) or Compiler.

**Pluses and minuses:**

| +                           | −                                                |
| --------------------------- | ------------------------------------------------ |
| Cuts redundant subtree work | New function/object props defeat memo every time |
| Simple API                  | Wrong `arePropsEqual` hides bugs                 |

**Examples:**

```tsx
const Row = memo(function Row({ item }: { item: Item }) {
  return <li>{item.name}</li>;
});
```

**See also:** [`useMemo`](./4_react_hooks.md#usememo) · [`useCallback`](./4_react_hooks.md#usecallback) · [React Compiler](./4_react_server.md#react-compiler)

**Sources:**

[react.dev/reference/react/memo](https://react.dev/reference/react/memo)

startTransition

</details>

<details>
  <summary>startTransition</summary>

### startTransition

**Definition:** `startTransition(scope)` marks state updates inside `scope` as **transitions** — interruptible, lower priority than urgent input updates.

**How it works:** Same scheduling as [`useTransition`](./4_react_hooks.md#usetransition) but without `isPending` — use the hook when you need a pending flag. Can wrap async Actions (React 19). Combine with [`addTransitionType - 19`](#addtransitiontype---19) for View Transition styling.

**When to use:** Tab switches, large list filters, chart redraws while typing stays responsive.

**Examples:**

```tsx
import { startTransition } from "react";

function selectTab(id: string) {
  startTransition(() => {
    setTab(id);
  });
}
```

**See also:** [`useTransition`](./4_react_hooks.md#usetransition) · [Concurrent rendering](./4_react_core.md#concurrent-rendering)

**Sources:**

[react.dev/reference/react/startTransition](https://react.dev/reference/react/startTransition)

use - 19

</details>

<details>
  <summary>use - 19</summary>

### use - 19

**Definition:** `use(resource)` reads a **Promise** or **Context** during render. For Promises, suspends until resolved — same channel as Suspense.

**How it works:**

- `use(context)` — like `useContext` but can be called conditionally (rules differ from hooks)
- `use(promise)` — suspends; resolved value is returned on retry
- Full patterns, pitfalls, and examples: [Hooks — `use() - 19`](./4_react_hooks.md#use---19)

**When to use:** Promise from Server Component or cache; context in branches where hooks cannot run.

**Sources:**

[react.dev/reference/react/use](https://react.dev/reference/react/use)

addTransitionType - 19

</details>

<details>
  <summary>addTransitionType - 19</summary>

### addTransitionType - 19

**Definition:** `addTransitionType(type)` tags the **current** transition with a string cause — used with [`startTransition`](#starttransition) and [`<ViewTransition>`](./4_react_components.md#viewtransition---19) for CSS / class / event-based animations.

**How it works:** Call only inside `startTransition(() => { addTransitionType('nav-forward'); ... })`. Multiple types can accumulate. Types reset after commit; Suspense reveal timing affects which types apply.

**When to use:** Custom enter/exit animations keyed on navigation direction, submit vs cancel, etc.

**Examples:**

```tsx
startTransition(() => {
  addTransitionType("submit-click");
  submitAction();
});
```

**See also:** [ViewTransition - 19](./4_react_components.md#viewtransition---19)

**Sources:**

[react.dev/reference/react/addTransitionType](https://react.dev/reference/react/addTransitionType)

captureOwnerStack

</details>

<details>
  <summary>captureOwnerStack</summary>

### captureOwnerStack

**Definition:** `captureOwnerStack()` reads the current **Owner Stack** in development and returns it as a string, or `null` if unavailable.

**How it works:** Owner stack shows which components created a given node — complementary to JS stack traces. Dev-only diagnostic; not for production logging.

**When to use:** Custom error messages, dev tooling, debugging component ownership (React 19.2+ improvements).

**Examples:**

```tsx
if (process.env.NODE_ENV === "development") {
  const owner = captureOwnerStack();
  console.log(owner);
}
```

**Sources:**

[react.dev/reference/react/captureOwnerStack](https://react.dev/reference/react/captureOwnerStack)

act

</details>

<details>
  <summary>act</summary>

### act

**Definition:** `await act(asyncActFn)` is a **test helper** that flushes all React updates triggered inside `asyncActFn` before assertions run.

**How it works:** Wrap `render`, `fireEvent`, and state updates in `act` (or use Testing Library, which wraps `act` for you). Async `act` also awaits microtasks and updates across `await` boundaries.

**When to use:** Unit/integration tests with React DOM or React Native test renderer — not application runtime code.

**Examples:**

```tsx
import { act } from "react";
import { createRoot } from "react-dom/client";

await act(async () => {
  root.render(<Counter />);
});
expect(container.textContent).toBe("0");
```

**Sources:**

[react.dev/reference/react/act](https://react.dev/reference/react/act)

Client APIs

</details>

### Client APIs

Client APIs catalog

<details>
  <summary>Client APIs catalog</summary>

### Client APIs catalog

**Definition:** **`react-dom/client`** mounts or hydrates React in the browser; **`react-dom`** also exposes `createPortal` and `flushSync` for DOM-specific behavior.

**How it works:**

| API                           | Import             | Role                                             |
| ----------------------------- | ------------------ | ------------------------------------------------ |
| [`createRoot`](#createroot)   | `react-dom/client` | Client-only SPA mount (React 18+)                |
| [`hydrateRoot`](#hydrateroot) | `react-dom/client` | Attach to server-rendered HTML                   |
| `createPortal`                | `react-dom`        | Render children into another DOM node            |
| `flushSync`                   | `react-dom`        | Force synchronous render + commit (escape hatch) |

Prefer frameworks (Next.js) over hand-rolled roots in production apps.

**Sources:**

[react.dev/reference/react-dom/client](https://react.dev/reference/react-dom/client)

createRoot

</details>

<details>
  <summary>createRoot</summary>

### createRoot

**Definition:** `createRoot(domNode, options?)` creates a **root** and returns `{ render, unmount }` to display React in a browser DOM node.

**How it works:** Replaces legacy `ReactDOM.render`. Enables concurrent features and automatic batching. Call `root.render(<App />)` for initial and updated trees. Optional `onUncaughtError` / `onCaughtError` for logging.

**When to use:** Every client-only React web app entry (`main.tsx`). Match `identifierPrefix` with server `renderToReadableStream` if using `useId` across SSR + client.

**Examples:**

```tsx
import { createRoot } from "react-dom/client";

const root = createRoot(document.getElementById("root")!);
root.render(<App />);
```

**See also:** [React vs ReactDOM](./4_react_core.md#react-vs-reactdom) · [hydrateRoot](#hydrateroot)

**Sources:**

[react.dev/reference/react-dom/client/createRoot](https://react.dev/reference/react-dom/client/createRoot)

hydrateRoot

</details>

<details>
  <summary>hydrateRoot</summary>

### hydrateRoot

**Definition:** `hydrateRoot(domNode, reactNode, options?)` attaches React to **existing server HTML** inside `domNode` and makes it interactive.

**How it works:** First client render must match server markup (see [Hydration](./4_react_server.md#hydration)). Returns `{ render, unmount }` like `createRoot`. Pass `formState` when resuming after a form Action with `useActionState` + permalink.

**When to use:** SSR/RSC apps after HTML is streamed or prerendered — not for pure CSR.

**Examples:**

```tsx
import { hydrateRoot } from "react-dom/client";

hydrateRoot(document.getElementById("root")!, <App />);
```

**See also:** [Streaming & Suspense](./4_react_server.md#streaming--suspense) · [createRoot](#createroot)

**Sources:**

[react.dev/reference/react-dom/client/hydrateRoot](https://react.dev/reference/react-dom/client/hydrateRoot)

Server APIs

</details>

### Server APIs

Server APIs catalog

<details>
  <summary>Server APIs catalog</summary>

### Server APIs catalog

**Definition:** **`react-dom/server`** renders React trees to HTML on the server; **`cache`** / **`cacheSignal`** from **`react`** dedupe and abort server fetches in RSC.

**How it works:**

| API                                                 | Import             | Role                                                         |
| --------------------------------------------------- | ------------------ | ------------------------------------------------------------ |
| [`renderToReadableStream`](#rendertoreadablestream) | `react-dom/server` | Streaming SSR (Web Streams — edge, modern runtimes)          |
| [`renderToPipeableStream`](#rendertopipeablestream) | `react-dom/server` | Streaming SSR (Node.js pipe)                                 |
| [`renderToString`](#rendertostring)                 | `react-dom/server` | Legacy one-shot HTML string (limited Suspense)               |
| `renderToStaticMarkup`                              | `react-dom/server` | HTML without React data attributes (emails, static snippets) |
| [`cache`](#cache)                                   | `react`            | Per-request memo in Server Components                        |
| [`cacheSignal`](#cachesignal)                       | `react`            | AbortSignal when server render completes                     |

Prefer streaming APIs over `renderToString` for Suspense and progressive HTML.

**Sources:**

[react.dev/reference/react-dom/server](https://react.dev/reference/react-dom/server)

cache

</details>

<details>
  <summary>cache</summary>

### cache

**Definition:** `cache(fn)` returns a **per-request memoized** version of `fn` in **Server Components** — deduplicates calls with the same arguments during one render pass.

**How it works:**

- Cache is **invalidated each server request**
- Each `cache(fn)` call creates a **separate** memoized function (caches do not merge)
- Errors are cached too — same args rethrow the same error
- Client Components cannot use `cache`

**When to use:** Expensive server fetches or metrics shared by multiple components in one tree.

**Examples:**

```tsx
import { cache } from "react";

const getMetrics = cache(async (data: Data) => compute(data));
```

**See also:** [`cacheSignal`](#cachesignal) · [RSC](./4_react_server.md#spa-vs-ssr-vs-rsc)

**Sources:**

[react.dev/reference/react/cache](https://react.dev/reference/react/cache)

cacheSignal

</details>

<details>
  <summary>cacheSignal</summary>

### cacheSignal

**Definition:** `cacheSignal()` returns an `AbortSignal` aborted when React **finishes rendering** the current server pass.

**How it works:** Server Components only — `null` on client. Pair with [`cache`](#cache) + `fetch`.

**Sources:**

[react.dev/reference/react/cacheSignal](https://react.dev/reference/react/cacheSignal)

renderToReadableStream

</details>

<details>
  <summary>renderToReadableStream</summary>

### renderToReadableStream

**Definition:** `renderToReadableStream(reactNode, options?)` renders a React tree to a **Web ReadableStream** of HTML — primary streaming API for edge and modern runtimes.

**How it works:** Emits shell first, then streams Suspense boundaries as they resolve. Options: `bootstrapScripts`, `onShellReady`-style callbacks via stream consumption, `formState` for Actions, `identifierPrefix` for `useId`, `onError`, `signal` to abort.

**When to use:** Custom SSR handlers on Deno, Workers, or any Web Streams environment — or when not using a meta-framework.

**Examples:**

```tsx
import { renderToReadableStream } from "react-dom/server";

const stream = await renderToReadableStream(<App />, {
  bootstrapScripts: ["/main.js"],
});
return new Response(stream, { headers: { "content-type": "text/html" } });
```

**See also:** [hydrateRoot](#hydrateroot) · [Streaming & Suspense](./4_react_server.md#streaming--suspense)

**Sources:**

[react.dev/reference/react-dom/server/renderToReadableStream](https://react.dev/reference/react-dom/server/renderToReadableStream)

renderToPipeableStream

</details>

<details>
  <summary>renderToPipeableStream</summary>

### renderToPipeableStream

**Definition:** `renderToPipeableStream(reactNode, options?)` renders to a **Node.js pipeable stream** — `{ pipe, abort }` for HTTP responses.

**How it works:** `onShellReady` → start piping progressive HTML; `onAllReady` for crawlers/static snapshot; `onShellError` for fallback document. Node-specific — use `renderToReadableStream` on Web Streams platforms.

**When to use:** Custom Node.js HTTP server SSR without Next.js.

**Examples:**

```tsx
import { renderToPipeableStream } from "react-dom/server";

const { pipe } = renderToPipeableStream(<App />, {
  bootstrapScripts: ["/main.js"],
  onShellReady() {
    response.setHeader("content-type", "text/html");
    pipe(response);
  },
});
```

**Sources:**

[react.dev/reference/react-dom/server/renderToPipeableStream](https://react.dev/reference/react-dom/server/renderToPipeableStream)

renderToString

</details>

<details>
  <summary>renderToString</summary>

### renderToString

**Definition:** `renderToString(reactNode)` renders the tree to a **single HTML string** synchronously — legacy non-streaming SSR.

**How it works:** If a component suspends, React emits the **fallback** immediately — no waiting for async data. Fine for simple pages; poor fit for streaming Suspense. Works in Node; avoid calling from browser bundles.

**When to use:** Email templates, tests, or legacy integrations — prefer streaming APIs for interactive apps.

**Examples:**

```tsx
import { renderToString } from "react-dom/server";

const html = renderToString(<App />);
```

**See also:** [renderToReadableStream](#rendertoreadablestream)

**Sources:**

[react.dev/reference/react-dom/server/renderToString](https://react.dev/reference/react-dom/server/renderToString)

Static APIs

</details>

### Static APIs

Static APIs catalog

<details>
  <summary>Static APIs catalog</summary>

### Static APIs catalog

**Definition:** **`react-dom/static`** renders React to **static HTML** (SSG) — waits for async data, then outputs HTML you can cache or serve without per-request rendering.

**How it works:**

| API                              | Environment | Role                                                    |
| -------------------------------- | ----------- | ------------------------------------------------------- |
| [`prerender`](#prerender)        | Web Streams | `await prerender(<App />)` → `{ prelude, postponed }`   |
| `prerenderToNodeStream`          | Node.js     | Node stream variant (prefer over Web API on Node)       |
| `resumeAndPrerender`             | Web Streams | Continue from `postponed` state after partial prerender |
| `resumeAndPrerenderToNodeStream` | Node.js     | Node variant of resume                                  |

Hydrate on the client with [`hydrateRoot`](#hydrateroot) when the page should become interactive.

**Sources:**

[react.dev/reference/react-dom/static](https://react.dev/reference/react-dom/static)

prerender

</details>

<details>
  <summary>prerender</summary>

### prerender

**Definition:** `prerender(reactNode, options?)` renders a React tree to **static HTML** using a Web Stream; resolves when all data is ready.

**How it works:** Returns `{ prelude, postponed }`. `prelude` is HTML (stream or string depending on consumption). `postponed` can be stored and resumed with `resumeAndPrerender` if you abort early. Supports `bootstrapScripts` like server streaming.

**When to use:** Static site generation, marketing pages, or prebuilding HTML at deploy time — not for personalized per-request SSR.

**Examples:**

```tsx
import { prerender } from "react-dom/static";

const { prelude } = await prerender(<App />, {
  bootstrapScripts: ["/main.js"],
});
return new Response(prelude, { headers: { "content-type": "text/html" } });
```

**See also:** [renderToReadableStream](#rendertoreadablestream) · [hydrateRoot](#hydrateroot)

**Sources:**

[react.dev/reference/react-dom/static/prerender](https://react.dev/reference/react-dom/static/prerender)

---

</details>
