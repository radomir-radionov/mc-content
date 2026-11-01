# React — Architecture

How to **structure** React codebases and UI: design-system layers, folder methodologies, and team boundaries.

[← Back to hub](./4_react.md)

### Related docs

[React — hub](./4_react.md) · [Patterns — composition](./4_react_patterns.md) · [Ecosystem — routing & libraries](./4_react_ecosystem.md) · [React 19 & SSR](./4_react_server.md) · [Architecture patterns (canonical)](./7_special_architecture.md)

**Not here:** **Routing** (React Router, URL loaders) → [Ecosystem — Client routing](./4_react_ecosystem.md#client-routing). **Rendering** (SPA vs SSR vs RSC) → [React 19 — SPA vs SSR vs RSC](./4_react_server.md#spa-vs-ssr-vs-rsc).

### Main concepts

| Topic                                                                   | Covers                                            |
| ----------------------------------------------------------------------- | ------------------------------------------------- |
| [Architecture overview](#architecture-overview)                         | Picking a structural model                        |
| [Atomic Design](#atomic-design)                                         | Atoms → molecules → organisms → templates → pages |
| [Feature-Sliced Design (FSD)](#feature-sliced-design-fsd)               | Folder layers and import rules                    |
| [Container / Presentational](#container--presentational)                | Smart vs dumb components                          |
| [Modular monolith (feature folders)](#modular-monolith-feature-folders) | Lightweight `features/` layout                    |
| [Vertical slice architecture](#vertical-slice-architecture)             | End-to-end slice per use case                     |
| [Feature-Sliced Design (FSD)](#feature-sliced-design-fsd)               | Folder layers and import rules                    |
| [Bulletproof React](#bulletproof-react)                                 | Opinionated `src/` tree (features-first)          |
| [Screaming Architecture](#screaming-architecture)                       | Folders named by domain, not by file type         |
| [Micro-frontends (MFE)](#micro-frontends-mfe)                           | Team boundaries, Module Federation                |
| [Monorepo packages](#monorepo-packages)                                 | Apps + shared packages in one repo                |

### Sections

- [Architecture overview](#architecture-overview)
- [Atomic Design](#atomic-design)
- [Container / Presentational](#container--presentational)
- [Modular monolith (feature folders)](#modular-monolith-feature-folders)
- [Vertical slice architecture](#vertical-slice-architecture)
- [Feature-Sliced Design (FSD)](#feature-sliced-design-fsd)
- [Bulletproof React](#bulletproof-react)
- [Screaming Architecture](#screaming-architecture)
- [Micro-frontends (MFE)](#micro-frontends-mfe)
- [Monorepo packages](#monorepo-packages)

---

## Architecture overview

<details>
<summary>Architecture overview</summary>

**Definition:** **Frontend architecture** (in this topic) means how you **organize components and folders** and **draw team boundaries** — not which router or SSR mode you use.

**How it works:**

| Model                                  | Unit of organization                        | Best when                               |
| -------------------------------------- | ------------------------------------------- | --------------------------------------- |
| **Atomic Design**                      | UI granularity (atom → page)                | Design systems, Storybook               |
| **Container / Presentational**         | Data/effects vs pure UI in components       | Reuse UI, test presentation             |
| **Modular monolith / feature folders** | `features/<name>/`                          | One team, one deploy                    |
| **Vertical slice**                     | One folder = UI + hooks + API for a flow    | Minimize cross-folder jumps per feature |
| **Feature-Sliced Design (FSD)**        | Strict layers + slice public API            | Large SPA, enforced import rules        |
| **Bulletproof React**                  | Documented `src/` layout (features, shared) | Greenfield SPA, team template           |
| **Screaming Architecture**             | Top-level folders = business domains        | Domain obvious from tree                |
| **Micro-frontends (MFE)**              | Multiple deployable frontends               | Many teams, independent release         |
| **Monorepo packages**                  | `apps/web` + `packages/ui`                  | Shared kit across apps                  |

**When to use:** Combine **Atomic** (UI kit) with **feature folders** or **FSD** (app code). Use **Container/Presentational** inside features. **Bulletproof React** and **Screaming Architecture** are naming/layout conventions — compatible with FSD. **MFE** only when deploy boundaries require it.

**See also:** System-level patterns → [Architecture patterns — Frontend structure](./7_special_architecture.md#frontend-structure).

**Pluses and minuses:**

|             | Pluses                                       | Minuses                                          |
| ----------- | -------------------------------------------- | ------------------------------------------------ |
| Atomic + DS | Reusable primitives, clear Storybook stories | Does not replace folder rules for business logic |
| FSD         | Enforces dependency direction                | Ceremony for tiny apps                           |
| MFE         | Autonomous teams                             | Integration, shared React version, design drift  |

**Thoughts:** Job posts saying “MFA” often mean **MFE**. Routing → [Client routing](./4_react_ecosystem.md#client-routing); rendering → [SPA vs SSR vs RSC](./4_react_server.md#spa-vs-ssr-vs-rsc).

**Sources:** [Feature-Sliced Design](https://feature-sliced.design/) · [Atomic Design (Brad Frost)](https://atomicdesign.bradfrost.com/)

</details>

---

## Atomic Design

<details>
<summary>Atomic Design</summary>

**Definition:** **Atomic Design** is a methodology for building UIs in **five levels** — from smallest pieces to full screens — popular for **design systems** and component libraries.

**How it works:**

| Level         | React role                           | Examples                                                            |
| ------------- | ------------------------------------ | ------------------------------------------------------------------- |
| **Atoms**     | Smallest UI units, no business logic | `Button`, `Input`, `Label`, icon wrapper                            |
| **Molecules** | Composed atoms with one job          | `SearchField` (input + button), `FormField` (label + input + error) |
| **Organisms** | Distinct sections of interface       | `Header`, `ProductCard`, `LoginForm`                                |
| **Templates** | Page layout without real data        | `DashboardTemplate` with slots/props                                |
| **Pages**     | Templates + real content/state       | `DashboardPage` wired to data                                       |

**When to use:** Document components in **Storybook**; enforce consistency via a shared kit (often under `shared/ui` in FSD or `components/ui` in simpler repos).

**Pluses and minuses:**

| +                                 | −                                                             |
| --------------------------------- | ------------------------------------------------------------- |
| Shared vocabulary with design     | Level names are subjective — document team rules              |
| Encourages small, testable pieces | Atoms can become “god components” if every prop is crammed in |

**Examples:**

```tsx
// Atom
export function Button({ children, ...props }: ButtonProps) {
  return (
    <button type="button" className="btn" {...props}>
      {children}
    </button>
  );
}

// Molecule
export function SearchField({ onSubmit }: { onSubmit: (q: string) => void }) {
  const [q, setQ] = useState("");
  return (
    <form
      onSubmit={(e) => {
        e.preventDefault();
        onSubmit(q);
      }}
    >
      <Input
        value={q}
        onChange={(e) => setQ(e.target.value)}
        aria-label="Search"
      />
      <Button type="submit">Search</Button>
    </form>
  );
}
```

**Thoughts:** **Molecules** are where accessibility and validation UX usually live (label + control + hint). Pair with [Patterns — composition](./4_react_patterns.md); folder placement with [FSD `shared/ui`](#feature-sliced-design-fsd).

**Sources:** [Atomic Design](https://atomicdesign.bradfrost.com/chapter-2/) · [Storybook — Component hierarchy](https://storybook.js.org/docs)

</details>

---

## Container / Presentational

<details>
<summary>Container / Presentational</summary>

**Definition:** Split components into **containers** (data, effects, wiring) and **presentational** components (markup, styles, callbacks via props) — also called **smart vs dumb** components.

**How it works:**

| Role               | Responsibility                        | Typical hooks                          |
| ------------------ | ------------------------------------- | -------------------------------------- |
| **Container**      | Fetch/subscribe, map state to props   | `useQuery`, `useSelector`, `useEffect` |
| **Presentational** | Render UI from props; no global store | Optional local UI state only           |

**When to use:** Reuse the same UI with different data sources; snapshot-test presentational trees.

**Examples:**

```tsx
export function UserList({ users, onSelect }: UserListProps) {
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

export function UserListContainer() {
  const { data } = useGetUsersQuery();
  const navigate = useNavigate();
  return (
    <UserList users={data ?? []} onSelect={(id) => navigate(`/users/${id}`)} />
  );
}
```

**Thoughts:** **Custom hooks** often replace container components — same idea, less nesting. See [Patterns — composition](./4_react_patterns.md).

**Sources:** [Presentational and Container Components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a04c7f) · [react.dev — Custom hooks](https://react.dev/learn/reusing-logic-with-custom-hooks)

</details>

---

## Modular monolith (feature folders)

<details>
<summary>Modular monolith (feature folders)</summary>

**Definition:** A **modular monolith** keeps **one app and one deploy** but groups code by **feature** instead of technical type alone (`components/`, `hooks/`, `utils/` scattered).

**How it works:**

Typical shape:

```text
src/
  app/          # providers, router mount
  features/
    auth/
    cart/
    catalog/
  shared/       # ui kit, api, lib
  pages/        # optional route entry components
```

| Practice                                          | Why                                       |
| ------------------------------------------------- | ----------------------------------------- |
| Colocate hooks + components + api for one feature | Changes stay local                        |
| `shared/ui` for Atomic-level primitives           | Avoid duplicating Button in every feature |
| Thin `pages/`                                     | Only compose features — no business logic |

**When to use:** Single team or shared release; stepping stone before full **FSD** rules.

**Pluses and minuses:**

| +                                | −                                                               |
| -------------------------------- | --------------------------------------------------------------- |
| Simple to adopt                  | Without import rules, features can import each other circularly |
| Less boilerplate than FSD slices | Can outgrow flat `features/` — migrate to FSD layers            |

**Thoughts:** Server-state and auth boundaries are **architectural** too — document which feature owns session vs which only consumes `useUser()`. Data libraries → [Ecosystem — Data fetching](./4_react_ecosystem.md#data-fetching).

**See also:** [Feature-Sliced Design (FSD)](#feature-sliced-design-fsd) · [Micro-frontends (MFE)](#micro-frontends-mfe)

**Sources:** [React — Patterns](./4_react_patterns.md)

</details>

---

## Vertical slice architecture

<details>
<summary>Vertical slice architecture</summary>

**Definition:** **Vertical slice** means one **use case** owns a **vertical** cut of the stack — UI, state, and API client for that flow live together — instead of horizontal layers like `components/` + `services/` + `stores/` shared by every feature.

**How it works:**

```text
features/checkout/
  CheckoutPage.tsx
  useCheckout.ts
  checkoutApi.ts
  checkout.types.ts
```

| Horizontal (layer-first) | Vertical (slice-first)                |
| ------------------------ | ------------------------------------- |
| All hooks in `hooks/`    | Hook next to the feature that uses it |
| Shared `api/` god module | `checkoutApi.ts` colocated            |
| Change touches many dirs | Change stays in one slice folder      |

**When to use:** Same benefits as [modular monolith](#modular-monolith-feature-folders); emphasize **end-to-end ownership** per user story (checkout, onboarding, admin export).

**Thoughts:** **FSD** is a formalized vertical-slice system with layer names; **Bulletproof React** is another slice-friendly layout. Backend “vertical slice” (MediatR, etc.) is a separate idea — [Architecture patterns](./7_special_architecture.md).

**See also:** [Modular monolith (feature folders)](#modular-monolith-feature-folders) · [Feature-Sliced Design (FSD)](#feature-sliced-design-fsd)

**Sources:** [Jimmy Bogard — Vertical Slice Architecture](https://www.jimmybogard.com/vertical-slice-architecture/)

</details>

---

## Feature-Sliced Design (FSD)

<details>
<summary>Feature-Sliced Design (FSD)</summary>

**Definition:** Frontend folder methodology: **layers** (app → pages → widgets → features → entities → shared) plus **slices** with a public `index.ts` — scales teams without micro-frontend runtime complexity.

**How it works:**

| Layer      | React examples                                              |
| ---------- | ----------------------------------------------------------- |
| `app`      | Providers, global styles, store setup                       |
| `pages`    | Route compositions — wire widgets/features                  |
| `widgets`  | Header, sidebar, dashboard blocks                           |
| `features` | User scenarios: “add to cart”, “edit profile”               |
| `entities` | Business nouns: User, Product                               |
| `shared`   | UI kit ([Atomic Design](#atomic-design)), `api` client, lib |

Import rule: **lower layers must not import from higher layers**. A slice is split into **segments** (`ui`, `model`, `api`, `lib`). Other slices import only the slice **public API** (`index.ts`), not a file inside the slice. Router setup → [Ecosystem — Client routing](./4_react_ecosystem.md#client-routing).

**When to use:** Medium/large SPAs; alternative to [Micro-frontends (MFE)](#micro-frontends-mfe) with one deploy.

**Sources:** [feature-sliced.design](https://feature-sliced.design/) · [FSD layers](https://feature-sliced.design/docs/reference/layers) · [Frontend structure (canonical)](./7_special_architecture.md#frontend-structure)

</details>

---

## Bulletproof React

<details>
<summary>Bulletproof React</summary>

**Definition:** **Bulletproof React** is a community **reference architecture** (Alan Aydogmus) for scalable React apps — feature-first folders, shared kernel, and clear places for routes, assets, and tests.

**How it works:**

Typical `src/` layout:

```text
src/
  app/           # providers, router, global styles
  features/      # domain features (auth, discussions, …)
  components/    # shared UI (often design-system level)
  hooks/         # shared hooks
  lib/           # axios client, react-query config, utils
  stores/        # global client state (if needed)
  types/         # shared TS types
  assets/
  config/
```

| Folder        | Role                                                                      |
| ------------- | ------------------------------------------------------------------------- |
| `features/*`  | Business capabilities; may contain `components/`, `api/`, `hooks/` inside |
| `components/` | App-wide UI not tied to one feature                                       |
| `lib/`        | Cross-cutting infra (API client, env)                                     |

**When to use:** Greenfield SPA when you want a **copy-paste template**; aligns with [modular monolith](#modular-monolith-feature-folders) and can migrate toward [FSD](#feature-sliced-design-fsd) by renaming layers.

**Pluses and minuses:**

| +                                           | −                                                         |
| ------------------------------------------- | --------------------------------------------------------- |
| Well-documented, widely cited in interviews | Not an official React team standard                       |
| Features-first matches product mental model | `components/` vs `features/*/components` needs team rules |

**Thoughts:** Pair with [Atomic Design](#atomic-design) by treating shared `components/` as atoms/molecules. Data layer → [Ecosystem — Data fetching](./4_react_ecosystem.md#data-fetching).

**Sources:** [bulletproof-react](https://github.com/alan2207/bulletproof-react)

</details>

---

## Screaming Architecture

<details>
<summary>Screaming Architecture</summary>

**Definition:** **Screaming Architecture** (Uncle Bob) means the **top-level structure shouts the domain** — `catalog/`, `billing/`, `shipping/` — not `controllers/`, `views/`, `models/` or a flat `components/` soup.

**How it works:**

| Screaming (domain-first)       | Technical (layer-first)                |
| ------------------------------ | -------------------------------------- |
| `src/catalog/ProductList.tsx`  | `src/components/ProductList.tsx`       |
| `src/billing/InvoiceTable.tsx` | `src/hooks/useInvoices.ts` far from UI |

React mapping: each **domain folder** contains its own components, hooks, and API helpers (same as [vertical slice](#vertical-slice-architecture)); **shared/** holds cross-domain UI and lib.

**When to use:** Onboarding and refactors — newcomers find “checkout” code under `features/checkout` or `checkout/`, not scattered by file type.

**Pluses and minuses:**

| +                                 | −                                              |
| --------------------------------- | ---------------------------------------------- |
| Aligns code with product language | Domains overlap → need rules (FSD slices help) |
| Pairs with modular monolith / FSD | Wrong domain cuts are hard to rename           |

**Thoughts:** FSD **slices** (`features/add-to-cart`) are a screaming architecture with enforced layer direction.

**Sources:** [Robert C. Martin — Screaming Architecture](https://blog.cleancoder.com/uncle-bob/2011/09/30/Screaming-Architecture.html)

</details>

---

## Micro-frontends (MFE)

<details>
<summary>Micro-frontends (MFE)</summary>

**Definition:** **Micro-frontends** split one product UI into **independently deployable** frontends (by team or domain), composed in the browser — multiple React roots or remote bundles on one site.

**How it works:**

| Pattern                     | Integration                                  | Tradeoff                                                                             |
| --------------------------- | -------------------------------------------- | ------------------------------------------------------------------------------------ |
| **Build-time composition**  | Monorepo packages imported into one shell    | Shared release; simplest                                                             |
| **Module Federation**       | Host `remotes` + remote `exposes` at runtime | Independent deploys; version skew                                                    |
| **Iframe / Web Components** | Hard isolation                               | UX and styling cost                                                                  |
| **Route-owned remotes**     | Shell loads remote per URL prefix            | Clear ownership (routing setup → [Ecosystem](./4_react_ecosystem.md#client-routing)) |

Shared `react` / `react-dom` must be **singletons** in Federation to avoid two Reacts.

**When to use:** Large orgs with separate ship cadence; **not** the default — prefer [FSD](#feature-sliced-design-fsd) or [modular monolith](#modular-monolith-feature-folders) until deploy boundaries hurt.

**Pluses and minuses:**

| +                           | −                                      |
| --------------------------- | -------------------------------------- |
| Team autonomy               | Design system and state sharing harder |
| Smaller deploy blast radius | Runtime integration bugs               |

**Thoughts:** For code splitting inside **one** app, use [Code splitting](./4_react_perf.md#code-splitting) — not MFE.

**See also:** [Architecture overview](#architecture-overview) · [Feature-Sliced Design (FSD)](#feature-sliced-design-fsd)

**Sources:** [Webpack — Module Federation](https://webpack.js.org/concepts/module-federation/) · [Rspack — Module Federation](https://rspack.dev/guide/features/module-federation)

</details>

---

## Monorepo packages

<details>
<summary>Monorepo packages</summary>

**Definition:** A **monorepo** hosts multiple **packages** and **apps** in one git repository — e.g. `apps/web` (React SPA) + `packages/ui` (design system) + `packages/api-types` (shared contracts).

**How it works:**

```text
apps/
  web/              # main React app (Vite/Next)
  admin/            # optional second app
packages/
  ui/               # shared Atomic components
  config-eslint/    # shared tooling
  api-client/       # typed client generated from OpenAPI
```

| Tooling                                  | Role                     |
| ---------------------------------------- | ------------------------ |
| **pnpm workspaces** / **npm workspaces** | Link local packages      |
| **Turborepo** / **Nx**                   | Cache builds, task graph |

**When to use:** One design system feeding web + admin; shared types with backend; **not** the same as [MFE](#micro-frontends-mfe) — usually one deploy per app, shared code at build time.

**Pluses and minuses:**

| +                           | −                                                                                                 |
| --------------------------- | ------------------------------------------------------------------------------------------------- |
| Single PR updates app + kit | Repo size and CI complexity                                                                       |
| Enforces package boundaries | Needs [Module / package boundaries](./7_special_architecture.md#module--package-boundaries) rules |

**Thoughts:** [Atomic Design](#atomic-design) kit often lives in `packages/ui`. TypeScript API types → [TypeScript — API contracts](./5_ts_api_contracts.md).

**See also:** [Micro-frontends (MFE)](#micro-frontends-mfe) · [Architecture patterns — Module boundaries](./7_special_architecture.md#module--package-boundaries)

**Sources:** [Turborepo docs](https://turbo.build/repo/docs) · [pnpm workspaces](https://pnpm.io/workspaces)

</details>
