# React — REST API client

Calling REST APIs from React: `fetch`, axios, errors, and client-side caching.

[← Back to hub](./4_react.md)

### Related docs

[Web APIs — REST](./6_web_api_rest.md) · [Web APIs — HTTP](./6_web_api_http.md) · [TypeScript — API contracts](./5_ts_api_contracts.md) · [TanStack Query](./4_react_tanstack.md) · [RTK Query](./4_react_redux.md#rtk-query-essentials) · [Ecosystem — Data fetching](./4_react_ecosystem.md#data-fetching)

### Main concepts

| Topic | Covers |
| ----- | ------ |
| [Overview](#overview) | Layers: transport, client, cache |
| [fetch](#fetch) | Native API, `Response`, errors |
| [axios](#axios) | Instances, interceptors |
| [Error handling](#error-handling) | Typed errors, UI states |
| [Caching strategies](#caching-strategies) | Query libraries vs manual state |

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

**Definition:** A **REST client layer** in React turns HTTP requests into **typed data** for components — with consistent **errors**, **auth headers**, and **cache/refetch** policy.

**How it works:**

```text
Component → hook (useQuery / custom) → client (fetch/axios) → REST API
                ↑                              ↑
           cache library                  interceptors, base URL
```

| Concern | Where it lives |
| ------- | -------------- |
| URL + verbs | API module or generated OpenAPI client |
| Auth header | axios interceptor or `fetch` wrapper |
| Loading/error UI | Component or suspense boundary |
| Cache + refetch | TanStack Query, RTK Query, SWR |

**When to use:** Almost every React SPA. Prefer a **cache library** over raw `useEffect` + `fetch` for server state.

**Sources:** [MDN — fetch](https://developer.mozilla.org/en-US/docs/Web/API/fetch) · [REST contract](./6_web_api_rest.md#rest-principles-and-contract)

</details>

---

## fetch

<details>
<summary>fetch</summary>

**Definition:** **`fetch`** is the browser (and Node) standard for HTTP — returns a `Promise<Response>`; body is read separately (`json()`, `text()`).

**How it works:**

- Always check `response.ok` — `fetch` does not reject on 404/500
- Parse body once; handle empty `204`
- Pass `signal` from `AbortController` for cleanup on unmount
- `credentials: 'include'` for cookie sessions; mind CORS ([HTTP — CORS](./6_web_api_http.md))

**When to use:** No extra dependency; wrappers for all app calls.

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

**Thoughts:** Prefer moving fetch into **query hooks** ([Caching strategies](#caching-strategies)) instead of bare `useEffect`.

**Sources:** [MDN — Using fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)

</details>

---

## axios

<details>
<summary>axios</summary>

**Definition:** **axios** is a popular HTTP client with **instances**, **interceptors**, automatic JSON transform, and request/response hooks.

**How it works:**

| Feature | Use |
| ------- | --- |
| `axios.create({ baseURL, timeout })` | Shared config |
| Request interceptor | Attach `Authorization`, request IDs |
| Response interceptor | Global 401 → logout, error normalization |
| `axios.isAxiosError(e)` | Type-narrow errors |

**When to use:** Teams wanting centralized auth/refresh; simpler JSON POST APIs than raw `fetch`.

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
| Interceptors, instances | Extra bundle vs fetch |
| Familiar API | Must still layer caching (Query/RTK) |

**Sources:** [axios — docs](https://axios-http.com/docs/intro)

</details>

---

## Error handling

<details>
<summary>Error handling</summary>

**Definition:** **Error handling** maps HTTP and network failures to **typed outcomes** the UI can render — not generic `alert()` or silent failures.

**How it works:**

| Failure | User-facing pattern |
| ------- | ------------------- |
| **4xx** validation | Field errors from `{ errors: { field: msg } }` |
| **401/403** | Redirect login or “no permission” |
| **5xx** / network | Retry button, support id, offline banner |
| **Abort** | Ignore (unmount) — do not set error state |

Discriminated union in hooks:

```ts
type RequestState<T> =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: T }
  | { status: "error"; error: ApiError };
```

**When to use:** Every public API module; align with backend [error contract](./6_web_api_rest.md#rest-principles-and-contract).

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

**Definition:** **Client caching** stores server JSON in memory (keyed by query), dedupes in-flight requests, and **refetches** on focus, interval, or mutation — separate from HTTP `Cache-Control` on CDN.

**How it works:**

| Library | Model | React integration |
| ------- | ----- | ----------------- |
| **TanStack Query** | `useQuery` / `useMutation`, query keys | De facto for new SPAs |
| **RTK Query** | `createApi` + tags in Redux store | When Redux already central |
| **SWR** | `useSWR` stale-while-revalidate | Lightweight widgets |

**When to use:** Default for **server state**. Keep **UI state** local (`useState`) or Redux/Zustand only when shared.

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
| No manual `useEffect` sync | Must learn query keys / invalidation |
| Background refetch | Stale UI if misconfigured `staleTime` |

**Thoughts:** HTTP caching for static GET → [HTTP caching](./6_web_api_http.md). Next.js server cache → [Next.js data](./4_nextjs_data.md).

**Sources:** [TanStack Query](https://tanstack.com/query/latest) · [Ecosystem — Data fetching](./4_react_ecosystem.md#data-fetching) · [SWR](./4_react_ecosystem.md#swr-stale-while-revalidate)

</details>
