# JS Core — Operators

Выражения, операторы, сравнения, optional chaining и `?.` / `??`. Поток управления и итераторы — в [Циклы](./3_js_loops.md).

[ ← Назад в JS-хаб](./3_js.md)

### Related docs

- [Язык JS](./3_js_core.md) · [Переменные](./3_js_variables.md) · [Циклы](./3_js_loops.md) · [Типы данных](./3_js_data_types.md) · [Структуры данных](./3_js_data_structures.md) · [Массивы JS](./3_js_array.md) · [Функции JS](./3_js_functions.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md) · [JS Документ](./3_js_browser.md) · [JS Events](./3_js_events.md) · [JS Async](./3_js_async.md) · [JS Interview](./3_js_interview.md)
- [Информатика](./7_special_cs.md)

### Main concepts

| Площадь       | Темы                                                                                       |
| ------------- | ------------------------------------------------------------------------------------------ |
| **Выражения** | Оператор против выражения, ASI, виды выражений                                             |
| **Операторы** | Арифметика, сравнение, `Object.is`, логический, побитовый, optional chaining, nullish `??` |

### Sections

> Теория `<details>` блоки следуют: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**. **Definition** обязательно; остальные разделы по необходимости.

- [Expressions](#expressions)
- [Operators](#operators)
- [Интервью](./3_js_interview.md)

Соседние темы Core: [Язык](./3_js_core.md) · [Переменные](./3_js_variables.md) · [Типы данных](./3_js_data_types.md) · [Структуры данных](./3_js_data_structures.md) · [Циклы](./3_js_loops.md) · [Массив](./3_js_array.md) · [Функции](./3_js_functions.md) · [Объекты](./3_js_objects.md) · [Классы](./3_js_classes.md).

<a name="expressions"></a>

## Expressions

<details>
  <summary>Statement vs expression</summary>

### Statement vs expression

**Definition:**

**оператор** выполняет действие (назначить, зациклить, объявить). **выражение** принимает значение (`42`, `a + b`, `fn()`).

| | Заявление | Выражение |
| | --- | ----------- |
| | Возвращаемое значение | Нет | Да |
| | Примеры | `if`, `for`, `const x = 1` | `1 + 2`, тройная, стрелка fn тело без фигурных скобок |

**When to use:** Предпочитайте выражения в функциональных конвейерах; заявления о потоке управления и побочных эффектах на границах.

**Examples:**

```js
const doubled = items.map((x) => x * 2); // expression-bodied callback
if (items.length === 0) return []; // statement
```

**Thoughts:**

`const x = (y = 1)` — присваивание внутри выражения допустимо, но ухудшает читабельность.

---

</details>

<details>
  <summary>ASI (automatic semicolon insertion)</summary>

### ASI (automatic semicolon insertion)

**Definition:** Когда операторы разделяются переносом строки, движок часто вставляет неявный оператор. `;`. Так происходит **не** всегда — начало некоторых строк продолжает предыдущее выражение.

**How it works:**

- Большинство новых строк действуют как конец оператора: `alert('Hello')\nalert('World')` все в порядке.
- Новая строка после неполного выражения **не** вставляется. `;` (например, заканчивая на `+`).
- Новая строка перед `[...]` **не** вставляет `;` — следующая строка может быть проанализирована как доступ к свойству предыдущего выражения.
- Тот же класс огнестрельного оружия: голый `return` затем возвращается новая строка `undefined` (вставки ASI `;` после `return`).

**When to use:** Поставьте явное `;` после каждого заявления (по умолчанию сообщества). Дополнительный `;` после блоков/`for` безвреден; пропавших без вести раньше `[`, `(`, `` ` ``, `/` — опасные случаи.

**Examples:**

```js
alert("Hello");
[1, 2].forEach(alert); // Hello, then 1, then 2

alert("Hello")[(1, 2)].forEach(alert); // treated as alert("Hello")[1, 2].forEach(alert) — broken
```

**Thoughts:** ASI — вот почему руководства по стилю предпочитают точки с запятой — ошибки редки, но их трудно обнаружить.

**Sources:** [javascript.info/structure](https://javascript.info/structure) · [MDN ASI](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#automatic_colon_insertion)

</details>

<details>
  <summary>Expressions</summary>

### Expressions

**Definition:** An **expression** evaluates to a value; a **statement** performs an action and does not produce a usable value on its own (though it may contain expressions).

**How it works:**

- Expression kinds: literals, identifiers, operators (`a + b`), calls (`fn()`), `new`, `await`, arrows, ternary, template literals, object/array literals.
- **Assignment** is an expression: `x = y = 1` chains because `=` returns the assigned value.
- **Statement** examples: `if`, `for`, `return`, `const x = 1` (declaration statement; initializer is an expression).
- Distinguish from [Statement vs expression](#statement-vs-expression) for ASI and control flow.

| Expression type | Example                   |
| --------------- | ------------------------- |
| Arithmetic      | `3 + 4` → `7`             |
| String          | `'Hello ' + 'World'`      |
| Logical         | `true && false` → `false` |
| Assignment      | `x = 10` → `10`           |

**When to use:** Expression-bodied arrows and `map` callbacks; statements at boundaries (I/O, mutations).

**Examples:**

```js
const ok = items.length > 0 && items[0].id; // expression
if (!ok) return null; // statement

const label = user ? user.name : "Guest"; // ternary expression
```

**Thoughts:** Every expression produces a value (except thrown errors). Prefer clarity over clever nested ternaries.

**Sources:** [MDN — Expressions and operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_operators)

</details>

<a name="operators"></a>

## Operators

<details>
  <summary>Operators</summary>

### Operators

**Definition:**

Операторы выполняют действия с операндами — арифметические, присваивания, сравнения, логические и т. д.

| Тип        | Операторы                              |
| ---------- | -------------------------------------- |
| Арифметика | `+`, `-`, `*`, `/`, `%`, `**`          |
| Назначение | `=`, `+=`, `-=`, `??=`                 |
| Сравнение  | `==`, `===`, `>`, `<`, `!==`, `!=`     |
| Логический | `&&`, `\|\|`, `!`, `??`                |
| Побитовый  | `&`, `\|`, `^`, `~`, `<<`, `>>`, `>>>` |
| Тройная    | `cond ? a : b`                         |

**Sources:** [Операторы MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators)

</details>

<details>
  <summary>Math operators</summary>

### Math operators

**Definition:** Арифметические и унарные операторы над числами; `+` также объединяет строки, если любой из операндов является строкой.

**How it works:**

- `+` со строкой → объединение строк; в противном случае числовое добавление (с принуждением).
- `-`, `*`, `/`, `%`, `**` преобразовать операнды в числа.
- Унарный `+x` принуждает к номеру; унарный `-x` отрицает.
- `++`/`--` префикс возвращает новое значение; postfix возвращает старое значение, а затем обновляет.
- `%` это остаток, а не математический модуль для отрицаний.
- Назначение `=` является оператором: он записывает, а затем **возвращает** присвоенное значение, поэтому `a = b = c = 2` цепи справа налево. Предпочитайте отдельные строки для удобства чтения.
- Запятая `,` вычисляет слева направо и возвращает **последнее** значение (`(1 + 2, 3 + 4)` → `7`). Редкий снаружи `for` инициализировать; знайте это при чтении кода библиотеки.

**When to use:** Использовать `Number(x)` or `+x` за явное принуждение; всегда передавайте систему счисления в `parseInt(s, 10)`.

**Pluses and minuses:**

| +                               | −                                         |
| ------------------------------- | ----------------------------------------- |
| `**` для возведения в степень   | `+` двойная роль вызывает ошибки          |
| Унарный `+` быстрое принуждение | Округление с плавающей запятой (IEEE-754) |

**Examples:**

```js
"5" + 1; // '51' (string concat)
"5" - 1; // 4 (numeric coercion)
+"42"; // 42
parseInt("08", 10); // 8
Number.isFinite(42); // true
1n + 2n; // 3n — bigint, cannot mix with number without conversion

let a, b, c;
a = b = c = 2 + 2; // all 4
let x = (1 + 2, 3 + 4); // 7 — comma returns last
```

**Sources:** [javascript.info/operators](https://javascript.info/operators)

</details>

<details>
  <summary>Comparisons</summary>

### Comparison operators

**Definition:** Операторы сравнения возвращают логические значения (или управляют `sort` компараторы). `===` избегает принуждения; `==` сначала принуждает.

**How it works:**

- `===` / `!==` — без преобразования типов; разные типы всегда неравны.
- `==` / `!=` — применять абстрактные правила равенства (удивительно, `''`, `0`, `false`).
- Реляционный `>`, `<`, `>=`, `<=` — обычно конвертируют в числа; строки сравниваются лексикографически, если обе строки являются строками.
- `Object.is` для крайних случаев (`NaN`, знак нуля) — см. [Object.is и равенство](#object-is-and-equality).
- **`null` / `undefined` причуды:** `null == undefined` is `true`, но ни одно из них не равно никакому другому значению с `==`. Реляционные операции преобразования `null` → `0` и `undefined` → `NaN`, so `null >= 0` is `true` пока `null > 0` и `null == 0` оба `false`.

**When to use:** По умолчанию `===`; `value == null` для нулевого/неопределенного; Компараторы числовой сортировки возвращают отрицательный/ноль/положительный результат. **не** используйте `>`, `<`, `>=`, `<=` по значению, которое может быть `null`/`undefined` — проверьте их отдельно.

**Pluses and minuses:**

| Подход      | +                   | −                          |
| ----------- | ------------------- | -------------------------- |
| `===`       | Предсказуемый       | Подробные проверки на ноль |
| `==`        | `null == undefined` | Пулеметы (`'' == 0`)       |
| `Object.is` | Правильный `NaN`    | Ниша                       |

**Examples:**

```js
null === undefined; // false
null == undefined; // true
"2" > "12"; // true — string compare
"2" > 12; // false — number compare

null > 0; // false
null == 0; // false — == has a special null/undefined rule
null >= 0; // true  — >= converts null → 0
undefined > 0; // false (NaN)
undefined == 0; // false
```

**Sources:** [javascript.info/comparison](https://javascript.info/comparison)

</details>

<details>
  <summary>Object.is & equality</summary>

### Object.is & equality

**Definition:** Множественные механизмы равенства: `==` (принуждение), `===` (строгий), `Object.is` (то же значение) и `SameValueZero` (используется `Map`/`Set`).

**How it works:**

- `===` — того же типа и значения; `NaN !== NaN`, `+0 === -0`.
- `Object.is` - нравиться `===` но `Object.is(NaN, NaN)` is `true`, `Object.is(+0, -0)` is `false`.
- `SameValueZero` — `Map`/`Set` поиск ключей; `NaN` равно `NaN`.
- `==` — приводит операнды (избегайте, кроме `value == null`).

| Сравнение   | `NaN === NaN` | `+0 === -0` | Принуждение |
| ----------- | ------------- | ----------- | ----------- |
| `===`       | ложный        | правда      | нет         |
| `Object.is` | правда        | ложный      | нет         |
| `==`        | ложный        | правда      | да          |

**When to use:** `Object.is` для `NaN` чеки; `===` повсюду; `Number.isNaN(x)` для одного значения `NaN` тест без принуждения.

**Pluses and minuses:**

| + (`Object.is`)       | −                                           |
| --------------------- | ------------------------------------------- |
| Правильный `NaN` чеки | Редко требуется против `===`                |
| Отличает `+0`/`-0`    | Не используется `Map`/`Set` (SameValueZero) |

**Examples:**

```js
NaN === NaN; // false
Object.is(NaN, NaN); // true
Object.is(+0, -0); // false
+0 === -0; // true

const m = new Map();
m.set(NaN, "val");
m.get(NaN); // 'val' — SameValueZero
```

**Sources:** [MDN Object.is](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is)

</details>

<details>
  <summary>Logical operators</summary>

### Logical operators

**Definition:** `&&`, `||` и `!` используют **короткое замыкание** — возвращают операнд, определивший результат, а не всегда `boolean`.

**How it works:**

- `a || b` — если `a` truthy, результат `a`; иначе `b`.
- `a && b` — если `a` falsy, результат `a`; иначе `b`.
- `!a` — логическое отрицание после приведения.
- Шаблоны: `user && greet(user)`; `name || 'Guest'`.
- Значения по умолчанию для `null`/`undefined` → [Nullish coalescing operator](#nullish-coalescing-operator).

| Оператор | Возврат                                       |
| -------- | --------------------------------------------- |
| `\|\|`   | Первый truthy (или последний, если все falsy) |
| `&&`     | Первый falsy (или последний, если все truthy) |
| `!`      | Boolean                                       |

**When to use:** `||` когда любой falsy — «пусто»; `&&` для условного вызова.

**Pluses and minuses:**

| +                                      | −                                   |
| -------------------------------------- | ----------------------------------- |
| Краткие guard и значения по умолчанию  | `\|\|` считает `0` / `''` пустыми   |
| Короткое замыкание экономит вычисления | Вложенность хуже читается, чем `if` |

**Examples:**

```js
0 || "default"; // 'default'
name && greet(name);
user || createGuest();
```

**Sources:** [javascript.info/logical-operators](https://javascript.info/logical-operators)

</details>

<details>
  <summary>Bitwise operators</summary>

### Bitwise operators (basics)

**Definition:** Операторы, обрабатывающие числа как 32-битные целые числа: `&`, `|`, `^`, `~`, `<<`, `>>`, `>>>`.

**How it works:**

- Операнды перед операцией преобразуются в 32-битные целые числа со знаком.
- `&` И, `|` OR, `^` исключающее ИЛИ, `~` НЕ, сдвиги перемещают биты влево/вправо.
- `>>>` беззнаковый сдвиг вправо заполняется нулем.

**When to use:** Флаги разрешений, хеш-трюки, библиотеки, критичные к производительности. Большинству кода приложения никогда не требуются побитовые операции.

**Pluses and minuses:**

| +                            | −                                           |
| ---------------------------- | ------------------------------------------- |
| Быстрые флаги и маски        | Редко в типичном коде приложения            |
| Низкоуровневые битовые трюки | 32-битное ограничение на побитовые операции |

**Examples:**

```js
5 & 1; // 1 — odd check
1 << 3; // 8 — multiply by 8 via shift
```

**Sources:** [javascript.info/bitwise-operators](https://javascript.info/bitwise-operators)

</details>

<details>
  <summary>Optional chaining</summary>

### Optional chaining (`?.`)

**Definition:** `?.` прерывает доступ к свойству, индексу или вызову, если значение слева — `null` или `undefined`, вместо выброса ошибки.

**How it works:**

- `obj?.prop`, `obj?.[expr]`, `fn?.()` — останов на первом nullish шаге.
- `user?.sayHi(x++)` **не** вычисляет `x++`, если `user` nullish.
- Переменная слева от `?.` должна быть **объявлена** — `user?.x` с необъявленным `user` → `ReferenceError`.
- **Только чтение/удаление** — присваивание через `?.` недопустимо.
- С дефолтом: `user?.profile?.name ?? "Anonymous"` → [Nullish coalescing operator](#nullish-coalescing-operator).

**When to use:** Опциональные поля API и вложенный config; не скрывайте отсутствие обязательного корневого объекта.

**Pluses and minuses:**

| +                                             | −                                                |
| --------------------------------------------- | ------------------------------------------------ |
| Безопасный глубокий доступ без вложенных `if` | Может скрывать ошибки данных (тихий `undefined`) |

**Examples:**

```js
const user = null;
user?.profile?.name;
user?.admin?.();
user?.[key];
```

**Sources:** [javascript.info/optional-chaining](https://javascript.info/optional-chaining)

</details>

<details>
  <summary>Nullish coalescing operator</summary>

### Nullish coalescing operator (`??`)

**Definition:** `??` возвращает правый операнд только если левый — `null` или `undefined` (не для `0`, `''`, `false`).

**How it works:**

- `a ?? b` — nullish слева → `b`, иначе `a`.
- В отличие от `||`: `0 ?? 1` → `0`; `0 || 1` → `1`.
- `a ?? b || c` и `a || b ?? c` требуют скобок.
- С optional chaining: `config.port ?? 3000`, `response?.data ?? []`.

**When to use:** Дефолты в config, когда `0` и `''` допустимы; «нет значения» = только nullish.

**Pluses and minuses:**

| +                       | −                                       |
| ----------------------- | --------------------------------------- |
| Сохраняет `0` и `''`    | Смешивание `??` и `\|\|` нужны скобки   |
| Явная nullish-семантика | Легко заменить явную проверку без нужды |

**Examples:**

```js
0 || "default";
0 ?? "default"; // 0

const port = config.port ?? 3000;
const title = settings.title ?? "Untitled";
```

**Sources:** [javascript.info/nullish-coalescing-operator](https://javascript.info/nullish-coalescing-operator)

</details>
