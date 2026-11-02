# Angular — Interview

Interview Q&A aligned with common Angular lists (EN/RU). Theory depth lives in sibling topics — open **See also** links before memorizing long answers.

[← Back to hub](./4_angular.md)

### Related docs

- [Angular — Core](./4_angular_core.md) · [Angular — Platform](./4_angular_platform.md) · [Angular — Ecosystem](./4_angular_ecosystem.md)

### Sections

- [Questions & Answers](#questions-answers)
- [Angular interview course index](#angular-interview--course-index)
- [RU cheatsheet index](#ru-cheatsheet-index)
- [Sources](#sources)

---

<a name="angular--interview-checklist"></a>

## Questions & Answers

Compact checklist — search with `Cmd+F` using RU or EN phrasing from your list.

What is Angular?

<details>
  <summary>What is Angular?</summary>

### What is Angular?

**Answer:** TypeScript **platform** for SPAs: components, templates, DI, RxJS, CLI. Differs from AngularJS (1.x). Bootstraps with `bootstrapApplication` or `AppModule`.

**See also:** [Angular overview](./4_angular_core.md#angular-overview)

</details>

AngularJS vs Angular?

<details>
  <summary>AngularJS vs Angular?</summary>

### AngularJS vs Angular?

**Answer:** AngularJS = `$scope`, controllers, old digest cycle. Angular 2+ = components, TypeScript, Ivy renderer, different DI/CD. No shared API — migrate or rewrite.

**See also:** [AngularJS vs Angular](./4_angular_core.md#angularjs-vs-angular)

</details>

Component lifecycle hooks?

<details>
  <summary>Component lifecycle hooks?</summary>

### Component lifecycle hooks?

**Answer:** `constructor` → `ngOnChanges` → `ngOnInit` → `ngDoCheck` → content hooks → view hooks → `ngOnDestroy`. Init with inputs in `ngOnInit`; cleanup subscriptions in `ngOnDestroy`.

**See also:** [Component lifecycle](./4_angular_core.md#component-lifecycle)

</details>

<a name="constructor-vs-ngoninit"></a>

constructor vs ngOnInit?

<details>
  <summary>constructor vs ngOnInit?</summary>

### constructor vs ngOnInit?

**Answer:** Constructor = **DI only**. `ngOnInit` = first run after `@Input()` bindings exist — fetch data here.

**See also:** [constructor vs ngOnInit](./4_angular_core.md#constructor-vs-ngoninit)

</details>

Data binding types?

<details>
  <summary>Data binding types?</summary>

### Data binding types?

**Answer:** Interpolation `{{ }}`, property `[ ]`, event `( )`, two-way `[(ngModel)]`, class/style bindings.

**See also:** [Data binding](./4_angular_core.md#data-binding)

</details>

AOT vs JIT?

<details>
  <summary>AOT vs JIT?</summary>

### AOT vs JIT?

**Answer:** **AOT** compiles templates at build time (production default). **JIT** compiled in browser — legacy dev-only path.

**See also:** [AOT vs JIT](./4_angular_platform.md#aot-vs-jit)

</details>

Change detection?

<details>
  <summary>Change detection?</summary>

### Change detection?

**Answer:** Angular compares bindings and updates DOM. **Default** checks whole tree; **OnPush** checks on input ref change, events, `async` pipe, or `markForCheck`.

**See also:** [Change detection](./4_angular_platform.md#change-detection)

</details>

NgZone and Zone.js?

<details>
  <summary>NgZone and Zone.js?</summary>

### NgZone and Zone.js?

**Answer:** Zone.js patches async APIs to schedule CD. `NgZone.runOutsideAngular` skips CD for hot paths; `run()` re-enters when updating Angular state.

**See also:** [NgZone](./4_angular_platform.md#ngzone) · [Zone.js](./4_angular_platform.md#zonejs)

</details>

TypeScript decorators?

<details>
  <summary>TypeScript decorators?</summary>

### TypeScript decorators?

**Answer:** Functions attached with `@` on classes, methods, properties, parameters — Angular uses them for `@Component`, `@Injectable`, `@Input`, etc. Require `experimentalDecorators` / standard decorators per TS version.

**See also:** [TS decorators](../5_ts_decorators.md)

</details>

Angular pros and cons?

<details>
  <summary>Angular pros and cons?</summary>

### Angular pros and cons?

**Answer:** **Pros:** full stack opinion, TypeScript-first, DI, CLI, strong enterprise patterns. **Cons:** learning curve, bundle size, verbosity vs React/Vue, RxJS required literacy.

**See also:** [Angular vs React and Vue](#angular-vs-react-and-vue)

</details>

Dependency injection?

<details>
  <summary>Dependency injection?</summary>

### Dependency injection?

**Answer:** Inject services via constructor/`inject()`. Register with `providedIn: 'root'` or route/component providers. Enables testing with mocks.

**See also:** [Dependency injection](./4_angular_platform.md#dependency-injection)

</details>

Directives and projection?

<details>
  <summary>Directives and projection?</summary>

### Directives and projection?

**Answer:** Structural `*ngIf/*ngFor`; attribute `ngClass`. `ng-template` holds fragments; `ng-container` groups without DOM; `ng-content` projects children; `*ngTemplateOutlet` renders templates.

**See also:** [Directives](./4_angular_ecosystem.md#directives) · [Projection primitives](./4_angular_ecosystem.md#projection-primitives)

</details>

Dynamic components?

<details>
  <summary>Dynamic components?</summary>

### Dynamic components?

**Answer:** Get `ViewContainerRef` → `createComponent(Comp)` → set inputs → destroy in `ngOnDestroy`. Prefer standalone `createComponent` over deprecated `ComponentFactoryResolver`.

**See also:** [Dynamic components](./4_angular_ecosystem.md#dynamic-components)

</details>

Routing and RouterModule?

<details>
  <summary>Routing and RouterModule?</summary>

### Routing and RouterModule?

**Answer:** Define `Routes`, `<router-outlet>`, guards, lazy `loadChildren` / `loadComponent`. **`forRoot`** once in app config; **`forChild`** in feature modules.

<a name="routermodule-forroot-vs-forchild"></a>

**See also:** [Routing](./4_angular_ecosystem.md#routing)

</details>

<a name="interpolation"></a>

Interpolation?

<details>
  <summary>Interpolation?</summary>

### Interpolation?

**Answer:** `{{ expression }}` one-way binding to text content; updated on change detection.

</details>

When to use NgRx?

<details>
  <summary>When to use NgRx?</summary>

### When to use NgRx?

**Answer:** Large shared state, many actors, need reducers/time-travel — not for trivial apps.

**See also:** [When to use NgRx](./4_angular_ecosystem.md#when-to-use-ngrx)

</details>

Smart vs presentational?

<details>
  <summary>Smart vs presentational?</summary>

### Smart vs presentational?

**Answer:** Smart = data + services; presentational = `@Input`/`@Output` only.

**See also:** [Smart vs presentational](./4_angular_core.md#smart-vs-presentational-components)

</details>

ViewChild vs ContentChild?

<details>
  <summary>ViewChild vs ContentChild?</summary>

### ViewChild vs ContentChild?

**Answer:** ViewChild = own template; ContentChild = projected content.

**See also:** [ViewChild and ContentChild](./4_angular_core.md#viewchild-and-contentchild)

</details>

<a name="template-reference-variables"></a>

Template reference variables?

<details>
  <summary>Template reference variables?</summary>

### Template reference variables?

**Answer:** `#ref` on template node; use in template or `@ViewChild('ref')` in class.

</details>

View encapsulation modes?

<details>
  <summary>View encapsulation modes?</summary>

### View encapsulation modes?

**Answer:** `Emulated` (default), `ShadowDom`, `None` (global styles).

**See also:** [View encapsulation](./4_angular_core.md#view-encapsulation)

</details>

Storing data in Angular?

<details>
  <summary>Storing data in Angular?</summary>

### Storing data in Angular?

**Answer:** Component fields, services (`BehaviorSubject`/signals), localStorage (careful), NgRx store for global immutable state.

</details>

Template-driven vs reactive forms?

<details>
  <summary>Template-driven vs reactive forms?</summary>

### Template-driven vs reactive forms?

**Answer:** Template-driven = `ngModel`, quick forms. Reactive = `FormGroup`, validators, dynamic controls, easier unit tests.

**See also:** [Forms](./4_angular_ecosystem.md#forms)

</details>

Inject a service?

<details>
  <summary>Inject a service?</summary>

### Inject a service?

**Answer:** `@Injectable({ providedIn: 'root' })` + constructor `private svc: MyService` or `inject(MyService)`.

</details>

Improve Angular performance?

<details>
  <summary>Improve Angular performance?</summary>

### Improve Angular performance?

**Answer:** OnPush, lazy routes, trackBy, async pipe, defer blocks, production AOT, avoid unnecessary subscriptions, profile with DevTools.

**See also:** [Performance](./4_angular_ecosystem.md#performance)

</details>

Protect routed components?

<details>
  <summary>Protect routed components?</summary>

### Protect routed components?

**Answer:** `canActivate` / `canMatch` functional guards; return `UrlTree` redirect if unauthorized.

**See also:** [Route guards](./4_angular_ecosystem.md#route-guards)

</details>

<a name="declarations-providers-imports"></a>

declarations, providers, imports in NgModule?

<details>
  <summary>declarations, providers, imports in NgModule?</summary>

### declarations, providers, imports in NgModule?

**Answer:** **declarations** = components/directives/pipes owned by module. **imports** = other modules. **providers** = DI tokens for this module scope.

</details>

Reactive programming in Angular?

<details>
  <summary>Reactive programming in Angular?</summary>

### Reactive programming in Angular?

**Answer:** RxJS streams for HTTP, forms, router; compose with operators; expose to templates via `async` pipe.

**See also:** [RxJS in Angular](./4_angular_ecosystem.md#rxjs-in-angular)

</details>

BehaviorSubject vs Observable?

<details>
  <summary>BehaviorSubject vs Observable?</summary>

### BehaviorSubject vs Observable?

**Answer:** BehaviorSubject stores **current value** for late subscribers; Observable alone is cold unless shared/hot.

**See also:** [Subjects comparison](./4_angular_ecosystem.md#subjects-comparison)

</details>

Error display component?

<details>
  <summary>Error display component?</summary>

### Error display component?

**Answer:** Dumb component with `@Input() message` + optional `@Input() severity`; parent sets message from HTTP/interceptor or form errors.

</details>

Child to parent data?

<details>
  <summary>Child to parent data?</summary>

### Child to parent data?

**Answer:** `@Output() event = new EventEmitter()` + `(event)="handler($event)"` or shared service / store.

</details>

<a name="ngform-formgroup-formcontrol"></a>

NgForm vs FormGroup vs FormControl?

<details>
  <summary>NgForm vs FormGroup vs FormControl?</summary>

### NgForm vs FormGroup vs FormControl?

**Answer:** **NgForm** wraps template-driven form state. **FormGroup** = reactive form container. **FormControl** = single control value + validators.

</details>

SharedModule and lazy loading?

<details>
  <summary>SharedModule and lazy loading?</summary>

### SharedModule and lazy loading?

**Answer:** Do not re-provide singleton services from SharedModule in lazy modules — use `providedIn: 'root'`.

**See also:** [SharedModule anti-pattern](./4_angular_core.md#sharedmodule-anti-pattern)

</details>

switchMap vs concatMap vs mergeMap?

<details>
  <summary>switchMap vs concatMap vs mergeMap?</summary>

### switchMap vs concatMap vs mergeMap?

**Answer:** **switchMap** cancels prior inner observable. **concatMap** queues sequentially. **mergeMap** runs inner streams concurrently.

</details>

Pure vs impure pipes?

<details>
  <summary>Pure vs impure pipes?</summary>

### Pure vs impure pipes?

**Answer:** **Pure** (default) recalculates only when input reference changes. **Impure** (`pure: false`) runs every CD cycle — costly.

</details>

HttpClient advantages?

<details>
  <summary>HttpClient advantages?</summary>

### HttpClient advantages?

**Answer:** Typed responses, interceptors, testing utilities, Observable-based API integrated with Angular DI.

**See also:** [HttpClient](./4_angular_ecosystem.md#httpclient)

</details>

<a name="aot-benefits"></a>

AOT benefits?

<details>
  <summary>AOT benefits?</summary>

### AOT benefits?

**Answer:** Smaller runtime, early template errors, no compiler in browser, better tree shaking.

</details>

Service workers in Angular?

<details>
  <summary>Service workers in Angular?</summary>

### Service workers in Angular?

**Answer:** `@angular/service-worker` for offline caching and update notifications — enable via `ng add @angular/pwa`.

</details>

Angular platform?

<details>
  <summary>Angular platform?</summary>

### Angular platform?

**Answer:** Runtime that bootstraps modules/components — browser (`platformBrowser`), server (`platformServer`). `platformBrowserDynamic().bootstrapModule/App`.

</details>

ngFor trackBy?

<details>
  <summary>ngFor trackBy?</summary>

### ngFor trackBy?

**Answer:** Stable identity function prevents DOM thrash when array items move or update.

**See also:** [trackBy with ngFor](./4_angular_ecosystem.md#trackby-with-ngfor)

</details>

RxJS pros and cons?

<details>
  <summary>RxJS pros and cons?</summary>

### RxJS pros and cons?

**Answer:** **Pros:** powerful composition, cancellation, multicasting. **Cons:** learning curve, subscription leaks if unmanaged.

</details>

RxJS error handling?

<details>
  <summary>RxJS error handling?</summary>

### RxJS error handling?

**Answer:** `catchError`, `retry`/`retryWhen`, error callback in `subscribe`, `HttpInterceptor` for HTTP errors.

</details>

Unsubscribe importance?

<details>
  <summary>Unsubscribe importance?</summary>

### Unsubscribe importance?

**Answer:** Prevents memory leaks and stale UI updates. Use `takeUntilDestroyed()`, `async` pipe, or `Subscription` cleanup in `ngOnDestroy`.

</details>

AsyncPipe errors?

<details>
  <summary>AsyncPipe errors?</summary>

### AsyncPipe errors?

**Answer:** `async` pipe does not surface errors to template — handle in stream with `catchError` or use `@if` with error state from service.

</details>

<a name="angular-animations"></a>

Angular animations?

<details>
  <summary>Angular animations?</summary>

### Angular animations?

**Answer:** `@angular/animations` triggers/states/transitions on `:enter/:leave` or class changes; import `provideAnimationsAsync()`.

</details>

`as` keyword in templates?

<details>
  <summary>`as` keyword in templates?</summary>

### `as` keyword in templates?

**Answer:** Alias in `*ngIf="user as u"` or `@if (obs | async; as value)` to reuse scoped variable.

</details>

HTTP interceptors?

<details>
  <summary>HTTP interceptors?</summary>

### HTTP interceptors?

**Answer:** Chain that mutates requests/responses — auth headers, logging, error mapping.

**See also:** [HTTP interceptors](./4_angular_ecosystem.md#http-interceptors)

</details>

SSR in Angular?

<details>
  <summary>SSR in Angular?</summary>

### SSR in Angular?

**Answer:** Server renders HTML; client hydrates. Use `@angular/ssr` — guard browser-only APIs.

**See also:** [SSR and Universal](./4_angular_platform.md#ssr-and-universal)

</details>

Angular proxy config?

<details>
  <summary>Angular proxy config?</summary>

### Angular proxy config?

**Answer:** `proxy.conf.json` + `ng serve --proxy-config` forwards API paths to backend.

**See also:** [Dev proxy](./4_angular_ecosystem.md#dev-proxy)

</details>

SOLID in Angular?

<details>
  <summary>SOLID in Angular?</summary>

### SOLID in Angular?

**Answer:** Single-responsibility components/services, inject abstractions (DIP), smart/dumb split (SRP), prefer composition over god services.

**See also:** [OOP cheatsheet](../7_oop_interview.md#oop--fp--cheatsheet)

</details>

<a name="markforcheck-vs-detectchanges"></a>

markForCheck vs detectChanges?

<details>
  <summary>markForCheck vs detectChanges?</summary>

### markForCheck vs detectChanges?

**Answer:** `markForCheck()` schedules branch for next CD; `detectChanges()` runs CD now on subtree.

**See also:** [markForCheck vs detectChanges](./4_angular_platform.md#markforcheck-vs-detectchanges)

</details>

<a name="injector-trees"></a>

Injector trees?

<details>
  <summary>Injector trees?</summary>

### Injector trees?

**Answer:** Element injectors follow component tree; environment injector for app-wide providers; lazy routes add scopes.

**See also:** [Hierarchical injectors](./4_angular_platform.md#hierarchical-injectors)

</details>

<a name="service-state-management"></a>

Service state management?

<details>
  <summary>Service state management?</summary>

### Service state management?

**Answer:** Injectable service exposing `readonly state$ = new BehaviorSubject(...)` with imperative `update()` methods — simple NgRx alternative.

</details>

Structural directive asterisk?

<details>
  <summary>Structural directive asterisk?</summary>

### Structural directive asterisk?

**Answer:** `*` prefix desugars to `<ng-template>` host — required sugar for structural directives (`*ngIf`, custom `*appUnless`).

</details>

Higher-order Observable?

<details>
  <summary>Higher-order Observable?</summary>

### Higher-order Observable?

**Answer:** Observable that emits inner Observables — flattened with `switchMap`, `mergeMap`, `concatMap`, `exhaustMap`.

</details>

<a name="angular-vs-react-and-vue"></a>

Angular vs React and Vue?

<details>
  <summary>Angular vs React and Vue?</summary>

### Angular vs React and Vue?

**Answer:** **Angular** = full platform + DI + RxJS + CLI. **React** = view library + ecosystem choices (Redux, routers). **Vue** = progressive, templates + Composition API, lighter core. Angular strongest for enterprise conventions; React/Vue for flexible stack picking.

**See also:** [Vue interview](../4_vue_interview.md) · [React platforms](../4_react_platforms.md)

</details>

Angular vs React MVC?

<details>
  <summary>Angular vs React MVC?</summary>

### Angular vs React MVC?

**Answer:** Angular ships structured layers (components, services, modules). React is view-focused — MVC/MVVM emerges via patterns (containers, hooks, state libraries), not framework mandate.

</details>

WebSocketSubject?

<details>
  <summary>WebSocketSubject?</summary>

### WebSocketSubject?

**Answer:** RxJS wrapper around WebSocket — multicasts messages as Observable stream; use in injectable service with reconnect logic.

</details>

<a name="rxjs-operators-index"></a>

RxJS operators index

<details>
  <summary>RxJS operators index</summary>

### RxJS operators index

| Operator                     | Use                                  |
| ---------------------------- | ------------------------------------ |
| `map`                        | Transform each value                 |
| `filter`                     | Skip values                          |
| `tap`                        | Side effects without changing stream |
| `catchError`                 | Recover / replace error              |
| `switchMap`                  | Cancel previous inner (search)       |
| `concatMap`                  | Queue inner sequentially             |
| `mergeMap` / `flatMap`       | Concurrent inners                    |
| `exhaustMap`                 | Ignore new outer while inner active  |
| `combineLatest`              | Latest tuple when any source emits   |
| `forkJoin`                   | Wait for all to complete once        |
| `merge`                      | Merge multiple streams               |
| `concat`                     | Append sequences                     |
| `of` / `from`                | Create from values / iterable        |
| `debounceTime`               | Quiet period before emit             |
| `distinctUntilChanged`       | Skip duplicate consecutive           |
| `share` / `shareReplay`      | Multicast hot                        |
| `startWith`                  | Seed initial value                   |
| `withLatestFrom`             | Pair with latest from other          |
| `scan`                       | Accumulator over time                |
| `takeUntil`                  | Complete when notifier fires         |
| `retry` / `retryWhen`        | Retry on error                       |
| `ajax`                       | HTTP via RxJS                        |
| `bufferTime` / `bufferCount` | Batch emissions                      |
| `window` / `windowTime`      | Split into nested streams            |
| `sequenceEqual`              | Compare two sequences                |
| `publish` / `refCount`       | Connectable observable               |

Deep dives: [JS async](../3_js_async.md) · [RxJS in Angular](./4_angular_ecosystem.md#rxjs-in-angular)

</details>

<a name="angular-interview--syllabus-blocks"></a>

### Syllabus — animation

Animation transitions, wildcard, and triggers?

<details>
  <summary>Animation transitions, wildcard, and triggers?</summary>

### Animation transitions, wildcard, and triggers?

**Answer:** **`@angular/animations`**: a **trigger** names an animation (`trigger('fade', [...])`). Each **state** (`state('open', style(...))`) defines look. A **transition** (`transition('open => closed', animate('300ms'))`) runs when state changes. **`* => *`** or **`void => *`** are **wildcard** transitions for any state change or enter/leave. Bind with `[@fade]="state"` or `:enter`/`:leave` aliases.

**See also:** [Angular animations](./4_angular_interview.md#angular-animations) · [angular.dev — animations](https://angular.dev/guide/animations)

</details>

### Syllabus — architecture

Race conditions in Angular apps?

<details>
  <summary>Race conditions in Angular apps?</summary>

### Race conditions in Angular apps?

**Answer:** A **race condition** happens when overlapping async work completes **out of order** (e.g. two HTTP searches — stale response overwrites newer UI). Mitigate with **`switchMap`** (cancel prior), **`exhaustMap`**, request **ids/tokens**, **`takeUntil`** on route change, and immutable store updates. NgRx effects should not assume last emission wins without cancellation.

**See also:** [switchMap vs concatMap vs mergeMap](#switchmap-vs-concatmap-vs-mergemap) · [RxJS index](#rxjs-operators-index)

</details>

What is a SharedModule?

<details>
  <summary>What is a SharedModule?</summary>

### What is a SharedModule?

**Answer:** A **SharedModule** exports **reusable declarations** (components, directives, pipes) and common imports (`CommonModule`, `ReactiveFormsModule`) for feature modules. It should **not** re-provide app-wide **singleton services** — that breaks lazy loading. Prefer **standalone** `imports: [SharedButton]` over giant SharedModules in new apps.

**See also:** [SharedModule anti-pattern](./4_angular_core.md#sharedmodule-anti-pattern) · [SharedModule and lazy loading](#sharedmodule-and-lazy-loading)

</details>

Signals vs NgRx / RxJS for state?

<details>
  <summary>Signals vs NgRx / RxJS for state?</summary>

### Signals vs NgRx / RxJS for state?

**Answer:** Use **signals** (`signal`, `computed`, `effect`) for **local and medium shared UI state** with fine-grained updates and simpler mental model. Use **NgRx** (or event-sourced stores) when you need **time-travel**, many actors, audit trails, or complex cross-feature reducers. **RxJS services** fit async pipelines (HTTP, websockets) — often **`toSignal()`** bridges streams into templates. Many apps mix: NgRx for domain, signals in components.

**See also:** [Angular signals](./4_angular_platform.md#angular-signals) · [When to use NgRx](#when-to-use-ngrx)

</details>

Signals architecture, migration, and anti-patterns?

<details>
  <summary>Signals architecture, migration, and anti-patterns?</summary>

### Signals architecture, migration, and anti-patterns?

**Answer:** **Layers:** component `signal`/`computed` → feature services exposing `readonly` signals → optional global store. **Migrate gradually:** keep `@Input()`/`@Output()`; add **`input()`/`output()`** on new components; replace `BehaviorSubject` UI state with signals + `toSignal()` for HTTP. **Anti-patterns:** giant `effect()` blocks with side effects, mutating signal values without `update`/`set`, duplicating NgRx in every feature, reading signals in constructors before inputs exist, bypassing OnPush benefits with impure pipes.

**See also:** [Angular signals](./4_angular_platform.md#angular-signals)

</details>

### Syllabus — API & platform

Renderer2 vs native DOM?

<details>
  <summary>Renderer2 vs native DOM?</summary>

### Renderer2 vs native DOM?

**Answer:** **`Renderer2`** (or **`ElementRef` + renderer**) keeps DOM tweaks **SSR-safe** and **Web Worker–friendly**, and avoids direct `document` access that breaks hydration. Use native DOM only when integrating third-party libs — wrap updates in **`NgZone.run()`** if they must trigger CD.

**See also:** [NgZone and Zone.js](#ngzone-and-zonejs)

</details>

Angular signals — types and template usage?

<details>
  <summary>Angular signals — types and template usage?</summary>

### Angular signals — types and template usage?

**Answer:** Core APIs: **`signal(initial)`**, **`computed(() => ...)`**, **`effect(() => ...)`** (side effects), **`input()` / `output()`** (signal-based component API). In templates, read signals with **`()`** — `{{ count() }}`, `@if (user())`. Unlike **`async` pipe**, signal reads are **synchronous** and participate in **signal-based change detection** without subscriptions. **`model()`** enables two-way signal binding.

**See also:** [Angular signals](./4_angular_platform.md#angular-signals)

</details>

### Syllabus — templates

Template binding — property vs attribute, optional brackets, @if?

<details>
  <summary>Template binding — property vs attribute, optional brackets, @if?</summary>

### Template binding — property vs attribute, optional brackets, @if?

**Answer:** **`[prop]`** sets a **DOM property** (object identity, boolean properties). **`[attr.name]`** sets an **attribute** (string in HTML). **`class` / `style` / `id`** can omit brackets when binding a **static** string. **Control flow:** `@if (cond) { } @else { }`, `@for (item of items; track item.id) { }` replace `*ngIf`/`*ngFor` — no `ng-template` sugar required.

**See also:** [Data binding types](#data-binding-types) · [Directives and projection](#directives-and-projection)

</details>

Async pipe subscribed multiple times?

<details>
  <summary>Async pipe subscribed multiple times?</summary>

### Async pipe subscribed multiple times?

**Answer:** Each **`async` pipe** creates its **own subscription** to the Observable. Using `| async` twice on the same cold Observable runs **two HTTP calls** unless the stream is **shared** (`shareReplay(1)`, `publishReplay`). Prefer **one** `async` + **`as`** alias: `@if (users$ | async; as users)`.

**See also:** [AsyncPipe errors](#asyncpipe-errors)

</details>

Active class on list item click?

<details>
  <summary>Active class on list item click?</summary>

### Active class on list item click?

**Answer:** Track **`selectedId` signal** or field; bind **`[class.active]="item.id === selectedId"`** or **`[ngClass]`** on `(click)="select(item.id)"`. With **`@for`**, use **`track`** for stable DOM.

**See also:** [Data binding types](#data-binding-types)

</details>

### Syllabus — components

Component minimum requirements and vs directive?

<details>
  <summary>Component minimum requirements and vs directive?</summary>

### Component minimum requirements and vs directive?

**Answer:** A **component** needs `@Component({ selector, template or templateUrl })` and a class. **Directives** attach behavior without a full template — **structural** (`*ngIf`) or **attribute** (`ngClass`). Components **own a view**; directives **modify** an existing element or `ng-template`.

**See also:** [Angular overview](./4_angular_core.md#angular-overview) · [Directives](./4_angular_ecosystem.md#directives)

</details>

Standalone components?

<details>
  <summary>Standalone components?</summary>

### Standalone components?

**Answer:** **`standalone: true`** components declare **`imports: [...]`** directly (other standalone components, `RouterModule`, `ReactiveFormsModule`) — no NgModule wrapper. Bootstrap with **`bootstrapApplication(AppComponent, { providers: [...] })`**. Default in modern CLI schematics.

**See also:** [Angular overview](./4_angular_core.md#angular-overview)

</details>

Parent ↔ child data and EventEmitter scope?

<details>
  <summary>Parent ↔ child data and EventEmitter scope?</summary>

### Parent ↔ child data and EventEmitter scope?

**Answer:** **Down:** `@Input()` / `input()`. **Up:** `@Output() EventEmitter` / `output()` — only **parent** template `(saved)="onSaved($event)"` receives emits (siblings do not). **Two-way:** `[(ngModel)]` or **`model()`** signal. **Cross-cutting:** shared service or store.

**See also:** [Child to parent data](#child-to-parent-data) · [Data binding types](#data-binding-types)

</details>

Cache HTTP data in Angular?

<details>
  <summary>Cache HTTP data in Angular?</summary>

### Cache HTTP data in Angular?

**Answer:** Service with **`shareReplay({ bufferSize: 1, refCount: true })`**, **`@ngrx/component-store`**, or NgRx **entity cache**. Invalidate on param change via **`switchMap`**. For route-level reuse, **`RouteReuseStrategy`** or resolver cache.

**See also:** [Storing data in Angular](#storing-data-in-angular) · [HttpClient](./4_angular_ecosystem.md#httpclient)

</details>

### Syllabus — forms

FormBuilder, submit, validation states, errors in template?

<details>
  <summary>FormBuilder, submit, validation states, errors in template?</summary>

### FormBuilder, submit, validation states, errors in template?

**Answer:** **`FormBuilder.group({ name: ['', Validators.required] })`** reduces boilerplate. **Submit:** `(ngSubmit)="save()"` on `<form [formGroup]="form">` — call **`form.valid`** / **`markAllAsTouched()`** before POST. **States:** **`pristine`/`dirty`** (value changed), **`touched`/`untouched`** (focused/blurred). Show errors: `@if (form.get('email')?.invalid && form.get('email')?.touched) { ... }` or `*ngIf` on **`errors?.['required']`**.

**See also:** [Template-driven vs reactive forms](#template-driven-vs-reactive-forms) · [NgForm vs FormGroup vs FormControl](#ngform-formgroup-formcontrol) · [Forms](./4_angular_ecosystem.md#forms)

</details>

### Syllabus — NgModule

NgModule purpose, forRoot/forChild, providedIn, exports?

<details>
  <summary>NgModule purpose, forRoot/forChild, providedIn, exports?</summary>

### NgModule purpose, forRoot/forChild, providedIn, exports?

**Answer:** **NgModule** groups declarations and DI scope in **module-based** apps (legacy default). Create a **feature module** when a lazy area shares declarations/providers. **`forRoot`** registers **singleton** providers once (Router, Store); **`forChild`** adds routes/providers per lazy feature without duplicating root tokens. **`providedIn: 'root'`** tree-shakable app-wide service. **`exports`** re-exports declarations so importing modules can use your components/pipes. **App-wide service:** `providedIn: 'root'` or **CoreModule** imported once in `AppModule`.

**See also:** [declarations, providers, imports](#declarations-providers-imports) · [Routing and RouterModule](#routing-and-routermodule)

</details>

### Syllabus — RxJS

Observable vs Promise, Subject, caching, scan vs reduce, multicasting?

<details>
  <summary>Observable vs Promise, Subject, caching, scan vs reduce, multicasting?</summary>

### Observable vs Promise, Subject, caching, scan vs reduce, multicasting?

**Answer:** **Promise** = single async value, not cancellable by default. **Observable** = stream of 0..n values, lazy until subscribe, composable with operators. **Subject** = hot multicast hub; **BehaviorSubject** remembers last value; **ReplaySubject** buffers N; **AsyncSubject** emits last on complete. **Cache:** `shareReplay(1)`. **Sequential API:** `concatMap` or `switchMap` chain. **`scan`** accumulates over time (like reduce but emits each step); **`reduce`** emits final value once. **Multicasting:** `share`, `shareReplay`, `Subject` — one subscription to source, many observers.

**See also:** [BehaviorSubject vs Observable](#behaviorsubject-vs-observable) · [RxJS operators index](#rxjs-operators-index)

</details>

combineLatest vs forkJoin and zip?

<details>
  <summary>combineLatest vs forkJoin and zip?</summary>

### combineLatest vs forkJoin and zip?

**Answer:** **`combineLatest`** emits whenever **any** source emits (needs initial values). **`forkJoin`** waits until **all complete**, emits **last values once** (good for parallel HTTP then join). **`zip`** pairs **nth** emissions across sources — slowest source paces output.

**See also:** [RxJS operators index](#rxjs-operators-index)

</details>

### Syllabus — performance

Detach CD, runOutsideAngular, Web Workers, lazy load, preload?

<details>
  <summary>Detach CD, runOutsideAngular, Web Workers, lazy load, preload?</summary>

### Detach CD, runOutsideAngular, Web Workers, lazy load, preload?

**Answer:** **`ChangeDetectorRef.detach()`** stops CD on a subtree until **`reattach()`/`detectChanges()`**. **`NgZone.runOutsideAngular`** for high-frequency events (mousemove) — re-enter with **`run()`** when updating bindings. **Web Workers** run CPU work off main thread; postMessage back into **`NgZone.run()`**. **Lazy loading:** `loadChildren` / `loadComponent` splits bundles. **Preload:** `PreloadAllModules` or custom **`PreloadingStrategy`** after bootstrap.

**See also:** [Improve Angular performance](#improve-angular-performance) · [Preloading strategies](./4_angular_ecosystem.md#preloading-strategies) · [NgZone and Zone.js](#ngzone-and-zonejs)

</details>

### Syllabus — pipes

Pipe basics, async pipe, custom pipe?

<details>
  <summary>Pipe basics, async pipe, custom pipe?</summary>

### Pipe basics, async pipe, custom pipe?

**Answer:** **Pipes** transform template expressions (`{{ price | currency }}`). **`async` pipe** subscribes to **Observable or Promise**, marks component for check, **unsubscribes on destroy** — prevents leaks. **Custom:** `@Pipe({ name: 'truncate', pure: true })` class with `transform`. **Pure vs impure:** see [Pure vs impure pipes](#pure-vs-impure-pipes).

**See also:** [Pure vs impure pipes](#pure-vs-impure-pipes)

</details>

### Syllabus — router

loadChildren, ActivatedRoute vs RouterState, guard types, RouterOutlet?

<details>
  <summary>loadChildren, ActivatedRoute vs RouterState, guard types, RouterOutlet?</summary>

### loadChildren, ActivatedRoute vs RouterState, guard types, RouterOutlet?

**Answer:** **`loadChildren`** (or **`loadComponent`**) lazy-loads route config. Lazy module loads **when route first matches** (after guard checks). **`ActivatedRoute`** = snapshot + observables for **current route** params/data; **`Router.routerState`** = tree of **`RouterStateSnapshot`**. **Guards:** **`canActivate`/`canActivateChild`** (navigation), **`canDeactivate`** (leave), **`canMatch`/`canLoad`** (lazy load permission). **`<router-outlet>`** hosts routed components.

**See also:** [Routing and RouterModule](#routing-and-routermodule) · [Protect routed components](#protect-routed-components) · [Route guards](./4_angular_ecosystem.md#route-guards)

</details>

### Syllabus — services, directives, styles, testing

Services — singleton, lifecycle, providers?

<details>
  <summary>Services — singleton, lifecycle, providers?</summary>

### Services — singleton, lifecycle, providers?

**Answer:** **Services** hold reusable logic/state. **`providedIn: 'root'`** = app singleton (default). **Component/route providers** = new instance per injector scope. Lifecycle matches injector — destroyed with owning component/route unless root.

**See also:** [Inject a service](#inject-a-service) · [Injector trees](#injector-trees)

</details>

Structural directives — identify and ngIf + ngFor?

<details>
  <summary>Structural directives — identify and ngIf + ngFor?</summary>

### Structural directives — identify and ngIf + ngFor?

**Answer:** Structural directives prefix **`*`** (desugars to `<ng-template>`). **Cannot** put `*ngIf` and `*ngFor` on the **same element** — wrap with **`ng-container`**. Prefer **`@if` / `@for`** control flow on one element.

**See also:** [Structural directive asterisk](#structural-directive-asterisk) · [Directives and projection](#directives-and-projection)

</details>

Component styling — :host, :host-context, piercing?

<details>
  <summary>Component styling — :host, :host-context, piercing?</summary>

### Component styling — :host, :host-context, piercing?

**Answer:** **`:host`** styles the component element. **`:host-context(.theme-dark)`** applies when **ancestor** has class (deprecated pattern — prefer CSS variables). **`::ng-deep`** (deprecated) pierced encapsulation — avoid; use **`ViewEncapsulation.None`** or global styles intentionally. **`::ng-deep` replacement:** host CSS variables or `:host ::ng-deep` sparingly.

**See also:** [View encapsulation modes](#view-encapsulation-modes)

</details>

Angular style guide and testing basics?

<details>
  <summary>Angular style guide and testing basics?</summary>

### Angular style guide and testing basics?

**Answer:** Follow [angular.dev style guide](https://angular.dev/style-guide): one component per file, **`OnPush` where possible**, **`providedIn: 'root'`**, feature folders, avoid logic in templates. **Testing:** **unit** (TestBed + Jasmine/Jest), **integration**, **e2e** (Playwright/Cypress). **Coverage** = lines/branches executed in tests. **Spy** = Jasmine `jasmine.createSpyObj` or Jest mocks for dependencies.

**See also:** [7_special_testing_interview.md](../7_special_testing_interview.md)

</details>

TypeScript topics on Angular interviews?

<details>
  <summary>TypeScript topics on Angular interviews?</summary>

### TypeScript topics on Angular interviews?

**Answer:** Custom types (`type`/`interface`), **interface vs class** (shape vs runtime), **discriminated unions** for `@Input()` variants, **generics** in services/components, **decorators** for Angular metadata — covered in TypeScript conspect.

**See also:** [5_ts_interview.md](../5_ts_interview.md#typescript-interview--course-index) · [TS decorators](../5_ts_decorators.md)

</details>

<a name="angular-interview--course-index"></a>

<details>
<summary>Angular interview — course index (RU syllabus)</summary>

### Angular interview — course index (RU syllabus)

**«Вопросы с ответами для собеседования по Angular»** — each row links **Theory** (handbook) and **Interview** (checklist / syllabus blocks). Search EN summaries with `Cmd+F` when the RU wording differs.

#### Анимация

| RU topic (syllabus)             | Theory                                                           | Interview                                                                             |
| ------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| Переход между двумя состояниями | [angular.dev — animations](https://angular.dev/guide/animations) | [Animation transitions, wildcard, and triggers?](#angular-interview--syllabus-blocks) |
| Состояние wildcard              | same                                                             | same                                                                                  |
| Триггер анимации                | same                                                             | same · [#angular-animations](#angular-animations)                                     |

#### Архитектура

| RU topic                        | Theory                                                                            | Interview                                                                                  |
| ------------------------------- | --------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| Когда использовать NgRx/store   | [When to use NgRx](./4_angular_ecosystem.md#when-to-use-ngrx)                     | [When to use NgRx?](#when-to-use-ngrx)                                                     |
| Race condition                  | [RxJS in Angular](./4_angular_ecosystem.md#rxjs-in-angular)                       | [Race conditions in Angular apps?](#angular-interview--syllabus-blocks)                    |
| Smart vs presentational         | [Smart vs presentational](./4_angular_core.md#smart-vs-presentational-components) | [Smart vs presentational?](#smart-vs-presentational)                                       |
| Shared модуль                   | [SharedModule anti-pattern](./4_angular_core.md#sharedmodule-anti-pattern)        | [What is a SharedModule?](#angular-interview--syllabus-blocks)                             |
| Signals vs NgRx/RxJS            | [Angular signals](./4_angular_platform.md#angular-signals)                        | [Signals vs NgRx / RxJS for state?](#angular-interview--syllabus-blocks)                   |
| Слои приложения на Signals      | [Angular signals](./4_angular_platform.md#angular-signals)                        | [Signals architecture, migration, and anti-patterns?](#angular-interview--syllabus-blocks) |
| Миграция Input/Output → Signals | [Angular signals](./4_angular_platform.md#angular-signals)                        | same block                                                                                 |
| Анти‑паттерны Signals           | same                                                                              | same block                                                                                 |

#### API

| RU topic                         | Theory                                                       | Interview                                                                             |
| -------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------------------------------- |
| Renderer vs native JS            | [NgZone](./4_angular_platform.md#ngzone)                     | [Renderer2 vs native DOM?](#angular-interview--syllabus-blocks)                       |
| Resize при изменении окна        | [Core — binding](./4_angular_core.md#data-binding)           | HostListener / `ResizeObserver` — [Improve performance](#improve-angular-performance) |
| Пример NgZone                    | [NgZone](./4_angular_platform.md#ngzone)                     | [NgZone and Zone.js?](#ngzone-and-zonejs)                                             |
| Защита компонента через роутер   | [Route guards](./4_angular_ecosystem.md#route-guards)        | [Protect routed components?](#protect-routed-components)                              |
| ViewChild vs ContentChild        | [Core](./4_angular_core.md#viewchild-and-contentchild)       | [ViewChild vs ContentChild?](#viewchild-vs-contentchild)                              |
| Dependency Injection             | [DI](./4_angular_platform.md#dependency-injection)           | [Dependency injection?](#dependency-injection)                                        |
| constructor vs ngOnInit          | [Core](./4_angular_core.md#constructor-vs-ngoninit)          | [#constructor-vs-ngoninit](#constructor-vs-ngoninit)                                  |
| Service worker                   | [Service workers](./4_angular_ecosystem.md#service-workers)  | [Service workers in Angular?](#service-workers-in-angular)                            |
| zone.js                          | [Zone.js](./4_angular_platform.md#zonejs)                    | [NgZone and Zone.js?](#ngzone-and-zonejs)                                             |
| Angular Signals (типы)           | [Angular signals](./4_angular_platform.md#angular-signals)   | [Angular signals — types and template usage?](#angular-interview--syllabus-blocks)    |
| signals вместо Input/Output      | same                                                         | same · [Parent ↔ child](#angular-interview--syllabus-blocks)                          |
| Signals в шаблоне vs Observable  | same                                                         | same                                                                                  |
| Signals vs Observable (CD, perf) | [Change detection](./4_angular_platform.md#change-detection) | same · [Change detection?](#change-detection)                                         |

#### Шаблоны (template)

| RU topic                                | Theory                                                                         | Interview                                                                        |
| --------------------------------------- | ------------------------------------------------------------------------------ | -------------------------------------------------------------------------------- |
| Класс active при клике                  | [Data binding](./4_angular_core.md#data-binding)                               | [Active class on list item click?](#angular-interview--syllabus-blocks)          |
| Template variable                       | [Core](./4_angular_core.md#data-binding)                                       | [#template-reference-variables](#template-reference-variables)                   |
| Несколько async pipe на поток           | [RxJS in Angular](./4_angular_ecosystem.md#rxjs-in-angular)                    | [Async pipe subscribed multiple times?](#angular-interview--syllabus-blocks)     |
| ng-content / ng-container / ng-template | [Projection](./4_angular_ecosystem.md#projection-primitives)                   | [Directives and projection?](#directives-and-projection)                         |
| Property vs attribute binding           | [Data binding](./4_angular_core.md#data-binding)                               | [Template binding — property vs attribute…](#angular-interview--syllabus-blocks) |
| Байндинг без скобок                     | same                                                                           | same                                                                             |
| new `@if` syntax                        | [angular.dev — control flow](https://angular.dev/guide/templates/control-flow) | same block                                                                       |

#### Компоненты

| RU topic                            | Theory                                                                            | Interview                                                              |
| ----------------------------------- | --------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Минимальные требования к компоненту | [Angular overview](./4_angular_core.md#angular-overview)                          | [Component minimum requirements…](#angular-interview--syllabus-blocks) |
| Компонент vs директива              | [Directives](./4_angular_ecosystem.md#directives)                                 | same block                                                             |
| Взаимодействие компонентов          | [Smart vs presentational](./4_angular_core.md#smart-vs-presentational-components) | [Parent ↔ child data…](#angular-interview--syllabus-blocks)            |
| Двухстороннее связывание            | [Data binding](./4_angular_core.md#data-binding)                                  | same · [Data binding types?](#data-binding-types)                      |
| Компонент ошибок                    | [Forms / UX patterns](./4_angular_ecosystem.md#forms)                             | [Error display component?](#error-display-component)                   |
| Lifecycle Hooks                     | [Component lifecycle](./4_angular_core.md#component-lifecycle)                    | [Component lifecycle hooks?](#component-lifecycle-hooks)               |
| View Encapsulation                  | [Core](./4_angular_core.md#view-encapsulation)                                    | [View encapsulation modes?](#view-encapsulation-modes)                 |
| Динамические компоненты             | [Dynamic components](./4_angular_ecosystem.md#dynamic-components)                 | [Dynamic components?](#dynamic-components)                             |
| Standalone компоненты               | [Angular overview](./4_angular_core.md#angular-overview)                          | [Standalone components?](#angular-interview--syllabus-blocks)          |

#### Компоненты и state

| RU topic           | Theory                                            | Interview                                                          |
| ------------------ | ------------------------------------------------- | ------------------------------------------------------------------ |
| Родитель → ребёнок | [Data binding](./4_angular_core.md#data-binding)  | [Parent ↔ child data…](#angular-interview--syllabus-blocks)        |
| Ребёнок → родитель | same                                              | [Child to parent data?](#child-to-parent-data)                     |
| Кто получает emit  | same                                              | same block                                                         |
| Кэш данных         | [HttpClient](./4_angular_ecosystem.md#httpclient) | [Cache HTTP data in Angular?](#angular-interview--syllabus-blocks) |

#### Формы

| RU topic                         | Theory                                  | Interview                                                                |
| -------------------------------- | --------------------------------------- | ------------------------------------------------------------------------ |
| Template-driven vs reactive      | [Forms](./4_angular_ecosystem.md#forms) | [Template-driven vs reactive forms?](#template-driven-vs-reactive-forms) |
| Отправка формы                   | same                                    | [FormBuilder, submit, validation…](#angular-interview--syllabus-blocks)  |
| NgForm / FormGroup / FormControl | same                                    | [#ngform-formgroup-formcontrol](#ngform-formgroup-formcontrol)           |
| FormBuilder                      | same                                    | syllabus forms block                                                     |
| Валидация + FormBuilder          | same                                    | same                                                                     |
| dirty / touched / pristine       | same                                    | same                                                                     |
| Ошибки в шаблоне                 | same                                    | same                                                                     |

#### ngModule

| RU topic                   | Theory                                                                     | Interview                                                          |
| -------------------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| Зачем NgModule             | [Component vs NgModule](./4_angular_core.md#component-vs-ngmodule)         | [NgModule purpose…](#angular-interview--syllabus-blocks)           |
| Когда новый NgModule       | same                                                                       | same                                                               |
| forRoot / forChild         | [Routing](./4_angular_ecosystem.md#routing)                                | [Routing and RouterModule?](#routing-and-routermodule)             |
| providedIn                 | [DI](./4_angular_platform.md#dependency-injection)                         | syllabus NgModule block                                            |
| Что в shared / не в shared | [SharedModule anti-pattern](./4_angular_core.md#sharedmodule-anti-pattern) | same                                                               |
| Сервис на всё приложение   | [DI](./4_angular_platform.md#dependency-injection)                         | syllabus NgModule block                                            |
| exports                    | [Modules](./4_angular_core.md#component-vs-ngmodule)                       | [#declarations-providers-imports](#declarations-providers-imports) |
| SharedModule в lazy load   | [SharedModule anti-pattern](./4_angular_core.md#sharedmodule-anti-pattern) | [#sharedmodule-and-lazy-loading](#sharedmodule-and-lazy-loading)   |

#### RxJS

| RU topic                         | Theory                                                                                     | Interview                                                                  |
| -------------------------------- | ------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------- |
| Observable vs Promise            | [RxJS in Angular](./4_angular_ecosystem.md#rxjs-in-angular) · [JS async](../3_js_async.md) | [Observable vs Promise…](#angular-interview--syllabus-blocks)              |
| Observable vs Subject            | [Subjects](./4_angular_ecosystem.md#subjects-comparison)                                   | same · [BehaviorSubject vs Observable?](#behaviorsubject-vs-observable)    |
| Кэш observable                   | same                                                                                       | syllabus RxJS block                                                        |
| Последовательные API запросы     | same                                                                                       | [switchMap vs concatMap vs mergeMap?](#switchmap-vs-concatmap-vs-mergemap) |
| switchMap / concatMap / mergeMap | same                                                                                       | same                                                                       |
| scan vs reduce                   | [RxJS index](#rxjs-operators-index)                                                        | syllabus RxJS block                                                        |
| Subject                          | [Subjects comparison](./4_angular_ecosystem.md#subjects-comparison)                        | same                                                                       |
| Behavior / Replay / AsyncSubject | same                                                                                       | same                                                                       |
| Higher-Order Observable          | same                                                                                       | [Higher-order Observable?](#higher-order-observable)                       |
| of vs from                       | [RxJS index](#rxjs-operators-index)                                                        | table in index                                                             |
| Multicasting                     | same                                                                                       | syllabus RxJS block                                                        |
| Stream в RxJS                    | same                                                                                       | same                                                                       |
| combineLatest vs forkJoin        | same                                                                                       | [combineLatest vs forkJoin and zip?](#angular-interview--syllabus-blocks)  |
| Обработка ошибок                 | same                                                                                       | [RxJS error handling?](#rxjs-error-handling)                               |
| zip                              | [RxJS index](#rxjs-operators-index)                                                        | combineLatest / zip block                                                  |

#### Производительность

| RU topic                    | Theory                                                                  | Interview                                                            |
| --------------------------- | ----------------------------------------------------------------------- | -------------------------------------------------------------------- |
| Улучшить производительность | [Performance](./4_angular_ecosystem.md#performance)                     | [Improve Angular performance?](#improve-angular-performance)         |
| Change Detection            | [Change detection](./4_angular_platform.md#change-detection)            | [Change detection?](#change-detection)                               |
| OnPush                      | [OnPush](./4_angular_platform.md#onpush-strategy)                       | checklist · CD block                                                 |
| Отключение CD               | [Change detection](./4_angular_platform.md#change-detection)            | [Detach CD, runOutsideAngular…](#angular-interview--syllabus-blocks) |
| Local change detection      | same                                                                    | same                                                                 |
| Запуск вне Angular          | [NgZone](./4_angular_platform.md#ngzone)                                | same                                                                 |
| trackBy \*ngFor             | [trackBy](./4_angular_ecosystem.md#trackby-with-ngfor)                  | [ngFor trackBy?](#ngfor-trackby)                                     |
| Web Workers                 | [Performance](./4_angular_ecosystem.md#performance)                     | syllabus performance block                                           |
| Lazy Loading                | [Routing](./4_angular_ecosystem.md#routing)                             | syllabus performance / router blocks                                 |
| Preloading strategies       | [Preloading strategies](./4_angular_ecosystem.md#preloading-strategies) | syllabus performance block                                           |
| zone.js (повтор)            | [Zone.js](./4_angular_platform.md#zonejs)                               | [NgZone and Zone.js?](#ngzone-and-zonejs)                            |

#### Пайпы

| RU topic                   | Theory                                                      | Interview                                                       |
| -------------------------- | ----------------------------------------------------------- | --------------------------------------------------------------- |
| Что такое pipe             | [angular.dev — pipes](https://angular.dev/guide/pipes)      | [Pipe basics, async pipe…](#angular-interview--syllabus-blocks) |
| async pipe                 | [RxJS in Angular](./4_angular_ecosystem.md#rxjs-in-angular) | same                                                            |
| Данные для async pipe      | same                                                        | same                                                            |
| Кастомный pipe             | same                                                        | same                                                            |
| async pipe и утечки памяти | same                                                        | same · [Unsubscribe importance?](#unsubscribe-importance)       |
| Чистые vs нечистые         | [Performance](./4_angular_ecosystem.md#performance)         | [Pure vs impure pipes?](#pure-vs-impure-pipes)                  |

#### Роутер

| RU topic                                | Theory                                                | Interview                                                                        |
| --------------------------------------- | ----------------------------------------------------- | -------------------------------------------------------------------------------- |
| forRoot vs forChild                     | [Routing](./4_angular_ecosystem.md#routing)           | [Routing and RouterModule?](#routing-and-routermodule)                           |
| loadChildren                            | same                                                  | [loadChildren, ActivatedRoute…](#angular-interview--syllabus-blocks)             |
| Отдельный Routing Module                | same                                                  | same                                                                             |
| Когда грузится lazy module              | same                                                  | same                                                                             |
| ActivatedRoute vs RouterState           | same                                                  | same                                                                             |
| Guards                                  | [Route guards](./4_angular_ecosystem.md#route-guards) | [Protect routed components?](#protect-routed-components) · syllabus router block |
| RouterOutlet                            | [Routing](./4_angular_ecosystem.md#routing)           | syllabus router block                                                            |
| CanActivate / Child / Deactivate / Load | [Route guards](./4_angular_ecosystem.md#route-guards) | syllabus router block                                                            |

#### Сервисы

| RU topic                   | Theory                                                           | Interview                                                                 |
| -------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------- |
| Зачем сервисы              | [DI](./4_angular_platform.md#dependency-injection)               | [Inject a service?](#inject-a-service)                                    |
| Внедрение сервисов         | same                                                             | [Dependency injection?](#dependency-injection)                            |
| Singleton                  | same                                                             | [Services — singleton…](#angular-interview--syllabus-blocks)              |
| Жизненный цикл             | [Injector trees](./4_angular_platform.md#hierarchical-injectors) | same block                                                                |
| providers                  | same                                                             | same · [#declarations-providers-imports](#declarations-providers-imports) |
| Использование в компоненте | same                                                             | [Inject a service?](#inject-a-service)                                    |

#### Структурные директивы

| RU topic                       | Theory                                            | Interview                                                        |
| ------------------------------ | ------------------------------------------------- | ---------------------------------------------------------------- |
| Структурная директива          | [Directives](./4_angular_ecosystem.md#directives) | [Structural directives…](#angular-interview--syllabus-blocks)    |
| Как узнать в шаблоне           | same                                              | [Structural directive asterisk?](#structural-directive-asterisk) |
| ngFor + ngIf на одном элементе | same                                              | syllabus structural block                                        |

#### Стили

| RU topic               | Theory                                                       | Interview                                                         |
| ---------------------- | ------------------------------------------------------------ | ----------------------------------------------------------------- |
| Стилизация компонента  | [View encapsulation](./4_angular_core.md#view-encapsulation) | [Component styling — :host…](#angular-interview--syllabus-blocks) |
| Псевдокласс родителя   | same                                                         | same                                                              |
| Дочерние компоненты    | same                                                         | same                                                              |
| Класс у предка до root | same                                                         | same                                                              |

#### Style guide

| RU topic                 | Theory                                                     | Interview                                                                      |
| ------------------------ | ---------------------------------------------------------- | ------------------------------------------------------------------------------ |
| Рекомендации Style Guide | [angular.dev style guide](https://angular.dev/style-guide) | [Angular style guide and testing basics?](#angular-interview--syllabus-blocks) |
| Зачем следовать          | same                                                       | same                                                                           |

#### Тесты

| RU topic          | Theory                                                 | Interview                   |
| ----------------- | ------------------------------------------------------ | --------------------------- |
| Виды тестирования | [Testing interview](../7_special_testing_interview.md) | style guide / testing block |
| Code coverage     | same                                                   | same                        |
| Spy object        | same                                                   | same                        |

#### TypeScript (в блоке Angular)

| RU topic            | Theory                                                    | Interview                                                                        |
| ------------------- | --------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Кастомный тип       | [5_ts_types.md](../5_ts_types.md)                         | [TypeScript topics on Angular interviews?](#angular-interview--syllabus-blocks)  |
| Interface vs Class  | [5_ts_classes.md](../5_ts_classes.md)                     | same · [5_ts_interview](../5_ts_interview.md#typescript-interview--course-index) |
| Discriminated union | [5_ts_type_manipulation.md](../5_ts_type_manipulation.md) | same                                                                             |
| Generics            | [5_ts_type_manipulation.md](../5_ts_type_manipulation.md) | same                                                                             |
| Decorators          | [5_ts_decorators.md](../5_ts_decorators.md)               | [TypeScript decorators?](#typescript-decorators)                                 |

</details>

<a name="ru-cheatsheet-index"></a>

<details>
<summary>RU cheatsheet — compact index</summary>

### RU cheatsheet — compact index

Legacy **1–117** rollup plus the structured **«Собеседование по Angular»** syllabus. Prefer [#angular-interview--course-index](#angular-interview--course-index) for **Theory | Interview** per topic.

| Section (syllabus)                          | Interview home                                                                                          | Theory hub                                                                |
| ------------------------------------------- | ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| Анимация · Архитектура · API · Шаблоны      | [#angular-interview--course-index](#angular-interview--course-index)                                    | [Core](./4_angular_core.md) · [Platform](./4_angular_platform.md)         |
| Компоненты · State · Формы · NgModule       | [Syllabus Q&A blocks](#angular-interview--syllabus-blocks) · [Checklist](#angular--interview-checklist) | [Ecosystem](./4_angular_ecosystem.md)                                     |
| RxJS · Performance · Pipes · Router         | [RxJS operators index](#rxjs-operators-index)                                                           | [RxJS in Angular](./4_angular_ecosystem.md#rxjs-in-angular)               |
| Services · Directives · Styles · Tests · TS | [Checklist](#angular--interview-checklist)                                                              | [5_ts_interview](../5_ts_interview.md#typescript-interview--course-index) |

<details>
<summary>Legacy rollup (#1–117)</summary>

| #     | RU question (short)  | Theory                                           | Interview                                                                                           |
| ----- | -------------------- | ------------------------------------------------ | --------------------------------------------------------------------------------------------------- |
| 1     | Что такое Angular?   | [Overview](./4_angular_core.md#angular-overview) | [Checklist — What is Angular?](#angular--interview-checklist)                                       |
| 2     | AngularJS vs Angular | [Core](./4_angular_core.md#angularjs-vs-angular) | [Checklist](#angular--interview-checklist)                                                          |
| 3–117 | (see legacy groups)  | [Course index](#angular-interview--course-index) | [Checklist](#angular--interview-checklist) · [Syllabus blocks](#angular-interview--syllabus-blocks) |

</details>

</details>

## Sources

<details>
  <summary>Sources</summary>

**Sources:**

- [angular.dev](https://angular.dev/)
- [rxjs.dev](https://rxjs.dev/)

</details>
