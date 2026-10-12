# React — APIs

API пакета `react` и точки входа **react-dom** Client, Server и Static (mount, SSR, SSG).

[ ← Вернуться в центр](./4_react.md)

### Related docs

- [React — Core](./4_react_core.md) · [React — Hooks](./4_react_hooks.md) · [React — Components](./4_react_components.md) · [React — APIs](./4_react_apis.md) · [React — React 19](./4_react_server.md) · [React — Ecosystem](./4_react_ecosystem.md) · [React — Интервью](./4_react_interview.md)

### Sections

- [6. APIs](#6-apis)

---

## 6. APIs

### Overview

APIs overview

<details>
  <summary>APIs overview</summary>

### APIs overview

**Definition:** Справочник API охватывает **`react`** и **`react-dom`**: **client** (mount/hydrate), **server** (HTML-потоки), **static** (SSG).

**How it works:**

| Область         | Пакет / путь                    | На этой странице                                                     |
| --------------- | ------------------------------- | -------------------------------------------------------------------- |
| **React APIs**  | `react`                         | Context, lazy, memo, transitions, `use`, тесты — [ниже](#react-apis) |
| **Client APIs** | `react-dom/client`, `react-dom` | `createRoot`, `hydrateRoot` — [Client APIs](#client-apis)            |
| **Server APIs** | `react-dom/server`, `react`     | Streaming SSR, `cache` — [Server APIs](#server-apis)                 |
| **Static APIs** | `react-dom/static`              | Prerender — [Static APIs](#static-apis)                              |

Хуки: [Hooks](./4_react_hooks.md). Компоненты: [Components](./4_react_components.md). RSC: [React 19](./4_react_server.md).

**Sources:**

[react.dev/reference/react/apis](https://react.dev/reference/react/apis) · [react.dev/reference/react-dom](https://react.dev/reference/react-dom)

React APIs

</details>

### React APIs

<details>
  <summary>createContext</summary>

### createContext

**Definition:** `createContext(defaultValue)` создаёт объект context для `useContext` без prop drilling.

**How it works:** Один вызов на модуль; React 19: `<Ctx value={v}>`. Подробнее — [`useContext`](./4_react_hooks.md#usecontext).

**Examples:**

```tsx
export const ThemeContext = createContext<"light" | "dark">("light");
```

**Sources:**

[react.dev/reference/react/createContext](https://react.dev/reference/react/createContext)

lazy

</details>

<details>
  <summary>lazy</summary>

### lazy

**Definition:** `lazy(load)` загружает модуль при **первом рендере**; с [`<Suspense>`](./4_react_components.md#suspense).

**See also:** [React.lazy + Suspense](./4_react_hooks.md#reactlazy--suspense)

**Sources:**

[react.dev/reference/react/lazy](https://react.dev/reference/react/lazy)

memo

</details>

<details>
  <summary>memo</summary>

### memo

**Definition:** `memo(Component)` пропускает re-render при неизменных props (shallow compare).

**Sources:**

[react.dev/reference/react/memo](https://react.dev/reference/react/memo)

startTransition

</details>

<details>
  <summary>startTransition</summary>

### startTransition

**Definition:** `startTransition(fn)` помечает обновления внутри `fn` как transition.

**See also:** [`useTransition`](./4_react_hooks.md#usetransition)

**Sources:**

[react.dev/reference/react/startTransition](https://react.dev/reference/react/startTransition)

use - 19

</details>

<details>
  <summary>use - 19</summary>

### use - 19

**Definition:** `use(resource)` для Promise/Context в render. Полный разбор — [Hooks — `use() - 19`](./4_react_hooks.md#use---19).

**Sources:**

[react.dev/reference/react/use](https://react.dev/reference/react/use)

addTransitionType - 19

</details>

<details>
  <summary>addTransitionType - 19</summary>

### addTransitionType - 19

**Definition:** `addTransitionType(type)` внутри `startTransition` — причина transition для View Transitions.

**Sources:**

[react.dev/reference/react/addTransitionType](https://react.dev/reference/react/addTransitionType)

captureOwnerStack

</details>

<details>
  <summary>captureOwnerStack</summary>

### captureOwnerStack

**Definition:** Dev-only строка Owner Stack или `null`.

**Sources:**

[react.dev/reference/react/captureOwnerStack](https://react.dev/reference/react/captureOwnerStack)

act

</details>

<details>
  <summary>act</summary>

### act

**Definition:** `await act(fn)` — flush React-обновлений в тестах перед expect.

**Sources:**

[react.dev/reference/react/act](https://react.dev/reference/react/act)

Client APIs

</details>

### Client APIs

Client APIs catalog

<details>
  <summary>Client APIs catalog</summary>

### Client APIs catalog

**Definition:** **`react-dom/client`** — mount и hydrate в браузере; **`react-dom`** — `createPortal`, `flushSync`.

**How it works:**

| API                           | Import             | Роль                             |
| ----------------------------- | ------------------ | -------------------------------- |
| [`createRoot`](#createroot)   | `react-dom/client` | SPA mount (React 18+)            |
| [`hydrateRoot`](#hydrateroot) | `react-dom/client` | Интерактивность поверх SSR HTML  |
| `createPortal`                | `react-dom`        | Рендер в другой DOM-узел         |
| `flushSync`                   | `react-dom`        | Синхронный commit (escape hatch) |

**Sources:**

[react.dev/reference/react-dom/client](https://react.dev/reference/react-dom/client)

createRoot

</details>

<details>
  <summary>createRoot</summary>

### createRoot

**Definition:** `createRoot(domNode)` создаёт root и `{ render, unmount }` для клиентского DOM.

**How it works:** Замена `ReactDOM.render`; concurrent features и batching. `identifierPrefix` согласуйте с SSR при `useId`.

**Examples:**

```tsx
import { createRoot } from "react-dom/client";

createRoot(document.getElementById("root")!).render(<App />);
```

**See also:** [hydrateRoot](#hydrateroot)

**Sources:**

[react.dev/reference/react-dom/client/createRoot](https://react.dev/reference/react-dom/client/createRoot)

hydrateRoot

</details>

<details>
  <summary>hydrateRoot</summary>

### hydrateRoot

**Definition:** `hydrateRoot(domNode, reactNode)` подключает React к **уже отрендеренному** server HTML.

**How it works:** Разметка клиента должна совпадать с сервером. Опция `formState` для Actions + `useActionState`.

**Sources:**

[react.dev/reference/react-dom/client/hydrateRoot](https://react.dev/reference/react-dom/client/hydrateRoot)

Server APIs

</details>

### Server APIs

Server APIs catalog

<details>
  <summary>Server APIs catalog</summary>

### Server APIs catalog

**Definition:** **`react-dom/server`** — HTML на сервере; **`cache`** / **`cacheSignal`** из **`react`** — dedupe и abort в RSC.

**How it works:**

| API                                                 | Import             | Роль                        |
| --------------------------------------------------- | ------------------ | --------------------------- |
| [`renderToReadableStream`](#rendertoreadablestream) | `react-dom/server` | Streaming SSR (Web Streams) |
| [`renderToPipeableStream`](#rendertopipeablestream) | `react-dom/server` | Streaming SSR (Node pipe)   |
| [`renderToString`](#rendertostring)                 | `react-dom/server` | Legacy строка HTML          |
| [`cache`](#cache)                                   | `react`            | Memo на запрос (RSC)        |
| [`cacheSignal`](#cachesignal)                       | `react`            | AbortSignal после render    |

**Sources:**

[react.dev/reference/react-dom/server](https://react.dev/reference/react-dom/server)

cache

</details>

<details>
  <summary>cache</summary>

### cache

**Definition:** `cache(fn)` — memo вызовов `fn` на **один server request** (RSC).

**Sources:**

[react.dev/reference/react/cache](https://react.dev/reference/react/cache)

cacheSignal

</details>

<details>
  <summary>cacheSignal</summary>

### cacheSignal

**Definition:** `cacheSignal()` — AbortSignal при завершении server render.

**Sources:**

[react.dev/reference/react/cacheSignal](https://react.dev/reference/react/cacheSignal)

renderToReadableStream

</details>

<details>
  <summary>renderToReadableStream</summary>

### renderToReadableStream

**Definition:** Рендер дерева в **Web ReadableStream** HTML — основной streaming API.

**How it works:** Shell сначала, затем Suspense boundaries. Опции: `bootstrapScripts`, `formState`, `identifierPrefix`, `signal`.

**Sources:**

[react.dev/reference/react-dom/server/renderToReadableStream](https://react.dev/reference/react-dom/server/renderToReadableStream)

renderToPipeableStream

</details>

<details>
  <summary>renderToPipeableStream</summary>

### renderToPipeableStream

**Definition:** Node.js pipeable stream — `{ pipe, abort }` для HTTP.

**Sources:**

[react.dev/reference/react-dom/server/renderToPipeableStream](https://react.dev/reference/react-dom/server/renderToPipeableStream)

renderToString

</details>

<details>
  <summary>renderToString</summary>

### renderToString

**Definition:** Синхронная **одна строка** HTML — legacy SSR без ожидания Suspense.

**Sources:**

[react.dev/reference/react-dom/server/renderToString](https://react.dev/reference/react-dom/server/renderToString)

Static APIs

</details>

### Static APIs

Static APIs catalog

<details>
  <summary>Static APIs catalog</summary>

### Static APIs catalog

**Definition:** **`react-dom/static`** — статический HTML (SSG) после готовности данных.

**How it works:**

| API                       | Среда       | Роль                       |
| ------------------------- | ----------- | -------------------------- |
| [`prerender`](#prerender) | Web Streams | `await prerender(<App />)` |
| `prerenderToNodeStream`   | Node.js     | Node-вариант               |
| `resumeAndPrerender`      | Web Streams | Продолжение из `postponed` |

**Sources:**

[react.dev/reference/react-dom/static](https://react.dev/reference/react-dom/static)

prerender

</details>

<details>
  <summary>prerender</summary>

### prerender

**Definition:** `prerender(reactNode)` — статический HTML через Web Stream; ждёт async data.

**How it works:** Возвращает `{ prelude, postponed }`. Клиент — [`hydrateRoot`](#hydrateroot).

**Sources:**

[react.dev/reference/react-dom/static/prerender](https://react.dev/reference/react-dom/static/prerender)

---

</details>
