# Angular — Ecosystem

Routing, forms, HTTP, RxJS, state, security, and performance.

[← Back to hub](./4_angular.md)

### Related docs

- [Angular — Core](./4_angular_core.md) · [Angular — Platform](./4_angular_platform.md) · [Angular — Ecosystem](./4_angular_ecosystem.md) · [Angular — Interview](./4_angular_interview.md)

### Sections

- [1. Directives & dynamic UI](#1-directives-dynamic-ui)
- [2. Routing](#2-routing)
- [3. Forms](#3-forms)
- [4. HTTP & security](#4-http-security)
- [5. RxJS in Angular](#5-rxjs-in-angular)
- [6. State & architecture](#6-state-management)
- [7. SPA & comparisons](#7-spa)

---

## 1. Directives & dynamic UI

Directives

<details>
  <summary>Directives</summary>

### Directives

**Definition:** **Directives** extend templates: **components** (with templates), **attribute** directives (behavior/appearance), **structural** directives (DOM add/remove).

**How it works:** `*ngIf`, `*ngFor`, `*ngSwitch` are structural — the `*` prefix is syntactic sugar for `<ng-template>` + directive. Custom structural directives use `TemplateRef` + `ViewContainerRef`.

**See also:** [ng-template, ng-container, ng-content](./4_angular_ecosystem.md#projection-primitives)

</details>

Projection primitives

<details>
  <summary>Projection primitives</summary>

### Projection primitives

**Definition:**

| Primitive | Role |
| --------- | ---- |
| `<ng-template>` | Inert template fragment; used by structural directives and `*ngTemplateOutlet` |
| `<ng-container>` | Group nodes without extra DOM |
| `<ng-content>` | Project light DOM from parent (content projection) |
| `*ngTemplateOutlet` | Render a referenced `ng-template` |

**How it works:** `<ng-content select="[slot]">` projects matching child nodes. Multi-slot layouts compose design systems.

</details>

Dynamic components

<details>
  <summary>Dynamic components</summary>

### Dynamic components

**Definition:** **Dynamic components** are created at runtime instead of static template tags.

**How it works (modern):** Use **`ViewContainerRef.createComponent(MyComponent)`** (standalone) or load routes with **`loadComponent`**. Legacy **`ComponentFactoryResolver`** is deprecated — prefer `createComponent`.

**Steps:** host `ViewContainerRef` → `createComponent` → set `instance` inputs → destroy on teardown.

</details>

Custom elements

<details>
  <summary>Custom elements</summary>

### Custom elements

**Definition:** **Custom elements** are standard Web Components (`customElements.define`).

**How it works:** Angular **`@angular/elements`** wraps components as custom elements for micro-frontends or embedding in non-Angular pages. Requires bundling the component factory and bootstrap logic.

</details>

## 2. Routing

Routing

<details>
  <summary>Routing</summary>

### Routing

**Definition:** The **Angular Router** maps URL paths to components with optional guards, resolvers, and lazy loading.

**How it works:** Configure **`Routes`** with `path`, `component` / `loadComponent` / `loadChildren`. Template: `<router-outlet>`, `routerLink`, `Router.navigate`.

**Examples:**

```ts
export const routes: Routes = [
  { path: "", redirectTo: "home", pathMatch: "full" },
  { path: "home", component: HomeComponent },
  {
    path: "admin",
    loadChildren: () => import("./admin/routes").then((m) => m.ADMIN_ROUTES),
    canActivate: [authGuard],
  },
];
```

**See also:** [Router lifecycle](./4_angular_ecosystem.md#router-lifecycle) · [forRoot vs forChild](./4_angular_interview.md#routermodule-forroot-vs-forchild)

</details>

Router lifecycle

<details>
  <summary>Router lifecycle</summary>

### Router lifecycle

**Definition:** Navigation emits a stream of **router events** (`NavigationStart`, `RoutesRecognized`, `GuardsCheckStart`, `ResolveStart`, `ActivationEnd`, `NavigationEnd`, …).

**How it works:** **Guards** (`canActivate`, `canDeactivate`, `canMatch`) gate access. **Resolvers** prefetch data before activation.

</details>

Route guards

<details>
  <summary>Route guards</summary>

### Route guards

**Definition:** **Route guards** are injectable functions that allow or block navigation.

**How it works:** Implement `CanActivateFn` (functional guards) returning `boolean`, `UrlTree`, or `Observable`/`Promise` of those. Redirect unauthenticated users with `router.parseUrl('/login')`.

</details>

Preloading strategies

<details>
  <summary>Preloading strategies</summary>

### Preloading strategies

**Definition:** **Preloading** loads lazy modules in the background after bootstrap.

**How it works:** `PreloadAllModules`, `NoPreloading`, or custom `PreloadingStrategy` implementing `preload(route, load)`.

</details>

## 3. Forms

Forms

<details>
  <summary>Forms</summary>

### Forms

**Definition:** Angular supports **template-driven** forms (`NgForm`, `ngModel`) and **reactive** forms (`FormGroup`, `FormControl`, `FormBuilder`).

**How it works:**

| Approach | Best for |
| -------- | -------- |
| Template-driven | Simple forms, prototypes, two-way binding |
| Reactive | Complex validation, dynamic fields, unit tests, immutable value streams |

**See also:** [NgForm vs FormGroup](./4_angular_interview.md#ngform-formgroup-formcontrol)

</details>

## 4. HTTP & security

HttpClient

<details>
  <summary>HttpClient</summary>

### HttpClient

**Definition:** **`HttpClient`** is Angular's typed HTTP API built on RxJS `Observable`s.

**How it works:** JSON parsing, interceptors, progress events, and testable `HttpTestingController`. Prefer **`HttpClient`** over raw `fetch` for interceptor chains and DI.

**See also:** [HTTP interceptors](./4_angular_ecosystem.md#http-interceptors)

</details>

HTTP interceptors

<details>
  <summary>HTTP interceptors</summary>

### HTTP interceptors

**Definition:** **Interceptors** mutate outgoing requests or incoming responses in a chain.

**How it works:** `HttpInterceptorFn` (functional) or class interceptors — attach auth headers, log errors, retry. Register with `provideHttpClient(withInterceptors([...]))`.

</details>

Security best practices

<details>
  <summary>Security best practices</summary>

### Security best practices

**Definition:** Front-end security layers: sanitize untrusted HTML, enforce **CSP**, use **HttpOnly/Secure** cookies for sessions, validate input, and keep Angular patched.

**How it works:** Angular sanitizes binding by default; avoid `bypassSecurityTrustHtml` unless trusted. Enable **`HttpClientXsrfModule`** for CSRF tokens. Route guards are UX gates — **authorize on the server**.

**See also:** [Web security interview](../7_special_security_interview.md#security--front-end-checklist)

</details>

Service workers

<details>
  <summary>Service workers</summary>

### Service workers

**Definition:** Angular **`@angular/service-worker`** adds offline caching and update prompts for PWAs.

**How it works:** `ng add @angular/pwa` configures `ngsw-config.json` and registers the worker in production builds.

</details>

Performance

<details>
  <summary>Performance</summary>

### Performance

**Definition:** Angular performance combines **OnPush**, lazy routes, **`trackBy`**, avoiding unnecessary subscriptions, and production builds (AOT, tree shaking).

**How it works:** Profile with Angular DevTools. Use `async` pipe instead of manual subscribe when possible. Defer heavy work with `@defer` blocks (v17+).

</details>

trackBy with ngFor

<details>
  <summary>trackBy with ngFor</summary>

### trackBy with ngFor

**Definition:** **`trackBy`** tells Angular how to identify row identity when lists change.

**How it works:** Without `trackBy`, Angular re-creates DOM nodes when array reference changes. `trackBy: (_, item) => item.id` preserves DOM and component state.

</details>

## 5. RxJS in Angular

RxJS in Angular

<details>
  <summary>RxJS in Angular</summary>

### RxJS in Angular

**Definition:** Angular uses **RxJS** for HTTP, forms, router events, and async templates (`async` pipe).

**How it works:** Always **unsubscribe** or use **`takeUntilDestroyed()`** / `async` pipe. Prefer **`switchMap`** for cancellable searches; **`concatMap`** for ordered writes; **`mergeMap`** for parallel work with limits.

**See also:** [JS async — Observables](../3_js_async.md) · [Operator map](./4_angular_interview.md#rxjs-operators-index)

</details>

Promise vs Observable

<details>
  <summary>Promise vs Observable</summary>

### Promise vs Observable

**Definition:** A **Promise** resolves once. An **Observable** can emit many values over time and be cancelled via unsubscribe.

**How it works:** HttpClient returns Observables — convert with `firstValueFrom` / `lastValueFrom` when you need a Promise.

</details>

Subjects comparison

<details>
  <summary>Subjects comparison</summary>

### Subjects comparison

| Type | Behavior |
| ---- | -------- |
| `Observable` | Cold by default; no cached value |
| `BehaviorSubject` | Replays latest value to new subscribers |
| `ReplaySubject` | Replays buffer of N values |
| `AsyncSubject` | Emits only last value on complete |

</details>

## 6. State management

State management

<details>
  <summary>State management</summary>

### State management

**Definition:** Local component state, **services with `BehaviorSubject`**, or **NgRx Store** for global predictable state.

**How it works:** Use **NgRx** when many features share complex state, need time-travel debugging, or strict action/reducer audit trails. Simple apps: services + signals/`BehaviorSubject`.

**See also:** [Service state pattern](./4_angular_interview.md#service-state-management)

</details>

When to use NgRx

<details>
  <summary>When to use NgRx</summary>

### When to use NgRx

**Definition:** **@ngrx/store** implements Redux-style immutable state with actions and reducers.

**When to use:** Cross-feature shared state, complex async workflows, large teams needing conventions. Skip for small apps — service + signals is enough.

</details>

## 7. SPA

SPA

<details>
  <summary>SPA</summary>

### SPA

**Definition:** A **Single-Page Application** loads one shell HTML page and swaps views client-side via the router.

**How it works:** Angular SPAs boot once; subsequent navigation avoids full reloads. Trade-offs: initial bundle size vs MPA — mitigate with lazy loading and SSR.

**See also:** [Angular vs React](../4_react_platforms.md) · [Interview comparisons](./4_angular_interview.md#angular-vs-react-and-vue)

</details>

Angular CLI generator

<details>
  <summary>Angular CLI generator</summary>

### Angular CLI generator

**Definition:** **`ng generate`** scaffolds components, services, guards, pipes, and modules with consistent structure.

**How it works:** `ng g c feature/widget --standalone` creates component + test + style files. Schematics enforce team conventions.

</details>

Dev proxy

<details>
  <summary>Dev proxy</summary>

### Dev proxy

**Definition:** **`proxy.conf.json`** forwards dev-server paths to a backend to avoid CORS during local development.

**How it works:** `ng serve --proxy-config proxy.conf.json` maps `/api` → `http://localhost:3000`.

</details>

Input required initializer

<details>
  <summary>Input required initializer</summary>

### Input required initializer

**Definition:** TypeScript strict mode requires definite assignment for `@Input()` fields.

**How it works:** Use **`input.required<T>()`** (signal inputs) or `@Input({ required: true }) field!: T` with definite assignment assertion, or provide defaults.

</details>
