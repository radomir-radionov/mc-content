# React — TanStack

TanStack Query, Router, Table, Virtual, Form и связанные инструменты экосистемы.

[← Назад к хабу](./4_react.md)

### Related docs

- [React — Core](./4_react_core.md) · [React — Hooks](./4_react_hooks.md) · [React — Components](./4_react_components.md) · [React — APIs](./4_react_apis.md) · [React — React 19](./4_react_server.md) · [React — Ecosystem](./4_react_ecosystem.md) · [React — Redux](./4_react_redux.md) · [React — TanStack](./4_react_tanstack.md) · [React — Интервью](./4_react_interview.md)

### Sections

- [Overview](#overview)
- [TanStack Query](#tanstack-query)
- [Working with TanStack Query](#working-with-tanstack-query)
- [Optimistic updates](#optimistic-updates)
- [SSR prefetch & hydration](#ssr-prefetch--hydration)
- [Persistent cache](#persistent-cache)
- [TanStack Query vs RTK Query](#tanstack-query-vs-rtk-query)
- [TanStack Router](#tanstack-router)
- [TanStack Table](#tanstack-table)
- [TanStack Virtual](#tanstack-virtual)
- [TanStack Form](#tanstack-form)
- [Codegen (orval)](#codegen-orval)

---

## Overview

<details>
  <summary>TanStack ecosystem</summary>

### TanStack ecosystem

**Definition:** **TanStack** — семейство **headless, framework-agnostic** библиотек для типичных задач React-приложений — каждый пакет отвечает за логику и состояние, а не за разметку UI.

**How it works:**

| Библиотека           | Пакет                     | Задача                                                     |
| -------------------- | ------------------------- | ---------------------------------------------------------- |
| **TanStack Query**   | `@tanstack/react-query`   | Server/async state — fetch, кэш, refetch, mutations        |
| **TanStack Router**  | `@tanstack/react-router`  | Типобезопасный роутинг, loaders, search params             |
| **TanStack Table**   | `@tanstack/react-table`   | Headless таблицы — sort, filter, pagination, select        |
| **TanStack Virtual** | `@tanstack/react-virtual` | Виртуализация длинных списков — постоянный размер DOM      |
| **TanStack Form**    | `@tanstack/react-form`    | Headless формы — state, validation, submit                 |
| **TanStack Start**   | `@tanstack/react-start`   | Full-stack React (routing + SSR) — аналог Next от TanStack |

Общие цели: **TypeScript-first**, **tree-shakeable**, **без навязанного UI** — вы приносите Tailwind, shadcn/ui или свои компоненты.

**When to use:**

Client SPA (Vite) с кэшем server-state → **Query**. Типизированные URL и loaders без Next.js → **Router**. Admin-таблицы → **Table**. Списки 1000+ строк → **Virtual**. Типобезопасные формы без RHF → **Form**.

**Pluses and minuses:**

| +                                   | −                                                |
| ----------------------------------- | ------------------------------------------------ |
| Query — лучший server-state для SPA | Ещё одна экосистема рядом с React Router         |
| Headless — любая дизайн-система     | Пересечение с Next.js App Router                 |
| Единые паттерны между пакетами      | TanStack Start пересекается с Next.js full-stack |

**Thoughts:** _TanStack Query — ответ на «как fetch JSON в client SPA?» — не Redux и не `useEffect` + `fetch`._

**See also:** [Working with TanStack Query](#working-with-tanstack-query) · [Optimistic updates](#optimistic-updates) · [SSR prefetch & hydration](#ssr-prefetch--hydration) · [Persistent cache](#persistent-cache) · [TanStack Router](#tanstack-router) · [Data fetching](./4_react_ecosystem.md#data-fetching)

**Sources:**

[tanstack.com](https://tanstack.com/)

TanStack Query essentials

</details>

## TanStack Query

<details>
  <summary>TanStack Query essentials</summary>

### TanStack Query essentials

**Definition:** **TanStack Query** (ранее React Query) управляет **server state** — данными на сервере, асинхронным fetch, устареванием и общим доступом между компонентами.

**How it works:**

1. Обёртка `QueryClientProvider` + `QueryClient`
2. `useQuery({ queryKey, queryFn })` — fetch и кэш по **query key** (массив — включайте каждый вход, влияющий на ответ)
3. **staleTime** — сколько данные считаются свежими (по умолчанию **0** → refetch при mount/focus)
4. **gcTime** (в v4 было `cacheTime`) — сколько неиспользуемый кэш живёт в памяти (по умолчанию **5 мин**)
5. `useMutation` для записи; `queryClient.invalidateQueries({ queryKey })` помечает ключи stale и refetch активных observers

Server state ≠ client state. Не кладите toggles форм, модалки или UI корзины в Query — используйте `useState`, Zustand или Redux.

**When to use:**

Client SPA с poll/refetch API, optimistic updates или dedupe параллельных запросов. В Next.js сначала Server Component `fetch`; Query — только для live client-обновлений.

#### Common pitfalls

- v4 позиционный `useQuery(key, fn)` — v5 требует **объект**
- `queryKey` без filter param → неверные cache hits
- UI state в query cache
- `useEffect` + `fetch`, когда Query уже даёт loading/error/deduping

**Pluses and minuses:**

| +                                         | −                                  |
| ----------------------------------------- | ---------------------------------- |
| Автоматический dedupe, background refetch | Кривая обучения keys/invalidation  |
| Встроенные loading/error/success          | Не замена RSC server fetch         |
| Devtools для инспекции кэша               | Легко перекэшировать редкие данные |

**Examples:**

```tsx
import {
  QueryClient,
  QueryClientProvider,
  useQuery,
} from "@tanstack/react-query";

const queryClient = new QueryClient();

export function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Todos />
    </QueryClientProvider>
  );
}

function Todos() {
  const { isPending, error, data } = useQuery({
    queryKey: ["todos"],
    queryFn: () => fetch("/api/todos").then((r) => r.json()),
    staleTime: 30_000,
  });
  if (isPending) return "Loading…";
  if (error) return `Error: ${error.message}`;
  return (
    <ul>
      {data.map((t) => (
        <li key={t.id}>{t.title}</li>
      ))}
    </ul>
  );
}
```

**Thoughts:** _Query владеет «что сервер последний раз сказал» — не «что пользователь сейчас вводит»._

**See also:** [Working with TanStack Query](#working-with-tanstack-query) · [Optimistic updates](#optimistic-updates) · [SSR prefetch & hydration](#ssr-prefetch--hydration) · [Persistent cache](#persistent-cache) · [TanStack Query vs RTK Query](#tanstack-query-vs-rtk-query) · [Data fetching](./4_react_ecosystem.md#data-fetching)

**Sources:**

[tanstack.com/query](https://tanstack.com/query/latest)

Working with TanStack Query

</details>

## Working with TanStack Query

<details>
  <summary>Working with TanStack Query</summary>

### Working with TanStack Query

**Definition:** Практические паттерны queries, mutations, invalidation и интеграции в React.

**How it works:**

| Задача                | API / паттерн                                                                  |
| --------------------- | ------------------------------------------------------------------------------ |
| Fetch при mount       | `useQuery({ queryKey, queryFn })`                                              |
| Пропуск до готовности | `enabled: !!userId`                                                            |
| Зависимый fetch       | `queryKey: ['user', userId]` — refetch при смене `userId`                      |
| Create/update/delete  | `useMutation` + `invalidateQueries`                                            |
| Optimistic UI         | [Optimistic updates](#optimistic-updates) — `onMutate` + rollback + invalidate |
| Пагинация / infinite  | `useInfiniteQuery` + `getNextPageParam`                                        |
| Prefetch на hover     | `queryClient.prefetchQuery({ queryKey, queryFn })`                             |
| Глобальные defaults   | `new QueryClient({ defaultOptions: { queries: { staleTime, queryFn } } })`     |

**Invalidation:** `queryClient.invalidateQueries({ queryKey: ['todos'] })` помечает matches stale (перебивает `staleTime`) и refetch observers. Предпочитайте targeted invalidation ручной правке кэша.

**When to use:**

Любой client-side CRUD к REST/GraphQL. Пара с **orval** для generated hooks из OpenAPI.

#### Common pitfalls

- Слишком широкая invalidation → thundering herd refetch
- Забытый `enabled` для query, ждущего ID из другого query
- Derived UI state в `select`, когда хватит `useMemo` на `data`

**Examples:**

```tsx
import { useMutation, useQuery, useQueryClient } from "@tanstack/react-query";

function TodoList() {
  const queryClient = useQueryClient();
  const { data: todos } = useQuery({
    queryKey: ["todos"],
    queryFn: fetchTodos,
  });
  const { mutate, isPending } = useMutation({
    mutationFn: addTodo,
    onSuccess: () => queryClient.invalidateQueries({ queryKey: ["todos"] }),
  });
  return (
    <>
      <button disabled={isPending} onClick={() => mutate({ title: "New" })}>
        Add
      </button>
      <ul>
        {todos?.map((t) => (
          <li key={t.id}>{t.title}</li>
        ))}
      </ul>
    </>
  );
}
```

**Thoughts:** _Invalidate минимальный query key, покрывающий изменённые данные — `['todos']`, не все ключи приложения._

**See also:** [TanStack Query essentials](#tanstack-query-essentials) · [Optimistic updates](#optimistic-updates) · [SSR prefetch & hydration](#ssr-prefetch--hydration) · [Persistent cache](#persistent-cache) · [Codegen (orval)](#codegen-orval) · [TanStack Query vs RTK Query](#tanstack-query-vs-rtk-query)

**Sources:**

[tanstack.com/query — invalidation](https://tanstack.com/query/latest/docs/framework/react/guides/query-invalidation) · [tanstack.com/query — mutations](https://tanstack.com/query/latest/docs/framework/react/guides/mutations)

Optimistic updates

</details>

## Optimistic updates

<details>
  <summary>Optimistic updates</summary>

### Optimistic updates

**Definition:** **Optimistic updates** меняют кэш Query **до** ответа сервера, чтобы UI реагировал мгновенно; при ошибке откатываются к снимку из `onMutate`.

**How it works:**

1. **`onMutate`** — `await queryClient.cancelQueries({ queryKey })`, чтобы in-flight fetch не перезаписал патч
2. Снимок — `const previous = queryClient.getQueryData(['todos'])`
3. Патч — `queryClient.setQueryData(['todos'], updater)` (или `setQueriesData` для paginated keys)
4. Вернуть снимок как **context** для rollback
5. **`onError`** — `queryClient.setQueryData(['todos'], context.previousTodos)`
6. **`onSettled`** — `invalidateQueries`, чтобы после success/failure победили данные сервера

Для простых toggles подойдёт React 19 [`useOptimistic - 19`](./4_react_hooks.md#useoptimistic-19) без правки кэша Query — optimistic flow Query нужен, когда UI читает данные из `useQuery`.

**When to use:** Like/unlike, чекбоксы todo, reorder списков, inline edit, где заметна задержка сети.

#### Common pitfalls

- Без `cancelQueries` медленный refetch перезапишет optimistic patch
- Optimistic item без стабильного `id` — rollback и merge ломаются
- Нет `invalidateQueries` в `onSettled` — UI расходится с сервером

**Pluses and minuses:**

| +                               | −                                     |
| ------------------------------- | ------------------------------------- |
| Мгновенный отклик UI            | Нужен понятный rollback при ошибке    |
| Работает со всеми `useQuery`    | Больше кода, чем только `onSuccess`   |
| Официальный lifecycle mutations | Легко сломать paginated/infinite keys |

**Examples:**

```tsx
import { useMutation, useQuery, useQueryClient } from "@tanstack/react-query";

type Todo = { id: string; title: string; done: boolean };

function TodoList() {
  const queryClient = useQueryClient();
  const { data: todos = [] } = useQuery({
    queryKey: ["todos"],
    queryFn: fetchTodos,
  });

  const toggleDone = useMutation({
    mutationFn: ({ id, done }: { id: string; done: boolean }) =>
      patchTodo(id, { done }),
    onMutate: async ({ id, done }) => {
      await queryClient.cancelQueries({ queryKey: ["todos"] });
      const previousTodos = queryClient.getQueryData<Todo[]>(["todos"]);
      queryClient.setQueryData<Todo[]>(["todos"], (old = []) =>
        old.map((t) => (t.id === id ? { ...t, done } : t)),
      );
      return { previousTodos };
    },
    onError: (_err, _vars, context) => {
      queryClient.setQueryData(["todos"], context?.previousTodos);
    },
    onSettled: () => {
      queryClient.invalidateQueries({ queryKey: ["todos"] });
    },
  });

  return (
    <ul>
      {todos.map((t) => (
        <li key={t.id}>
          <input
            type="checkbox"
            checked={t.done}
            onChange={() => toggleDone.mutate({ id: t.id, done: !t.done })}
          />
          {t.title}
        </li>
      ))}
    </ul>
  );
}
```

**Thoughts:** _Optimistic UI — это запись в кэш, не второй store; в `onSettled` всегда сверяйтесь с сервером._

**See also:** [Working with TanStack Query](#working-with-tanstack-query) · [useOptimistic - 19](./4_react_hooks.md#useoptimistic-19)

**Sources:**

[tanstack.com/query — optimistic updates](https://tanstack.com/query/latest/docs/framework/react/guides/optimistic-updates) · [tanstack.com/query — mutations](https://tanstack.com/query/latest/docs/framework/react/guides/mutations)

SSR prefetch & hydration

</details>

## SSR prefetch & hydration

<details>
  <summary>SSR prefetch & hydration</summary>

### SSR prefetch & hydration

**Definition:** **SSR prefetch + hydration** загружает данные на сервере в `QueryClient`, сериализует через `dehydrate` и восстанавливает на клиенте через `HydrationBoundary`, чтобы `useQuery` сразу отдал кэш — без loading flash при первом paint.

**How it works:**

| Шаг               | Где                         | API                                                      |
| ----------------- | --------------------------- | -------------------------------------------------------- |
| 1. Prefetch       | Server Component или loader | `await queryClient.prefetchQuery({ queryKey, queryFn })` |
| 2. Serialize      | Тот же request              | `dehydrate(queryClient)`                                 |
| 3. Передать state | Server → client boundary    | `<HydrationBoundary state={dehydratedState}>`            |
| 4. Потребить      | Client Component            | `useQuery` с тем же `queryKey` + `queryFn`               |

**Provider (Next.js App Router):** один `QueryClient` на **request** на сервере; **singleton** в браузере, чтобы Suspense не пересоздавал client. `defaultOptions.queries.staleTime` > `0` (например 60s), иначе клиент сразу refetch после hydration.

В Next.js для initial data чаще достаточно **RSC `fetch`**; TanStack Query добавляйте, когда те же данные должны обновляться после hydration. См. [Next.js — Data](./4_nextjs_data.md#client-side-fetching).

**When to use:** Vite/SPA с SSR, Client Components в Next.js с совпадением server HTML, loaders TanStack Router/Start с dehydrated cache.

#### Common pitfalls

- Один global `QueryClient` на все server requests — утечка данных между пользователями
- `staleTime: 0` — refetch сразу после hydration
- Разные `queryKey` / `queryFn` на сервере и клиенте
- Без `HydrationBoundary` клиент стартует с пустым кэшем

**Pluses and minuses:**

| +                                      | −                                       |
| -------------------------------------- | --------------------------------------- |
| Нет спиннера на hydrated queries       | Больше wiring, чем только RSC fetch     |
| Можно смешивать prefetch и client-only | Нужны границы server/client             |
| Framework-agnostic hydration API       | `HydrationBoundary` на сегмент маршрута |

**Examples:**

```tsx
// app/providers.tsx
"use client";

import { QueryClient, QueryClientProvider } from "@tanstack/react-query";

function makeQueryClient() {
  return new QueryClient({
    defaultOptions: { queries: { staleTime: 60_000 } },
  });
}

let browserQueryClient: QueryClient | undefined;

function getQueryClient() {
  if (typeof window === "undefined") return makeQueryClient();
  if (!browserQueryClient) browserQueryClient = makeQueryClient();
  return browserQueryClient;
}

export function Providers({ children }: { children: React.ReactNode }) {
  return (
    <QueryClientProvider client={getQueryClient()}>
      {children}
    </QueryClientProvider>
  );
}
```

```tsx
// app/posts/page.tsx — Server Component
import {
  dehydrate,
  HydrationBoundary,
  QueryClient,
} from "@tanstack/react-query";
import { Posts } from "./posts";

export default async function PostsPage() {
  const queryClient = new QueryClient();
  await queryClient.prefetchQuery({
    queryKey: ["posts"],
    queryFn: getPosts,
  });
  return (
    <HydrationBoundary state={dehydrate(queryClient)}>
      <Posts />
    </HydrationBoundary>
  );
}
```

```tsx
// app/posts/posts.tsx
"use client";

import { useQuery } from "@tanstack/react-query";

export function Posts() {
  const { data } = useQuery({ queryKey: ["posts"], queryFn: getPosts });
  return (
    <ul>
      {data?.map((p) => (
        <li key={p.id}>{p.title}</li>
      ))}
    </ul>
  );
}
```

**Thoughts:** _Server Components — ещё один loader: prefetch → dehydrate → hydrate._

**See also:** [Hydration](./4_react_server.md#hydration) · [Next.js — Data](./4_nextjs_data.md#client-side-fetching) · [TanStack Router](#tanstack-router)

**Sources:**

[tanstack.com/query — Advanced SSR](https://tanstack.com/query/latest/docs/framework/react/guides/advanced-ssr) · [tanstack.com/query — SSR](https://tanstack.com/query/latest/docs/framework/react/guides/ssr)

Persistent cache

</details>

## Persistent cache

<details>
  <summary>Persistent cache</summary>

### Persistent cache

**Definition:** **Persistent cache** сохраняет dehydrated кэш Query на **диск** (обычно `localStorage`) и восстанавливает при следующем визите, чтобы queries рендерились до ответа сети.

**How it works:**

1. Пакеты `@tanstack/react-query-persist-client` и `@tanstack/query-async-storage-persister`
2. `PersistQueryClientProvider` вместо `QueryClientProvider`
3. `persistOptions={{ persister, maxAge, buster }}`
4. При mount кэш **восстанавливается**; queries ждут окончания restore
5. Запись в storage **throttled** (по умолчанию ~1s) при обновлениях кэша

`gcTime` должен быть **≥ `maxAge`**, иначе записи исчезнут из памяти раньше, чем истекут на диске. `buster` инвалидирует весь persisted cache после деплоя или смены схемы.

**When to use:** Быстрый повторный визит, offline **read** несекретных списков, меньше спиннеров в больших SPA. **Не** храните токены, PII и admin-данные без шифрования и threat model.

#### Common pitfalls

- Секреты в plain `localStorage`
- `gcTime` < `maxAge` — restore один раз, потом GC
- Persistence вместо server validation — auth и mutations всё равно проверяйте
- Огромный cache — фильтруйте через `dehydrateOptions.shouldDehydrateQuery`

**Pluses and minuses:**

| +                               | −                                       |
| ------------------------------- | --------------------------------------- |
| Мгновенный UI при возврате      | Stale data при слишком большом `maxAge` |
| Offline read для cached queries | Лимиты storage в браузере               |
| `buster` для безопасного deploy | Доп. пакеты и edge cases restore        |

**Examples:**

```tsx
"use client";

import { QueryClient } from "@tanstack/react-query";
import { PersistQueryClientProvider } from "@tanstack/react-query-persist-client";
import { createAsyncStoragePersister } from "@tanstack/query-async-storage-persister";

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      gcTime: 1000 * 60 * 60 * 24,
      staleTime: 60_000,
    },
  },
});

const persister = createAsyncStoragePersister({
  storage: typeof window !== "undefined" ? window.localStorage : undefined,
});

export function Providers({ children }: { children: React.ReactNode }) {
  return (
    <PersistQueryClientProvider
      client={queryClient}
      persistOptions={{
        persister,
        maxAge: 1000 * 60 * 60 * 24,
        buster: "v1",
      }}
    >
      {children}
    </PersistQueryClientProvider>
  );
}
```

**Thoughts:** _Persistence ускоряет публичные read — не security-слой и не offline-write._

**See also:** [TanStack Query essentials](#tanstack-query-essentials) · [SSR prefetch & hydration](#ssr-prefetch--hydration)

**Sources:**

[tanstack.com/query — persistQueryClient](https://tanstack.com/query/latest/docs/framework/react/plugins/persistQueryClient) · [tanstack.com/query — createAsyncStoragePersister](https://tanstack.com/query/latest/docs/framework/react/plugins/createAsyncStoragePersister)

TanStack Query vs RTK Query

</details>

## TanStack Query vs RTK Query

<details>
  <summary>TanStack Query vs RTK Query</summary>

### TanStack Query vs RTK Query

**Definition:** Два кэша **server-state**. TanStack Query — автономный кэш с ключом `queryKey`. RTK Query — та же идея в Redux Toolkit (`createApi`), кэш сервера в Redux store.

**How it works:**

Оба кэшируют JSON в памяти (не HTTP `Cache-Control`). TanStack Query v5: `useQuery({ queryKey, queryFn, staleTime })`. По умолчанию `staleTime` — **0**; `gcTime` — **5 минут**. RTK Query: `createApi` + `keepUnusedDataFor` (по умолчанию **60 с**); хуки из endpoints.

| | TanStack Query | RTK Query |
| | -------------- | --------------------------------------------- | ------------------------------ |
| | Store | Свой `QueryClient` | Redux store |
| | Boilerplate | Низкий | Выше (`createApi`, slices) |
| | Лучше с | Zustand / Context для UI state | Существующее Redux Toolkit app |
| | Invalidation | `queryClient.invalidateQueries({ queryKey })` | `invalidatesTags` на mutations |

**When to use:**

Новый SPA без Redux → TanStack Query. Большая кодовая база Redux → RTK Query. RSC/Next.js server `fetch` сначала; client cache — только для live-обновлений.

#### Common pitfalls

v4 позиционные аргументы — v5 требует объект. UI state в Query. `queryKey` без всех входов, влияющих на ответ.

**Pluses and minuses:**

| +                                        | −                                            |
| ---------------------------------------- | -------------------------------------------- |
| Query: гибкий, framework-agnostic        | Легко использовать как global UI store (нет) |
| RTK Query: один store, DevTools, codegen | Связывает server cache с Redux               |

**Examples:**

```tsx
import { useQuery, useMutation, useQueryClient } from "@tanstack/react-query";

function Todos() {
  const queryClient = useQueryClient();
  const { data } = useQuery({
    queryKey: ["todos"],
    queryFn: fetchTodos,
    staleTime: 30_000,
  });
  const { mutate } = useMutation({
    mutationFn: addTodo,
    onSuccess: () => queryClient.invalidateQueries({ queryKey: ["todos"] }),
  });
  return (
    <ul>
      {data?.map((t) => (
        <li key={t.id}>{t.title}</li>
      ))}
    </ul>
  );
}
```

**Thoughts:** Query кэширует **server** data. Redux/Zustand — **client** data. Не смешивайте.

**See also:** [TanStack Query essentials](#tanstack-query-essentials) · [Redux — RTK Query](./4_react_redux.md#rtk-query) · [When to use Redux](./4_react_redux.md#when-to-use-redux)

**Sources:**

[tanstack.com/query](https://tanstack.com/query/latest) · [redux-toolkit.js.org/rtk-query](https://redux-toolkit.js.org/rtk-query/overview)

TanStack Router

</details>

## TanStack Router

<details>
  <summary>TanStack Router</summary>

### TanStack Router

**Definition:** Полностью типобезопасный React router с search params, loaders и file-based routes — типы `params` и loader data выводятся из определения маршрута.

**How it works:** File routes: `Route = createFileRoute('/posts/$postId')({ loader, validateSearch, component })`. `Route.useParams()`, `Route.useLoaderData()`, `Route.useSearch()` наследуют типы из файла. `validateSearch` (часто Zod) парсит query string; `loaderDeps` передают validated search в loader.

**When to use:**

Vite/SPA с типизированными URL; dashboards с фильтрами в query string. **Не** замена Next.js App Router RSC.

#### Common pitfalls

Next.js `app/` conventions внутри TanStack app. Пропуск `validateSearch` — `?offset=abc` ломает loader.

**Pluses and minuses:**

| +                                          | −                                                 |
| ------------------------------------------ | ------------------------------------------------- |
| Типизированные params, search, loader data | Другой router, если уже есть Next.js App Router   |
| Search params как first-class state        | Настройка file-plugin для `createFileRoute`       |
| Loaders без fetch-on-mount waterfalls      | Пересечение с Next.js / React Router v7 framework |

**Examples:**

```tsx
import { createFileRoute } from "@tanstack/react-router";
import { z } from "zod";

export const Route = createFileRoute("/posts/$postId")({
  validateSearch: z.object({ tab: z.enum(["info", "comments"]).catch("info") }),
  loader: async ({ params }) => {
    const post = await fetchPost(params.postId);
    return { post };
  },
  component: PostPage,
});

function PostPage() {
  const { post } = Route.useLoaderData();
  const { postId } = Route.useParams();
  const { tab } = Route.useSearch();
  return (
    <article>
      <h1>{post.title}</h1>
      <p>
        {postId} / {tab}
      </p>
    </article>
  );
}
```

**Thoughts:** Модуль маршрута _является_ источником типов — hooks без ручных generics.

**See also:** [Routing](./4_react_ecosystem.md#routing) · [nuqs](./4_react_ecosystem.md#nuqs-url-search-params) · [TanStack ecosystem](#tanstack-ecosystem)

**Sources:**

[tanstack.com/router](https://tanstack.com/router)

TanStack Table

</details>

## TanStack Table

<details>
  <summary>TanStack Table</summary>

### TanStack Table

**Definition:** **Headless** table engine — sorting, filtering, pagination, row selection и column visibility как **state + helpers**, без встроенной разметки.

**How it works:**

1. **Columns** (`createColumnHelper` или `accessorKey`)
2. `useReactTable({ data, columns, getCoreRowModel, … })` — instance таблицы
3. Опциональные **features** через `tableFeatures` — filtering, sorting, pagination (client row models или server-side manual state)
4. Рендер через `flexRender` — вы владеете `<table>`, `<tr>`, `<td>`

Для больших datasets — пара с [TanStack Virtual](#tanstack-virtual).

**When to use:**

Admin dashboards, data grids с кастомным дизайном. Пропустите для простой статической `<table>` на 10 строк.

#### Common pitfalls

Ожидание styled components — только логика. Забытый `getCoreRowModel()`. Client row models на 100k строк — server-side pagination.

**Pluses and minuses:**

| +                              | −                                 |
| ------------------------------ | --------------------------------- |
| Полный контроль markup и a11y  | Больше setup, чем `<table>` + map |
| Client или server sort/filter  | Сложный API для простых таблиц    |
| Любая UI library (shadcn, MUI) | Virtualization — отдельный пакет  |

**Examples:**

```tsx
import {
  createColumnHelper,
  flexRender,
  getCoreRowModel,
  useReactTable,
} from "@tanstack/react-table";

type Person = { name: string; age: number };
const columnHelper = createColumnHelper<Person>();

const columns = [
  columnHelper.accessor("name", { header: "Name" }),
  columnHelper.accessor("age", { header: "Age" }),
];

function DataTable({ data }: { data: Person[] }) {
  const table = useReactTable({
    data,
    columns,
    getCoreRowModel: getCoreRowModel(),
  });
  return (
    <table>
      <thead>
        {table.getHeaderGroups().map((hg) => (
          <tr key={hg.id}>
            {hg.headers.map((h) => (
              <th key={h.id}>
                {flexRender(h.column.columnDef.header, h.getContext())}
              </th>
            ))}
          </tr>
        ))}
      </thead>
      <tbody>
        {table.getRowModel().rows.map((row) => (
          <tr key={row.id}>
            {row.getVisibleCells().map((cell) => (
              <td key={cell.id}>
                {flexRender(cell.column.columnDef.cell, cell.getContext())}
              </td>
            ))}
          </tr>
        ))}
      </tbody>
    </table>
  );
}
```

**Thoughts:** _Headless = вы стилизуете; библиотека владеет sort/filter/pagination state._

**See also:** [TanStack Virtual](#tanstack-virtual) · [List virtualization](./4_react_ecosystem.md#list-virtualization)

**Sources:**

[tanstack.com/table](https://tanstack.com/table)

TanStack Virtual

</details>

## TanStack Virtual

<details>
  <summary>TanStack Virtual</summary>

### TanStack Virtual

**Definition:** Рендер только **видимых строк** (плюс overscan) в длинных scrollable списках — число DOM-узлов постоянно.

**How it works:** `useVirtualizer({ count, getScrollElement, estimateSize })` вычисляет индексы в viewport по `scrollTop` и высоте контейнера. Только они монтируются; off-screen отсутствуют в DOM.

**When to use:** Таблицы на тысячи строк, infinite feeds, chat, логи. Порог: **1000+ DOM nodes** в одном scroll container.

**Pluses and minuses:**

| +                                  | −                                                      |
| ---------------------------------- | ------------------------------------------------------ |
| 10k+ строк без замедления браузера | Фиксированная высота строк проще; variable — измерения |
| Постоянная память и paint cost     | Find-in-page не видит off-screen строки                |
| Пара с TanStack Table              | Keyboard navigation — доп. a11y                        |

**Examples:**

```tsx
import { useRef } from "react";
import { useVirtualizer } from "@tanstack/react-virtual";

function VirtualList({ items }: { items: string[] }) {
  const parentRef = useRef<HTMLDivElement>(null);
  const virtualizer = useVirtualizer({
    count: items.length,
    getScrollElement: () => parentRef.current,
    estimateSize: () => 36,
  });
  return (
    <div ref={parentRef} style={{ height: 400, overflow: "auto" }}>
      <div style={{ height: virtualizer.getTotalSize(), position: "relative" }}>
        {virtualizer.getVirtualItems().map((row) => (
          <div
            key={row.index}
            style={{ position: "absolute", top: row.start, height: row.size }}
          >
            {items[row.index]}
          </div>
        ))}
      </div>
    </div>
  );
}
```

**See also:** [List virtualization](./4_react_ecosystem.md#list-virtualization) · [TanStack Table](#tanstack-table)

**Sources:**

[tanstack.com/virtual](https://tanstack.com/virtual)

TanStack Form

</details>

## TanStack Form

<details>
  <summary>TanStack Form</summary>

### TanStack Form

**Definition:** **Headless**, type-safe form state для React — field-level validation, async validators, submit без навязанной разметки.

**How it works:**

`useForm({ defaultValues, onSubmit })` возвращает form API. Поля через `form.Field` с `validators` (sync/async). Fine-grained updates — re-render только подписанных полей.

**When to use:**

Типобезопасные формы с headless control. Если уже **RHF + Zod** — можно остаться на RHF — см. [Forms](./4_react_ecosystem.md#forms).

#### Common pitfalls

Переписывание RHF field arrays без TanStack Form array APIs. Смешение uncontrolled native inputs с form state.

**Pluses and minuses:**

| +                            | −                                      |
| ---------------------------- | -------------------------------------- |
| Сильный TypeScript inference | Меньше экосистема, чем React Hook Form |
| Headless — любой UI          | Новее; меньше ответов на SO            |
| Framework-agnostic core      | shadcn/ui чаще с RHF                   |

**Examples:**

```tsx
import { useForm } from "@tanstack/react-form";

function SignupForm() {
  const form = useForm({
    defaultValues: { email: "" },
    onSubmit: async ({ value }) => {
      await saveUser(value);
    },
  });
  return (
    <form
      onSubmit={(e) => {
        e.preventDefault();
        form.handleSubmit();
      }}
    >
      <form.Field
        name="email"
        validators={{
          onChange: ({ value }) =>
            !value.includes("@") ? "Invalid email" : undefined,
        }}
      >
        {(field) => (
          <>
            <input
              value={field.state.value}
              onChange={(e) => field.handleChange(e.target.value)}
            />
            {field.state.meta.errors.join(", ")}
          </>
        )}
      </form.Field>
      <button type="submit">Save</button>
    </form>
  );
}
```

**Thoughts:** _RHF — размер экосистемы; TanStack Form — headless + общий core между фреймворками._

**See also:** [Forms](./4_react_ecosystem.md#forms) · [Controlled vs uncontrolled inputs](./4_react_core.md#controlled-vs-uncontrolled-inputs)

**Sources:**

[tanstack.com/form](https://tanstack.com/form)

Codegen (orval)

</details>

## Codegen (orval)

<details>
  <summary>Codegen (orval)</summary>

### Codegen (orval)

**Definition:** Генерирует TypeScript clients, models и опциональные **TanStack Query hooks** из OpenAPI/Swagger — типы фронтенда совпадают с API contract.

**How it works:** `orval.config.ts` на YAML/JSON spec; `output.client: 'react-query'`. Orval эмитит query functions, keys и `useX` hooks. **Mutator** для shared Axios/`fetch` (auth). Re-run в CI при смене spec.

**When to use:** Backend публикует OpenAPI; фронтенд хочет typed hooks без ручного `queryKey` + `queryFn` на endpoint.

**See also:** [Working with TanStack Query](#working-with-tanstack-query) · [orval](./4_react_ecosystem.md#orval-openapi-codegen)

**Sources:**

[orval.dev](https://orval.dev/)

</details>
