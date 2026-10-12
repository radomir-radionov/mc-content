# JavaScript — hub

Язык ECMAScript, объектная модель, среда выполнения браузера (DOM/BOM/события) и асинхронное программирование. Теория живет в темах Core, Browser и Async. Вопросы для собеседования находятся в [Интервью](./3_js_interview.md).

## Resources & links

<details>
  <summary>Primary references</summary>

- [Учебник по современному JavaScript (javascript.info)](https://learn.javascript.ru/)
- [MDN — JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [Спецификация ECMA-262](https://tc39.es/ecma262/)
- [Блог о движке V8](https://v8.dev/)
- [Шпаргалка по современному JS](https://github.com/mbeaudru/modern-js-cheatsheet)
- [33 концепции JavaScript](https://github.com/leonardomso/33-js-concepts)\* [Чистый код JavaScript] (https://github.com/ryanmcdermott/clean-code-javascript)
- [WTFJS](https://github.com/denysdovhan/wtfjs)
- [Шаблоны проектирования JavaScript](https://refactoring.guru/design-patterns/catalog)
- [Дока — ООП](https://doka.guide/tools/oop/)
- [Дока — как работает память](https://doka.guide/js/how-the-memory-works/)\* [Как работает JavaScript (ByteByteGo)](https://blog.bytebytego.com/p/ep106-how-does-javascript-work)

</details>

<details>
  <summary>Tools & practice</summary>

### Tools & practice

- [JSV9000 — Визуализатор цикла событий] (https://www.jsv9000.app/)
- [Лупа — цикл событий](http://latentflip.com/loupe/)
- [AST Explorer](https://astexplorer.net/)
- [Codewars](https://www.codewars.com/dashboard)
- [Лучший из JS-проектов](https://bestofjs.org/projects?sort=total&page=2)

</details>

<details>
  <summary>Related docs in this repo</summary>

### Related docs in this repo

- [HTML — браузер и рендеринг](1_html.md)
- [CSS](2_css.md)
- [Реакция](4_react.md)
- [TypeScript](5_ts.md)
- [Веб-API — HTTP, хранилище, безопасность](6_web_api.md)
- [Node.js](8_nodejs.md)
- [БД](9_database.md)
- [Центр интервью](0_interview.md) · [JS — Интервью](./3_js_interview.md)

</details>

---

## JavaScript — overview

Охватывает язык ECMAScript, объектную модель, среду выполнения браузера (DOM/BOM/события) и асинхронное программирование.

### Main concepts

| Площадь           | Темы                                                                                                                                                                                                                                                                                                                                                                     |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Core**          | [Hub](./3_js_core_hub.md) — [Язык](./3_js_core.md) · [Переменные](./3_js_variables.md) · [Типы данных](./3_js_data_types.md) · [Структуры данных](./3_js_data_structures.md) · [Операторы](./3_js_operators.md) · [Циклы](./3_js_loops.md) · [Массив](./3_js_array.md) · [Функции](./3_js_functions.md) · [Объекты](./3_js_objects.md) · [Классы](./3_js_classes.md)     |
| **Modules**       | [Modules](./3_js_modules.md), [export / import](./3_js_import_export.md), [dynamic `import()`](./3_js_dynamic_import.md) → [additional](./3_js_extra.md)                                                                                                                                                                                                                 |
| **Браузер**       | [Hub](./3_js_browser_hub.md) — [Документ](./3_js_browser.md), [События](./3_js_events.md), [Окна](./3_js_browser_windows.md), [Файлы](./3_js_browser_files.md), [Сеть](./3_js_browser_network.md), [Анимация](./3_js_browser_animation.md), [Web Components](./3_js_web_components.md), [Ещё API](./3_js_browser_extra.md) · [Web API](./6_web_api_core.md#uri-url--urn) |
| **Асинхронный**   | [Event loop](./3_js_async_event_loop.md), [callbacks](./3_js_async_callbacks.md), [promises](./3_js_async_promises.md), [`async/await`](./3_js_async_await.md), [APIs](./3_js_async_apis.md) · [hub](./3_js_async.md)                                                                                                                                                    |
| **Дополнительно** | `Intl`, RegExp, Сигналы (TC39), `eval` → [дополнительно](./3_js_extra.md)                                                                                                                                                                                                                                                                                                |

### Navigation

| Часть 1                                                                                                                | Часть 2                                                                                                                   |
| ---------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| [Core — язык](./3_js_core.md) · [переменные](./3_js_variables.md) · [типы данных](./3_js_data_types.md)                | [Core — структуры данных](./3_js_data_structures.md) · [операторы](./3_js_operators.md) · [циклы](./3_js_loops.md)        |
| [Core — массив](./3_js_array.md) · [функции](./3_js_functions.md) (вкл. [closures](./3_js_functions.md#closures))      | [Core — объекты](./3_js_objects.md) · [классы](./3_js_classes.md)                                                         |
| [Modules](./3_js_modules.md) · [export / import](./3_js_import_export.md) · [dynamic import](./3_js_dynamic_import.md) | [Браузер — hub](./3_js_browser_hub.md) · [Документ](./3_js_browser.md) · [События](./3_js_events.md)                      |
| [Браузер — Окна](./3_js_browser_windows.md) · [Файлы](./3_js_browser_files.md) · [Сеть](./3_js_browser_network.md)     | [Анимация](./3_js_browser_animation.md) · [Web Components](./3_js_web_components.md) · [Ещё API](./3_js_browser_extra.md) |
| [Async — event loop](./3_js_async_event_loop.md)                                                                       |                                                                                                                           |
| [Async — callbacks](./3_js_async_callbacks.md)                                                                         | [Async — promises](./3_js_async_promises.md)                                                                              |
| [Async — await](./3_js_async_await.md)                                                                                 | [Async — APIs](./3_js_async_apis.md)                                                                                      |
| [Дополнительно](./3_js_extra.md)                                                                                       | [Интервью — вопросы](./3_js_interview.md)                                                                                 |
| [Центр интервью](./0_interview.md)                                                                                     |                                                                                                                           |

### Topic template

Каждая основная тема в подфайлах следующая:

**Определение** → **Плюсы/минусы** → **Когда использовать** → **Пример** → **Примечания** → **Источник** (javascript.info).
