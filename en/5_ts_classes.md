# TypeScript — Classes

Classes, inheritance, access modifiers, abstract classes, and `implements` — from the [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/2/classes.html).

[← Back to hub](./5_ts.md)

### Related docs

[TypeScript — hub](./5_ts.md) · [Object Types](./5_ts_objects.md) · [Functions](./5_ts_functions.md) · [Type Manipulation](./5_ts_type_manipulation.md) · [OOP in JavaScript](./3_js_objects.md#oop-in-javascript) · [Interview](./5_ts_interview.md)

### Main concepts

| Topic                                         | Covers                                            |
| --------------------------------------------- | ------------------------------------------------- |
| [Class Members](#class-members)               | Fields, constructors, methods                     |
| [Parameter Properties](#parameter-properties) | Constructor shorthand for declared fields         |
| [Member Visibility](#member-visibility)       | `public`, `protected`, `private`, `readonly`, `#` |
| [Inheritance](#inheritance)                   | `extends`, `super`, method overrides              |
| [`implements`](#implements)                   | Class satisfies an interface contract             |
| [Abstract Classes](#abstract-classes)         | Shared base code + required overrides             |
| [Static Members](#static-members)             | Class-level fields and methods                    |

### Sections

- [Class Members](#class-members)
- [Parameter Properties](#parameter-properties)
- [Member Visibility](#member-visibility)
- [Inheritance](#inheritance)
- [`implements`](#implements)
- [Abstract Classes](#abstract-classes)
- [Static Members](#static-members)

---

## Class Members

<details>
<summary>Class Members</summary>

**Definition:** A `class` groups fields, a constructor, and methods. TypeScript adds type annotations on top of ES2015 classes.

**How it works:**

- **Fields** — declare properties with types or initializers (`x: number`, `y = 0`).
- **Constructor** — runs on `new`; assign fields with `this.`; call `super()` before `this` in subclasses.
- **Methods** — same annotations as functions; use `this.` for instance members.
- With `strictPropertyInitialization`, every non-optional field must be initialized in the constructor, at declaration, or with `!` (definite assignment assertion).

**Examples:**

```typescript
class Point {
  x = 0;
  y = 0;

  constructor(x = 0, y = 0) {
    this.x = x;
    this.y = y;
  }

  scale(n: number): void {
    this.x *= n;
    this.y *= n;
  }
}

const pt = new Point(3, 4);
pt.scale(2); // x=6, y=8
```

**Sources:** [TypeScript handbook — Class Members](https://www.typescriptlang.org/docs/handbook/2/classes.html#class-members)

</details>

---

## Parameter Properties

<details>
<summary>Parameter Properties</summary>

**Definition:** A constructor parameter with an access modifier (`public`, `private`, `protected`, or `readonly`) declares and initializes a class field in one line.

**How it works:**

- `constructor(public name: string)` creates `this.name` automatically.
- Combine modifiers: `constructor(private readonly id: string)`.
- Without a modifier, the parameter is only a local variable — no field is created.

**Examples:**

```typescript
class User {
  constructor(
    public name: string,
    public age: number,
    private nickName: string,
  ) {}
}

const dev = new User("RR", 30, "webDev");
dev.name; // OK
// dev.nickName; // Error — private
```

**Sources:** [TypeScript handbook — Parameter Properties](https://www.typescriptlang.org/docs/handbook/2/classes.html#parameter-properties)

</details>

---

## Member Visibility

<details>
<summary>Member Visibility</summary>

**Definition:** Keywords that control who can read or write class members: `public` (default), `protected`, `private`, and `readonly`.

**How it works:**

| Modifier    | Visible to                                   |
| ----------- | -------------------------------------------- |
| `public`    | Everywhere (default — can omit)              |
| `protected` | Class and subclasses                         |
| `private`   | Declaring class only (not even subclasses)   |
| `readonly`  | Assign only at declaration or in constructor |

- TS `private` / `protected` are **compile-time only** — erased in emitted JS.
- ECMAScript `#field` is **runtime-private** — use when you need real hiding.
- Two classes with the same public shape but different `private` members are **not** assignable to each other.

**Examples:**

```typescript
class User {
  public name: string;
  private nickName: string;
  protected age: number;
  readonly pass: number;

  constructor(name: string, age: number, nickName: string, pass: number) {
    this.name = name;
    this.age = age;
    this.nickName = nickName;
    this.pass = pass;
  }
}

class SecureUser {
  #token = "secret";
  getToken() {
    return this.#token;
  }
}
```

**Thoughts:** For library code shipped as JS, prefer `#` when privacy matters at runtime. In app-only TS, `private` is often enough.

**Sources:** [TypeScript handbook — Member Visibility](https://www.typescriptlang.org/docs/handbook/2/classes.html#member-visibility) · [MDN — Private elements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_elements)

</details>

---

## Inheritance

<details>
<summary>Inheritance</summary>

**Definition:** A derived class `extends` a base class, inherits its members, and can add or override behavior.

**How it works:**

- Call `super(...)` in the subclass constructor **before** using `this`.
- Override methods with `super.method()` to call the base implementation.
- Overrides must stay compatible with the base signature — a derived method cannot require extra parameters if callers use a base-class reference.
- JavaScript allows only **one** base class; implement multiple contracts with `implements A, B`.

**Examples:**

```typescript
class Animal {
  move() {
    console.log("Moving along!");
  }
}

class Dog extends Animal {
  woof(times: number) {
    for (let i = 0; i < times; i++) console.log("woof!");
  }
}

class Base {
  greet() {
    console.log("Hello, world!");
  }
}

class Derived extends Base {
  greet(name?: string) {
    if (name === undefined) {
      super.greet();
    } else {
      console.log(`Hello, ${name.toUpperCase()}`);
    }
  }
}
```

**Sources:** [TypeScript handbook — Class Heritage](https://www.typescriptlang.org/docs/handbook/2/classes.html#class-heritage)

</details>

---

## implements

<details>
<summary>implements</summary>

**Definition:** An `implements` clause checks that a class satisfies an `interface` (or type) shape at compile time.

**How it works:**

- `class Sonar implements Pingable` — `tsc` errors if required members are missing.
- A class can `implements A, B` for multiple contracts.
- `implements` is a **type check only** — it does not change inferred parameter types inside methods.
- Optional interface properties are not auto-created on the class — declare them explicitly if needed.
- Extra class members beyond the interface are allowed.

**Examples:**

```typescript
interface Pingable {
  ping(): void;
}

class Sonar implements Pingable {
  ping() {
    console.log("ping!");
  }
}

interface User {
  name: string;
  age: number;
}

interface Pass {
  getPass(): string;
}

class Employee implements User, Pass {
  name = "Ada";
  age = 31;

  getPass() {
    return `${this.name}${this.age}`;
  }
}
```

**Sources:** [TypeScript handbook — implements Clauses](https://www.typescriptlang.org/docs/handbook/2/classes.html#implements-clauses)

</details>

---

## Abstract Classes

<details>
<summary>Abstract Classes</summary>

**Definition:** An `abstract class` cannot be instantiated directly. It may contain both implemented methods and `abstract` members that subclasses must override.

**How it works:**

- Use when several classes share base implementation but differ on specific methods.
- `abstract getPass(): string` — no body; concrete subclass must implement.
- Unlike interfaces, abstract classes can hold fields, constructors, and concrete methods.
- Prefer `interface` for pure contracts; abstract class when you need shared code.

**Examples:**

```typescript
abstract class User {
  constructor(
    public name: string,
    public age: number,
  ) {}

  greet() {
    console.log(this.name);
  }

  abstract getPass(): string;
}

class Child extends User {
  constructor(age: number) {
    super("Yauhen", age);
  }

  getPass() {
    return `${this.age}${this.name}`;
  }
}

const y = new Child(31);
y.greet(); // "Yauhen"
```

**Sources:** [TypeScript handbook — Classes](https://www.typescriptlang.org/docs/handbook/2/classes.html)

</details>

---

## Static Members

<details>
<summary>Static Members</summary>

**Definition:** `static` fields and methods belong to the class constructor, not to individual instances.

**How it works:**

- Access as `MyClass.x` or `MyClass.printX()` — not on instances.
- Static members can use `public`, `protected`, and `private` modifiers.
- Static members are inherited by subclasses (`Derived.getGreeting()`).
- Avoid naming static members `name`, `length`, or `call` — they conflict with `Function` prototype properties.
- Generic class type parameters cannot be used in static members.

**Examples:**

```typescript
class MyClass {
  static x = 0;

  static printX() {
    console.log(MyClass.x);
  }
}

MyClass.x = 1;
MyClass.printX(); // 1

class Base {
  static getGreeting() {
    return "Hello world";
  }
}

class Derived extends Base {
  greeting = Derived.getGreeting();
}
```

**Sources:** [TypeScript handbook — Static Members](https://www.typescriptlang.org/docs/handbook/2/classes.html#static-members)

</details>
