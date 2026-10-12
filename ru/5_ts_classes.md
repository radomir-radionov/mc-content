# TypeScript — Classes

Поля, наследование, модификаторы доступа, абстрактные классы и `implements` — по [справочнику TypeScript](https://www.typescriptlang.org/docs/handbook/2/classes.html).

[← Back to hub](./5_ts.md)

### Related docs

[TypeScript — hub](./5_ts.md) · [Object Types](./5_ts_objects.md) · [Functions](./5_ts_functions.md) · [Type Manipulation](./5_ts_type_manipulation.md) · [OOP in JavaScript](./3_js_objects.md#oop-in-javascript) · [Interview](./5_ts_interview.md)

### Main concepts

| Topic                                         | Covers                                            |
| --------------------------------------------- | ------------------------------------------------- |
| [Class Members](#class-members)               | Поля, конструкторы, методы                        |
| [Parameter Properties](#parameter-properties) | Сокращённое объявление полей в конструкторе       |
| [Member Visibility](#member-visibility)       | `public`, `protected`, `private`, `readonly`, `#` |
| [Inheritance](#inheritance)                   | `extends`, `super`, переопределение методов       |
| [`implements`](#implements)                   | Класс выполняет контракт интерфейса               |
| [Abstract Classes](#abstract-classes)         | Общий базовый код + обязательные override         |
| [Static Members](#static-members)             | Поля и методы на уровне класса                    |

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

**Definition:** `class` объединяет поля, конструктор и методы. TypeScript добавляет аннотации типов поверх классов ES2015.

**How it works:**

- **Fields** — свойства с типами или инициализаторами (`x: number`, `y = 0`).
- **Constructor** — выполняется при `new`; присваивайте поля через `this.`; в подклассах вызывайте `super()` до `this`.
- **Methods** — те же аннотации, что у функций; для членов экземпляра используйте `this.`.
- При `strictPropertyInitialization` каждое неопциональное поле должно быть инициализировано в конструкторе, при объявлении или через `!` (definite assignment assertion).

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

**Definition:** Параметр конструктора с модификатором доступа (`public`, `private`, `protected` или `readonly`) объявляет и инициализирует поле класса в одной строке.

**How it works:**

- `constructor(public name: string)` автоматически создаёт `this.name`.
- Модификаторы можно комбинировать: `constructor(private readonly id: string)`.
- Без модификатора параметр — только локальная переменная; поле не создаётся.

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

**Definition:** Ключевые слова, задающие, кто может читать или писать члены класса: `public` (по умолчанию), `protected`, `private` и `readonly`.

**How it works:**

| Modifier    | Visible to                                          |
| ----------- | --------------------------------------------------- |
| `public`    | Везде (по умолчанию — можно опустить)               |
| `protected` | Класс и подклассы                                   |
| `private`   | Только объявляющий класс (даже подклассы — нет)     |
| `readonly`  | Присвоение только при объявлении или в конструкторе |

- TS `private` / `protected` — **только на этапе компиляции**; в emit стираются.
- ECMAScript `#field` — **runtime-private**; используйте, когда нужна реальная изоляция.
- Два класса с одинаковой public-формой, но разными `private`-членами, **не** взаимозаменяемы.

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

**Thoughts:** Для библиотек, публикуемых как JS, при необходимости runtime-приватности предпочитайте `#`. В app-only TS часто хватает `private`.

**Sources:** [TypeScript handbook — Member Visibility](https://www.typescriptlang.org/docs/handbook/2/classes.html#member-visibility) · [MDN — Private elements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_elements)

</details>

---

## Inheritance

<details>
<summary>Inheritance</summary>

**Definition:** Производный класс `extends` базовый, наследует его члены и может добавлять или переопределять поведение.

**How it works:**

- Вызывайте `super(...)` в конструкторе подкласса **до** использования `this`.
- Переопределяйте методы с `super.method()` для вызова базовой реализации.
- Override должны оставаться совместимы с базовой сигнатурой — производный метод не может требовать лишних параметров, если вызывающий код держит ссылку на базовый тип.
- В JavaScript только **один** базовый класс; несколько контрактов — через `implements A, B`.

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

**Definition:** Клауза `implements` проверяет, что класс удовлетворяет форме `interface` (или type) на этапе компиляции.

**How it works:**

- `class Sonar implements Pingable` — `tsc` выдаст ошибку, если не хватает обязательных членов.
- Класс может `implements A, B` для нескольких контрактов.
- `implements` — **только проверка типов**; не меняет вывод типов параметров внутри методов.
- Опциональные свойства интерфейса не создаются автоматически — объявите их явно, если нужны.
- Дополнительные члены класса сверх интерфейса разрешены.

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

**Definition:** `abstract class` нельзя инстанцировать напрямую. В нём могут быть и реализованные методы, и `abstract`-члены, которые подкласс обязан переопределить.

**How it works:**

- Используйте, когда несколько классов делят базовую реализацию, но отличаются в отдельных методах.
- `abstract getPass(): string` — без тела; конкретный подкласс обязан реализовать.
- В отличие от interface, abstract class может содержать поля, конструкторы и конкретные методы.
- Для чистых контрактов — `interface`; для общего кода — abstract class.

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

**Definition:** Поля и методы с `static` принадлежат конструктору класса, а не отдельным экземплярам.

**How it works:**

- Доступ: `MyClass.x` или `MyClass.printX()` — не через экземпляры.
- Static-члены могут иметь модификаторы `public`, `protected`, `private`.
- Static-члены наследуются подклассами (`Derived.getGreeting()`).
- Не называйте static-члены `name`, `length` или `call` — конфликт с прототипом `Function`.
- Параметры типа generic-класса нельзя использовать в static-членах.

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
