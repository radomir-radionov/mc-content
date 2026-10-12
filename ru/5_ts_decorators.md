# TypeScript — Decorators

Декораторы stage 2 и stage 3 для классов и членов.

[← Назад к хабу](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Интервью](./5_ts_interview.md)

### Sections

- [Decorators](#decorators)

---


## Decorators

<details>
  <summary>Decorators</summary>

### Decorators

**Definition:** Функции, связанные с `@` к классам, методам, свойствам, средствам доступа или параметрам.

**How it works:**

| Рамочная | Примеры |
|-----------|----------|
| **Угловой** | `@Component`, `@Injectable` |
| **NestJS** | `@Controller`, `@Get`, `@Post` |
| **ТипORM** | `@Entity`, `@Column` |

Декораторы оборачивают классы/члены для добавления журналирования, проверки, внедрения внедрения или кэширования **без изменения бизнес-логики**.

**When to use:** Включить для каждого проекта (этап 2) `experimentalDecorators` или ТС 5+ стадия 3). Активно используется в NestJS, Angular, TypeORM.

**Thoughts:** В справочнике описаны декораторы **этапа 2 (экспериментальный)** (`experimentalDecorators: true`). В **TypeScript 5.0+** добавлены декораторы **этапа 3** — другой синтаксис и семантика; проверьте, какой из них использует ваш проект.

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Key concepts

**Definition:**

// Состав: фабрики оцениваются сверху вниз,

// функции декоратора, вызываемые снизу вверх
@first()
@секунда()
метод() {}
// Вывод: first(): фабрика → секунда(): фабрика → секунда(): вызвано → first(): вызвано

**How it works:**

класс, метод, свойство, метод доступа, декораторы параметров.

| Декоратор | Получает | Заметки |
|-----------|----------|-------|
| **Класс** | `constructor` | Если он возвращает значение, заменяет объявление класса |
| **Недвижимость** | `target` (прототип), `propertyKey` | Невозможно использовать в `.d.ts` / окружающий контекст |
| **Метод** | `target`, `propertyKey`, `descriptor` | Можно наблюдать, изменять или заменять метод |
| **Доступ** | то же, что и метод | Применяется для получения/установки `PropertyDescriptor` |
| **Параметр** | параметры конструктора или метода | Только метаданные — ограниченное использование во время выполнения |

См. [Декораторы в TypeScript 5.0](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-0.html#decorators) — нет `experimentalDecorators` нужный; соответствует стандарту ECMAScript.

[Декораторы](https://www.typescriptlang.org/docs/handbook/decorators.html)

`instanceof` является **защитником типа** для экземпляров класса (см. Сужение типов), а не декоратором. Это сужает `unknown`/`union` к определенному классу внутри `if (x instanceof Foo)`.

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

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Decorator examples — all four kinds

**Definition:**

// Декоратор свойства — цель — прототип, propertyKey — имя поля

const logProperty = (цель: объект, propertyKey: строка | символ) => {
  console.log(propertyKey); // "секрет"
};

| Аспект | Деталь |
|--------|--------|
| Резюме | // Декоратор свойства — цель — прототип, propertyKey — имя поля |
const logProperty = (цель: объект, propertyKey: |

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

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Decorator examples — interview patterns

**Definition:**

// Декоратор метода — получает PropertyDescriptor

const logMethod = (
  _target: объект,
  propertyKey: строка | символ,
  дескриптор: PropertyDescriptor,
) => {
  const original = descriptor.value;
  descriptor.value = функция (...args: неизвестно []) {
    консоль.log(`Calling ${String(propertyKey)}`);
    вернуть original.apply(this, args);
  };
};

| Аспект | Деталь |
|--------|--------|
| Резюме | // Декоратор метода — получает PropertyDescriptor |
const logMethod = (
  _target: объект,
  PropertyKey: строка \| символ |

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

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)

### Decorator composition — exact execution order

**Definition:**

Состав декоратора — точный порядок выполнения — язык TypeScript или функция компилятора.

| Аспект | Деталь |
|--------|--------|
| Резюме | Состав декоратора — точный порядок выполнения — язык TypeScript или функция компилятора. |

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

**Sources:** [Справочник по TypeScript — введение](https://www.typescriptlang.org/docs/handbook/intro.html)



</details>
