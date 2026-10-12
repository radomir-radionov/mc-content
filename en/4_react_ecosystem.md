# React — Ecosystem

Routing, libraries, performance, and legacy.

[← Back to hub](./4_react.md)

### Related docs

- [React — Core](./4_react_core.md) · [React — Hooks](./4_react_hooks.md) · [React — Components](./4_react_components.md) · [React — APIs](./4_react_apis.md) · [React — React 19](./4_react_server.md) · [React — Ecosystem](./4_react_ecosystem.md) · [React — Redux](./4_react_redux.md) · [React — Interview](./4_react_interview.md) · [Architecture patterns](./7_special_architecture.md)

### Sections

- [9. Ecosystem & architecture](#9-ecosystem-architecture)
- [10. Performance](#10-performance)
- [11. Legacy & migration](#11-legacy-migration)

---

## 9. Ecosystem & architecture

Routing

<details>
  <summary>Routing</summary>

### Routing

**Definition:** **Client-side routing** maps URL paths to components **without** a full page reload — the browser history API updates the URL; React swaps the view.

**How it works:**

1. User clicks `<Link to="/users">` or browser back/forward
2. Router matches URL to route config (path, params, nested layouts)
3. Matching components render; others unmount
4. Optional **loader** fetches data before/during render (React Router, Remix pattern)
5. In Next.js App Router, **file system** defines routes (`app/users/page.tsx`)

| Library                | Notes                              |
| ---------------------- | ---------------------------------- |
| **Next.js App Router** | File-based routes + RSC by default |
| **React Router v7**    | SPA + framework mode with SSR/RSC  |
| **TanStack Router**    | Type-safe routes, search params    |

**When to use:** Any multi-page React app. Next.js if you want file-based routing + SSR/RSC; React Router for SPA or framework mode.

**Examples:**

```tsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/users/:id" element={<UserProfile />} />
</Routes>
```

**Thoughts:** _URL drives which components mount — no full document reload._

**See also:** [React Router essentials](#react-router-essentials) · [TanStack Router](./4_react_tanstack.md#tanstack-router)

**Sources:**

[reactrouter.com](https://reactrouter.com/)

React Router essentials

</details>

<details>
  <summary>React Router essentials</summary>

### React Router essentials

**Definition:** **React Router v7** maps URLs to UI, supports nested layouts, data **loaders**, and framework mode with SSR/RSC — the de facto client router for React SPAs.

**How it works:** Routes form a tree — parent routes render an `<Outlet />` where child routes appear. **Loaders** run before navigation/render to fetch data (parallel to route matching); results are available via `useLoaderData()`. **Actions** handle form mutations. In framework mode, file-based routes and SSR integrate like Next.js lite. SPA mode uses `<BrowserRouter>` + `<Routes>`; data APIs use `createBrowserRouter` + `RouterProvider`.

**When to use:** Dashboard with sidebar layout + nested pages, prefetch user on `/users/:id` via loader, form POST via action.

**Pluses and minuses:**

| +                                                | −                                    |
| ------------------------------------------------ | ------------------------------------ |
| Nested layouts without remounting parent         | v6→v7 migration learning curve       |
| Loaders reduce fetch-on-mount waterfalls         | Framework mode overlaps with Next.js |
| Type-safe routes via TanStack Router alternative | Client-only mode lacks RSC           |

**Examples:**

```tsx
const router = createBrowserRouter([
  {
    path: "/",
    element: <Layout />,
    loader: () => fetchRootData(),
    children: [
      { index: true, element: <Home /> },
      {
        path: "users/:userId",
        loader: ({ params }) => fetchUser(params.userId),
        element: <UserProfile />,
      },
    ],
  },
]);

function UserProfile() {
  const user = useLoaderData() as User;
  return <h1>{user.name}</h1>;
}

function Layout() {
  return (
    <>
      <nav />
      <Outlet />
      <footer />
    </>
  );
}
```

**Thoughts:** Prefer loaders over `useEffect` fetch for route-bound data. Split loaders per route so slow child data does not block parent shell.

**Sources:**

[reactrouter.com](https://reactrouter.com/) · [Route loaders](https://reactrouter.com/start/framework/route-module#loader)

Accessibility in React (a11y)

</details>

<details>
  <summary>Accessibility in React (a11y)</summary>

### Accessibility in React (a11y)

**Definition:** Building UIs that work for keyboard, screen reader, and assistive-tech users — React does not change HTML semantics; you must apply **ARIA**, **focus management**, and semantic elements correctly.

**How it works:** Use native interactive elements (`<button>`, `<a href>`, `<input>`) before ARIA widgets. Pair labels with `htmlFor`/`id` or wrap inputs in `<label>`. Use `useId()` for SSR-safe ID links. Manage focus in modals: trap focus inside, restore on close, set `aria-modal="true"`. Announce dynamic updates with `role="alert"` or `aria-live` regions. Radix UI / React Aria provide accessible primitives with keyboard roving tabindex built in.

**When to use:** Form validation messages (`aria-describedby`), icon-only buttons (`aria-label`), comboboxes (use library), skip links, live regions for toast notifications.

**Pluses and minuses:**

| +                                                | −                                      |
| ------------------------------------------------ | -------------------------------------- |
| Semantic HTML covers most cases                  | Custom widgets need full keyboard spec |
| `eslint-plugin-jsx-a11y` catches common mistakes | Portals/modals need manual focus trap  |
| Libraries (Radix) handle complex patterns        | ARIA misuse can harm more than help    |

**Examples:**

```tsx
function Dialog({ title, onClose, children }: DialogProps) {
  const titleId = useId();
  const ref = useRef<HTMLDialogElement>(null);
  useEffect(() => {
    ref.current?.showModal();
  }, []);
  return (
    <dialog ref={ref} aria-labelledby={titleId} onClose={onClose}>
      <h2 id={titleId}>{title}</h2>
      {children}
      <button type="button" onClick={onClose}>
        Close
      </button>
    </dialog>
  );
}
```

**Thoughts:** First rule of ARIA: don't use ARIA if native HTML suffices. Test with keyboard-only navigation and VoiceOver/NVDA.

**Sources:**

[react.dev/learn/accessibility](https://react.dev/learn/accessibility) · [WAI-ARIA APG](https://www.w3.org/WAI/ARIA/apg/)

Data fetching

</details>

<details>
  <summary>Data fetching</summary>

### Data fetching

**Definition:**

Patterns for loading server data in React — framework loaders, client caches, Server Components, and Suspense.

| Approach                     | When                                             |
| ---------------------------- | ------------------------------------------------ |
| **Server Component** `fetch` | Default in Next.js App Router — cached on server |
| **TanStack Query**           | Client SPAs — cache, refetch, optimistic updates |
| **RTK Query**                | Same idea inside Redux Toolkit (`createApi`)     |
| `use()` **+ Suspense**       | Async in component tree with React 19            |
| **Server Actions**           | Mutations from forms in Next.js                  |

**How it works:** Framework `fetch` / RSC cache on the **server**. Client libraries cache **JSON in memory** keyed by endpoint + args. That is **not** HTTP `Cache-Control` (browser/CDN). RTK Query: reference-counted subscriptions; unused data is removed after `keepUnusedDataFor` (default **60** seconds).

**Examples:**

```tsx
// Next.js Server Component
async function Page() {
  const res = await fetch("https://api.example.com/posts", {
    next: { revalidate: 60 },
  });
  const posts = await res.json();
  return <PostList posts={posts} />;
}
```

**Thoughts:** Don't `useEffect` + `fetch` for initial page data if RSC or router loader can provide it. HTTP headers vs client cache → [6_web_api.md — HTTP caching](./6_web_api.md). TanStack Query → [React — TanStack](./4_react_tanstack.md). RTK Query → [React — Redux](./4_react_redux.md).

**Sources:**

[react.dev/reference/rsc/server-components](https://react.dev/reference/rsc/server-components) · [RTK Query cache behavior](https://redux-toolkit.js.org/rtk-query/usage/cache-behavior)

Forms

</details>

<details>
  <summary>Forms</summary>

### Forms

**Definition:** Patterns for collecting and validating user input in React — from native controlled inputs to React 19 Actions and schema-driven libraries.

**How it works:**

| Approach                              | When                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| **Controlled** (`value` + `onChange`) | Full control, small forms                                    |
| **Uncontrolled + Actions**            | Simple submits, React 19, progressive enhancement            |
| **RHF + Zod**                         | Complex validation, field arrays, perf-sensitive large forms |

`<form action={fn}>` — browser collects `FormData`; React calls action with pending state. No manual `e.preventDefault()` for default submit behavior.

**When to use:**

#### Common pitfalls

- Missing field in Zod schema — RHF won't include it in submit data (add as optional)
- Controlled input without `onChange` — read-only field
- Mixing controlled and uncontrolled on same input (`value` + `defaultValue`)

**Examples:**

```tsx
const schema = z.object({ email: z.string().email() });
const { register, handleSubmit } = useForm({ resolver: zodResolver(schema) });
```

**Thoughts:** _Simple forms → Actions; complex validation → RHF + Zod._

**See also:** [Controlled vs uncontrolled inputs](./4_react_core.md#controlled-vs-uncontrolled-inputs) · [Actions](./4_react_server.md#actions) · [useActionState - 19](./4_react_hooks.md#useactionstate-19)

**Sources:**

[react-hook-form.com](https://react-hook-form.com/)

Styling in React

</details>

<details>
  <summary>Styling in React</summary>

### Styling in React

**Definition:**

React has no built-in styling — attach CSS via `className`, CSS Modules, utility frameworks, or CSS-in-JS.

| Approach                                   | Tradeoff                                                                  |
| ------------------------------------------ | ------------------------------------------------------------------------- |
| **Global CSS**                             | Simple; naming collisions at scale                                        |
| **CSS Modules**                            | Scoped classes; zero runtime — `import styles from './Button.module.css'` |
| **Tailwind**                               | Utility-first; fast iteration; pairs well with component libraries        |
| **CSS-in-JS** (styled-components, Emotion) | Colocated styles; runtime cost; poor RSC compatibility                    |
| **Vanilla Extract**                        | Zero-runtime, type-safe CSS files                                         |

**How it works:** Tailwind or CSS Modules for new apps. Runtime CSS-in-JS (Emotion) conflicts with RSC — use Vanilla Extract or Tailwind instead. Inline `style={{}}` for dynamic values only (position from state).

**When to use:** CSS Modules or Tailwind for most new projects. Avoid runtime CSS-in-JS in RSC-heavy apps.

**Examples:**

```tsx
import styles from "./Button.module.css";

export function Button({ children }: { children: React.ReactNode }) {
  return <button className={styles.primary}>{children}</button>;
}
```

**Thoughts:** _Prefer zero-runtime CSS in RSC apps — styles must not depend on client JS injection._

**Sources:**

[react.dev/learn](https://react.dev/learn)

</details>

<details>
  <summary>Feature-Sliced Design (FSD)</summary>

### Feature-Sliced Design (FSD)

**Definition:** Frontend folder methodology: layers (app → pages → widgets → features → entities → shared) plus slices with a public `index.ts`.

Canonical: [Feature-Sliced Design](./7_special_architecture.md#feature-sliced-design-fsd). React-specific: keep server-state libraries out of `entities` models — [State at scale](./7_special_architecture.md#state-at-scale).

**Sources:** [feature-sliced.design](https://feature-sliced.design/)

</details>

<details>
  <summary>TanStack Router</summary>

### TanStack Router

**Definition:** Fully type-safe React router with loaders and validated search params.

Full guide → [React — TanStack](./4_react_tanstack.md#tanstack-router).

**See also:** [Routing](#routing) · [nuqs (URL search params)](#nuqs-url-search-params)

**Sources:**

[tanstack.com/router](https://tanstack.com/router)

nuqs (URL search params)

</details>

<details>
  <summary>nuqs (URL search params)</summary>

### nuqs (URL search params)

**Definition:** Type-safe search-param state for React: `useQueryState` works like `useState`, but the source of truth is the URL query string. `qs` is a generic query-string parser; nuqs is the React/Next-oriented replacement for filter/pagination state.

**How it works:** Wrap the app with `NuqsAdapter` (Next.js App Router: `nuqs/adapters/next/app`). In client components, `useQueryState('name')` reads/writes `?name=`. Parsers (`parseAsInteger`, `parseAsStringLiteral`, …) coerce and default values. Share one parser map: `useQueryStates` on the client, `createLoader` / `nuqs/server` in Server Components. `setState(null)` removes the key.

**When to use:**

Faceted search, pagination, tabs in the URL. Skip when [TanStack Router](./4_react_tanstack.md#tanstack-router) `validateSearch` already owns the query string, or when the param is a one-off `?ref=` you never write from UI.

#### Common pitfalls

Importing parsers from `nuqs` in Server Components — use `nuqs/server`. Forgetting `NuqsAdapter`. Treating nuqs as a cache for server data (use Query for that).

**Pluses and minuses:**

| +                                              | −                                                                               |
| ---------------------------------------------- | ------------------------------------------------------------------------------- |
| Shareable, bookmarkable filter state           | Client hook only — wrap with adapter                                            |
| Typed parsers instead of raw `URLSearchParams` | Next.js App Router already has `useSearchParams`; nuqs adds typing and batching |
| Same parsers on server and client              | Extra library if you only need one string param                                 |

**Examples:**

```tsx
// app/layout.tsx
import { NuqsAdapter } from "nuqs/adapters/next/app";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html>
      <body>
        <NuqsAdapter>{children}</NuqsAdapter>
      </body>
    </html>
  );
}

// client
("use client");
import { parseAsInteger, useQueryState } from "nuqs";

function Pager() {
  const [page, setPage] = useQueryState("page", parseAsInteger.withDefault(1));
  return <button onClick={() => setPage((p) => p + 1)}>Page {page}</button>;
}
```

**Thoughts:** URL is state. Parse it; do not hand-roll `%5B%5D` encoding.

**Sources:**

[nuqs.dev](https://nuqs.dev/) · [github.com/47ng/nuqs](https://github.com/47ng/nuqs)

CASL (authorization)

</details>

<details>
  <summary>CASL (authorization)</summary>

### CASL (authorization)

**Definition:** Isomorphic authorization library: you define **abilities** (`can` / `cannot`) for actions on subjects, then check them in UI and on the server. Supports RBAC (roles → rules) and ABAC (rules on attributes).

**How it works:** Build an `Ability` with `AbilityBuilder` / `defineAbility`. `@casl/react` exposes `AbilityProvider`, `<Can I="update" this={article}>`, and `useAbility()`. Subject type can be a string (`'Article'`) or a class with `detectSubjectType`. Persist rules from the backend; never trust UI-only checks.

**When to use:**

Admin panels, “edit own post”, feature flags that are really permissions. Pair with NestJS guards or Next.js Server Actions that re-check `ability.can(...)`.

#### Common pitfalls

Hiding a button without repeating the check in the mutation. Using CASL as authentication (it is **authz**, not login).

**Pluses and minuses:**

| +                                      | −                                           |
| -------------------------------------- | ------------------------------------------- |
| Same rules on client and API           | Easy to forget server-side enforcement      |
| Declarative `<Can>` keeps JSX readable | Complex ABAC conditions need tests          |
| TypeScript `MongoAbility` tuples       | Mongo-style conditions are a learning curve |

**Examples:**

```tsx
import { defineAbility } from "@casl/ability";
import { Can } from "@casl/react";

const ability = defineAbility((can, cannot) => {
  can("read", "Article");
  can("update", "Article", { authorId: user.id });
  cannot("delete", "Article");
});

function Editor({ article }: { article: Article }) {
  return (
    <Can I="update" this={article}>
      <button type="button">Edit</button>
    </Can>
  );
}
```

**Thoughts:** UI can hide; the server must deny.

**Sources:**

[casl.js.org](https://casl.js.org/) · [@casl/react](https://github.com/stalniy/casl/tree/master/packages/casl-react)

orval (OpenAPI codegen)

</details>

<details>
  <summary>orval (OpenAPI codegen)</summary>

### orval (OpenAPI codegen)

**Definition:** Generates TypeScript clients, models, and optional mocks from an OpenAPI/Swagger spec — typically [TanStack Query](./4_react_tanstack.md#codegen-orval) hooks keyed to operations.

**How it works:** Point `orval.config.ts` at a YAML/JSON spec; set `output.client: 'react-query'`. Orval emits query functions, query keys, and `useX` hooks. Use a **mutator** for a shared Axios/`fetch` instance (auth headers). Re-run on spec change in CI so frontend types cannot drift from the contract.

**When to use:**

Backend-first teams with OpenAPI; large REST surfaces. Skip for one or two endpoints.

#### Common pitfalls

Committing generated files that nobody regenerates. Editing generated files by hand.

**Pluses and minuses:**

| +                                          | −                                              |
| ------------------------------------------ | ---------------------------------------------- |
| Spec is the source of truth                | Garbage spec → garbage hooks                   |
| Query keys + invalidation can be generated | Another build step to keep green               |
| Optional MSW mocks from the same spec      | Custom endpoints still need hand-written hooks |

**Examples:**

```ts
import { defineConfig } from "orval";

export default defineConfig({
  petstore: {
    input: { target: "./openapi.yaml" },
    output: {
      mode: "tags-split",
      target: "src/api/petstore.ts",
      schemas: "src/api/model",
      client: "react-query",
      override: {
        mutator: { path: "./src/api/mutator.ts", name: "customInstance" },
      },
    },
  },
});
```

**Thoughts:** Generate from the contract; do not hand-type DTOs that already live in OpenAPI.

**Sources:**

[orval.dev](https://orval.dev/)

shadcn/ui

</details>

<details>
  <summary>shadcn/ui</summary>

### shadcn/ui

**Definition:** Not an npm component library you import as a black box — a CLI that **copies** accessible primitives (Radix + Tailwind) into _your_ repo so you own the source.

**How it works:** `npx shadcn@latest init` writes `components.json`. `npx shadcn@latest add button` drops `components/ui/button.tsx` into the project. You edit that file like any other component. Copy-paste without the CLI also works; `components.json` is only required for the CLI.

**When to use:**

Product UI that will diverge from a theme. **When NOT:** you need a locked third-party kit with semver upgrades only.

#### Common pitfalls

Calling it “a library we depend on” in interviews — interviewers want “we own the code.” Wrapping every primitive until nothing is customizable.

**Pluses and minuses:**

| +                                     | −                                                |
| ------------------------------------- | ------------------------------------------------ |
| Full ownership and customization      | You maintain the copies (a11y + updates)         |
| Works with RSC (no runtime CSS-in-JS) | Not a versioned design-system package by default |
| Accessible primitives (Radix)         | Team must agree on when to pull upstream diffs   |

**Examples:**

```bash
npx shadcn@latest add button
```

```tsx
import { Button } from "@/components/ui/button";

export function SaveBar() {
  return <Button type="submit">Save</Button>;
}
```

**Thoughts:** Copy, own, customize — it is a distribution model, not a runtime dependency.

**Sources:**

[ui.shadcn.com](https://ui.shadcn.com/)

Charts (Nivo / amCharts)

</details>

<details>
  <summary>Charts (Nivo / amCharts)</summary>

### Charts (Nivo / amCharts)

**Definition:** React charting layers over SVG/Canvas. **Nivo** is open-source, React-first, D3 under the hood. **amCharts** is a commercial-grade suite (JS + React wrapper) used when product needs polished maps/stock charts.

**How it works:** You pass data + keys; the library handles scales, axes, and animation. Keep charts in **Client Components** (`"use client"`) — they need DOM and often `ResizeObserver`. For RSC apps, fetch data on the server and pass a serializable array into the chart leaf.

**When to use:** Dashboards, analytics. Prefer **lazy** `React.lazy` / `next/dynamic` so the chart chunk is not on the landing page.

**Pluses and minuses:**

| +                                             | −                                                     |
| --------------------------------------------- | ----------------------------------------------------- |
| Nivo: composable, MIT, good defaults          | Bundle size — lazy-load chart routes                  |
| amCharts: maps, Gantt, stock, licensed polish | License cost; heavier API                             |
| Declarative React API                         | Accessibility of SVG charts needs titles/`aria-label` |

**Examples:**

```tsx
"use client";
import { ResponsiveBar } from "@nivo/bar";

export function RevenueBar({
  data,
}: {
  data: { month: string; value: number }[];
}) {
  return (
    <div style={{ height: 320 }} role="img" aria-label="Revenue by month">
      <ResponsiveBar data={data} keys={["value"]} indexBy="month" />
    </div>
  );
}
```

**Thoughts:** Charts are client leaves. Data fetching can stay on the server.

**Sources:**

[nivo.rocks](https://nivo.rocks/) · [amcharts.com](https://www.amcharts.com/)

i18n (internationalization)

</details>

<details>
  <summary>i18n (internationalization)</summary>

### i18n (internationalization)

**Definition:** Serving UI strings, dates, and numbers per locale — plus optional locale-prefixed routing (`/en/about`, `/de/ueber-uns`).

**How it works:** Message catalogs (JSON/ICU) keyed by namespace. In Next.js App Router, **next-intl** loads messages in `i18n/request.ts` via `getRequestConfig`, wraps the tree with `NextIntlClientProvider`, and uses `useTranslations('HomePage')` in components. Format dates with `Intl` (see [3_js_core.md](./3_js_core.md)) rather than concatenating strings.

**When to use:**

Multi-language product, SEO per locale. Skip if the product is single-locale forever.

#### Common pitfalls

Hard-coded UI strings. Splitting sentences across keys (`t('hello') + name + t('excl')`) — use ICU `{name}` instead. Lazy-loading locale JSON for unused languages on the client.

**Pluses and minuses:**

| +                                                                          | −                                                                                     |
| -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| ICU placeholders avoid broken grammar                                      | Routing + middleware/proxy must stay in sync                                          |
| Server Components can translate without shipping all locales to the client | Missing keys fail at runtime unless typed                                             |
| `next-intl` plugin wires `next.config`                                     | Next.js 16 `proxy.ts` is Node-only — Edge i18n middleware may stay as `middleware.ts` |

**Examples:**

```tsx
import { useTranslations } from "next-intl";

export default function HomePage() {
  const t = useTranslations("HomePage");
  return <h1>{t("title")}</h1>;
}
```

```ts
// i18n/request.ts
import { getRequestConfig } from "next-intl/server";

export default getRequestConfig(async () => {
  const locale = "en";
  return {
    locale,
    messages: (await import(`../messages/${locale}.json`)).default,
  };
});
```

**Thoughts:** Translate messages and format with `Intl`; do not concatenate locale grammar.

**Sources:**

[next-intl.dev](https://next-intl.dev/) · [MDN Intl](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl)

Testing

</details>

<details>
  <summary>Testing</summary>

### Testing

**Definition:** Test **behavior users see** — roles, labels, and visible output — not implementation details.

**How it works:** Render → query by role/label → `userEvent` → assert on screen. Full pyramid, AAA, RTL patterns, MSW, Vitest/Jest, and coverage live in the **Testing** track.

**When to use:** Component and page behavior in jsdom; full user flows → E2E in the Testing track.

**Examples:**

```tsx
render(<LoginForm />);
await userEvent.type(screen.getByLabelText(/email/i), "ada@example.com");
await userEvent.click(screen.getByRole("button", { name: /sign in/i }));
expect(await screen.findByText(/welcome/i)).toBeInTheDocument();
```

**Thoughts:** Do not duplicate the Testing handbook here — link to it for runners, doubles, and layers.

**See also:** [Testing — React Testing Library](./7_special_testing_tooling.md#react-testing-library) · [Vitest vs Jest](./7_special_testing_tooling.md#vitest-vs-jest) · [MSW](./7_special_testing_tooling.md#msw-mock-service-worker) · [Integration](./7_special_testing_layers.md#integration-testing) · [E2E](./7_special_testing_layers.md#e2e-testing)

**Sources:** [Testing Library — React intro](https://testing-library.com/docs/react-testing-library/intro/)

---

</details>

## 10. Performance

Diagnosing re-renders

<details>
  <summary>Diagnosing re-renders</summary>

### Diagnosing re-renders

**Definition:** A **re-render** is React calling your component function again. Not every re-render is a problem — only fix when Profiler shows measurable cost.

**How it works:**

| Cause                                | Why child re-renders                              |
| ------------------------------------ | ------------------------------------------------- |
| Parent state changed                 | Default — children re-render unless memoized      |
| Context value changed                | All consumers re-render                           |
| New object/function prop each render | `style={{}}`, `onClick={() => ...}` breaks `memo` |
| Hook deps changed                    | `useContext`, custom hooks                        |

React DevTools **Profiler** — record interaction → "why did this render?" Highlight updates toggles flash re-renders in dev.

1. **Move state down** — only leaves that need it hold state
2. **Split context** — theme vs user vs dispatch
3. **Memoize context value** — `useMemo` on provider `value`
4. `React.memo` **/ Compiler** on expensive pure children
5. **Virtualize** long lists — [TanStack Virtual](./4_react_tanstack.md#tanstack-virtual) (`@tanstack/react-virtual`)

**Examples:**

```tsx
// New object/function every render → memo child still re-renders
<Item style={{ color }} onClick={() => save(id)} />;

// Stabilize when Profiler shows cost
const style = useMemo(() => ({ color }), [color]);
const onClick = useCallback(() => save(id), [id]);
```

**Thoughts:** _Profile first — most re-renders are cheap; fix architecture before sprinkling_ `memo`_._

**See also:** [memo / useMemo / useCallback vs Compiler](#memo--usememo--usecallback-vs-compiler) · [Render pipeline](./4_react_core.md#render-pipeline)

**Sources:**

[react.dev/learn/react-developer-tools](https://react.dev/learn/react-developer-tools)

memo / useMemo / useCallback vs Compiler

</details>

<details>
  <summary>memo / useMemo / useCallback vs Compiler</summary>

### memo / useMemo / useCallback vs Compiler

**Definition:** Manual memoization (`memo`, `useMemo`, `useCallback`) skips work when deps are unchanged. **React Compiler** does this automatically at build time.

**How it works:**

| Tool          | What it caches                              |
| ------------- | ------------------------------------------- |
| `React.memo`  | Whole component if props shallow-equal      |
| `useMemo`     | Expensive computed value                    |
| `useCallback` | Function reference for stable child deps    |
| **Compiler**  | All of the above where analysis proves safe |

No Compiler yet; proven bottleneck in Profiler; third-party `memo` children need stable props.

**When to use:**

Premature optimization; mutating props in place (memo won't help).

| Without Compiler                 | With React Compiler                  |
| -------------------------------- | ------------------------------------ |
| Manual `memo` on list items      | Compiler auto-skips if props stable  |
| `useCallback` for child handlers | Usually unnecessary                  |
| Profile before memoizing         | Still profile — Compiler isn't magic |

**Examples:**

```tsx
const Row = React.memo(function Row({ item }: { item: Item }) {
  return <div>{item.name}</div>;
});
```

**Thoughts:** _Compiler replaces manual memo for most apps — profile, don't guess._

**See also:** [React Compiler](./4_react_server.md#react-compiler) · [useMemo](./4_react_hooks.md#usememo) · [useCallback](./4_react_hooks.md#usecallback)

**Sources:**

[react.dev/learn/react-compiler](https://react.dev/learn/react-compiler)

Code splitting

</details>

<details>
  <summary>Code splitting</summary>

### Code splitting

**Definition:** Split JavaScript into **multiple chunks** so users download only code needed for the current route or interaction — not the entire app upfront.

**How it works:** Bundlers (Vite, webpack, Rollup) create separate files per dynamic `import()`. `React.lazy` triggers a chunk load on first render; Suspense shows fallback until it arrives. Route-level splitting maps one chunk per route. Component-level splitting defers heavy libs (charts, editors). Event-handler splitting loads code on click (`import()` inside handler). Frameworks auto-split per route in file-based routers.

**When to use:** Admin panel, PDF viewer, i18n locale files, rarely used settings screens.

**Pluses and minuses:**

| +                                  | −                                          |
| ---------------------------------- | ------------------------------------------ |
| Smaller initial bundle, faster TTI | More HTTP requests (mitigated by HTTP/2)   |
| Pay-for-what-you-use per route     | Waterfall if chunks chain without prefetch |
| Easy with `lazy` + dynamic import  | Load failures need error boundaries        |

**Examples:**

```tsx
// Route-level
const Settings = lazy(() => import("./Settings"));

// Interaction-level
async function exportPdf() {
  const { generatePdf } = await import("./pdf");
  generatePdf(data);
}
```

**Thoughts:** Analyze bundles with `rollup-plugin-visualizer` or Next.js bundle analyzer. Prefetch routes on hover/link visibility when latency matters.

**Sources:**

[react.dev/reference/react/lazy](https://react.dev/reference/react/lazy)

List virtualization

</details>

<details>
  <summary>List virtualization</summary>

### List virtualization

**Definition:** Render only **visible rows** (plus a small overscan buffer) in long scrollable lists — DOM node count stays constant regardless of list size.

**How it works:** A virtualizer calculates which items intersect the scroll viewport from `scrollTop`, container height, and fixed/variable row heights. Only those indices mount as DOM nodes; off-screen items are not in the document. On scroll, the virtualizer updates the visible window and repositions rows with `transform` or absolute `top` offsets. Libraries: [@tanstack/react-virtual](./4_react_tanstack.md#tanstack-virtual), `react-window`, `react-virtuoso`.

**When to use:** Data tables with thousands of rows, infinite feeds, chat message history, log viewers. Threshold: consider when **1000+ DOM nodes** in one scroll container cause jank.

**Pluses and minuses:**

| +                                   | −                                                              |
| ----------------------------------- | -------------------------------------------------------------- |
| 10k+ rows without browser slowdown  | Fixed row height is simpler; variable height needs measurement |
| Constant memory and paint cost      | Breaks native find-in-page for off-screen rows                 |
| Essential for data grids, chat logs | Keyboard navigation needs extra a11y work                      |

**Examples:**

```tsx
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

**See also:** [TanStack Virtual](./4_react_tanstack.md#tanstack-virtual)

**Sources:**

[tanstack.com/virtual](https://tanstack.com/virtual) · [react-window](https://react-window.vercel.app/)

Web Vitals in React apps

</details>

<details>
  <summary>Web Vitals in React apps</summary>

### Web Vitals in React apps

**Definition:** Core performance metrics (LCP, INP, CLS) applied to React apps — bundle size, hydration cost, and layout stability directly affect scores.

**How it works:**

| Metric  | React lever                                                                                             |
| ------- | ------------------------------------------------------------------------------------------------------- |
| **LCP** | Smaller bundle (`lazy`), RSC for static content, SSR/streaming for fast HTML                            |
| **INP** | `useTransition` for heavy updates, avoid long sync work on click handler, code-split                    |
| **CLS** | Match skeleton dimensions to final content; reserve image space; avoid inserting above existing content |

- Analyze bundle: `rollup-plugin-visualizer` / Next.js analyzer
- Prefer RSC for static, non-interactive content
- Defer non-critical JS with `lazy` + `Suspense`
- Avoid layout thrashing in `useLayoutEffect`
- INP: granular `<Suspense>`, React 19.2 `<Activity mode="hidden">` → [6_web_api.md — INP](./1_html_performance.md#inp-interaction-to-next-paint)
- CLS causes in HTML/CSS → [1_html.md — CLS](./1_html_performance.md#cls-cumulative-layout-shift)

**Examples:**

Field RUM — mount once after the app shell (Vite/CRA). Flush on `visibilitychange` → `hidden` via `sendBeacon` (`unload` breaks **bfcache**). Full pattern → [6_web_api.md — Core Web Vitals](./1_html_performance.md#core-web-vitals).

```tsx
import { useEffect } from "react";
import { onCLS, onINP, onLCP } from "web-vitals";

export function WebVitalsReporter() {
  useEffect(() => {
    const send = (metric) =>
      navigator.sendBeacon("/analytics", JSON.stringify(metric));
    onCLS(send);
    onINP(send);
    onLCP(send);
  }, []);
  return null;
}
```

```tsx
// main.tsx — render <WebVitalsReporter /> next to <App />
```

Next.js — keep `'use client'` on a tiny reporter; import from `app/layout.tsx` → [4_nextjs.md — CWV](./4_nextjs_optimizations.md#core-web-vitals-on-vercel).

**LCP** — smaller initial JS; defer below-fold UI:

```tsx
import { lazy, Suspense } from "react";

const Dashboard = lazy(() => import("./Dashboard"));

export function App() {
  return (
    <Suspense fallback={<div style={{ minHeight: 400 }} />}>
      <Dashboard />
    </Suspense>
  );
}
```

**INP** — mark heavy updates non-urgent so clicks paint sooner:

```tsx
import { useState, useTransition } from "react";

const [filter, setFilter] = useState("");
const [isPending, startTransition] = useTransition();

const onChange = (e: React.ChangeEvent<HTMLInputElement>) =>
  startTransition(() => setFilter(e.target.value));
```

**CLS** — skeleton matches final hero dimensions:

```tsx
{
  hero ? (
    <img src={hero.url} width={1200} height={630} alt="" fetchPriority="high" />
  ) : (
    <div style={{ width: 1200, height: 630 }} aria-hidden />
  );
}
```

Lab bundle size:

```bash
npm run build && npx @next/bundle-analyzer
```

**Thoughts:** _Vitals measure user-perceived speed — optimize bundle, hydration, and main-thread work. Ship field RUM first; use Lighthouse only to reproduce a bad p75._

**Sources:**

[web.dev/articles/vitals](https://web.dev/articles/vitals) · [web-vitals](https://github.com/GoogleChrome/web-vitals) · [react.dev — Activity](https://react.dev/reference/react/Activity)

---

</details>

## 11. Legacy & migration

Class components (legacy)

<details>
  <summary>Class components (legacy)</summary>

### Class components (legacy)

**Definition:**

ES6 classes extending `React.Component` with `render()` method. Still supported but not recommended for new code.

| Class lifecycle                | Hooks equivalent               |
| ------------------------------ | ------------------------------ |
| `constructor`                  | `useState` initial value       |
| `componentDidMount`            | `useEffect(() => {}, [])`      |
| `componentDidUpdate`           | `useEffect(() => {}, [deps])`  |
| `componentWillUnmount`         | `useEffect` cleanup            |
| `shouldComponentUpdate`        | `React.memo` + immutable props |
| `this.state` / `this.setState` | `useState` / `useReducer`      |

**How it works:**

- Error boundaries (still class-only)
- `this.forceUpdate()` (anti-pattern — use `key` remount)

**Examples:**

```tsx
class Counter extends React.Component {
  state = { count: 0 };
  render() {
    return (
      <button onClick={() => this.setState((s) => ({ count: s.count + 1 }))}>
        {this.state.count}
      </button>
    );
  }
}
```

**Sources:**

[react.dev/reference/react/Component](https://react.dev/reference/react/Component)

PureComponent

</details>

<details>
  <summary>PureComponent</summary>

### PureComponent

**Definition:** Legacy class component base (`extends React.PureComponent`) that **skips re-render** when props and state are **shallow-equal** to the previous render.

**How it works:** `PureComponent` implements `shouldComponentUpdate` with shallow comparison — same as `React.memo` for function components. Compares each prop/state field with `Object.is`. Nested object changes inside the same reference do **not** trigger re-render (same pitfall as `memo`). Modern equivalent: `React.memo(Component)` or automatic memoization via React Compiler.

**When to use:** Legacy class list items with primitive props, migration reference when reading old code. **New code:** `memo` + immutable updates, or React Compiler.

**Pluses and minuses:**

| +                                 | −                                     |
| --------------------------------- | ------------------------------------- |
| Cheap skip for stable props       | Shallow compare misses deep mutations |
| Familiar in legacy codebases      | Class-only — not for new code         |
| Reduces unnecessary child renders | Manual — Compiler may replace need    |

**Examples:**

```tsx
// Legacy class
class Item extends React.PureComponent<{ name: string }> {
  render() {
    return <li>{this.props.name}</li>;
  }
}

// Modern equivalent
const MemoizedItem = React.memo(function Item({ name }: { name: string }) {
  return <li>{name}</li>;
});
```

**Thoughts:** Shallow equality means `{ user: { name: 'Ada' } }` with mutated `user.name` won't re-render — always treat props as immutable.

**Sources:**

[react.dev/reference/react/PureComponent](https://react.dev/reference/react/PureComponent) · [memo](https://react.dev/reference/react/memo)

Create React App

</details>

<details>
  <summary>Create React App</summary>

### Create React App

**Definition:** Former official **zero-config** React scaffold (`create-react-app`) — bundled webpack, Babel, and a single-page app template without ejecting.

**How it works:** `npx create-react-app my-app` generated a project with `react-scripts` hiding webpack config. Ejecting exposed config irreversibly. CRA supported client-only SPA — no SSR, no file-based routing, slow dev builds at scale. React team **deprecated CRA** (Feb 2025) in favor of Vite and meta-frameworks.

**When to use:** Maintaining existing CRA apps (migrate when feasible). **New projects:** Vite, Next.js, or Expo.

**Pluses and minuses:**

| +                                        | −                                  |
| ---------------------------------------- | ---------------------------------- |
| Zero config for beginners (historically) | Deprecated — no longer recommended |
| Familiar to many developers              | Slow webpack dev server vs Vite    |
|                                          | No SSR/RSC out of the box          |
|                                          | Hard to customize without eject    |

**Examples:**

```bash
# Deprecated — do not start new projects
# npx create-react-app my-app

# Recommended replacements
npm create vite@latest my-app -- --template react-ts
npx create-next-app@latest my-app
```

**Thoughts:** Migration path: Vite for SPA (`@vitejs/plugin-react`), Next.js for full-stack. See [sunsetting CRA blog post](https://react.dev/blog/2025/02/14/sunsetting-create-react-app).

**Sources:**

[react.dev/blog/2025/02/14/sunsetting-create-react-app](https://react.dev/blog/2025/02/14/sunsetting-create-react-app)

forwardRef → ref prop

</details>

<details>
  <summary>forwardRef → ref prop</summary>

### forwardRef → ref prop

**Definition:** React 19 lets function components accept `ref` as a **normal prop** instead of wrapping with `forwardRef`.

**How it works:**

Declare `ref` in props type, pass to DOM element or child. `forwardRef` still works for backward compatibility — migrate when touching code. TypeScript: `ref?: Ref<HTMLInputElement>` or `ComponentPropsWithRef<'input'>`.

Design system inputs, wrappers around native elements, any component parents need to focus/measure.

**Examples:**

```tsx
// Before (React 18)
const Input = forwardRef<HTMLInputElement, Props>((props, ref) => (
  <input ref={ref} {...props} />
));

// After (React 19)
function Input({ ref, ...props }: Props & { ref?: Ref<HTMLInputElement> }) {
  return <input ref={ref} {...props} />;
}
```

**Thoughts:** `ref` _is just a prop now — no HOC wrapper._

**See also:** [ref as prop - 19](./4_react_hooks.md#ref-as-prop-19)

**Sources:**

[react.dev/blog/2024/12/05/react-19](https://react.dev/blog/2024/12/05/react-19)

Migration checklist

</details>

<details>
  <summary>Migration checklist</summary>

### Migration checklist

**Definition:** Step-by-step upgrades when modernizing legacy React codebases to React 18/19 patterns.

**How it works:**

| Step | From                       | To                                    |
| ---- | -------------------------- | ------------------------------------- |
| 1    | `ReactDOM.render`          | `createRoot` / `hydrateRoot`          |
| 2    | Class components           | Function + hooks                      |
| 3    | `UNSAFE_*` lifecycles      | `useEffect` with cleanup              |
| 4    | `forwardRef` wrappers      | `ref` as prop (React 19)              |
| 5    | Manual `isLoading` forms   | Actions + `useActionState`            |
| 6    | `useEffect` fetch on mount | Router loaders / RSC / TanStack Query |
| 7    | Manual `memo` everywhere   | Evaluate React Compiler               |
| 8    | CRA                        | Vite (SPA) or Next.js (full-stack)    |

**Examples:**

```tsx
// React 18+ root API
import { createRoot } from "react-dom/client";
createRoot(document.getElementById("root")!).render(<App />);
```

**Thoughts:** _Upgrade runtime first, then patterns — one breaking change at a time._

**Sources:**

[react.dev/blog/2022/03/08/react-18-upgrade-guide](https://react.dev/blog/2022/03/08/react-18-upgrade-guide)

---

</details>
