# JS Core — Classes

`class` syntax, inheritance, static members, private/protected `#`, built-in `extends`, `instanceof`, getters/setters, and mixins.

### Related docs

- [JS Language](./3_js_core.md) · [Objects](./3_js_objects.md) · [Functions](./3_js_functions.md) · [JS Interview](./3_js_interview.md)
- [OOP & SOLID](./7_special_engineering.md#object-oriented-programming-oop) · [Classes (TypeScript)](./5_ts_classes.md)

[← Back to JS hub](./3_js.md)

### Main concepts

| Area              | Topics                                       |
| ----------------- | -------------------------------------------- |
| **Class basics**  | Syntax, static members                       |
| **Inheritance**   | `extends` / `super`, built-ins, `instanceof` |
| **Encapsulation** | Private `#`, getters / setters               |
| **Patterns**      | Mixins, constructor injection                |

### Sections

> Theory `<details>` blocks follow: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**. **Definition** is required; other sections are optional.

- [Class basics](#class-basics)
- [Inheritance](#inheritance)
- [Encapsulation](#encapsulation)
- [Patterns](#patterns)
- **Class basics:** [Class syntax](#class-syntax) · [Static methods and properties](#static-methods-and-properties)
- **Inheritance:** [Inheritance and `super`](#inheritance-and-super) · [Extending built-in classes](#extending-built-in-classes) · [Class checking: `instanceof`](#class-checking-instanceof)
- **Encapsulation:** [Private and protected properties and methods](#private-and-protected-properties-and-methods) · [Getters and setters](#getters-and-setters)
- **Patterns:** [Mixins](#mixins) · [Constructor injection](#constructor-injection)
- [Interview](./3_js_interview.md)

Sibling Core topics: [Objects](./3_js_objects.md) · [Arrays](./3_js_array.md) · [Functions](./3_js_functions.md).

<a name="classes-full"></a>

## Class basics

### Class basics

<details>
  <summary>Class syntax</summary>

### Class syntax

**Definition:** `class` declares a constructor function and puts methods on its `.prototype`. `typeof MyClass === 'function'`.

**How it works:**

- `class Name {}` creates function `Name` in TDZ until line runs; body always strict.
- Instance methods live on `Name.prototype` (non-enumerable); static methods on `Name` itself.
- Calling class without `new` throws `TypeError`.

1. Creates function `User` from `constructor` (or empty if missing)
2. Stores methods like `sayHi` on `User.prototype`

**When to use:**

Domain entities; React class components (legacy); service classes with instance state.

#### Common pitfalls

Using class before declaration (TDZ); expecting `class` to hoist like `function`; thinking private `#` fields are on prototype (they are per-instance).

| Feature            | Behavior                            |
| ------------------ | ----------------------------------- |
| Strict mode        | Always on inside class bodies       |
| Methods            | Non-enumerable by default           |
| Call without `new` | TypeError                           |
| Hoisting           | Hoisted but in TDZ until definition |

**Pluses and minuses:**

| +                                  | −                                                    |
| ---------------------------------- | ---------------------------------------------------- |
| Familiar syntax, `extends`/`super` | Still prototypes underneath — surprises in debugging |
| Strict by default                  | TDZ — cannot use before declaration                  |
| Clear constructor/method split     | Not hoisted like `function` declarations             |

**Examples:**

```js
// ES5 constructor
function Bike(model, color) {
  this.model = model;
  this.color = color;
}
Bike.prototype.getDetails = function () {
  return `${this.model} bike has ${this.color} color`;
};

// ES6 class — equivalent pattern
class BikeClass {
  constructor(model, color) {
    this.model = model;
    this.color = color;
  }
  getDetails() {
    return `${this.model} bike has ${this.color} color`;
  }
}
```

**Thoughts:** Classes are prototype inheritance with cleaner syntax — not a new object model.

**Sources:**

[javascript.info/class](https://javascript.info/class)

#### Class declarations vs expressions

**Definition:**

Classes can be declared or expressed, named or anonymous — similar to functions.

| Form             | Example                                                     |
| ---------------- | ----------------------------------------------------------- |
| Declaration      | `class Foo {}`                                              |
| Expression       | `const Foo = class {}`                                      |
| Named expression | `const Foo = class Bar {}` — `Bar` visible inside body only |

**How it works:** Classes are hoisted but uninitialized — `new Rectangle()` before `class Rectangle {}` throws `ReferenceError`.

**Examples:**

```js
// Named expression
const Rect = class Rectangle {
  constructor(w, h) {
    this.w = w;
    this.h = h;
  }
};
console.log(Rect.name); // 'Rectangle'
```

**Sources:** [javascript.info/class](https://javascript.info/class)

</details>

<details>
  <summary>Static methods and properties</summary>

### Static methods and properties

**Definition:** Static members belong to the class itself, not instances. Called as `ClassName.method()`.

**How it works:**

- `static` keyword defines properties on the constructor function object, not on instances.
- Accessed as `ClassName.method()`; inherited statics available on subclasses via prototype chain of constructor.

**When to use:**

Comparison/sort helpers; named constructors; parsing factories; constants scoped to type.

Comparison utilities, factories, constants — anything not tied to a single instance.

#### Common pitfalls

Calling static method on instance (`user.createGuest()` — undefined); static block (ES2022) runs once — side effects persist.

| Static                    | Instance            |
| ------------------------- | ------------------- |
| `User.createGuest()`      | `user.sayHi()`      |
| Class-level utilities     | Per-object behavior |
| Factory methods, counters | Instance state      |

**Pluses and minuses:**

| +                                      | −                                             |
| -------------------------------------- | --------------------------------------------- |
| Namespace for utilities                | Can become grab-bag if overused               |
| Factory methods (`User.createGuest()`) | Harder to mock than instance methods in tests |
| Counters/constants on class            | Static-only state is global-like              |

**Examples:**

```js
class Article {
  constructor(title) {
    this.title = title;
  }

  static compare(a, b) {
    return a.title.localeCompare(b.title);
  }

  static createToday(title) {
    return new Article(title);
  }
}

Article.compare(new Article("A"), new Article("B"));
```

**Sources:** [javascript.info/static-properties-methods](https://javascript.info/static-properties-methods)

</details>

---

## Inheritance

### Class inheritance

<details>
  <summary>Inheritance and `super`</summary>

### Inheritance and `super`

**Definition:** `class Child extends Parent` sets `Child.prototype.__proto__` to `Parent.prototype`. `super` calls parent constructor or methods.

**How it works:**

- `extends` sets up prototype chain: `Child.prototype.__proto__` → `Parent.prototype`.
- `super()` in constructor calls `Parent` as constructor with current `this`.
- `super.method()` uses home object mechanism to call parent version.

**When to use:**

`Dog extends Animal`; overriding with `super.method()` for extension not replacement. Built-in `extends` → [Extending built-in classes](#extending-built-in-classes).

#### Common pitfalls

Using `this` before `super()` in derived constructor; forgetting `super()` when derived class has constructor; diamond problem — JS has no multiple inheritance.

| Use case                | Syntax                             |
| ----------------------- | ---------------------------------- |
| Call parent constructor | `super(args)` in child constructor |
| Call parent method      | `super.method()` in override       |
| Static inheritance      | `super` in static methods          |

**Pluses and minuses:**

| +                                | −                                                        |
| -------------------------------- | -------------------------------------------------------- |
| Built-in `extends`/`super`       | Single inheritance only                                  |
| `instanceof` works through chain | Deep hierarchies fragile                                 |
| Method override with parent call | Must call `super()` before `this` in derived constructor |

**Examples:**

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    return `${this.name} makes a sound`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }
  speak() {
    return `${super.speak()} (${this.breed})`;
  }
}

const d = new Dog("Rex", "lab");
console.log(d.speak());
```

**Thoughts:**

In a derived constructor, `this` is inaccessible before `super()` — runtime error.

JavaScript has single inheritance. For multiple behavior sources, use composition or mixins.

**Sources:** [javascript.info/class-inheritance](https://javascript.info/class-inheritance)

</details>

<details>
  <summary>Extending built-in classes</summary>

### Extending built-in classes

**Definition:** `class MyError extends Error` or `class PowerArray extends Array` keeps native internal behavior (stack, `length`, iterators) while adding domain methods. Full treatment (species, pitfalls, `HttpError` / `PowerArray` examples): [Objects — Extending built-in classes](./3_js_objects.md#extending-built-in-classes).

**How it works:**

- Derived constructor must call `super(...)` before using `this`.
- Methods like `map` / `filter` may return plain `Array` unless you override `static get [Symbol.species]()`.
- Custom `Error` subclasses need `super(message)` and usually `this.name = "MyError"` for useful stacks.

**When to use:** Typed errors, extended collections, framework wrappers around DOM builtins (where supported).

**Examples:**

```js
class HttpError extends Error {
  constructor(status, message) {
    super(message);
    this.name = "HttpError";
    this.status = status;
  }
}

throw new HttpError(404, "Not found");
```

**Sources:** [javascript.info/extend-natives](https://javascript.info/extend-natives) · [Objects — Extending built-in classes](./3_js_objects.md#extending-built-in-classes)

</details>

<details>
  <summary>Class checking: instanceof</summary>

### Class checking: `instanceof`

**Definition:** `obj instanceof Constructor` is `true` when `Constructor.prototype` appears anywhere on `obj`’s `[[Prototype]]` chain — so subclass instances match parent constructors too.

**How it works:**

- Works with `class` because `extends` wires `Child.prototype` → `Parent.prototype`.
- Custom constructors can define `static [Symbol.hasInstance](obj)` for branded checks.
- Unreliable across iframes/realms; not for primitives. Deep dive: [Objects — `instanceof`](./3_js_objects.md#instanceof).

**When to use:** Guarding polymorphic APIs; distinguishing `Error` subclasses; quick type checks in app code (prefer TS or schemas at boundaries).

**Examples:**

```js
class Animal {}
class Rabbit extends Animal {}

const r = new Rabbit();
console.log(r instanceof Rabbit); // true
console.log(r instanceof Animal); // true
console.log(r instanceof Object); // true
```

**Sources:** [javascript.info/instanceof](https://javascript.info/instanceof) · [Objects — `instanceof`](./3_js_objects.md#instanceof)

</details>

---

## Encapsulation

### Encapsulation

<details>
  <summary>Private and protected properties and methods</summary>

### Private and protected properties and methods

**Definition:** **Private** members use `#` — fields and methods visible only inside the declaring class body (ES2022). JavaScript has **no `protected` keyword**; “protected” behavior is a **convention** (`_name`) or **controlled exposure** via getters / `super` hooks. For compile-time `protected`, see [Classes (TypeScript)](./5_ts_classes.md).

**How it works:**

- `#field` / `#method()` live in per-instance private slots; access outside the class is a `SyntaxError`.
- A subclass **does not** inherit access to a parent’s `#member` — redeclare `#x` in the child is a **different** private name.
- **Protected-style API:** public `this._cache` documented for subclasses, or `get _hook()` used only in overrides; prefer composition when possible.
- Private **methods** factor internal logic (`#validate()`) without exposing helpers on `.prototype`.

**When to use:** Internal state (`#balance`); helpers that must not be part of the public API; subclass extension points via public/`_` methods or `super`, not parent `#` fields.

#### Common pitfalls

Expecting `#parentSecret` in `extends Child`; `#` in object literals; JSON.stringify drops `#` data; DevTools may show `#` for debugging — not a language escape hatch.

**Pluses and minuses:**

| +                               | −                                          |
| ------------------------------- | ------------------------------------------ |
| Real private fields and methods | No cross-class `#` access in subclasses    |
| No `_prefix` for true privacy   | “Protected” is discipline only in plain JS |
| Works with getters for exposure | TS `protected` needs the type checker      |

**Examples:**

```js
class Account {
  #balance = 0;

  #assertPositive(amount) {
    if (amount <= 0) throw new RangeError("amount");
  }

  deposit(amount) {
    this.#assertPositive(amount);
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}

class LoggedAccount extends Account {
  // cannot read parent #balance — use public API
  deposit(amount) {
    console.log("deposit", amount);
    super.deposit(amount);
  }
}
```

**Sources:** [javascript.info/private-protected-properties-methods](https://javascript.info/private-protected-properties-methods) · [MDN — private elements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_elements) · [Classes (TypeScript)](./5_ts_classes.md)

</details>

<details>
  <summary>Getters and setters</summary>

### Getters and setters

**Definition:** Accessor properties use `get` / `set` to control read/write access — they look like normal properties in external code.

**How it works:**

- Defined with `get prop()` / `set prop(v)` on object or class; accessed like normal properties.
- Descriptor form: `{ get, set, enumerable, configurable }` — no `value`/`writable` with accessors.
- Setters often validate and store in private `#field` or `_field`.

**When to use:**

Lazy computed values; input validation; exposing read-only derived state; compatibility when renaming internal field.

#### Common pitfalls

`get name() { return this.name }` — infinite loop; forgetting setter when adding getter for writable API; enumerable getters show in `for...in`.

**Pluses and minuses:**

| +                                     | −                                   |
| ------------------------------------- | ----------------------------------- |
| Validation on set                     | Slight overhead vs plain properties |
| Computed values on get                | Can complicate debugging            |
| Encapsulation with private `#` fields | Overuse adds boilerplate            |

| +                                 | −                                         |
| --------------------------------- | ----------------------------------------- |
| Validation on assignment          | Harder to trace in debugger than methods  |
| Computed read-only properties     | Infinite recursion if setter calls itself |
| Backward-compatible API evolution | Slight perf cost vs data properties       |

**Examples:**

```js
class User {
  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name;
  }

  set name(value) {
    if (typeof value !== "string" || !value.trim()) {
      throw new TypeError("Invalid name");
    }
    this._name = value.trim();
  }
}

const u = new User("Ann");
u.name = "  Bob  ";
console.log(u.name); // 'Bob'
```

**Sources:** [javascript.info/property-accessors](https://javascript.info/property-accessors)

</details>

---

## Patterns

### Composition patterns

<details>
  <summary>Mixins</summary>

### Mixins

**Definition:** A mixin is a class whose methods are copied into other classes without inheritance — JavaScript supports single inheritance only.

**How it works:**

- Copy method objects onto `Class.prototype` via `Object.assign` or helper function.
- No `super` chain between mixin and class — methods merge flatly; name collisions last wins.

**When to use:**

Cross-cutting concerns (serialization, event emitter); legacy code sharing; when `extends` is wrong semantically.

#### Common pitfalls

Two mixins defining same method; mixin assuming `this` shape without contract; prefer composition (`has-a`) for new code.

| Approach                             | When                                    |
| ------------------------------------ | --------------------------------------- |
| `extends`                            | True `is-a` subtype                     |
| Mixin (`Object.assign` to prototype) | Share behavior across unrelated classes |
| Composition                          | `has-a` — preferred for flexibility     |

**Pluses and minuses:**

| +                                  | −                                |
| ---------------------------------- | -------------------------------- |
| Share behavior without inheritance | Method name collisions           |
| Multiple behaviors on one class    | No `super` to mixin              |
| Works with unrelated classes       | Harder to trace than composition |

**Examples:**

```js
const sayHi = {
  sayHi() {
    return `Hi, ${this.name}`;
  },
};

const sayBye = {
  sayBye() {
    return `Bye, ${this.name}`;
  },
};

class User {
  constructor(name) {
    this.name = name;
  }
}

Object.assign(User.prototype, sayHi, sayBye);

const u = new User("Ann");
console.log(u.sayHi()); // 'Hi, Ann'
```

**Thoughts:** Without interfaces, mixins are untyped feature bags. TypeScript `implements` makes composition readable.

**Sources:**

[javascript.info/mixins](https://javascript.info/mixins) · [Doka — OOP](https://doka.guide/tools/oop/)

</details>

<a name="constructor-injection"></a>

<details>
  <summary>Constructor injection</summary>

### Constructor injection

**Definition:** Pass collaborators into `constructor` and store them on `this`, instead of constructing them inside the class. The class that _needs_ others is the dependent.

**How it works:**

- `DailyAmount` needs Budget and History — receive them, don't `new` them inside.
- Dependency arrows stay one-way (matches a UML diagram) and avoid **cycles** (A constructs B constructs A).
- Tests pass fakes: `new DailyAmount(budgetStub, historyStub)`.
- This is manual DIP. JS has no interfaces, so the "abstraction" is a duck-typed shape unless you add TypeScript.

**When to use:**

Domain services that combine entities (`DailyAmount`); any class that would otherwise `import` a concrete logger/DB.

#### Common pitfalls

Constructing deps inside the class (hidden coupling); circular `new` between two classes; injecting concretions when a small interface would do (TS).

**Pluses and minuses:**

| +                                 | −                                            |
| --------------------------------- | -------------------------------------------- |
| Testable; swap implementations    | Wiring at the composition root               |
| Clear ownership of collaborators  | Easy to pass too many deps (god constructor) |
| Matches domain relation direction | Without types, nothing checks the shape      |

**Examples:**

```js
class DailyAmount {
  constructor(budget, history) {
    this.budget = budget;
    this.history = history;
  }
  get valueOf() {
    return this.budget.perDay - this.history.spentToday;
  }
}

const daily = new DailyAmount(new Budget(10000, 10), new History());
```

**Thoughts:** High-level policy receives tools; it does not build them.

**Sources:** [Doka — OOP](https://doka.guide/tools/oop/) · [7_special.md — DIP](./7_special_engineering.md#9-oop-and-solid)

</details>

---

<a name="advanced-topics"></a>
