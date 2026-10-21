# Core — Browser platform

**Browser work principles** и как современные браузеры выполняют страницы: навигация, pipeline рендеринга, JavaScript на main thread, многопроцессная изоляция и безопасность.

[← Core index](./0_core.md)

### Related docs

- [Accessibility](./1_html_a11y.md) · [Web security](./7_special_security.md) · [Browser security](./6_web_api_security.md)
- [HTML — Rendering (этапы CRP)](./1_html_rendering.md#from-url-to-pixels) · [JS — Browser environment](./3_js_browser.md#browser-environment) · [JS — Language (V8)](./3_js_core.md#v8-engine-architecture) · [Event loop (полный)](./3_js_async_event_loop.md#event-loop) · [JS — Browser hub](./3_js_browser_hub.md)

### Main concepts

| Area           | Topics                                                                              |
| -------------- | ----------------------------------------------------------------------------------- |
| **Principles** | 8 подтем: navigation → DOM → CSSOM → layout → JS → event loop → processes → sandbox |
| **Processes**  | Browser, renderer, GPU; site isolation                                              |
| **Engine**     | V8, JIT, generational GC                                                            |
| **Rendering**  | DOM/CSSOM, main thread; этапы → [HTML — Rendering](./1_html_rendering.md)           |
| **Security**   | Same-origin, CORS, XSS, CSP                                                         |
| **Deep dives** | CRP по шагам (HTML) · V8/GC (JS Core) · event loop (JS Async)                       |

### Sections

- [Browser work principles](#browser-work-principles)
- [Browser platform](#browser-platform)

---

<a name="browser-work-principles"></a>

## Browser work principles

<details>
  <summary>Browser work principles</summary>

### Browser work principles

**Definition:** **Browser work principles** — устойчивые правила того, как **user agent** загружает, рендерит, выполняет скрипты и изолирует сайты. Для глубины на собеседовании раскрывайте подтемы ниже.

**How it works:**

| Step | Subtopic                                                                | Focus                    |
| ---- | ----------------------------------------------------------------------- | ------------------------ |
| 1    | [Navigation and fetching](#navigation-and-fetching)                     | URL → response bytes     |
| 2    | [HTML parsing and the DOM](#html-parsing-and-the-dom)                   | Markup → live tree       |
| 3    | [CSSOM and style computation](#cssom-and-style-computation)             | Cascade → computed style |
| 4    | [Layout, paint, and compositing](#layout-paint-and-compositing)         | Geometry → GPU frame     |
| 5    | [JavaScript engine and main thread](#javascript-engine-and-main-thread) | ECMAScript + DOM APIs    |
| 6    | [Event loop and task queues](#event-loop-and-task-queues)               | Microtasks vs macrotasks |
| 7    | [Multi-process architecture](#multi-process-architecture)               | Renderer isolation       |
| 8    | [Sandbox and same-origin policy](#sandbox-and-same-origin-policy)       | Trust boundaries         |

```text
URL → fetch → parse HTML → DOM ──┐
       ↓                         ├→ render tree → layout → paint → composite
     fetch CSS → CSSOM ──────────┘
                       ↑ JS on main thread (mutates → style/layout)
```

**When to use:** Формулировки вроде «понимание принципов работы браузеров» — сначала обзор, затем каждый accordion.

**Thoughts:** Эта страница — карта **Frontend → Core**. Длинные эссе — в sibling-топиках, здесь их не дублируем.

| Topic                          | Canonical file                                                                                                                 |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------ |
| URL → pixels (DNS…composite)   | [HTML — Rendering](./1_html_rendering.md#from-url-to-pixels)                                                                   |
| Layout/paint, dirty nodes      | [HTML — Rendering](./1_html_rendering.md#layout-reflow) · [Progressive rendering](./1_html_rendering.md#progressive-rendering) |
| `window`, spec layers, workers | [JS Browser — Document](./3_js_browser.md#browser-environment)                                                                 |
| V8, JIT, GC                    | [JS Core — V8](./3_js_core.md#v8-engine-architecture)                                                                          |
| Microtasks, macrotasks, rAF    | [JS Async — Event loop](./3_js_async_event_loop.md#event-loop)                                                                 |

**Sources:** [MDN — How browsers work](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web)

</details>

<a name="navigation-and-fetching"></a>

<details>
  <summary>Navigation and fetching</summary>

### Navigation and fetching

**Definition:** **Navigation** — всё от открытия URL до момента, когда у браузера есть **поток HTTP-ответа** для парсинга: DNS, TCP/TLS, HTTP, редиректы и **кэширование**.

**How it works:**

| Phase        | What happens                                                     |
| ------------ | ---------------------------------------------------------------- |
| **URL**      | Scheme, host, port, path, query; `#fragment` остаётся на клиенте |
| **DNS**      | Host → IP (hosts file → кэш браузера → ОС → resolver)            |
| **Connect**  | TCP handshake; TLS для HTTPS (проверка сертификата)              |
| **HTTP**     | Заголовки и тело ответа; cookies прикрепляются                   |
| **Cache**    | Memory/disk cache может обойти сеть (`Cache-Control`, `ETag`)    |
| **Redirect** | `3xx` + `Location` запускает новую навигацию                     |

**Navigation kinds (same tab):**

| Kind               | What changes                        | Typical API / trigger                         |
| ------------------ | ----------------------------------- | --------------------------------------------- |
| **Cross-document** | Новый document в renderer (часто)   | Клик `<a href>`, `location.assign`, `301/302` |
| **Same-document**  | URL/history без полной перезагрузки | `#hash`, `history.pushState`, `popstate`      |
| **Reload**         | Revalidate или обход кэша           | F5, `location.reload()`                       |
| **Back/forward**   | Может восстановить из **bfcache**   | Навигация по history                          |

- **Document vs subresource:** навигация HTML управляет жизненным циклом **main frame**; `<img>`, scripts и `fetch()` — отдельные запросы со своими ключами кэша и приоритетами.
- **HTTP/2+:** одно соединение **мультиплексирует** потоки (меньше TCP/TLS handshakes); **HTTP/3** — QUIC поверх UDP; в DevTools **Network** всё равно видны TTFB и download.
- **Early hints (`103`):** сервер может отдать **preload** link headers до полного тела ответа — снижает latency критического пути, где поддерживается.
- **Preload scanner:** пока HTML-парсер заблокирован sync-скриптом, вторичный сканер находит `<link rel="stylesheet">`, `<script src>`, `<img>` и раньше стартует fetch.
- **bfcache** восстанавливает страницу из памяти при back/forward (не каждая страница подходит — `unload`, `Cache-Control: no-store` и т.д. могут отключить).
- **Prefetch / preconnect** (`rel=preconnect`, `dns-prefetch`, `modulepreload`) → [HTML — Performance](./1_html_performance.md).
- **Service Worker** может перехватывать навигации и `fetch` → [Web API](./6_web_api.md).

**Examples:** **Performance** → **Navigation timing**: `domainLookupStart` → `connectStart` → `requestStart` → `responseStart` (TTFB) → `domInteractive` отделяет сеть от парсинга.

**When to use:** Медленный TTFB, устаревший документ, циклы редиректов, «мягкая» SPA-навигация vs полная перезагрузка, waterfall в DevTools.

#### Common pitfalls

Игнорировать стоимость установки соединения. Считать, что каждая навигация обходит кэш.

**See also:** [From URL to pixels](./1_html_rendering.md#from-url-to-pixels) · [HTTP caching](./6_web_api_http.md)

**Sources:** [MDN — HTTP caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching) · [Navigation API](https://developer.mozilla.org/en-US/docs/Web/API/Navigation_API)

</details>

<a name="html-parsing-and-the-dom"></a>

<details>
  <summary>HTML parsing and the DOM</summary>

### HTML parsing and the DOM

**Definition:** **HTML parser** строит живое **DOM**-дерево из байт. Скрипты и CSS работают с этим деревом — не с исходным файлом разметки.

**How it works:**

- **Pipeline:** байты → **tokenizer** (теги, текст, комментарии) → **tree builder** (open elements, foster parenting) → живые **DOM**-узлы с JS bindings.
- **Streaming parse:** DOM растёт, пока HTML ещё качается; движок может **layout и paint инкрементально** до `</html>`.
- **Error recovery:** битая разметка чинится по правилам HTML (auto-closing, foster parenting для `<table>`) — в проде не полагайтесь на «безопасный» провал.
- **Blocking stylesheet:** `<link rel="stylesheet">` в `<head>` блокирует **выполнение script** ниже, пока лист не загружен и CSSOM не построен (если `media` не совпадает или `disabled` — исключения).
- **Script impact on parse:**

| Script type                 | Parse blocking?                 | Run order                          |
| --------------------------- | ------------------------------- | ---------------------------------- |
| Sync `<script src>` in body | Да, на fetch+run                | Порядок в document                 |
| `defer`                     | Нет (кроме порядка между defer) | После parse, до `DOMContentLoaded` |
| `async`                     | Только на время выполнения      | Кто первый готов                   |
| `type="module"`             | По умолчанию deferred           | Порядок графа модулей              |

- **`DOMContentLoaded`:** HTML распарсен; defer/module скрипты отработали — можно вешать UI, которому нужен полный DOM (не все картинки).
- **`load`:** подресурсы (images, stylesheets, iframes) загружены — «страница готова» для метрик.
- **`document.write`:** во время parse может вставить разметку; после завершения parse может **заменить** весь document — legacy, избегать.

**Examples:**

```html
<!-- Blocks parse + first paint until downloaded and executed -->
<script src="/heavy-sync.js"></script>
<!-- Parser continues; runs in order after parse -->
<script defer src="/app.js"></script>
```

**When to use:** Пустой экран до бандла, парсер заблокирован sync-скриптом в `<head>`, «мигание» до hydration.

#### Common pitfalls

Крупный sync-скрипт до first paint. `document.write` после load (может стереть document).

**See also:** [Script loading](./3_js_browser.md#script-loading) · [DOM tree](./3_js_browser.md#dom-tree)

**Sources:** [HTML parsing](https://html.spec.whatwg.org/multipage/parsing.html) · [MDN — DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)

</details>

<a name="cssom-and-style-computation"></a>

<details>
  <summary>CSSOM and style computation</summary>

### CSSOM and style computation

**Definition:** **CSSOM** хранит правила stylesheet. Движок **каскадирует** (origin, importance, specificity, order), затем **вычисляет** used values для каждого элемента.

**How it works:**

- Источники: `<link rel="stylesheet">`, `<style>`, inline `style=""`, `@import` (виден только после загрузки родительского листа — избегайте длинных цепочек).
- **Cascade (упрощённо):** origin (author / user / browser) → порядок **`@layer`** → importance (`!important`) → **specificity** → source order.
- **Render-blocking:** CSS в `<head>` часто задерживает **first render**, пока нет нужного CSSOM — **critical CSS** inline или split бандлов сокращает окно блокировки.
- **Render tree:** DOM + computed styles; поддеревья с `display: none` (и аналогами) обычно не layout; `visibility: hidden` в layout участвует.
- **Computed vs used:** наследование и `%`/`em` дают **computed**; **used** — то, что читает layout (например used `width` после flex/grid).
- **Invalidation:** смена class/DOM/style помечает dirty nodes; пересчёт **инкрементальный**, не full-page на каждый символ.
- **Performance hooks:** `content-visibility: auto` пропускает off-screen layout/paint; `contain: layout style paint` ограничивает радиус invalidation.

**Examples:** Смена `element.className` → **recalc style** → возможно **layout** → **paint** → **composite** в DevTools **Performance**.

**When to use:** FOUC, войны specificity/`@layer`, critical CSS, «почему победил этот селектор?».

#### Common pitfalls

Цепочки `@import` откладывают discovery CSS. Анимация layout-heavy свойств каждый кадр.

**See also:** [CSS Core](./2_css_core.md) · [Critical rendering path](./1_html_rendering.md#first-render--full-pipeline)

**Sources:** [MDN — CSSOM](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Object_Model)

</details>

<a name="layout-paint-and-compositing"></a>

<details>
  <summary>Layout, paint, and compositing</summary>

### Layout, paint, and compositing

**Definition:** **Layout** (reflow) считает геометрию; **paint** заливает пиксели; **compositing** сливает **layers** на GPU.

**How it works:**

| Stage         | Triggers (examples)                                | Cost   |
| ------------- | -------------------------------------------------- | ------ |
| **Layout**    | Вставка DOM, смена текста, read метрик после write | High   |
| **Paint**     | Цвет, текст, тени в затронутой области             | Medium |
| **Composite** | `transform`, `opacity` на promoted layer           | Lower  |

- **Layout outputs:** box geometry, **fragment** trees для inline; **flex/grid** в современных движках до block flow.
- **Paint:** **display lists** (текст, фоны, borders, shadows) по слоям; **repaint** без layout, если геометрия не менялась.
- **Layers / compositing:** promoted layers (`will-change: transform`, `<video>`, `<canvas>`, часть анимаций) на GPU; **composite** каждый кадр.
- **Stacking contexts:** `z-index`, `opacity`, `transform`, `isolation` — порядок групп; «z-index только среди siblings в одном context».
- **Layout thrashing:** чередование read (`offsetWidth`, `getBoundingClientRect`) и write в цикле форсирует sync layout — **сначала все read, потом write** (или read в `requestAnimationFrame` после write прошлого кадра).
- **CLS:** неожиданные сдвиги — резервируйте место (`width`/`height`, `aspect-ratio`) и шрифты (`font-display`, preload).
- **CRP:** меньше blocking CSS/JS для first paint → [First render — full pipeline](./1_html_rendering.md#first-render--full-pipeline).
- **Incremental layout:** **dirty** поддеревья вместо full-page reflow → [Global vs incremental layout](./1_html_rendering.md#global-vs-incremental-layout-dirty-nodes).
- **Progressive rendering:** paint до полной загрузки document → [Progressive rendering](./1_html_rendering.md#progressive-rendering).
- **`requestAnimationFrame`:** перед следующим **paint**; для визуальных обновлений; всё ещё main thread.

**When to use:** Jank, настройка анимаций, Core Web Vitals (LCP/CLS/INP), DevTools **Rendering** → «Layer borders».

**Examples:**

```js
const w = els.map((el) => el.offsetWidth);
els.forEach((el, i) => {
  el.style.width = w[i] + 10 + "px";
});
```

**Sources:** [MDN — Rendering](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Rendering) · [web.dev — Rendering performance](https://web.dev/articles/rendering-performance)

</details>

<a name="javascript-engine-and-main-thread"></a>

<details>
  <summary>JavaScript engine and main thread</summary>

### JavaScript engine and main thread

**Definition:** **JS engine** (V8, SpiderMonkey, JavaScriptCore) выполняет ECMAScript на **main thread** renderer вместе с parse, style и layout — пока не выносите в **Web Workers**.

**How it works:**

- **Compile pipeline:** parse → bytecode → **JIT optimized code** (inline caches, hidden classes) → иногда **deopt** в interpreter при смене типов → [V8 architecture](./3_js_core.md#v8-engine-architecture).
- **Realms:** у каждого **window** / **iframe** origin свой изолированный **realm**; same-origin iframes делят правила browsing context — безопасность на host bindings.
- **Host bindings:** `document`, `fetch`, timers, DOM API — мост в C++ (Blink/WebKit/Gecko); переход дорогой — батчите DOM.
- **Long tasks:** >50 ms JS на main thread задерживает input и кадры → плохой **INP**; дробите (`scheduler.postTask`, chunks, workers).
- **Workers:** **Dedicated** / **Shared** workers off-thread; `postMessage` + **Transferable**; **без DOM** — **OffscreenCanvas** для части графики.
- **WebAssembly:** тот же renderer process, sandboxed linear memory — не второй event loop на main thread, пока не `await` обратно в JS.
- **GC:** generational collectors; **GC pauses** дают jank → [Garbage collection](./3_js_objects.md#garbage-collection).

**Examples:** DevTools **Performance** — фиолетовый «Scripting» при input = contention main thread; **Coverage** — неиспользуемый JS на cold load.

**When to use:** Профилирование main thread, вынос CPU, объяснение «async ≠ параллельность на main thread».

#### Common pitfalls

Тяжёлые вычисления с `async/await` всё ещё на main thread. Утечки через detached DOM + closures.

**See also:** [Event loop](#event-loop-and-task-queues) · [Browser environment](./3_js_browser.md#browser-environment)

**Sources:** [MDN — JS engines](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/JavaScript_technologies_overview#javascript_engines)

</details>

<a name="event-loop-and-task-queues"></a>

<details>
  <summary>Event loop and task queues</summary>

### Event loop and task queues

**Definition:** **Event loop** планирует JS на main thread: выполнить script → drain **microtasks** → один **macrotask** → (возможно) render → повтор.

**How it works:**

| Kind           | Examples                                                                                                 |
| -------------- | -------------------------------------------------------------------------------------------------------- |
| **Microtasks** | `Promise.then`, `queueMicrotask`, `MutationObserver`                                                     |
| **Macrotasks** | `setTimeout`, `setInterval`, I/O, user input, `postMessage`, колбэки `requestAnimationFrame` (как tasks) |
| **Rendering**  | Между macrotasks браузер может **style/layout/paint**, если нужен кадр                                   |

**One turn (упрощённо):**

1. Выполнять JS, пока call stack пуст не станет (до опустошения стека).
2. **Полностью drain microtasks** (каждый `Promise.then` может добавить ещё).
3. Один **macrotask** из очереди.
4. Опционально **update rendering** (rAF перед paint кадра).
5. Повтор.

```js
console.log("1");
setTimeout(() => console.log("4"), 0);
Promise.resolve().then(() => console.log("3"));
console.log("2");
// 1 → 2 → 3 → 4
```

```js
// Microtask starvation — blocks paint and input until loop ends
function spin() {
  queueMicrotask(spin);
}
spin();
```

- **`async/await`:** каждый `await` продолжается через **microtasks** — длинные async-цепи всё равно монополизируют main thread между macrotasks.
- **Node vs browser:** Node добавляет фазы **libuv**; **browser** связывает rendering с **HTML event loop** — порядок SSR vs client не идентичен.

**When to use:** Баги порядка задач; голодание UI microtask-циклом; «почему DOM не обновился до моего log?».

**See also:** [Event loop (full)](./3_js_async_event_loop.md#event-loop)

**Sources:** [MDN — Event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop)

</details>

<a name="multi-process-architecture"></a>

<details>
  <summary>Multi-process architecture</summary>

### Multi-process architecture

**Definition:** Браузер делит работу на **processes** для восстановления после crash и **site isolation**.

**How it works:**

| Process      | Role                                                         |
| ------------ | ------------------------------------------------------------ |
| **Browser**  | UI chrome, profile, permissions, координация renderers       |
| **Renderer** | DOM, JS, layout, paint (на **site** / **site group**)        |
| **GPU**      | Compositing, GL/Vulkan для layers                            |
| **Network**  | HTTP stack, cookies, cache (Chromium — не во всех браузерах) |

- **Site isolation:** cross-site страницы в **отдельных renderer processes** (или строгих **site groups**) — скомпрометированный renderer не читает память другого origin.
- **OOPIF:** **out-of-process iframes** — cross-site `<iframe>` может быть в своём renderer; кооперация через `postMessage` / `MessageChannel`; parent не трогает cross-origin DOM.
- **IPC:** процессы общаются через **Mojo** / IPC — handles и сериализованные сообщения; минимизируйте cross-frame chatter.
- **Tab crash:** падает renderer; browser process жив — «Aw, snap!»; другие вкладки работают.
- **Utility / plugin** (зависит от vendor): audio, PDF, extensions — дополнительные границы изоляции в Chromium-подобных стеках.

**When to use:** «Почему не могу прочитать DOM iframe?»; архитектура безопасности; память на вкладку в Task Manager.

**See also:** [Browser platform architecture](#browser-platform-architecture)

**Sources:** [Chromium multi-process](https://www.chromium.org/developers/design-documents/multi-process-architecture/) · [Site isolation](https://www.chromium.org/Home/chromium-security/site-isolation/)

</details>

<a name="sandbox-and-same-origin-policy"></a>

<details>
  <summary>Sandbox and same-origin policy</summary>

### Sandbox and same-origin policy

**Definition:** **Sandbox** ограничивает доступ web-контента к ОС. **Same-origin policy** ограничивает cross-origin DOM/storage. **Origin** = scheme + host + port.

**How it works:**

- **SOP:** нельзя читать DOM, `localStorage` и большинство свойств cross-origin `Window`/`Document` из вашего script.
- **CORS:** заголовки сервера разрешают **чтение** cross-origin **HTTP responses** в JS (`fetch`/XHR) — браузер **отправляет** запрос; без `Access-Control-Allow-Origin` тело может быть скрыто от script.
- **`<iframe sandbox>`:** токены (`allow-scripts`, `allow-same-origin`, …) снимают возможности даже у same-origin embed при правильной комбинации.
- **Permissions Policy** (заголовок `Permissions-Policy`): отключает API (camera, geolocation, `payment`) per frame или origin.
- **Mixed content:** активные HTTP-подресурсы на HTTPS блокируются или upgrade — целостность secure pages.
- **Cross-origin isolation:** `Cross-Origin-Opener-Policy` + `Cross-Origin-Embedder-Policy` → `SharedArrayBuffer` и жёстче timing isolation → [Web API security](./6_web_api_security.md).
- **CSP / XSS / cookies:** → [Security model overview](#security-model-overview).

**Examples:** `fetch('https://api.other.com')` с `https://app.example` — сеть может пройти, JS упадёт без CORS headers; `<iframe>` с `https://bank.com` не даёт вашей странице читать его DOM.

**When to use:** Threat modeling; ошибки CORS в консоли; sandbox виджетов; почему cross-origin `<iframe>` не читает parent.

**Sources:** [MDN — SOP](https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Same-origin_policy)

</details>

<a name="browser-platform"></a>

## Browser platform

<details>
  <summary>Browser platform architecture</summary>

### Browser platform architecture

**Definition:** **Browser** — многопроцессный хост: HTML/CSS/JS одного сайта обычно в **renderer** process с **JavaScript engine** (V8 в Chrome/Edge). Parse, layout и paint делят **main thread** renderer с вашими скриптами.

**How it works:**

| Layer                 | Role                                          | Deep dive                                                                                                               |
| --------------------- | --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **Browser process**   | Вкладки, permissions, координация сети        | [Chromium multi-process](https://www.chromium.org/developers/design-documents/multi-process-architecture/)              |
| **Renderer + engine** | DOM, JS, layout, paint; **isolate** per realm | [Browser environment](./3_js_browser.md#browser-environment) · [V8 architecture](./3_js_core.md#v8-engine-architecture) |
| **GPU process**       | Compositing layers на экран                   | [HTML — Rendering](./1_html_rendering.md)                                                                               |
| **Scheduling**        | Sync → microtasks → macrotasks → render       | [Event loop](./3_js_async_event_loop.md#event-loop)                                                                     |

- **Site isolation:** отдельные renderer processes (или strict site groups) — скомпрометированная вкладка не читает память другого сайта.
- **Не одна подсистема:** V8 выполняет ECMAScript; **Blink** (Chromium) строит DOM и layout — на собеседовании называйте оба.

**When to use:** База **Core** до a11y/security; вопросы «как работает браузер» на стыке HTML, JS и HTTP.

#### Common pitfalls

Винить **V8** в layout-багах. Считать **CORS** политикой DOM (он решает, может ли **page JS** прочитать cross-origin **HTTP** response). Забывать, что длинный JS блокирует parse и paint на main thread.

**Pluses and minuses:**

| +                                             | −                                           |
| --------------------------------------------- | ------------------------------------------- |
| Одна карта для HTML, JS и network на интервью | Детали engine/process различаются по vendor |
| Ссылки на полные эссе по API и атакам         | Не замена практике в DevTools               |

**Thoughts:** Начните с подтем [Browser work principles](#browser-work-principles). Соседи Core: [Accessibility](./1_html_a11y.md) · [Web security](./7_special_security.md) · [JS Browser](./3_js_browser_hub.md).

**Sources:** [MDN — How browsers work](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web) · [V8.dev](https://v8.dev/)

</details>

<details>
  <summary>Web platform stack (HTML, CSS, JavaScript)</summary>

### Web platform stack (HTML, CSS, JavaScript)

**Definition:** **Web platform** делит роли: **HTML** — структура и смысл; **CSS** — представление; **JavaScript** — поведение и I/O со страницы.

**How it works:**

| Layer          | Spec / role                  | Runtime artifact                               |
| -------------- | ---------------------------- | ---------------------------------------------- |
| **HTML**       | Семантика, формы, media      | **DOM** + accessibility tree                   |
| **CSS**        | Cascade, layout modes, paint | **CSSOM** + used styles                        |
| **JavaScript** | Logic, fetch, workers, WASM  | V8/SpiderMonkey/JSC на main thread или workers |

- Parser строит **DOM** из HTML; CSS — **CSSOM**; scripts меняют дерево через **host bindings** (не перепарсивая исходники на каждое изменение).
- **Web APIs** (fetch, storage, observers) — **не** ECMAScript; объекты браузера, подключённые к JS engine.
- Scripts с `defer`, `async`, `type="module"` по-разному влияют на parse и first paint → [HTML — Performance](./1_html_performance.md).
- **Progressive enhancement:** осмысленный HTML первым; CSS и JS улучшают — согласуется с [Accessibility](./1_html_a11y.md) (работа без JS где возможно).

**When to use:** Как разметка, стили и логика сходятся на **Core** до attack surfaces.

**See also:** [HTML — Core](./1_html_core.md#web-platform-layers) · [JS — Core language](./3_js_core.md)

**Sources:** [MDN — How the web works](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/How_the_Web_works)

</details>

<a name="security-model-overview"></a>

<details>
  <summary>Security model overview</summary>

### Security model overview

**Definition:** Браузер **sandbox**-ит страницы: ограниченный доступ к файлам/ОС, **same-origin** для DOM и storage, HTTP-механизмы (**CORS**, **CSP**, флаги cookies) вместе с безопасной разметкой и JS.

**How it works:**

| Control                | Stops / limits                                                   | Topic                                                      |
| ---------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------- |
| **Same-origin policy** | Cross-origin чтение DOM                                          | [CORS / SOP](./3_js_browser_extra.md#cors-same-origin)     |
| **CORS**               | Cross-origin **response read** из `fetch`/XHR без opt-in сервера | [Web API — CORS](./6_web_api_security.md#cors)             |
| **XSS**                | Injected script от вашего origin                                 | [XSS](./7_special_security.md#xss-cross-site-scripting)    |
| **CSP**                | Какие scripts/styles/frames можно грузить и выполнять            | [CSP](./7_special_security.md#content-security-policy-csp) |

- **CORS ≠ защита от CSRF** для форм с cookie-auth; **XSS** ломает same-origin, потому что код атакующего выполняется **как** страница жертвы.

**When to use:** Мост от архитектуры platform к эссе **Security** на Core и подготовке к интервью.

**Sources:** [OWASP — XSS Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html) · [MDN — CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP)

</details>

<a name="critical-rendering-path-detailed-stages"></a>

<details>
  <summary>Critical rendering path (detailed stages)</summary>

### Critical rendering path (detailed stages)

**Definition:** **CRP** — цепочка от первых байт HTML/CSS через DOM, CSSOM, render tree, layout, paint и compositing. **Core** даёт принципы выше; пошаговая глубина — в **HTML — Rendering**.

**How it works:** В dedicated-топике — DNS, fetching HTML, parsing, CSSOM, render tree, layout, paint, compositing, сравнение layout/paint/composite, reflow/animation frames, чеклисты first render.

**When to use:** Интервью по HTML track, Performance в DevTools, LCP/CLS через порядок разметки и ресурсов.

**Full topic:** [HTML — Rendering](./1_html_rendering.md#from-url-to-pixels) · [First render — full pipeline](./1_html_rendering.md#first-render--full-pipeline)

**Sources:** [MDN — Critical rendering path](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Critical_rendering_path)

</details>

<a name="browser-environment-javascript-host"></a>

<details>
  <summary>Browser environment (JavaScript host)</summary>

### Browser environment (JavaScript host)

**Definition:** Для JavaScript **browser host** — `globalThis` / `window`, bindings DOM/HTML/CSSOM и API с permissions — отдельно от ECMAScript и от C++ layout в движке рендеринга.

**How it works:** ECMAScript vs web platform, `document` vs `window`, feature detection и security для script — на **JS Browser — Document**. Процессы и main thread → [Multi-process architecture](#multi-process-architecture) и [JavaScript engine and main thread](#javascript-engine-and-main-thread) выше.

**When to use:** Изоморфный код, «почему в Node нет `fetch`?», язык vs хост vs сеть на интервью.

**Full topic:** [JS Browser — Browser environment](./3_js_browser.md#browser-environment) · hub [JS — Browser](./3_js_browser_hub.md)

**Sources:** [MDN — JavaScript technologies overview](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/JavaScript_technologies_overview)

</details>
