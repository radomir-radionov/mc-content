# JS Core — Array

Упорядоченные коллекции, методы массивов и шаблоны интервью `map` / `filter` / `reduce`.

[ ← Назад в JS-хаб](./3_js.md)

### Related docs

- [Язык JS](./3_js_core.md) · [Переменные](./3_js_variables.md) · [Типы данных](./3_js_data_types.md) · [Структуры данных](./3_js_data_structures.md) · [Операторы](./3_js_operators.md) · [Циклы](./3_js_loops.md) · [Функции JS](./3_js_functions.md) · [JS OOP](./3_js_operators.md) · [JS Async](./3_js_async.md) · [JS-документ](./3_js_browser.md)
- [JS-интервью](./3_js_interview.md) · [Задание корзины TypeScript](./5_ts_interview.md#interview-practice)

### Main concepts

| Площадь        | Темы                                                            |
| -------------- | --------------------------------------------------------------- |
| **Форма**      | Индексы, `length`, редкие отверстия, `Array.isArray`            |
| **Копировать** | Распространение, `slice`, `Array.from`, мелкий против глубокого |
| **API**        | Мутирующие и немутирующие, поиск, сортировка, выравнивание      |
| **ХОФ**        | `map`, `filter`, `reduce`, `forEach`                            |

### Sections

> Теория `<details>` блоки следуют: **Определение** · **Как это работает** · **Когда использовать** · **Плюсы и минусы** · **Примеры** · **Мысли** · **Источники**. **Определение** обязательно; другие разделы являются необязательными.

- [Array basics](#array-basics)
- [Copy & creation](#copy-and-creation)
- [Mutating methods](#mutating-methods)
- [map, filter, reduce & iteration](#map-filter-reduce-and-iteration)
- [Search, sort & flat](#search-sort-and-flat)
- [Интервью](./3_js_interview.md)

<a name="array-basics"></a>

## Array basics

<details>
  <summary>Array basics</summary>

### Array basics

**Definition:** `Array` — объект для упорядоченного списка значений по неотрицательным целочисленным индексам плюс `length`, на единицу больше максимального индекса. Это не ассоциативная карта — строковые ключи обычные свойства, не элементы.

**How it works:**

- С нуля: последний индекс `length - 1`. `arr[2]` и `arr["2"]` — один элемент; `arr["02"]` — другое свойство.
- `length` растёт при записи за конец (`arr[5] = x`) и **удаляет** элементы при уменьшении.
- Дырки (`arr[1] = empty`) — не то же, что `undefined`. Старые методы (`forEach`, `map`, `filter`) пропускают дырки; новые (`keys`, `find`, `includes`) трактуют дырки как `undefined`.
- `typeof [] === "object"` — используйте `Array.isArray(value)` (или `Object.prototype.toString.call(value) === "[object Array]"`).

**When to use:** Последовательности, где важны порядок и доступ по индексу. `Set` — только уникальность, `Map` — нестроковые ключи.

**Common pitfalls:**

```js
Array.isArray([]); // true
Array.isArray({ length: 2 }); // false — array-like is not an Array

const sparse = ["a"];
sparse[2] = "c";
sparse.length; // 3
sparse.map((x) => x); // ['a', empty, 'c'] — hole preserved

fruits.length = 10; // extends with holes, not undefined elements
fruits.length = 2; // deletes indexes >= 2
```

**Pluses and minuses:**

| +                      | −                                            |
| ---------------------- | -------------------------------------------- |
| Литералы и rest/spread | Редкие массивы сбивают с толку               |
| Итерируемость, JSON    | Не кортеж — смешанные типы и дырки допустимы |
| Богатый встроенный API | Строковые ключи — свойства, не элементы      |

**Thoughts:** На интервью ждут `Array.isArray`, дырка vs `undefined` и живой `length`, а не «массив — особый примитив».

**Sources:** [MDN Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) · [javascript.info/array](https://javascript.info/array)

</details>

<a name="copy-and-creation"></a>

## Copy & creation

<details>
  <summary>Copy & creation</summary>

### Copy & creation

**Definition:** Большинство копий массива **поверхностные**: новый массив с теми же ссылками на элементы. Вложенные объекты spread, `slice`, `concat` и `map` не клонируют.

**How it works:**

- `[...arr]`, `arr.slice()` и `arr.concat()` — новый массив с теми же ссылками.
- `Array.from(iterable, mapFn?)` строит настоящий `Array` из array-like (`{ length, 0: … }`) или любого итерируемого (`Set`, ключи `Map`, строки).
- `Array.of(a, b, c)` как `[a, b, c]`, но корректно с одним числом (`Array.of(7)` → `[7]`, не семь дырок).
- Глубокое клонирование — `structuredClone`, библиотека или обход вручную, не один spread.

**When to use:** Копируйте перед мутацией в React state или общих данных. `Array.from` — для итерируемого или DOM `NodeList`; если уже массив — достаточно spread.

**Examples:**

```js
const arr = ["banana", "apple", "peach"];
const copy = arr.slice(); // shallow copy
const next = [...arr, "mango"]; // new array

Array.from({ length: 3 }, (_, i) => i); // [0, 1, 2]
Array.from("hi"); // ['h', 'i']
```

**Pluses and minuses:**

| +                               | −                                                             |
| ------------------------------- | ------------------------------------------------------------- |
| Spread/slice быстры и читаемы   | Легко принять shallow за deep clone                           |
| `Array.from` мапит при создании | `Array(n)` — дырки; `Array.from({ length: n })` — `undefined` |

**Sources:** [MDN Array.from](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from) · [MDN spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

</details>

<a name="mutating-methods"></a>

## Mutating methods

<details>
  <summary>Mutating methods</summary>

### Mutating methods

**Definition:** Часть методов меняет массив на месте (`push`, `splice`, `sort`, …). В ES2023 появились немутирующие пары (`toSpliced`, `toSorted`, `toReversed`) — новый массив, оригинал без изменений.

**How it works:**

| Mutating                             | Non-mutating alternative               |
| ------------------------------------ | -------------------------------------- |
| `push` / `pop` / `shift` / `unshift` | `concat`, `slice`, spread              |
| `splice`                             | `toSpliced`                            |
| `sort`, `reverse`                    | `toSorted`, `toReversed`               |
| `fill`, `copyWithin`                 | build a new array with `map` / `slice` |

- `push` / `pop` — конец (стек). `shift` / `unshift` — начало (очередь); `shift` — O(n), индексы сдвигаются.
- `splice(start, deleteCount, …items)` удаляет/вставляет на месте; возвращает удалённое.
- `delete arr[i]` оставляет **дырку** и не меняет `length`; для укорочения списка — `splice`.

**When to use:** Мутируйте локальные буферы; для props, Redux и общих данных — немутирующие методы. `splice` — когда нужна вставка/удаление на месте и мутация допустима.

**Examples:**

```js
const arr = ["banana", "apple", "peach"];
arr.push("kiwi"); // mutates; length 4

const removed = arr.splice(1, 1); // removes 'apple'
const safe = arr.toSpliced(1, 0, "pear"); // new array, arr unchanged
```

**Thoughts:** _Стек — `push`/`pop`. Очередь — `push`/`shift`, но помните о стоимости `shift` на больших массивах._

**Sources:** [MDN Array#mutator_methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#mutator_methods) · [MDN toSpliced](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSpliced)

</details>

<a name="map-filter-reduce-and-iteration"></a>

## map, filter, reduce & iteration

<details>
  <summary>map, filter, reduce & iteration</summary>

### map, filter, reduce & iteration

**Definition:** Итерационные методы принимают `(element, index, array)` и либо строят новый массив, либо сворачивают в одно значение. `map` / `filter` / `flatMap` копируют; `forEach` возвращает `undefined`; `reduce` — накопленный результат.

**How it works:**

- `map(fn)` — новый массив **той же длины**; пропущенные дырки остаются. Не используйте `map` только для побочных эффектов.
- `filter(fn)` — новый массив truthy-результатов; дырки пропускаются.
- `reduce(fn, init)` — свёртка. Всегда передавайте `init`, если массив может быть пустым (`TypeError`).
- `forEach(fn)` — обход назначенных индексов; `break` нельзя (используйте `for...of` / `some`).
- `flatMap(fn)` — `map`, затем `flat(1)`; удобно, когда элемент разворачивается в ноль или больше значений.
- `["1","2","3"].map(parseInt)` → `[1, NaN, NaN]` — `map` передаёт `(element, index)`, `parseInt` воспринимает индекс как radix.

| Method    | Returns                            | Mutates?           |
| --------- | ---------------------------------- | ------------------ |
| `map`     | новый массив той же длины          | нет                |
| `filter`  | новый массив, может быть короче    | нет                |
| `flatMap` | новый массив, flatten на 1 уровень | нет                |
| `reduce`  | аккумулятор (любой тип)            | нет                |
| `forEach` | `undefined`                        | нет (колбэк может) |

**When to use:** `map` — преобразование; `filter` — отбор; `reduce` — агрегация/группировка; `forEach` / `for...of` — эффекты. Цепочка `filter` затем `map`, а не оба в одном `reduce`, если на выходе нужен массив.

**Pluses and minuses:**

| +                                        | −                                                              |
| ---------------------------------------- | -------------------------------------------------------------- |
| Декларативно, цепочки, стандарт интервью | `reduce` легко превратить в нечитаемую свёртку                 |
| Копирующие методы не трогают оригинал    | Редкий массив + `map` сохраняет дырки; ловушка `map(parseInt)` |

**Examples:**

```js
[1, 2, 3].map((n) => n * 2); // [2, 4, 6]
[1, 2, 3].filter((n) => n > 1); // [2, 3]
[1, 2, 3].reduce((sum, n) => sum + n, 0); // 6
[1, 2, 3].forEach((n) => console.log(n)); // undefined

["1", "2", "3"].map(Number); // [1, 2, 3]
["1", "2", "3"].map((s) => parseInt(s, 10)); // [1, 2, 3]
```

**Thoughts:** _`map` преобразует, `filter` отбирает, `reduce` агрегирует, `forEach` только работа._ Один объект или число из списка — `reduce`; список на выходе — `map`/`filter`.

**Sources:** [MDN Array.prototype.map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) · [Итеративные методы MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#iterative_methods) · [javascript.info/array-methods](https://javascript.info/array-methods)

</details>

<a name="search-sort-and-flat"></a>

## Search, sort & flat

<details>
  <summary>Search, sort & flat</summary>

### Search, sort & flat

**Definition:** Методы поиска находят элемент или индекс; `sort` упорядочивает на месте (или `toSorted` сначала копирует). `flat` / `flatMap` разворачивают вложенные массивы на заданную глубину.

**How it works:**

- Поиск: `find` / `findIndex` / `includes` / `indexOf` / `lastIndexOf`. `includes` — SameValueZero (`NaN` совпадает); `indexOf` — `===` (`NaN` не найдёт).
- `sort(compareFn?)` мутирует и возвращает **тот же** массив. Без compare — строки (UTF-16), `[10, 2]` → `[10, 2]`. Для чисел — `(a, b) => a - b`.
- `toSorted(compareFn?)` — новый отсортированный массив (ES2023).
- `flat(depth = 1)` копирует и разворачивает вложенность; `flat(Infinity)` — полностью.

| Method               | Returns                                 | Mutates?                        |
| -------------------- | --------------------------------------- | ------------------------------- |
| `find` / `findIndex` | элемент / индекс или `undefined` / `-1` | нет                             |
| `sort`               | тот же массив                           | **да** — используйте `toSorted` |
| `flat`               | новый плоский массив                    | нет                             |

**When to use:** `find` — первый подходящий объект; `includes` — примитивы и `NaN`. Для числовой или locale-сортировки всегда передавайте compare. `flatMap` вместо `map(...).flat()`, если шаг может дать несколько элементов.

**Examples:**

```js
[10, 2, 1].toSorted((a, b) => a - b); // [1, 2, 10]
[1, [2, [3]]].flat(2); // [1, 2, 3]
[NaN].includes(NaN); // true
[NaN].indexOf(NaN); // -1
```

**Pluses and minuses:**

| +                                          | −                                         |
| ------------------------------------------ | ----------------------------------------- |
| `toSorted` / `flat` без сюрпризной мутации | `sort` без compare — лексикографический   |
| `includes` находит `NaN`                   | `shift` + sort на огромных списках дорого |

**Sources:** [MDN Array.prototype.sort](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) · [MDN Array.prototype.flat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat) · [MDN Array.prototype.find](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find)

</details>
