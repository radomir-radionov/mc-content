# HTML — Structure

Скелет документа, метаданные, элементы и атрибуты.

[ ← Вернуться в центр](./1_html.md)

### Related docs

- [HTML — Структура](./1_html_structure.md) · [HTML — Семантика](./1_html_semantics.md) · [HTML — Формы](./1_html_forms.md) · [HTML — Медиа](./1_html_media.md) · [HTML — Рендеринг](./1_html_rendering.md) · [HTML — Производительность](./1*html* Performance.md) · [HTML — Доступность](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Расширенный](./1_html_advanced.md) · [HTML — Интервью](./1_html_interview.md)

### Sections

- [Document structure & metadata](#document-structure-metadata)
- [Elements reference](#elements-reference)
- [Attributes reference](#attributes-reference)

---

## Document structure & metadata

<details>
  <summary>Basic HTML document structure</summary>

### Basic HTML document structure

**Definition:**

Действительный документ HTML5 имеет DOCTYPE, корневой `<html>` с `lang`, a `<head>` для метаданных и `<body>` для видимого контента.

| Часть             | Роль                                        |
| ----------------- | ------------------------------------------- |
| `<!DOCTYPE html>` | Запускает стандартный режим                 |
| `<html lang>`     | Корневой элемент; язык для a11y/SEO         |
| `<head>`          | Метаданные, ссылки — не содержимое страницы |
| `<body>`          | Видимый контент                             |

**How it works:**

- `<!DOCTYPE html>` → стандартный режим.
- `<html lang>` устанавливает язык документа.
- `<head>`: кодировка, область просмотра, заголовок, связанные ресурсы.
- `<body>`: только видимое содержимое потока.

**When to use:**

Статический сайт · SPA-оболочка (`<div id="app">`) · Минимальная документация в стиле AMP.

#### Common pitfalls

- Отсутствующий `<meta charset>` (ошибки кодирования).
- Несколько `<main>` или нет.
- Размещение видимого контента в `<head>`.

**Pluses and minuses:**

| +                                           | −                                                        |
| ------------------------------------------- | -------------------------------------------------------- |
| Действительные скелетные помощники a11y/SEO | Шаблонный шаблон, который легко пропустить во фрагментах |
| `lang` помогает программам чтения с экрана  | Неправильный `lang` произношение больно                  |

**Examples:**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Page title</title>
    <link rel="stylesheet" href="/styles.css" />
  </head>
  <body></body>
</html>
```

**Sources:** [MDN — Начало работы](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Getting_started)

</details>

<details>
  <summary>DOCTYPE</summary>

### DOCTYPE

**Definition:**

- `<!DOCTYPE html>` Объявление сообщает браузеру использовать режим стандартов HTML5. Без него браузеры могут перейти в режим совместимости с непоследовательным поведением макета.

| Режим     | Триггер                      | Эффект                                    |
| --------- | ---------------------------- | ----------------------------------------- |
| Стандарты | `<!DOCTYPE html>`            | Компоновка, соответствующая спецификациям |
| Причуды   | Отсутствует/неверный DOCTYPE | Устаревшая эмуляция IE                    |

**How it works:**

- `<!DOCTYPE html>` — это тип документа HTML5, а не элемент.
- Запускает **стандартный режим** (без особенностей).
- Должна быть первая линия; никаких атрибутов.

| Режим     | Триггер                      | Эффект                                    |
| --------- | ---------------------------- | ----------------------------------------- |
| Стандарты | `<!DOCTYPE html>`            | Компоновка, соответствующая спецификациям |
| Причуды   | Отсутствует/неверный DOCTYPE | Ошибки устаревшей коробочной модели       |

**When to use:**

Новые страницы HTML5 · На устаревших страницах отсутствует DOCTYPE (особенности).

#### Common pitfalls

- Прописные буквы `<!DOCTYPE HTML>` все еще работает, но непоследовательный стиль.
- XML-стиль `<!DOCTYPE html />` это неправильно.
- Документы XHTML на страницах HTML5.

**Examples:**

```html
<!DOCTYPE html>
<html lang="en">
  <body></body>
</html>
```

**Sources:** [MDN — DOCTYPE](https://developer.mozilla.org/en-US/docs/Glossary/Doctype)

</details>

<details>
  <summary>HTML vs XML</summary>

### HTML vs XML

**Definition:** HTML – это **фиксированный** набор тегов для страниц в браузере. XML — это тот же _вид_ разметки, но **вы даете имена тегам** — это синтаксис данных/документов, а не язык пользовательского интерфейса.

**How it works:** XML является строгим (один корень, закрытые теги, атрибуты в кавычках). Парсеры HTML5 восстанавливаются после ошибок. `DOCTYPE` — переключатель режима HTML; XML необязательно `<?xml …?>` это метакодировка/версия. Полный синтаксис, пространства имен, SVG `xmlns`: [6_web_api.md — XML](./6_web_api_rest.md#xml).

**When to use:** Интервью «Чем XML не HTML?» · почему `.svg` потребности в файлах `xmlns` но встроенный SVG в HTML часто этого не делает.

**Thoughts:** HTML-дисплеи; XML (и JSON) переносят данные.

**Sources:** [MDN — введение в XML](https://developer.mozilla.org/en-US/docs/Web/XML/Guides/XML_introduction)

</details>

<details>
  <summary>Meta tags</summary>

### Meta tags

**Definition:**

Метаэлементы живут в `<head>` и предоставить метаданные документа — кодировку, область просмотра, описание и директивы robots.

| Мета          | Цель                                               |
| ------------- | -------------------------------------------------- |
| `charset`     | Кодировка символов                                 |
| `viewport`    | Адаптивное масштабирование для мобильных устройств |
| `description` | Текст фрагмента поиска                             |
| `robots`      | Директивы сканирования/индексирования              |

**How it works:**

- `<meta>` пустые элементы в `<head>` для метаданных документа.
- `charset` должно быть ранним (в идеале первые 1024 байта).
- `viewport` требуется для мобильной адаптивной верстки.
- `description`/`robots` влияют на фрагменты поиска и сканирование.

| Мета          | Цель                      |
| ------------- | ------------------------- |
| `charset`     | Кодировка символов        |
| `viewport`    | Мобильное масштабирование |
| `description` | Фрагмент поиска           |
| `robots`      | Директивы сканирования    |

**When to use:**

Публичные страницы (мета SEO) · `noindex` постановка · `theme-color` ПВА.

#### Common pitfalls

- Дублировать `viewport` теги.
- `keywords` мета (игнорируется основными движками).
- `refresh` перенаправление (плохо для a11y; используйте перенаправление HTTP).

**Examples:**

```html
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<meta name="description" content="Short page summary for search results" />
<meta name="robots" content="index, follow" />
```

**Sources:** [Google — специальные теги](https://developers.google.com/search/docs/crawling-indexing/special-tags)

</details>

<details>
  <summary>link rel types & resource hints</summary>

### link rel types & resource hints

**Definition:** - `<link>` Элемент соединяет документ с внешними ресурсами — таблицами стилей, значками, каноническими URL-адресами и рекомендациями по производительности.

**How it works:**

- `<link rel="stylesheet">` — CSS, блокирующий рендеринг на критическом пути.
- `canonical` — предпочтительный URL-адрес для дублированного контента (SEO).
- `preconnect` — ранний DNS+TCP+TLS; `preload` — высокоприоритетная выборка текущей страницы; `prefetch` — следующая навигация с низким приоритетом.

| `rel` ценность | Цель                                              |
| -------------- | ------------------------------------------------- |
| `stylesheet`   | CSS                                               |
| `canonical`    | Предпочтительный URL для SEO                      |
| `preconnect`   | Раннее подключение (DNS+TCP+TLS)                  |
| `preload`      | Высокоприоритетная загрузка текущей страницы      |
| `prefetch`     | Низкоприоритетная выборка для следующей навигации |

**When to use:**

Критическая предварительная загрузка шрифта · Предварительное подключение CDN · Предварительная загрузка следующей страницы.

| `rel`        | Когда                   |
| ------------ | ----------------------- |
| `stylesheet` | CSS приложения          |
| `preconnect` | Стороннее происхождение |
| `preload`    | Изображение/шрифт LCP   |

#### Common pitfalls

- Предварительная загрузка всего (растраченная пропускная способность).
- `preload` без соответствия `as` тип.
- `preconnect` к истокам никогда не использовался.

**Examples:**

```html
<link rel="stylesheet" href="/app.css" />
<link rel="canonical" href="https://example.com/page" />
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link
  rel="preload"
  href="/hero.woff2"
  as="font"
  type="font/woff2"
  crossorigin
/>
<script
  src="https://cdn.example.com/lib.js"
  integrity="sha384-..."
  crossorigin="anonymous"
></script>
```

**Thoughts:** Почему `preconnect` существует (TCP+TLS RTT) → [TCP против UDP](./6_web_api_interview.md#tcp-vs-udp).

**Sources:** [MDN — типы ссылок](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel)

</details>

<details>
  <summary>Favicon & app icons (link rel icon)</summary>

### Favicon & app icons (link rel icon)

**Definition:** `<link rel="icon">` и связанные с этим `rel` значения объявляют значки значков и значки на главном экране, которые браузеры и операционные системы используют для вкладок, закладок и пользовательского интерфейса установки PWA.

**How it works:**

- Браузеры загружают `rel="icon"` от `<head>` (отступать: `/favicon.ico` в корне сайта).
- Предоставить размеры PNG/SVG; `apple-touch-icon` для iOS; манифестировать `icons` для запросов на установку PWA.
- Масштаб значков SVG; PNG требует нескольких размеров для сетчатки.

| `rel`              | Цель                        |
| ------------------ | --------------------------- |
| `icon`             | Стандартный значок          |
| `apple-touch-icon` | Главный экран iOS           |
| `mask-icon`        | Закрепленная вкладка Safari |

**When to use:**

Брендинг вкладок браузера · Установка PWA · темный/светлый значок через `media` атрибут включен `<link>`.

#### Common pitfalls

- 404 on `/favicon.ico` когда связан только PNG.
- Один значок размером 16×16 размыт на сетчатке.
- Манифест `icons` отсутствует разрешение 192×512 для установки Android.

**Pluses and minuses:**

| +                                                | −                                |
| ------------------------------------------------ | -------------------------------- |
| Профессиональная полировка во вкладках/закладках | Многие размеры для поддержания   |
| SVG масштабируется в один файл                   | Старый IE игнорировал значки SVG |

**Examples:**

```html
<link rel="icon" href="/favicon.svg" type="image/svg+xml" />
<link rel="icon" href="/favicon-32.png" sizes="32x32" type="image/png" />
<link rel="apple-touch-icon" href="/apple-touch-icon.png" sizes="180x180" />
```

**Sources:** [MDN — значок ссылки](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel#icon)

</details>

<details>
  <summary>modulepreload & dns-prefetch resource hints</summary>

### modulepreload & dns-prefetch resource hints

**Definition:** Подсказки по ресурсам сообщают браузеру, что нужно заранее разрешить DNS или получить модули, сокращая задержку на критически важных ресурсах перекрестного происхождения или ESM.

**How it works:**

- `dns-prefetch` — Только поиск DNS (дешевая подсказка для сторонних доменов).
- `preconnect` — DNS + TCP + TLS (используйте для шрифтов/CDN, которые вы обязательно будете использовать).
- `modulepreload` — ранняя выборка + разбор ES-модуля; использовать для входа `.mjs` файлы.

| Подсказка       | Стоимость          | Используйте, когда         |
| --------------- | ------------------ | -------------------------- |
| `dns-prefetch`  | только DNS         | Многие сторонние источники |
| `preconnect`    | Полное подключение | Критический CDN/шрифты     |
| `modulepreload` | Модуль выборки     | точка входа в ESM          |

**When to use:**

DNS-prefetch домена Analytics · Предварительное подключение Google Fonts · `modulepreload` для `app.mjs`.

#### Common pitfalls

- `preconnect` к источникам, никогда не используемым на странице.
- `modulepreload` без соответствия `crossorigin` на модулях CORS.
- Замена всего `preconnect` с `dns-prefetch` когда необходим полный прогрев соединения.

**Pluses and minuses:**

| +                             | −                                                          |
| ----------------------------- | ---------------------------------------------------------- |
| Сокращает задержку соединения | Чрезмерные намеки приводят к потере пропускной способности |
| Ускоренный запуск ESM         | `modulepreload` потребности `crossorigin` если КОРС        |

**Examples:**

```html
<link rel="dns-prefetch" href="https://analytics.example.com" />
<link rel="preconnect" href="https://fonts.googleapis.com" crossorigin />
<link rel="modulepreload" href="/app.mjs" crossorigin />
```

**Sources:** [MDN — dns-prefetch](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel/dns-prefetch) · [modulepreload](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel/modulepreload)

</details>

<details>
  <summary>base element</summary>

### base element

**Definition:**

- `<base>` Элемент устанавливает URL-адрес и цель по умолчанию для всех относительных ссылок на странице. Используйте с осторожностью — по одному на документ, должно быть в `<head>`.

| Атрибут  | Эффект                               |
| -------- | ------------------------------------ |
| `href`   | Базовый URL для относительных ссылок |
| `target` | По умолчанию `target` для ссылок     |

**How it works:**

- Одинокий `<base href>` in `<head>` перезаписывает относительные URL-адреса для `a`, `link`, `img`, `form`и т. д.
- Необязательно `target` устанавливает цель ссылки по умолчанию.
- Влияет на весь документ — используйте с осторожностью.

**When to use:**

HTML-шаблоны для электронной почты · встроенные документы в iframe · зеркальные сайты.

#### Common pitfalls

- Несколько `<base>` элементы (недействительно).
- `<base>` после того, как относительные URL-адреса уже проанализированы.
- `target="_blank"` по базе, затрагивающую все ссылки.

**Pluses and minuses:**

| +                                     | −                                                                            |
| ------------------------------------- | ---------------------------------------------------------------------------- |
| Переносимые относительные пути        | Разрыв ссылок, содержащих только фрагменты, если они установлены неправильно |
| Одно место для изменения базового URL | Трудно отладить неожиданные решения                                          |

**Examples:**

```html
<base href="https://example.com/assets/" target="_blank" />
```

**Sources:** [MDN — база](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/base)

</details>

<details>
  <summary>Absolute vs relative paths</summary>

### Absolute vs relative paths

**Definition:**

URL-адреса в HTML могут быть относительными к текущему документу или абсолютными. Относительные пути переносимы; Для внешних ресурсов требуются абсолютные URL-адреса.

| Тип                  | Пример                           | Вариант использования |
| -------------------- | -------------------------------- | --------------------- |
| Родственник          | `/images/logo.png`               | Тот же сайт           |
| Родственный родитель | `../css/app.css`                 | Родственные каталоги  |
| Абсолютный URL       | `https://cdn.example.com/app.js` | Внешний CDN           |

**How it works:**

- **Относительный** — разрешается по текущему URL-адресу или `<base>`.
- **Относительно корня** (`/assets/x`) — из корня сайта.
- **Абсолютно** (`https://...`) — полный URL, необходимый для перекрестного происхождения.

| Тип                | Пример                | Использование  |
| ------------------ | --------------------- | -------------- |
| Корень-родственник | `/img/logo.png`       | Тот же сайт    |
| Родственник        | `../css/app.css`      | Вложенные пути |
| Абсолют            | `https://cdn.../x.js` | Внешний CDN    |

**When to use:**

Активы одного репо (`/css/app.css`) · родной реж. (`../img/x`) · CDN (`https://cdn...`).

#### Common pitfalls

- Неработающие пути при перемещении файлов без обновления. `../`.
- относительно протокола `//cdn...` (устаревший шаблон).
- Относительные пути в электронных письмах (ненадежно).

**Examples:**

```html
<img src="/images/logo.png" alt="Logo" />
<link rel="stylesheet" href="../css/app.css" />
<script src="https://cdn.example.com/lib.js"></script>
```

**Sources:** [MDN — Ссылки](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Creating_hyperlinks)

</details>

<details>
  <summary>Comments & HTML entities</summary>

### Comments & HTML entities

**Definition:**

Комментарии HTML не отображаются. Сущности представляют собой специальные символы, которые в противном случае нарушили бы разметку или отсутствовали бы на клавиатуре.

| Сущность        | Персонаж                 |
| --------------- | ------------------------ |
| `&amp;`         | `&`                      |
| `&lt;` / `&gt;` | `<` / `>`                |
| `&copy;`        | ©                        |
| `&nbsp;`        | неразрывное пространство |

**How it works:**

- `<!-- comment -->` не отображается; не может гнездиться.
- Объекты кодируют зарезервированные/специальные символы: `&lt;`, `&amp;`, `&copy;`.
- Числовые сущности `&#169;` / `&#xA9;` для любой кодовой точки Unicode.

| Сущность        | Персонаж                 |
| --------------- | ------------------------ |
| `&amp;`         | `&`                      |
| `&lt;` / `&gt;` | `<` / `>`                |
| `&nbsp;`        | неразрывное пространство |

**When to use:**

Примечания к шаблону в исходнике · отображение `<` в примерах кода · символы авторского права.

#### Common pitfalls

- Неизбежный `&` в URL-адресах/атрибутах.
- `&nbsp;` для интервала макета (используйте CSS).
- Комментарии с `--` внутри (недействительно).

**Examples:**

```html
<!-- This is a comment; not rendered -->
<p>&copy; 2025 &mdash; Price: &euro;10 &lt; &euro;20</p>
```

**Sources:** [MDN — Сущности](https://developer.mozilla.org/en-US/docs/Glossary/Entity)

</details>

---

<a name="elements-reference"></a>

### HTML structure

**Definition:** HTML (язык гипертекстовой разметки) — это стандартный язык разметки веб-страниц. HTML-документ — это текстовый файл, который браузер анализирует в виде дерева элементов, передающих структуру, значение и поведение.

**How it works:**

- Исходный файл содержит теги, атрибуты и текстовые узлы.
- Парсер браузера создает живое дерево DOM из разметки.
- CSS и JS присоединяются к этому дереву для представления и поведения.

| Срок        | Роль                                                                       |
| ----------- | -------------------------------------------------------------------------- |
| **Тег**     | Синтаксис разметки (`<p>`, `</p>`) в исходном коде                         |
| **Элемент** | Узел Live DOM — открывающий тег + контент + закрывающий тег (или тег void) |
| **Атрибут** | Метаданные «ключ/значение» элемента (`href`, `class`, `alt`)               |

**When to use:**

Каждая веб-страница · Электронная почта в формате HTML · Вывод SSR/SSG · Шаблоны компонентов.

Каждая веб-страница начинается с HTML — это структурная основа CSS и JavaScript.

#### Common pitfalls

- Путаница тегов (синтаксиса) с элементами (узлами DOM).
- Опуская `lang` on `<html>`.
- Пустые элементы, записанные с закрывающими тегами (недопустимо для некоторых).

**Pluses and minuses:**

| +                                        | −                                                  |
| ---------------------------------------- | -------------------------------------------------- |
| Универсальный веб-фундамент              | Не только по Тьюрингу                              |
| Семантические элементы помогают a11y/SEO | Неверная разметка зависит от восстановления ошибок |

**Examples:**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>My page</title>
  </head>
  <body>
    <p class="intro">Hello, world.</p>
  </body>
</html>
```

**Thoughts:** Пустотные элементы (`<img>`, `<br>`, `<input>`, `<meta>`, `<link>`) не имеют закрывающего тега. Подтвердите с помощью [validator.w3.org](https://validator.w3.org/).

**Sources:** [MDN — HTML](https://developer.mozilla.org/en-US/docs/Web/HTML) · [WHATWG HTML Living Standard](https://html.spec.whatwg.org/)

<a name="browser-rendering-pipeline"></a>

Браузерный рендеринг (анализ → макет → рисование → композит) находится в [HTML — Rendering](./1_html_rendering.md).

## Elements reference

<details>
  <summary>Block vs inline elements</summary>

### Block vs inline elements

**Definition:**

Элементы уровня блока складываются вертикально и принимают размер полной блочной модели. Встроенные элементы следуют за текстом и обычно содержат только фразовый контент.

| Тип         | Поведение             | Примеры                        |
| ----------- | --------------------- | ------------------------------ |
| Блокировать | Полная ширина, стопки | `div`, `p`, `section`, `table` |
| Встроенный  | Потоки с текстом      | `span`, `a`, `strong`, `em`    |

**How it works:**

- **Блок** — начинает новую строку, принимает ось блока ширины/высоты/поля.
- **Inline** — перетекает в текстовую строку; ширина/высота игнорируется (в основном).
- Категории контента HTML5 уточняют правила вложения.

| Тип         | Поведение              | Примеры               |
| ----------- | ---------------------- | --------------------- |
| Блокировать | Складывает вертикально | `div`, `p`, `section` |
| Встроенный  | Потоки с текстом       | `span`, `a`, `em`     |

**When to use:**

Абзацы (`p`) · встроенное выделение (`span`, `a`) · `display` CSS может изменить представление.

#### Common pitfalls

- Блокировать элементы внутри `p` (автоматическое закрытие `p`).
- Интерактивная встроенная недопустимая вложенность.
- Предполагая `display` меняет категорию HTML.

**Examples:**

```html
<div><p>Block paragraph</p></div>
<p>Inline <span>wrapper</span> inside text.</p>
```

**Sources:** [MDN — элементы уровня блока](https://developer.mozilla.org/en-US/docs/Glossary/Block-level_content)

</details>

<details>
  <summary>Links (anchor element)</summary>

### Links (anchor element)

**Definition:**

- `<a>` элемент создает гиперссылки для навигации. Использовать `<button>` для действий на странице, а не для ссылок.

| Атрибут    | Цель                             |
| ---------- | -------------------------------- |
| `href`     | URL или фрагмент                 |
| `target`   | `_self`, `_blank`и т. д.         |
| `rel`      | `noopener noreferrer` с `_blank` |
| `download` | Предложить скачать файл          |

**How it works:**

- `<a href>` для навигации по URL-адресам или фрагментам.
- Абсолютный `https://…` vs относительный: нет хоста (браузер использует текущий источник/ `<base>`); `/path` от корня; `../` вверх на один сегмент; `mailto:` для электронной почты. Полная анатомия: [URI, URL и URN](./6_web_api_core.md#uri-url--urn).
- `target="_blank"` потребности `rel="noopener noreferrer"`.
- `download` предлагает загрузку файлов для URL-адресов одного и того же происхождения.

| Атрибут    | Цель                    |
| ---------- | ----------------------- |
| `href`     | Пункт назначения        |
| `rel`      | `noopener` для `_blank` |
| `download` | Сохранить как файл      |

**When to use:**

Внутренние маршруты · внешние документы · пропуск ссылок (`href="#main"`).

#### Common pitfalls

- `<a>` без `href` (не фокусируется на клавиатуре).
- `javascript:void(0)` hrefs.
- С использованием `<a>` для кнопок (используйте `<button>`).

**Examples:**

```html
<a href="/about" target="_blank" rel="noopener noreferrer">About</a>
<a href="mailto:hi@example.com">Email</a>
```

**Sources:** [MDN — a](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a) · [URI, URL и URN](./6_web_api_core.md#uri-url--urn)

</details>

<details>
  <summary>Tables</summary>

### Tables

**Definition:**

Таблицы представляют табличные данные с группами строк заголовка и тела. Использовать `scope` on `<th>` для доступности.

| Элемент               | Роль             |
| --------------------- | ---------------- |
| `<caption>`           | Название таблицы |
| `<thead>` / `<tbody>` | Группы строк     |
| `<th scope>`          | Ячейка заголовка |
| `<td>`                | Ячейка данных    |

**How it works:**

- Таблицы только для **табличных данных**, а не для макета.
- `<caption>`, `<thead>`, `<tbody>`, `<th scope>` структура помощи.
- `scope="col"` / `scope="row"` для программ чтения с экрана.

| Элемент      | Роль             |
| ------------ | ---------------- |
| `<caption>`  | Название таблицы |
| `<th scope>` | Ячейка заголовка |
| `<td>`       | Ячейка данных    |

**When to use:**

Финансовые отчеты · матрицы сравнения · таблицы данных (рассмотрим ARIA для комплекса).

#### Common pitfalls

- Таблицы макета (используйте CSS Grid/Flex).
- Отсутствуют заголовки в таблицах данных.
- `th` без `scope` в простых таблицах (еще лучше с областью действия).

**Examples:**

```html
<table>
  <caption>
    Q4 sales by region
  </caption>
  <thead>
    <tr>
      <th scope="col">Region</th>
      <th scope="col">Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">EMEA</th>
      <td>$1.2M</td>
    </tr>
  </tbody>
</table>
```

**Sources:** [MDN — таблица](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/table)

</details>

<details>
  <summary>iframe and embed security</summary>

### iframe and embed security

**Definition:**

- `<iframe>` встраивает другой документ. Ограничить возможности с помощью `sandbox`, набор `title` для доступности и управления реферером с помощью `referrerpolicy`.

| Атрибут          | Цель                                   |
| ---------------- | -------------------------------------- |
| `sandbox`        | Ограничить скрипты, формы и т. д.      |
| `referrerpolicy` | Заголовок Control Referer              |
| `title`          | Требуется для программ чтения с экрана |

**How it works:**

- `<iframe>` встраивает другой контекст просмотра.
- `sandbox` атрибут ограничивает скрипты, формы, всплывающие окна.
- Всегда устанавливаю `title` для доступности.

| Атрибут          | Цель                     |
| ---------------- | ------------------------ |
| `sandbox`        | Ограничения возможностей |
| `referrerpolicy` | Контроль рефералов       |
| `title`          | Доступное имя            |

**When to use:**

Платежные виджеты · встраивание видео · изолированный сторонний контент.

#### Common pitfalls

- Отсутствующий `sandbox` на ненадежных вставках.
- `allow-scripts allow-same-origin` вместе сводит на нет изоляцию.
- Нет `title` (Пользователи SR слышат только «кадр»).

**Examples:**

```html
<iframe
  src="https://example.com/embed"
  sandbox="allow-scripts allow-same-origin"
  referrerpolicy="no-referrer"
  title="Embedded content"
></iframe>
```

**Sources:** [MDN — iframe](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe)

</details>

<details>
  <summary>Miscellaneous elements</summary>

### Miscellaneous elements

**Definition:**

HTML включает в себя специализированные элементы для предложений, прогресса, вывода и резервных вариантов без JS.

| Элемент      | Цель                                     |
| ------------ | ---------------------------------------- |
| `<datalist>` | Предложения по `<input list>`            |
| `<meter>`    | Скалярное значение в известном диапазоне |
| `<progress>` | Выполнение задачи                        |
| `<noscript>` | Резервный вариант при отключении JS      |

**How it works:**

- `<datalist>` — предложения по `<input list>`.
- `<meter>` — датчик в известном диапазоне (не прогресс).
- `<progress>` — завершение задачи.
- `<noscript>` — резервный вариант при отключенном JS.

| Элемент      | Цель                    |
| ------------ | ----------------------- |
| `<datalist>` | Предложения по вводу    |
| `<progress>` | Индикатор завершения    |
| `<noscript>` | Резервный вариант No-JS |

**When to use:**

Предложения автозаполнения · ход загрузки · откат критического контента.

#### Common pitfalls

- Сбивает с толку `<meter>` с `<progress>`.
- Опираясь на `<noscript>` только для основных функций.
- `datalist` в качестве единственной проверки (не принудительно).

**Examples:**

```html
<input list="browsers" name="browser" />
<datalist id="browsers">
  <option value="Chrome"></option>
  <option value="Firefox"></option>
</datalist>
<noscript><p>Enable JavaScript for full experience.</p></noscript>
```

**Sources:** [MDN — список данных](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/datalist)

</details>

---

<a name="forms--validation"></a>

## Attributes reference

<details>
  <summary>Common attributes</summary>

### Common attributes

**Definition:**

Атрибуты предоставляют метаданные и поведение элементов HTML — глобальные атрибуты работают с любым элементом; другие зависят от элемента.

| Атрибут | Элемент(ы)                 | Цель                           |
| ------- | -------------------------- | ------------------------------ |
| `src`   | img, скрипт, iframe        | URL-адрес ресурса              |
| `href`  | а, ссылка                  | Цель ссылки                    |
| `class` | Глобальный                 | CSS-хук                        |
| `id`    | Глобальный                 | Уникальный идентификатор       |
| `name`  | элементы управления формой | Введите предоставленные данные |

**How it works:**

- Специфические для элемента и глобальные атрибуты изменяют поведение.
- `src`/`href` найти ресурсы; `name` ключи формируют данные; `class`/`id` перехват CSS/JS.

| Атрибут | Цель              |
| ------- | ----------------- |
| `src`   | URL-адрес ресурса |
| `name`  | Ключ поля формы   |

**When to use:**

Изображение `src`+`alt` · связь `href` · контроль форм `name`.

#### Common pitfalls

- `id` повторно использовано на странице.
- `class` для хуков только для JS (предпочтительнее `data-*`).
- Отсутствующий `alt` при использовании `src` на изображении.

**Examples:**

```html
<img src="/logo.png" alt="Logo" class="brand" id="site-logo" />
<a href="/about" class="nav-link">About</a>
<input id="email" name="email" type="email" required />
```

**Sources:** [MDN — Атрибуты](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes)

</details>

<details>
  <summary>class vs id</summary>

### class vs id

**Definition:**

`class` может появляться на многих элементах для общего стиля. `id` должен быть уникальным на странице — используется для привязок, ассоциации меток и уникальных целей.

|                  | `class`         | `id`                  |
| ---------------- | --------------- | --------------------- |
| Мощность         | Многие элементы | Один на страницу      |
| CSS-селектор     | `.my-class`     | `#my-id`              |
| Использовать для | Общие шаблоны   | Уникальный якорь/цель |

**How it works:**

- `class` — многоразовое использование, много на странице, CSS `.class`.
- `id` — уникальный для каждого документа, `#id` селектор, цели фрагмента.
- `label for` и ссылка на ARIA `id`.

|      | `class` | `id`             |
| ---- | ------- | ---------------- |
| Граф | Многие  | Один на страницу |

**When to use:**

Общий стиль карточки (`class`) · пропустить цель ссылки (`id="main"`).

#### Common pitfalls

- Повторяющиеся идентификаторы (недействительные).
- Стилизация с помощью идентификаторов (высокая специфичность).
- Автоматически сгенерированные идентификаторы конфликтуют в циклах.

**Examples:**

```html
<p class="intro highlight">Shared classes</p>
<main id="main">Unique landmark anchor</main>
<label for="email">Email</label>
```

**Sources:** [MDN — идентификатор](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/id)

</details>

<details>
  <summary>Global attributes</summary>

### Global attributes

**Definition:**

Глобальные атрибуты можно использовать для любого элемента HTML — `class`, `id`, `hidden`, `inert`, `tabindex`, `data-*`, и все `aria-*` атрибуты.

| Атрибут            | Цель                               |
| ------------------ | ---------------------------------- |
| `data-*`           | Пользовательские данные для JS/CSS |
| `aria-*`           | Семантика доступности              |
| `tabindex`         | Порядок фокусировки (`0` or `-1`)  |
| `hidden` / `inert` | Видимость и взаимодействие         |

**How it works:**

- Действительно для любого элемента: `class`, `id`, `hidden`, `inert`, `tabindex`, `data-*`, `aria-*`, `lang`, `title`.

| Атрибут  | Цель                    |
| -------- | ----------------------- |
| `data-*` | Пользовательские данные |
| `aria-*` | Доступность             |

**When to use:**

`data-user-id` для JS · `aria-label` на кнопках со значками · `inert` во время модального режима.

#### Common pitfalls

- `tabindex="1"` положительные значения (избегать).
- `title` в качестве первичной метки (ненадежно).
- Неверный `data-*` имена (заглавные).

**Examples:**

```html
<div data-user-id="42" aria-label="User profile card" tabindex="0"></div>
```

**Sources:** [MDN — Глобальные атрибуты](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes)

</details>

---

<a name="interview-practice"></a>
