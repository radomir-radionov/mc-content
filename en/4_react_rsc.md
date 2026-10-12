# React — React Server Components

Server Components, Server Functions, and RSC directives in framework-backed apps.

[← Back to hub](./4_react.md)

### Related docs

- [React — APIs](./4_react_apis.md) · [React 19](./4_react_server.md) · [Next.js — Rendering](./4_nextjs_rendering.md)

### Sections

- [Server Components](#server-components)
- [Server Functions](#server-functions)
- [Directives](#directives)

---

## Server Components

<details>
  <summary>Server Components</summary>

### Server Components

**Definition:** React components that run **only on the server**. They can be `async`, fetch data directly, and ship **zero JavaScript** to the client for their own logic.

**How it works:**

- No `useState`, `useEffect`, or event handlers (`onClick`, etc.)
- No browser APIs (`window`, `localStorage`)
- May `await` data on the server and import **Client Components** as leaves
- Client Components **cannot** import Server Components directly — pass server output as `children` or serializable props

**When to use:** Static content, layouts, data fetching, non-interactive UI. Keep interactivity in Client Components at the leaves.

**Pluses and minuses:**

| +                       | −                                               |
| ----------------------- | ----------------------------------------------- |
| Smaller client bundles  | No interactivity in the same component          |
| Direct DB/API access    | Requires a framework with RSC support           |
| Streaming with Suspense | Caching and invalidation are framework-specific |

**Examples:**

```tsx
// app/posts/page.tsx — Server Component by default (Next.js App Router)
async function PostsPage() {
  const posts = await db.posts.findAll();
  return (
    <ul>
      {posts.map((p) => (
        <li key={p.id}>
          <LikeButton postId={p.id} />
          {p.title}
        </li>
      ))}
    </ul>
  );
}
```

**See also:** ['use client'](#use-client) · [SPA vs SSR vs RSC](./4_react_server.md#spa-vs-ssr-vs-rsc) · [Hydration](./4_react_server.md#hydration)

**Sources:**

[react.dev/reference/rsc/server-components](https://react.dev/reference/rsc/server-components)

Server Functions

</details>

<details>
  <summary>Server Functions</summary>

### Server Functions

**Definition:** Functions marked with the **`'use server'`** directive that execute on the server. The bundler exposes a **server reference** to the client so calls become network requests with a serializable payload.

**How it works:**

1. **Module directive** — top of a file: `'use server'` exports functions the client can import
2. **Inline directive** — inside a Server Component: `async function fn() { 'use server'; ... }` passed as props (e.g. `onClick`, `action`)
3. Client receives a server reference — not the function body
4. Invoked from `<form action={fn}>`, handlers in `startTransition`, or `await fn()` when allowed

**When to use:** Mutations, form posts, server-only logic triggered from UI — pair with React 19 Actions when possible.

**Examples:**

```js
"use server";

export async function createNote() {
  await db.notes.create();
}
```

**See also:** ['use server'](#use-server) · [Actions](./4_react_server.md#actions)

**Sources:**

[react.dev/reference/rsc/server-functions](https://react.dev/reference/rsc/server-functions)

Directives

</details>

### Directives

<details>
  <summary>Directives</summary>

### Directives

**Definition:** String literals **`'use client'`** and **`'use server'`** mark module and function boundaries so the bundler splits client and server code in RSC apps.

**How it works:**

| Directive      | Scope                            | Effect                                          |
| -------------- | -------------------------------- | ----------------------------------------------- |
| `'use client'` | Top of a module                  | File becomes a Client Component bundle boundary |
| `'use server'` | Top of module or inside function | Exports or body run only on the server          |

Server Components are the **default** in frameworks like the Next.js App Router.

**See also:** ['use client'](#use-client) · ['use server'](#use-server)

**Sources:**

[react.dev/reference/rsc/use-client](https://react.dev/reference/rsc/use-client) · [react.dev/reference/rsc/use-server](https://react.dev/reference/rsc/use-server)

'use client'

</details>

<details>
  <summary>'use client'</summary>

### 'use client'

**Definition:** A module directive that marks **Client Components** — code that hydrates in the browser and may use hooks, state, and event handlers.

**How it works:** Place at the top of the module. Server Components may import Client Components; Client Components **cannot** import Server Components — pass server output as `children` or props.

**When to use:** Buttons, forms, `useEffect`, browser APIs, client-only libraries. Push the boundary to **leaves**.

**Examples:**

```tsx
"use client";

import { useState } from "react";

export function LikeButton({ postId }: { postId: string }) {
  const [liked, setLiked] = useState(false);
  return <button onClick={() => setLiked(!liked)}>Like</button>;
}
```

**Sources:**

[react.dev/reference/rsc/use-client](https://react.dev/reference/rsc/use-client)

'use server'

</details>

<details>
  <summary>'use server'</summary>

### 'use server'

**Definition:** A directive that marks a module or function as a **Server Function** — server-only code callable from the client via a server reference.

**How it works:** File-level `'use server'` exports server references; inline `'use server'` inside a Server Component creates a function passed to client children. Arguments and return values must be **serializable**.

**When to use:** Mutations, form handlers, privileged server logic.

**Examples:**

```js
"use server";

export async function incrementLike() {
  return db.likes.increment();
}
```

**See also:** [Server Functions](#server-functions)

**Sources:**

[react.dev/reference/rsc/use-server](https://react.dev/reference/rsc/use-server)

</details>
