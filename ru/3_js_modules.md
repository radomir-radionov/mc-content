# JS — Modules

Как ECMAScript делит код на файлы с собственной областью видимости — module-скрипты в браузере и ESM в Node.

[← Modules hub](./3_js_modules_hub.md) · [JS hub](./3_js.md)

### Related docs

- [Export and Import](./3_js_import_export.md) · [Dynamic imports](./3_js_dynamic_import.md) · [Language](./3_js_core.md) · [Async / Await](./3_js_async_await.md#top-level-await) · [Node.js — Modules](./8_nodejs_modules.md) · [TypeScript — Modules](./5_ts_modules.md) · [JS Interview](./3_js_interview.md)

### Main concepts

| Area              | Topics                                                    |
| ----------------- | --------------------------------------------------------- |
| **Module system** | Область модуля, script vs module, оценка, ESM vs CommonJS |
| **Runtime**       | `<script type="module">`, strict mode, CORS, бандлеры     |

### Sections

- [Modules](#modules)
- [Interview](./3_js_interview.md)

Соседние темы Modules: [Export and Import](./3_js_import_export.md) · [Dynamic imports](./3_js_dynamic_import.md).

<a name="modules"></a>

## Modules

<details>
  <summary>Modules</summary>

### Modules

**Definition:** **Модуль** — файл (или скрипт) с **областью модуля**: привязки верхнего уровня не становятся глобальными, пока их явно не экспортируют. Современный JavaScript использует **ES modules (ESM)** со статическими `import` / `export`. В Node и старых проектах ещё встречается **CommonJS** (`require` / `module.exports`).

**How it works:**

- **Область модуля:** `let`, `const`, `function`, `class` на верхнем уровне приватны для файла, если не экспортированы.
- **Strict mode:** код модуля автоматически в strict mode (директива `"use strict"` не нужна).
- **Script vs module:** классический `<script>` делит глобальный объект; `<script type="module">` отложенный, в области модуля, с `import` / `export`.
- **Оценка:** тело модуля выполняется **один раз** при первой загрузке; повторные импорты используют те же привязки (live exports — [Export and Import](./3_js_import_export.md#export-and-import)).
- **Граф:** статические `import` задают граф зависимостей на этапе разбора; загрузчик подтягивает и выполняет в порядке зависимостей.
- **Браузер:** cross-origin module-скрипты требуют CORS; bare specifiers вроде `"lodash"` — import map или бандлер.
- **Node:** `.mjs`, `"type": "module"` в `package.json` или настроенный ESM в `.js`; `require` — CommonJS.
- **Import maps:** в HTML `<script type="importmap">` сопоставляет bare specifiers с URL для нативного ESM в браузере без бандлера ([MDN — import maps](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/script/type/importmap)).

**When to use:**

| ESM (модули)                       | CommonJS (`require`)               |
| ---------------------------------- | ---------------------------------- |
| Новые браузерные и Node-приложения | Legacy только на Node              |
| Собранный фронтенд                 | Часть инструментов только CJS      |
| Статический граф для tree-shaking  | Синхронная загрузка конфига в Node |

Для условной или ленивой загрузки — [Dynamic imports](./3_js_dynamic_import.md).

**Pluses and minuses:**

| +                        | −                                         |
| ------------------------ | ----------------------------------------- |
| Явный граф зависимостей  | CORS для cross-origin скриптов в браузере |
| Нет случайных глобалов   | Нет синхронного `require` в браузере      |
| Tree-shaking с бандлером | Разная семантика ESM/CJS в Node           |

**Examples:**

```html
<script type="module">
  import { init } from "./app.js";
  init();
</script>
```

```js
// app.js — привязки верхнего уровня локальны для модуля
const secret = 42;
export function init() {
  console.log(secret);
}
```

**Sources:** [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) · [javascript.info/modules-intro](https://javascript.info/modules-intro)

</details>
