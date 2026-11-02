# Angular — Platform

Change detection, Zone.js, dependency injection, compilation, and SSR.

[← Back to hub](./4_angular.md)

### Related docs

- [Angular — Core](./4_angular_core.md) · [Angular — Platform](./4_angular_platform.md) · [Angular — Ecosystem](./4_angular_ecosystem.md) · [Angular — Interview](./4_angular_interview.md)

### Sections

- [1. Change detection](#1-change-detection)
- [2. Zone.js & NgZone](#2-zonejs-ngzone)
- [3. Dependency injection](#3-dependency-injection)
- [4. Compilation & rendering](#4-compilation-rendering)

---

## 1. Change detection

Change detection

<details>
  <summary>Change detection</summary>

### Change detection

**Definition:** **Change detection** walks the component tree and updates the DOM when bindings may have changed.

**How it works:** By default (**`ChangeDetectionStrategy.Default`**), Angular checks all components on each cycle (often triggered by Zone.js patching async APIs). **`OnPush`** checks a component only when inputs change, events fire from its template, observables via `async` pipe emit, or you **`markForCheck()`** / run CD manually.

**Common pitfalls:** Mutating objects without new references breaks OnPush; third-party widgets outside Angular need `NgZone.run()` or manual CD.

**See also:** [OnPush](./4_angular_platform.md#onpush-strategy) · [markForCheck vs detectChanges](./4_angular_interview.md#markforcheck-vs-detectchanges)

</details>

OnPush strategy

<details>
  <summary>OnPush strategy</summary>

### OnPush strategy

**Definition:** **`ChangeDetectionStrategy.OnPush`** limits checks to input reference changes, template events, `async` pipe updates, and explicit marks.

**How it works:** Improves performance for large trees. Pair with **immutable** data or signals/observables. Call **`ChangeDetectorRef.markForCheck()`** when parent sets state that OnPush child must reflect.

</details>

markForCheck vs detectChanges

<details>
  <summary>markForCheck vs detectChanges</summary>

### markForCheck vs detectChanges

**Definition:** **`markForCheck()`** schedules this branch for the **next** global CD cycle. **`detectChanges()`** runs CD **immediately** on this subtree.

**How it works:** Prefer `markForCheck()` in OnPush components. Use `detectChanges()` sparingly when you must sync view mid-method (tests, integrations).

</details>

runOutsideAngular and detach

<details>
  <summary>runOutsideAngular and detach</summary>

### runOutsideAngular and detach

**Definition:** **`NgZone.runOutsideAngular`** runs work without triggering CD. **`ChangeDetectorRef.detach()`** removes a component from the global CD tree until **`reattach()`**.

**How it works:** Use for high-frequency events (mousemove, scroll handlers) that update non-Angular UI, then re-enter the zone when Angular state must update.

**See also:** [NgZone](./4_angular_platform.md#ngzone)

</details>

## 2. Zone.js & NgZone

NgZone

<details>
  <summary>NgZone</summary>

### NgZone

**Definition:** **`NgZone`** wraps Angular's execution context so async tasks can trigger change detection when they complete.

**How it works:** Zone.js patches promises, timers, and DOM events. `ngZone.run(() => …)` re-enters Angular after third-party callbacks. **`runOutsideAngular`** skips CD for hot paths.

**See also:** [Zone.js](./4_angular_platform.md#zonejs)

</details>

Zone.js

<details>
  <summary>Zone.js</summary>

### Zone.js

**Definition:** **Zone.js** monkey-patches async primitives so Angular knows when to run change detection.

**How it works:** New apps may use **`provideExperimentalZonelessChangeDetection()`** (zoneless) — you then trigger updates via signals or manual CD. Most production apps still ship Zone.js unless explicitly migrated.

**Sources:**

[angular.dev/guide/zone](https://angular.dev/guide/zone)

</details>

## 3. Dependency injection

Dependency injection

<details>
  <summary>Dependency injection</summary>

### Dependency injection

**Definition:** **Dependency injection (DI)** supplies collaborators to classes via constructors (`inject()` / constructor params) instead of `new`.

**How it works:** Register providers on **`bootstrapApplication`**, route config, component `providers`, or `@Injectable({ providedIn: 'root' })`. Angular builds an **injector tree** matching the component hierarchy.

**Examples:**

```ts
@Injectable({ providedIn: "root" })
export class UserService {
  getUsers() {
    return this.http.get<User[]>("/api/users");
  }
  constructor(private http: HttpClient) {}
}
```

**See also:** [Hierarchical injectors](./4_angular_platform.md#hierarchical-injectors) · [Resolution modifiers](./4_angular_platform.md#resolution-modifiers)

</details>

Hierarchical injectors

<details>
  <summary>Hierarchical injectors</summary>

### Hierarchical injectors

**Definition:** Each component (and module, in NgModule apps) can have an **injector**. Child injectors inherit parent providers and can **override** them locally.

**How it works:** `ElementInjector` tree follows the DOM; `EnvironmentInjector` holds app-wide providers. Lazy routes get their own injector scope.

**See also:** [Injector trees](./4_angular_interview.md#injector-trees)

</details>

Resolution modifiers

<details>
  <summary>Resolution modifiers</summary>

### Resolution modifiers

**Definition:** **`@Optional()`**, **`@Self()`**, **`@SkipSelf()`**, **`@Host()`** control **where** DI looks for a token.

**How it works:** `@Self()` = only this injector; `@SkipSelf()` = skip current, ask parent; `@Host()` = up to component host boundary; `@Optional()` = null if missing.

</details>

## 4. Compilation & rendering

AOT vs JIT

<details>
  <summary>AOT vs JIT</summary>

### AOT vs JIT

**Definition:** **AOT (Ahead-of-Time)** compiles templates at **build** time. **JIT (Just-in-Time)** compiled in the **browser** at runtime (legacy dev flow).

**How it works:** Production builds use **AOT** for smaller bundles, earlier template errors, and no compiler in the browser. `ng serve` uses AOT by default in modern CLI.

**See also:** [AOT benefits](./4_angular_interview.md#aot-benefits)

</details>

Angular Ivy

<details>
  <summary>Angular Ivy</summary>

### Angular Ivy

**Definition:** **Ivy** is Angular's current rendering engine and compiler pipeline (tree-shakable, localized, faster builds).

**How it works:** Replaced View Engine; enables standalone components, improved DI metadata, and smaller production bundles.

</details>

SSR and Universal

<details>
  <summary>SSR and Universal</summary>

### SSR and Universal

**Definition:** **SSR** renders HTML on the server for first paint and SEO; the client **hydrates** interactivity.

**How it works:** Use **`@angular/ssr`** / `ng add @angular/ssr`. Avoid direct DOM access in constructors; use `PLATFORM_ID` + `isPlatformBrowser` guards.

**See also:** [SPA overview](./4_angular_ecosystem.md#spa)

</details>

Angular signals

<details>
  <summary>Angular signals</summary>

### Angular signals

**Definition:** **Signals** — синхронные reactive-примитивы Angular: **`signal`**, **`computed`**, **`effect`**. Шаблон читает **`signal()`** и получает точечные обновления при signal-based change detection.

**How it works:** **`input()` / `output()` / `model()`** на новых компонентах; **`toSignal(obs$)`** для RxJS. UI-состояние — signals; HTTP/потоки — RxJS.

**When to use:** Локальное и общее UI-состояние, постепенная миграция с `BehaviorSubject`. Сочетать с **OnPush**.

**See also:** [Interview — signals syllabus](./4_angular_interview.md#angular-interview--syllabus-blocks) · [angular.dev — signals](https://angular.dev/guide/signals)

**Sources:**

[angular.dev — signals](https://angular.dev/guide/signals)

</details>

Tree shaking

<details>
  <summary>Tree shaking</summary>

### Tree shaking

**Definition:** **Tree shaking** removes unused exports from bundles during production builds.

**How it works:** Prefer **`providedIn: 'root'`**, standalone imports, and side-effect-free RxJS operator imports (`import { map } from 'rxjs/operators'`). Ivy marks components/directives for shaking when not referenced.

</details>
