# JS — Export and Import

Статические `export` и `import`, реэкспорт и live bindings между модулями.

[← Modules hub](./3_js_modules_hub.md) · [JS hub](./3_js.md)

### Related docs

- [Modules hub](./3_js_modules_hub.md) · [Modules](./3_js_modules.md) · [Dynamic imports](./3_js_dynamic_import.md) · [Language](./3_js_core.md) · [Node.js — Modules](./8_nodejs_modules.md) · [JS Interview](./3_js_interview.md)

### Main concepts

| Area          | Topics                                                            |
| ------------- | ----------------------------------------------------------------- |
| **Export**    | Named, default, re-export, `export from`                          |
| **Import**    | Named, default, namespace, side-effect imports                    |
| **Semantics** | Live bindings, hoisting, static `import` только на верхнем уровне |

### Sections

- [Export and Import](#export-and-import)
- [Interview](./3_js_interview.md)

Соседние темы Modules: [Modules](./3_js_modules.md) · [Dynamic imports](./3_js_dynamic_import.md).

<a name="export-and-import"></a>

## Export and Import

<details>
  <summary>Export and Import</summary>

### Export and Import

**Definition:** **`export`** помечает привязки для других модулей; **`import`** читает их из URL или пути модуля. Статические формы разрешаются при разборе и должны быть на верхнем уровне модуля (не внутри `if`).

**How it works:**

- **Named export:** `export const x = 1;` или `export { x };` → `import { x } from "./mod.js"`.
- **Default export:** не больше одного на файл → `export default fn;` → `import fn from "./mod.js"` (любой идентификатор, без `{ }`).
- **Смешанный импорт:** `import multiply, { PI, add } from "./math.js"` — сначала default, затем named в `{ }`.
- **Re-export:** `export { x } from "./mod.js"` или `export { default as User } from "./user.js"` — единая точка входа API.
- **Namespace import:** `import * as math from "./math.js"` — объект пространства имён (включая `default`, если есть).
- **Side-effect import:** `import "./polyfills.js"` — только выполнение модуля для инициализации.
- **Live bindings:** импортированные имена отслеживают экспортёр; присваивать импортам нельзя (только чтение).
- **Hoisting:** статический `import` выполняется до остального тела модуля.
- **`import.meta`:** `import.meta.url` — URL этого модуля (ESM в браузере и Node).
- **Условная загрузка:** dynamic `import()` — [Dynamic imports](./3_js_dynamic_import.md).

#### Common pitfalls

```js
// Static import cannot be inside if — use import() instead
// if (flag) import { x } from "./mod.js";

// import { x } from "./mod.js"; // x is a live binding, not a copy of a snapshot
```

**When to use:**

| Паттерн            | Когда                                     |
| ------------------ | ----------------------------------------- |
| Named exports      | Несколько публичных API в одном файле     |
| Default export     | Один главный экспорт (компонент, фабрика) |
| Barrel re-export   | Один `index.js` для папки пакета          |
| Side-effect import | Плагины, полифиллы, регистрация           |

**Pluses and minuses:**

| +                                  | −                                   |
| ---------------------------------- | ----------------------------------- |
| Явная публичная поверхность        | Споры default vs named              |
| Статический анализ                 | Ветвления — только через `import()` |
| Live bindings без устаревших копий | Импорты нельзя переприсвоить        |

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

**Sources:** [MDN — export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) · [MDN — import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) · [MDN — import.meta](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import.meta) · [javascript.info/import-export](https://javascript.info/import-export)

</details>
