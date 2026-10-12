# TypeScript — Compiler fundamentals

Static type-checking, `tsc`, emit behavior, explicit types, and downleveling — from the [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html).

[← Back to hub](./5_ts.md)

### Related docs

[TypeScript — hub](./5_ts.md) · [The Basics](./5_ts_basics.md) · [Types](./5_ts_types.md)

### Sections

- [Static Type-Checking](#static-type-checking)
- [Non-Exception Failures](#non-exception-failures)
- [Types for Tooling](#types-for-tooling)
- [tsc — The TypeScript Compiler](#tsc-the-typescript-compiler)
- [Emitting with Errors](#emitting-with-errors)
- [Explicit Types](#explicit-types)
- [Erased Types](#erased-types)
- [Downleveling](#downleveling)

---

## Static Type-Checking

<details>
  <summary>Static Type-Checking</summary>

**Definition:**

Analysis of code at compile time without running it — TS checks types and reports errors before deploy.

| When to use                                                                                                 | Notes                                                                                                                                                                                                    |
| ----------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Run `tsc` or rely on IDE diagnostics. Fix errors before shipping; use `--noEmit` in CI for type-check only. | TypeScript doesn't stop your JS from running by default. It reports errors but still emits JavaScript unless you opt into stricter emit rules. Think of it as a linter with superpowers, not a runtime e |

**Examples:**

```typescript
const message = "hello!";
message(); // Error: Type 'string' has no call signatures
// Caught before you ever run the code
```

**Sources:** [TypeScript handbook — typescript-from-scratch](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

</details>

---

## Non-Exception Failures

<details>
  <summary>Non-Exception Failures</summary>

**Definition:**

Type errors that are not thrown exceptions — TS reports them at compile time.

| When to use                                                                               | Notes                                                                                                                                                                                                    |
| ----------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Read IDE squiggles and `tsc` output. Fix before merge; they won't appear in runtime logs. | TS sometimes rejects _valid_ JavaScript that wouldn't crash immediately (e.g. accessing `user.location` when it's not on the type). That's intentional — the goal is catching real bugs, not mirroring e |

**Examples:**

```typescript
const user = { name: "Daniel", age: 26 };
user.location; // Error in TS — undefined in plain JS
```

```typescript
const announcement = "Hello World!";
announcement.toLocaleLowercase(); // Error — typo caught
announcement.toLocalLowerCase(); // Error — typo caught
announcement.toLocaleLowerCase(); // OK
```

```typescript
function flipCoin() {
  return Math.random < 0.5; // Error — forgot () on Math.random
  // return Math.random() < 0.5; // OK
}
```

**Sources:** [TypeScript handbook — typescript-from-scratch](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

</details>

---

## Types for Tooling

<details>
  <summary>Types for Tooling</summary>

**Definition:**

Types exist primarily to power editor features — autocomplete, refactor, go-to-def.

| When to use                                                                     | Notes                                                                                                                                                                  |
| ------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hover for types, use rename-symbol and find references; good types = better DX. | You get tooling benefits even before your code is fully typed. Partial typing still helps autocomplete and navigation. The more you type, the smarter the editor gets. |

**Examples:**

```typescript
import express from "express";

const app = express();

app.get("/", function (req, res) {
  res.send; // editor suggests: send, sendDate, sendFile, sendStatus...
});

app.listen(3000);
```

**Sources:** [TypeScript handbook — typescript-from-scratch](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

</details>

---

## tsc — The TypeScript Compiler

<details>
  <summary>tsc — The TypeScript Compiler</summary>

**Definition:**

The CLI that type-checks TypeScript and emits JavaScript (`typescript` package).

| When to use                                                                                                                             | Notes                                                                                                                                                                                                    |
| --------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Run `tsc` with `tsconfig.json`, or `tsc --noEmit` to check only. Bundlers (Vite, esbuild) often transpile; `tsc` still validates types. | `tsc` does two jobs at once: type-checking AND compiling TS → JS. Many modern setups use `tsc --noEmit` for checking only and let Vite/esbuild/SWC handle the actual bundling. Know which tool does what |

**Examples:**

```typescript
// hello.ts
console.log("Hello world!");
```

```bash
tsc hello.ts
# No console output = no type errors
# Creates hello.js alongside hello.ts
```

```typescript
function greet(person, date) {
  console.log(`Hello ${person}, today is ${date}!`);
}

greet("Brendan"); // Error: Expected 2 arguments, but got 1
```

**Sources:** [TypeScript handbook — typescript-from-scratch](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

</details>

---

## Emitting with Errors

<details>
  <summary>Emitting with Errors</summary>

**Definition:**

By default `tsc` emits JS even when type errors exist.

| When to use                                                                       | Notes                                                                                                                                                                                                    |
| --------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Set `"noEmitOnError": true` in CI/production builds so broken types block output. | You can have a broken `.ts` file and a freshly updated `.js` file side by side. Use `--noEmitOnError` or `"noEmitOnError": true` in `tsconfig.json` when you want the compiler to refuse output until er |

**Examples:**

```bash
# Default: emits JS despite errors
tsc hello.ts

# Strict emit: no JS file if errors exist
tsc --noEmitOnError hello.ts
```

**Sources:** [TypeScript handbook — typescript-from-scratch](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

</details>

---

## Explicit Types

<details>
  <summary>Explicit Types</summary>

**Definition:**

Type annotations you write yourself instead of relying on inference.

| When to use                                                                     | Notes                                                                                                                                                                |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Annotate function params at public API boundaries; let inference handle locals. | `Date()` (without `new`) returns a **string** in JavaScript, not a `Date` object. TypeScript catches `greet("Maddison", Date())` as an error. You need `new Date()`. |

**Examples:**

```typescript
function greet(person: string, date: Date) {
  console.log(`Hello ${person}, today is ${date.toDateString()}!`);
}

greet("Maddison", new Date()); // OK

// greet("Maddison", Date());     // Error: string not assignable to Date
// greet("Maddison");             // Error: expected 2 arguments
```

```typescript
let msg = "hello there!";
// TS infers: string — same as writing `let msg: string`
```

**Sources:** [TypeScript handbook — typescript-from-scratch](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

</details>

---

## Erased Types

<details>
  <summary>Erased Types</summary>

**Definition:**

All type information is removed when TS compiles to JS — zero runtime footprint.

| When to use                                                                                              | Notes                                                                                                                                                                            |
| -------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Don't expect `typeof` or runtime `instanceof` on interfaces; use validation libraries for external data. | Type annotations never change runtime behavior. If TS says it's fine but your types are wrong (e.g. bad assertion), you get a runtime crash anyway. Types are compile-time only. |

**Examples:**

```typescript
// TypeScript source
function greet(person: string, date: Date) {
  console.log(`Hello ${person}, today is ${date.toDateString()}!`);
}
greet("Maddison", new Date());
```

```javascript
// Emitted JavaScript — types gone
"use strict";
function greet(person, date) {
  console.log(
    "Hello ".concat(person, ", today is ").concat(date.toDateString(), "!"),
  );
}
greet("Maddison", new Date());
```

**Sources:** [TypeScript handbook — typescript-from-scratch](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

</details>

---

## Downleveling

<details>
  <summary>Downleveling</summary>

**Definition:**

Rewriting newer JS syntax to older targets (e.g. ES2020 → ES5).

| When to use                                                                                           | Notes                                                                                                                                                                                                    |
| ----------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Set `"target"` in `tsconfig`. Let TS rewrite async/await, optional chaining, etc. for older browsers. | Default target is **ES5** (very old). Most browsers today support ES2015+. Set `"target": "ES2015"` (or higher) in `tsconfig.json` unless you need ancient browser support. Wrong target = bloated or tr |

**Examples:**

```typescript
// Source (ES2015+ syntax)
`Hello ${person}, today is ${date.toDateString()}!`;
```

```javascript
// Emitted with target ES5
"Hello ".concat(person, ", today is ").concat(date.toDateString(), "!");
```

```javascript
// Emitted with target ES2015 — template literal preserved
`Hello ${person}, today is ${date.toDateString()}!`;
```

**Sources:** [TypeScript handbook — typescript-from-scratch](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html)

</details>

