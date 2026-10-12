# TypeScript — The Basics

tsconfig, declaration files, and a short intro for JavaScript programmers.

[← Back to hub](./5_ts.md)

### Related docs

- [TypeScript — hub](./5_ts.md) · [Compiler fundamentals](./5_ts_compiler_fundamentals.md) · [Variable Declaration](./5_ts_variable_declaration.md) · [Interview](./5_ts_interview.md)

### Sections

- [TypeScript overview](#typescript-overview)
- [Compiler & tsconfig](#compiler-tsconfig)
- [tsconfig.json](#tsconfig-json)
- [Declaration files (.d.ts)](#declaration-files-dts)
- [TypeScript for JavaScript programmers](#typescript-for-javascript-programmers)
- [Design principles](#design-principles)
- [Modern TypeScript](#modern-typescript)

---

## TypeScript overview

<details>
  <summary>TypeScript overview</summary>

**Definition:** JavaScript is dynamically typed; TypeScript is a **superset** with optional static types checked at compile time, then erased.

**Compiler fundamentals:** [Compiler fundamentals](./5_ts_compiler_fundamentals.md) — static type-checking, `tsc`, emit, explicit types, downleveling (handbook order on that page).

**Type system (everyday types):** [Types hub](./5_ts_types.md).

**Sources:** [TypeScript from scratch](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

</details>

---

## Compiler & tsconfig

<details>
  <summary>Strictness & tsconfig</summary>

### Strictness

**Definition:**

Family of compiler flags that increase type safety (`strict`, `noImplicitAny`, etc.).

| When to use                                                                 | Notes                                                                                                                                                                  |
| --------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Enable `"strict": true` on new projects; tighten legacy code incrementally. | `"strict": true` in `tsconfig.json` flips on all strict flags at once. You can also enable them individually. The two biggest: `noImplicitAny` and `strictNullChecks`. |

**Sources:** [TypeScript handbook — tsconfig-json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

### `noImplicitAny`

**Definition:**

A compiler flag that errors when TS would otherwise infer `any` for a variable or parameter.

| When to use                                                                                                             | Notes                                                                                                                                                                       |
| ----------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Enable via `strict` or `"noImplicitAny": true`. Forces explicit types or better inference — catches silent `any` leaks. | Without this flag, untyped function parameters silently become `any` and you lose all checking inside the function. This one flag alone dramatically improves code quality. |

**Examples:**

```typescript
// With noImplicitAny: OFF — no error, person is any
function greet(person) {
  console.log(person.toUpperCase());
}

// With noImplicitAny: ON — must annotate
function greet(person: string) {
  console.log(person.toUpperCase());
}
```

**Sources:** [TypeScript handbook — tsconfig-json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

### `strictNullChecks`

**Definition:**

A flag making `null` and `undefined` distinct types that must be handled explicitly.

| When to use                                                                                                                  | Notes                                                                                                                                                                                                    |
| ---------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Enable in `tsconfig`. Use optional chaining `?.`, nullish coalescing `??`, and guards before accessing possibly-null values. | Turning this on in an existing codebase can produce hundreds of errors overnight. Worth it for new code; plan a gradual migration for legacy projects. Pair with optional chaining (`?.`) and nullish co |

**Examples:**

```typescript
// strictNullChecks: OFF — no error
function getLength(s: string) {
  return s.length;
}
getLength(null); // runs, crashes at runtime

// strictNullChecks: ON
function getLength(s: string | null) {
  if (s === null) return 0;
  return s.length; // s narrowed to string
}
```

**Sources:** [TypeScript handbook — tsconfig-json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

### Recommended tsconfig for new projects

**Definition:**

Recommended tsconfig for new projects — TypeScript language or compiler feature.

| Flag               | Purpose                                                          |
| ------------------ | ---------------------------------------------------------------- |
| `strict`           | Enables `noImplicitAny`, `strictNullChecks`, etc.                |
| `moduleResolution` | How imports resolve (`node`, `bundler`, `node16`)                |
| `paths`            | Import aliases (`@/components` → `src/components`)               |
| `isolatedModules`  | Each file must be transpilable alone (required for Vite/esbuild) |
| `skipLibCheck`     | Skip type-checking `.d.ts` files — faster builds                 |
| `noEmitOnError`    | Block JS output when types fail                                  |

`strict: true` enables `noImplicitAny`, `strictNullChecks`, and other strict flags together.

**Examples:**

```json
{
  "compilerOptions": {
    "strict": true,
    "target": "ES2015",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "noEmitOnError": true,
    "isolatedModules": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "resolveJsonModule": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```

**Sources:** [TypeScript handbook — tsconfig-json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

### Advanced `tsconfig` flags — stricter and modern

**Definition:**

Enable stricter flags **incrementally** — `noUncheckedIndexedAccess` alone can add hundreds of errors on existing codebases.

| Flag                                        | Purpose                                                                                                                                            |
| ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `noUncheckedIndexedAccess`                  | `arr[i]` and `obj[k]` become `T \| undefined` — safer, noisier                                                                                     |
| `exactOptionalPropertyTypes`                | `prop?: T` ≠ `prop: T \| undefined` — stricter optional props                                                                                      |
| `strictFunctionTypes`                       | Contravariant function params (fixes unsound bivariance)                                                                                           |
| `verbatimModuleSyntax`                      | Enforce `import type` / `export type`; cleaner ESM/CJS boundaries                                                                                  |
| `moduleResolution: "node16"` / `"nodenext"` | Correct resolution for Node ESM + `package.json` `"type"`                                                                                          |
| `declaration` + `declarationMap`            | Emit `.d.ts` (+ source maps) for library consumers                                                                                                 |
| `composite` + `incremental`                 | Project references and faster incremental builds                                                                                                   |
| `noImplicitOverride`                        | Require `override` when overriding a base class method                                                                                             |
| `erasableSyntaxOnly`                        | Ban syntax that emits runtime JS (parameter properties, enums, namespaces) so `tsc` can type-check without a transform — Node/ts-blank-space style |

Enable stricter flags **incrementally** — `noUncheckedIndexedAccess` alone can add hundreds of errors on existing codebases.

**Examples:**

```json
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "verbatimModuleSyntax": true,
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "declaration": true,
    "declarationMap": true,
    "composite": true
  }
}
```

**Sources:** [TypeScript handbook — tsconfig-json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

### `@ts-ignore` vs `@ts-expect-error`

**Definition:**

Escape hatches when the compiler is wrong or types are missing. Prefer fixing types; use sparingly.

| Aspect  | Detail                                                                                              |
| ------- | --------------------------------------------------------------------------------------------------- |
| Summary | Escape hatches when the compiler is wrong or types are missing. Prefer fixing types; use sparingly. |

Escape hatches when |

Escape hatches when the compiler is wrong or types are missing. Prefer fixing types; use sparingly.

**How it works:** `@ts-expect-error` is preferred — it fails when the underlying issue is fixed, so suppressions don't rot.

**Examples:**

```typescript
// @ts-ignore — suppresses next line always (no error if line is fine)
// @ts-expect-error — errors if the NEXT line has NO type error (safer — catches stale suppressions)

// @ts-expect-error — legacy API not typed yet
legacyApi.doSomething();
```

**Sources:** [TypeScript handbook — tsconfig-json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

</details>

<details>
  <summary>tsconfig.json</summary>

### tsconfig.json

**Definition:**

A JSON project file that tells the TypeScript compiler **which files belong to the project** and **how to type-check and emit** them.

| When to use                                                                                         | Notes                                                                                                                                                      |
| --------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Every TypeScript project. Run `tsc --init` to scaffold a starter file, then tune `compilerOptions`. | `tsc` looks for `tsconfig.json` in the current folder (or use `tsc -p path/to/tsconfig.json`). Without it, only the files you pass on the CLI are checked. |

**How it works:**

- **`compilerOptions`** — flags such as `strict`, `target`, `module`, `moduleResolution`, `paths`, `noEmit`.
- **`include` / `exclude`** — glob patterns for source files (for example `"src/**/*"`; exclude `node_modules` and tests).
- **`files`** — explicit file list when you do not want globs.
- **`extends`** — inherit a shared base config; child `compilerOptions` merge, but `files` / `include` / `exclude` in the child **replace** the base lists.

**Examples:**

```json
{
  "compilerOptions": {
    "strict": true,
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "noEmit": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

```json
{
  "extends": "./tsconfig.base.json",
  "compilerOptions": {
    "outDir": "./dist"
  }
}
```

**Sources:** [TypeScript handbook — tsconfig.json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html) · [TSConfig Reference](https://www.typescriptlang.org/tsconfig/)

</details>

<details>
  <summary>Declaration files (.d.ts)</summary>

### Declaration files (.d.ts)

**Definition:**

Type-only files that describe the **shape** of JavaScript code — they emit **no runtime JavaScript** when written by hand, but give TypeScript (and your editor) types for JS libraries and globals.

| When to use                                                                                                        | Notes                                                                                                                                        |
| ------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Consuming untyped npm packages (`@types/node`, `@types/react`), publishing a typed library, or augmenting globals. | During module resolution, TypeScript prefers `.ts` / `.d.ts` over plain `.js` so imports get checking without reading implementation source. |

**How it works:**

- **Hand-written `.d.ts`** — describe third-party or legacy JS (`declare module "lodash"`, ambient globals).
- **Emitted `.d.ts`** — turn on `"declaration": true` in `tsconfig.json` when publishing a TS library so consumers import your public API types.
- **`@types/*` packages** — community or DefinitelyTyped declarations installed beside JavaScript packages.
- **`skipLibCheck`** — skips deep checking of declaration files for faster builds; does not remove the types themselves.

**Examples:**

```typescript
// types/legacy-lib.d.ts — shape of an untyped package
declare module "legacy-lib" {
  export function parse(input: string): Record<string, unknown>;
}
```

```typescript
// global.d.ts — value loaded outside modules (e.g. script tag)
declare const APP_CONFIG: {
  apiUrl: string;
};
```

```typescript
// Consuming installed declarations
import _ from "lodash";
_.padStart("Hello TypeScript!", 20, " ");
```

**Sources:** [Declaration files — introduction](https://www.typescriptlang.org/docs/handbook/declaration-files/introduction.html) · [Consuming declarations](https://www.typescriptlang.org/docs/handbook/declaration-files/consumption.html) · [The role of declaration files](https://www.typescriptlang.org/docs/handbook/modules/theory.html#the-role-of-declaration-files)

</details>

<a name="everyday-types"></a>

## TypeScript for JavaScript programmers

<details>
  <summary>Types by inference</summary>

### Types by inference

**Definition:**

TypeScript infers types from values — you often do not need explicit annotations.

| When to use                                                                                                  | Notes                                                                       |
| ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------- |
| Let inference work for locals and simple assignments; add annotations when inference is too wide or unclear. | VS Code uses TypeScript for JavaScript IntelliSense — same inference model. |

**Examples:**

```typescript
let helloWorld = "Hello World";
// inferred: string
helloWorld(); // Error: Type 'string' has no call signatures
```

**Sources:** [TypeScript for JavaScript Programmers](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)

</details>

<details>
  <summary>Defining types</summary>

### Defining types with interfaces

**Definition:**

Describe object shapes with `interface` or annotate variables with `: TypeName`.

**Examples:**

```typescript
interface User {
  name: string;
  id: number;
}

const user: User = { name: "Hayes", id: 0 };
```

**Sources:** [TypeScript for JavaScript Programmers](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)

</details>

<details>
  <summary>Composing types</summary>

### Unions and generics (overview)

**Definition:**

Combine types with unions (`A | B`) and generic parameters (`<T>`) — see [Types — Combining](./5_ts_types_combining.md) and [Type Manipulation](./5_ts_type_manipulation.md).

**Examples:**

```typescript
type WindowStates = "open" | "closed" | "minimized";

function getLength(obj: string | string[]) {
  return obj.length;
}

interface Backpack<Type> {
  add: (obj: Type) => void;
  get: () => Type;
}
```

**Sources:** [TypeScript for JavaScript Programmers](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)

</details>

<details>
  <summary>Structural type system</summary>

### Structural typing

**Definition:**

TypeScript compares **shape**, not declaration name — if two values have compatible fields, they match.

**Examples:**

```typescript
interface Point {
  x: number;
  y: number;
}

function logPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}

const point = { x: 12, y: 26 };
logPoint(point); // OK — same shape
```

**Sources:** [TypeScript for JavaScript Programmers](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)

</details>

---

<a name="design-principles"></a>

## Design principles

<details>
  <summary>SOLID in TypeScript</summary>

### SOLID in TypeScript

**Definition:** The five **SOLID** principles applied to **TypeScript interfaces, classes, and modules** — compile-time contracts via `interface`, `implements`, and constructor injection.

| Principle                     | Meaning                         | TypeScript example                                                                    |
| ----------------------------- | ------------------------------- | ------------------------------------------------------------------------------------- |
| **S** — Single Responsibility | One reason to change per module | `UserRepository` reads data · `UserFormatter` shapes labels — not one god class       |
| **O** — Open/Closed           | Extend via new code, not edits  | New `StripeProcessor implements PaymentProcessor` — don't edit `CheckoutService`      |
| **L** — Liskov Substitution   | Subtypes honor parent contract  | Every `UserStore` passed to `loadProfile()` must return the same `{ id, name }` shape |
| **I** — Interface Segregation | Small focused interfaces        | `ReadableLedger` for reports — not a fat `Ledger` with `write()` / `delete()`         |
| **D** — Dependency Inversion  | Depend on abstractions          | `placeOrder(id, logger: Logger)` — inject `Logger`, not a concrete console adapter    |

**How it works:**

Each letter matches the **Examples** below — look for `// good` and `// bad`:

1. **S** — split repository vs formatter with separate `interface`s — not one service class.
2. **O** — new provider = new class implementing the interface — not a growing `if/else` in one class.
3. **L** — `implements UserStore` must satisfy the return type — `tsc` catches broken substitutes.
4. **I** — pass `ReadableLedger` where reads are enough — don't require unused write APIs.
5. **D** — domain functions take `Logger` / `DbClient` types — wiring layer picks concrete implementations.

**When to use:** Domain models, services, DI graphs, public module APIs. Skip heavy interfaces on a one-off script.

**Examples:**

```typescript
type DbClient = { query(id: string): Promise<{ id: string; name: string }> };

// S — good: repository vs formatter — separate interfaces
interface UserRepository {
  findById(id: string): Promise<{ id: string; name: string }>;
}

interface UserFormatter {
  label(user: { name: string; email: string }): string;
}

class DbUserRepository implements UserRepository {
  constructor(private db: DbClient) {}
  findById(id: string) {
    return this.db.query(id);
  }
}

// O — good: new processor = new class implementing PaymentProcessor
interface PaymentProcessor {
  charge(amount: number): Promise<{ ok: true }>;
}

class StripeProcessor implements PaymentProcessor {
  charge(amount: number) {
    return Promise.resolve({ ok: true as const });
  }
}

class CheckoutService {
  constructor(private payments: PaymentProcessor) {}
  checkout(amount: number) {
    return this.payments.charge(amount);
  }
}

// L — good: any UserStore works in loadProfile
interface UserStore {
  get(id: string): Promise<{ id: string; name: string }>;
}

async function loadProfile(id: string, store: UserStore) {
  return store.get(id);
}

// I — good: report needs only ReadableLedger
interface ReadableLedger {
  read(id: string): Promise<{ id: string; total: number }>;
}

function buildReport(ledger: ReadableLedger) {
  return ledger.read("summary");
}

// D — good: inject Logger abstraction
interface Logger {
  info(message: string): void;
}

function placeOrder(orderId: string, logger: Logger) {
  logger.info(`Processing ${orderId}`);
}
```

**Thoughts:** TypeScript makes **I**, **L**, and **D** visible at compile time — but types erase at runtime; behavior contracts still matter. Class syntax: [Classes](./5_ts_classes.md). JS fundamentals: [SOLID in Classes](./3_js_core.md#solid-in-classes) · [SOLID in Functions](./3_js_core.md#solid-in-functions). Framework mapping: [SOLID in React](./4_react_core.md#solid-in-react) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express). Full reference: [OOP & SOLID](./7_special_engineering.md#solid-overview).

**Sources:** [TypeScript handbook — Object Types](https://www.typescriptlang.org/docs/handbook/2/objects.html) · [TypeScript handbook — Classes](https://www.typescriptlang.org/docs/handbook/2/classes.html) · [SOLID (overview)](./7_special_engineering.md#solid-overview) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions)

</details>

---

<a name="modern-typescript"></a>

## Modern TypeScript

<details>
  <summary>Modern TypeScript</summary>

### Quick list — checklist

1. **`const` type parameters** — literal inference in generics (TS 5.0+)
2. **`using` / `Disposable`** — scoped resource cleanup (TS 5.2+)
3. **`override` keyword** — explicit, checked method overrides
4. **Stricter compiler flags** — `noUncheckedIndexedAccess`, `exactOptionalPropertyTypes`
5. **ESM/CJS** — `moduleResolution: node16` / `nodenext`, `verbatimModuleSyntax`
6. **Project references** — split monorepos, incremental `tsc --build`
7. **Publishing typed packages** — `declaration`, `types` field, bundling `.d.ts`
8. **Recursive types** — `Json`, trees, deep `Partial`
9. **Variance** — covariant returns, contravariant params, bivariance pitfalls
10. **Runtime validation** — TS types ≠ runtime; use Zod/io-ts for external data

### `const` Type Parameters (TS 5.0+)

**Definition:**

Generic parameter modifier that preserves literal types in inference.

| When to use                                                                                               | Notes                                                                                             |
| --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| `function routes<const T extends readonly string[]>(paths: T)` — keeps tuple of literals, not `string[]`. | recursion must have a **base case** (`T extends object` guard) or the compiler hits depth limits. |

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Variance (functions and assignability)

**Definition:**

How subtyping works for function params (contravariant) and returns (covariant).

| Aspect  | Detail                                                                           |
| ------- | -------------------------------------------------------------------------------- |
| Summary | How subtyping works for function params (contravariant) and returns (covariant). |

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

</details>
