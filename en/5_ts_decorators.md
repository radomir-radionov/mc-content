# TypeScript — Decorators

Stage 2 and stage 3 decorators for classes, methods, properties, and parameters.

[← Back to hub](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Interview](./5_ts_interview.md)

### Sections

- [Decorators](#decorators)

---


## Decorators

<details>
  <summary>Decorators</summary>

### Decorators

**Definition:** Functions attached with `@` to classes, methods, properties, accessors, or parameters.

**How it works:**

| Framework | Examples |
|-----------|----------|
| **Angular** | `@Component`, `@Injectable` |
| **NestJS** | `@Controller`, `@Get`, `@Post` |
| **TypeORM** | `@Entity`, `@Column` |

Decorators wrap classes/members to add logging, validation, DI, or caching **without changing business logic**.

**When to use:** Enable per project (stage 2 `experimentalDecorators` or TS 5+ stage 3). Used heavily in NestJS, Angular, TypeORM.

**Thoughts:** The handbook documents **stage 2 (experimental)** decorators (`experimentalDecorators: true`). **TypeScript 5.0+** added **stage 3** decorators — different syntax and semantics; check which your project uses.

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Key concepts

**Definition:**

// Composition: factories evaluated top-to-bottom,

// decorator functions called bottom-to-top
@first()
@second()
method() {}
// Output: first(): factory → second(): factory → second(): called → first(): called

**How it works:**

class, method, property, accessor, parameter decorators.

| Decorator | Receives | Notes |
|-----------|----------|-------|
| **Class** | `constructor` | If it returns a value, replaces the class declaration |
| **Property** | `target` (prototype), `propertyKey` | Cannot use in `.d.ts` / ambient context |
| **Method** | `target`, `propertyKey`, `descriptor` | Can observe, modify, or replace method |
| **Accessor** | same as method | Applied to get/set `PropertyDescriptor` |
| **Parameter** | constructor or method params | Metadata only — limited runtime use |

See [Decorators in TypeScript 5.0](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators) — no `experimentalDecorators` needed; aligns with ECMAScript standard.

[Decorators](https://www.typescriptlang.org/docs/handbook/decorators.html)

`instanceof` is a **type guard** for class instances (see Type Narrowing) — not a decorator. It narrows `unknown`/`union` to a specific class inside `if (x instanceof Foo)`.

**Examples:**

```typescript
// Decorator factory — returns the actual decorator
function color(value: string) {
  return function (target: object) {
    // use target and value
  };
}

// Composition: factories evaluated top-to-bottom,
// decorator functions called bottom-to-top
@first()
@second()
method() {}
// Output: first(): factory → second(): factory → second(): called → first(): called

// Evaluation order inside a class:
// 1. Instance member decorators (per member)
// 2. Static member decorators
// 3. Constructor parameter decorators
// 4. Class decorator
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Decorator examples — all four kinds

**Definition:**

// Property decorator — target is prototype, propertyKey is field name

const logProperty = (target: object, propertyKey: string | symbol) => {
  console.log(propertyKey); // "secret"
};

| Aspect | Detail |
|--------|--------|
| Summary | // Property decorator — target is prototype, propertyKey is field name
const logProperty = (target: object, propertyKey: |

**Examples:**

```typescript
// Class decorator — logs constructor; return value would replace class
const logClass = (constructor: Function) => {
  console.log(constructor); // class User {}
};

// Property decorator — target is prototype, propertyKey is field name
const logProperty = (target: object, propertyKey: string | symbol) => {
  console.log(propertyKey); // "secret"
};

// Method decorator — receives PropertyDescriptor
const logMethod = (
  target: object,
  propertyKey: string | symbol,
  descriptor: PropertyDescriptor,
) => {
  console.log(propertyKey); // "getPass"
};

// Accessor decorator — on get/set
const logSet = (
  target: object,
  propertyKey: string | symbol,
  descriptor: PropertyDescriptor,
) => {
  console.log(propertyKey); // "myAge"
};

@logClass
class User {
  @logProperty
  secret: number;

  constructor(public name: string, public age: number, secret: number) {
    this.secret = secret;
  }

  @logMethod
  getPass() {
    return `${this.name}${this.age}`;
  }

  @logSet
  set myAge(age: number) {
    this.age = age;
  }
}
```

```typescript
// Parameter decorator — metadata on constructor/method params (NestJS DI uses this pattern)
function logParameter(
  _target: object,
  propertyKey: string | symbol,
  parameterIndex: number,
) {
  console.log(`Param ${parameterIndex} of ${String(propertyKey)}`);
}

class Account {
  constructor(
    public name: string,
    @logParameter public balance: number,
  ) {}
}

// Class decorator that returns a constructor REPLACES the class declaration
function Timestamped<T extends new (...args: any[]) => object>(Base: T) {
  return class extends Base {
    createdAt = Date.now();
  };
}

@Timestamped
class User {
  constructor(public name: string) {}
}

const u = new User("Alice");
// u.name + u.createdAt — subclass instance, not plain User
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Decorator examples — interview patterns

**Definition:**

// Method decorator — receives PropertyDescriptor

const logMethod = (
  _target: object,
  propertyKey: string | symbol,
  descriptor: PropertyDescriptor,
) => {
  const original = descriptor.value;
  descriptor.value = function (...args: unknown[]) {
    console.log(`Calling ${String(propertyKey)}`);
    return original.apply(this, args);
  };
};

| Aspect | Detail |
|--------|--------|
| Summary | // Method decorator — receives PropertyDescriptor
const logMethod = (
  _target: object,
  propertyKey: string \| symbol |

**Examples:**

```typescript
// Class decorator — receives constructor
const logClass = (constructor: Function) => {
  console.log(constructor.name);
};

// Method decorator — receives PropertyDescriptor
const logMethod = (
  _target: object,
  propertyKey: string | symbol,
  descriptor: PropertyDescriptor,
) => {
  const original = descriptor.value;
  descriptor.value = function (...args: unknown[]) {
    console.log(`Calling ${String(propertyKey)}`);
    return original.apply(this, args);
  };
};

// Decorator factory — factory runs first (top-to-bottom), decorators bottom-to-top
const enumerable = (value: boolean) => (
  _target: object,
  _key: string | symbol,
  descriptor: PropertyDescriptor,
) => {
  descriptor.enumerable = value;
};

@logClass
class User {
  constructor(public name: string) {}

  @logMethod
  @enumerable(false)
  getPass() {
    return this.name;
  }
}
// Factory order: logMethod factory → enumerable factory → enumerable called → logMethod called
```

```typescript
// AOP logging decorator — frameworks like NestJS use similar patterns
function Log(
  _target: object,
  propertyKey: string,
  descriptor: PropertyDescriptor,
) {
  const original = descriptor.value;
  descriptor.value = function (...args: unknown[]) {
    console.log(`Call: ${propertyKey}(${args.join(", ")})`);
    return original.apply(this, args);
  };
}

class Service {
  @Log
  getData(id: number) {
    return { id, name: "Test" };
  }
}

new Service().getData(42); // Console: Call: getData(42)
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Decorator composition — exact execution order

**Definition:**

Decorator composition — exact execution order — TypeScript language or compiler feature.

| Aspect | Detail |
|--------|--------|
| Summary | Decorator composition — exact execution order — TypeScript language or compiler feature. |

---

**Examples:**

```typescript
const first = () => {
  console.log("first() completing");
  return () => console.log("first() called");
};
const second = () => {
  console.log("second() completing");
  return () => console.log("second() called");
};

class User {
  @first()
  @second()
  getPass() {
    return "pass";
  }
}
// Console output:
// "first() completing"   ← factory 1 evaluated (top to bottom)
// "second() completing"  ← factory 2 evaluated
// "second() called"      ← decorator 2 applied (bottom to top)
// "first() called"       ← decorator 1 applied
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)



</details>
