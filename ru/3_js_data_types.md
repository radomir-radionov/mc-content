# JS Core — Data Types

Примитивы, приведение типов и модель памяти (стек и куча). Привязки и структуры данных — в соседних темах Core.

[← К оглавлению JS](./3_js.md)

### Related docs

- [Переменные](./3_js_variables.md) · [Структуры данных](./3_js_data_structures.md) · [Язык JS](./3_js_core.md) · [Операторы](./3_js_operators.md) · [Циклы](./3_js_loops.md) · [Массивы JS](./3_js_array.md) · [Функции JS](./3_js_functions.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md) · [JS Interview](./3_js_interview.md)
- [Итераторы](./3_js_loops.md#iterators) · [Computer science](./7_special_cs.md)

### Main concepts

| Область      | Темы                                                          |
| ------------ | ------------------------------------------------------------- |
| **Types**    | Кратко о каждом типе, затем `undefined` … `symbol` и `object` |
| **Common**   | Память, проверки, приведение, шаблонные строки                |
| **Advanced** | `Object.is`, абстрактное приведение, обёртки, clone vs JSON   |

### Sections

> Блоки `<details>`: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**.

- [Types](#types)
- [Common](#common)
- [Advanced](#advanced)
- [Each type in short](#each-type-in-short) · [undefined](#undefined) · [null](#null) · [boolean](#boolean) · [Numbers](#numbers) · [BigInt](#bigint) · [Strings](#strings) · [Symbol](#symbol) · [object](#object)
- [Stack and heap](#memory-stack-heap) · [typeof](#typeof) · [Falsy values](#falsy-values) · [Object keys](#object-keys) · [Type conversions](#type-conversions) · [Template literals](#template-literals)
- [Object.is & SameValueZero](#object-is-and-samevaluezero) · [ToPrimitive & abstract conversions](#toprimitive-and-abstract-conversions) · [Primitive wrappers](#primitive-wrappers) · [JSON vs structuredClone](#json-vs-structuredclone)

- [Интервью](./3_js_interview.md)

Соседние темы Core: [Язык](./3_js_core.md) · [Переменные](./3_js_variables.md) · [Типы данных](./3_js_data_types.md) · [Структуры данных](./3_js_data_structures.md) · [Операторы](./3_js_operators.md) · [Циклы](./3_js_loops.md)

## Types

### Types

<a name="data-types"></a>
<a name="each-type-in-short"></a>

<details>
  <summary>Each type in short</summary>

### Each type in short

**Definition:** В ECMAScript **7 примитивных типов** и **один непримитивный** — **`object`**. Примитивы неизменяемы и копируются по значению; объекты изменяемы и копируются по ссылке. **Функции** — объекты; только `typeof` даёт им `'function'`. Этот блок — обзор и шпаргалка; развёрнутые эссе по типам ниже в секции.

**How it works:**

**Краткая шпаргалка (собеседование)**

| Type        | Example          | `typeof`                  | One line                                                        |
| ----------- | ---------------- | ------------------------- | --------------------------------------------------------------- |
| `undefined` | `let x;`         | `'undefined'`             | «Не задано»; единственное значение — `undefined`                |
| `null`      | `null`           | `'object'` ⚠️             | Намеренная «пустая» ссылка; проверка `=== null`                 |
| `boolean`   | `true` / `false` | `'boolean'`               | Только логические значения; приведение в `if` / `&&` / `\|\|`   |
| `number`    | `42`, `NaN`      | `'number'`                | IEEE-754 float64; см. [Numbers](#numbers)                       |
| `bigint`    | `123n`           | `'bigint'`                | Целые произвольной длины; не смешивать с `number` без конверсии |
| `string`    | `'hi'`           | `'string'`                | Неизменяемый UTF-16; копирование по значению                    |
| `symbol`    | `Symbol('id')`   | `'symbol'`                | Уникальный идентификатор; скрыт от `Object.keys` / JSON         |
| `object`    | `{}`, `[]`, `fn` | `'object'` / `'function'` | Изменяемая ссылка; `===` — идентичность, не глубокое равенство  |

**Primitives (7)** — значение хранится напрямую; присваивание копирует значение; `===` сравнивает значения:

| Primitive   | Example        | Deep topic (this file)  |
| ----------- | -------------- | ----------------------- |
| `undefined` | `let x;`       | [undefined](#undefined) |
| `null`      | `null`         | [null](#null)           |
| `boolean`   | `true`         | [boolean](#boolean)     |
| `number`    | `42`, `NaN`    | [Numbers](#numbers)     |
| `bigint`    | `123n`         | [BigInt](#bigint)       |
| `string`    | `'hi'`         | [Strings](#strings)     |
| `symbol`    | `Symbol('id')` | [Symbol](#symbol)       |

**Non-primitive — `object`** — в переменной **ссылка** на объект в куче; присваивание копирует указатель; `===` сравнивает идентичность ссылки. Подробнее → [object](#object):

| Kind (все `typeof` → `'object'`, кроме функций)         | Example                       |
| ------------------------------------------------------- | ----------------------------- |
| Plain object                                            | `{ a: 1 }`                    |
| Array                                                   | `[1, 2]`                      |
| `Date`, `RegExp`, `Map`, `Set`, …                       | `new Date()`                  |
| **Function** (callable object; `typeof` → `'function'`) | `() => {}`, `function f() {}` |

`null` — **примитив**; почему `typeof null` обманывает → [typeof](#typeof).

- **Примитивы** — присвоение/копирование по значению; `===` сравнивает значения.
- **Объекты** (массивы, функции, даты, …) — по ссылке; `===` сравнивает идентичность ссылки.
- **Боксирование:** методы на примитивах через временные обёртки (`'hi'.toUpperCase()`). Избегайте `new String()` / `new Number()` — ломают `===` ([Object keys](#object-keys)).

Проверки → [typeof](#typeof). Falsy → [Falsy values](#falsy-values). Модель памяти → [Memory: stack and heap](#memory-stack-heap).

**Pluses and minuses:**

| + (примитивы)                        | − (примитивы)                       |
| ------------------------------------ | ----------------------------------- |
| Предсказуемая семантика копирования  | Нет методов без бокса               |
| Потокобезопасность в однопоточном JS | `number` имеет ограничения IEEE-754 |

| + (объекты)                 | − (объекты)                         |
| --------------------------- | ----------------------------------- |
| Изменяемый, расширяемый     | Общие ссылки вызывают ошибки        |
| Богатые API (массивы, даты) | `typeof` слишком грубо для подтипов |

**Examples:**

```js
const a = { x: 1 };
const b = a;
b.x = 2;
console.log(a.x); // 2 (shared reference)

const s1 = "hi";
const s2 = s1;
// независимы — строки неизменяемые примитивы
```

**Sources:** [javascript.info/types](https://javascript.info/types) · [MDN typeof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof)

</details>

<a name="undefined"></a>

<details>
  <summary>undefined</summary>

### undefined

**Definition:** **`undefined`** — переменная объявлена, но не присвоена, отсутствует свойство объекта или функция без `return`. Примитив; `typeof` → `'undefined'`.

**How it works:**

- Одно значение: `undefined` (глобальный `undefined` в современных средах нельзя перезаписать).
- `let x;` → `x === undefined`. Нет ключа: `({}).foo === undefined`.
- `void expression` всегда даёт `undefined`.
- **Falsy**, но не равно `null` (`null === undefined` — false).
- `Number(undefined)` → `NaN`; `String(undefined)` → `'undefined'`.

**When to use:** Обычно **не** присваивайте `undefined` явно — для «пусто намеренно» используйте `null` или не задавайте свойство. Для проверки — `value === undefined` или `value == null` (nullish).

**Examples:**

```js
let a;
a === undefined; // true
void 0; // undefined
({}).missing; // undefined
Number(undefined); // NaN
```

**Sources:** [MDN undefined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) · [javascript.info/types](https://javascript.info/types)

</details>

<a name="null"></a>

<details>
  <summary>null</summary>

### null

**Definition:** **`null`** — примитив «нет объекта» / намеренно пустое значение. Не то же самое, что `undefined`.

**How it works:**

- Литерал `null`. `typeof null` → `'object'` (исторический баг) — только `value === null`.
- **Falsy**. `null == undefined` — true; `null === undefined` — false.
- `Number(null)` → `0`; `String(null)` → `'null'`.
- JSON: `null` сериализуется; свойства со значением `undefined` опускаются.

**When to use:** Явное «пусто» в API (например, `getElementById` без элемента). Для fallback — `??`. Не смешивайте `null` и `undefined` в публичных контрактах.

**Examples:**

```js
document.getElementById("missing"); // null (в браузере)
value == null; // null ИЛИ undefined
value === null; // только null
JSON.stringify({ a: undefined, b: null }); // '{"b":null}'
```

**Sources:** [MDN null](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null) · [MDN typeof — null](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof)

</details>

<a name="boolean"></a>

<details>
  <summary>boolean</summary>

### boolean

**Definition:** **`boolean`** — примитив с двумя значениями: `true` и `false`.

**How it works:**

- `typeof true` → `'boolean'`. Только `true` и `false` — булевы (не `0`/`1`, пока не приведены).
- `&&`, `||`, `!` могут вернуть **последний вычисленный операнд**, не только boolean.
- `Boolean(x)` / `!!x` / `if (x)` — по списку [falsy](#falsy-values) (восемь значений).
- `Number(true)` → `1`, `Number(false)` → `0`.

**When to use:** Флаги и условия. Явный `Boolean(x)`, если API должен вернуть boolean.

**Examples:**

```js
Boolean("0"); // true
Boolean(0); // false
!!"hello"; // true
true && "ok"; // 'ok'
```

**Sources:** [MDN Boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean) · [javascript.info/type-conversions](https://javascript.info/type-conversions)

</details>

<a name="numbers"></a>

<details>
  <summary>Numbers</summary>

### Numbers

**Definition:** Обычные числа JS представляют собой двойные числа IEEE-754 **float64** (безопасные целые числа ±`2^53−1`). `BigInt` охватывает большие целые числа. Пишите большие/маленькие литералы с помощью `_` сепараторы, `e` обозначение, или `0x` / `0b` / `0o` префиксы.

**How it works:**

- Литералы: `1_000_000_000`, `1e9`, `1e-6`, `0xff`, `0b11111111`, `0o377`.
- `num.toString(base)` - база `2`…`36` (шестнадцатеричные цвета, короткие идентификаторы). Вызов литерала с двумя точками: `255..toString(16)` or `(255).toString(16)`.
- Округление: `Math.floor` / `ceil` / `round` / `trunc`; точность посредством умножения-разделения или `toFixed(n)` (**возвращает строку**).
- Поплавок: `0.1 + 0.2 !== 0.3` — сравните с эпсилоном или круглыми деньгами (`toFixed` / целое число центов).
- Проверки: предпочитают `Number.isNaN` / `Number.isFinite` / `Number.isInteger` / `Number.isSafeInteger` над глобальным `isNaN` / `isFinite` (эти принуждения).
- Разбор: `parseInt(str, radix)` / `parseFloat(str)` остановиться на первом недопустимом символе; `Number(str)` / `+str` требовать, чтобы вся строка была числовой (после обрезки).
- Произвольные целые → [BigInt](#bigint).

**When to use:** Арифметика, идентификаторы, измерения. Деньги → округляйте осторожно или используйте целые младшие единицы / `BigInt` / десятичная библиотека. Никогда не используйте `new Number()`.

#### Common pitfalls

```js
0.1 + 0.2; // 0.30000000000000004
(12.34).toFixed(1); // "12.3" — string, not number
parseInt("100px", 10); // 100
Number("100px"); // NaN
isNaN("hello"); // true — coerces
Number.isNaN("hello"); // false
(123456).toString(36); // "2n9c"
```

**Pluses and minuses:**

| +                                                      | −                                                             |
| ------------------------------------------------------ | ------------------------------------------------------------- |
| Один тип числа для большинства математических операций | Двоичная погрешность с плавающей запятой                      |
| Богатый `Math.*` + преобразование системы счисления    | `toFixed` возвращает строку                                   |
| Безопасные целочисленные помощники                     | Целые числа за пределами `MAX_SAFE_INTEGER` потерять точность |

**Examples:**

```js
Math.round(1.23456 * 100) / 100; // 1.23
+(1.005).toFixed(2); // often 1 — float trap; prefer cents

Number.isFinite(Infinity); // false
Number.isSafeInteger(2 ** 53); // false
Number.MAX_SAFE_INTEGER; // 9007199254740991
```

**Thoughts:** _Считать равенство с плавающей запятой приблизительным; обращаться `parseInt` vs `Number` как «префикс против целой строки»._

**Sources:** [javascript.info/number](https://javascript.info/number) · [MDN Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) · [MDN Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)

</details>

<a name="bigint"></a>

<details>
  <summary>BigInt</summary>

### BigInt

**Definition:** `bigint` хранит целые произвольной длины (`123n` или `BigInt("123")`). Отдельный примитив от `number` (float IEEE-754).

**How it works:**

- Суффикс `n` или `BigInt(...)`. Операции остаются в BigInt (`5n / 2n === 2n` — целочисленное деление).
- **Нельзя смешивать** с `number` (`1n + 2` бросает) — явно `Number()` / `BigInt()`.
- `==` может приводить (`1 == 1n`); `===` — нет. Унарный `+` для bigint недопустим.
- `typeof 1n` → `'bigint'`. `0n` — falsy.

**When to use:** ID и математика за `Number.MAX_SAFE_INTEGER`; не для денег с дробной частью (минорные единицы или decimal-библиотека).

**Examples:**

```js
1n + 2n; // 3n
// 1n + 2; // TypeError
Number(1n) + 2; // 3
1 == 1n; // true
1 === 1n; // false
```

**Sources:** [javascript.info/bigint](https://javascript.info/bigint) · [MDN BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)

</details>

<a name="strings"></a>

<details>
  <summary>Strings</summary>

### Strings

**Definition:** `string` — неизменяемый примитив UTF-16. Методы возвращают новые строки; присваивание копирует значение.

**How it works:**

- Методы: `slice`, `includes`, `startsWith`, `trim`, `split`, `replace`, `replaceAll`.
- Строки **итерируемы** (`for...of` / `Array.from`); `length` и индексы — в единицах UTF-16.
- Суррогатные пары: `'😂'.length === 2`; `codePointAt` / `fromCodePoint` / `for...of`.
- `str.normalize()` (NFC) перед сравнением при разном порядке диакритик.
- Экранирование: `\n`, `\t`, `\u{1F600}`.

**When to use:** UI-текст, URL, `split`/`join`, regex; `trim()` на вводе; `includes` до RegExp.

**Pluses and minuses:**

| +                                   | −                              |
| ----------------------------------- | ------------------------------ |
| Неизменяемость — безопасно делиться | Конкат в циклах — лучше `join` |
| Богатый API                         | UTF-16 и `.length` для эмодзи  |

**Examples:**

```js
"  hi  ".trim(); // 'hi'
"a-b-c".split("-"); // ['a','b','c']
["a", "b"].join("-"); // 'a-b'
"hello".includes("ell"); // true
"hello".replaceAll("l", "L"); // 'heLLo'
[..."𝒳😂"];
"S\u0307\u0323".normalize() === "S\u0323\u0307".normalize(); // true
"𝒳".codePointAt(0).toString(16); // '1d4b3'
```

Интерполяция и многострочность → [Template literals](#template-literals).

**Sources:** [javascript.info/string](https://javascript.info/string) · [javascript.info/unicode](https://javascript.info/unicode) · [javascript.info/primitives-methods](https://javascript.info/primitives-methods)

</details>

<a name="symbol"></a>

<details>
  <summary>Symbol</summary>

### Symbol

**Definition:** `symbol` является примитивом для **уникальных идентификаторов** — каждый `Symbol()` отличается даже при одном и том же описании.

**How it works:**

- Создано с `Symbol('description')` or `Symbol.for('globalKey')` (глобальный реестр).
- `Symbol.keyFor(sym)` возвращает ключ реестра только для **глобальных** символов (`undefined` для местных); использовать `sym.description` для метки любого символа.
- Используется как ключи объекта, которые не конфликтуют со строковыми ключами или другими библиотеками.
- Символьные клавиши **опущены** `Object.keys`, `JSON.stringify`, и `for...in`.
- Использовать `Object.getOwnPropertySymbols(obj)` or `Reflect.ownKeys` перечислить их.
- `Object.assign` **копирует** свойства символа (в отличие от `for...in` / `Object.keys`).
- Известные символы: `Symbol.iterator`, `Symbol.toPrimitive`, `Symbol.toStringTag`, `Symbol.hasInstance`.

**When to use:** Метаданные частных объектов, точки расширения библиотеки, определение пользовательских итераций. Предпочитать `class` частные поля `#x` для истинной конфиденциальности класса в современном коде.

**Pluses and minuses:**

| +                                | −                                       |
| -------------------------------- | --------------------------------------- |
| Гарантированные уникальные ключи | Не подлежит сериализации в формате JSON |
| Скрывает внутренние свойства     | Сложнее отлаживать/проверять            |
| Протокол итерации Powers         | Легко злоупотреблять                    |

**Examples:**

```js
const id = Symbol("id");
const user = { name: "Ann", [id]: 42 };

Object.keys(user); // ['name']
user[id]; // 42
Symbol.for("app") === Symbol.for("app"); // true — global registry
Symbol.keyFor(Symbol.for("app")); // "app"
Symbol.keyFor(Symbol("app")); // undefined — not global

Object.assign({}, user)[id]; // 42 — symbols are copied
```

**Sources:** [javascript.info/symbol](https://javascript.info/symbol)

</details>

<a name="object"></a>

<details>
  <summary>object</summary>

### object

**Definition:** **`object`** — непримитивный тип: набор свойств. Массивы, функции, даты и `{}` — объекты. Присваивание копирует **ссылку**; `===` сравнивает идентичность.

**How it works:**

- Plain object: `{ key: value }`. Ключи (кроме `Symbol`) приводятся к строке — [Object keys](#object-keys).
- **Массивы** — объекты с индексами и `length`; `Array.isArray`.
- **Функции** — вызываемые объекты; `typeof fn` → `'function'`.
- `null` — **примитив**, не объект, несмотря на `typeof null`.
- Изменения видны через все ссылки на тот же объект.
- Глубокого сравнения в языке нет — для тестов библиотека или рекурсия / ограниченно `JSON.stringify`.

**When to use:** Записи, кэши, экземпляры. `const` фиксирует ссылку, свойства можно менять. Паттерны структур → [Структуры данных](./3_js_data_structures.md).

**Pluses and minuses:**

| +                           | −                            |
| --------------------------- | ---------------------------- |
| Изменяемость, расширяемость | Общие ссылки — тонкие баги   |
| Богатые встроенные типы     | `typeof` грубый для подтипов |

**Examples:**

```js
const a = { n: 1 };
const b = a;
b.n = 2;
a.n; // 2
Array.isArray([]); // true
typeof (() => {}); // 'function'
```

**Sources:** [javascript.info/object](https://javascript.info/object) · [MDN Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)

</details>

## Common

### Common

<details>
  <summary>Stack and heap</summary>

<a name="memory-stack-heap"></a>

### Memory: stack and heap

**Definition:** Процесс получает изолированное **виртуальное** адресное пространство. Быстрая **стековая** память хранит кадры вызовов (LIFO); **куча** содержит значения, размер которых неизвестен или должен пережить функцию. JS-движки GC — куча; вы не `malloc`/`free`. Знание этой модели упрощает DevTools (трассировки стека, снимки кучи) и вопросы на собеседованиях о назначениях/вызовах.

**How it works:**

- **Ментальная модель ЦП:** фон Неймана — инструкции и данные совместно используют одно хранилище (одну шину). Гарвард их разделяет. Реальные чипы представляют собой гибрид (например, отдельный I-кэш L1/D-кэш). Память пронумерована **ячейками**; число представляет собой **адрес** фиксированной ширины (произвольный доступ, а не только последовательный). Значения, превышающие одну ячейку, охватывают несколько. 64-битная ширина → 2⁶⁴ адреса (≈ 16 ЭБ _адресного пространства_, а не «у вас 18 ЭБ ОЗУ»).- **Виртуальный или физический:** ЦП сопоставляет виртуальные адреса с оперативной памятью. ОС передает процессу **страницы** (часто 4–8 КБ), многие из них.
- **Процесс или поток:** ОС изолирует процессы (нет доступа к другому процессу, если он этого не разрешает). Процесс может иметь несколько потоков, каждый из которых имеет свой стек. Потоки POSIX — это один настоящий API — хосты различаются (вкладка браузера и узел).Планировщик, переключение контекста, сигналы: [Процессы, многозадачность и прерывания](./0_system_design_core.md#processes-multitasking). Уровень узла: [Процесс и потоки](./8_nodejs_process.md#process--threads).
- **Образ процесса:** в это пространство запустите программу **код**, **глобальные значения**, затем **стек** и **кучу**.
- **Стек:** увеличение/уменьшение путем перемещения **указателя стека** (регистра процессора) — дешево. Вы можете **читать** данные стека;вы **пишите** только вверху (нажимаете). Вы не можете освободить дыру посередине; перемотайте указатель назад, чтобы удалить все, что находится над ним. Истощение → **переполнение стека**. Схема обучения: куча растет «вверх», стопка «вниз», они встречаются в свободном зазоре.
- **Кадр стека:** аргументы одного вызова, локальные значения и «что запускать после возврата». **Указатель кадра** отмечает активный кадр. Вложенные вызовы: `logDogs` рамка → `countDogs(20)` → поп → `countDogs(9)` → поп → поп `logDogs`.
- **Расслабься:** `throw` выталкивает кадры до тех пор, пока `catch` (или программа умирает). **Трассировка стека** — это кадры в обратном порядке вызова.
- **Куча:** требуется, когда размер неизвестен во время построения кадра (массивы, которые `.push`, объекты, замыкания) или данные должны пережить функцию. В слоте стека хранится **ссылка фиксированного размера** (адрес); полезная нагрузка находится в куче. Измените массив по этому адресу. Позже сборщик мусора восстанавливает недоступные объекты кучи — [Сборка мусора](./3_js_objects.md#garbage-collection). MDN: JS выделяет место при создании и освобождает неиспользуемые объекты; в отличие от C нет `malloc`/`free`.
- **Предупреждение JS:** динамические типы означают, что движок _угадывает_, сколько стека нужно зарезервировать; догадки могут быть ошибочными (деопт). «Примитивы в стеке, объекты в куче» — это обучающая модель, а не гарантированный макет V8 (маленькие целые числа могут быть помеченными указателями; большие строки — это куча).

**When to use:**

Почему `Maximum call stack size exceeded`; почему объекты/массивы доступны по ссылке; почему местные жители исчезают после возвращения; чтение трассировки стека.

| | Стек | Куча |
| | --- | --- |
| | **\*+** | Tiny alloc/free (переместить указатель); фреймы, удобные для кэширования | Произвольный размер; данные могут пережить звонящего |
| | **-** | Фиксированный размер; только ЛИФО; переполнение | Медленное выделение; ГК; утечки через оставшиеся ссылки |

#### Common pitfalls

Рассматривать диаграмму как структуру физической оперативной памяти; предполагая, что JS всегда знает размеры слотов стека; размещение огромных или растущих данных «в стеке» у себя в голове — это объект-куча плюс ссылка; запутанное переполнение стека вызовов с кучей OOM (`JavaScript heap out of memory` в узле).

**Examples:**

```js
function countDogs(happyDogs) {
  const sadCoefficient = 0.1;
  if (happyDogs < 10) {
    throw new Error("too few happy dogs");
  }
  return happyDogs + sadCoefficient * happyDogs;
}

function logDogs() {
  console.log(countDogs(20), countDogs(9));
}

logDogs();
// frames: logDogs → countDogs(20) → pop → countDogs(9) → throw → unwind
```

```js
function createDogArray() {
  const dogs = ["🐶", "🐶", "🐶"]; // payload on heap; `dogs` on the frame is a ref
  if (Math.random() > 0.5) dogs.push("🐶"); // size unknown at compile time
}
```

**Thoughts:** _Рамки дешевые LIFO; все, что имеет неизвестный или постоянный размер, является объектом-кучей за указателем — GC, а не `free()`._

**Sources:** [Doka — как работает память](https://doka.guide/js/how-the-memory-works/) · [Управление памятью MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Memory_management) · [javascript.info/garbage-collection](https://javascript.info/garbage-collection)

</details>

<a name="typeof"></a>

<details>
  <summary>typeof</summary>

### typeof

**Definition:** `typeof` возвращает строковую метку типа значения. Для примитивов удобен; для надёжных проверок сочетайте с `Array.isArray`, `Number.isNaN` и строгим `===`. Примитив vs object → [Each type in short](#each-type-in-short).

**How it works:**

| Type        | `typeof`      | Notes                                         |
| ----------- | ------------- | --------------------------------------------- |
| `undefined` | `'undefined'` | Unassigned variable                           |
| `null`      | `'object'` ⚠️ | Intentional empty value — legacy `typeof` bug |
| `boolean`   | `'boolean'`   | `true` / `false`                              |
| `number`    | `'number'`    | IEEE-754 double; `NaN`, `±Infinity`           |
| `bigint`    | `'bigint'`    | Arbitrary integers (`123n`)                   |
| `string`    | `'string'`    | Immutable UTF-16 sequence                     |
| `symbol`    | `'symbol'`    | Unique identifier                             |
| `object`    | `'object'`    | Arrays, dates, plain objects, etc.            |
| `function`  | `'function'`  | Callable object — only `typeof` distinguishes |

| Situation             | Use                                                       |
| --------------------- | --------------------------------------------------------- |
| Is it `null`?         | `value === null` (not `typeof`)                           |
| Is it an array?       | `Array.isArray(value)`                                    |
| Is it `NaN`?          | `Number.isNaN(value)`                                     |
| Is it a plain object? | `Object.prototype.toString.call(v) === '[object Object]'` |

- `typeof null` → `'object'` (legacy) — используйте `value === null`.
- `typeof []` → `'object'` — `Array.isArray(value)`.
- `typeof (() => {})` → `'function'`.
- `typeof` на несуществующей привязке → `ReferenceError`; на объявленной без значения `let` → `'undefined'`.
- `NaN` — `typeof 'number'`; `NaN === NaN` ложно — `Number.isNaN` или `Object.is(NaN, NaN)`.
- `+0 === -0` истинно; `Object.is(+0, -0)` ложно.

**When to use:** Быстрые проверки примитивов; не полагайтесь только на `typeof` для `null`, массивов и `NaN`.

**Examples:**

```js
value === null; // null only (not undefined)
value == null; // null OR undefined
Array.isArray(value); // arrays (typeof is 'object')
Number.isNaN(value); // NaN only (typeof NaN is 'number')
Number.isFinite(value); // finite number (excludes NaN, ±Infinity)
Object.prototype.toString.call(v); // '[object Array]', '[object Date]', …

typeof null; // 'object' — use value === null
typeof []; // 'object' — use Array.isArray([])
typeof (() => {}); // 'function'
let x;
typeof x; // 'undefined'

typeof NaN; // 'number'
NaN === NaN; // false
Object.is(NaN, NaN); // true
Number.isNaN(NaN); // true
isNaN("hello"); // true — coerces first (footgun)
Number.isNaN("hello"); // false
+0 === -0; // true
Object.is(+0, -0); // false
```

**Sources:** [javascript.info/types](https://javascript.info/types) · [MDN typeof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof)

</details>

<a name="falsy-values"></a>

<details>
  <summary>Falsy values</summary>

### Falsy values

**Definition:** Восемь значений дают `false` в логическом контексте (`if`, `&&`, `||`, `Boolean()`). Остальное **truthy**, в том числе `[]`, `{}`, `'0'`.

**How it works:**

Falsy: `false`, `0`, `-0`, `0n`, `''`, `null`, `undefined`, `NaN`.

Неявное приведение в `if (value)` и `value || fallback`; для только null/undefined — `??`.

**When to use:** Ранние выходы и значения по умолчанию; явный `Boolean(x)`, когда API должен вернуть boolean.

**Examples:**

```js
Boolean([]); // true
Boolean("0"); // true
Boolean(0); // false
Boolean(""); // false
```

Сюрпризы `+` / `==` → [Type conversions](#type-conversions).

**Sources:** [javascript.info/type-conversions](https://javascript.info/type-conversions)

</details>

<a name="object-keys"></a>

<details>
  <summary>Object keys</summary>

### Object keys

**Definition:** Ключи обычного объекта (кроме `Symbol`) приводятся к строке через `ToString`. Обёртки `new String()` / `new Number()` — объекты и ломают `===` с примитивами.

**How it works:**

- `{[obj]: 1}` вызывает `obj.toString()` — часто `'[object Object]'`.
- Ключи `Symbol` не попадают в `Object.keys` / `JSON.stringify` — см. [Symbol](#symbol).
- `Map` принимает ключи любого типа; `{}` — для JSON DTO и литералов.

| Use `Map`           | Use plain `Object`          |
| ------------------- | --------------------------- |
| Keys of any type    | JSON serialization needed   |
| Frequent add/delete | Literal config / DTO shapes |
| `.size` needed      | Prototype chain / methods   |

**When to use:** `Map` для нестроковых ключей или частого add/delete; `{}` для сериализуемых записей.

**Examples:**

```js
const obj = {};
const key = { id: 1 };
obj[key] = "value";
Object.keys(obj); // ['[object Object]']

const s = new String("hi");
typeof s; // 'object'
s === "hi"; // false
```

**Sources:** [javascript.info/map-set](https://javascript.info/map-set) · [javascript.info/primitives-methods](https://javascript.info/primitives-methods)

</details>

<a name="type-conversions"></a>

<details>
  <summary>Type conversions</summary>

### Type conversions

**Definition:** Значения преобразуются между типами **явно** (`String()`, `Number()`, `Boolean()`) или **неявно** во время операций (`+`, `==`логические контексты). Предпочитать `===` и явные преобразования в коде приложения.

**How it works:**

- **В строку:** `String(x)`, литералы шаблона, `+` со строковым операндом.
- **На номер:** `Number(x)`, унарный `+x`, арифметические операторы (кроме `+` со строкой).
- **Для логического значения:** `Boolean(x)`, `!!x`, или любой логический/`if` контекст — использует ложный список.
- **`==`** принуждает обе стороны перед сравнением; **`===`** нет.
- `null` и `undefined` равны друг другу только с `==`.

| Использование                                             | Избегайте                            |
| --------------------------------------------------------- | ------------------------------------ |
| `Number(value)` для пользовательского ввода               | `parseInt` без системы счисления     |
| `value == null` для нулевой/неопределённой проверки       | `==` за всеобщее равенство           |
| `Boolean(x)` or `!!x` когда вам нужно логическое значение | Опираясь на неявное приведение в API |
| `String(x)` перед конкатенацией                           | `x + ''` в критических путях кода    |

| Преобразование       | Результат/правило                           |
| -------------------- | ------------------------------------------- |
| `Number(undefined)`  | `NaN`                                       |
| `Number(null)`       | `0`                                         |
| `Number('')`         | `0`                                         |
| `Number(' 42 ')`     | `42` (обрезает пробелы)                     |
| `Number(true)`       | `1`                                         |
| `Boolean(0)`         | `false` — см. [Falsy values](#falsy-values) |
| `String(123)`        | `'123'`                                     |
| `parseInt('08', 10)` | `8` — всегда передавать систему счисления   |

**When to use:**

#### Common pitfalls

```js
[] + []; // ''
[] + {}; // '[object Object]'
"5" + 1; // '51' — string concat wins
"5" - 1; // 4 — numeric coercion
isNaN("hello"); // true — coerces first (footgun)
Number.isNaN("hello"); // false — no coercion
```

**Pluses and minuses:**

| Подход                   | +                               | −                                      |
| ------------------------ | ------------------------------- | -------------------------------------- |
| `===` + явный `Number()` | Предсказуемый                   | Более подробный                        |
| `==`                     | Более короткие проверки на ноль | Удивительное принуждение               |
| `??`                     | Только нулевой запасной вариант | Необходимо заключить в скобки с `\|\|` |

**Examples:**

```js
String(true); // 'true'
Number("123"); // 123
Number(""); // 0
Boolean([]); // true — only 8 values are falsy
Boolean("0"); // true
null == undefined; // true
null === undefined; // false
"" == 0; // true
"" === 0; // false
```

**Sources:** [javascript.info/type-conversions](https://javascript.info/type-conversions) · [javascript.info/comparison](https://javascript.info/comparison)

</details>

<a name="template-literals"></a>

<details>
  <summary>Template literals</summary>

### Template literals

**Definition:** **Template literals** — обратные кавычки, интерполяция `${expr}` и многострочный текст без `\n`.

**How it works:**

- `` `Hello ${name}` `` — вложенные шаблоны допустимы.
- Tagged templates (`fn\`...\``) — продвинутый приём (MDN).
- Нет автоэкранирования HTML — санитизируйте пользовательский ввод.

**When to use:** Динамические сообщения, фрагменты SQL (с параметризацией), многострочные сниппеты.

**Examples:**

```js
const name = "Ann";
const greeting = `Hello, ${name}!`;
const multiline = `line 1
line 2`;
```

**Sources:** [javascript.info/string](https://javascript.info/string) · [MDN Template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

</details>

## Advanced

### Advanced

<a name="object-is-and-samevaluezero"></a>

<details>
  <summary>Object.is & SameValueZero</summary>

### Object.is & SameValueZero

**Definition:** В JS несколько отношений **равенства**. В коде чаще `===`; движок также использует **SameValue** (`Object.is`) и **SameValueZero** (ключи `Map`/`Set`, `Array.includes`).

**How it works:**

| Relation      | `NaN`    | `+0` / `-0`  | Coercion |
| ------------- | -------- | ------------ | -------- |
| `==`          | не равны | равны        | да       |
| `===`         | не равны | равны        | нет      |
| `Object.is`   | равны    | **не** равны | нет      |
| SameValueZero | равны    | равны        | нет      |

- `Object.is(a, b)` — SameValue: как `===`, но `Object.is(NaN, NaN)` true и `Object.is(+0, -0)` false.
- **SameValueZero** — как SameValue, но `+0` и `-0` считаются равными; внутри `Map`/`Set` и `includes`.
- Для одного `NaN` без приведения — `Number.isNaN(x)`. Подробнее → [Object.is & equality](./3_js_operators.md#object-is-and-equality).

**When to use:** `Object.is`, когда важны знаковый ноль или sameness для `NaN`; не используйте `==` для общего сравнения.

**Examples:**

```js
Object.is(NaN, NaN); // true
NaN === NaN; // false
Object.is(+0, -0); // false
+0 === -0; // true

const m = new Map([[NaN, "ok"]]);
m.get(NaN); // 'ok'
[NaN].includes(NaN); // true
```

**Sources:** [MDN Object.is](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) · [MDN Equality comparisons and sameness](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Equality_comparisons_and_sameness)

</details>

<a name="toprimitive-and-abstract-conversions"></a>

<details>
  <summary>ToPrimitive & abstract conversions</summary>

### ToPrimitive & abstract conversions

**Definition:** Неявное приведение в `+`, `==` и `if` — это **абстрактные операции** `ToPrimitive`, `ToNumber`, `ToString`. Объекты участвуют через `Symbol.toPrimitive` или `valueOf` / `toString`.

**How it works:**

- **`ToPrimitive(input, hint)`** — для объекта: `[Symbol.toPrimitive](hint)` или `valueOf`/`toString` в порядке по `hint` (`'number'`, `'string'`, `'default'`).
- **`ToNumber`** / **`ToString`** — после получения примитива.
- **`ToBoolean`** — [Falsy values](#falsy-values); только восемь falsy.
- `hint === 'default'` часто в `==` и `+` — отсюда сюрпризы вроде `{} + 1`.

| Hint      | Сначала    | Затем      |
| --------- | ---------- | ---------- |
| `number`  | `valueOf`  | `toString` |
| `string`  | `toString` | `valueOf`  |
| `default` | `valueOf`  | `toString` |

Правила для кода → [Type conversions](#type-conversions). Хуки объектов → [Objects to primitives](./3_js_objects.md#objects-to-primitives).

**Examples:**

```js
const obj = {
  [Symbol.toPrimitive](hint) {
    return hint === "number" ? 42 : "hi";
  },
};
obj + 1; // 43
String(obj); // 'hi'
```

**Sources:** [javascript.info/type-conversions](https://javascript.info/type-conversions) · [MDN Symbol.toPrimitive](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/toPrimitive)

</details>

<a name="primitive-wrappers"></a>

<details>
  <summary>Primitive wrappers</summary>

### Primitive wrappers

**Definition:** Примитивы не объекты, но есть **обёртки** (`String`, `Number`, `Boolean`, …). Вызов метода на примитиве **боксирует** его временно; `new String()` создаёт объект, ломающий `===`.

**How it works:**

- `'hi'.toUpperCase()` — временный объект `String`, метод, сброс обёртки.
- `new String('hi')` — `typeof` `'object'`, `===` с примитивом false.
- В приложениях не используйте `new String()` / `new Number()` / `new Boolean()`.
- `Symbol()` и `123n` — без `new` в обычном стиле.

Связь с ключами → [Object keys](#object-keys). Обзор → [Each type in short](#each-type-in-short).

**Examples:**

```js
typeof "x"; // 'string'
typeof new String("x"); // 'object'
new String("x") === "x"; // false
```

**Sources:** [javascript.info/primitives-methods](https://javascript.info/primitives-methods) · [MDN String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)

</details>

<a name="json-vs-structuredclone"></a>

<details>
  <summary>JSON vs structuredClone</summary>

### JSON vs structuredClone

**Definition:** Копирование и передача данных пересекают границы **типов**. `JSON.stringify`/`parse` — только JSON-безопасные значения; `structuredClone` — алгоритм structured clone, сохраняет больше встроенных типов.

**How it works:**

| Подход            | Циклы | `undefined` / функции | `Date`, `Map`, `Set`       |
| ----------------- | ----- | --------------------- | -------------------------- |
| `JSON.stringify`  | throw | опускаются            | `Date` → строка; нет `Map` |
| `structuredClone` | OK    | в клоне               | клонируются где поддержано |

- JSON: ключи — строки; `NaN`/`Infinity` → `null` в JSON.
- `structuredClone` не клонирует DOM и некоторые host-объекты; функции не клонируются.
- Поверхностная копия POJO — spread / `Object.assign` → [Shallow vs deep copy](./3_js_objects.md#shallow-vs-deep-copy).

**When to use:** API и `localStorage` → JSON; снимок состояния с `Date`/`Map` в памяти → `structuredClone`.

**Examples:**

```js
JSON.stringify({ a: 1, b: undefined }); // '{"a":1}'
structuredClone({ m: new Map([[1, 2]]) }).m.get(1); // 2
```

**Sources:** [MDN structuredClone](https://developer.mozilla.org/en-US/docs/Web/API/structuredClone) · [MDN JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON)

</details>
