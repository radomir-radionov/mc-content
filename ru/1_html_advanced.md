# HTML — Advanced

Веб-компоненты, теневой DOM и новые API HTML.

[ ← Вернуться в центр](./1_html.md)

### Related docs

* [HTML — Структура](./1_html_structure.md) · [HTML — Семантика](./1_html_semantics.md) · [HTML — Формы](./1_html_forms.md) · [HTML — Медиа](./1_html_media.md) · [HTML — Рендеринг](./1_html_rendering.md) · [HTML — Производительность](./1_html_ Performance.md) · [HTML — Доступность](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Расширенный](./1_html_advanced.md) · [HTML — Интервью](./1_html_interview.md)

### Sections

- [Advanced topics](#advanced-topics)

---

## Advanced topics

<details>
  <summary>Web Components (all three specs)</summary>

### Web Components (all three specs)

**Definition:**

Веб-компоненты сочетают в себе пользовательские элементы, теневой DOM и шаблоны HTML для многократного использования инкапсулированного пользовательского интерфейса.

| Технология | Цель |
|------------|---------|
| Пользовательские элементы | Новые HTML-теги с жизненным циклом |
| Тень ДОМ | Инкапсулированные стили DOM + |
| `<template>` + `<slot>` | Инертная разметка клонирована в DOM |

**How it works:**

- Пользовательские элементы — новые имена тегов + жизненный цикл.
- Shadow DOM — инкапсулированное поддерево + стили.
- `<template>` + `<slot>` — декларативная композиция.

| Спецификация | Цель |
|------|--------|
| Пользовательские элементы | Новые теги |
| Тень ДОМ | Инкапсуляция |

**When to use:**

Примитивы системы проектирования · встраиваемые виджеты · компоненты, не зависящие от платформы.

#### Common pitfalls

- Shadow DOM неожиданно нарушает глобальные стили форм.
- Не проецирование контента с помощью слотов.
- Закрытые теневые корни, блокирующие законное тестирование.

**Examples:**

```html
<template id="card-tpl">
  <div class="card"><slot name="title"></slot><slot></slot></div>
</template>
<user-card>
  <template shadowrootmode="open">
    <style>:host { display: block; }</style>
    <slot></slot>
  </template>
  <span>Projected content</span>
</user-card>
```

**Thoughts:** JS API пользовательских элементов → [3_js_browser_extra.md](./3_js_browser_extra.md).

**Sources:** [MDN — Веб-компоненты](https://developer.mozilla.org/en-US/docs/Web/API/Web_comComponents)

</details>

<details>
  <summary>Popover API</summary>

### Popover API

**Definition:**

API Popover предоставляет встроенные наложения верхнего слоя с возможностью отключения света и управления фокусом через `popover` атрибут.

| Значение | Поведение |
|-------|----------|
| `popover="auto"` | Закрывается по внешнему щелчку |
| `popover="manual"` | Для закрытия требуется JS |

**How it works:**

- `popover` атрибут помещает элемент в верхний слой.
- `popovertarget` Кнопка включения переключает цель по идентификатору.
- `popover="auto"` свет-уволить; `manual` нужен JS близко.

| Значение | Поведение |
|-------|----------|
| `auto` | Внешний щелчок закрывается |
| `manual` | JS-контролируется |

**When to use:**

Меню · подсказки · обучение пользовательскому интерфейсу без войн по z-индексам.

#### Common pitfalls

- Вложенность поповеров без фокус-плана.
- Неправильная замена доступных шаблонов меню.
- Проверка поддержки браузера перед производством.

**Examples:**

```html
<button popovertarget="menu" popovertargetaction="toggle">Open</button>
<div id="menu" popover>
  <p>Popover content</p>
</div>
```

**Sources:** [MDN — Popover API](https://developer.mozilla.org/en-US/docs/Web/API/Popover_API)

</details>

<details>
  <summary>inert, hidden, contenteditable</summary>

### inert, hidden, contenteditable

**Definition:**

Эти глобальные атрибуты управляют видимостью, интерактивностью и поведением встроенного редактирования.

| Атрибут | Поведение |
|-----------|----------|
| `hidden` | Не отображается(`display: none`) |
| `inert` | Исключено из взаимодействия и внимания |
| `contenteditable="true"` | Пользователь может редактировать контент онлайн |

**How it works:**

- `hidden` — `display:none`, не отображается, не фокусируется.
- `inert` — виден, но удален от взаимодействия/фокуса (модальный фон).
- `contenteditable` — пользователь может редактировать внутренний HTML-код.

| Атрибут | Поведение |
|-----------|----------|
| `hidden` | Не отображается |
| `inert` | Нет взаимодействия |

**When to use:**

Модальный фон (`inert` на главной) · переключать разделы (`hidden`) · редактор форматированного текста.

#### Common pitfalls

- `aria-hidden` вместо `inert` для модальных братьев и сестер (утечка фокуса).
- `contenteditable` без санитарной обработки пасты.
- `hidden` элементы все еще находятся в порядке табуляции, если существуют фокусируемые дочерние элементы.

**Examples:**

```html
<div hidden>Not visible</div>
<main inert>Background while modal open</main>
<p contenteditable="true">Editable paragraph</p>
```

**Sources:** [MDN — инертный](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/inert)

</details>

<details>
  <summary>Progressive enhancement vs graceful degradation</summary>

### Progressive enhancement vs graceful degradation

**Definition:**

Прогрессивное улучшение начинается с работы с базовым HTML и слоями CSS/JS. Грациозная деградация создает полноценный опыт с резервными вариантами.

| Подход | Стратегия |
|----------|----------|
| Прогрессивное улучшение | HTML работает первым; JS улучшает |
| Изящная деградация | Полный опыт работы с резервными вариантами |

**How it works:**

- **PE:** базовый HTML-код работает; Улучшение JS/CSS.
- **GD:** создайте полный опыт; обеспечить запасные варианты.
- PE — это современный стандарт по умолчанию для устойчивых веб-приложений.

| Подход | Стратегия |
|----------|----------|
| ЧП | HTML прежде всего |
| ГД | Полный + резервный |

**When to use:**

Форма работает с POST · поиск работает без автозаполнения JS · `<noscript>` уведомление.

#### Common pitfalls

- Пустой `<body>` пока JS не загрузится (без PE).
- Основные действия только через обработчики кликов JS.
- Предполагается быстрый JS на всех устройствах.

**Examples:**

```html
<form action="/search" method="get">
  <label for="q">Search</label>
  <input id="q" name="q" type="search" />
  <button type="submit">Search</button>
</form>
<!-- JS can enhance with autocomplete, no-JS still works -->
```

**Sources:** [MDN — Прогрессивное улучшение](https://developer.mozilla.org/en-US/docs/Glossary/Progressive_Enhancement)

</details>

<details>
  <summary>Feature detection vs user-agent sniffing</summary>

### Feature detection vs user-agent sniffing

**Definition:**

Обнаружение функции проверяет, существует ли возможность. Анализ пользовательского агента анализирует строки браузера — хрупкий и обескураживающий.

| Техника | Надежность |
|-----------|-------------|
| Обнаружение функций | Предпочтительный |
| Обнюхивание пользовательского агента | Хрупкость — избегайте |

**How it works:**

- Обнаружение функций: `if ('showModal' in HTMLDialogElement.prototype)`.
- Обнюхивание UA: парсинг `navigator.userAgent` — хрупкий.
- `@supports` в CSS для функций стиля.

| Техника | Надежность |
|-----------|-------------|
| Обнаружение функций | Хорошо |
| UA нюхает | Бедные |

**When to use:**

Диалог Polyfill · прогрессивный `popover` · Резервный вариант CSS-сетки.

#### Common pitfalls

- Блокировка браузеров по строке версии.
- Анализ мобильного UA на наличие адаптивного макета (используйте CSS).
- Обнаружение Safari UA на наличие полифилов (ложные срабатывания).

**Examples:**

```html
<!-- Prefer @supports in CSS and feature checks in JS -->
<!-- Never gate on browser name/version strings -->
```

**Sources:** [MDN — Обнаружение функций](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Feature_detection)

</details>

<details>
  <summary>HTML validation</summary>

### HTML validation

**Definition:**

Валидация проверяет HTML на соответствие спецификации. Неверный HTML-код может отображаться, но вызывает проблемы с доступностью, SEO и обслуживанием.

| Тип | Чеки |
|------|--------|
| Синтаксис | Правильно сформированная разметка |
| Логический | Порядок заголовков, уникальные идентификаторы, замещающий текст |

**How it works:**

- Валидатор W3C проверяет соответствие спецификации HTML.
- Неверная разметка все равно может отображаться (восстановление ошибок), но вредит a11y/SEO.
- Логическая проверка: одна `h1`, уникальные идентификаторы, alt для изображений.

| Тип | Чеки |
|------|--------|
| Синтаксис | Правильно сформированные теги |
| Логика | Порядок заголовков, уникальные идентификаторы |

**When to use:**

CI HTML lint · предварительный аудит · HTML электронной почты (более строгие клиенты).

#### Common pitfalls

- Дублировать `id` значения (недействительные, разрывают метки).
- Незакрытые теги, зависящие от исправлений браузера.
- Пользовательские элементы без дефиса (недопустимое имя CE).

**Examples:**

```html
<!DOCTYPE html>
<html lang="en">
  <head><title>Valid page</title></head>
  <body><main><h1>Title</h1></main></body>
</html>
```

**Sources:** [Валидатор W3C](https://validator.w3.org/)

</details>

<details>
  <summary>Cross-browser compatibility</summary>

### Cross-browser compatibility

**Definition:**

Функции HTML зависят от поддержки браузера. Проверьте caniuse.com перед использованием новых API; используйте полифилы или прогрессивное улучшение.

| Инструмент | Цель |
|------|---------|
| caniuse.com | Матрица поддержки функций |
| Прогрессивное улучшение | Базовый HTML всегда работает |

**How it works:**

- Проверьте caniuse.com на наличие новых HTML API.
- Прогрессивное улучшение: базовый HTML всегда работает.
- Полифиллы для `dialog`, `popover` где это необходимо.

| Инструмент | Цель |
|------|--------|
| caniuse.com | Поддержка матрицы |
| ЧП | Базовый вариант без новых API |

**When to use:**

Родной `<dialog>` с запасным вариантом · `input type="date"` меняющийся пользовательский интерфейс.

#### Common pitfalls

- Особенности доставки без поддержки матрицы.
- Polyfill без функции обнаружения ворот.
- Тестируем только Chrome.

**Examples:**

```html
<dialog id="modal">
  <p>Native modal — check support, provide fallback.</p>
</dialog>
```

**Sources:** [caniuse.com](https://caniuse.com/)

</details>

<details>
  <summary>HTML5 as an open web platform</summary>

### HTML5 as an open web platform

**Definition:**

HTML5 является частью более широкой платформы: HTML + CSS + JavaScript + DOM API. Разметка обеспечивает структуру; поведение и представление взяты из JS и CSS.

| Слой | Роль |
|-------|------|
| HTML | Структура и семантика |
| CSS | Презентация → [2_css.md](./2_css.md) |
| Дж.С. | Поведение → [3_js_browser.md](./3_js_browser.md) |

**How it works:**

- HTML5 = HTML + связанные API (CSS, JS, DOM).
- Разметка обеспечивает структуру; поведение от JS; презентация из CSS.
- Уровень жизни в WHATWG — непрерывная эволюция.

| Слой | Роль |
|-------|------|
| HTML | Структура |
| CSS | Презентация |
| Дж.С. | Поведение |

**When to use:**

PWA (HTML+manifest+SW) · медиа-страницы · веб-компоненты.

#### Common pitfalls

- Считать HTML устаревшим в SPA.
- Встроенные стили/скрипты вместо разделения.
- Игнорирование семантического HTML в компонентах.

**Examples:**

```html
<!DOCTYPE html>
<html lang="en">
  <head><link rel="stylesheet" href="/app.css" /></head>
  <body><main id="app"></main><script src="/app.js" defer></script></body>
</html>
```

**Sources:** [WHATWG HTML Living Standard](https://html.spec.whatwg.org/)

</details>

<details>
  <summary>ES module import maps</summary>

### ES module import maps

**Definition:**

Карты импорта позволяют браузерам преобразовывать пустые спецификаторы модулей в URL-адреса без упаковщика.

| Ключ | Карты |
|-----|---------|
| `"lodash"` | `/vendor/lodash.esm.js` |
| `"components/"` | `/js/components/` |

**How it works:**

- `<script type="importmap">` сопоставляет голые спецификаторы с URL-адресами.
- Включает `import 'lodash'` без упаковщика в современных браузерах.
- Объект JSON: `imports` и необязательно `scopes`.

| Ключ | Карты |
|-----|------|
| `imports` | Пустой спецификатор → URL |
| `scopes` | Переопределения, специфичные для пути |

**When to use:**

Прототипы без бандлеров · Простой импорт CDN · Сопоставления с ограниченной областью действия.

#### Common pitfalls

- Импортировать карту после скриптов модуля (должна быть первой).
- Отсутствует CORS в сопоставленных URL-адресах CDN.
- Производство без резервного варианта для неподдерживаемых браузеров.

**Examples:**

```html
<script type="importmap">
{
  "imports": {
    "lodash": "/vendor/lodash.esm.js",
    "components/": "/js/components/"
  }
}
</script>
<script type="module" src="/app.mjs"></script>
```

**Sources:** [MDN — импорт карт](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script/type/importmap)

</details>

<details>
  <summary>Web App Manifest (PWA)</summary>

### Web App Manifest (PWA)

**Definition:**

Манифест веб-приложения определяет устанавливаемые метаданные PWA — имя, значки, `start_url`, `display`и цвета темы.

| Поле | Цель |
|-------|---------|
| `name` | Отображаемое имя приложения |
| `icons` | Значки главного экрана |
| `display: standalone` | Chrome, похожий на приложение |

**How it works:**

- `<link rel="manifest" href="/manifest.webmanifest">` — JSON для метаданных установки.
- Поля: `name`, `icons`, `start_url`, `display`, `theme_color`.
- Работает с Service Worker в автономном режиме; только манифест = только устанавливаемые метаданные.

| Поле | Цель |
|-------|--------|
| `icons` | Установить иконки |
| `display` | `standalone` vs `browser` |

**When to use:**

Добавить на главный экран · автономный дисплей · тематическую заставку.

#### Common pitfalls

- Манифест не отправлен с правильным типом MIME.
- Отсутствуют значки 192/512.
- `start_url` не кэшируется.

**Examples:**

```html
<link rel="manifest" href="/manifest.webmanifest" />
<meta name="theme-color" content="#1a1a2e" />
```

**Sources:** [MDN — манифест веб-приложения](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)

</details>

<details>
  <summary>Obsolete elements — do not use</summary>

### Obsolete elements — do not use

**Definition:**

Устаревшие элементы по-прежнему анализируются в браузерах, но их не следует использовать — замените их CSS или современными альтернативами.

| Устарело | Замена |
|----------|-------------|
| `<font>`, `<center>` | CSS типографика/выравнивание |
| `<marquee>` | CSS-анимация |
| `<frame>`, `<frameset>` | Никогда не используйте |
| `<acronym>` | `<abbr>` |

**How it works:**

- Устаревшие презентационные теги удалены или устарели: `font`, `center`, `marquee`, `frame`/`frameset`.
- Используйте CSS для презентации; семантические элементы структуры.
- Браузеры по-прежнему могут отображать, но недействительно и вредно для a11y.

| Устарело | Замена |
|----------|-------------|
| `<font>` | CSS `color`/`font-family` |
| `<center>` | CSS `text-align` |

**When to use:**

Миграция устаревших сайтов · очистка старых результатов CMS.

#### Common pitfalls

- Копирование примеров эпохи W3Schools.
- `u` для несобственных имен (предпочитайте CSS).
- Теги макета таблицы (`width` на столе).

**Examples:**

```html
<p class="centered">Use CSS instead of obsolete tags.</p>
```

**Thoughts:** Замены CSS → [2_css.md](./2_css.md).

**Sources:** [MDN — устаревшие функции](https://developer.mozilla.org/en-US/docs/Web/HTML/Element#obsolete_and_deprecated_elements)

</details>

<details>
  <summary>HTML security basics (XSS)</summary>

### HTML security basics (XSS)

**Definition:**

HTML — это первая линия защиты от XSS. Избегайте пользовательского ввода, избегайте встроенных обработчиков и используйте CSP и SRI.

| Риск | Безопасная практика |
|------|---------------|
| Пользовательский ввод в HTML | Побег; использовать `textContent` нет `innerHTML` |
| Встроенные обработчики | Избегать `onclick`; использовать `addEventListener` |
| Сторонние скрипты | НИИ `integrity` атрибут |
| `target="_blank"` | Добавлять `rel="noopener noreferrer"` |

**How it works:**

- Никогда не вставляйте ненадежные данные в HTML без экранирования.
- Избегайте встроенных обработчиков событий (`onclick`); используйте ЦСП.
- НИИ `integrity` на сторонних скриптах; `rel="noopener"` on `_blank`.

| Риск | смягчение последствий |
|------|------------|
| `innerHTML` инъекции | `textContent` / дезинфицировать |
| Встроенные обработчики | CSP + addEventListener |

**When to use:**

Пользовательские комментарии · Контент CMS · Сторонние виджеты.

#### Common pitfalls

- `innerHTML` с пользовательским вводом.
- `target="_blank"` без `noopener`.
- Скрипты CDN без SRI.

**Examples:**

```html
<a href="https://external.com" target="_blank" rel="noopener noreferrer">External</a>
<script src="https://cdn.example.com/lib.js" integrity="sha384-..." crossorigin="anonymous"></script>
```

**Thoughts:** Шаблоны DOM XSS → [3_js_browser.md](./3_js_browser.md). Сохраненное или отраженное, CSP, кликджекинг → [7_special.md](./7_special_security.md#14-common-web-attacks) · [6_web_api.md](./6_web_api_security.md#security).

**Sources:** [MDN — XSS](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting)

</details>

---

<a name="attributes-reference"></a>
