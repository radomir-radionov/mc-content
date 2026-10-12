# JS — Dynamic imports

Загрузка модулей по требованию через `import()` — условные пути, code splitting и ленивые маршруты.

[← Modules hub](./3_js_modules_hub.md) · [JS hub](./3_js.md)

### Related docs

- [Modules hub](./3_js_modules_hub.md) · [Modules](./3_js_modules.md) · [Export and Import](./3_js_import_export.md) · [Async / Await](./3_js_async_await.md) · [Language](./3_js_core.md) · [JS Interview](./3_js_interview.md)

### Main concepts

| Area        | Topics                                             |
| ----------- | -------------------------------------------------- |
| **Syntax**  | `import(specifier)`, promise namespace, `.default` |
| **Runtime** | Условная загрузка, classic scripts, чанки бандлера |

### Sections

- [Dynamic imports](#dynamic-imports)
- [Interview](./3_js_interview.md)

Соседние темы Modules: [Modules](./3_js_modules.md) · [Export and Import](./3_js_import_export.md).

<a name="dynamic-imports"></a>

## Dynamic imports

<details>
  <summary>Dynamic imports</summary>

### Dynamic imports

**Definition:** **`import(specifier)`** — выражение в стиле функции: загружает модуль в момент вызова и возвращает **Promise** пространства имён. В отличие от статического `import`, допустимо внутри функций, веток и классических (не module) скриптов.

**How it works:**

- **Результат:** `const ns = await import("./heavy.js")` — named exports — свойства `ns`; default — `ns.default`.
- **Время:** fetch и оценка начинаются при вызове; модуль не входит в начальный статический граф до этого.
- **Specifier:** часто относительный путь `./chart.js`; может вычисляться (`import(\`./locales/${lang}.js\`)`). В браузере bare names (`import("lodash")`) без **import map** дают ошибку; Node резолвит из `node_modules`.
- **Бандлеры:** часто выносят цель dynamic `import()` в отдельный **chunk** (code splitting).
- **Ошибки:** сбой загрузки или выполнения — rejected promise; обрабатывайте `try` / `catch` или `.catch()`.
- **Со static import:** статический `import` поднимается и грузится с графом; `import()` — для маршрутов, локалей, редакторов и тяжёлого кода не на первом экране.

**When to use:**

| Static `import`           | Dynamic `import()`                      |
| ------------------------- | --------------------------------------- |
| Всегда нужные зависимости | Код по маршруту или feature flag        |
| Верх файла модуля         | В обработчиках событий или `async` init |
| Граф на этапе разбора     | После действия пользователя или idle    |

**Pluses and minuses:**

| +                              | −                                            |
| ------------------------------ | -------------------------------------------- |
| Меньший начальный бандл        | Дополнительный сетевой запрос при первом use |
| Работает в non-module скриптах | Сложнее статически проследить зависимости    |
| Удобно с `async` / `await`     | Переиспользование может дробить загрузку     |

**Examples:**

```js
button.addEventListener("click", async () => {
  const { openEditor } = await import("./editor.js");
  openEditor();
});
```

```js
const locale = "en";
const messages = await import(`./i18n/${locale}.js`);
```

```js
// Classic script (not type="module")
(async () => {
  const lodash = await import("https://cdn.example.com/lodash-es.js");
  lodash.default.chunk([1, 2, 3], 2);
})();
```

**Sources:** [MDN — import()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import) · [javascript.info/modules-dynamic-imports](https://javascript.info/modules-dynamic-imports) · [MDN — JavaScript modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)

</details>
