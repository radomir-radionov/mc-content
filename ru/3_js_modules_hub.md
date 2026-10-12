# JS — Modules

Область ES-модуля, статические `import` / `export` и dynamic `import()`. Ниже — структурированный обзор в аккордеонах; полная теория — на отдельных страницах тем.

[← К JS hub](./3_js.md)

### Related docs

- [Intl API](./3_js_extra.md) · [Language](./3_js_core.md) · [Async / Await — top-level await](./3_js_async_await.md#top-level-await) · [Node.js — Modules](./8_nodejs_modules.md) · [TypeScript — Modules](./5_ts_modules.md) · [Build — bundlers](./10_build_bundlers.md) · [JS Interview](./3_js_interview.md)

### Main concepts

| Topic              | Covers                                            |
| ------------------ | ------------------------------------------------- |
| **Modules**        | Область модуля, script vs module, ESM vs CommonJS |
| **Export/Import**  | Named/default, re-export, live bindings           |
| **Dynamic import** | Ленивый `import()`, code splitting, условные пути |

### Sections

- [Modules](#modules)
- [Export and Import](#export-and-import)
- [Dynamic imports](#dynamic-imports)

---

<a name="modules"></a>

## Modules

<details>
  <summary>Modules</summary>

### Modules

**Definition:** **Модуль** — единица JavaScript с **собственной областью верхнего уровня**. Привязки на верхнем уровне не становятся свойствами глобального объекта, пока вы явно их туда не повесите. Стандарт в современном коде — **ECMAScript modules (ESM)** (`import` / `export`). **CommonJS** (`require` / `module.exports`) всё ещё часто встречается в старых Node-пакетах.

**How it works:**

- К **модульному** коду автоматически применяется **strict mode**.
- **Классический скрипт** (`<script>` без `type="module"`): общий глобальный объект; `var` и function declaration могут стать глобалами.
- **Module script** (`<script type="module">`): как `defer`, один раз на URL модуля, есть `import` / `export`, top-level не попадает в `window`.
- **Оценка:** движок строит **граф** по статическим `import` / `export`. Тело модуля выполняется **один раз**; повторные импорты делят экземпляр и **live** export bindings.
- **Спецификаторы:** в браузере работают относительные и абсолютные URL; **bare** имена (`"lodash"`) — [import map](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script/type/importmap) или бандлер. Node резолвит bare из `node_modules`.
- **Node ESM:** `"type": "module"` в `package.json`, `.mjs` или настроенный ESM `.js`; `require()` — CommonJS и внутри ESM недоступен без `createRequire`.
- **Interop:** бандлеры и Node потребляют ESM и CJS; правила default export различаются — [Node.js — Modules](./8_nodejs_modules.md).

| Classic `<script>`        | `<script type="module">`       |
| ------------------------- | ------------------------------ |
| Global scope              | Module scope                   |
| Нет `import`/`export`     | `import` / `export`            |
| Сразу при разборе         | Deferred (порядок в документе) |
| `document.write` рискован | Строже (часто CORS)            |

**When to use:**

| Выбирайте ESM                      | CommonJS (пока)                |
| ---------------------------------- | ------------------------------ |
| Новые браузерные и Node-приложения | Legacy Node без миграции       |
| Tree-shaking с бандлером           | Синхронный `require` в тулинге |
| Общий код browser + Node           | Скрипты только на `require()`  |

Для опциональной или тяжёлой загрузки — [Dynamic imports](./3_js_dynamic_import.md).

**Pluses and minuses:**

| +                             | −                                        |
| ----------------------------- | ---------------------------------------- |
| Явные зависимости при разборе | Cross-origin module scripts — CORS       |
| Нет случайных глобалов        | Нет sync `require` в браузере            |
| Dead-code elimination         | Смешение ESM/CJS в Node требует внимания |

**Examples:**

```html
<script type="module">
  import { boot } from "./app.js";
  boot();
</script>
```

```js
// counter.js
let count = 0;
export function inc() {
  return ++count;
}
// main.js — оба импортёра делят один `count`
import { inc } from "./counter.js";
```

**Full topic:** [Modules](./3_js_modules.md)

**Sources:** [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) · [MDN — import maps](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script/type/importmap) · [javascript.info/modules-intro](https://javascript.info/modules-intro)

</details>

<a name="export-and-import"></a>

## Export and Import

<details>
  <summary>Export and Import</summary>

### Export and Import

**Definition:** **`export`** объявляет, какие привязки модуль отдаёт наружу; **`import`** связывает имена со спецификатором другого модуля. Статические `import` и `export` **синтаксические** — разрешаются до тела модуля и только на **верхнем уровне** (не в `if`, циклах, функциях).

**How it works:**

- **Named:** `export const id = 1;` · `export { id };` → `import { id } from "./mod.js"`.
- **Rename:** `export { id as userId };` · `import { id as userId } from "./mod.js"`.
- **Default:** не больше одного `export default` → `import anyName from "./mod.js"`.
- **Combined:** `import def, { a, b } from "./mod.js"`.
- **Namespace:** `import * as ns from "./mod.js"` (`ns.default` при наличии).
- **Side-effect:** `import "./init.js"` — только выполнение.
- **Re-export:** `export { a } from "./a.js";` · `export { default as B } from "./b.js";` · `export * from "./c.js"` (default не реэкспортируется).
- **Live bindings:** `import { x }` следует за экспортёром; присвоение `x` в импортёре — `TypeError`.
- **Hoisting:** импорты выполняются первыми в порядке зависимостей.
- **`import.meta`:** в модулях `import.meta.url` — URL модуля (ассеты, пути в browser/Node ESM).

#### Common pitfalls

```js
// SyntaxError — static import нельзя внутри блоков
// if (dev) import { debug } from "./debug.js";

// Live binding — при count++ у экспортёра импортёры видят новое значение
import { count } from "./state.js";
```

**When to use:**

| Паттерн            | Типично                                   |
| ------------------ | ----------------------------------------- |
| Named exports      | Утилиты, несколько компонентов, константы |
| Default export     | Один главный экспорт (компонент, плагин)  |
| Barrel             | Публичный API папки пакета                |
| Side-effect import | Полифиллы, регистрация CSS/телеметрии     |

**Pluses and minuses:**

| +                            | −                                     |
| ---------------------------- | ------------------------------------- |
| Статический граф для tooling | Нужна договорённость default vs named |
| Tree-shaking                 | Условные deps — `import()`            |
| Явный public API             | Импорты нельзя переприсвоить          |

**Examples:**

```js
// math.js
export const PI = 3.14;
export function add(a, b) {
  return a + b;
}
export default function multiply(a, b) {
  return a * b;
}

// app.js
import multiply, { PI, add } from "./math.js";
```

```js
// api/index.js — barrel
export { User } from "./user.js";
export { listPosts } from "./posts.js";
```

**Full topic:** [Export and Import](./3_js_import_export.md)

**Sources:** [MDN — export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) · [MDN — import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) · [MDN — import.meta](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import.meta) · [javascript.info/import-export](https://javascript.info/import-export)

</details>

<a name="dynamic-imports"></a>

## Dynamic imports

<details>
  <summary>Dynamic imports</summary>

### Dynamic imports

**Definition:** **`import(specifier)`** — выражение: загружает модуль **в момент выполнения** и возвращает **Promise** с объектом пространства имён. В отличие от static `import`, допустимо в функциях, ветках и в классических скриптах.

**How it works:**

- **Результат:** `const m = await import("./chart.js")` — `m.default` для default; named — свойства `m`.
- **Время:** загрузка и оценка в точке вызова; цель не в начальном статическом графе.
- **Спецификатор:** строка или template literal; бандлеры могут выносить chunks.
- **Bare в браузере:** `import("pkg")` без import map — `TypeError`.
- **Кэш:** после первой успешной загрузки — тот же экземпляр, что у static import.
- **Ошибки:** сеть/парсинг **reject** promise — `try` / `catch` в UI.
- **Top-level await:** `await import(...)` на верхнем уровне ES-модуля — [Top-level await](./3_js_async_await.md#top-level-await).
- **Бандлеры:** Vite, webpack, Rollup и др. — **async chunks** на dynamic `import()`.

**When to use:**

| Static `import`       | Dynamic `import()`               |
| --------------------- | -------------------------------- |
| Код старта приложения | UI по маршруту (editor, admin)   |
| Общие utils всегда    | Локали по языку                  |
| Простой граф          | Feature flags, эксперименты      |
|                       | Триггеры click / idle / observer |

**Pluses and minuses:**

| +                      | −                                    |
| ---------------------- | ------------------------------------ |
| Меньший первый payload | Задержка при первом открытии chunk   |
| Вне module scripts     | Сложнее статический анализ           |
| С `async` / `await`    | Много мелких chunks — много запросов |

**Examples:**

```js
button.addEventListener("click", async () => {
  const { openEditor } = await import("./editor.js");
  openEditor();
});
```

```js
const locale = document.documentElement.lang || "en";
const { messages } = await import(`./i18n/${locale}.js`);
```

```js
async function loadChart() {
  try {
    const chart = await import("./chart.js");
    chart.render("#app");
  } catch (err) {
    console.error("Chart failed to load", err);
  }
}
```

**Full topic:** [Dynamic imports](./3_js_dynamic_import.md)

**Sources:** [MDN — import()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import) · [javascript.info/modules-dynamic-imports](https://javascript.info/modules-dynamic-imports) · [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)

</details>
