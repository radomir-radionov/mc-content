# HTML — Performance

Загрузка сценариев, подсказки по ресурсам, основные веб-показатели и FOUC.

[ ← Вернуться в центр](./1_html.md)

### Related docs

- [HTML — Структура](./1_html_structure.md) · [HTML — Семантика](./1_html_semantics.md) · [HTML — Формы](./1_html_forms.md) · [HTML — Медиа](./1_html_media.md) · [HTML — Рендеринг](./1_html_rendering.md) · [HTML — Производительность](./1*html* Performance.md) · [HTML — Доступность](./1_html_a11y.md) · [HTML — SEO](./1_html_seo.md) · [HTML — Расширенный](./1_html_advanced.md) · [HTML — Интервью](./1_html_interview.md)

### Sections

- [Performance & resource loading](#performance-resource-loading)

---

## Performance & resource loading

<details>
  <summary>async vs defer vs module</summary>

### async vs defer vs module

**Definition:**

Атрибуты загрузки скрипта управляют загрузкой и выполнением JavaScript относительно анализа HTML.

| Атрибут         | Скачать       | Выполнить              | Заказать                 |
| --------------- | ------------- | ---------------------- | ------------------------ |
| (нет)           | Парсер блоков | Немедленно             | Последовательный         |
| `defer`         | Параллельно   | После разбора          | Сохранено                |
| `async`         | Параллельно   | Когда все будет готово | Не гарантировано         |
| `type="module"` | Параллельно   | После разбора          | Сохранено; строгий режим |

**How it works:**

- По умолчанию `<script>` блокирует анализ HTML во время загрузки + выполнения.
- `defer` — параллельная загрузка, выполнение по порядку после разбора.
- `async` — параллельная загрузка, выполняется по готовности (неупорядоченная).
- `type="module"` — отсроченный, строгий, `import`/`export`.

| Атрибут  | Заказать         | Блокирует разбор?             |
| -------- | ---------------- | ----------------------------- |
| `defer`  | Сохранено        | Нет (выполнить после анализа) |
| `async`  | Не гарантировано | Во время выполнения           |
| `module` | Сохранено        | Нет                           |

**When to use:**

Пакет приложений → `defer` · аналитика → `async` · ES-приложение → `module`.

#### Common pitfalls

- Два `async` скрипты с зависимостями.
- Скрипты модулей требуют CORS для перекрестного происхождения. `src`.
- Встроенный скрипт, предполагающий, что отложенный внешний файл уже запущен.

**Examples:**

```html
<script src="app.js" defer></script>
<script src="analytics.js" async></script>
<script type="module" src="app.mjs"></script>
```

**Thoughts:** Полное руководство по загрузке скриптов → [3_js_browser.md](./3_js_browser.md).

**Sources:** [MDN — загрузка скрипта](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script)

</details>

<details>
  <summary>Page load optimization (HTML-level)</summary>

### Page load optimization (HTML-level)

**Definition:**

Оптимизация на уровне HTML уменьшает размер передачи, предотвращает сдвиг макета и позволяет избежать блокировки ресурсов синтаксического анализатора.

| Техника                          | Выгода                               |
| -------------------------------- | ------------------------------------ |
| `srcset` / современные форматы   | Уменьшенные изображения              |
| `width`/`height` на изображениях | Предотвратить CLS                    |
| `defer` / `module` сценарии      | Неблокирующий JS                     |
| Подсказки по ресурсам            | Ускорение критически важных ресурсов |

**How it works:**

- Минимизируйте ресурсы, блокирующие рендеринг, на критическом пути.
- Предварительная загрузка ресурсов LCP; ленивая загрузка изображений ниже сгиба.
- `defer`/`module` для сценариев приложений; `async` для независимых тегов.

| Техника                      | Выгода                         |
| ---------------------------- | ------------------------------ |
| `srcset`/современные форматы | Уменьшенные изображения        |
| `defer` сценарии             | Неблокирующий JS               |
| Подсказки по ресурсам        | Ускоренная критическая выборка |

**When to use:**

Целевая страница LCP · блог с множеством изображений · составление бюджета на основе сторонних сценариев.

#### Common pitfalls

- Предварительная загрузка слишком большого количества ресурсов.
- CSS, блокирующий рендеринг, внизу.
- Сторонние скрипты без `async`/`defer`.

**Examples:**

```html
<link rel="preconnect" href="https://cdn.example.com" />
<img src="photo.webp" alt="Photo" width="600" height="400" loading="lazy" />
<script src="/app.js" defer></script>
```

**Sources:** [web.dev — Производительность](https://web.dev/explore/ Performance)

</details>

<details>
  <summary>Core Web Vitals</summary>

### Core Web Vitals

**Definition:**

Показатели качества страниц Google (2020+). **Основной** набор — **LCP** (основное содержимое нарисовано), **INP** (нажатие/тап/клавиша → следующая отрисовка), **CLS** (неожиданный сдвиг макета). URL **проходит**, если **все три** «хороши» на уровне **p75**, **мобильные и настольные устройства оцениваются отдельно**. **FCP** и **TTFB** являются диагностическими, а не ключевыми и не ранжирующими.

| Метрика  | Вопрос                                                | 🟢 Хорошо | 🟡 Требует улучшения | 🔴 Плохо |
| -------- | ----------------------------------------------------- | --------- | -------------------- | -------- |
| **LCP**  | Когда я увидел основной контент?                      | ≤ 2,5 с   | 2,5–4,0 с            | > 4,0 с  |
| **INP**  | После нажатия UI обновился быстро?                    | ≤ 200 мс  | 200–500 мс           | > 500 мс |
| **CLS**  | Контент подпрыгнул, и я мог нажать не на тот элемент? | ≤ 0,1     | 0,1–0,25             | > 0,25   |
| **FCP**  | Когда _что-нибудь_ появилось?                         | ≤ 1,8 с   | 1,8–3,0 с            | > 3,0 с  |
| **TTFB** | Как скоро сервер начал отвечать?                      | ≤ 0,8 с   | 0,8–1,8 с            | > 1,8 с  |

Пороговые значения одинаковы на телефоне и настольном компьютере; **выборки — нет**. При ранжировании используются данные **поля CrUX** (28-дневное окно), а не лабораторный Lighthouse. **CLS** безразмерный; остальные — время. **FID** был Core до **12 марта 2024** — его заменил **INP** (все клики/тапы/клавиши, полный путь до отрисовки). Lighthouse без пользователя показывает **TBT**, а не INP.

|               | **Лаборатория**                  | **Поле** (RUM)                                       |
| ------------- | -------------------------------- | ---------------------------------------------------- |
| Инструменты   | Lighthouse, PSI lab, Performance | CrUX, PSI field, Search Console, `web-vitals`        |
| Использование | Воспроизвести _что исправить_    | p75 реальных визитов — _что использует ранжирование_ |

Лаборатория ≠ поле: INP нужен реальный ввод; CLS в поле включает скролл/баннеры; элемент LCP и кэш различаются. Рабочий процесс: сначала поле → лабораторный срез (не «улучшить LCP») → перепроверить все три. Медленный hero → [LCP](#lcp-largest-contentful-paint); неотзывчивые кнопки → [INP](#inp-interaction-to-next-paint); скачок → [CLS](#cls-cumulative-layout-shift); пустой экран → [FCP](#first-contentful-paint-fcp) / [TTFB](#ttfb-time-to-first-byte). Начинайте с **мобильных** URL с высоким трафиком.

**When to use:**

SEO-рейтинг, разбор инцидентов, бюджеты Lighthouse CI, тикеты с числовой целью p75.

**Пример** — поле (`web-vitals`). Отправка на `visibilitychange` → `hidden` через `sendBeacon` (`unload` ломает **bfcache**). Отчитывайтесь по **p75**, не по среднему.

```js
import { onCLS, onINP, onLCP } from "web-vitals";

function send(metric) {
  navigator.sendBeacon("/analytics", JSON.stringify(metric));
}

onCLS(send);
onINP(send);
onLCP(send);
```

```js
import { onLCP, onINP, onCLS } from "web-vitals/attribution";
onLCP(({ attribution }) => console.log(attribution)); // TTFB, load delay/duration, render delay
onINP(({ attribution }) =>
  console.log(attribution.inputDelay, attribution.processingDuration),
);
onCLS(({ attribution }) => console.log(attribution.largestShiftTarget));
```

Next.js: [4_nextjs.md — CWV](./4_nextjs_optimizations.md#core-web-vitals-on-vercel). Подробнее по метрикам: [LCP](#lcp-largest-contentful-paint) · [INP](#inp-interaction-to-next-paint) · [CLS](#cls-cumulative-layout-shift). DevTools: [11_vsc_browser.md](./11_vsc_browser.md#devtools--core-web-vitals).

#### Common pitfalls

Доверять Lighthouse, пока Search Console красный. Ждать CrUX на следующий день. Оптимизировать **среднее**. Называть **FID** Core. Считать **TBT** за INP. Preload всего подряд. Аналитика на `unload`.

**Pluses and minuses:**

| +                                    | −                                           |
| ------------------------------------ | ------------------------------------------- |
| Общий язык с SEO                     | Зелёный Lighthouse ≠ зелёный Search Console |
| Attribution указывает элемент/скрипт | 28-дневный лаг CrUX; среднее скрывает хвост |

**Thoughts:** Лаборатория = как воспроизвести; поле p75 = важно ли пользователям (и ранжированию).

**See also:**

[LCP](#lcp-largest-contentful-paint) · [FCP](#first-contentful-paint-fcp) · [TTFB](#ttfb-time-to-first-byte) · [INP](#inp-interaction-to-next-paint) · [Next.js CWV](./4_nextjs_optimizations.md#core-web-vitals-on-vercel)

---

**Sources:** [web.dev — Vitals](https://web.dev/articles/vitals) · [web.dev — INP](https://web.dev/articles/inp) · [Search — CWV](https://developers.google.com/search/docs/appearance/core-web-vitals) · [web-vitals](https://github.com/GoogleChrome/web-vitals)

</details>

<details>
  <summary>LCP (Largest Contentful Paint)</summary>

### LCP (Largest Contentful Paint)

**Definition:**

LCP — это время от начала навигации до момента рисования **самого большого элемента контента в области просмотра**. Браузер выбирает элемент (главное изображение, обложку статьи, большой текстовый блок, видеопостер) — вручную его назначить нельзя. Проверьте его в PageSpeed ​​Insights или на панели «Производительность».

Хорошо: **≤ 2,5 с**. Плохо: **> 4 с**. Поле «проходит» — p75 (см. [Core Web Vitals](#core-web-vitals)).До тех пор, пока это не произойдет, пользователь не сможет сказать, что попал на правильную страницу; медленный LCP повышает риск отскока.

**How it works:**

Google разбивает LCP на четыре последовательные части (без пробелов). На **хорошо оптимизированной** странице _типичная_ доля составляет ~40% TTFB, <10% задержка загрузки ресурса, ~40% продолжительность загрузки ресурса, <10% задержка рендеринга элемента — реальные сайты различаются; исправьте большой фрагмент в _вашей_ атрибуции (`web-vitals/attribution`: `timeToFirstByte`, `resourceLoadDelay`, `resourceLoadDuration`, `elementRenderDelay`).

| Подчасть                               | Что это такое                                 | Типичная доля (оптимизированная) |
| -------------------------------------- | --------------------------------------------- | -------------------------------- |
| **ТТФБ**                               | Начало навигации → первый байт HTML           | ~40%                             |
| **Задержка загрузки ресурса**          | Первый байт HTML → начало запроса ресурса LCP | <10%                             |
| **Продолжительность загрузки ресурса** | Загрузка ресурса LCP                          | ~40%                             |
| **Задержка отрисовки элемента**        | Ресурс готов → рисовать на экране             | <10%                             |

Только текстовый LCP (без дополнительных файлов) **не имеет** задержки/длительности ресурса — только TTFB + задержка рендеринга.

Дополнительные редиректы, медленный источник, отсутствие CDN/кеша в HTML. Подробности: [TTFB](#ttfb-время до первого байта). До первого байта браузер не может проанализировать или обнаружить URL-адрес LCP.

Поместите изображение LCP в HTML как `<img src>` поэтому сканер предварительной загрузки находит его — не JS-внедренный, не `data-src`. **не** используйте `loading="lazy"` на этом. Набор `fetchpriority="high"` на ресурсах **1–2** (в противном случае изображения в области просмотра начинаются с низкого уровня до макета). Перекрестное происхождение: `<link rel="preconnect">`, или того же происхождения. CSS `background-image` обнаруживается только после CSS — предпочитайте `<img>`или предварительно загрузить:

```html
<link rel="preload" as="image" href="hero.avif" fetchpriority="high" />
<link
  rel="preload"
  as="image"
  imagesrcset="hero-800.avif 800w, hero-1600.avif 1600w"
  imagesizes="100vw"
  fetchpriority="high"
/>
```

Следующий.js: `priority` on `next/image` отключает отложенную загрузку и повышает приоритет выборки; в **Next.js 16** предпочитают `preload` (`priority` устарел). → [4_nextjs.md](./4_nextjs_optimizations.md#core-web-vitals-on-vercel).

Фотографии → растр **AVIF → WebP → JPEG** через `<picture>`, а не SVG. Служить `srcset`/`sizes`. CDN изображений могут выбирать формат/размер из Client Hints/UA. Длинный `Cache-Control` на статических файлах; сортировка/очистка ненужных запросов очистки кеша; избегать `Set-Cookie` о кэшируемых ответах. Понизить конкурирующую работу: `fetchpriority="low"` на карусельных слайдах без LCP; `defer`/`async` сторонние скрипты. Маленькие значки: `data:` URL или **встроенный `<svg>`** (СВГО); base64 на ~⅓ больше и использует общий кеш документов — не для героев. Полевые данные часто показывают преобладание TTFB/задержек — проверьте перед марафонским кодированием.

Уменьшите CSS, блокирующий рендеринг (критическое встроенное, покрытие для неиспользуемых). Избегайте синхронизации JS в `<head>` (`defer`/`async` или конец `body`). SSR/SSG, чтобы узел LCP находился в первом HTML. Длинные задачи после прибытия изображения задерживают покраску. Текст ЛКП: `font-display: swap` + предварительно загрузить критический шрифт.

Асинхронный некритичный CSS (риск FOUC):

```html
<link
  rel="preload"
  href="non-critical.css"
  as="style"
  onload="this.onload=null;this.rel='stylesheet'"
/>
<noscript><link rel="stylesheet" href="non-critical.css" /></noscript>
```

[Правила спекуляции] (https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script/type/speculationrules) могут предварительно отображать вероятный следующий URL-адрес, поэтому LCP для этой навигации будет близок к нулю (аналогично восстановлению **bfcache**). Хром-только при записи; Задержка Safari/Firefox. `eagerness`: `immediate` / `eager` / `moderate` / `conservative` — более ранние предположения теряют больше смысла, если пользователь никогда не нажимает на ссылку.

```html
<script type="speculationrules">
  {
    "prerender": [
      { "where": { "href_matches": "/*" }, "eagerness": "moderate" }
    ]
  }
</script>
```

**When to use:**

Герой продукта, обложка статьи, маркетинговый лендинг. Подтвердите слабый фрагмент с помощью CrUX/атрибуции, прежде чем сжимать файлы «на всякий случай».

#### Common pitfalls

- `loading="lazy"` или JS/`data-src` на герое (сканер предзагрузки никогда не видит настоящего `src`).
- `fetchpriority="high"` на многих активах (сигнал становится шумом).
- Герой только для CSS, без `preload`.
- Предварительная загрузка всего и ограничение реального запроса LCP.
- Оптимизация размера файла, когда атрибуция показывает TTFB или задержку рендеринга.

**Pluses and minuses:**

| +                                                  | −                                                              |
| -------------------------------------------------- | -------------------------------------------------------------- |
| Сопоставляет видимый момент «основного содержания» | Изменение элемента LCP в зависимости от области просмотра/кэша |
| Четыре именных кусочка для билетов                 | Лабораторный холодный тайник ≠ полевой теплый тайник           |

**Examples:**

```html
<img
  src="hero.avif"
  alt="Cover"
  width="1200"
  height="630"
  fetchpriority="high"
/>

<picture>
  <source
    type="image/avif"
    srcset="hero-640.avif 640w, hero-1280.avif 1280w"
    sizes="100vw"
  />
  <source
    type="image/webp"
    srcset="hero-640.webp 640w, hero-1280.webp 1280w"
    sizes="100vw"
  />
  <img
    src="hero-1280.jpg"
    srcset="hero-640.jpg 640w, hero-1280.jpg 1280w"
    sizes="100vw"
    alt="Cover"
    width="1280"
    height="720"
    fetchpriority="high"
  />
</picture>
```

**Thoughts:** Вы не выбираете узел LCP — вы делаете все, что выбрал браузер, быстрым, сокращая TTFB, задержку обнаружения, байты, а затем ожидание основного потока/CSS/шрифта.

**See also:** [Core Web Vitals](#core-web-vitals) · [TTFB](#ttfb-time-to-first-byte) · [FCP](#first-contentful-paint-fcp) · [INP](#inp-interaction-to-next-paint) · [img](./1_html_media.md#img-element-and-responsive-images) · [DevTools](./11_vsc_browser.md#devtools--core-web-vitals)

**Sources:**

[web.dev — Оптимизация LCP](https://web.dev/articles/optimize-lcp) · [web.dev — LCP](https://web.dev/articles/lcp) · [web.dev — Приоритет выборки](https://web.dev/articles/fetch-priority)

---

</details>

<details>
  <summary>First Contentful Paint (FCP)</summary>

### First Contentful Paint (FCP)

**Definition:** FCP — это время от начала навигации до момента, когда браузер отрисовывает первый **контент**: текст, изображение, SVG или непустой объект. `<canvas>`. Это означает «пустой экран закончился», а не «страница готова».

**How it works:**

- FCP – это **Web Vital**, используемый для диагностики нагрузки. Это **не** Core Web Vital (это LCP, INP, CLS) и **не** напрямую влияет на рейтинг.
- Медленный FCP обычно означает медленный **LCP**: оба ждут одного и того же раннего пути — [TTFB](#ttfb-time-to-first-byte) и **блокировка рендеринга** CSS/JS перед первой отрисовкой.
- Оценка **75-го процентиля** отдельно для мобильных и настольных компьютеров. **Хорошо:** ≤ 1,8 с.**Требует улучшения:** 1,8–3 с. **Плохо:** > 3 с.
- Хороший FCP все равно может заставить пользователя ждать: основной контент (**LCP**) и скорость ввода (**INP**) могут появиться гораздо позже.

- Убран блокирующий рендеринг CSS/JS; минимизировать CSS; удалите неиспользуемый CSS.
- Нижний ТТФБ; добавлять `<link rel="preconnect">` (DNS+TCP+TLS); удалить лишние редиректы.
- `<link rel="preload">` для файлов, необходимых для первой покраски; оптимизировать шрифты (`font-display`, подмножество неиспользуемых глифов).
- Меньший вес страницы; HTTP-кэширование.

**When to use:**

Длинный белый экран. Когда LCP плох, FCP сообщает вам: «еще ничего не нарисовано» или «неправильный самый большой элемент». Те же ранние рычаги, что и LCP/TTFB.

#### Common pitfalls

- Называть FCP ключевым веб-важным или считать его «страницей готовой».
- Оптимизация логотипа-заставки, чтобы FCP выглядел хорошо, пока LCP задерживается.

**Pluses and minuses:**

| +                                                 | −                                                    |
| ------------------------------------------------- | ---------------------------------------------------- |
| Самый ранний видимый пользователю сигнал нагрузки | Не основные веб-показатели / нет рейтинга            |
| Разделяет критический путь с LCP                  | Первый логотип или строка текста ≠ полезная страница |

**Examples:**

```js
import { onFCP } from "web-vitals";
onFCP(console.log);
```

Предпочитать `web-vitals` над сырым `PerformanceObserver` on `paint`: поле FCP также обрабатывает фоновые вкладки, восстановление bfcache, iframe и предварительную визуализацию (`activationStart`).

**Thoughts:** FCP = первые пиксели контента. Исправление TTFB и блокировки ресурсов; еще измеряю LCP и INP.

**See also:** [Core Web Vitals](#core-web-vitals) · [LCP](#lcp-largest-contentful-paint) · [TTFB](#ttfb-time-to-first-byte) · [INP](#inp-interaction-to-next-paint) · [preload, prefetch, preconnect](./6_web_api_performance.md#preload-prefetch-preconnect--prerender) · [HTML — first краска](./1_html_rendering.md#first-render--full-pipeline)

**Sources:**

[web.dev — FCP](https://web.dev/articles/fcp) · [web.dev — Vitals](https://web.dev/articles/vitals)

---

</details>

<details>
  <summary>TTFB (Time to First Byte)</summary>

### TTFB (Time to First Byte)

**Definition:** TTFB — это время от начала навигации до получения браузером **первого байта** ответа. Это **диагностический** показатель (а не Core Web Vital) для настройки соединения и реагирования источника/CDN.

**How it works:**

TTFB представляет собой сумму:

- редиректы (каждый 301/302 — это еще один путь туда и обратно);
- запуск сервис-воркера, если он управляет страницей;
- DNS-поиск;
- TCP-соединение + TLS-квитирование;
- сервер работает до первого байта ответа.

Пока этот байт не прибудет, браузеру нечего анализировать, поэтому **FCP и LCP ждут**. Высокий TTFB часто снижает другие показатели.**Низкий** TTFB **не** гарантирует хороший LCP/FCP — ресурсы, JS и отрисовка по-прежнему следуют за ним.

| Рейтинг ([web.dev](https://web.dev/articles/optimize-ttfb)) | ТТФБ      |
| ----------------------------------------------------------- | --------- |
| Хорошо                                                      | ≤ 0,8 с   |
| Требует улучшения                                           | 0,8–1,8 с |
| Бедные                                                      | > 1,8 с   |

Измерьте в PageSpeed ​​Insights, DevTools **Сеть** (Ожидание/TTFB), `web-vitals` `onTTFB`или данные поля через **CrUX**. Типичные причины: **сеть** (расстояние, отсутствие CDN) или **происхождение** (отрисовка HTML, база данных, сторонние API).

**Сеть**

- CDN для статических ресурсов и, если возможно, HTML — обычно также HTTP/2, HTTP/3, TLS 1.3. См. [CDN](./6_web_api_core.md#cdn-content-delivery-network).
- Кэшировать HTML с помощью `Cache-Control`; сделать недействительным при развертывании; избегайте уникальных строк запроса, которые разрушают CDN/общие кеши. См. [HTTP-кэширование](./6_web_api_http.md#http-caching).
- Отключить редиректы (правильная схема и косая черта). `Strict-Transport-Security` удаляет HTTP→HTTPS при **повторных** посещениях; при первом посещении по-прежнему выполняется перенаправление, если хост не находится в **списке предварительной загрузки HSTS**. См. [Подтверждение SSL/TLS](./6_web_api_http.md#ssltls--https-handshake).

**Сервер**

- Поток HTML (`renderToPipeableStream` + `<Suspense>` в React), чтобы браузер мог анализировать фрагменты, пока медленно загружаются данные. См. [проектирование системы — потоковая SSR](./0_system_design_architecture.md#rendering-strategies-when-html-is-built).
- Next.js: SSG / [ISR](./4_nextjs_rendering.md#incremental-static-regeneration-isr) для страниц, которым не требуется HTML для каждого посетителя — тогда CDN может его обслуживать. Рычаги: [Core Web Vitals на Vercel](./4_nextjs_optimizations.md#core-web-vitals-on-vercel).
- Сервис-воркер **stale- while-revalidate** может немедленно вернуть оболочку приложения из кэша; **не** используйте его для персонализированного HTML-кода для каждого запроса (это может привести к устаревшему документу). См. [Service Workers](./6_web_api_performance.md#service-workers).
- **103 ранних подсказки:** пока источник готовит окончательный ответ, отправьте `Link` подсказки, чтобы браузер мог запустить важные ресурсы. Некоторые CDN (например, Cloudflare) выдают 103 до того, как существует исходный HTML. Принятие по-прежнему неравномерно.
- Здоровье Origin (память, время работы). Разбить время происхождения с помощью `Server-Timing`; эти метки отображаются в браузере через **Время навигации** (`PerformanceNavigationTiming`).

**When to use:**

Медленный LCP с поздним документом; пользователи вдали от места происхождения; SSR, который ожидает DB/API перед отправкой любого HTML.

#### Common pitfalls

Уникальные параметры аналитических запросов в обход кеша CDN. Дополнительные 301/302 прыжков. Edge SSR рядом с удаленной базой данных (TTFB хуже, чем у регионального узла). Предполагается, что кэшированный персонализированный HTML безопасен. Рассматривать TTFB как замену LCP.

**Pluses and minuses:**

| +                                            | −                                                                                                  |
| -------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| Объясняет зависание FCP/LCP перед анализом   | Не является основной веб-важной задачей; погоня за TTFB не дает возможности увидеть JS/изображения |
| Атрибуция разделяет DNS, соединение и запрос | 103 Early Hints могут сделать «первый байт» промежуточным ответом, а не окончательным HTML         |

**Examples:**

```js
import { onTTFB } from "web-vitals";

onTTFB((metric) => {
  console.log("TTFB:", metric.value, "ms", metric.rating);
  const nav = metric.entries[0];
  if (nav) {
    console.log("DNS:", nav.domainLookupEnd - nav.domainLookupStart);
    console.log("Connect:", nav.connectEnd - nav.connectStart);
    console.log("Request→first byte:", nav.responseStart - nav.requestStart);
  }
});
```

```http
Server-Timing: db;dur=53, app;dur=47.2;desc="render"
103 Early Hints
Link: </hero.webp>; rel=preload; as=image
```

**Thoughts:** Первый байт разблокирует синтаксический анализ. Исправьте работу CDN, перенаправлений и источников, а затем оптимизируйте LCP после запуска HTML.

**See also:** [Core Web Vitals](#core-web-vitals) · [LCP](#lcp-largest-contentful-paint) · [FCP](#first-contentful-paint-fcp) · [INP](#inp-interaction-to-next-paint) · [CDN](./6_web_api_core.md#cdn-content-delivery-network)

**Sources:**

[web.dev — TTFB](https://web.dev/articles/ttfb) · [web.dev — Оптимизация TTFB](https://web.dev/articles/optimize-ttfb) · [web-vitals `onTTFB`](https://github.com/GoogleChrome/web-vitals) · [MDN — Server-Timing](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Server-Timing) · [MDN — 103 ранних подсказки](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/103)

---

</details>

<details>
  <summary>INP (Interaction to Next Paint)</summary>

### INP (Interaction to Next Paint)

**Definition:**

Полевая метрика для реагирования: время от щелчка, касания или нажатия клавиши до того, как браузер отрисует следующий кадр, который может показать видимый ответ. **Хорошо** ≤ 200 мс, **требует улучшения** ≤ 500 мс, **плохо** > 500 мс — на **75-м процентиле** просмотров страниц отдельно на мобильных устройствах и компьютерах.

INP **не** ожидает сети или более поздней версии асинхронного пользовательского интерфейса. Он измеряет, как долго блокируется следующая краска\*\*.Медленная обратная связь выглядит так, будто действие не удалось (двойное добавление в корзину, отказ от оформления заказа).

**How it works:**

INP отслеживает **все** соответствующие взаимодействия на протяжении всего срока службы страницы. Значением обычно является **худшее** взаимодействие, игнорируя выбросы (один самый высокий показатель на 50 взаимодействий). Наведение курсора, прокрутка и масштабирование **не** учитываются, если они не включают в себя щелчок/касание/клавишу. Взаимодействия iframe учитываются для пользователей (CrUX), но JS RUM часто не видит iframe из разных источников.

Взаимодействие — это один жест (`pointerdown` + `click`, or `keydown` + `keyup`). Задержка — это самое продолжительное событие в этой группе, от начала до возможности следующей отрисовки.

Три фазы (сумма = задержка взаимодействия):

| Фаза                            | Что                                                   | Типичная причина                                 |
| ------------------------------- | ----------------------------------------------------- | ------------------------------------------------ |
| **Задержка ввода**              | Действие пользователя → запускается первый обработчик | Основной поток занят (JS, длинные задачи)        |
| **Продолжительность обработки** | Обработчики + другая работа по синхронизации          | Обработчики тяжелых кликов, большой коммит React |
| **Задержка презентации**        | Обработчики готовы → обновлен фрейм                   | Верстка/рисовка, сторонний JS, огромный DOM      |

FID (Core Vital до **марта 2024**) измерял только **первое** взаимодействие и только **задержку ввода**. INP заменил его: все клики/нажатия/клавиши, полный путь к следующей отрисовке. Lab Lighthouse без отчетов пользователей **TBT**, а не INP.

Чем INP хуже:

- **Длинные задачи** (> 50 мс) в основном потоке.- Тяжелые обработчики/большие обновления клиентского DOM.
- **Перестановка макета** — писать стили → читать `offsetHeight` → написать еще раз (раскладка принудительной синхронизации).
- Большой/глубокий DOM (Маяк предупреждает ~800 узлов, серьезный ~1400).
- Клиент `innerHTML` большой разметки (один длинный анализ/вставка; сканер предварительной загрузки пропускает вложенные ресурсы → также LCP).

`useTransition` / `useDeferredValue` отмечать тяжелые обновления как несрочные. Раздельное увлажнение с помощью `<Suspense>`; RSC пропускает гидратацию. Реакция **19.2** `<Activity mode="hidden">` сохраняет состояние/DOM, срывает эффекты, скрывается с помощью `display: none`. Подробности — [4_react.md](./4_react_ecosystem.md#web-vitals-in-react-apps).

**Долгая работа** после критического пользовательского интерфейса. `scheduler.yield()` (Chrome 129+, Firefox 142+; **не Safari**) продолжает работать с приоритетом планировщика; `setTimeout(0)` переходит в конец очереди макрозадач. **не** полагайтесь на `navigator.scheduling.isInputPending()` (может быть `false` после щелчка).

```js
function yieldToMain() {
  if (globalThis.scheduler?.yield) return scheduler.yield();
  return new Promise((resolve) => setTimeout(resolve, 0));
}

async function saveSettings() {
  validateForm();
  showSpinner();
  updateUI();
  await yieldToMain();
  saveToLocalStorage();
  sendAnalytics();
}

async function runJobs(jobQueue, deadline = 50) {
  let lastYield = performance.now();
  for (const job of jobQueue) {
    job();
    if (performance.now() - lastYield > deadline) {
      await yieldToMain();
      lastYield = performance.now();
    }
  }
}
```

Веб-воркеры для **чистого** процессора (сортировка, анализ) — без DOM. Комлинк обертывает `postMessage`; большие полезные нагрузки: передача `ArrayBuffer`.

часто доминируют задержки презентации. Рабочие не могут запускать GTM как есть (требуется DOM). **Partytown** передает DOM в основной поток; Следующий `next/script` `strategy="worker"` потребности `experimental.nextScriptWorkers`. Предпочитать `lazyOnload`; удалить неиспользуемые теги. Стоимость тега: пиксели (только HTTP) < пользовательские шаблоны < собственный HTML (макет). Контейнер GTM должен быть небольшим (жесткий размер 300 КБ). Фасад для встраивания (`lite-youtube-embed`); `loading="lazy"` + `width`/`height` в iframe (CLS).

**When to use:**

Поиск, корзина, оформление заказа, фильтры, автозаполнение. Сначала измерьте поле (`web-vitals` атрибуция: `inputDelay`, `processingDuration`, `presentationDelay`).

**Исправьте порядок — сначала поставьте меньше JS.** Анализ + выполнение происходит в основном потоке. Большие связки повреждают INP и могут задерживать LCP.

1. **Измерить** — `webpack-bundle-analyzer` / `@next/bundle-analyzer`.
2. **Разделение кода** — по маршруту; `import()` / `React.lazy` / `next/dynamic` для диаграмм, редакторов, модальных окон. См. [10_build.md](./10_build_bundlers.md#code-splitting).
3. **Tree-shake** — под названием ESM (`lodash-es`, `date-fns`), нет `import _ from 'lodash'` / Момент. [10_build.md](./10_build_bundlers.md#tree-shaking).
4. **Напильники** — `export *` от `index.ts` может вытащить целый комплект пользовательского интерфейса. Следующий: [`optimizePackageImports`](./4_nextjs_optimizations.md#optimizepackageimports) (В следующих документах он по-прежнему отмечен как экспериментальный; Turbopack часто сам анализирует импорт).
5. **Работа на сервере** — RSC вместо клиентского Markdown и т. д. Лучший JS — это JS, который никогда не отправляется. Затем более легкие библиотеки, Brotli, бюджет пакета CI.

Другие методы:

- Отказ от дребезга `input`; дроссель `scroll`/`resize`.
- Прервать устаревание `fetch` с `AbortController` — [AbortController](./3_js_async_apis.md#abortcontroller).
- Предпочитаю одноразовый `setTimeout` сверх остатков `setInterval`; перенести тяжелую работу по таймеру на работника.
- Анимировать `transform`/`opacity` в CSS; JS включен `width`/`top`/`box-shadow` Расположение сил — [2_css.md](./2_css.md).
- Пакетный DOM: **все читает, затем все записывает**.
- Меньший DOM: фрагменты, более плоские деревья, не отображать скрытые вкладки, пока они не понадобятся; `content-visibility: auto` — [2_css.md](./2_css_architecture.md#contain--content-visibility).
- Более дешевые селекторы (длинные цепочки требуют больше затрат на пересчет стиля в большом DOM).
- Файлы JS меньшего размера (как правило, ~100 КБ); `type="module"` анализируется как отдельные фрагменты, поэтому ввод может проходить между ними.
- `touch-action: manipulation` на кнопках — пропускает историческое ожидание двойного касания и масштабирования примерно в 300 мс.

#### Common pitfalls

Yield/Workers перед вырезанием неиспользуемого JS. Рассматривать FID или TBT как INP. Счет прокрутки/наведения как INP. `isInputPending()` вместо того, чтобы уступить. Пользовательский HTML-код GTM для наглядного пользовательского интерфейса. Дублирующиеся теги в HTML **и** диспетчере тегов.

**Pluses and minuses:**

| +                                                                                | −                                                          |
| -------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| Соответствует пользовательскому интерфейсу после загрузки (большая часть сеанса) | Нет INP, если никто не щелкнул/нажал/напечатал             |
| Контроль имен атрибуции + этап                                                   | Лаборатория INP зависит от того, какие клики вы выполняете |
| Общий язык с SEO                                                                 | Прокрутка/наведение курсора выходят за рамки               |

**Examples:**

```tsx
import dynamic from "next/dynamic";

const HeavyChart = dynamic(() => import("./heavy-chart"), { ssr: false });
```

```js
let controller;
input.addEventListener("input", async (e) => {
  const query = e.target.value.trim();
  controller?.abort();
  if (!query) return;
  controller = new AbortController();
  const res = await fetch(`/search?q=${encodeURIComponent(query)}`, {
    signal: controller.signal,
  });
});
```

```js
// bad — layout each iteration
boxes.forEach((box) => {
  const height = box.offsetHeight;
  box.style.height = `${height * 2}px`;
});
// good — one layout
const heights = boxes.map((box) => box.offsetHeight);
boxes.forEach((box, i) => {
  box.style.height = `${heights[i] * 2}px`;
});
```

**Thoughts:** Нарисуйте ответ **сейчас** (спиннер, оптимистичный интерфейс); держите основной поток свободным от длинного JS.

**See also:** [Core Web Vitals](#core-web-vitals) · [LCP](#lcp-largest-contentful-paint) · [4_react.md — Web Vitals](./4_react_ecosystem.md#web-vitals-in-react-apps) · [4_nextjs.md — CWV](./4_nextjs_optimizations.md#core-web-vitals-on-vercel)

**Sources:**

[web.dev — INP](https://web.dev/articles/inp) · [web.dev — Оптимизация длительных задач](https://web.dev/articles/optimize-long-tasks) · [MDN — Scheduler.yield](https://developer.mozilla.org/en-US/docs/Web/API/Scheduler/yield) · [React `<Activity>`](https://react.dev/reference/react/Activity)

---

</details>

</details>

<details>
  <summary>CLS (Cumulative Layout Shift)</summary>

### CLS (Cumulative Layout Shift)

**Definition:** CLS — это основной веб-важный фактор визуальной стабильности: сколько уже видимого контента неожиданно перемещается во время (и после) загрузки. Хорошо **≤ 0,1**, требует улучшения **0,1–0,25**, плохо **> 0,25** (75-й процентиль).

**How it works:**

- **Сдвиг макета** фиксируется, когда видимый элемент меняет положение между кадрами без причины, определяемой пользователем. Классический случай: вы нажимаете «Купить», вверху загружается баннер, контент подскакивает, вы нажимаете «Отменить».
- Оценка одного сдвига: **доля воздействия × доля расстояния** — доля области просмотра, которая сместилась × насколько далеко она переместилась (в виде доли области просмотра). Большая территория + более продолжительное путешествие = хуже.- Группа смен в **окна сеансов**: новое окно запускается, если промежуток между сменами составляет **≥ 1 с**; окно длится не более **5 секунд**. CLS страницы — это **худшее окно**, а не сумма всех сдвигов на долгоживущей вкладке.
- **Ожидаемые** сдвиги исключены: дискретный вход (щелчок/касание) имеет отсрочку **500 мс** (`hadRecentInput`). Непрерывные жесты (прокрутка, масштабирование) **не** подпадают под это исключение.
- Лаборатория (Lighthouse) ≠ поле (CrUX). Отладка: DevTools **Performance → Layout Shifts**. Измерение / RUM: [Core Web Vitals](#core-web-vitals).

- Изображения/видео без `width`/`height` (или CSS `aspect-ratio`) — выкладывается пустым, тогда соседи подпрыгивают при приходе файла.
- Объявления, виджеты, iframe неизвестной высоты, особенно асинхронные.
- Вставляет **выше** уже видимое содержимое: баннеры cookie, панели рекомендаций, всплывающие сообщения об ошибках при рендеринге. Вставка одинакового размера вблизи **верхней** области просмотра оценивается хуже, чем вставка внизу.
- Веб-шрифты: резервные метрики ≠ пользовательские метрики → перекомпоновка.**FOIT** (текст скрыт до загрузки шрифта) и **FOUT** (резервный текст, затем замена). CSS: [Загрузка шрифта](./2_css_visual.md#font-loading).
- Анимация `top` / `left` / `margin` / `width` вместо `transform` — пересчет макета может перемещать одноуровневые элементы. CSS: [transform](./2_css_motion.md#transform).

- Всегда общий запасной вариант: `"Google Sans", sans-serif` — никогда не использовать одиночное пользовательское семейство (метрики по умолчанию в браузере расходятся сильнее).
- Сопоставьте резервный вариант с веб-шрифтом с помощью `@font-face` **`size-adjust`**, `ascent-override`, `descent-override`, `line-gap-override`.
- Предварительная загрузка критически важных шрифтов (`<link rel="preload" as="font" … crossorigin>`). Предпочитаю **WOFF2**, переменные шрифты, `unicode-range` подмножества (загрузка только для кириллического текста).
- `next/font` самостоятельные хосты (без перехода Google CDN) и резервная настройка `size-adjust`.

```tsx
import { Geist } from "next/font/google";

const geist = Geist({ subsets: ["latin"] });

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html className={geist.className}>
      <body>{children}</body>
    </html>
  );
}
```

- Контейнер `min-height` or `aspect-ratio` для **вероятного** слота; медиа-запросы для размеров точек останова. `<iframe width height>` когда известно.
- Баннеры cookie: зарезервируйте слот (или наложение). На мобильных устройствах они также могут стать элементом **LCP**. `preconnect` / `dns-prefetch` если сторонний.
- Не вводите данные над областью просмотра без жеста пользователя. Отдавайте предпочтение нижнему сгибу, наложению или слоту фиксированного размера.
- Бесконечная лента: переключайтесь внутри карусели **фиксированного размера**; предварительная выборка **до того, как** пользователь достигнет конца (исключение после взаимодействия 500 мс); или ** «Загрузить еще»**, чтобы ожидать изменения.

оживлять `transform` (`translate` / `scale` / `rotate`) и `opacity`. Они выполняются после макета.

Кэш назад/вперед мгновенно восстанавливает всю страницу; LCP/FCP/CLS в этой навигации часто ~0. web.dev: ~**10%** настольных компьютеров и ~**20%** мобильных устройств навигации вперед/назад. Распространенные блокаторы: `unload` (использовать `pagehide`) и `Cache-Control: no-store`. DevTools → Приложение → Кэш назад/вперед → Тест. [11_vsc_browser.md](./11_vsc_browser.md#devtools--core-web-vitals).

**When to use:**

Поздние медиа, сторонние внедрения, пользовательский интерфейс согласия, бесконечная прокрутка, пользовательские шрифты.

#### Common pitfalls

- CLS рассматривается как сумма всех смен (это **максимальное окно сеанса**).
- Счет сдвигов после щелчка как неожиданных (исключение 500 мс).
- Панель файлов cookie в потоке на мобильных устройствах (CLS ** и ** LCP).
- `font-family` без общего резервного варианта; `font-display: block` ФОИТ.
- Автоматическое добавление элементов с бесконечной прокруткой без зарезервированного места.

**Pluses and minuses:**

| +                                                              | −                                                                                      |
| -------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| Стабильные цели касаний, меньше ошибочных кликов               | Перед первой отрисовкой должны существовать файлы cookie/рекламные места               |
| Резервировать место дешевле, чем гнаться за плохим результатом | Наложение против потока: наложения позволяют избежать CLS, но могут скрыть LCP/контент |

**Examples:**

```html
<img src="cat.jpg" alt="Cat" width="800" height="600" />
<style>
  img {
    height: auto;
    width: 100%;
  }
</style>
```

`srcset` / `<picture>`: сохранять **одинаковое соотношение сторон** для всех кандидатов; набор `width`/`height` on `<img>` (и дальше `<source>` в современных браузерах). Next.js `<Image>` требует `width`+`height` or `fill`; статический импорт заполняет размеры. Примечания к фреймворку: [4_nextjs.md](./4_nextjs_optimizations.md#core-web-vitals-on-vercel).

**Thoughts:** Зарезервируйте место **до** поступления позднего контента; не помещайте новые блоки в текущую область просмотра, если только пользователь не попросил их.

**Sources:** [web.dev — CLS](https://web.dev/articles/cls) · [web.dev — Оптимизация CLS](https://web.dev/articles/optimize-cls) · [web.dev — Vitals](https://web.dev/articles/vitals) · [шрифты Next.js](https://nextjs.org/docs/app/getting-started/fonts) · [Изображение Next.js](https://nextjs.org/docs/app/api-reference/comComponents/image)

</details>

<details>
  <summary>Flash of Unstyled Content (FOUC)</summary>

### Flash of Unstyled Content (FOUC)

**Definition:**

FOUC возникает, когда HTML отображается до применения CSS. Держите критический CSS в себе `<head>` — CSS, блокирующий рендеринг, создан намеренно.

| Стратегия                  | Эффект                               |
| -------------------------- | ------------------------------------ |
| CSS в `<head>`             | Стили применяются до первой покраски |
| Встроенный критический CSS | Быстрая покраска сверху              |
| `font-display: swap`       | Контрольный шрифт FOUC (CSS)         |

**How it works:**

- HTML может отрисовываться до применения CSS → флэш без стилей.
- Сохраняйте CSS `<head>`; критический CSS может быть встроен.
- `font-display` в CSS контролируется шрифт FOUC.

| Стратегия                  | Эффект                       |
| -------------------------- | ---------------------------- |
| CSS в `<head>`             | Стили перед первой покраской |
| Встроенный критический CSS | Быстро выше раза             |

**When to use:**

Медленный CSS CDN · веб-шрифты · CSS, загружаемый через JS (анти-шаблон).

#### Common pitfalls

- `@import` цепочки, задерживающие CSS.
- Сокрытие тела с `opacity:0` до загрузки (плохо для LCP).
- Загрузка стилей в конце тела.

**Examples:**

```html
<head>
  <link rel="stylesheet" href="/app.css" />
  <link rel="preload" href="/app.css" as="style" />
</head>
```

**Thoughts:** `font-display` и критический CSS → [2_css.md](./2_css.md).

**Sources:** [MDN — FOUC](https://developer.mozilla.org/en-US/docs/Glossary/FOUC)

</details>

<details>
  <summary>Caching (historical note: ApplicationCache)</summary>

### Caching (historical note: ApplicationCache)

**Definition:**

ApplicationCache (appcache) представлял собой автономный API HTML5, который устарел и был удален. Вместо этого используйте Service Workers и заголовки кэша HTTP.

| Подход                       | Статус                              |
| ---------------------------- | ----------------------------------- |
| Кэш приложений               | Устарело                            |
| Работники сферы обслуживания | Современное программное кэширование |
| HTTP `Cache-Control`         | Стандартное кэширование активов     |

**How it works:**

- ApplicationCache (appcache) — удалено; не используйте.
- Современный: сервисные работники + HTTP `Cache-Control`.
- `<link rel="manifest">` для метаданных установки PWA.

| Подход                       | Статус      |
| ---------------------------- | ----------- |
| Кэш приложений               | Устарело    |
| Работники сферы обслуживания | Современный |
| HTTP-кеш                     | Стандарт    |

**When to use:**

Статическое кэширование активов (заголовки HTTP) · автономный SPA (Service Worker).

#### Common pitfalls

- Копирование примеров appcache из старых руководств.
- Нет очистки кеша при развернутом HTML.
- Путаница кеша браузера с пограничным кешем CDN.

**Examples:**

```html
<!-- Modern: link manifest for PWA, HTTP headers for caching -->
<link rel="manifest" href="/manifest.webmanifest" />
```

**Sources:** [MDN — HTTP-кэширование](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching)

</details>

---

<a name="accessibility"></a>
