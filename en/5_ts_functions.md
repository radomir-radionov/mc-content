# TypeScript — Functions

Function types, parameters, generics, and overloads — from the [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/2/functions.html).

[← Back to hub](./5_ts.md)

### Related docs

[TypeScript — hub](./5_ts.md) · [Types](./5_ts_types.md) · [Narrowing](./5_ts_narrowing.md) · [Object Types](./5_ts_objects.md) · [Type Manipulation](./5_ts_type_manipulation.md) · [Interview](./5_ts_interview.md)

### Main concepts

| Topic                                                                                                         | Covers                                   |
| ------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| [Function Type Expressions](#function-type-expressions)                                                       | `(a: string) => void` syntax and aliases |
| [Call Signatures](#call-signatures)                                                                           | Callable objects with properties         |
| [Construct Signatures](#construct-signatures)                                                                 | `new (...)` constructor types            |
| [Generic Functions](#generic-functions)                                                                       | Type parameters on functions             |
| [Inference](#inference)                                                                                       | Type args inferred at the call site      |
| [Constraints](#constraints)                                                                                   | `Type extends …` bounds                  |
| [Working with Constrained Values](#working-with-constrained-values)                                           | What you may return as `Type`            |
| [Specifying Type Arguments](#specifying-type-arguments)                                                       | Explicit `fn<string>(…)`                 |
| [Guidelines for Writing Good Generic Functions](#guidelines-for-writing-good-generic-functions)               | Inference-friendly patterns              |
| [Optional Parameters](#optional-parameters)                                                                   | `?` and default values                   |
| [Optional Parameters in Callbacks](#optional-parameters-in-callbacks)                                         | `index?` and strict callbacks            |
| [Function Overloads](#function-overloads)                                                                     | Multiple call shapes, one body           |
| [Overload Signatures and the Implementation Signature](#overload-signatures-and-the-implementation-signature) | What callers vs the body see             |
| [Writing Good Overloads](#writing-good-overloads)                                                             | Unions vs overloads                      |
| [Declaring `this` in a Function](#declaring-this-in-a-function)                                               | Typed `this` parameter                   |
| [Other Types to Know About](#other-types-to-know-about)                                                       | Return/value types in APIs               |
| [void](#void)                                                                                                 | “No useful return”                       |
| [object](#object)                                                                                             | Non-primitive values                     |
| [unknown](#unknown)                                                                                           | Safe top type for inputs                 |
| [never](#never)                                                                                               | No value / exhaustiveness                |
| [Function](#function)                                                                                         | Untyped callable — avoid                 |
| [Rest Parameters and Arguments](#rest-parameters-and-arguments)                                               | `...rest` vs spread                      |
| [Rest Parameters](#rest-parameters)                                                                           | Collect trailing args                    |
| [Rest Arguments](#rest-arguments)                                                                             | Spread into a call                       |
| [Parameter Destructuring](#parameter-destructuring)                                                           | Typed `{ a, b }` params                  |
| [Assignability of Functions](#assignability-of-functions)                                                     | When one function type fits another      |
| [Return type void](#return-type-void)                                                                         | Contextual `() => void` vs `(): void`    |

### Sections

- [Function Type Expressions](#function-type-expressions)
- [Call Signatures](#call-signatures)
- [Construct Signatures](#construct-signatures)
- [Generic Functions](#generic-functions)
- [Inference](#inference)
- [Constraints](#constraints)
- [Working with Constrained Values](#working-with-constrained-values)
- [Specifying Type Arguments](#specifying-type-arguments)
- [Guidelines for Writing Good Generic Functions](#guidelines-for-writing-good-generic-functions)
- [Optional Parameters](#optional-parameters)
- [Optional Parameters in Callbacks](#optional-parameters-in-callbacks)
- [Function Overloads](#function-overloads)
- [Overload Signatures and the Implementation Signature](#overload-signatures-and-the-implementation-signature)
- [Writing Good Overloads](#writing-good-overloads)
- [Declaring `this` in a Function](#declaring-this-in-a-function)
- [Other Types to Know About](#other-types-to-know-about)
- [void](#void)
- [object](#object)
- [unknown](#unknown)
- [never](#never)
- [Function](#function)
- [Rest Parameters and Arguments](#rest-parameters-and-arguments)
- [Rest Parameters](#rest-parameters)
- [Rest Arguments](#rest-arguments)
- [Parameter Destructuring](#parameter-destructuring)
- [Assignability of Functions](#assignability-of-functions)
- [Return type void](#return-type-void)

### Topic groups

Handbook sections shown as grouped index on the topic page and in the sidebar:

- **Generic Functions** — Generic Functions, Inference, Constraints, Working with Constrained Values, Specifying Type Arguments, Guidelines for Writing Good Generic Functions
- **Optional Parameters** — Optional Parameters, Optional Parameters in Callbacks
- **Function Overloads** — Function Overloads, Overload Signatures and the Implementation Signature, Writing Good Overloads
- **Other Types to Know About** — Other Types to Know About, void, object, unknown, never, Function
- **Rest Parameters and Arguments** — Rest Parameters and Arguments, Rest Parameters, Rest Arguments

Top-level (no section header): Function Type Expressions, Call Signatures, Construct Signatures, Declaring `this` in a Function, Parameter Destructuring, Assignability of Functions, Return type void.

---

## Function Type Expressions

<details>
<summary>Function Type Expressions</summary>

**Definition:** The simplest way to describe a function type — syntax like an arrow function: `(param: Type) => ReturnType`.

**How it works:**

- `(a: string) => void` — one `string` param, no meaningful return.
- Omitted parameter types default to `any` (avoid).
- The **parameter name is required**: `(string) => void` means a parameter literally named `string` of type `any`, not “takes a string”.
- Name the type with a type alias when reused: `type Greet = (name: string) => void`.

**Examples:**

```typescript
function greeter(fn: (a: string) => void) {
  fn("Hello, World");
}

function printToConsole(s: string) {
  console.log(s);
}

greeter(printToConsole);

type GreetFunction = (a: string) => void;
```

**Sources:** [TypeScript handbook — Function Type Expressions](https://www.typescriptlang.org/docs/handbook/2/functions.html#function-type-expressions)

</details>

---

## Call Signatures

<details>
<summary>Call Signatures</summary>

**Definition:** An object type that describes something **callable** and may also have properties — used when a function has metadata attached.

**How it works:**

- Use `:` before the return type inside `{ }`, not `=>`.
- Properties sit alongside the call signature in the same object type.

**Examples:**

```typescript
type DescribableFunction = {
  description: string;
  (someArg: number): boolean;
};

function doSomething(fn: DescribableFunction) {
  console.log(fn.description + " returned " + fn(6));
}

function myFunc(someArg: number) {
  return someArg > 3;
}
myFunc.description = "default description";
doSomething(myFunc);
```

**Sources:** [TypeScript handbook — Call Signatures](https://www.typescriptlang.org/docs/handbook/2/functions.html#call-signatures)

</details>

---

## Construct Signatures

<details>
<summary>Construct Signatures</summary>

**Definition:** Describes a value callable with `new` — a constructor that returns an instance type.

**How it works:**

- Prefix the call signature with `new`: `new (s: string) => SomeObject`.
- Some values support both call and construct (`Date`) — combine both signatures on one type.

**Examples:**

```typescript
type SomeConstructor = {
  new (s: string): SomeObject;
};

function fn(ctor: SomeConstructor) {
  return new ctor("hello");
}

interface CallOrConstruct {
  (n?: number): string;
  new (s: string): Date;
}

fn(Date); // Date works both ways
```

**Sources:** [TypeScript handbook — Construct Signatures](https://www.typescriptlang.org/docs/handbook/2/functions.html#construct-signatures)

</details>

---

## Generic Functions

<details>
<summary>Generic Functions</summary>

**Definition:** A function with **type parameters** in angle brackets that link argument types to return types — e.g. `firstElement<Type>(arr: Type[]): Type | undefined`.

**How it works:**

- Type parameters are declared once on the function: `function identity<T>(arg: T): T`.
- The same `T` can appear in several parameter or return positions so they stay in sync.
- Generics are erased at runtime; they exist only for the type checker.

**When to use:** When the result type should follow an argument type (`first`, `map`, `identity`). Skip generics that never relate two or more types.

**Examples:**

```typescript
function firstElement<Type>(arr: Type[]): Type | undefined {
  return arr[0];
}

function map<Input, Output>(
  arr: Input[],
  func: (arg: Input) => Output,
): Output[] {
  return arr.map(func);
}
```

**See also:** [Generics](./5_ts_type_manipulation.md#generics) for interfaces, classes, and advanced patterns.

**Sources:** [TypeScript handbook — Generic Functions](https://www.typescriptlang.org/docs/handbook/2/functions.html#generic-functions)

</details>

---

## Inference

<details>
<summary>Inference</summary>

**Definition:** TypeScript usually **figures out** type arguments from the values you pass — you do not write `<Type>` on every call.

**How it works:**

- `firstElement(["a", "b"])` infers `Type` as `string`.
- Inference uses argument positions tied to the same type parameter.
- If inference would pick a type that is too wide or ambiguous, you may need an explicit type argument or a better parameter annotation (see [Guidelines](#guidelines-for-writing-good-generic-functions)).

**Examples:**

```typescript
const s = firstElement(["a", "b", "c"]); // string
const n = firstElement([1, 2, 3]); // number

function identity<T>(arg: T): T {
  return arg;
}
const x = identity("hello"); // string — T inferred
```

**Sources:** [TypeScript handbook — Inference](https://www.typescriptlang.org/docs/handbook/2/functions.html#inference)

</details>

---

## Constraints

<details>
<summary>Constraints</summary>

**Definition:** A **constraint** limits what a type argument may be: `Type extends SomeShape`.

**How it works:**

- `extends` means “`Type` must be assignable to `SomeShape`”.
- Inside the function you may use members guaranteed by the constraint (e.g. `.length` when `extends { length: number }`).
- Constraints do not change runtime behavior — only what the compiler allows.

**Examples:**

```typescript
function longest<Type extends { length: number }>(a: Type, b: Type) {
  return a.length >= b.length ? a : b;
}

longest([1, 2], [1, 2, 3]); // number[]
longest("alice", "bob"); // string
```

**Sources:** [TypeScript handbook — Constraints](https://www.typescriptlang.org/docs/handbook/2/functions.html#constraints)

</details>

---

## Working with Constrained Values

<details>
<summary>Working with Constrained Values</summary>

**Definition:** A generic return type `Type` means “the **same** `Type` the caller passed in” — not “any value that satisfies the constraint”.

**How it works:**

- Returning a fresh object `{ length: minimum }` is not assignable to `Type` even when `Type extends { length: number }`.
- Safe pattern: return the input `obj` when valid, or narrow with a type predicate / overload if you truly construct a new value.

**Examples:**

```typescript
function minimumLength<Type extends { length: number }>(
  obj: Type,
  minimum: number,
): Type {
  if (obj.length >= minimum) {
    return obj;
  }
  return { length: minimum }; // Error: not assignable to Type
}
```

**Sources:** [TypeScript handbook — Working with Constrained Values](https://www.typescriptlang.org/docs/handbook/2/functions.html#working-with-constrained-values)

</details>

---

## Specifying Type Arguments

<details>
<summary>Specifying Type Arguments</summary>

**Definition:** You can pass type arguments explicitly: `fn<TypeArg>(value)`.

**How it works:**

- Use when inference picks the wrong type or cannot infer (e.g. empty array `[]`).
- Explicit args must still satisfy any `extends` constraint.
- Prefer inference when it works — less noise at call sites.

**Examples:**

```typescript
firstElement<string>(["a", "b", "c"]);

function toArray<T>(value: T): T[] {
  return [value];
}
toArray<number>(3);
toArray<string>("x");

const nums = firstElement<number[]>([[1, 2]]); // explicit when inference is unclear
```

**Sources:** [TypeScript handbook — Specifying Type Arguments](https://www.typescriptlang.org/docs/handbook/2/functions.html#specifying-type-arguments)

</details>

---

## Guidelines for Writing Good Generic Functions

<details>
<summary>Guidelines for Writing Good Generic Functions</summary>

**Definition:** Conventions that keep generic functions easy to call and correctly inferred.

**How it works:**

- Prefer `arr: Type[]` over `arr: Type extends any[]` when you only need an array of `Type` — inference behaves better.
- A type parameter should relate **two or more** places (input and output). If it appears only once, it often adds noise.
- Use few type parameters; split into multiple functions if signatures diverge.
- Add constraints only when you need properties on `Type` inside the body.

**Examples:**

```typescript
// Prefer
function firstElement<Type>(arr: Type[]): Type | undefined {
  return arr[0];
}

// Harder to infer — avoid unless you need it
function firstElementBad<Type extends any[]>(arr: Type) {
  return arr[0];
}
```

**Sources:** [TypeScript handbook — Guidelines for Writing Good Generic Functions](https://www.typescriptlang.org/docs/handbook/2/functions.html#guidelines-for-writing-good-generic-functions)

</details>

---

## Optional Parameters

<details>
<summary>Optional Parameters</summary>

**Definition:** Parameters that may be omitted — marked with `?` or a **default value**.

**How it works:**

- `x?: number` → type is `number | undefined` inside the function.
- `x = 10` → type is `number`; `undefined` is replaced by the default.
- Optional parameters must follow required ones.
- Callers may pass `undefined` explicitly — it simulates a missing argument.

**Examples:**

```typescript
function f(x?: number) {
  // x: number | undefined
}
f();
f(10);
f(undefined);

function g(x = 10) {
  // x: number
}
```

**Sources:** [TypeScript handbook — Optional Parameters](https://www.typescriptlang.org/docs/handbook/2/functions.html#optional-parameters)

</details>

---

## Optional Parameters in Callbacks

<details>
<summary>Optional Parameters in Callbacks</summary>

**Definition:** Marking a **callback** parameter as optional (`index?`) means the **caller** of the callback may omit that argument — your function must type the callback parameter as `number | undefined`.

**How it works:**

- `forEach`-style APIs often pass every argument; a callback typed with `index?` still allows implementations that only use the first arg.
- Under `strictNullChecks`, using `index` without a check is an error when `index?` is in the callback type.

**Examples:**

```typescript
function myForEach(arr: any[], callback: (arg: any, index?: number) => void) {
  for (let i = 0; i < arr.length; i++) {
    callback(arr[i]); // legal — index may be omitted
  }
}

myForEach([1, 2, 3], (a, i) => {
  console.log(i.toFixed()); // Error: i is possibly undefined
});

myForEach([1, 2, 3], (a, i) => {
  if (i !== undefined) console.log(i.toFixed());
});
```

**Sources:** [TypeScript handbook — Optional Parameters in Callbacks](https://www.typescriptlang.org/docs/handbook/2/functions.html#optional-parameters-in-callbacks)

</details>

---

## Function Overloads

<details>
<summary>Function Overloads</summary>

**Definition:** **Overloads** describe multiple ways to call the same function name — different argument counts or types — while one implementation body handles them all.

**How it works:**

- Overloads are useful when return type or arity changes with argument shape (e.g. `Date` from timestamp vs `y, m, d`).
- They are not the same as a union parameter when each overload implies a different return type.

**Examples:**

```typescript
function makeDate(timestamp: number): Date;
function makeDate(m: number, d: number, y: number): Date;
function makeDate(mOrTimestamp: number, d?: number, y?: number): Date {
  if (d !== undefined && y !== undefined) {
    return new Date(y, mOrTimestamp, d);
  }
  return new Date(mOrTimestamp);
}

const d1 = makeDate(12345678);
const d2 = makeDate(5, 5, 5);
```

**Sources:** [TypeScript handbook — Function Overloads](https://www.typescriptlang.org/docs/handbook/2/functions.html#function-overloads)

</details>

---

## Overload Signatures and the Implementation Signature

<details>
<summary>Overload Signatures and the Implementation Signature</summary>

**Definition:** **Overload signatures** are the callable faces shown to callers; the **implementation signature** is the single function body underneath — it is not directly callable from outside.

**How it works:**

- List one or more overload signatures, then one implementation with a compatible (often wider) parameter list.
- The implementation must accept every overload’s inputs and produce assignable outputs.
- TypeScript checks calls against overloads, not the implementation signature.

**Examples:**

```typescript
function pick(id: string): object;
function pick(id: string, version: number): object;
function pick(id: string, version?: number): object {
  // implementation — wider than any single overload
  return version === undefined ? { id } : { id, version };
}

pick("abc");
pick("abc", 1);
// pick("abc", "v2"); // Error — no matching overload
```

**Sources:** [TypeScript handbook — Overload Signatures and the Implementation Signature](https://www.typescriptlang.org/docs/handbook/2/functions.html#overload-signatures-and-the-implementation-signature)

</details>

---

## Writing Good Overloads

<details>
<summary>Writing Good Overloads</summary>

**Definition:** Prefer the simplest typing that accepts real call sites — often a **union** instead of many overloads.

**How it works:**

- Use overloads when return type or arity truly depends on arguments in ways a union cannot express cleanly.
- Do **not** use overloads only to model optional trailing parameters — use `?` or defaults instead.
- A union parameter accepts a union of inputs; overloads do not (`len("hi")` and `len([0])` vs `len(cond ? "hi" : [0])`).

**Examples:**

```typescript
// Prefer union when return type is the same
function len(x: string | any[]) {
  return x.length;
}
len(Math.random() > 0.5 ? "hello" : [0]); // OK

// Overloads would reject the conditional union argument
```

**Sources:** [TypeScript handbook — Writing Good Overloads](https://www.typescriptlang.org/docs/handbook/2/functions.html#writing-good-overloads)

</details>

---

## Declaring `this` in a Function

<details>
<summary>Declaring `this` in a Function</summary>

**Definition:** A fake first parameter `this: SomeType` declares what `this` means inside a **function** body — it is not passed at the call site.

**How it works:**

- TypeScript infers `this` for methods on objects, but callback APIs often need an explicit `this` type.
- **Arrow functions cannot declare `this`** — they capture lexical `this` from the enclosing scope.

**Examples:**

```typescript
interface User {
  admin: boolean;
}

interface DB {
  filterUsers(filter: (this: User) => boolean): User[];
}

const db = getDB();
const admins = db.filterUsers(function (this: User) {
  return this.admin;
});

// db.filterUsers(() => this.admin); // Error — arrow captures outer this
```

**Sources:** [TypeScript handbook — Declaring this in a Function](https://www.typescriptlang.org/docs/handbook/2/functions.html#declaring-this-in-a-function)

</details>

---

## Other Types to Know About

<details>
<summary>Other Types to Know About</summary>

**Definition:** Types that show up often when you annotate function parameters and return types — especially in callbacks and generic boundaries.

**How it works:**

- The subsections below (`void`, `object`, `unknown`, `never`, `Function`) are the handbook’s reference for each name.
- Prefer precise function types over the global `Function` type.
- Pair `unknown` inputs with narrowing; pair `never` with exhaustiveness checks.

**Sources:** [TypeScript handbook — Other Types to Know About](https://www.typescriptlang.org/docs/handbook/2/functions.html#other-types-to-know-about)

</details>

---

## void

<details>
<summary>void</summary>

**Definition:** **`void`** means the caller should not rely on a return value — not “the value `undefined`”.

**How it works:**

- Functions with no `return` or bare `return` infer `void`.
- See [Return type void](#return-type-void) for contextual typing vs an explicit `(): void` annotation.

**Examples:**

```typescript
function logMessage(msg: string): void {
  console.log(msg);
}
```

**Sources:** [TypeScript handbook — void](https://www.typescriptlang.org/docs/handbook/2/functions.html#void)

</details>

---

## object

<details>
<summary>object</summary>

**Definition:** **`object`** is any non-primitive value — not the same as `Object` (the instance type of the global constructor).

**How it works:**

- Excludes `string`, `number`, `boolean`, `symbol`, `bigint`, `undefined`, `null` (primitives).
- Function values are objects in JavaScript; use a function type when you need to call them.

**Examples:**

```typescript
function printNonPrimitive(value: object) {
  console.log(value);
}

printNonPrimitive({ a: 1 });
printNonPrimitive(() => {});
// printNonPrimitive(42); // Error
```

**Sources:** [TypeScript handbook — object](https://www.typescriptlang.org/docs/handbook/2/functions.html#object)

</details>

---

## unknown

<details>
<summary>unknown</summary>

**Definition:** **`unknown`** is the type-safe counterpart to `any` — any value is assignable **to** `unknown`, but you must narrow before using it.

**How it works:**

- Use for inputs you do not trust yet (JSON, user data).
- Narrow with `typeof`, `in`, or custom guards before calling methods.

**Examples:**

```typescript
function processValue(value: unknown): string {
  if (typeof value === "string") {
    return value.toUpperCase();
  }
  return "Unknown";
}
```

**See also:** [unknown](./5_ts_types_safety.md#unknown) in Types — Safety.

**Sources:** [TypeScript handbook — unknown](https://www.typescriptlang.org/docs/handbook/2/functions.html#unknown)

</details>

---

## never

<details>
<summary>never</summary>

**Definition:** **`never`** is the bottom type — no value is assignable to it except `never` itself.

**How it works:**

- Functions that always `throw` or loop forever return `never`.
- Use in `default` branches with exhaustiveness checking (see [Narrowing — Exhaustiveness checking](./5_ts_narrowing.md#exhaustiveness-checking)).

**Examples:**

```typescript
function fail(msg: string): never {
  throw new Error(msg);
}
```

**Sources:** [TypeScript handbook — never](https://www.typescriptlang.org/docs/handbook/2/functions.html#never)

</details>

---

## Function

<details>
<summary>Function</summary>

**Definition:** The global **`Function`** type describes “some callable” with untyped parameters and an `any` return — **avoid** in application APIs.

**How it works:**

- Prefer `(args: T) => R` or an overload/call signature.
- If you will not call the value, `() => void` is enough.

**Examples:**

```typescript
// Avoid
function doSomething(f: Function) {
  return f(1, 2, 3); // returns any
}

// Prefer
type Binary = (a: number, b: number) => number;
function apply(f: Binary, a: number, b: number) {
  return f(a, b);
}
```

**Sources:** [TypeScript handbook — Function](https://www.typescriptlang.org/docs/handbook/2/functions.html#function)

</details>

---

## Rest Parameters and Arguments

<details>
<summary>Rest Parameters and Arguments</summary>

**Definition:** **Rest parameters** collect extra arguments in a function definition; **rest arguments** (spread) expand an array or tuple into separate arguments at a call site.

**How it works:**

- Definition side: `...name: T[]` must be last in the parameter list.
- Call side: `fn(...items)` passes each element as its own argument.
- Tuple types matter for fixed-arity targets like `Math.atan2` — see [Rest Arguments](#rest-arguments).

**Sources:** [TypeScript handbook — Rest Parameters and Arguments](https://www.typescriptlang.org/docs/handbook/2/functions.html#rest-parameters-and-arguments)

</details>

---

## Rest Parameters

<details>
<summary>Rest Parameters</summary>

**Definition:** A **rest parameter** gathers zero or more trailing arguments into an array (or tuple) inside the function.

**How it works:**

- Annotate as `number[]` or a tuple type for fixed counts.
- Only one rest parameter per function, always last.

**Examples:**

```typescript
function multiply(n: number, ...m: number[]) {
  return m.map((x) => n * x);
}
multiply(10, 1, 2, 3, 4); // [10, 20, 30, 40]
```

**Sources:** [TypeScript handbook — Rest Parameters](https://www.typescriptlang.org/docs/handbook/2/functions.html#rest-parameters)

</details>

---

## Rest Arguments

<details>
<summary>Rest Arguments</summary>

**Definition:** **Spread** at a call site passes iterable elements as separate arguments: `fn(...arr)`.

**How it works:**

- Spreading `number[]` into a function that expects exactly two numbers may fail — the array is not known to be length `2`.
- `as const` on a tuple literal preserves length for the checker.

**Examples:**

```typescript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
arr1.push(...arr2);

const args = [8, 5];
// Math.atan2(...args); // Error — number[], not a 2-tuple

const args2 = [8, 5] as const;
Math.atan2(...args2); // OK
```

**Sources:** [TypeScript handbook — Rest Arguments](https://www.typescriptlang.org/docs/handbook/2/functions.html#rest-arguments)

</details>

---

## Parameter Destructuring

<details>
<summary>Parameter Destructuring</summary>

**Definition:** Unpack object (or array) properties directly in the parameter list, with a type on the whole pattern.

**How it works:**

- Inline: `function sum({ a, b, c }: { a: number; b: number; c: number })`.
- Extract a named type or interface when the shape is reused.
- Works the same as JavaScript destructuring — only the outer value is the parameter.

**Examples:**

```typescript
function sum({ a, b, c }: { a: number; b: number; c: number }) {
  console.log(a + b + c);
}

sum({ a: 10, b: 3, c: 9 });

type ABC = { a: number; b: number; c: number };
function sum2({ a, b, c }: ABC) {
  console.log(a + b + c);
}
```

**Sources:** [TypeScript handbook — Parameter Destructuring](https://www.typescriptlang.org/docs/handbook/2/functions.html#parameter-destructuring)

</details>

---

## Assignability of Functions

<details>
<summary>Assignability of Functions</summary>

**Definition:** Whether a function value may be used where another function type is expected — parameter types are checked **in reverse** (contravariant), return types follow normal assignability.

**How it works:**

- A function with **more** parameters (or optional extras) can be assigned to a type that expects **fewer** — the target may ignore extra args.
- Under `strictFunctionTypes`, parameter types are checked more strictly for function types (methods on objects are bivariant for historical reasons).
- Return type must be assignable to the target return type.

**Examples:**

```typescript
type Handler = (x: string) => void;

const f: Handler = (x: string, y?: number) => {
  console.log(x, y);
}; // OK — extra optional param

const g: Handler = (x: string | number) => {
  console.log(x);
}; // Error under strictFunctionTypes — param not assignable
```

**Sources:** [TypeScript handbook — Assignability of Functions](https://www.typescriptlang.org/docs/handbook/2/functions.html#assignability-of-functions)

</details>

---

## Return type void

<details>
<summary>Return type void</summary>

**Definition:** How **`void`** behaves in **contextual** typing (e.g. callback parameters) vs an explicit **`: void`** return annotation on the implementation.

**How it works:**

- **Contextual** `() => void` on a parameter allows a callback that returns any value — the return is ignored (`const f: () => void = () => true` is OK).
- **Explicit** `function f(): void { return true; }` is an error — you promised not to return a meaningful value.
- `void` is not the same as `undefined` as a return type for assignability in all positions.

**Examples:**

```typescript
type VoidFunc = () => void;
const f1: VoidFunc = () => true; // OK — return discarded

function f2(): void {
  return true; // Error
}

function f3(): void {
  return;
}
```

**Sources:** [TypeScript handbook — Return type void](https://www.typescriptlang.org/docs/handbook/2/functions.html#return-type-void)

</details>
