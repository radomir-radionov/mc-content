# TypeScript — Mixins

Композиция классов из частичных блоков.

[← Назад к хабу](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Интервью](./5_ts_interview.md)

### Sections

- [Mixins](#mixins)

---


## Mixins

<details>
  <summary>Mixins</summary>

### Mixins

**Definition:**

Функции, которые принимают класс и возвращают расширенный класс, формируют поведение без множественного наследования.

| Когда использовать | Заметки |
|-------------|-------|
| `` class User extends WithId(WithTimestamp(Entity)) `` — стек фабрик миксинов. | Лучше всего поддерживается через **выражение класса**, возвращаемое из универсальной фабрики, а не через декораторы (миксины декораторов не объединяют типы в класс). |

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Class expression pattern

**Definition:**

Шаблон выражения класса — язык TypeScript или функция компилятора.

| Аспект | Деталь |
|--------|--------|
| Резюме | Шаблон выражения класса — язык TypeScript или функция компилятора. |

**Ограниченные миксины** — общее базовое ограничение гарантирует наличие необходимых методов:

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

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)



</details>
