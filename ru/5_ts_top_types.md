# TypeScript — any, unknown, never

Верхние и нижние типы — когда что использовать.

[← Назад к хабу](./5_ts.md)

### Related docs

* [TypeScript — hub](./5_ts.md) · [Интервью](./5_ts_interview.md)

### Sections

- [Top types](#top-types)

---

## Top types

<details>
  <summary>Top types</summary>

### `any`

**Definition:**

Тип, который отключает статическую проверку значения — TypeScript обрабатывает его как обычный JavaScript.

| Тип | Безопасность |
|------|--------|
| `any` | Проверка отключена |
| `unknown` | Сначала необходимо сузить |
| `never` | Нет допустимых значений |

**Sources:** [Справочник по TypeScript — повседневные типы] (https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

### `any` vs `unknown` vs `never` — interview comparison

**Definition:** `any` отключает проверку. `unknown` является типобезопасным верхним типом — перед использованием его необходимо сузить. `never` это нижний тип — никаких значений, используемых для полноты и функций, которые не возвращают значения.

**How it works:**

Назначьте что-нибудь на `any` or `unknown`. Только `unknown` силы `typeof` / предикаты перед доступом к свойству. `never` присваивается *любому* типу, но не принимает никаких значений (кроме других `never`). `useUnknownInCatchVariables` типы `catch (e)` as `unknown`.


| `any` | `unknown` | `never` |
| --- | ------- | ----------- |
| Принимает любое значение | да | да | нет (необитаемый) |
| Использовать без сужения | да | **нет** | н/д |
| Назначается всему | да | только `unknown`/`any` | да |
| Типичное использование | аварийный люк (избегать) | внешние данные/JSON | `throw`, бесконечный цикл, полнота |

1. Динамические данные из API → `unknown` + сужение, или Zod/io-ts
2. Нетипизированные сторонние библиотеки → `@types/*` or `declare module`
3. По-настоящему динамичные формы → дженерики или индексные подписи.
4. Прототипирование → штраф в краткосрочной перспективе; затяните перед отправкой

**When to use:**

`JSON.parse` → `unknown`. Ошибка в `catch` → `unknown`. Дискриминированный профсоюз `default` → `const _x: never = x`.

#### Common pitfalls

`any` заразительно — производные значения становятся `any`. Скрытый `any` когда `noImplicitAny` выключен.

**Pluses and minuses:**

| + | − |
|------|------|
| `unknown` + честное сужение границ API | Больше шаблона, чем `any` |
| `never` приводит к сбою компиляции коммутатора | Легко «исправить» `as any` и потеряю чек |

**Examples:**

```typescript
// any — no checks
let a: any = 10;
a.foo(); // OK (crashes at runtime)

// unknown — must narrow first
function processValue(value: unknown): string {
  if (typeof value === "string") return value.toUpperCase();
  return "Unknown";
}

// never — does not return
function fail(msg: string): never {
  throw new Error(msg);
}
```

```typescript
let obj: any = { x: 0 };

obj.foo();           // no error — but might crash at runtime
obj = "hello";       // also fine, apparently
const n: number = obj; // sure, why not

// any in tuples and arrays — all type safety gone
let y: [any, any] = ["hello", 42];
let z: Array<any> = [10, "hello"];

let notSure: any = false;
notSure = true;      // boolean — no error
notSure = 42;        // number — no error
notSure = "hello";   // string — no error
```

```typescript
// Without a type annotation and no inference context, TS defaults to any:
function log(value) {  // implicit any if noImplicitAny is off
  console.log(value);
}
```

**Thoughts:**

Предпочитать `unknown` на границе; `any` это аварийный люк, а не тип.

`any` является заразным. Как только значение `any`, все, что происходит из него, имеет тенденцию становиться `any` слишком. Это запасной выход, а не образ жизни. Давать возможность `noImplicitAny` in `tsconfig` чтобы уловить места, куда TypeScript молча возвращается `any`.

**Sources:** [Справочник по TypeScript — повседневные типы] (https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)

</details>


