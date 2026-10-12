# JS Core — Data Structures

Объекты, коллекции, классические деревья/графы, даты и JSON. Примитивы и типы — в [Типы данных](./3_js_data_types.md); привязки — в [Переменные](./3_js_variables.md). Сложность и собеседование → [Структуры данных (CS)](./7_special_data_structures.md).

[← К оглавлению JS](./3_js.md)

### Related docs

- [Типы данных](./3_js_data_types.md) · [Переменные](./3_js_variables.md) · [Массивы JS](./3_js_array.md) · [Язык JS](./3_js_core.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md) · [YAML vs JSON](./10_build_ci.md#yaml) · [JS Interview](./3_js_interview.md)

### Main concepts

| Область                    | Темы                                           |
| -------------------------- | ---------------------------------------------- |
| **Объекты**                | Plain objects, утилиты `Object.*`              |
| **Коллекции**              | `Map`, `Set`, `WeakMap`, `WeakSet`             |
| **Классические структуры** | Деревья, графы, связные списки, стек и очередь |
| **Обмен**                  | `Date`, `JSON.parse` / `stringify`             |

### Sections

- [Objects](#objects)
- [Collections](#collections)
- [Classic structures](#classic-structures)
- [Interop](#interop)
- [Objects and Object utilities](#objects-and-object-utilities)
- [Collections in short](#collections-in-short) · [Map](#map) · [Set](#set) · [WeakMap](#weakmap) · [WeakSet](#weakset)
- [Classic structures in short](#classic-structures-in-short) · [Trees](#trees) · [Graphs](#graphs) · [Linked lists](#linked-lists) · [Stack and queue](#stack-and-queue)
- [Date](#date) · [JSON](#json)
- [Интервью](./3_js_interview.md)

Соседние темы Core: [Язык](./3_js_core.md) · [Типы данных](./3_js_data_types.md) · [Переменные](./3_js_variables.md) · [Операторы](./3_js_operators.md) · [Циклы](./3_js_loops.md) · [Массив](./3_js_array.md).

<a name="data-structures"></a>

## Objects

### Objects

<a name="objects-and-object-utilities"></a>

<details>
  <summary>Objects and Object utilities</summary>

### Plain objects & Object utilities

**Definition:** **Простой объект** – это `{}` or `new Object()` — коллекция свойств «ключ-значение». `Object.*` статические методы работают с объектами без необходимости использования экземпляра.

**How it works:**

- Ключи представляют собой строки или `Symbol`; другие типы преобразуют в строку (`{[obj]: 1}` → `"[object Object]"`).
- Доступ к собственности: точка `obj.key` или кронштейн `obj['key']` / `obj[expr]`.
- Сокращение: `{ name, age }` равно `{ name: name, age: age }`; вычисленные ключи `{ [dynamic]: 1 }`.
- `in` проверяет цепочку прототипов; `Object.hasOwn(obj, key)` проверяет только свои свойства.
- Распространение / `Object.assign` мелкое слияние объектов.

| Метод                           | Возврат                                  |
| ------------------------------- | ---------------------------------------- |
| `Object.keys(obj)`              | Собственные перечислимые строковые ключи |
| `Object.values(obj)`            | Собственные перечислимые значения        |
| `Object.entries(obj)`           | `[key, value]` пары                      |
| `Object.assign(target, ...src)` | Цель после неглубокого слияния           |
| `Object.hasOwn(obj, prop)`      | Логическое значение (собственная опора)  |
| `Object.freeze(obj)`            | Замороженный объект (неглубокий)         |

| | `key in obj` | `Object.hasOwn(obj, key)` |
| | --- | -------------- |
| | Собственная недвижимость | да | да |
| | Унаследованное имущество | да | нет |
| | Используйте, когда | Необходимо знать, существует ли где-нибудь недвижимость | Нужны только собственные ключи (сейф `for...in` фильтр) |

**When to use:** Объекты конфигурации, полезные данные API, карты со строковыми ключами. Использовать `Map` для нестроковых ключей; см. [Объекты](./3_js_objects.md) · [Классы](./3_js_classes.md) для прототипов, дескрипторов и глубоких шаблонов объектов.

**Pluses and minuses:**

| +                                                   | −                                                                                                 |
| --------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| JSON-дружественные DTO                              | Ключи всегда преобразуются в строку/Символ                                                        |
| Буквальный синтаксис краток                         | Общие ссылки на неглубокий текст                                                                  |
| `Object.entries` + `fromEntries` для преобразований | До ES2015 не было гарантированного порядка итераций (теперь порядок вставки для строковых ключей) |

**Examples:**

```js
const user = { name: "Ann", age: 30 };
const { name, ...rest } = user;

"age" in user; // true
Object.hasOwn(user, "age"); // true
Object.hasOwn(user, "toString"); // false — inherited

const merged = { ...defaults, ...user };
const pairs = Object.fromEntries(
  Object.entries(user).map(([k, v]) => [k, String(v)]),
);
```

**Sources:** [javascript.info/object](https://javascript.info/object) · [javascript.info/keys-values-entries](https://javascript.info/keys-values-entries)

</details>

## Collections

### Collections

<a name="map-set-weakmap-weakset"></a>
<a name="collections-in-short"></a>

<details>
  <summary>Collections in short</summary>

### Collections in short

**Definition:** Четыре встроенных типа коллекций помимо простых объектов и массивов. `Map` является хранилищем ключей→значений; `Set` это мешок уникальных ценностей. `WeakMap` и `WeakSet` подобны своим сильным аналогам, но содержат **слабые** ссылки на ключи/значения объектов, поэтому сборщик мусора может их вернуть, когда ничто другое не ссылается на эти объекты.

| | `Map` | `Set` | `WeakMap` | `WeakSet` |
| | --------------------- | ------------------------------ | ------------------- | ----------------------- |
| | Магазины | Пары ключ → значение | Уникальные значения | Ключ (объект) → значение | Уникальные объекты |
| | Типы ключей/значений | Любой тип | Любой тип | **Только объекты** (ключи) | **Только объекты** |
| | Повторяющиеся ключи/значения | Ключи уникальные; `set` перезаписывает | Уникальные ценности | Ключи уникальные | Уникальные ценности |
| | Итерируемый | Да (`for...of`) | Да | Нет | Нет |
| | `.size` | Да | Да | Нет | Нет |
| | ГК | Сильные ссылки — сохраняют ключи живыми | Сильные ссылки | Слабые ссылки на **ключи** | Слабые ссылки на **значения** |
| | Сериализация | Нет `JSON.stringify` | No `JSON.stringify` | Нет | Нет |

**Thoughts:**

Использовать `Map`/`Set` когда вам нужна семантика коллекции (любой тип ключа, уникальность, `.size`, итерация). Использовать `WeakMap`/`WeakSet` для **сторонних метаданных**, прикрепленных к объектам, без предотвращения сбора этих объектов.

**Decision guide:**

```text
Need primitives as keys/values?     → Map / Set
Need to loop or know .size?          → Map / Set
Attach hidden data to objects?       → WeakMap
Tag "already seen" object refs?      → WeakSet
Prevent GC of keys/values?           → Map / Set (strong refs)
Let GC reclaim when object unused?   → WeakMap / WeakSet
```

Подробнее → [Map](#map) · [Set](#set) · [WeakMap](#weakmap) · [WeakSet](#weakset).

#### Common pitfalls

```js
// Pitfall 1: object keys in Map — must keep same reference
const key = { id: 1 };
const map = new Map([[key, "a"]]);
map.get({ id: 1 }); // undefined — different object
map.get(key); // 'a'

// Pitfall 2: WeakMap key collected → entry gone (by design)
let obj = {};
const wm = new WeakMap();
wm.set(obj, "data");
obj = null; // eventually GC removes the entry

// Pitfall 3: Set dedupe uses SameValueZero, not deep equality
new Set([{ a: 1 }, { a: 1 }]).size; // 2
```

**Sources:** [javascript.info/map-set](https://javascript.info/map-set) · [javascript.info/weakmap-weakset](https://javascript.info/weakmap-weakset) · [Карта MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

</details>

<a name="map"></a>

<details>
  <summary>Map</summary>

### Map

**Definition:** Заказанный сбор `[key, value]` записи. Ключи могут быть любого типа — примитивы, объекты, функции, символы — а не только строки/символы, такие как простые объекты.

**How it works:**

- Создано с `new Map()` or `new Map(iterable)` где итерация дает `[key, value]` пары.
- При поиске ключей используется равенство **SameValueZero** (`NaN` равно `NaN`; `+0` равно `-0`).
- Порядок вставки сохранен; `for...of` повторяет записи в этом порядке.
- API: `set`, `get`, `has`, `delete`, `clear`, `size`, `keys`, `values`, `entries`, `forEach`.
- Ключи объекта хранятся **по ссылке**, а не в строковой форме — это две разные `{id:1}` объекты - это два разных ключа.

**When to use:**

| Использовать `Map`                                                            | Используйте простой `Object`                         |
| ----------------------------------------------------------------------------- | ---------------------------------------------------- |
| Ключи — это объекты, узлы DOM или нестроки                                    | Сериализация JSON/REST DTO                           |
| Частое добавление/удаление записей                                            | Конфигурационные литералы фиксированной формы        |
| Нуждаться `.size` или итерация в порядке размещения                           | Нуждаться `Object.keys` / эргономика распространения |
| Ключи не должны конфликтовать из-за `toString()` (`{}` → `"[object Object]"`) | Методы прототипа или `hasOwnProperty` узоры          |

**Pluses and minuses:**

| +                                                                     | −                                                                                                     |
| --------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| Любой тип ключа (объекты, функции)                                    | Нет сериализации JSON                                                                                 |
| Предсказуемый порядок итераций                                        | Немного больше памяти, чем у обычного объекта, для случаев, когда используются только строковые ключи |
| `.size` за O(1)                                                       | Нет цепочки прототипов/нет наследуемых ключей                                                         |
| Частый `set`/`delete` без накладных расходов на преобразование ключей | Менее знакомо читателям, ожидающим `{}`                                                               |

**Examples:**

```js
const user = { id: 1 };
const meta = new Map([
  ["name", "Ann"],
  [user, { lastLogin: Date.now() }], // object as key
]);

meta.set(42, "answer");
meta.get(user); // { lastLogin: ... }
meta.has("name"); // true
meta.size; // 3

for (const [key, value] of meta) {
  console.log(key, value);
}

// SameValueZero
const m = new Map();
m.set(NaN, "not a number");
m.get(NaN); // 'not a number'
```

</details>

<a name="set"></a>

<details>
  <summary>Set</summary>

### Set

**Definition:** Упорядоченная коллекция **уникальных** значений. Добавление дубликата не требуется (то же значение для SameValueZero).

**How it works:**

- Создано с `new Set()` or `new Set(iterable)`.
- `add`, `has`, `delete`, `clear`, `size`, итерация через `for...of` / `values()` (то же, что и записи для набора).
- Уникальность примитивов определяется по значению (SameValueZero), а не по идентификатору ссылки.
- Две разные ссылки на объекты всегда являются отдельными записями.

**When to use:**

| Использовать `Set`                              | Использовать `Array`                                        |
| ----------------------------------------------- | ----------------------------------------------------------- |
| Уникальность (`[...new Set(arr)]`)              | Заказ + дубликаты имеют значение                            |
| Быстрая циклическая проверка членства           | Нуждаться `map`/`filter`/`reduce` по индексированным данным |
| Списки тегов, посещенные узлы, флаги разрешений | Список сериализуемых JSON                                   |

**Pluses and minuses:**

| +                                        | −                                               |
| ---------------------------------------- | ----------------------------------------------- |
| O(1) среднее `has` / `add` / `delete`    | Нет доступа к индексу (`set[0]` недействителен) |
| Дедупликация в одну строку               | Нет сопоставления ключей → значений             |
| Итерируемый, удобный для распространения | Объекты сравниваются только по ссылке           |

**Examples:**

```js
const tags = new Set(["js", "react", "js"]); // {'js', 'react'}
tags.add("ts");
tags.has("js"); // true
tags.delete("react");

// Deduplicate array
const unique = [...new Set([1, 2, 2, 3])]; // [1, 2, 3]

// Set operations
const a = new Set([1, 2, 3]);
const b = new Set([2, 3, 4]);
const intersection = [...a].filter((x) => b.has(x)); // [2, 3]

// Objects: reference identity
const s = new Set();
s.add({ id: 1 });
s.add({ id: 1 });
s.size; // 2 — two different object references
```

</details>

<a name="weakmap"></a>

<details>
  <summary>WeakMap</summary>

### WeakMap

**Definition:** Структура, подобная карте, чьи **ключи должны быть объектами** (или зарегистрированными символами в некоторых движках). Ключи удерживаются **слабо**: если ключевой объект не имеет других сильных ссылок, он и его запись могут быть подвергнуты сборке мусора. Ценности твердо придерживаются.

**How it works:**

- Только методы: `get`, `set`, `has`, `delete` — no `size`, `clear`или итерация.
- Вы не можете перечислять ключи — API намеренно минимален, чтобы сборщик мусора мог работать без раскрытия неработающих ключей.
- Типичный шаблон: частные/сторонние данные об объектах, которыми вы не владеете (узлы DOM, экземпляры классов).
- Если ключ собран, `get(key)` в любом случае нельзя было бы вызвать; запись бесшумно исчезает.

**When to use:**

| Использовать `WeakMap`                                                   | Использовать `Map` вместо этого   |
| ------------------------------------------------------------------------ | --------------------------------- |
| Кэш или метаданные, идентифицированные по идентификатору объекта         | Необходимо перебрать записи       |
| Узел DOM → состояние обработчика (браузер)                               | Ключи включают примитивы          |
| Шаблон частных полей раньше `#` or `Symbol`                              | Нуждаться `.size` or `clear()`    |
| Экземпляр → вычисляемый кеш, который должен умереть вместе с экземпляром | Долговечный реестр всех элементов |

**Pluses and minuses:**

| +                                                                             | −                                                  |
| ----------------------------------------------------------------------------- | -------------------------------------------------- |
| Отсутствие утечек памяти из кэшированных метаданных на недолговечных объектах | Невозможно выполнить итерацию или получить `.size` |
| Подходит для библиотек, прикрепляющих данные к посторонним объектам           | Ключи должны быть объектами                        |
| Скрывает данные от `Object.keys` / сериализация                               | Невозможно легко проверить содержимое для отладки  |

**Examples:**

```js
const privateData = new WeakMap();

class User {
  constructor(name) {
    privateData.set(this, { name, visits: 0 });
  }
  getName() {
    return privateData.get(this).name;
  }
  visit() {
    privateData.get(this).visits++;
  }
}

const u = new User("Ann");
u.visit();
// When `u` is no longer referenced, the WeakMap entry is GC-eligible.

// DOM metadata (browser)
const listeners = new WeakMap();
function track(el, handler) {
  listeners.set(el, handler);
  // When `el` is removed from DOM and unreferenced, entry can be collected.
}
```

</details>

<a name="weakset"></a>

<details>
  <summary>WeakSet</summary>

### WeakSet

**Definition:** Структура, подобная множеству, которая принимает только **объекты**. Членство является слабой ссылкой: если у объекта нет других сильных ссылок, он автоматически выпадает из набора.

**How it works:**

- Методы: `add`, `has`, `delete` — нет итерации, нет `size`, no `clear`.
- `add` возвращает набор (например, `Set`).
- Полезно в качестве **набора тегов**: «мы уже обработали этот объект?»

**When to use:**

| Использовать `WeakSet`                                     | Использовать `Set` вместо этого                |
| ---------------------------------------------------------- | ---------------------------------------------- |
| Отметить объекты как посещенные/обработанные               | Сохранение примитивов                          |
| Отказ от участия или черный список экземпляров объектов    | Необходимо перебрать или подсчитать участников |
| Предотвратить двойную регистрацию одного и того же объекта | Сохранение членства между сеансами             |

**Pluses and minuses:**

| +                                                   | −                                           |
| --------------------------------------------------- | ------------------------------------------- |
| Автоматическая очистка при удалении объектов        | Нет возможности перечислить участников      |
| Дешево «видели этот объект?» чеки                   | Только объекты                              |
| Предотвращает утечку памяти в долгоживущих реестрах | Невозможно сериализовать или сделать снимок |

**Examples:**

```js
const processed = new WeakSet();

function handleOnce(obj) {
  if (processed.has(obj)) return;
  processed.add(obj);
  // ... expensive work
}

const node = document.createElement("div");
handleOnce(node);
handleOnce(node); // skipped

// Circular structures — WeakSet detects revisiting same object
function deepWalk(obj, seen = new WeakSet()) {
  if (obj && typeof obj === "object") {
    if (seen.has(obj)) return;
    seen.add(obj);
    for (const v of Object.values(obj)) deepWalk(v, seen);
  }
}
```

</details>

## Classic structures

### Classic structures

<a name="classic-structures-in-short"></a>

<details>
  <summary>Classic structures in short</summary>

### Classic structures in short

**Definition:** Помимо встроенных `Object` / `Map` / `Set`, **абстрактные** структуры (дерево, граф, список, стек, очередь) — это **паттерны**, которые вы собираете из объектов, массивов и ссылок. В языке нет классов `Tree` или `Graph`.

**How it works:**

| Structure       | Idea                 | Typical JS shape                                 | In the wild                  |
| --------------- | -------------------- | ------------------------------------------------ | ---------------------------- |
| **Tree**        | Иерархия, без циклов | `{ val, children[] }` или `{ val, left, right }` | DOM, JSON, деревья React     |
| **Graph**       | Узлы и рёбра         | adjacency list `Map` / `Record`                  | Граф модулей, зависимости    |
| **Linked list** | Цепочка `next`       | `{ val, next }`                                  | LRU, задачи на собеседовании |
| **Stack**       | LIFO                 | `push` / `pop`                                   | undo, DFS со стеком          |
| **Queue**       | FIFO                 | массив + индекс головы или узлы                  | BFS, очереди задач           |

Индексированные списки → [Массивы JS](./3_js_array.md). Таблицы сложности → [Linear (CS)](./7_special_data_structures_linear.md) · [Trees & graphs (CS)](./7_special_data_structures_trees.md).

В этом файле → [Trees](#trees) · [Graphs](#graphs) · [Linked lists](#linked-lists) · [Stack and queue](#stack-and-queue).

**Sources:** [Choosing a data structure](./7_special_data_structures.md#choosing-a-data-structure) · [javascript.info/array](https://javascript.info/array)

</details>

<a name="trees"></a>

<details>
  <summary>Trees</summary>

### Trees

**Definition:** **Дерево** — иерархия узлов с родителем/детьми и **без циклов**. **Бинарное** дерево — не более двух детей (`left` / `right`).

**How it works:**

- **N-арное:** `{ value, children: [...] }` — меню, файловые деревья.
- **Бинарное:** `{ val, left, right }`, `null` для пустого ребёнка.
- **Обход:** DFS — рекурсия или явный **стек**; BFS — **очередь**.
- **Браузер:** DOM — живое дерево → [DOM tree](./3_js_browser.md#dom-tree).
- Глубокие деревья — итерация или лимит рекурсии → [стек вызовов](./3_js_data_types.md#memory-stack-heap).

**When to use:** Вложенный UI, конфиги, выражения. BST / trie → [Tree (CS)](./7_special_data_structures_trees.md#tree).

**Examples:**

```js
const binary = {
  val: 2,
  left: { val: 1, left: null, right: null },
  right: { val: 3, left: null, right: null },
};

function walk(node, visit) {
  if (!node) return;
  visit(node.val);
  walk(node.left, visit);
  walk(node.right, visit);
}
```

**Sources:** [Tree (CS)](./7_special_data_structures_trees.md#tree) · [Tree traversal (CS)](./7_special_data_structures_trees.md#tree-traversal-recursive-vs-iterative)

</details>

<a name="graphs"></a>

<details>
  <summary>Graphs</summary>

### Graphs

**Definition:** **Граф** — вершины и **рёбра**; направленный или нет; возможны циклы (в отличие от дерева).

**How it works:**

- **Список смежности:** `Map<узел, сосед[]>` или `Record<string, string[]>`.
- **Матрица смежности** — редко в обычном JS, если граф не маленький и плотный.
- **Обход:** BFS (очередь) — кратчайшие пути без весов; DFS — связность, циклы, топосорт.
- Бандлеры строят **граф** из `import`.

**When to use:** Маршруты, рекомендации, автоматы, зависимости пакетов → [Graph (CS)](./7_special_data_structures_trees.md#graph).

**Examples:**

```js
const graph = new Map([
  ["A", ["B", "C"]],
  ["B", ["D"]],
  ["C", []],
  ["D", []],
]);

function bfs(start, visit) {
  const q = [start];
  const seen = new Set([start]);
  while (q.length) {
    const v = q.shift();
    visit(v);
    for (const n of graph.get(v) ?? []) {
      if (!seen.has(n)) {
        seen.add(n);
        q.push(n);
      }
    }
  }
}
```

**Sources:** [Graph (CS)](./7_special_data_structures_trees.md#graph) · [Graph Theory — Interview](./7_algo_graphs_interview.md)

</details>

<a name="linked-lists"></a>

<details>
  <summary>Linked lists</summary>

### Linked lists

**Definition:** **Связный список** — узлы `{ val, next }` (двусвязный: `prev` + `next`). Нет O(1) доступа по индексу — только от `head`.

**How it works:**

- **Односвязный:** храните `head` (и при необходимости `tail`).
- **Двусвязный:** O(1) удаление при ссылке на узел.
- **Массив** в JS — список по умолчанию; связные списки — собеседования и LRU.

**When to use:** Паттерны интервью, кэши. Таблица операций → [Linked list (CS)](./7_special_data_structures_linear.md#linked-list).

**Examples:**

```js
function append(head, val) {
  const node = { val, next: null };
  if (!head) return node;
  let cur = head;
  while (cur.next) cur = cur.next;
  cur.next = node;
  return head;
}
```

**Sources:** [Linked list (CS)](./7_special_data_structures_linear.md#linked-list) · [Linked Lists — Interview](./7_ds_linked_lists_interview.md)

</details>

<a name="stack-and-queue"></a>

<details>
  <summary>Stack and queue</summary>

### Stack and queue

**Definition:** **Стек** — LIFO; **очередь** — FIFO. В JS обычно на **массивах** или связных узлах.

**How it works:**

| ADT   | На массиве          | Подводный камень в JS |
| ----- | ------------------- | --------------------- |
| Stack | `push` / `pop`      | O(1) с конца массива  |
| Queue | хвост + индекс head | `shift()` — **O(n)**  |

- **Приоритетная очередь** — куча в [Hash & heap (CS)](./7_special_data_structures_advanced.md).
- Стек — DFS и undo; очередь — BFS и задачи.

**When to use:** Парсинг, undo, шаблоны BFS/DFS → [Stack](./7_special_data_structures_linear.md#stack) · [Queue](./7_special_data_structures_linear.md#queue).

**Examples:**

```js
const stack = [];
stack.push(1);
stack.pop();

const queue = [];
let head = 0;
queue.push("a");
queue.push("b");
const item = queue[head++];
```

**Sources:** [Stack (CS)](./7_special_data_structures_linear.md#stack) · [Queue (CS)](./7_special_data_structures_linear.md#queue)

</details>

## Interop

### Interop

<details>
  <summary>Date</summary>

<a name="date"></a>

### Date

**Definition:** `Date` хранит одну временную метку — миллисекунды с **1 января 1970 года по всемирному координированному времени** (эпоха Unix). Локальные геттеры/сеттеры используют часовой пояс времени выполнения; `getUTC*` / `setUTC*` используйте UTC.

**How it works:**

- Создавать: `new Date()` (сейчас), `new Date(ms)`, `new Date(dateString)` (тот же алгоритм, что и `Date.parse`), `new Date(y, monthIndex, …)` — **месяц — 0–11**.
- Читать: `getFullYear`, `getMonth` (0–11), `getDate` (день месяца 1–31), `getDay` (день недели 0=Вс…6=Сб), `getHours`…, `getTime()`, `getTimezoneOffset()`.
- Писать: `setFullYear` / `setMonth` / `setDate` / `setHours`… / `setTime(ms)` — значения, выходящие за пределы допустимого диапазона **автокоррекция** (например, 32 января → 1 февраля).
- Разница: `date1 - date2` or `getTime()` разница → миллисекунды. `Date.now()` текущая временная метка без выделения `Date`.
- Предпочитать `getFullYear()` — никогда не используйте устаревшие `getYear()`.

**When to use:** Временные метки, прошедшее время, простая календарная математика с помощью автокоррекции. Дисплей → `Intl.DateTimeFormat` ([Intl](./3_js_extra.md#intl)). Сложные календари/часовые пояса → рассмотрите Temporal (если доступно) или специальную библиотеку.

#### Common pitfalls

```js
new Date(2011, 0, 1); // 1 Jan 2011 local — month 0 = January
new Date("2017-01-26"); // date-only → UTC midnight, then shown in local TZ
date.getDate(); // day of month — not getDay (weekday)
date.setDate(0); // last day of previous month
```

**Pluses and minuses:**

| +                                     | −                                                          |
| ------------------------------------- | ---------------------------------------------------------- |
| Встроенная, точность мс               | На основе месяца 0; особенности анализа строки даты        |
| Автокоррекция смещений                | Изменчивый; изменение общих дат является источником ошибки |
| Легкие различия по прошедшему времени | Никакой первоклассной арифметики TZ                        |

**Examples:**

```js
const start = Date.now();
// …work…
const elapsedMs = Date.now() - start;

const d = new Date(2016, 1, 28);
d.setDate(d.getDate() + 2); // 1 Mar 2016 (leap year)

Date.parse("2017-01-26"); // ms or NaN
```

**Thoughts:** _Эпоха мс — переносимое значение; `Date` — это представление с методами доступа local/UTC — индекс месяца и изменяемые API — это ловушки для собеседований._

**Sources:** [javascript.info/date](https://javascript.info/date) · [Дата MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) · [MDN Date.parse](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/parse)

</details>

<details>
  <summary>JSON</summary>

### JSON

**Definition:** **JSON** (нотация объектов JavaScript) — это наиболее распространенный текстовый формат для обмена данными между приложениями. Он выглядит как объект JS, но это независимая от языка спецификация ([ECMA-404](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/)); то, как JS использует его, описано в ECMAScript. Файлы используют `.json` расширение (`package.json` самый известный пример).

**How it works:**

JSON — это пары ключ-значение, разделенные `,`. Ключ всегда представляет собой **строку, заключенную в двойные кавычки**; `:` разделяет ключ и значение. Ценности могут быть:

| Тип        | Пример               |
| ---------- | -------------------- |
| Строка     | `"I love JSON!"`     |
| Номер      | `21`                 |
| Логическое | `true` / `false`     |
| Массив     | `[18, true, "lost"]` |
| Объект     | `{"isValid": true}`  |
| `null`     | `null`               |

Типичный сетевой поток: запрос → ожидание → получение текста JSON → `JSON.parse` → используйте значения JS. `fetch` помощники: `await res.json()`.

**`JSON.stringify(value[, replacer, space])`** — значение JS → JSON **строка**. Тип результата `'string'` (or `undefined` для одинокой функции/`undefined`/`Symbol`).

Опущено (свойства объекта): `undefined`, функции/методы, `Symbol` значения, **Символические клавиши** (даже с `replacer`). Посещаются только **перечисляемые** свойства (`Object.keys` заказ).

Становиться `null`: `Infinity`, `NaN`; `undefined` **внутри массивов**. `Map` / `Set` преобразовать в строку `{}` (нет перечисляемых собственных ключей данных).

Броски **`TypeError`:** циклические ссылки, `BigInt` (если вы не добавите `toJSON`). `Date` / `Temporal` использовать `toJSON()` (строка ISO); синтаксический анализ **не** восстанавливает их как даты.

`replacer` — массив ключей (только эти свойства) или `(key, value) =>` трансформировать; возвращаться `undefined` пропустить. `space` — количество пробелов (ограничено до 10) или отступ строки для удобства чтения (`JSON.stringify(obj, null, 2)`). Обычай `toJSON()` на объекте переопределяет сериализацию.

**`JSON.parse(text[, reviver])`** — строка JSON → значение JS. Неверный JSON → **`SyntaxError`**. Обернуть ненадежный ввод в `try/catch`. `reviver(key, value)` работает в глубину; возвращаться `undefined` удалить свойство. Используйте его, чтобы оживить `Date` из строк ISO.

**When to use:**

Полезная нагрузка REST/API, `localStorage` строки, нпм `package.json` и другая конфигурация, состояние приложения отправляется на серверную часть. Альтернативы: **XML**, **YAML** (комментарии), Protobuf/MessagePack (размер). Глубокое клонирование JSON-безопасных данных в обе стороны; использовать `structuredClone` для циклов/`Date`/`Map`. JSON5 существует как более свободный диалект, а не как языковой стандарт.

#### Common pitfalls

- Ключи/строки должны содержать **двойные кавычки**; конечные запятые и комментарии JS недействительны.
- `undefined` в объект упал; в массиве это становится `null`.
- `JSON.parse(JSON.stringify(obj))` теряет функции, `undefined`, символы, повороты `Date` в строку, выдает циклически.
- `JSON.parse` не запускает JS — нет `new Date(...)` внутри текста.
- Сетевые API (например, OpenWeather) возвращают JSON; все еще анализировать/проверять на границе.

**Pluses and minuses:**

| +                                     | −                                                             |
| ------------------------------------- | ------------------------------------------------------------- |
| Универсальный формат API/конфигурации | Не полный клон (типы, функции, `undefined` потерял)           |
| Встроенный, быстрый, человекочитаемый | Циркулярные ссылки / `BigInt` бросить                         |
| Красивый принт с `space`              | Никаких комментариев в стандартном JSON (`//` недействителен) |
| У большинства языков есть библиотеки  | Нет встроенной проверки схемы                                 |

**Examples:**

```js
const hero = {
  nickname: "BestHealerEver",
  level: 7,
  things: ["sword", "helmet"],
  money: { gold: 6387, diamonds: 2 },
};

typeof hero; // 'object'
typeof JSON.stringify(hero); // 'string'
JSON.stringify(hero, ["nickname", "level"], 2);
// pretty JSON with only those keys

const json = '{"name":"Luke Skywalker","gender":"male","birth_year":"19BBY"}';
const jedi = JSON.parse(json);
jedi.name; // 'Luke Skywalker'

JSON.stringify({ fn() {}, n: NaN, u: undefined }); // '{"n":null}'
JSON.stringify([undefined]); // '[null]'
JSON.parse("{ invalid"); // SyntaxError
```

**Thoughts:** _JSON — это строгое подмножество данных синтаксиса объектов JS: преобразуйте его в строку для отправки, анализируйте для использования и никогда не рассматривайте его как полный клон объекта._

**Sources:** [Doka — JSON](https://doka.guide/js/json/) · [javascript.info/json](https://javascript.info/json) · [MDN stringify](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) · [MDN синтаксический анализ] (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)

</details>
