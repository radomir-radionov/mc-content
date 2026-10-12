# TypeScript — Mixins

Compose classes from partial building blocks with intersection types.

[← Back to hub](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Interview](./5_ts_interview.md)

### Sections

- [Mixins](#mixins)

---


## Mixins

<details>
  <summary>Mixins</summary>

### Mixins

**Definition:**

Functions that take a class and return an extended class — compose behavior without multiple inheritance.

| When to use | Notes |
|-------------|-------|
| `` class User extends WithId(WithTimestamp(Entity)) `` — stack mixin factories. | Best supported via **class expression** return from a generic factory — not via decorators (decorator mixins don't merge types onto the class). |

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)

### Class expression pattern

**Definition:**

Class expression pattern — TypeScript language or compiler feature.

| Aspect | Detail |
|--------|--------|
| Summary | Class expression pattern — TypeScript language or compiler feature. |

**Constrained mixins** — generic base constraint ensures required methods exist:

---

**Examples:**

```typescript
type Constructor = new (...args: any[]) => {};

function Scale<TBase extends Constructor>(Base: TBase) {
  return class Scaling extends Base {
    _scale = 1;
    setScale(scale: number) { this._scale = scale; }
    get scale() { return this._scale; }
  };
}

class Sprite {
  name = "";
  constructor(name: string) { this.name = name; }
}

const EightBitSprite = Scale(Sprite);
const bird = new EightBitSprite("Bird");
bird.setScale(0.8);
```

```typescript
type GConstructor<T = {}> = new (...args: any[]) => T;
type Positionable = GConstructor<{ setPos(x: number, y: number): void }>;

function Jumpable<TBase extends Positionable>(Base: TBase) {
  return class extends Base {
    jump() { this.setPos(0, 20); }
  };
}
```

```typescript
// Stacking mixins — WithId + WithTimestamp
type Constructor<T = object> = new (...args: any[]) => T;

function WithId<TBase extends Constructor>(Base: TBase) {
  return class extends Base {
    id = Math.random().toString(36).slice(2);
  };
}

function WithTimestamp<TBase extends Constructor>(Base: TBase) {
  return class extends Base {
    timestamp = Date.now();
  };
}

class Entity {}

class User extends WithId(WithTimestamp(Entity)) {
  name = "Alice";
}

const u = new User();
// u.id, u.timestamp, u.name — all present
```

**Sources:** [TypeScript handbook — intro](https://www.typescriptlang.org/docs/handbook/intro.html)



</details>
