# React — REST API client

Вызов REST API из React: `fetch`, axios, ошибки и клиентское кэширование.

[← Назад к хабу](./4_react.md)

### Related docs

[Web APIs — REST](./6_web_api_rest.md) · [Web APIs — HTTP](./6_web_api_http.md) · [TypeScript — API contracts](./5_ts_api_contracts.md) · [TanStack Query](./4_react_tanstack.md) · [RTK Query](./4_react_redux.md#rtk-query-essentials) · [Ecosystem — Data fetching](./4_react_ecosystem.md#data-fetching)

### Main concepts

| Topic | Covers |
| ----- | ------ |
| [Overview](#overview) | Слои: transport, client, cache |
| [fetch](#fetch) | Native API, `Response`, ошибки |
| [axios](#axios) | Instances, interceptors |
| [Error handling](#error-handling) | Типизированные ошибки, состояния UI |
| [Caching strategies](#caching-strategies) | Query-библиотеки vs ручной state |

### Sections

- [Overview](#overview)
- [fetch](#fetch)
- [axios](#axios)
- [Error handling](#error-handling)
- [Caching strategies](#caching-strategies)

---

## Overview

<details>
<summary>Overview</summary>

**Definition:** **REST client layer** в React превращает HTTP-запросы в **типизированные данные** для компонентов — с единообразными **ошибками**, **auth headers** и политикой **cache/refetch**.

**How it works:**

```text
Component → hook (useQuery / custom) → client (fetch/axios) → REST API
                ↑                              ↑
           cache library                  interceptors, base URL
```

| Concern | Where it lives |
| ------- | -------------- |
| URL + verbs | API-модуль или сгенерированный OpenAPI client |
| Auth header | axios interceptor или обёртка `fetch` |
| Loading/error UI | Компонент или suspense boundary |
| Cache + refetch | TanStack Query, RTK Query, SWR |

**When to use:** Почти каждое React SPA. Для server state предпочитайте **cache library**, а не голый `useEffect` + `fetch`.

**Sources:** [MDN — fetch](https://developer.mozilla.org/en-US/docs/Web/API/fetch) · [REST contract](./6_web_api_rest.md#rest-principles-and-contract)

</details>

---

## fetch

<details>
<summary>fetch</summary>

**Definition:** **`fetch`** — стандарт браузера (и Node) для HTTP — возвращает `Promise<Response>`; тело читается отдельно (`json()`, `text()`).

**How it works:**

- Всегда проверяйте `response.ok` — `fetch` не reject на 404/500
- Парсите body один раз; обрабатывайте пустой `204`
- Передавайте `signal` из `AbortController` для cleanup при unmount
- `credentials: 'include'` для cookie-сессий; учитывайте CORS ([HTTP — CORS](./6_web_api_http.md))

**When to use:** Без лишней зависимости; обёртки для всех вызовов приложения.

**Examples:**

```ts
type ApiError = { status: number; message: string };

async function apiGet<T>(path: string, init?: RequestInit): Promise<T> {
  const res = await fetch(`/api${path}`, {
    ...init,
    headers: { Accept: "application/json", ...init?.headers },
  });
  if (!res.ok) {
    const message = await res.text().catch(() => res.statusText);
    throw { status: res.status, message } satisfies ApiError;
  }
  return res.json() as Promise<T>;
}
```

```tsx
useEffect(() => {
  const ac = new AbortController();
  apiGet<User[]>("/users", { signal: ac.signal })
    .then(setUsers)
    .catch((e: ApiError) => setError(e.message));
  return () => ac.abort();
}, []);
```

**Thoughts:** Лучше вынести fetch в **query hooks** ([Caching strategies](#caching-strategies)), а не оставлять голый `useEffect`.

**Sources:** [MDN — Using fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)

</details>

---

## axios

<details>
<summary>axios</summary>

**Definition:** **axios** — популярный HTTP client с **instances**, **interceptors**, автоматическим JSON и hooks запроса/ответа.

**How it works:**

| Feature | Use |
| ------- | --- |
| `axios.create({ baseURL, timeout })` | Общий config |
| Request interceptor | `Authorization`, request IDs |
| Response interceptor | Глобальный 401 → logout, нормализация ошибок |
| `axios.isAxiosError(e)` | Сужение типа ошибок |

**When to use:** Команды с централизованным auth/refresh; JSON POST проще, чем сырой `fetch`.

**Examples:**

```ts
import axios from "axios";

export const http = axios.create({
  baseURL: import.meta.env.VITE_API_URL,
  timeout: 15_000,
});

http.interceptors.request.use((config) => {
  const token = localStorage.getItem("access_token");
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

http.interceptors.response.use(
  (res) => res,
  (error) => {
    if (axios.isAxiosError(error) && error.response?.status === 401) {
      window.dispatchEvent(new Event("auth:logout"));
    }
    return Promise.reject(error);
  },
);
```

**Pluses and minuses:**

| + | − |
| - | - |
| Interceptors, instances | Доп. размер бандла vs fetch |
| Привычный API | Всё равно нужен слой кэша (Query/RTK) |

**Sources:** [axios — docs](https://axios-http.com/docs/intro)

</details>

---

## Error handling

<details>
<summary>Error handling</summary>

**Definition:** **Error handling** сопоставляет HTTP и сетевые сбои с **типизированными исходами**, которые UI может отрисовать — не generic `alert()` и не тихие провалы.

**How it works:**

| Failure | User-facing pattern |
| ------- | ------------------- |
| **4xx** validation | Ошибки полей из `{ errors: { field: msg } }` |
| **401/403** | Редирект на login или «нет прав» |
| **5xx** / network | Retry, support id, offline banner |
| **Abort** | Игнор (unmount) — не ставить error state |

Discriminated union в hooks:

```ts
type RequestState<T> =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: T }
  | { status: "error"; error: ApiError };
```

**When to use:** Каждый публичный API-модуль; согласуйте с backend [error contract](./6_web_api_rest.md#rest-principles-and-contract).

**Examples:**

```tsx
function UsersPanel() {
  const { data, error, isLoading, refetch } = useGetUsersQuery();

  if (isLoading) return <Spinner />;
  if (error)
    return (
      <Alert role="alert">
        Failed to load users.{" "}
        <button type="button" onClick={() => refetch()}>
          Retry
        </button>
      </Alert>
    );
  return <UserList users={data!} />;
}
```

**See also:** [Resilience — error boundaries](./4_react_resilience.md) · [Integration tests — MSW errors](./4_react_testing.md#integration-tests)

**Sources:** [TypeScript — API contracts](./5_ts_api_contracts.md)

</details>

---

## Caching strategies

<details>
<summary>Caching strategies</summary>

**Definition:** **Client caching** хранит server JSON в памяти (по ключу query), дедуплирует in-flight запросы и **refetch** по focus, interval или mutation — отдельно от HTTP `Cache-Control` на CDN.

**How it works:**

| Library | Model | React integration |
| ------- | ----- | ----------------- |
| **TanStack Query** | `useQuery` / `useMutation`, query keys | De facto для новых SPA |
| **RTK Query** | `createApi` + tags в Redux store | Когда Redux уже центральный |
| **SWR** | `useSWR` stale-while-revalidate | Лёгкие виджеты |

**When to use:** Дефолт для **server state**. **UI state** — локально (`useState`) или Redux/Zustand только когда shared.

**Examples:**

```tsx
import { useQuery, useMutation, useQueryClient } from "@tanstack/react-query";
import { http } from "@/lib/http";

function useUsers() {
  return useQuery({
    queryKey: ["users"],
    queryFn: () => http.get<User[]>("/users").then((r) => r.data),
    staleTime: 60_000,
  });
}

function useUpdateUser() {
  const qc = useQueryClient();
  return useMutation({
    mutationFn: (body: UpdateUser) => http.patch(`/users/${body.id}`, body),
    onSuccess: () => qc.invalidateQueries({ queryKey: ["users"] }),
  });
}
```

**Pluses and minuses:**

| + | − |
| - | - |
| Без ручной синхронизации `useEffect` | Нужно учить query keys / invalidation |
| Background refetch | Stale UI при неверном `staleTime` |

**Thoughts:** HTTP-кэш для static GET → [HTTP caching](./6_web_api_http.md). Серверный кэш Next.js → [Next.js data](./4_nextjs_data.md).

**Sources:** [TanStack Query](https://tanstack.com/query/latest) · [Ecosystem — Data fetching](./4_react_ecosystem.md#data-fetching) · [SWR](./4_react_ecosystem.md#swr-stale-while-revalidate)

</details>
