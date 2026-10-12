# PWA — Interview

Ответы заполнены.

[ ← Вернуться в хаб](./6_web_api.md)

### Related docs

- [Веб-API — Приложения](./6_web_api_apps.md) · [WebSockets — Интервью](./6_web_api_websockets_interview.md) · [PWA — Интервью](./6_web_api_pwa_interview.md) · [Веб-API — Интервью](./6_web_api_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Вопросы с [Devinterview.io — PWA](https://devinterview.io/questions/web-and-mobile-development/pwa-interview-questions/).

### PWA Fundamentals

<details>
<summary>Что такое Progressive Web App (PWA)?</summary>

### Что такое Progressive Web App (PWA)?

**Definition:**

Прогрессивное веб-приложение — сайт, который можно установить и, если добавить service worker, продолжать работу из кэша. Это по-прежнему веб: один URL, одна кодовая база и браузер как среда выполнения.

**How it works:**

- Манифест веб-приложения (имя, значки, `start_url`, `display`) говорит браузеру, как должно выглядеть установленное приложение.
- HTTPS или localhost в разработке нужны, прежде чем браузер предложит установку.
- Service worker — отдельный скрипт, который может перехватывать загрузки, заполнять Cache API и принимать push. Текущие правила установки Chromium его не требуют; офлайн-поведение требует.
- После установки приложение может открыться без хромированной оболочки браузера, если `display` равен `standalone`.
- Обновления приходят, когда пользователь онлайн и новый service worker берёт управление. Проверки магазина нет, пока вы дополнительно не упакуете сайт для магазина.
- Страница всё равно работает как обычный сайт в браузере, который её не устанавливает.

**Sources:**

- [MDN — Progressive web apps](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps)
</details>

<details>
<summary>Как PWAs differ from traditional web applications?</summary>

### Как PWAs differ from traditional web applications?

**Definition:**

Обычный сайт ждёт сеть на каждую навигацию и живёт во вкладке. PWA можно установить, открыть в своём окне и отвечать на навигацию из кэша, если вы дали ему service worker.

**How it works:**

- Устанавливаемость идёт от манифеста и безопасного контекста. Значок тогда стоит рядом с другими приложениями.
- У обычного сайта нет запроса на установку и нет отдельного окна, если пользователь сам не добавил закладку.
- Push и фоновая синхронизация нуждаются в service worker. Обычная страница не примет push после закрытия.
- Офлайн — это стратегия кэша, а не свойство «быть PWA». Без обработчика `fetch` установленное приложение всё равно падает без сети.
- Оба варианта можно открыть по ссылке и проиндексировать. Запись в магазине — необязательная упаковка, а не определение.
- Адаптивная вёрстка есть у любого современного сайта. Сама по себе она не делает сайт устанавливаемым.

**Examples:**

```html
<link rel="manifest" href="/manifest.webmanifest" />
```

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Можете ли вы list the core pillars of a PWA?</summary>

### Можете ли вы list the core pillars of a PWA?

**Definition:**

Части, которые есть у серьёзного PWA: безопасный origin, манифест, service worker, вёрстка под экран и быстрая первая загрузка. Установка и офлайн — два результата, которые видит пользователь.

**How it works:**

- HTTPS (или localhost) — ворота для service worker и для запроса установки.
- Манифест даёт имя, значки, стартовый URL и режим отображения.
- Service worker кэширует оболочку приложения и может отдать её, когда сеть отказала.
- Адаптивная вёрстка использует тот же CSS, что и любой сайт. Установленное окно — просто ещё один viewport.
- Оболочка приложения — устойчивая рамка (шапка, навигация, пустая область контента), которую кэшируют заранее, а данные дорисовывают потом.
- «Быстро» и «надёжно» — следствие кэша и небольших ресурсов, а не отдельных форматов файлов.
- Вовлечение — установка, push, фоновая синхронизация — стоит на воркере и необязательно.

**Sources:**

- [MDN — Progressive web apps](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps)
</details>

<details>
<summary>Что такое the advantages of developing a PWA over a native app?</summary>

### Что такое the advantages of developing a PWA over a native app?

**Definition:**

PWA поставляется как сайт: одна кодовая база, URL и обновление при следующей загрузке браузером. Нативное приложение получает более глубокий доступ к устройству и распространение через магазин ценой отдельной сборки на платформу.

**How it works:**

- Вы пишете HTML, CSS и JavaScript один раз. Сборки iOS и Android не обязательны для установки из браузера.
- Пользователь может открыть URL до любой установки. Поиск индексирует страницы.
- Новый деплой заменяет кэшированные ресурсы, когда обновляется service worker. За каждым исправлением в магазин не ходят.
- Установленный пакет часто меньше, потому что среду выполнения уже даёт браузер.
- Нативный код всё ещё выигрывает в тяжёлой графике, правилах фонового выполнения и API, которых нет в вебе.
- Магазины тоже могут распространять PWA (инструменты упаковки его оборачивают), но это необязательно. Источник остаётся сайтом.

**Sources:**

- [MDN — What is a progressive web app?](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/What_is_a_progressive_web_app)
</details>

<details>
<summary>Что такое the role of the Service Worker in a PWA?</summary>

### Что такое the role of the Service Worker in a PWA?

**Definition:**

Service worker — скрипт, который браузер выполняет отдельно от страницы. Он может перехватывать сетевые запросы, читать и писать Cache API и принимать push и события фоновой синхронизации, даже когда страница закрыта.

**How it works:**

- Его регистрируют со страницы, затем он устанавливается, активируется и позже заменяется, когда байты нового скрипта отличаются.
- Обработчик `fetch` выбирает «сначала кэш», «сначала сеть» или запасной ответ. Это и есть офлайн-поведение.
- DOM он не трогает. Он шлёт сообщения странице, а страница обновляет интерфейс.
- Push приходит на воркер. Воркер показывает уведомление, если пользователь дал разрешение.
- Background Sync (API Chromium) позволяет воркеру повторить поставленную в очередь запись, когда связь возвращается.
- Воркер работает только на безопасном origin и только в области, которую вы зарегистрировали.

**Examples:**

```js
self.addEventListener("fetch", (event) => {
  event.respondWith(
    caches.match(event.request).then((hit) => hit || fetch(event.request)),
  );
});
```

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>Как you make a web app installable on a user's home screen?</summary>

### Как you make a web app installable on a user's home screen?

**Definition:**

Отдайте сайт по HTTPS и подключите манифест, который удовлетворяет правилам установки браузера. Тогда браузер сам предложит установку; отдельный API «на домашний экран» вы не реализуете.

**How it works:**

- Chromium просит `name` или `short_name`, значки 192 и 512, `start_url` и `display` или `display_override`. `prefer_related_applications` не должен быть истинным.
- Каждая страница, которая должна считаться, включает `<link rel="manifest" href="...">`.
- Localhost допустим при разработке. Страница `file://` не устанавливается.
- Service worker нужен, чтобы добавить офлайн. В текущих критериях установки у MDN его нет как требования к запросу Chromium.
- `beforeinstallprompt` (Chromium, не iOS) позволяет показать свою кнопку и вызвать `prompt()` у события.
- iOS использует меню «Поделиться». События `beforeinstallprompt` там нет.
- После установки `display: "standalone"` открывает приложение без вкладок браузера.

**Examples:**

```json
{
  "name": "Notes",
  "start_url": "/",
  "display": "standalone",
  "icons": [{ "src": "/icon-512.png", "sizes": "512x512", "type": "image/png" }]
}
```

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Что такое manifest file and what is its significance in PWAs?</summary>

### Что такое manifest file and what is its significance in PWAs?

**Definition:**

Манифест веб-приложения — JSON-файл, который описывает установленное приложение: имя, значки, стартовый URL, цвета и открывается ли оно отдельным окном. Без него Chromium не предложит сайт к установке.

**How it works:**

- `name` и `short_name` — подписи. В `icons` нужно несколько размеров, чтобы лаунчер не растягивал одну картинку.
- `start_url` — страница, которая открывается со значка. `scope` ограничивает, какие URL остаются внутри установленного окна.
- `display` обычно `standalone` для окна приложения или `browser` для обычной вкладки. `display_override` перечисляет запасные варианты.
- `background_color` виден до загрузки таблицы стилей. `theme_color` красит окружающий интерфейс.
- `id` даёт приложению устойчивую личность, когда меняется `start_url`.
- HTML-страница указывает на файл через `<link rel="manifest">`. Манифест с ответом 404 значит, что сайт не устанавливается.

**Examples:**

```html
<link rel="manifest" href="/manifest.webmanifest" />
```

**Sources:**

- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
</details>

<details>
<summary>Что такое the puRPOse of the 'Add to Home Screen' feature?</summary>

### Что такое the puRPOse of the 'Add to Home Screen' feature?

**Definition:**

«На экран Домой» превращает сайт в значок лаунчера, который открывает стартовый URL в отдельном окне. Смысл — повторный визит в одно нажатие, а не вторая копия кода.

**How it works:**

- Браузер показывает свою установку, когда манифест подходит. Страница может добавить кнопку, которая вызывает `prompt()` у `beforeinstallprompt`, где это событие есть.
- Значок и имя берутся из манифеста, поэтому лаунчер совпадает с продуктом.
- Запуск использует `start_url` и `display`. `standalone` прячет адресную строку.
- Офлайн по-прежнему зависит от service worker. Сам значок ничего не кэширует.
- Практический результат — вовлечение: приложение остаётся на устройстве и может принимать push, если пользователь разрешил.
- iOS ставит из меню «Поделиться». Событие Chromium — не то, как устроена эта платформа.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Как you detect if your web app has been added to the user's home screen?</summary>

### Как you detect if your web app has been added to the user's home screen?

**Definition:**

Определяют режим отображения, в котором открылось окно, а не постоянный бит «установлено» на устройстве. `matchMedia("(display-mode: standalone)")` — проверка для разных браузеров, а `navigator.standalone` — старый сигнал iOS.

**How it works:**

- Когда манифест говорит `display: "standalone"` и пользователь открыл значок, медиазапрос совпадает.
- Вкладка на том же origin не совпадает, поэтому одна сборка может прятать кнопку установки только в установленном окне.
- iOS Safari давно показывает `navigator.standalone === true` для веб-приложения с домашнего экрана.
- `beforeinstallprompt` говорит, что браузер Chromium готов установить. Он не говорит, что установка уже была. Событие `appinstalled` приходит, когда пользователь согласился.
- `document.hidden` — о том, ушла ли вкладка в фон. Это не проверка установки.
- Нет веб-API, который перечислит каждое устройство, где пользователь установил приложение.

**Examples:**

```js
const installed = window.matchMedia("(display-mode: standalone)").matches
  || window.navigator.standalone === true;
```

**Sources:**

- [MDN — display-mode](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/display-mode)
</details>

<details>
<summary>Объясните how PWAs achieve offline functionality.</summary>

### Объясните how PWAs achieve offline functionality.

**Definition:**

Офлайн — это service worker, который отвечает на `fetch` из Cache API, плюс IndexedDB, когда странице нужны структурные данные, пережившие перезагрузку. Сам манифест ничего не кэширует.

**How it works:**

- При установке воркер открывает именованный кэш и кладёт оболочку: HTML, CSS, JS и значки.
- Обработчик «сначала кэш» возвращает кэшированный ответ, если он есть, иначе идёт в сеть. «Сначала сеть» пробует сеть и берёт кэш, когда она отказала.
- Версионируйте имя кэша. При активации удаляйте кэши, которых больше не называете, чтобы старые ресурсы не залипали.
- IndexedDB хранит записи (черновики, последний известный список). `localStorage` — маленькая синхронная карта строк и плохое место для набора данных.
- Background Sync может позже повторить поставленную запись в браузерах, которые это умеют. Сама очередь обычно живёт в IndexedDB.
- Покажите пользователю, что ответ пришёл из кэша. Тихая устаревшая страница выглядит как ошибка.

**Examples:**

```js
const cache = await caches.open("shell-v3");
await cache.addAll(["/", "/app.css", "/app.js"]);
```

**Sources:**

- [MDN — Caching](https://developer.mozilla.org/en-US/docs/Web/API/Cache)
</details>

<details>
<summary>Что такое the security requirements for a PWA?</summary>

### Что такое the security requirements for a PWA?

**Definition:**

PWA — сайт с дополнительными возможностями, поэтому он наследует правила веба: HTTPS, service worker того же origin и явное разрешение на push. Манифест не выдаёт доступ к камере или к чужим origin.

**How it works:**

- Service worker и запрос установки требуют безопасный контекст. Отдавайте TLS и перенаправляйте HTTP на HTTPS.
- Скрипт воркера — того же origin (или разрешён правилами области). Сторонний скрипт не может тихо стать вашим прокси `fetch`.
- Content-Security-Policy ограничивает, какие скрипты и соединения может использовать страница. Это важнее, когда страница установлена и выглядит как приложение.
- Push и уведомления требуют жест пользователя и разрешение. Отказ — нормальный исход.
- Кэшируйте только ответы, которым доверяете. Кэш страницы ошибки атакующего или непрозрачного чужого ответа, который вы не собирались хранить, станет следующей офлайн-загрузкой.
- Установленное приложение всё ещё веб. XSS на странице выполняется с властью origin, включая воркер, если страница может его зарегистрировать.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>Можете ли вы delineate between App Shell and content in a PWA context?</summary>

### Можете ли вы delineate between App Shell and content in a PWA context?

**Definition:**

Оболочка приложения — устойчивая рамка интерфейса, которую кэшируют, чтобы окно отрисовалось сразу. Контент — данные, которые меняются от пользователя или визита и дорисовываются, когда оболочка уже на экране.

**How it works:**

- Части оболочки — шапка, навигация и пустая область, куда придут результаты. Они меняются при деплое, а не когда пользователь нажал.
- Контент — JSON, изображения и текст этой сессии. Кэшируйте его с более коротким сроком или стратегией «сначала сеть».
- Воркер может вернуть оболочку на навигацию, а страница затем запрашивает данные. Офлайн оболочка всё равно рисуется, а запрос данных явно падает.
- Если положить персональный HTML в долгоживущий кэш оболочки, один пользователь увидит страницу другого.
- Разделение — выбор кэширования. Это не отдельный формат файла, которого требует браузер.

**Examples:**

```text
cache "shell-v3": /app.html, /app.css, /app.js
network, then cache: /api/notes
```

**Sources:**

- [MDN — Caching](https://developer.mozilla.org/en-US/docs/Web/API/Cache)
</details>

<details>
<summary>Как a PWA function on a low-bandwidth or offline network?</summary>

### Как a PWA function on a low-bandwidth or offline network?

**Definition:**

На слабой или отсутствующей сети service worker отвечает тем, что уже закэшировал, а страница читает структурные данные из IndexedDB. Запросы, которых в кэше не было, падают, и интерфейс должен это сказать.

**How it works:**

- Стратегия «сначала кэш» обходит медленную сеть для оболочки. Тайм-аут вокруг `fetch` может отступить в кэш, когда радио зависло, а не висеть.
- Оболочку кладут в кэш при установке. Кэш во время работы хранит ответы, которые пользователь реально запросил, например последнюю статью.
- IndexedDB держит черновики и последний список, чтобы перезагрузка офлайн не была пустым экраном.
- Background Sync, где он есть, ставит POST в очередь, пока браузер не решит, что сеть снова пригодна.
- У изображения может быть маленький кэшированный заполнитель. Не притворяйтесь, что запись прошла, если она только в очереди.
- Сами service worker требуют HTTPS, в том числе когда следующие загрузки отдаются из кэша.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>Что такое push notifications in the context of PWAs?</summary>

### Что такое push notifications in the context of PWAs?

**Definition:**

Веб-push доставляет сообщение service worker, пока страница закрыта. Воркер затем показывает уведомление, если пользователь дал разрешение и у браузера есть push-служба.

**How it works:**

- Страница спрашивает `Notification.permission`, затем `pushManager.subscribe` с публичным ключом VAPID. Подписка (endpoint и ключи) уходит на ваш сервер.
- Сервер шлёт нагрузку на этот endpoint. Push-служба браузера будит воркер событием `push`.
- Воркер вызывает `showNotification`. Обработчик `notificationclick` фокусирует или открывает окно.
- iOS поддерживает веб-push для установленных веб-приложений с домашнего экрана, не для случайной вкладки. Другие браузеры различаются.
- Разрешение — на origin, и его легко потерять. Не спрашивайте на первой отрисовке.
- Нагрузка push маленькая. Обычный приём — «что-то изменилось», а подробность страница забирает, когда откроется.

**Examples:**

```js
self.addEventListener("push", (event) => {
  event.waitUntil(self.registration.showNotification("Notes", { body: "New note" }));
});
```

**Sources:**

- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
</details>

<details>
<summary>Можете ли вы explain the concept of background sync in PWAs?</summary>

### Можете ли вы explain the concept of background sync in PWAs?

**Definition:**

Background Sync позволяет service worker повторить запись, которая не прошла, потому что устройство было офлайн. Страница ставит работу в очередь, а браузер вызывает событие `sync`, когда считает, что сеть вернулась.

**How it works:**

- Страница хранит нагрузку в IndexedDB и вызывает `registration.sync.register("send-note")`.
- Воркер слушает `sync`. Если `event.tag` совпал, он читает очередь, отправляет её и при успехе удаляет локальную копию.
- `event.waitUntil` держит воркер, пока промис не завершится. Отклонение просит браузер повторить позже, с паузой.
- Это Background Synchronization API, реализованный в Chromium. На Safari и Firefox гарантии нет, поэтому открытая страница всё равно должна уметь повторить попытку.
- Синхронизация не запускает произвольные таймеры. Periodic Background Sync — отдельный, более ограниченный API.
- Сервер должен считать повтор идемпотентным. Одна и та же заметка может прийти дважды, если ответ потерялся.

**Examples:**

```js
self.addEventListener("sync", (event) => {
  if (event.tag === "send-note") event.waitUntil(flushQueue());
});
```

**Sources:**

- [MDN — Background Synchronization API](https://developer.mozilla.org/en-US/docs/Web/API/Background_Synchronization_API)
</details>

<details>
<summary>Что такое Service Worker lifecycle?</summary>

### Что такое Service Worker lifecycle?

**Definition:**

Service worker сначала скачивают, затем устанавливают, затем активируют. Позже побайтно другой скрипт устанавливается в фоне и ждёт, пока не закроются страницы старого worker, если вы не пропускаете ожидание.

**How it works:**

- Состояния MDN: parsed, installing, installed (ожидающий worker), activating, activated и redundant. Redundant значит, что этот worker заменён более новым или установка не удалась.
- Первый заход скачивает скрипт. Дальше обновление случается при навигации в области видимости или когда событие срабатывает, а скрипт не скачивали последние 24 часа. Установка идёт, когда байты отличаются.
- install — место предварительного кэша. activate — место удаления старых кэшей. fetch — место ответа на запросы через respondWith. waitUntil на install или activate держит функциональные события, пока промис не завершится.
- skipWaiting переводит ожидающего worker в activating, не дожидаясь закрытия старых страниц. clients.claim позволяет активному worker управлять уже открытыми страницами.

**Examples:**

```js
self.addEventListener("install", (event) => {
  self.skipWaiting();
  event.waitUntil(caches.open("v1").then((c) => c.addAll(["/"])));
});
self.addEventListener("activate", (event) => {
  event.waitUntil(self.clients.claim());
});
```

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — ServiceWorker.state](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorker/state)
- [MDN — skipWaiting()](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/skipWaiting)
- [MDN — Clients.claim()](https://developer.mozilla.org/en-US/docs/Web/API/Clients/claim)
</details>

<details>
<summary>Как you handle compatibility issues across different browsers for PWAs?</summary>

### Как you handle compatibility issues across different browsers for PWAs?

**Definition:**

Установка и офлайн-поведение не одинаковы в каждом браузере. Проверяйте API и считайте отсутствующий обычным сайтом.

**How it works:**

- Браузеры Chromium ставят манифест, в котором есть name или short_name, иконки 192 и 512, start_url и display или display_override, а prefer_related_applications отсутствует или false. Firefox на десктопе из манифеста не ставит. Safari на macOS Sonoma и новее умеет Add to Dock с манифестом и без.
- На Android WebAPK ставят Chrome с Google Mobile Services и Samsung Internet на устройствах Samsung. Другие браузеры добавляют ярлык на домашний экран. На iOS 16.3 и раньше установка только из Safari. С iOS 16.4 меню «Поделиться» в Safari, Chrome, Edge, Firefox и Orion умеет ставить. beforeinstallprompt на iOS не поддерживается.
- Периодическая фоновая синхронизация требует разрешение, в Chrome — установленное приложение, и Chrome ещё смотрит на оценку вовлечённости. Push требует активного service worker. Отсутствующий API не должен ломать страницу.
- Сайт всё равно должен быть на HTTPS или localhost, чтобы ставиться. file:// — безопасный контекст, и для установки его всё равно мало.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — PeriodicSyncManager.register()](https://developer.mozilla.org/en-US/docs/Web/API/PeriodicSyncManager/register)
- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
</details>

<details>
<summary>Как бы вы test a PWA?</summary>

### Как бы вы test a PWA?

**Definition:**

Проверяйте состояния worker, кэш и правила установки. Синтаксическая ошибка в скрипте отклоняет register().

**How it works:**

- Смотрите registration.installing, waiting и active и события statechange. Состояния: installing, installed, activating, activated и redundant.
- Убедитесь, что waitUntil у install завершается: если addAll падает, установка падает. Убедитесь, что activate удаляет кэши, чьих имён нет в списке разрешённых.
- Грузите по HTTPS или localhost. Firefox может разрешить service workers по HTTP, только когда открыты DevTools и отмечена эта опция. Промышленный HTTP-origin не зарегистрируется.
- Включите offline в браузере и перезагрузите. Cache-first всё ещё должен нарисовать предварительно закэшированную оболочку. POST нельзя отдавать из кэша; MDN говорит, что кэшировать POST никогда не уместно.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — ServiceWorker.state](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorker/state)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
</details>

<details>
<summary>Что такое the role of HTTPS in PWAs?</summary>

### Что такое the role of HTTPS in PWAs?

**Definition:**

Service workers и устанавливаемые PWA требуют безопасный контекст. Для установки правило жёстче: HTTPS, либо localhost, либо 127.0.0.1.

**How it works:**

- MDN: service workers доступны только в безопасных контекстах, потому что HTTP можно подменить по пути, и с worker, который перехватывает запросы, это хуже. Браузеры считают http://localhost безопасным, чтобы локальная разработка работала.
- Установка не принимает file://, хотя безопасный контекст принимает. Каждая страница многостраничного приложения должна ссылаться на манифест.
- Push, геолокация и getUserMedia тоже ограничены безопасным контекстом. HTTP-страница теряет эти API и worker вместе.
- Переключатель DevTools в Firefox, который разрешает workers по HTTP, — помощь при отладке, а не план выкладки.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API)
- [MDN — MediaDevices.getUserMedia()](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia)
</details>

<details>
<summary>Что такое the limitations of PWAs?</summary>

### Что такое the limitations of PWAs?

**Definition:**

PWA — всё ещё сайт. Он не получает все нативные возможности, а часть тех, что получает, на некоторых браузерах отсутствует или закрыта.

**How it works:**

- Внутри worker нет доступа к DOM и нет синхронного XHR или Web Storage. Worker асинхронен и работает не в главном потоке.
- Интерфейс установки разный: на iOS нет beforeinstallprompt, в десктопном Firefox нет установки из манифеста, а во многих браузерах Android результатом будет ярлык, а не WebAPK.
- Периодическая синхронизация закрыта разрешением и в Chrome ограничена установленным приложением плюс оценкой вовлечённости. Firefox квотирует push-сообщения, которые не показывают уведомление; квота обновляется при визите. Chrome эту квоту не применяет.
- Кэш могут вытеснить, и пользователь может его очистить. Приложению cache-first всё ещё нужен запасной путь в сеть, иначе после вытеснения оно непригодно.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — PeriodicSyncManager.register()](https://developer.mozilla.org/en-US/docs/Web/API/PeriodicSyncManager/register)
- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

### Service Workers

<details>
<summary>Что такое the role of the 'install' event in a Service Worker?</summary>

### Что такое the role of the 'install' event in a Service Worker?

**Definition:**

Событие install срабатывает, когда регистрация получает нового устанавливаемого worker. Это момент заполнить кэш, который workerу понадобится.

**How it works:**

- Передайте промис кэширования в event.waitUntil. Если промис отклонён, установка не удаётся, и поздний fetch не обслуживает worker с незавершённым предварительным кэшем.
- Пример MDN открывает кэш и делает addAll оболочки: HTML, CSS, JS и картинки. Установка worker — не то же самое, что установка PWA пользователем. Событие идёт, когда скрипт скачан, обычно при первом визите.
- Здесь можно вызвать skipWaiting, чтобы новый worker не сидел в installed/waiting.
- Событие не всплывает и не отменяется. Тип — ExtendableEvent.

**Examples:**

```js
self.addEventListener("install", (event) => {
  event.waitUntil(
    caches.open("v1").then((cache) => cache.addAll(["/", "/app.js", "/style.css"]))
  );
});
```

**Sources:**

- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

<details>
<summary>Как you cache assets using Service Workers?</summary>

### Как you cache assets using Service Workers?

**Definition:**

Откройте именованный Cache и храните пары Request/Response. Предварительно кэшируйте в install. Дополняйте или обновляйте во время fetch.

**How it works:**

- caches.open(name), затем cache.addAll(urls) во время install, внутри waitUntil. addAll проваливает установку, если любой URL не удался.
- На fetch — caches.match. Тело можно прочитать один раз, поэтому cache.put получает response.clone(), а страница — оригинал.
- POST — не кандидат на кэш. JSON, который обязан быть свежим, — плохой кандидат для cache-first. Оболочка приложения — обычный предварительный кэш.
- Именуйте кэш с версией. activate удаляет каждое имя, которое не текущее.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
</details>

<details>
<summary>Можете ли вы explain how Service Worker updates are managed?</summary>

### Можете ли вы explain how Service Worker updates are managed?

**Definition:**

Браузер снова скачивает worker при навигации в области видимости или по событию, если прошло больше 24 часов с прошлой загрузки. Побайтно другой файл становится ожидающим worker.

**How it works:**

- Он не заменяет активного worker, пока страницы ещё пользуются старым. skipWaiting активирует его раньше. clients.claim заставляет открытые страницы пользоваться им без перезагрузки.
- Меняйте имя кэша, когда меняется список предварительного кэша, иначе activate продолжит отдавать старый кэш. Учебник MDN cycle-tracker для этого увеличивает константу VERSION.
- Неудачная установка оставляет старого worker активным. Новый становится redundant.
- Пользователи видят новую оболочку при следующей загрузке после активации, если вы не захватили клиентов.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — skipWaiting()](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/skipWaiting)
- [MDN — Clients.claim()](https://developer.mozilla.org/en-US/docs/Web/API/Clients/claim)
- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
</details>

<details>
<summary>Какие caching strategies можно реализовать с service workers?</summary>

### Какие caching strategies можно реализовать с service workers?

**Definition:**

MDN называет четыре стратегии. Выбирайте по ресурсу: оболочке нужна скорость, списку товаров — свежесть, а POST — только сеть.

**How it works:**

- Cache first: совпадение в кэше, иначе fetch, и сохранить клон, если ответ ok. Лучше всего для предварительно закэшированного интерфейса, который не меняется до следующего worker.
- Cache first with refresh, его же stale-while-revalidate: сразу вернуть кэш и обновить его из сети в фоне. MDN использует это, когда свежесть несколько важна, а скорость важнее, и пример пропускает JSON.
- Network first: fetch, сохранить клон, а при неудаче вернуть кэш или Response.error(). Пример MDN — входящие.
- Только сеть: не вызывать respondWith или не кэшировать. MDN говорит, что список товаров, который не должен быть устаревшим, живёт здесь, а POST кэшировать никогда не уместно.

**When to use:**

Cache first для оболочки. Stale-while-revalidate для ресурсов, которые могут отставать на версию. Network first для данных, которые должны быть свежими, но могут быть офлайн. Только сеть для платежей, POST и всего, что опасно в старом виде.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

<details>
<summary>Опишите how you would use a Service Worker to intercept network requests.</summary>

### Опишите how you would use a Service Worker to intercept network requests.

**Definition:**

Слушатель fetch вызывает event.respondWith с Response или промисом Response. Если вы его не вызвали, браузер выполняет обычный fetch.

**How it works:**

- Событие срабатывает для навигаций и подресурсов, не только для fetch() из вашего скрипта. Оно срабатывает, когда worker активен и запрос в области видимости.
- Читайте event.request. Пропускайте не-GET, если не собираетесь их кэшировать. Пример FetchEvent у MDN сразу возвращается для не-GET.
- Ответить можно из caches.match, из fetch или синтетическим Response. Попадание в кэш всё ещё может обновиться через event.waitUntil(cache.add(request)).
- У worker нет DOM. Перехват — это запрос и ответ, а не страница.

**Examples:**

```js
self.addEventListener("fetch", (event) => {
  if (event.request.method !== "GET") return;
  event.respondWith(caches.match(event.request).then((hit) => hit || fetch(event.request)));
});
```

**Sources:**

- [MDN — FetchEvent](https://developer.mozilla.org/en-US/docs/Web/API/FetchEvent)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

<details>
<summary>Что такое FetchEvent in the context of Service Workers?</summary>

### Что такое FetchEvent in the context of Service Workers?

**Definition:**

FetchEvent — объект, который передают слушателю fetch в service worker. Он описывает запрос и позволяет подставить ответ.

**How it works:**

- event.request — Request, который браузер собирается сделать. clientId — same-origin клиент, который его начал. resultingClientId — клиент, который заменяет его при навигации. preloadResponse — промис предзагруженного ответа навигации или undefined.
- respondWith отменяет обычный fetch и принимает ваш Response или промис. waitUntil, унаследованный от ExtendableEvent, продлевает событие для работы после ответа, например обновления кэша.
- handled — промис, висящий, пока событие не обработано. isReload истинен, когда пользователь перезагрузил страницу.
- Событие создаёт браузер. В обычном коде его не конструируют. Оно существует только в service workers.

**Sources:**

- [MDN — FetchEvent](https://developer.mozilla.org/en-US/docs/Web/API/FetchEvent)
</details>

<details>
<summary>Как you handle errors in a Service Worker script?</summary>

### Как you handle errors in a Service Worker script?

**Definition:**

Брошенная ошибка или отклонённый промис waitUntil проваливает этот шаг жизненного цикла. Обработчик fetch, который отклоняет respondWith, проваливает этот один запрос.

**How it works:**

- Если в скрипте синтаксическая ошибка, register() отклоняется. MDN говорит, что файл может быть недоступен или содержать синтаксическую ошибку.
- Если addAll предварительного кэша отклоняется внутри waitUntil у install, worker не активируется. Предыдущий worker, если он был, остаётся.
- Внутри fetch поймайте сетевой сбой и верните запасной кэш или Response.error(). Необработанное отклонение промиса respondWith — это неудачная загрузка, а не пропущенный перехват.
- Не используйте синхронный XHR или Web Storage в worker. Эти API недоступны, потому что worker асинхронен.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
</details>

<details>
<summary>Обсудите how Service Workers can be debugged.</summary>

### Обсудите how Service Workers can be debugged.

**Definition:**

Отлаживайте промис регистрации, состояние worker и содержимое кэша. Worker — отдельный скрипт без DOM.

**How it works:**

- Логируйте отклонение navigator.serviceWorker.register. Синтаксическая ошибка или отсутствующий файл видны там.
- Читайте registration.installing, waiting и active и слушайте statechange. Redundant значит, что worker отброшен.
- Если install так и не дошёл до activated, промис waitUntil отклонился. Обычная причина — неудачный addAll.
- Firefox может запускать workers по HTTP только с открытыми DevTools и включённой опцией небезопасного worker. Промышленности всё ещё нужны HTTPS или localhost. Перезагрузка offline проверяет, ответил ли обработчик fetch на самом деле.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — ServiceWorker.state](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorker/state)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
</details>

<details>
<summary>Что такое the 'activate' event in a Service Worker and what is it used for?</summary>

### Что такое the 'activate' event in a Service Worker and what is it used for?

**Definition:**

activate срабатывает, когда регистрация получает нового активного worker. Это безопасное время выбросить кэши предыдущего worker, потому что тот уже не работает.

**How it works:**

- MDN удаляет каждое имя кэша, которого нет в списке разрешённых, внутри event.waitUntil. Функциональные события ждут, пока этот промис не завершится.
- clients.claim() в этом обработчике сразу отдаёт уже открытых клиентов под управление, без перезагрузки.
- Уборку делайте здесь, не в install. Во время install старый worker ещё может обслуживать эти кэши.
- Событие не всплывает и не отменяется.

**Examples:**

```js
self.addEventListener("activate", (event) => {
  event.waitUntil(
    caches.keys().then((names) =>
      Promise.all(names.map((name) => (name === "v2" ? undefined : caches.delete(name))))
    )
  );
});
```

**Sources:**

- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
- [MDN — Clients.claim()](https://developer.mozilla.org/en-US/docs/Web/API/Clients/claim)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>Объясните the concept of "cache busting" and why it's important for PWAs.</summary>

### Объясните the concept of "cache busting" and why it's important for PWAs.

**Definition:**

Сброс кэша значит, что изменённый файл не отдаётся по URL, который кэш всё ещё считает тем же. Для service worker одинаковость — это байты скрипта и имя кэша.

**How it works:**

- Браузер ставит нового worker, только когда скачанный скрипт отличается побайтно. Неизменённый service-worker.js не становится новым поколением, поэтому новая оболочка, перечисленная в старом скрипте, так и не попадёт в предварительный кэш.
- Версионируйте имя кэша. activate удаляет имена, которые не текущие, и так старая оболочка уходит.
- URL ресурсов с отпечатком делают cache-first безопасным: новый бандл — новый запрос, и старый ответ не совпадает.
- skipWaiting и clients.claim только ускоряют принятие worker, который уже установился. Они не замечают изменение, которое вы забыли положить в байты скрипта.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

### Web App Manifest

<details>
<summary>Какие key properties содержит Web App Manifest?</summary>

### Какие key properties содержит Web App Manifest?

**Definition:**

Манифест — JSON-объект. В спецификации члены необязательны, но правила установки Chromium требуют конкретный набор.

**How it works:**

- Пример, который приводит MDN, содержит short_name, name, icons (192 и 512), start_url, display, theme_color и background_color.
- Установка в Chromium хочет name или short_name, иконки 192 и 512, start_url, display и/или display_override и prefer_related_applications равный false или отсутствующий.
- Подключают его через `<link rel="manifest" href="manifest.json">`. Предпочтительный тип — application/manifest+json. Браузеры принимают и .json.
- name, background_color и icons питают заставку на некоторых системах. display standalone прячет интерфейс браузера.

**Sources:**

- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Как you define the start URL in a manifest file?</summary>

### Как you define the start URL in a manifest file?

**Definition:**

start_url — член манифеста, который браузер загружает при запуске установленного приложения.

**How it works:**

- Пример MDN ставит `"start_url": "."`. Он относителен к URL манифеста.
- Chromium требует start_url для установки. Многостраничному приложению ссылка на манифест всё равно нужна на каждой странице.
- Это не scope. scope ограничивает, какие URL установленное приложение считает своими. start_url — первая страница.
- start_url вне scope — негодная цель запуска. Держите его внутри объявленного scope.

**Examples:**

```json
{ "start_url": ".", "scope": "/", "display": "standalone" }
```

**Sources:**

- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Что такое the guidelines for designing app icons for a PWA?</summary>

### Что такое the guidelines for designing app icons for a PWA?

**Definition:**

Для установки в Chromium запись icons должна включать иконку 192px и иконку 512px.

**How it works:**

- У каждой иконки есть src, sizes и type. Пример MDN использует image/png.
- Подсказка установки показывает имя и иконку из этих членов. На Android ещё показывают description и screenshots, если вы их задали.
- Заставка использует name, background_color и icons.
- Одна размытая иконка не проходит правило 192 и 512, даже если остальной манифест на месте.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
</details>

<details>
<summary>Как the web app manifest affect the appearance of the PWA on a device?</summary>

### Как the web app manifest affect the appearance of the PWA on a device?

**Definition:**

Манифест говорит ОС, как выглядит установленное приложение: имя, иконка, цвета и спрятан ли интерфейс браузера.

**How it works:**

- display standalone выглядит как платформенное приложение, без интерфейса браузера. display browser открывает обычную вкладку. Если режим не поддерживается, display откатывается по фиксированной цепочке. display_override заменяет эту цепочку.
- theme_color и background_color красят окружающий интерфейс и заставку. Заставка использует name, background_color и icons.
- Подсказка установки использует name и icons. На Android она может показать ещё description и screenshots.
- Без манифеста некоторые браузеры всё ещё могут поставить любой сайт как приложение, но продвигать его и применять эти члены не будут.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
</details>

<details>
<summary>Можете ли вы dynamically generate a web app manifest or must it be static?</summary>

### Можете ли вы dynamically generate a web app manifest or must it be static?

**Definition:**

Манифест — JSON-ответ, который браузер забирает по ссылке на странице. Это не обязан быть файл, зафиксированный статическими байтами, но ответ должен быть JSON, который браузер разбирает.

**How it works:**

- `<link rel="manifest" href="...">` — точка входа. Сгенерированный URL годится, если он возвращает объект. Если загрузке нужны учётные данные, поставьте crossorigin="use-credentials" даже для same-origin URL.
- Тип должен быть application/manifest+json. Браузеры принимают и application/json.
- Каждая страница многостраничного приложения должна указывать на манифест. Манифест, который появляется только после клиентского скрипта, опаздывает к проверке установки.
- Chromium всё равно требует члены установки в том JSON, который вы сгенерировали. Генерация не ослабляет name, icons, start_url и display.

**Sources:**

- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

### User Engagement & Experience

<details>
<summary>Как push notifications enhance user enGAgement in a PWA?</summary>

### Как push notifications enhance user enGAgement in a PWA?

**Definition:**

Push позволяет серверу разбудить service worker и показать уведомление после того, как пользователь подписался. Страница может быть закрыта.

**How it works:**

- Активный service worker вызывает PushManager.subscribe(). PushSubscription несёт endpoint и ключ шифрования. Endpoint — capability URL: кто его знает, тот может послать push, поэтому держите его в секрете.
- Входящие сообщения запускают событие push. Worker может вызвать registration.showNotification(). Firefox квотирует push, которые не показывают уведомление; квота обновляется при визите. У Chrome такой квоты нет.
- Защитите endpoint подписки от CSRF. MDN предупреждает об этом отдельно.
- Push не работает без worker, а worker не работает без безопасного контекста.

**Sources:**

- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>Какие стратегии can you use to prompt the user to install your PWA?</summary>

### Какие стратегии can you use to prompt the user to install your PWA?

**Definition:**

Выполните условия установки и затем либо дайте браузеру показать свою иконку, либо вызовите prompt() на сохранённом событии beforeinstallprompt.

**How it works:**

- Chromium продвигает приложение, отданное по HTTPS или localhost и имеющее нужные члены манифеста. Подсказка показывает name и icons.
- beforeinstallprompt срабатывает, когда браузер решил, что приложение можно поставить. preventDefault, сохраните событие и вызовите prompt() из своей кнопки. Прочитайте result.outcome.
- Это событие не поддерживается на iOS. Там пользователь ставит из меню «Поделиться».
- Не показывайте свою кнопку, пока событие не сработало. До этого приложение ещё нельзя поставить.

**Examples:**

```js
let deferred;
window.addEventListener("beforeinstallprompt", (event) => {
  event.preventDefault();
  deferred = event;
});
installButton.onclick = async () => {
  if (!deferred) return;
  const result = await deferred.prompt();
  deferred = null;
};
```

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — beforeinstallprompt](https://developer.mozilla.org/en-US/docs/Web/API/Window/beforeinstallprompt_event)
</details>

<details>
<summary>Как бы вы customize the prompt that asks users to add your PWA to their home screen?</summary>

### Как бы вы customize the prompt that asks users to add your PWA to their home screen?

**Definition:**

Можно отложить подсказку браузера и добавить контекст. Нельзя перерисовать сам диалог браузера, кроме полей, которые манифест и так в него кладёт.

**How it works:**

- preventDefault на beforeinstallprompt останавливает интерфейс по умолчанию. Ваша кнопка позже вызывает prompt().
- name и icons всегда в подсказке. На Android ещё показывают description и screenshots, когда они есть в манифесте.
- На iOS нет beforeinstallprompt, поэтому нет своей кнопки, которая открывает системную подсказку. Путь установки — меню «Поделиться».
- Исход — выбор пользователя. Отклонённую подсказку не надо тут же вызывать снова.

**Sources:**

- [MDN — beforeinstallprompt](https://developer.mozilla.org/en-US/docs/Web/API/Window/beforeinstallprompt_event)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Можете ли вы tailor the UX of a PWA depending on whether it's run in a browser or as an installed app?</summary>

### Можете ли вы tailor the UX of a PWA depending on whether it's run in a browser or as an installed app?

**Definition:**

Да. display standalone запускается без интерфейса браузера, и медиазапрос видит этот режим. Вкладка — это display browser.

**How it works:**

- Член display манифеста выбирает standalone или browser. Неподдерживаемые режимы откатываются. display_override меняет порядок отката.
- Сопоставьте `(display-mode: standalone)` в CSS или эквивалентную проверку в скрипте, чтобы убрать браузерные элементы, которые вы добавили сами.
- start_url — то, что запускается. Визит вкладкой может быть другой страницей. Не считайте, что установленное приложение и вкладка ничего не делят: они делят хранилище origin и тот же worker.
- Установка на iOS не использует beforeinstallprompt, но запущенное standalone-приложение всё ещё режим отображения, по которому можно ветвиться.

**When to use:**

Ветвитесь, когда установленное окно должно спрятать подсказку «открыть в браузере». Кодовая база одна. Второе приложение не выпускайте.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
</details>

<details>
<summary>Что такое the significance of having a responsive design in PWAs?</summary>

### Что такое the significance of having a responsive design in PWAs?

**Definition:**

Установленное PWA — окно на тот же сайт. Если вёрстка работает только на одной ширине, иконка на домашнем экране открывает сломанную страницу.

**How it works:**

- display standalone убирает интерфейс браузера, так что у вёрстки меньше хрома и те же обязанности viewport. Иконки и заставка вёрстку не чинят.
- История установки у MDN — это прогрессивное улучшение: браузер, который не может поставить приложение, всё ещё пользуется им как сайтом. Этот сайт должен быть пригоден.
- start_url и каждая другая страница делят ссылку на манифест. Отзывчивая оболочка — то, что рисуют и вкладка, и окно standalone.
- Кэш таблицы стилей только для десктопа не делает телефонную вёрстку. Закэшированный CSS должен включать узкую раскладку.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

### Performance & Optimization

<details>
<summary>Какие инструменты can be used for performance auditing of a PWA?</summary>

### Какие инструменты can be used for performance auditing of a PWA?

**Definition:**

Проверяйте водопад сети и worker, а не одну оценку. Проверки, которые MDN делает наблюдаемыми, — это устанавливаемость, стратегия кэша и первая отрисовка.

**How it works:**

- First Contentful Paint — момент, когда браузер впервые рисует текст или изображение, включая фоновое. Он отвечает, происходит ли что-нибудь.
- Предварительно закэшированная оболочка — как PWA рисует это первое содержимое без сети. Cache-first для HTML, CSS и JS — стратегия, которую MDN даёт интерфейсу.
- Time to Interactive — нестандартизованная метрика: момент после последней длинной задачи, за которой 5 секунд бездействия сети и главного потока. Это не API браузера, который вызывают.
- Убедитесь, что worker дошёл до activated и что addAll у install не отклонился. Worker, который так и не активировался, не может быть причиной быстрого второго открытия.

**Sources:**

- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
- [MDN — Time to Interactive](https://developer.mozilla.org/en-US/docs/Glossary/Time_to_interactive)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — ServiceWorker.state](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorker/state)
</details>

<details>
<summary>Как you optimize the loading time of your PWA?</summary>

### Как you optimize the loading time of your PWA?

**Definition:**

Отдавайте первую отрисовку из кэша и не ждите запуска worker для URL, которые вы уже знаете.

**How it works:**

- Cache-first по предварительно закэшированной оболочке возвращает HTML, CSS и JS без кругового похода в сеть. MDN называет отзывчивость причиной кэшировать, отдельно от offline.
- Сам worker может задержать первую загрузку после простоя, потому что браузер ждёт его старта. InstallEvent.addRoutes() может сказать браузеру забрать известные URL, не дожидаясь этого старта.
- Документ network-first ждёт сеть, даже когда хорошая оболочка в кэше. Используйте это только для ответов, которые обязаны быть свежими.
- Держите список предварительного кэша маленьким. addAll проваливает всю установку, если один URL не удался, а огромный список задерживает активацию.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>Обсудите the impact of JavaScript bundling and minification on PWAs.</summary>

### Обсудите the impact of JavaScript bundling and minification on PWAs.

**Definition:**

Сборка и минификация уменьшают скрипт, который первый визит обязан скачать. Они не заменяют кэш, и они должны менять URL, иначе worker сохранит старый файл.

**How it works:**

- Cache-first отдаёт то, что сохранено для этого URL запроса. Новый бандл по тому же URL невидим, пока запись кэша не заменят. Имя с отпечатком — новый запрос.
- Сам скрипт service worker обновляется, только когда меняются его байты. Импортируйте URL нового бандла из нового worker и поднимите имя кэша, чтобы activate выбросил старый.
- Меньше JavaScript до первой отрисовки помогает First Contentful Paint, то есть первому тексту или изображению. Меньший бандл — меньше разбирать на главном потоке до этой отрисовки и до простоя страницы.
- Минификация не даёт worker трогать DOM. Worker остаётся отдельным скриптом.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
</details>

<details>
<summary>Какую роль does image optimization play in PWAs?</summary>

### Какую роль does image optimization play in PWAs?

**Definition:**

Изображения часто и есть первая содержательная отрисовка и большая доля предварительного кэша. Меньшее изображение рисуется раньше и реже роняет addAll.

**How it works:**

- First Contentful Paint включает первое изображение, в том числе фоновые. Огромный герой над сгибом задерживает эту отметку.
- Предварительно кэшируйте изображения, которые нужны оболочке, а остальное оставьте поздней стратегии. addAll проваливает установку, если хотя бы одно изображение отвечает 404.
- Cache-first уместен для изображений, которые не меняются в этой версии. Новому изображению нужен новый URL или подъём имени кэша, иначе останутся старые байты.
- Не кладите в предварительный кэш каждое изображение галереи. MDN разрешает кэшировать и позже, на fetch или когда пользователь просит сохранить что-то для offline.

**Sources:**

- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
</details>

<details>
<summary>Как you track and improve the First Contentful Paint (FCP) in PWAs?</summary>

### Как you track and improve the First Contentful Paint (FCP) in PWAs?

**Definition:**

First Contentful Paint — время, когда браузер впервые рисует любой текст или изображение, включая фоновое. Он отвечает, происходит ли что-нибудь.

**How it works:**

- Предварительно закэшированная оболочка может нарисовать этот текст и CSS без сети. Это польза отзывчивости, которую MDN даёт кэшированию, а не только польза offline.
- Не блокируйте эту отрисовку документом network-first или стартом worker, который можно пропустить. addRoutes может забрать известные URL, не дожидаясь worker.
- Следите за ним в лаборатории и в поле с одним и тем же определением. Улучшать его значит, что первый текст или изображение приходят раньше, а не что сдвинулась поздняя метрика.
- Заставка из манифеста — не FCP. FCP — отрисовка содержимого DOM.

**Sources:**

- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>Что такое Time to Interactive (TTI), and how do you optimize it for PWAs?</summary>

### Что такое Time to Interactive (TTI), and how do you optimize it for PWAs?

**Definition:**

Time to Interactive — нестандартизованная метрика прогресса: момент, когда последняя длинная задача закончилась и за ней было 5 секунд без активности сети и главного потока.

**How it works:**

- MDN относит предложение к Web Incubator Community Group в 2018 году. Это не событие, которое слушают.
- Длинная задача на главном потоке отодвигает это тихое окно. Меньше JavaScript до первой отрисовки и worker, который не блокирует документ, оставляют главный поток свободным.
- Service worker работает вне главного потока и не имеет DOM. Перенос кэша и fetch туда сам по себе не заканчивает длинные задачи на странице.
- Пять тихих секунд — это определение. Страница, которая продолжает опрашивать, до него не доходит.

**Sources:**

- [MDN — Time to Interactive](https://developer.mozilla.org/en-US/docs/Glossary/Time_to_interactive)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
</details>

<details>
<summary>Как you implement lazy loading in a PWA?</summary>

### Как you implement lazy loading in a PWA?

**Definition:**

Ленивая загрузка забирает подресурс, когда его вот-вот увидят, вместо того чтобы класть его в предварительный кэш install.

**How it works:**

- Предварительный кэш — для оболочки, которая нужна первой отрисовке. MDN говорит, что кэшировать можно и во время fetch, или когда пользователь просит сохранить что-то большое, в том числе через Background Fetch.
- Изображение не из списка предварительного кэша запрашивается обычно. Изображение с loading="lazy" — этот запрос, отложенный. Обработчик fetch всё ещё может закэшировать ответ, когда он случится.
- Не загружайте лениво изображение, которое и есть First Contentful Paint. Эта отметка — первый текст или изображение.
- Если пользователь ушёл в offline до ленивого запроса, cache-first нечего вернуть. Оболочка всё равно должна нарисоваться.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
</details>

<details>
<summary>Почему is it important to prioritize above-the-fold content for PWAs?</summary>

### Почему is it important to prioritize above-the-fold content for PWAs?

**Definition:**

Содержимое над сгибом — то, что измеряет First Contentful Paint: первый текст или изображение, которые пользователь может увидеть.

**How it works:**

- Если это содержимое ждёт вызов network-first или большой скрипт, FCP ждёт вместе с ним. Предварительно кэшируйте HTML и CSS, которые его создают.
- Изображения в этой области считаются содержательной отрисовкой, включая фоны. Огромный герой задерживает отметку, которую должен был улучшить.
- Остальная страница может оставаться ленивой. Оболочка — нет.
- Заставка из манифеста — не замена. FCP — содержимое DOM.

**Sources:**

- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
</details>

<details>
<summary>Обсудите how to prevent janky animations and achieve smooth performance in a PWA.</summary>

### Обсудите how to prevent janky animations and achieve smooth performance in a PWA.

**Definition:**

Дёрганье — длинная задача на главном потоке между кадрами. Time to Interactive буквально ждёт последнюю длинную задачу и затем пять тихих секунд.

**How it works:**

- Держите работу анимации подальше от скрипта, который ещё и разбирает большой бандл. Меньший бандл — меньше времени главного потока, прежде чем страница сможет затихнуть.
- Service worker не анимирует. У него нет DOM. Не пытайтесь вести кадры из worker.
- Обработчик fetch, который делает тяжёлую синхронную работу, задерживает ответы. Worker асинхронен не просто так. MDN запрещает там синхронный XHR.
- Для движения, которое нужно первой отрисовке, предпочитайте CSS, чтобы FCP не ждал скрипт, вставляющий первое изображение.

**Sources:**

- [MDN — Time to Interactive](https://developer.mozilla.org/en-US/docs/Glossary/Time_to_interactive)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
</details>

<details>
<summary>Какие стратегии can be employed to minimize main-thread work in a PWA?</summary>

### Какие стратегии can be employed to minimize main-thread work in a PWA?

**Definition:**

Работа главного потока — то, что задерживает First Contentful Paint и что Time to Interactive называет длинной задачей.

**How it works:**

- Отдавайте меньше скрипта до первой отрисовки. Worker может перехватывать fetch вне главного потока, но DOM он трогать не может, поэтому отрисовка остаётся на странице.
- addRoutes позволяет браузеру забирать известные URL, не дожидаясь старта worker, — это соседняя с главным потоком цена старта, о которой говорит MDN.
- Не кэшируйте заранее столько, что waitUntil у install качает огромное перед активацией worker. Неудачная или медленная установка странице не помогает.
- Пять секунд тишины главного потока — часть определения TTI. Опрос и таймеры, которые не останавливаются, не пускают страницу в это окно.

**Sources:**

- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
- [MDN — Time to Interactive](https://developer.mozilla.org/en-US/docs/Glossary/Time_to_interactive)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

### PWA Architecture and Application Structure

<details>
<summary>Какие architectural patterns обычно используют в PWAs?</summary>

### Какие architectural patterns обычно используют в PWAs?

**Definition:**

Архитектура, которую описывает MDN, — страница плюс service worker, который проксирует запросы, плюс манифест для установки. Страница остаётся интерфейсом.

**How it works:**

- Worker — событийный скрипт: install, activate, fetch, push. DOM у него нет.
- Стратегии кэша различаются по ресурсу. Один worker может использовать cache-first для оболочки и network-first для входящих.
- Офлайн-записи идут в клиентское хранилище и в позднюю синхронизацию. Background Sync регистрирует одноразовый тег. Периодическая синхронизация — отдельный, более ограниченный API.
- Прогрессивное улучшение — это история установки: браузер, который не может поставить, всё равно получает сайт.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Offline and background operation](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Как следует you structure your files and directories for a PWA?</summary>

### Как следует you structure your files and directories for a PWA?

**Definition:**

Положите worker в область видимости, которая покрывает страницы, которыми он должен управлять, и положите манифест туда, откуда на него ссылается каждая страница.

**How it works:**

- register(url, { scope }) управляет страницами под этой областью. Скрипт во вложенной папке не может управлять родительским путём.
- Ссылка на манифест нужна на каждой странице многостраничного приложения. start_url — страница запуска.
- URL предварительного кэша — пути, которые запрашивает обработчик install. Они должны существовать. Один 404 проваливает addAll.
- Держите URL файла worker стабильным. Браузер сравнивает его байты. Ресурсы, которые он кэширует, должны быть с отпечатком.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
</details>

<details>
<summary>Можете ли вы explain the Model-View-ViewModel (MVVM) architecture in the context of PWAs?</summary>

### Можете ли вы explain the Model-View-ViewModel (MVVM) architecture in the context of PWAs?

**Definition:**

MVVM — разделение интерфейса: модель держит состояние, представление рисует, view-model приспосабливает одно к другому. PWA этого не требует. Worker — не view-model.

**How it works:**

- Worker не может трогать DOM, поэтому он не может быть представлением. Он может кэшировать и синхронизировать. Страница остаётся представлением.
- Офлайн-состояние принадлежит IndexedDB, который MDN описывает как клиентское хранилище значительных структурированных данных, включая файлы и blob, с индексами. Web Storage — меньшее хранилище, и внутри worker его использовать нельзя.
- View-model, который считает, что сеть есть, покажет пустое представление offline. Модель должна сначала читать локальное хранилище.
- Устанавливаемости и кэшированию всё равно, какой шаблон интерфейса использует страница.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
</details>

<details>
<summary>Как you manage state in a PWA?</summary>

### Как you manage state in a PWA?

**Definition:**

Отделите долговечные данные пользователя от HTTP-кэша. Кэш хранит ответы. IndexedDB хранит структурированные записи.

**How it works:**

- IndexedDB — для значительных структурированных данных, включая blob, с индексами для поиска. Web Storage — для меньших объёмов, и worker его использовать не может.
- Cache API держит пары Request/Response. Это верное место для оболочки и для GET-ответов. Это неверное место для черновика, который пользователь редактирует.
- Версионированное имя кэша — состояние оболочки. Поднимайте его, когда оболочка меняется, и удаляйте старое имя в activate.
- Синхронизация — тег, который регистрируют, когда в локальном хранилище есть что послать. Worker выполнит его позже. Страница не должна делать вид, что POST уже успешен.

**Sources:**

- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — Offline and background operation](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation)
</details>

<details>
<summary>Обсудите the role of front-end frameworks and libraries in PWA development.</summary>

### Обсудите the role of front-end frameworks and libraries in PWA development.

**Definition:**

Фреймворк не ставит worker или манифест за вас, пока вы не добавите эти файлы. Правила браузера те же.

**How it works:**

- Странице всё ещё нужны ссылка на манифест, HTTPS и регистрирующий скрипт. Chromium всё ещё хочет члены установки.
- Worker — отдельный файл. Сборщик, который ставит отпечаток на URL worker при каждой сборке, заставляет браузер считать его новым worker, только если байты изменились, а они изменятся. Держите регистрируемый URL стабильным и ставьте отпечаток на ресурсы.
- HTML фреймворка всё ещё имеет First Contentful Paint. Отдача всего фреймворка до любого текста её задерживает.
- HTML, отрисованный на сервере, можно предварительно закэшировать как оболочку. Worker фреймворк не запускает. Он кэширует ответ.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

<details>
<summary>Как a PWA handle dynamic content and user personalization?</summary>

### Как a PWA handle dynamic content and user personalization?

**Definition:**

Динамическому содержимому нужен network-first или только сеть. Персонализация, которая не должна устаревать, не принадлежит оболочке cache-first.

**How it works:**

- Пример network-first у MDN — путь входящих: попробовать сеть, обновить кэш, при неудаче откатиться к кэшу. Список товаров, который обязан быть текущим, остаётся только сетью.
- Оболочка вокруг этих данных всё ещё может быть в предварительном кэше. Каркас HTML и JSON — разные стратегии.
- POST и пользовательские записи не кэшируются. MDN говорит, что кэшировать POST никогда не уместно.
- Персональная страница, закэшированная по одному URL, будет показана следующей сессии на этом устройстве. Различайте кэш или не кэшируйте этот ответ.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

### Offline Capabilities

<details>
<summary>Как you design for offline-first in a PWA?</summary>

### Как you design for offline-first in a PWA?

**Definition:**

Offline-first значит, что оболочка рисуется из кэша, а правки пользователя ложатся в локальное структурированное хранилище, пока синхронизация не сможет их отправить.

**How it works:**

- Предварительно кэшируйте оболочку в install и отвечайте на навигации из этого кэша. Путь cache-first у MDN — интерфейс, который не меняется до следующего worker.
- Черновики идут в IndexedDB, не в Cache API. IndexedDB держит структурированные данные и blob. Cache API держит ответы.
- Регистрируйте тег фоновой синхронизации, когда устройство offline, чтобы worker мог повторить. Руководство MDN показывает sync.register со страницы, когда worker готов. Событие sync — не общий таймер.
- Пользователь должен видеть, что правка локальна. Отклонённый waitUntil просит браузер повторить с отсрочкой. Это не гарантия в каждом браузере.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — Offline and background operation](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation)
</details>

<details>
<summary>Что такое the role of IndexedDB in PWAs?</summary>

### Что такое the role of IndexedDB in PWAs?

**Definition:**

IndexedDB — клиентское хранилище значительных структурированных данных, включая файлы и blob, с индексами для поиска. Web Storage — только для меньших объёмов.

**How it works:**

- MDN противопоставляет его Web Storage, который менее полезен для больших структурированных данных. Service worker вообще не может использовать Web Storage, потому что worker асинхронен, а Web Storage синхронен.
- Используйте его для черновиков, очередей исходящих правок и закэшированных результатов запросов, которые нужно искать. Не используйте его вместо хранения ответов в Cache API.
- Он живёт в origin. Очистка данных сайта очищает и его. Не считайте его сервером.
- Индексы — как искать, не просматривая каждую запись. Это и есть смысл, который указывает MDN.

**Sources:**

- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>Как you synchronize local changes with a server once online?</summary>

### Как you synchronize local changes with a server once online?

**Definition:**

Поставьте изменение в локальную очередь и зарегистрируйте одноразовую фоновую синхронизацию. Worker отправит его, когда браузер вызовет событие sync.

**How it works:**

- Со страницы, когда worker готов: registration.sync.register(tag). Руководство MDN по offline использует этот вызов.
- Worker слушает sync, проверяет event.tag и передаёт отправку в event.waitUntil. Отклонение просит браузер повторить позже с отсрочкой.
- Это не Periodic Background Sync. Периодическая синхронизация — отдельный API, ей нужно разрешение periodic-background-sync, и в Chrome нужно установленное приложение плюс оценка вовлечённости.
- Сервер должен пережить повтор. Потерянный ответ может заставить ту же правку из очереди отправиться снова.

**Examples:**

```js
const reg = await navigator.serviceWorker.ready;
await reg.sync.register("send-note");
```

**Sources:**

- [MDN — Offline and background operation](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation)
- [MDN — PeriodicSyncManager.register()](https://developer.mozilla.org/en-US/docs/Web/API/PeriodicSyncManager/register)
</details>

<details>
<summary>Можете ли вы explain how to manage user-generated content in offline mode?</summary>

### Можете ли вы explain how to manage user-generated content in offline mode?

**Definition:**

Храните содержимое пользователя в IndexedDB, пока нет сети, и держите очередь того, что ещё надо отправить. Не кладите черновик в HTTP-кэш.

**How it works:**

- IndexedDB держит структурированные данные и blob, что покрывает заметку и приложенный файл. Cache API держит ответы и имеет неверную форму для редактируемого черновика.
- Покажите локальную копию сразу. Пометьте её несинхронизированной. Когда сеть вернётся, sync.register просит worker отправить очередь.
- Если пользователь дважды правит тот же элемент до успешной отправки, очередь должна держать последнюю копию, или сервер должен применять их по порядку.
- Очистка данных сайта удаляет локальную копию. Скажите пользователю, что устройство — единственная копия, пока синхронизация не удалась.

**Sources:**

- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Offline and background operation](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation)
</details>

<details>
<summary>Что такое the considerations for storing and reTrieving data locally in a PWA?</summary>

### Что такое the considerations for storing and reTrieving data locally in a PWA?

**Definition:**

Подберите хранилище под данные. Ответы идут в Cache API. Структурированные записи — в IndexedDB. Мелкие токены — единственное, что подходит Web Storage, и не внутри worker.

**How it works:**

- Cache API — постоянное хранилище Request/Response, доступное странице и worker. Предварительный кэш в install, обновление в fetch, удаление старых имён в activate.
- IndexedDB — для значительных структурированных данных и blob, с индексами. Web Storage — для меньших данных, и в worker его использовать нельзя.
- Браузер может вытеснить кэши. Пользователь может их очистить. Приложению cache-first нужен запасной путь в сеть.
- Не кэшируйте POST. Не кэшируйте ответ, который опасен устаревшим, например живой список товаров. MDN говорит, что там стратегия — только сеть.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
</details>

### Push Notifications

<details>
<summary>Как you register a Service Worker for push notifications?</summary>

### Как you register a Service Worker for push notifications?

**Definition:**

Регистрация push — это PushManager.subscribe() на активном service worker. Страница получает подписку, чей endpoint хранит сервер.

**How it works:**

- Worker уже должен быть активен. subscribe() возвращает PushSubscription с endpoint и ключами, которые серверу нужны, чтобы зашифровать тело.
- Endpoint секретен. MDN говорит, что знания его достаточно, чтобы послать сообщение вашему приложению.
- Worker обрабатывает push и может показать уведомление через showNotification(). Push, который так и не показывает уведомление, идёт в счёт квоты Firefox.
- Защитите запрос, который сохраняет подписку. MDN предупреждает о CSRF в этом потоке.

**Examples:**

```js
const reg = await navigator.serviceWorker.ready;
const sub = await reg.pushManager.subscribe({ userVisibleOnly: true });
```

**Sources:**

- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
</details>

<details>
<summary>Что такое the Web Push Protocol and how is it used in PWAs?</summary>

### Что такое the Web Push Protocol and how is it used in PWAs?

**Definition:**

Web Push — доставка от сервера к браузеру, которая приземляется в событие push service worker. Push API — браузерная сторона этой доставки.

**How it works:**

- Сервер шлёт на endpoint подписки, зашифровав ключом подписки. Браузер при необходимости запускает worker и вызывает push.
- Worker читает PushMessageData и обычно вызывает showNotification(). Страница может быть не загружена.
- pushsubscriptionchange срабатывает, когда подписка истекает или вот-вот истечёт. Переподпишитесь там, иначе сохранённый endpoint мёртв.
- Endpoint должен оставаться секретом. Считайте утёкший endpoint утёкшей возможностью отправки.

**Sources:**

- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
</details>

<details>
<summary>Можете ли вы explain the notification click event handling in a PWA?</summary>

### Можете ли вы explain the notification click event handling in a PWA?

**Definition:**

Щелчок по уведомлению обрабатывают в worker, потому что страница может быть закрыта. Worker фокусирует существующего клиента или открывает нового.

**How it works:**

- Обработчик push показывает уведомление. Обработчик щелчка — отдельное событие на worker. Используйте waitUntil, чтобы worker жил, пока вы фокусируете или открываете окно.
- clients.matchAll находит открытые окна. Если одно подходит, сфокусируйте его. Иначе откройте start_url или URL, который вы положили в данные уведомления.
- Страницы может не быть. Не обрабатывайте щелчок только слушателем на странице.
- Щелчок, который ничего не делает, — неудачное уведомление. Пользователь согласился увидеть результат.

**Sources:**

- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>Что такое the best practices for sending push notifications to users?</summary>

### Что такое the best practices for sending push notifications to users?

**Definition:**

Просите разрешение в контексте, показывайте уведомление на то, что посылаете, и держите endpoint в секрете.

**How it works:**

- subscribe только после того, как у пользователя есть причина. Push API доставляет, даже когда приложение не загружено, поэтому шумный отправитель дорог. Firefox будет квотировать тихие push.
- Подписки userVisibleOnly — те, что показывают уведомление. Тихий push — то, что Firefox ограничивает, и предел обновляется при визите на сайт. Chrome этот предел не применяет.
- Обработайте pushsubscriptionchange, иначе будете слать на мёртвый endpoint.
- Защитите от CSRF вызов, который сохраняет подписку. Endpoint достаточен, чтобы послать push.

**Sources:**

- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
</details>

<details>
<summary>Как you customize the notification appearance and behavior on different platforms?</summary>

### Как you customize the notification appearance and behavior on different platforms?

**Definition:**

Интерфейс уведомления принадлежит платформе. Вы выбираете заголовок, текст, иконку и данные. Один и тот же пиксельный диалог на каждой ОС вы не получите.

**How it works:**

- showNotification рисует средствами платформы. Иконка, которую вы передаёте, должна быть настоящим URL изображения, которое устройство может забрать, с той же заботой, что иконки манифеста.
- Щелчок обрабатывают в worker, а не на странице, которая может быть закрыта. Положите целевой URL в данные уведомления.
- Установка и push на iOS отличаются от Chromium. Проверяйте showNotification и PushManager, прежде чем обещать уведомления.
- Тихие уведомления — проблема квоты Firefox. Предпочитайте видимое уведомление.

**Sources:**

- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

### Scalability and Maintainability

<details>
<summary>Как you ensure that a PWA remains Scalable as features grow?</summary>

### Как you ensure that a PWA remains Scalable as features grow?

**Definition:**

Масштабируйте сайт так, как масштабируют сайт. Worker остаётся тонким прокси. Не кладите логику каждой функции в обработчик fetch.

**How it works:**

- Стратегии кэша остаются по шаблону URL: оболочка cache-first, свежие данные network-first или только сеть. Одна растущая лесенка if в fetch — так перехват становится непроверяемым.
- Структурированные данные остаются в IndexedDB. Cache API остаётся ответами. Смешение делает вытеснение и миграции одним комком.
- Новая версия worker — новый скрипт плюс новое имя кэша. activate удаляет старые кэши. Это миграция оболочки.
- Устанавливаемость не меняется оттого, что вы добавили маршрут. Правила манифеста и HTTPS остаются.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Обсудите the use of module bundlers in PWAs for better maintainability.</summary>

### Обсудите the use of module bundlers in PWAs for better maintainability.

**Definition:**

Сборщик должен ставить отпечаток на ресурсы и оставлять URL service worker стабильным, иначе каждая сборка выглядит как новый worker по неверной причине.

**How it works:**

- Браузер сравнивает байты worker. Хеш бандла внутри файла worker меняет эти байты, и это то, что нужно, когда изменился список предварительного кэша. Хеш только в имени файла worker меняет URL, который вы регистрируете.
- Список предварительного кэша должен быть именами, которые выдал сборщик. Устаревший список проваливает addAll или отдаёт отсутствующий файл.
- Минификация уменьшает скрипт главного потока, что помогает первой отрисовке. Семантику fetch она не меняет.
- Карты кода в предварительный кэш не кладут.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
</details>

<details>
<summary>Какие соображения must be taken into account for version control in PWAs?</summary>

### Какие соображения must be taken into account for version control in PWAs?

**Definition:**

Версионируйте worker и имя кэша вместе. Байты скрипта — версия, которую видит браузер.

**How it works:**

- Новый коммит, который меняет worker, ставит ожидающего worker. Старые страницы держат старого, пока не закроются, если вы не вызвали skipWaiting.
- Имя кэша — версия оболочки. activate удаляет каждое другое имя. Забытый подъём вечно отдаёт предыдущий предварительный кэш.
- Имена ресурсов с хешами значат, что старую запись кэша просто не запрашивают. Это безопаснее, чем перезаписывать один URL.
- Не заставляйте обновление со страницы по таймеру. Браузер и так проверяет при навигации и не реже чем раз в 24 часа, когда срабатывают события.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
- [MDN — skipWaiting()](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/skipWaiting)
</details>

<details>
<summary>Как you handle data migrations in PWAs?</summary>

### Как you handle data migrations in PWAs?

**Definition:**

Миграции оболочки — новое имя кэша и удаление старых имён в activate. Миграции данных — смена версии IndexedDB.

**How it works:**

- activate — момент, который MDN даёт для удаления кэшей, которыми пользовался предыдущий worker. Старого worker уже нет, поэтому эти имена можно выбросить.
- IndexedDB — структурированное хранилище. Смена схемы принадлежит его версионированному обновлению, а не Cache API.
- Не мигрируйте, читая каждый закэшированный ответ и надеясь, что форма совпадёт. Ответы — тела HTTP. Записи — IndexedDB.
- Если обновление не удалось, сохраните старую базу. Полузаписанное хранилище хуже старой схемы.

**Sources:**

- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

<details>
<summary>Можете ли вы implement feature toggles in PWAs?</summary>

### Можете ли вы implement feature toggles in PWAs?

**Definition:**

Переключатель функции — флаг времени выполнения. В PWA он должен жить там, где его могут прочитать и страница, и при необходимости worker, и закэшированная оболочка не должна заморозить флаг навсегда.

**How it works:**

- Не кладите JSON-флаг в cache-first, если флаг должен меняться до следующего worker. Пример stale-while-revalidate у MDN пропускает JSON как раз ради такой свежести, а только сеть — стратегия, когда устаревшее неверно.
- IndexedDB или чтение из сети могут держать флаг для страницы. Worker не видит память страницы.
- Переключатель, который меняет список предварительного кэша, требует нового worker. Байты скрипта должны измениться.
- По умолчанию держите флаг выключенным, если чтение не удалось, чтобы offline не включил недовыпущенную функцию.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

### PWA and E-commerce

<details>
<summary>Почему are PWAs well-suited for e-commerce platforms?</summary>

### Почему are PWAs well-suited for e-commerce platforms?

**Definition:**

Витрина — оболочка плюс свежие данные о товарах. Оболочку можно поставить и закэшировать. Каталог часто нельзя.

**How it works:**

- Cache-first рисует интерфейс offline — это польза отзывчивости и offline, которую описывает MDN. Список товаров, который расстроит пользователей устаревшим, остаётся только сетью.
- Установка из браузера или, где вы упаковываете, из магазина по правилам устанавливаемости. Тот же сайт всё ещё работает как сайт там, где установки нет.
- Push может сказать подписчику, что заказ изменился, и worker покажет уведомление. Для этого нужны активный worker и секретный endpoint.
- Платежи — это POST или Payment Request, а не закэшированный GET.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [MDN — Payment Request API](https://developer.mozilla.org/en-US/docs/Web/API/Payment_Request_API)
</details>

<details>
<summary>Как a PWA improve conversion rates for an e-commerce site?</summary>

### Как a PWA improve conversion rates for an e-commerce site?

**Definition:**

На страницах, которые у нас есть, нет процента конверсии. Что они говорят: закэшированная оболочка рисуется без сети, а устанавливаемое приложение запускается как платформенное.

**How it works:**

- First Contentful Paint — первый текст или изображение. Предварительно закэшированный документ может дать это, не дожидаясь сети.
- Продвижение установки требует манифест и HTTPS. Браузер, который не может поставить, всё равно получает сайт — это путь прогрессивного улучшения.
- Устаревший каталог — не функция конверсии. MDN говорит, что некоторые данные должны оставаться только сетью.
- Не цитируйте число до и после, которого в документации нет.

**Sources:**

- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Что такое the unique challenges of developing PWAs for e-commerce?</summary>

### Что такое the unique challenges of developing PWAs for e-commerce?

**Definition:**

Трудные места — свежесть, платежи и браузеры, которые ставят не одинаково.

**How it works:**

- Список товаров и цена не должны быть cache-first. Стратегия только сети у MDN — для ответов, которые неверны старыми.
- Оформление заказа — POST. Кэшировать POST никогда не уместно. Payment Request — способ выбрать сохранённый инструмент, а не новая платёжная сеть, и это не то, что кэшируют.
- Офлайн-корзины живут в IndexedDB, затем синхронизация, когда сеть вернулась. Остаток мог измениться. Сервер должен отвергнуть устаревшее оформление.
- На iOS нет beforeinstallprompt. Десктопный Firefox не ставит из манифеста. Сайт всё равно должен продавать во вкладке.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Payment Request API](https://developer.mozilla.org/en-US/docs/Web/API/Payment_Request_API)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Как бы вы handle checkout and payment processing in an e-commerce PWA?</summary>

### Как бы вы handle checkout and payment processing in an e-commerce PWA?

**Definition:**

Payment Request позволяет пользователю выбрать сохранённый способ оплаты и передаёт этот выбор продавцу. Это не новая платёжная сеть и не закэшированный ответ.

**How it works:**

- MDN: он даёт согласованный опыт продавцам и пользователям. Брошенные корзины — задача, на которую он нацелен, избегая длинной своей формы, когда сохранённый инструмент есть.
- Само списание всё ещё происходит на вашем сервере. API выбирает и возвращает данные инструмента, которые запросил продавец.
- Не перехватывайте этот поток обработчиком fetch в режиме cache-first. Платёжный POST входит в набор, который MDN запрещает кэшировать.
- Проверяйте наличие PaymentRequest. Где его нет, покажите свою форму. Это то же прогрессивное улучшение, что и установка.

**Sources:**

- [MDN — Payment Request API](https://developer.mozilla.org/en-US/docs/Web/API/Payment_Request_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Можете ли вы discuss strategies for integrating analytics in an e-commerce PWA?</summary>

### Можете ли вы discuss strategies for integrating analytics in an e-commerce PWA?

**Definition:**

Аналитика — сетевой вызов. Она не должна блокировать First Contentful Paint, и неудачная отправка не должна ломать оформление заказа.

**How it works:**

- FCP — первый текст или изображение. Скрипт, который ждёт ответ аналитики перед отрисовкой, задерживает эту отметку.
- Шлите после того, как оболочка встала. Если устройство offline, поставьте событие в очередь IndexedDB и повторите тегом sync. POST аналитики не кэшируйте.
- Service worker может слушать fetch, но кэшировать POST никогда не уместно. Пусть запрос аналитики идёт в сеть.
- Не кладите секреты в скрипт worker. Скрипт отдаётся каждому клиенту.

**Sources:**

- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — Offline and background operation](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation)
</details>

### APIs and Integration

<details>
<summary>Что такое the common APIs used in developing a PWA?</summary>

### Что такое the common APIs used in developing a PWA?

**Definition:**

API, которые в этих документах определяют PWA, — service worker, Cache API и манифест. Push, синхронизация и API устройства — добавки.

**How it works:**

- ServiceWorkerContainer.register, install, activate и fetch — это worker. Cache Storage держит ответы.
- Манифест даёт name, icons, start_url и display для установки.
- PushManager, sync.register и periodicSync.register покрывают уведомления и отложенную работу. Геолокация и getUserMedia покрывают GPS и камеру, оба в безопасном контексте, оба после запроса разрешения.
- IndexedDB покрывает структурированные офлайн-данные. Web Storage внутри worker не работает.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [MDN — Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API)
- [MDN — MediaDevices.getUserMedia()](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
</details>

<details>
<summary>Как PWAs integrate with native device features?</summary>

### Как PWAs integrate with native device features?

**Definition:**

Возможности устройства — это веб-API на безопасном origin, каждое за разрешением. Иконка установки их не выдаёт.

**How it works:**

- getUserMedia запрашивает камеру или микрофон и разрешается в MediaStream либо отклоняется, если пользователь отказал или устройство не подошло. Это API безопасного контекста.
- Геолокация спрашивает разрешение и затем сообщает координаты. Она тоже только в безопасном контексте.
- Push просит подписку. Уведомления показывает worker.
- Браузер, в котором API нет, оставляет вам сайт без этой возможности. Проверяйте её наличие.

**Sources:**

- [MDN — MediaDevices.getUserMedia()](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia)
- [MDN — Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API)
- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>Обсудите how a PWA can access a user's camera or GPS.</summary>

### Обсудите how a PWA can access a user's camera or GPS.

**Definition:**

Доступ к камере — getUserMedia. GPS — Geolocation API. Оба требуют безопасный контекст и разрешение пользователя.

**How it works:**

- getUserMedia() запрашивает медиавход и возвращает MediaStream. Видеодорожка может идти с камеры. Если пользователь отказал в разрешении или ничего не подошло, промис отклоняется.
- getCurrentPosition или watchPosition геолокации спрашивают разрешение и затем дают широту и долготу. Из соображений приватности браузер спрашивает до отчёта.
- Ни один из вызовов не принадлежит обработчику install service worker. У worker нет DOM, и эти запросы принадлежат жесту пользователя на странице.
- Нужны HTTPS или localhost. Небезопасная страница эти API не получает.

**Sources:**

- [MDN — MediaDevices.getUserMedia()](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia)
- [MDN — Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>Как you handle cross-origin requests in a PWA?</summary>

### Как you handle cross-origin requests in a PWA?

**Definition:**

Кросс-доменные запросы удаются, только если другой origin так сказал. Service worker не выключает CORS.

**How it works:**

- CORS — механизм, которым сервер перечисляет origin, кроме своего, с которых браузер может загружать ресурсы. Браузер может послать предварительный запрос, чтобы проверить, что настоящий запрос разрешат.
- fetch из worker — всё ещё fetch. Кросс-доменный ответ без нужных заголовков падает так же, как на странице.
- cache.put ответа, который вам не разрешили прочитать, не делает его читаемым позже.
- Учётные данные в кросс-доменном запросе требуют, чтобы сервер разрешил их явно. Не используйте origin-маску вместе с учётными данными.

**Sources:**

- [MDN — CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
- [MDN — FetchEvent](https://developer.mozilla.org/en-US/docs/Web/API/FetchEvent)
</details>

<details>
<summary>Как third-party services be integrated into a PWA?</summary>

### Как third-party services be integrated into a PWA?

**Definition:**

Сторонняя служба — другой origin. Грузите её, только если этот origin шлёт заголовки CORS, которые нужны вашему fetch, и не кладите в предварительный кэш скрипт, который вы не контролируете.

**How it works:**

- Кросс-доменный скрипт или API без CORS падает. Worker это не смягчает.
- Чужой URL в addAll делает установку зависимой от их доступности. Одна неудача отклоняет весь предварительный кэш.
- Payment Request и подписки push — точки интеграции, которые эти документы реально описывают. У каждой своё разрешение и свой сервер.
- Менеджер тегов, который вставляет скрипты, задерживает First Contentful Paint. Грузите его после оболочки.

**Sources:**

- [MDN — CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
- [MDN — Payment Request API](https://developer.mozilla.org/en-US/docs/Web/API/Payment_Request_API)
- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
</details>

### Advanced PWA Topics

<details>
<summary>Что такое Progressive Enhancement and Graceful Degradation in the context of PWAs?</summary>

### Что такое Progressive Enhancement and Graceful Degradation in the context of PWAs?

**Definition:**

Прогрессивное улучшение значит, что сайт работает до дополнительной возможности. Изящная деградация значит, что более богатый путь мягко возвращается к этому сайту.

**How it works:**

- Руководство по установке MDN говорит, что браузер, который не может поставить PWA, всё ещё пользуется им как обычным сайтом. В этом и есть улучшение: установка, worker и push — добавки.
- Проверяйте serviceWorker, BeforeInstallPromptEvent, PushManager и PaymentRequest. Отсутствующие пропускают эту ветку.
- Worker, который не установился, должен оставить сетевой путь рабочим. Не делайте документ зависимым от respondWith.
- iOS без beforeinstallprompt — случай деградации для своей кнопки установки. Меню «Поделиться» остаётся.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — beforeinstallprompt](https://developer.mozilla.org/en-US/docs/Web/API/Window/beforeinstallprompt_event)
</details>

<details>
<summary>Как you implement a periodic background sync in PWA?</summary>

### Как you implement a periodic background sync in PWA?

**Definition:**

Периодическая фоновая синхронизация регистрирует тег, который браузер может вызвать по расписанию. Это не одноразовая Background Sync, и она сильно ограничена.

**How it works:**

- periodicSync.register(tag, { minInterval }) на активной регистрации. minInterval — минимум в миллисекундах, а не обещание.
- InvalidStateError, если нет активного worker. NotAllowedError, если разрешение periodic-background-sync не дано. InvalidAccessError, если окно не верхнего уровня.
- В Chrome разрешение дают только установленному приложению, и Chrome ещё использует оценку вовлечённости, чтобы решить, срабатывать ли и как часто.
- Worker обрабатывает событие periodicsync и делает fetch внутри waitUntil. DOM он по-прежнему не трогает.

**Examples:**

```js
const reg = await navigator.serviceWorker.ready;
await reg.periodicSync.register("fetch-news", { minInterval: 24 * 60 * 60 * 1000 });
```

**Sources:**

- [MDN — PeriodicSyncManager.register()](https://developer.mozilla.org/en-US/docs/Web/API/PeriodicSyncManager/register)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>Обсудите the concept of Credential Management API in PWAs.</summary>

### Обсудите the concept of Credential Management API in PWAs.

**Definition:**

Credential Management API позволяет странице спросить у браузера сохранённые учётные данные вместо своей формы пароля, когда браузер это поддерживает.

**How it works:**

- Проверяйте `"credentials" in navigator`, затем credentials.get({ password: true }). Если свойства нет, оставьте старый вход.
- Это API страницы. Service worker подсказку не показывает и DOM не имеет.
- Он не заменяет сессию. Учётные данные всё равно отправляют на сервер. Этот POST не кэшируйте.
- Отсутствующий API — случай прогрессивного улучшения, а не ошибка.

**Sources:**

- [MDN — Navigator.credentials](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/credentials)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

<details>
<summary>Что такое the Payment Request API and how does it simplify transactions in a PWA?</summary>

### Что такое the Payment Request API and how does it simplify transactions in a PWA?

**Definition:**

Payment Request API позволяет пользователю выбрать предпочтительный способ оплаты и отдаёт эти сведения продавцу. Это не новый способ платежа.

**How it works:**

- MDN описывает согласованный опыт оформления и связывает его с брошенными корзинами из-за трения на кассе.
- Вы запрашиваете методы, которые поддерживаете. Браузер показывает те, что есть у пользователя. Списание всё ещё ваше, на сервере.
- Проверяйте API. Откатывайтесь к своей форме. Платёжный запрос не кэшируйте.
- Service worker не должен отвечать на платёжный POST из кэша.

**Sources:**

- [MDN — Payment Request API](https://developer.mozilla.org/en-US/docs/Web/API/Payment_Request_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

<details>
<summary>Как a PWA be part of the Internet of Things (IoT)?</summary>

### Как a PWA be part of the Internet of Things (IoT)?

**Definition:**

PWA не становится устройством в локальной сети оттого, что его можно поставить. Worker умеет синхронизировать и принимать push. Он не держит DOM и не держит гарантированное фоновое радио.

**How it works:**

- Service worker просыпается для push и для sync. Периодическая синхронизация ограничена и не является постоянным сокетом к датчику.
- Геолокация и getUserMedia работают на странице, в безопасном контексте, после разрешения. Их нет как тихого цикла в worker.
- Если отсчёт надо сохранить offline, положите структурированный образец в IndexedDB и синхронизируйте позже. Cache API — неверное хранилище для журнала датчика.
- Заметка MDN о будущем на странице worker упоминает геозону как будущий хук, а не как текущий API, который можно вызвать.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — PeriodicSyncManager.register()](https://developer.mozilla.org/en-US/docs/Web/API/PeriodicSyncManager/register)
- [MDN — Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
</details>

### Platform-Specific Considerations

<details>
<summary>Как you optimize a PWA for iOS devices?</summary>

### Как you optimize a PWA for iOS devices?

**Definition:**

На iOS установка — меню «Поделиться», а не beforeinstallprompt, и старые iOS ставят только из Safari.

**How it works:**

- iOS 16.3 и раньше: только Safari. iOS 16.4 и новее: «Поделиться» в Safari, Chrome, Edge, Firefox и Orion. beforeinstallprompt нет, поэтому не ждите это событие, чтобы показать подсказку установки.
- Манифест всё ещё важен для name, icons и display standalone, когда пользователь добавил приложение.
- HTTPS обязателен, как и везде. Service worker — по-прежнему офлайн-путь.
- Не обещайте поведение WebAPK. Это путь Android Chrome и Samsung Internet.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>Что такое the current limitations for PWAs on the Apple App Store?</summary>

### Что такое the current limitations for PWAs on the Apple App Store?

**Definition:**

Путь установки Apple — не путь Chromium. MDN не описывает отдельную квоту проверки App Store для PWA. Что он говорит — как iOS ставит из браузера.

**How it works:**

- Упаковка для магазинов своя у каждого магазина. MDN указывает на PWABuilder и перечисляет Play, Microsoft, Meta Quest и iOS App Store как цели упаковки. Правила установки из браузера отдельные.
- На iOS пользователь ставит из меню «Поделиться», с ограничениями версий выше. beforeinstallprompt не сработает.
- PWA, которое зависит от API, которого нет в iOS, должно сохранить путь обычного сайта. Это прогрессивное улучшение.
- Не выдумывайте причину отказа App Store, которой на странице нет.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Обсудите the differences between optimizing PWAs for Android vs. iOS.</summary>

### Обсудите the differences between optimizing PWAs for Android vs. iOS.

**Definition:**

Android Chromium может поставить WebAPK и вызвать beforeinstallprompt. iOS ставит из меню «Поделиться» и это событие не вызывает.

**How it works:**

- WebAPK с настоящей записью в лаунчере — это Chrome на устройствах с Google Mobile Services и Samsung Internet на устройствах Samsung. Другие браузеры Android добавляют ярлык.
- iOS 16.4 и новее позволяет ставить из меню «Поделиться» нескольких браузеров. Более ранние iOS — только Safari.
- description и screenshots в подсказке установки — поведение Android. На iOS их не ждите.
- Манифест, HTTPS и service worker общие. Подсказка — нет.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — beforeinstallprompt](https://developer.mozilla.org/en-US/docs/Web/API/Window/beforeinstallprompt_event)
</details>

<details>
<summary>Как the choice of web browser affect the behavior of a PWA?</summary>

### Как the choice of web browser affect the behavior of a PWA?

**Definition:**

Один и тот же origin может быть установленным приложением, ярлыком или только вкладкой, в зависимости от браузера.

**How it works:**

- Десктопный Chromium ставит из манифеста. Десктопный Safari с Sonoma умеет Add to Dock с манифестом и без. Десктопный Firefox из манифеста не ставит. Некоторые браузеры всё ещё могут поставить любой сайт, не продвигая его.
- Android WebAPK против ярлыка со значком — список браузеров выше. iOS — меню «Поделиться».
- Проверяйте возможности. Не ветвитесь по строке user-agent, в которой ошибётесь.
- Worker и кэш работают только там, где service workers есть, и только в безопасном контексте.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

### Testing & Debugging PWAs

<details>
<summary>Какие стратегии do you use for testing the Service Workers?</summary>

### Какие стратегии do you use for testing the Service Workers?

**Definition:**

Проверяйте регистрацию, жизненный цикл, кэш и перезагрузку в offline.

**How it works:**

- register() должен разрешиться. Синтаксическая ошибка его отклоняет. Затем смотрите installing, waiting и activated.
- Вызовите обновление, изменив байты worker и перезагрузив. Новый должен сесть в waiting, пока вы не вызовете skipWaiting или не закроете старых клиентов.
- Убедитесь, что URL из addAll возвращают 200. Одна неудача проваливает install.
- В Firefox workers по HTTP требуют открытые DevTools и включённую опцию. Проверьте и настоящий путь HTTPS.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — ServiceWorker.state](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorker/state)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
- [MDN — skipWaiting()](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/skipWaiting)
</details>

<details>
<summary>Как you simulate offline testing conditions for your PWA?</summary>

### Как you simulate offline testing conditions for your PWA?

**Definition:**

Режим offline браузера или упавшая сеть в обработчике fetch — это тест. Worker должен ответить оболочкой из кэша.

**How it works:**

- Cache-first возвращает caches.match, когда сети нет. Если вы ничего не закэшировали заранее, запасной путь — Response.error() или неудачная навигация.
- DevTools могут поставить сеть в offline и перезагрузить. Запрос документа — событие fetch с mode navigate.
- Не ограничивайтесь отключением Wi-Fi на машине, у которой ещё есть другой интерфейс. Настройка offline браузера — условие, которое видит отказ fetch.
- POST всё равно идёт в сеть и offline падает. Это правильно. Черновик уже должен быть в IndexedDB.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — FetchEvent](https://developer.mozilla.org/en-US/docs/Web/API/FetchEvent)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
</details>

<details>
<summary>Что такое the main aspects to test before deploying a PWA?</summary>

### Что такое the main aspects to test before deploying a PWA?

**Definition:**

Перед выкладкой worker устанавливается, оболочка грузится offline, манифест выполняет правила установки, и свежие данные не застревают в кэше.

**How it works:**

- HTTPS или localhost. Ссылка на манифест на каждой странице. Члены Chromium: name или short_name, иконки 192 и 512, start_url, display.
- Предварительный кэш install успешен. activate выбрасывает старые имена кэшей. Вторая загрузка offline рисует оболочку.
- Цены, входящие и POST — network-first или только сеть.
- Push, если вы его выпускаете, имеет секретный endpoint и видимое уведомление.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
</details>

<details>
<summary>Как you debug a PWA on a mobile device?</summary>

### Как you debug a PWA on a mobile device?

**Definition:**

Отлаживайте телефон на том же HTTPS-origin и смотрите worker из удалённого инспектора десктопа, когда браузер это позволяет.

**How it works:**

- Повторите установку из меню «Поделиться» на iOS или с иконки установки на Android. beforeinstallprompt на iOS не появится.
- Offline на устройстве — настоящий отказ fetch. Переключатель offline на десктопе не доказывает хранилище телефона.
- Нужен безопасный контекст. Сертификат, которому телефон не доверяет, worker не зарегистрирует.
- Логируйте statechange. Redundant или застрявшее installing — та же ошибка, что на десктопе.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — ServiceWorker.state](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorker/state)
</details>

<details>
<summary>Что такое common issues encountered when testing push notifications and how do you address them?</summary>

### Что такое common issues encountered when testing push notifications and how do you address them?

**Definition:**

Обычные отказы — отказ в разрешении, мёртвая подписка, тихий push, который Firefox квотирует, и обработчик щелчка, который считает, что страница открыта.

**How it works:**

- subscribe требует активного worker и жест пользователя, который может показать запрос разрешения. Отклонённое разрешение заканчивает поток.
- Храните endpoint надёжно и обновляйте его на pushsubscriptionchange. Старый endpoint не доставляет.
- Покажите уведомление. Firefox ограничивает push, которые этого не делают. Квота обновляется при визите на сайт. Chrome эту квоту не использует.
- Обрабатывайте щелчок в worker через waitUntil и фокусируйте или открывайте окно. Слушатель только на странице пропускает щелчок, когда приложение закрыто.

**Sources:**

- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

### PWA Case Studies and Real-life Applications

<details>
<summary>Можете ли вы provide examples of successful PWAs?</summary>

### Можете ли вы provide examples of successful PWAs?

**Definition:**

Эти документы не называют компанию-победителя. Они описывают свойства успешной установки: манифест, HTTPS и worker, который умеет кэшировать.

**How it works:**

- Chromium продвигает приложение с нужными членами манифеста. Подсказка показывает name и icons.
- Закэшированная оболочка грузится без сети. Это утверждение об offline и отзывчивости, которое делает MDN.
- Браузер, который не может поставить, всё ещё имеет сайт. Успех включает этот путь, а не только иконку.
- Не прикрепляйте число трафика, которого страница не публикует.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

<details>
<summary>Обсудите how PWAs have impacted mobile traffic for major websites.</summary>

### Обсудите how PWAs have impacted mobile traffic for major websites.

**Definition:**

MDN не публикует цифру трафика до и после для крупного сайта. Механизм, который он описывает, — закэшированная оболочка и устанавливаемый манифест.

**How it works:**

- Повторный визит может нарисоваться из кэша — это польза отзывчивости, отдельно от offline.
- Установка добавляет иконку и окно standalone. Сам HTML она не меняет.
- Где браузер не ставит, трафик остаётся на обычном сайте. Десктопный Firefox и старые iOS в этом наборе.
- Любой процент, который вы видели в другом месте, на этих страницах отсутствует. Не повторяйте его как факт.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Как the user enGAgement changed pre and post PWA implementation for a notable company?</summary>

### Как the user enGAgement changed pre and post PWA implementation for a notable company?

**Definition:**

И снова: эти страницы не дают дельту вовлечённости для названной компании. Функции вовлечённости, которые они описывают, — установка, офлайн-отрисовка и push по согласию.

**How it works:**

- Установка — подсказка, а не принудительная иконка на домашнем экране. Пользователь может отказаться.
- Push требует подписку и, чтобы избежать квоты Firefox, видимое уведомление.
- Offline помогает, только если оболочку заранее закэшировали и стратегия данных совпадает с ресурсом.
- Сообщайте о вовлечённости из своей аналитики по факту. Не заимствуйте разбор, которого в документации нет.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

<details>
<summary>С какими challenges столкнулись developers при переходе traditional web app в PWA?</summary>

### С какими challenges столкнулись developers при переходе traditional web app в PWA?

**Definition:**

Переход — это добавить манифест, HTTPS, если его не было, и worker, который кэширует намеренно. Сайт должен продолжать работать там, где это не удаётся.

**How it works:**

- Каждой странице нужна ссылка на манифест. Worker не должен зависеть от синхронного XHR или Web Storage.
- Выберите стратегию на ресурс. Кэш всего origin, включая POST и цены, — ошибка, ради предотвращения которой у MDN есть список стратегий.
- Обновления ждут закрытия старых страниц, если вы не вызвали skipWaiting. Пользователи могут сидеть на старом worker.
- iOS и Firefox не пройдут сценарий проверки установки Chromium. Проверяйте эти пути отдельно.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — skipWaiting()](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/skipWaiting)
</details>

### The Future of PWAs

<details>
<summary>Какие future advancements вы ожидаете в области PWAs?</summary>

### Какие future advancements вы ожидаете в области PWAs?

**Definition:**

MDN уже указывает несколько расширений: статические маршруты, чтобы worker не обязан стартовать, периодическая синхронизация под жёсткими пределами и push. Всё сверх этого на этих страницах не обещано.

**How it works:**

- InstallEvent.addRoutes() пропускает старт worker для URL, источник которых вы уже знаете.
- Периодическая фоновая синхронизация остаётся за разрешением, а в Chrome остаётся связанной с установленным приложением и вовлечённостью.
- Push уже будит worker, чтобы показать уведомление. Геозона перечислена как будущее использование контекста worker, а не как API, который можно выпускать.
- Долговечная часть не меняется: HTTPS, манифест и явная стратегия кэша.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — PeriodicSyncManager.register()](https://developer.mozilla.org/en-US/docs/Web/API/PeriodicSyncManager/register)
- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
</details>
