# Веб-API — Performance

Метрики загрузки, resource hints и browser performance APIs. Обзор Core Web Vitals → [HTML — Производительность](./1_html_performance.md#core-web-vitals).

[← Back to hub](./6_web_api.md)

### Related docs

- [Веб-API — Core](./6_web_api_core.md) · [Веб-API — HTTP](./6_web_api_http.md) · [Веб-API — стили API](./6_web_api_rest.md) · [Веб-API — аутентификация и хранение](./6_web_api_auth.md) · [Веб-API — Приложения](./6_web_api_apps.md) · [Веб-API — Безопасность](./6_web_api_security.md) · [Веб-API — Performance](./6_web_api_performance.md) · [Веб-API — Интервью](./6_web_api_interview.md)

### Sections

- [14. Advanced topics](#14-advanced-topics)
- [API and client performance optimization](#api-and-client-performance-optimization)

---

<a name="14-advanced-topics"></a>

## 14. Advanced topics

<details>
  <summary>Service Workers</summary>

### Service Workers

**Definition:**

Service Worker — это фоновый скрипт, который перехватывает сетевые запросы и обеспечивает автономное кэширование, push-уведомления и фоновую синхронизацию.

| Возможность       | Вариант использования                       |
| ----------------- | ------------------------------------------- |
| Получить перехват | Приложения, ориентированные на офлайн-режим |
| API кэша          | Предварительное кэширование активов         |
| Нажмите API       | Уведомления                                 |

**Examples:**

```js
navigator.serviceWorker.register("/sw.js");

// sw.js
self.addEventListener("fetch", (event) => {
  event.respondWith(
    caches.match(event.request).then((r) => r || fetch(event.request)),
  );
});
```

**Sources:**

[MDN — API Service Worker](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)

---

</details>

<details>
  <summary>Web Workers</summary>

### Web Workers

**Definition:**

Веб-воркеры запускают JavaScript в фоновом потоке, поэтому работа с нагрузкой на ЦП не блокирует основной поток пользовательского интерфейса.

| Тип                | Область применения               |
| ------------------ | -------------------------------- |
| Посвященный        | Скрипт с одним владельцем        |
| Общий              | Несколько скриптов/окон          |
| Сервисный работник | Прокси между приложением и сетью |

**Examples:**

```js
const worker = new Worker("hash-worker.js");
worker.postMessage(largeBuffer);
worker.onmessage = (e) => console.log("hash:", e.data);
```

**Sources:**

[MDN — API веб-работников](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API)

---

</details>

<details>
  <summary>Worklets</summary>

### Worklets

**Definition:**

Рабочие модули — это облегченные контексты параллельного выполнения для конкретных задач, таких как обработка звука (`AudioWorklet`) или живопись (`PaintWorklet`).

| Работа                  | Цель                             |
| ----------------------- | -------------------------------- |
| АудиоWorklet            | DSP аудио в реальном времени     |
| PaintWorklet            | Программное рисование CSS        |
| Анимационный рабочийлет | Анимация, связанная с прокруткой |

**Examples:**

```js
await audioContext.audioWorklet.addModule("processor.js");
const node = new AudioWorkletNode(audioContext, "gain-processor");
```

**Sources:**

[MDN — Worklet](https://developer.mozilla.org/en-US/docs/Web/API/Worklet)

---

</details>

<details>
  <summary>History API</summary>

### History API

**Definition:**

API истории позволяет SPA манипулировать URL-адресом браузера и стеком истории без полной перезагрузки страницы.

| Метод              | Эффект                                 |
| ------------------ | -------------------------------------- |
| `pushState`        | Добавить запись в историю              |
| `replaceState`     | Заменить текущую запись                |
| `popstate` событие | Пользователь перемещается назад/вперед |

**Examples:**

```js
history.pushState({ page: "settings" }, "", "/settings");
window.addEventListener("popstate", (e) => {
  renderPage(e.state?.page ?? "home");
});
```

**Sources:**

[MDN — API истории](https://developer.mozilla.org/en-US/docs/Web/API/History_API)

---

</details>

<details>
  <summary>TCP three-way handshake</summary>

### TCP three-way handshake

**Definition:**

TCP открывает соединение с помощью SYN → SYN-ACK → ACK, поэтому обе стороны согласовывают начальные порядковые номера перед потоком байтов.

| Шаг | Пакет         | Значение                                 |
| --- | ------------- | ---------------------------------------- |
| 1   | СИН           | ISN клиента `x`                          |
| 2   | СИН-АКТ       | ISN сервера `y`, ок `x + 1`              |
| 3   | ПОДТВЕРЖДЕНИЕ | Подтверждение клиента `y + 1`; подключен |

Полное сравнение UDP, TFO и `initcwnd`, `preconnect`, STUN/TURN → [TCP против UDP](./6_web_api_interview.md#tcp-vs-udp).

**Examples:**

```
Client → SYN (seq=100)
Server → SYN-ACK (seq=300, ack=101)
Client → ACK (ack=301)
→ Data can flow both ways
```

**Sources:**

[MDN — TCP-рукопожатие](https://developer.mozilla.org/en-US/docs/Glossary/TCP_handshake)

---

</details>

<details>
  <summary>Throttling & debouncing</summary>

### Throttling & debouncing

**Definition:** Шумные обработчики ограничения скорости. Полные примечания и примеры: [Debounce](./3_js_browser_extra.md#debounce) · [Throttle](./3_js_browser_extra.md#throttle).

**Sources:** [Глоссарий MDN — Debounce](https://developer.mozilla.org/en-US/docs/Glossary/Debounce) · [Глоссарий MDN — Throttle](https://developer.mozilla.org/en-US/docs/Glossary/Throttle)

</details>

<details>
  <summary>Axios interceptors</summary>

### Axios interceptors

**Definition:**

Перехватчики Axios — это перехватчики промежуточного программного обеспечения, которые преобразуют или проверяют запросы и ответы до того, как они достигнут кода вашего приложения.

| Перехватчик | Типичное использование                               |
| ----------- | ---------------------------------------------------- |
| Запрос      | Прикрепите токен аутентификации, войдите             |
| Ответ       | Развертывание данных, обработка ошибки 401 глобально |

**Examples:**

```js
import axios from "axios";

axios.interceptors.request.use((config) => {
  config.headers.Authorization = `Bearer ${getToken()}`;
  return config;
});

axios.interceptors.response.use(
  (res) => res,
  (err) => {
    if (err.response?.status === 401) logout();
    return Promise.reject(err);
  },
);
```

**Sources:**

[Аксиос — Перехватчики](https://axios-http.com/docs/interceptors)

---

</details>

<details>
  <summary>Progressive rendering</summary>

### Progressive rendering

**Definition:**

Прогрессивная рендеринг отображает содержательный контент до завершения загрузки всех ресурсов, что повышает воспринимаемую производительность.

| Техника                            | Эффект                               |
| ---------------------------------- | ------------------------------------ |
| Ленивые картинки(`loading="lazy"`) | Отложить изображения за кадром       |
| Критический CSS                    | Встроенные стили над сгибом          |
| Разделение кода                    | Загрузка фрагментов JS по требованию |

**Examples:**

```html
<img src="hero.jpg" loading="lazy" alt="Hero" />
<link rel="preload" href="font.woff2" as="font" crossorigin />
```

**Sources:**

[web.dev — быстрая загрузка](https://web.dev/explore/fast)

---

</details>

<details>
  <summary>SPA, SSR, PWA & SSG</summary>

### SPA, SSR, PWA & SSG

**Definition:** Полная таксономия (статическая/динамическая MPA, SPA, CSR, SSR, SSG, PWA, сравнение) находится в [Типы веб-приложений](./6_web_api_apps.md#12-types-of-web-apps).

**How it works:** SPA = одна страница, маршруты JS. CSR = первая отрисовка в браузере. SSR = первый HTML с сервера. SSG = HTML при сборке. **PWA** = прогрессивное улучшение (установка + API). Установка Chromium = HTTPS + манифест; Service Worker предназначен для автономного режима/принудительной отправки и не требуется для приглашения на установку. Полные примечания: [PWA](./6_web_api_apps.md#pwa-progressive-web-app).

**Examples:**

```json
{
  "name": "My App",
  "start_url": "/",
  "display": "standalone",
  "icons": [{ "src": "/icon-192.png", "sizes": "192x192" }]
}
```

**Sources:**

[Doka — Типы веб-приложений](https://doka.guide/tools/web-app-types/) · [MDN — Progressive Web Apps](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps)

---

</details>

<details>
  <summary>LCP (Largest Contentful Paint)</summary>

### LCP (Largest Contentful Paint)

**Definition:**

LCP — это время от начала навигации до момента рисования **самого большого элемента контента в области просмотра**. Браузер выбирает элемент (главное изображение, обложку статьи, большой текстовый блок, видеопостер) — вручную его назначить нельзя. Проверьте его в PageSpeed ​​Insights или на панели «Производительность».

Хорошо: **≤ 2,5 с**. Плохо: **> 4 с**. Поле «проходит» — p75 (см. [Core Web Vitals](./1_html_performance.md#core-web-vitals)).До тех пор, пока это не произойдет, пользователь не сможет сказать, что попал на правильную страницу; медленный LCP повышает риск отскока.

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

**See also:** [Core Web Vitals](./1_html_performance.md#core-web-vitals) · [TTFB](#ttfb-time-to-first-byte) · [FCP](#first-contentful-paint-fcp) · [INP](#inp-interaction-to-next-paint) · HTML → [1_html.md](./1*html* Performance.md#core-web-vitals--html-developer-impact) · [img](./1_html_media.md#img-element-and-Response-images) · [DevTools](./11_vsc_browser.md#devtools--core-web-vitals)

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

**See also:** [Core Web Vitals](./1_html_performance.md#core-web-vitals) · [LCP](#lcp-largest-contentful-paint) · [TTFB](#ttfb-time-to-first-byte) · [INP](#inp-interaction-to-next-paint) · [preload, prefetch, preconnect](#preload-prefetch-preconnect--prerender) · [HTML — first краска](./1_html_rendering.md#first-render--full-pipeline)

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
- Сервис-воркер **stale- while-revalidate** может немедленно вернуть оболочку приложения из кэша; **не** используйте его для персонализированного HTML-кода для каждого запроса (это может привести к устаревшему документу). См. раздел [Сервисные работники](#service-workers).
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

**See also:** [Core Web Vitals](./1_html_performance.md#core-web-vitals) · [LCP](#lcp-largest-contentful-paint) · [FCP](#first-contentful-paint-fcp) · [INP](#inp-interaction-to-next-paint) · [CDN](./6_web_api_core.md#cdn-content-delivery-network)

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

**See also:** [Core Web Vitals](./1_html_performance.md#core-web-vitals) · [LCP](#lcp-largest-contentful-paint) · [4_react.md — Web Vitals](./4_react_ecosystem.md#web-vitals-in-react-apps) · [4_nextjs.md — CWV](./4_nextjs_optimizations.md#core-web-vitals-on-vercel)

**Sources:**

[web.dev — INP](https://web.dev/articles/inp) · [web.dev — Оптимизация длительных задач](https://web.dev/articles/optimize-long-tasks) · [MDN — Scheduler.yield](https://developer.mozilla.org/en-US/docs/Web/API/Scheduler/yield) · [React `<Activity>`](https://react.dev/reference/react/Activity)

---

</details>

<details>
  <summary>preload, prefetch, preconnect & prerender</summary>

### preload, prefetch, preconnect & prerender

**Definition:**

Подсказки к ресурсам сообщают браузеру о необходимости раннего доступа или подключения, ускоряя текущую или будущую навигацию.

| Подсказка    | Когда             | Цель                                     |
| ------------ | ----------------- | ---------------------------------------- |
| `preload`    | Текущая страница  | Критический ресурс необходим сейчас      |
| `prefetch`   | Будущая навигация | Вероятно, следующий ресурс страницы      |
| `preconnect` | Ранний            | DNS + TCP + TLS к источнику              |
| `prerender`  | Будущая страница  | Отобразить всю страницу в фоновом режиме |

**Examples:**

```html
<link rel="preload" href="/app.js" as="script" />
<link rel="prefetch" href="/next-page.js" as="script" />
<link rel="preconnect" href="https://fonts.googleapis.com" />
```

**Sources:**

[MDN — rel=preload](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel/preload)

---

</details>

<details>
  <summary>PRPL pattern</summary>

### PRPL pattern

**Definition:**

PRPL (Push, Render, Pre-cache, Lazy-load) — это шаблон производительности для быстрых интерактивных веб-приложений.

| Шаг                         | Цель                                          |
| --------------------------- | --------------------------------------------- |
| Нажмите                     | Отправляйте критически важные ресурсы заранее |
| Рендеринг                   | Показать контент как можно скорее             |
| Предварительное кэширование | Кэшировать наиболее необходимые ресурсы       |
| Ленивая загрузка            | Отложить некритичный код/активы               |

**Examples:**

```html
<!-- Push critical route -->
<link rel="preload" href="/route-home.js" as="script" />
<!-- Lazy-load below-fold images -->
<img loading="lazy" src="/gallery/photo-9.jpg" />
```

**Sources:**

[web.dev — шаблон PRPL](https://web.dev/articles/apply-instant-loading-with-prpl)

---

---

</details>

<a name="api-and-client-performance-optimization"></a>

<details>
  <summary>API and client performance optimization</summary>

### API and client performance optimization

**Definition:** Сквозная оптимизация **backend API** (задержка, пропускная способность, размер ответа) и **клиента** (бандл, main thread, воспринимаемая скорость). Сначала меряйте в production; чините самое медленное звено на критическом пути.

**How it works:**

| Слой                    | Типичные узкие места                          | Рычаги                                                                                                                  |
| ----------------------- | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **API / origin**        | Медленная БД, N+1, cold start, тяжёлый JSON   | Индексы, eager loading, pagination/cursors, выбор полей, compression, cache headers, Redis, async jobs, `Server-Timing` |
| **Сеть**                | Waterfall, много round trip, нет CDN          | HTTP/2+, параллельные fetch, BFF/агрегация, CDN/edge cache, `preconnect` / `preload`                                    |
| **Клиентский runtime**  | Long tasks, лишние re-render, огромные списки | Code splitting, lazy routes/components, memo/virtualization, отложить third-party JS                                    |
| **Восприятие загрузки** | Поздний LCP/FCP, высокий INP/CLS              | SSR/SSG/RSC, оптимизация image/font, стабильный layout, дробление работы (`scheduler.yield`)                            |

Workflow: **профилирование** (RUM + APM/traces + Lighthouse/DevTools) → p95/p99 → одно узкое место → **регресс-контроль** (бюджеты, CI Lighthouse, load tests).

**When to use:** Пункты JD вроде «оптимизировать производительность API и клиентской части», медленные дашборды, высокий TTFB, плохие Core Web Vitals в Search Console, p99 API выше SLO.

**Pluses and minuses:**

| Подход                            | Плюс                              | Минус                                   |
| --------------------------------- | --------------------------------- | --------------------------------------- |
| Кэш (HTTP/Redis/CDN)              | Большой выигрыш на read-heavy API | Устаревание данных, сложная инвалидация |
| Pagination + sparse JSON          | Меньше payload, быстрее TTFB      | Больше логики на клиенте                |
| Memo/code split на клиенте        | Лучше INP, меньше бандл           | Сложнее отладка, легко пере-мemo        |
| Преждевременная микро-оптимизация | —                                 | Потеря времени без метрик               |

**Thoughts:** CWV клиента и latency API связаны — медленный TTFB блокирует FCP/LCP; тяжёлый client JS бьёт по INP даже при быстром API. Полные client-playbook → [§10 Performance](./4_react_ecosystem.md#10-performance) · [Next optimizations](./4_nextjs_optimizations.md) · БД/запросы → [N+1](./9_database_orm.md#n1-problem) · [Connection pooling](./9_database_orm.md#connection-pooling) · Node → [8_nodejs interview Q29](./8_nodejs_interview.md#interview-practice).

**Sources:** [web.dev — Vitals](https://web.dev/articles/vitals) · [web.dev — Optimize TTFB](https://web.dev/articles/optimize-ttfb) · [MDN — Server-Timing](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Server-Timing) · [web.dev — Optimize long tasks](https://web.dev/articles/optimize-long-tasks)

---

</details>
