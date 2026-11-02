# Angular — Core

Platform model, components, templates, lifecycle, and data binding.

[← Back to hub](./4_angular.md)

### Related docs

- [Angular — Core](./4_angular_core.md) · [Angular — Platform](./4_angular_platform.md) · [Angular — Ecosystem](./4_angular_ecosystem.md) · [Interview](./4_angular_interview.md#angular--interview-checklist) · [Course index](./4_angular_interview.md#angular-interview--course-index)

### Sections

- [1. Platform overview](#1-platform-overview)
- [2. Components & lifecycle](#2-components-lifecycle)
- [3. Templates & binding](#3-templates-binding)
- [4. Modules & architecture](#4-modules-architecture)

---

## 1. Platform overview

Angular overview

<details>
  <summary>Angular overview</summary>

### Angular overview

**Definition:** **Angular** (2+) is a TypeScript **platform** for building web apps: components + templates, dependency injection, RxJS for async streams, and a CLI for generate/build/test. It targets **SPAs** and supports SSR via Angular Universal.

**How it works:** `bootstrapApplication()` (standalone) or `AppModule` bootstraps the root component. The compiler turns templates into type-checked instructions; change detection keeps the view in sync with component state.

**When to use:** Large enterprise SPAs, teams wanting **opinionated structure**, strong typing, and batteries-included routing/forms/HTTP.

**See also:** [AngularJS vs Angular](#angularjs-vs-angular) · [Interview — pros/cons](./4_angular_interview.md#angular--interview-checklist)

**Sources:**

[angular.dev](https://angular.dev/)

</details>

AngularJS vs Angular

<details>
  <summary>AngularJS vs Angular</summary>

### AngularJS vs Angular

**Definition:** **AngularJS (1.x)** is the legacy JS framework (`$scope`, two-way binding, controllers). **Angular (2+)** is a **rewrite** on TypeScript with components, unidirectional data flow by default, and a different change-detection engine.

**How it works:** Migration paths exist (`@angular/upgrade`), but greenfield work uses Angular 17+ **standalone APIs** and the `angular.dev` docs — not AngularJS.

**Sources:**

[angular.dev](https://angular.dev/)

</details>

## 2. Components & lifecycle

Component lifecycle

<details>
  <summary>Component lifecycle</summary>

### Component lifecycle

**Definition:** Angular invokes **lifecycle hooks** on components and directives as they are created, checked, and destroyed.

**How it works:**

| Hook                                           | When                                                                  |
| ---------------------------------------------- | --------------------------------------------------------------------- |
| `constructor`                                  | Class instance created; inject dependencies only — inputs not set yet |
| `ngOnChanges`                                  | `@Input()` values change (first time included)                        |
| `ngOnInit`                                     | After first `ngOnChanges`; one-time init                              |
| `ngDoCheck`                                    | Every change-detection run; custom diffing                            |
| `ngAfterContentInit` / `ngAfterContentChecked` | Projected content (`ng-content`)                                      |
| `ngAfterViewInit` / `ngAfterViewChecked`       | Component view ready / checked                                        |
| `ngOnDestroy`                                  | Teardown — unsubscribe, clear timers                                  |

**Thoughts:** Prefer **`ngOnInit`** for init that needs `@Input()`; keep **`constructor`** for DI only.

**See also:** [constructor vs ngOnInit](./4_angular_interview.md#constructor-vs-ngoninit) · [Platform — change detection](./4_angular_platform.md#change-detection)

</details>

constructor vs ngOnInit

<details>
  <summary>constructor vs ngOnInit</summary>

### constructor vs ngOnInit

**Definition:** The **constructor** runs when the class is instantiated. **`ngOnInit`** runs after Angular has set initial `@Input()` bindings.

**How it works:** Use the constructor for **dependency injection** only. Fetch data, read inputs, or touch the view in **`ngOnInit`** (or later view hooks).

</details>

Component vs directive

<details>
  <summary>Component vs directive</summary>

### Component vs directive

**Definition:** A **component** owns a template (view + logic). A **directive** changes DOM behavior or structure without a full template (attribute or structural).

**How it works:** Components have `@Component({ selector, template })`. **Structural** directives (`*ngIf`, `*ngFor`) alter DOM trees; **attribute** directives (`ngClass`, custom) alter element behavior/appearance.

**See also:** [Structural directives](./4_angular_ecosystem.md#directives)

</details>

Component vs NgModule

<details>
  <summary>Component vs NgModule</summary>

### Component vs NgModule

**Definition:** **Components** declare UI. **NgModules** (legacy packaging) group declarations, imports, providers, and bootstrap targets.

**How it works:** Modern apps use **`bootstrapApplication`** + **`importProvidersFrom`** / route-level providers. NgModules remain for lazy chunks and older codebases.

**See also:** [declarations vs imports vs providers](./4_angular_interview.md#declarations-providers-imports)

</details>

## 3. Templates & binding

Data binding

<details>
  <summary>Data binding</summary>

### Data binding

**Definition:** **Data binding** connects component class state to the template and back.

**How it works:**

| Syntax                                      | Direction                        |
| ------------------------------------------- | -------------------------------- |
| `{{ expr }}`                                | Interpolation (one-way to view)  |
| `[prop]="expr"`                             | Property binding (class → view)  |
| `(event)="handler"`                         | Event binding (view → class)     |
| `[(ngModel)]="field"`                       | Two-way (requires `FormsModule`) |
| `[class.x]="flag"` / `[style.width.px]="n"` | Class / style binding            |

**See also:** [Interpolation](./4_angular_interview.md#interpolation)

</details>

Interpolation

<details>
  <summary>Interpolation</summary>

### Interpolation

**Definition:** **`{{ expression }}`** evaluates an expression and inserts the result into the DOM text.

**How it works:** Angular updates interpolations during change detection when bound values change. Avoid heavy work in templates — use `computed`/`pipe` patterns instead.

</details>

ViewChild and ContentChild

<details>
  <summary>ViewChild and ContentChild</summary>

### ViewChild and ContentChild

**Definition:** **`@ViewChild`** queries elements/components in the **component template**. **`@ContentChild`** queries **projected** content inside `<ng-content>`.

**How it works:** Queries resolve after the view initializes (`ngAfterViewInit` / `ngAfterContentInit`). `{ static: true }` allows access in `ngOnInit` when the target is always present.

**See also:** [template reference variables](./4_angular_interview.md#template-reference-variables)

</details>

View encapsulation

<details>
  <summary>View encapsulation</summary>

### View encapsulation

**Definition:** **View encapsulation** scopes component styles to the component's template.

**How it works:** Default **`Emulated`** adds unique attributes to template nodes and CSS. **`ShadowDom`** uses native shadow roots. **`None`** leaves styles global.

</details>

## 4. Modules & architecture

Smart vs presentational components

<details>
  <summary>Smart vs presentational components</summary>

### Smart vs presentational components

**Definition:** **Smart (container)** components load data, talk to services/store, and pass props down. **Presentational (dumb)** components render UI from `@Input()` and emit `@Output()` events.

**How it works:** Keeps templates testable and reuse high. Aligns with **OnPush** for dumb trees.

**See also:** [Ecosystem — state](./4_angular_ecosystem.md#state-management)

</details>

SharedModule anti-pattern

<details>
  <summary>SharedModule anti-pattern</summary>

### SharedModule anti-pattern

**Definition:** Importing **singleton services** from a `SharedModule` into **lazy** feature modules can create **duplicate service instances** or break lazy isolation.

**How it works:** Provide app-wide services in **`providedIn: 'root'`** or a **CoreModule** imported once. Share **declarations** via standalone imports or a module that exports components only — not service providers per lazy load.

**See also:** [DI — hierarchical injectors](./4_angular_platform.md#dependency-injection)

</details>
