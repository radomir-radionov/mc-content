# PWA — Interview

Answers are filled.

[← Back to hub](./6_web_api.md)

### Related docs

- [Web APIs — Apps](./6_web_api_apps.md) · [WebSockets — Interview](./6_web_api_websockets_interview.md) · [PWA — Interview](./6_web_api_pwa_interview.md) · [Web APIs — Interview](./6_web_api_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — PWA](https://devinterview.io/questions/web-and-mobile-development/pwa-interview-questions/).

### PWA Fundamentals

<details>
<summary>What is a Progressive Web App (PWA)?</summary>

### What is a Progressive Web App (PWA)?

**Definition:**

A progressive web app is a site that can be installed and, when you add a service worker, keep working from cached resources. It is still the web: one URL, one codebase, and the browser as the runtime.

**How it works:**

- A web app manifest (name, icons, `start_url`, `display`) tells the browser how the installed app should look.
- HTTPS, or localhost in development, is required before a browser will offer installation.
- A service worker is a separate script that can intercept fetches, fill the Cache API, and receive push events. Current Chromium install rules do not require it; offline behavior does.
- Installed, the app can open without the browser chrome when `display` is `standalone`.
- Updates ship when the user is online and the new service worker takes control. There is no store review unless you also package the site for a store.
- The page still works as a normal site in a browser that never installs it.

**Sources:**

- [MDN — Progressive web apps](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps)
</details>

<details>
<summary>How do PWAs differ from traditional web applications?</summary>

### How do PWAs differ from traditional web applications?

**Definition:**

A traditional site expects the network for each navigation and lives in a browser tab. A PWA can be installed, open in its own window, and answer navigations from a cache when you give it a service worker.

**How it works:**

- Installability comes from the manifest and a secure context. The icon then sits next to other apps.
- A normal site has no install prompt and no standalone window unless the user happens to bookmark it.
- Push and background sync need a service worker. A plain page cannot receive a push after it closes.
- Offline is a cache strategy, not a property of “being a PWA”. Without a fetch handler, the installed app still fails offline.
- Both are linkable and indexable. A store listing is optional packaging, not the definition.
- Responsive layout is shared by any modern site. It does not by itself make the site installable.

**Examples:**

```html
<link rel="manifest" href="/manifest.webmanifest" />
```

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Can you list the core pillars of a PWA?</summary>

### Can you list the core pillars of a PWA?

**Definition:**

The parts that show up in every serious PWA are a secure origin, a manifest, a service worker, a layout that fits the screen, and a fast first load. Install and offline are the two user-visible results.

**How it works:**

- HTTPS (or localhost) is the gate for service workers and for the install prompt.
- The manifest supplies the name, icons, start URL, and display mode.
- The service worker caches the app shell and can serve it when the network fails.
- Responsive layout uses the same CSS you would use for any site. The installed window is just another viewport.
- An app shell is the stable frame (header, nav, empty content area) cached up front, with data filled in after.
- “Fast” and “reliable” are outcomes of caching and small assets, not extra file formats.
- Engagement features — install, push, background sync — sit on top of the worker and are optional.

**Sources:**

- [MDN — Progressive web apps](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps)
</details>

<details>
<summary>What are the advantages of developing a PWA over a native app?</summary>

### What are the advantages of developing a PWA over a native app?

**Definition:**

A PWA ships as a website: one codebase, a URL, and updates the next time the browser fetches them. A native app gets deeper device access and the store’s distribution, at the cost of a separate build per platform.

**How it works:**

- You write HTML, CSS, and JavaScript once. iOS and Android builds are not required for the browser install path.
- Users can open the URL before they install anything. Search indexes the pages.
- A new deploy replaces cached assets when the service worker updates. Users do not go to a store for each fix.
- The installed package is often smaller because the browser already supplies the runtime.
- Native code still wins for heavy graphics, background execution rules, and APIs the web does not expose.
- Stores can distribute a PWA too (packaging tools wrap it), but that is optional. The site remains the source.

**Sources:**

- [MDN — What is a progressive web app?](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/What_is_a_progressive_web_app)
</details>

<details>
<summary>What is the role of the service worker in a PWA?</summary>

### What is the role of the service worker in a PWA?

**Definition:**

A service worker is a script the browser runs apart from the page. It can intercept network requests, read and write the Cache API, and receive push and background-sync events even when the page is closed.

**How it works:**

- It is registered from the page, then installed, activated, and later replaced when a new script byte-differs.
- A `fetch` handler decides cache-first, network-first, or a fallback response. That is the offline behavior.
- It does not touch the DOM. It posts messages to the page, which updates the UI.
- Push messages arrive on the worker. The worker shows a notification if the user granted permission.
- Background Sync (a Chromium API) lets the worker retry a queued write when connectivity returns.
- The worker only runs on a secure origin, and only for the scope you registered.

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
<summary>How do you make a web app installable on a user's home screen?</summary>

### How do you make a web app installable on a user's home screen?

**Definition:**

Serve the site over HTTPS and link a manifest that meets the browser’s install rules. The browser then offers installation; you do not implement a home-screen API yourself.

**How it works:**

- Chromium asks for `name` or `short_name`, icons at 192 and 512, `start_url`, and `display` or `display_override`. `prefer_related_applications` must not be true.
- Every page that should count includes `<link rel="manifest" href="...">`.
- Localhost is allowed during development. A `file://` page is not installable.
- A service worker is how you add offline. MDN’s current install criteria do not list it as a requirement for the Chromium prompt.
- `beforeinstallprompt` (Chromium, not iOS) lets you show your own button and call `prompt()` on the event.
- iOS uses the Share sheet. There is no `beforeinstallprompt` there.
- After install, `display: "standalone"` opens the app without browser tabs.

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
<summary>What is a manifest file and what is its significance in PWAs?</summary>

### What is a manifest file and what is its significance in PWAs?

**Definition:**

The web app manifest is a JSON file that describes the installed app: its name, icons, start URL, colors, and whether it opens as a standalone window. Without it, Chromium will not promote the site for installation.

**How it works:**

- `name` and `short_name` are the labels. `icons` needs several sizes so the launcher does not scale one bitmap badly.
- `start_url` is the page that opens from the icon. `scope` limits which URLs stay inside the installed window.
- `display` is usually `standalone` for an app window or `browser` for a normal tab. `display_override` lists fallbacks.
- `background_color` shows before the stylesheet loads. `theme_color` tints the surrounding UI.
- `id` gives the app a stable identity when `start_url` changes.
- The HTML page points at the file with `<link rel="manifest">`. A manifest that 404s means the site is not installable.

**Examples:**

```html
<link rel="manifest" href="/manifest.webmanifest" />
```

**Sources:**

- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
</details>

<details>
<summary>What is the purpose of the 'Add to Home Screen' feature?</summary>

### What is the purpose of the 'Add to Home Screen' feature?

**Definition:**

Add to Home Screen turns the site into a launcher icon that opens your start URL in a standalone window. The point is a repeat visit in one tap, not a second copy of the code.

**How it works:**

- The browser shows its own install UI when the manifest qualifies. Your page can add a button that calls `prompt()` on `beforeinstallprompt` where that event exists.
- The icon and name come from the manifest, so the launcher matches the product.
- Launch uses `start_url` and `display`. `standalone` hides the URL bar.
- Offline still depends on the service worker. The icon alone does not cache anything.
- Engagement is the practical result: the app stays on the device and can receive push if the user allowed it.
- iOS installs from the Share menu. The Chromium event is not how that platform works.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>How can you detect if your web app has been added to the user's home screen?</summary>

### How can you detect if your web app has been added to the user's home screen?

**Definition:**

You detect the display mode the window was opened with, not a permanent “installed” bit on the device. `matchMedia("(display-mode: standalone)")` is the cross-browser check, with `navigator.standalone` as the older iOS signal.

**How it works:**

- When the manifest says `display: "standalone"` and the user opened the icon, the media query matches.
- A tab on the same origin does not match, so the same build can hide an install button only in the installed window.
- iOS Safari has long exposed `navigator.standalone === true` for a home-screen web app.
- `beforeinstallprompt` tells you the Chromium browser is willing to install. It does not tell you that installation already happened. The `appinstalled` event fires when the user accepts.
- `document.hidden` is about whether the tab is backgrounded. It is not an install check.
- There is no web API that lists every device where the user installed the app.

**Examples:**

```js
const installed = window.matchMedia("(display-mode: standalone)").matches
  || window.navigator.standalone === true;
```

**Sources:**

- [MDN — display-mode](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/display-mode)
</details>

<details>
<summary>Explain how PWAs achieve offline functionality.</summary>

### Explain how PWAs achieve offline functionality.

**Definition:**

Offline behavior is a service worker answering `fetch` from the Cache API, plus IndexedDB when the page needs structured data that must survive a reload. The manifest does not cache anything by itself.

**How it works:**

- On install, the worker opens a named cache and adds the shell: HTML, CSS, JS, and icons.
- A cache-first handler returns the cached response when it exists and falls through to the network otherwise. A network-first handler tries the network and uses the cache when that fails.
- Version the cache name. On activate, delete caches you no longer name so old assets do not stick.
- IndexedDB stores records (drafts, a last-known list). `localStorage` is a small synchronous string map and is a poor place for the dataset.
- Background Sync can replay a queued write later, in browsers that implement it. The queue itself usually lives in IndexedDB.
- Show the user when the response came from cache. A silent stale page looks like a bug.

**Examples:**

```js
const cache = await caches.open("shell-v3");
await cache.addAll(["/", "/app.css", "/app.js"]);
```

**Sources:**

- [MDN — Caching](https://developer.mozilla.org/en-US/docs/Web/API/Cache)
</details>

<details>
<summary>What are the security requirements for a PWA?</summary>

### What are the security requirements for a PWA?

**Definition:**

A PWA is a website with extra privileges, so it inherits the web’s rules: HTTPS, a same-origin service worker, and explicit permission for push. The manifest does not grant access to the camera or to other origins.

**How it works:**

- Service workers and the install prompt require a secure context. Ship TLS, and redirect HTTP to HTTPS.
- The worker’s script is same-origin (or allowed by the worker’s scope rules). A third-party script cannot silently become your fetch proxy.
- A Content-Security-Policy limits which scripts and connections the page may use, which matters more once the page is installed and looks like an app.
- Push and notifications require a user gesture and permission. Denial is a normal outcome.
- Cache only responses you trust. Caching an attacker’s error page, or an opaque cross-origin response you did not mean to keep, becomes the next offline load.
- An installed app is still the web. XSS in the page runs with the origin’s power, including the worker if you let the page register one.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>Can you delineate between App Shell and content in a PWA context?</summary>

### Can you delineate between App Shell and content in a PWA context?

**Definition:**

The app shell is the stable UI frame you cache so the window paints immediately. Content is the data that changes per user or per visit and is filled in after the shell is on screen.

**How it works:**

- Shell pieces are the header, navigation, and the empty region where results will go. They change when you deploy, not when the user taps.
- Content is the JSON, images, and text fetched for this session. Cache it with a shorter lifetime or a network-first strategy.
- The worker can return the shell for a navigation, then the page requests the data. Offline, the shell still paints and the data request fails visibly.
- Putting personalized HTML into the long-lived shell cache shows one user another user’s page.
- The split is a caching choice. It is not a separate file format the browser requires.

**Examples:**

```text
cache "shell-v3": /app.html, /app.css, /app.js
network, then cache: /api/notes
```

**Sources:**

- [MDN — Caching](https://developer.mozilla.org/en-US/docs/Web/API/Cache)
</details>

<details>
<summary>How does a PWA function on a low-bandwidth or offline network?</summary>

### How does a PWA function on a low-bandwidth or offline network?

**Definition:**

On a weak or missing network the service worker answers what it already cached and the page reads structured data from IndexedDB. Requests that were never cached fail, and the UI should say so.

**How it works:**

- A cache-first strategy avoids the slow network for the shell. A timeout around `fetch` can fall back to cache when the radio stalls rather than hanging.
- Precache the shell at install. Runtime caching stores responses the user actually asked for, such as the last article.
- IndexedDB holds drafts and the last list so a reload offline is not an empty screen.
- Background Sync, where it exists, queues a POST until the browser thinks the network is usable again.
- Images can use a small cached placeholder. Do not pretend a write succeeded if it is only queued.
- Service workers themselves require HTTPS, including when the later fetches are served from cache.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>What are push notifications in the context of PWAs?</summary>

### What are push notifications in the context of PWAs?

**Definition:**

Web push delivers a message to the service worker while the page is closed. The worker then shows a notification, if the user has granted permission and the browser has a push service.

**How it works:**

- The page asks for `Notification.permission`, then `pushManager.subscribe` with a VAPID public key. The subscription (endpoint plus keys) goes to your server.
- Your server sends the payload to that endpoint. The browser’s push service wakes the worker with a `push` event.
- The worker calls `showNotification`. A `notificationclick` handler focuses or opens a window.
- iOS supports web push for installed home-screen web apps, not for a random tab. Other browsers vary.
- Permission is per origin and easy to lose. Do not ask on the first paint.
- The push payload is small. The usual pattern is “something changed”, and the page fetches the detail when it opens.

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
<summary>Can you explain the concept of background sync in PWAs?</summary>

### Can you explain the concept of background sync in PWAs?

**Definition:**

Background Sync lets a service worker retry a write that failed because the device was offline. The page queues the work, and the browser fires a `sync` event when it believes the network is back.

**How it works:**

- The page stores the payload in IndexedDB and calls `registration.sync.register("send-note")`.
- The worker listens for `sync`. If `event.tag` matches, it reads the queue, sends it, and deletes the local copy on success.
- `event.waitUntil` keeps the worker alive until that promise settles. A rejection asks the browser to retry later, with backoff.
- This is the Background Synchronization API, implemented in Chromium. It is not a guarantee on Safari or Firefox, so the page still needs a retry when it is open.
- The sync does not run arbitrary timers. Periodic Background Sync is a separate, more restricted API.
- The server must treat the replay as idempotent. The same note can arrive twice if the response was lost.

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
<summary>What is a service worker lifecycle?</summary>

### What is a service worker lifecycle?

**Definition:**

A service worker is downloaded, then installed, then activated. A later byte-different script installs in the background and waits until the old worker's pages are gone, unless you skip waiting.

**How it works:**

- MDN's states are parsed, installing, installed (the waiting worker), activating, activated, and redundant. Redundant means a newer worker replaced this one, or install failed.
- The first visit downloads the script. Later updates happen on a navigation in scope, or when an event fires and the script has not been downloaded in the last 24 hours. Installation runs when the bytes differ.
- install is where you precache. activate is where you delete old caches. fetch is where you answer requests with respondWith. waitUntil on install or activate holds functional events until the promise resolves.
- skipWaiting moves a waiting worker to activating without waiting for old pages to close. clients.claim lets the active worker control already-open pages.

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
<summary>How do you handle compatibility issues across different browsers for PWAs?</summary>

### How do you handle compatibility issues across different browsers for PWAs?

**Definition:**

Install and offline behavior are not the same in every browser. Feature-detect the API, and treat a missing one as a normal site.

**How it works:**

- Chromium browsers install a manifest that has a name or short_name, 192 and 512 icons, start_url, and display or display_override, with prefer_related_applications absent or false. Firefox does not install from a manifest on desktop. Safari on macOS Sonoma and later can Add to Dock with or without a manifest.
- On Android, WebAPK install is Chrome with Google Mobile Services and Samsung Internet on Samsung devices. Other browsers add a home-screen shortcut. On iOS 16.3 and earlier, install is Safari only. From iOS 16.4, the Share menu in Safari, Chrome, Edge, Firefox, and Orion can install. beforeinstallprompt is not supported on iOS.
- Periodic background sync needs the permission, an installed app in Chrome, and Chrome also consults an engagement score. Push requires an active service worker. A missing API should not break the page.
- The site still has to be HTTPS, or localhost, to be installable. file:// is a secure context and is still not enough for installability.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — PeriodicSyncManager.register()](https://developer.mozilla.org/en-US/docs/Web/API/PeriodicSyncManager/register)
- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
</details>

<details>
<summary>How would you test a PWA?</summary>

### How would you test a PWA?

**Definition:**

Test the worker's states, the cache, and the install rules. A syntax error in the script rejects register().

**How it works:**

- Watch registration.installing, waiting, and active, and the statechange events. The states are installing, installed, activating, activated, and redundant.
- Confirm install's waitUntil finishes: if addAll fails, installation fails. Confirm activate deletes caches whose names are not on the allowlist.
- Load over HTTPS or localhost. Firefox can allow service workers on HTTP only when DevTools is open and that option is checked. A production HTTP origin will not register.
- Toggle offline in the browser and reload. Cache-first should still paint the precached shell. A POST must not be served from the cache; MDN says caching is never appropriate for POST.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — ServiceWorker.state](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorker/state)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
</details>

<details>
<summary>What is the role of HTTPS in PWAs?</summary>

### What is the role of HTTPS in PWAs?

**Definition:**

Service workers and installable PWAs require a secure context. Installability is stricter: HTTPS, or localhost, or 127.0.0.1.

**How it works:**

- MDN: service workers are only available in secure contexts because HTTP can be altered in the middle, and that would be worse with a worker that intercepts requests. Browsers treat http://localhost as secure so local development works.
- Installability does not accept file://, even though a secure context does. Every page of a multi-page app must link the manifest.
- Push, geolocation, and getUserMedia are also limited to secure contexts. An HTTP page loses those APIs and the worker together.
- Firefox's DevTools switch that allows workers on HTTP is a debugging aid, not a deployment plan.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API)
- [MDN — MediaDevices.getUserMedia()](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia)
</details>

<details>
<summary>What are the limitations of PWAs?</summary>

### What are the limitations of PWAs?

**Definition:**

A PWA is still a website. It does not get every native capability, and several of the ones it does get are missing or gated on some browsers.

**How it works:**

- No DOM access inside the worker, and no synchronous XHR or Web Storage there. The worker is asynchronous and runs off the main thread.
- Install UI differs: no beforeinstallprompt on iOS, no manifest install in desktop Firefox, and on many Android browsers the result is a shortcut rather than a WebAPK.
- Periodic sync is permission-gated and, in Chrome, limited to an installed app plus an engagement score. Firefox quotas push messages that do not show a notification; the quota refreshes when the site is visited. Chrome does not apply that quota.
- The cache can be evicted, and the user can clear it. A cache-first app still needs a network fallback or it becomes unusable after eviction.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — PeriodicSyncManager.register()](https://developer.mozilla.org/en-US/docs/Web/API/PeriodicSyncManager/register)
- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

### Service Workers

<details>
<summary>What is the role of the 'install' event in a service worker?</summary>

### What is the role of the 'install' event in a service worker?

**Definition:**

The install event fires when a registration acquires a new installing worker. It is the moment to fill the cache the worker will need.

**How it works:**

- Pass the caching promise to event.waitUntil. If that promise rejects, installation fails, so a later fetch is not handled by a worker whose precache is incomplete.
- MDN's example opens a cache and addAll of the app shell: HTML, CSS, JS, and images. Install of the worker is not the same as the user installing the PWA. The event runs when the script is downloaded, typically on the first visit.
- skipWaiting can be called here so the new worker does not sit in installed/waiting.
- The event does not bubble and is not cancelable. The type is ExtendableEvent.

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
<summary>How do you cache assets using service workers?</summary>

### How do you cache assets using service workers?

**Definition:**

Open a named Cache and store Request/Response pairs. Precache in install. Fill or refresh during fetch.

**How it works:**

- caches.open(name) then cache.addAll(urls) during install, inside waitUntil. addAll fails the install if any URL fails.
- On fetch, caches.match. A body can be read once, so cache.put gets response.clone() and the page gets the original.
- POST is not a caching candidate. JSON that must be fresh is a poor cache-first candidate. The app shell is the usual precache.
- Name the cache with a version. activate deletes every name that is not the current one.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
</details>

<details>
<summary>Can you explain how service worker updates are managed?</summary>

### Can you explain how service worker updates are managed?

**Definition:**

The browser downloads the worker again on an in-scope navigation, or on an event if the last download was more than 24 hours ago. A byte-different file becomes a waiting worker.

**How it works:**

- It does not replace the active worker while pages still use the old one. skipWaiting activates it sooner. clients.claim makes open pages use it without a reload.
- Change the cache name when the precache list changes, or activate will keep serving the old cache. MDN's cycle-tracker tutorial bumps a VERSION constant for that reason.
- A failed install leaves the old worker active. The new one becomes redundant.
- Users see the new shell on the next load after activation, unless you claimed the clients.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — skipWaiting()](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/skipWaiting)
- [MDN — Clients.claim()](https://developer.mozilla.org/en-US/docs/Web/API/Clients/claim)
- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
</details>

<details>
<summary>What different caching strategies can be implemented with service workers?</summary>

### What different caching strategies can be implemented with service workers?

**Definition:**

MDN names four strategies. Pick per resource: the shell wants speed, a product list wants freshness, and a POST wants the network only.

**How it works:**

- Cache first: match the cache, otherwise fetch, and store a clone if the response is ok. Best for precached UI that does not change until the next worker.
- Cache first with refresh, also called stale-while-revalidate: return the cache immediately and update it from the network in the background. MDN uses it when freshness matters somewhat and speed matters more, and the example skips JSON.
- Network first: fetch, store a clone, and on failure return the cache or Response.error(). MDN's example is an inbox.
- Network only: do not call respondWith, or do not cache. MDN says a product list that must not be stale belongs here, and POST is never appropriate to cache.

**When to use:**

Cache first for the app shell. Stale-while-revalidate for assets that can be a version behind. Network first for data that should be fresh but may be offline. Network only for payments, POST, and anything wrong when old.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

<details>
<summary>Describe how you would use a service worker to intercept network requests.</summary>

### Describe how you would use a service worker to intercept network requests.

**Definition:**

A fetch listener calls event.respondWith with a Response or a promise of one. If you do not call it, the browser performs its normal fetch.

**How it works:**

- The event fires for page navigations and for subresources, not only for fetch() from your script. It fires once the worker is active and the request is in scope.
- Read event.request. Skip non-GET if you do not intend to cache them. MDN's FetchEvent example returns early for non-GET.
- You may answer from caches.match, from fetch, or with a synthetic Response. A cached hit can still refresh with event.waitUntil(cache.add(request)).
- The worker has no DOM. Interception is the request and the response, not the page.

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
<summary>What is a FetchEvent in the context of service workers?</summary>

### What is a FetchEvent in the context of service workers?

**Definition:**

FetchEvent is the object passed to a fetch listener on the service worker. It describes the request and lets you supply the response.

**How it works:**

- event.request is the Request the browser intends to make. clientId is the same-origin client that started it. resultingClientId is the client that replaces it during navigation. preloadResponse is a promise of a preloaded navigation response, or undefined.
- respondWith prevents the default fetch and takes your Response or a promise of one. waitUntil, inherited from ExtendableEvent, extends the event for work after the response, such as updating the cache.
- handled is a promise pending until the event is handled. isReload is true when the user reloaded.
- The browser creates the event. You do not construct it in normal code. It exists only in service workers.

**Sources:**

- [MDN — FetchEvent](https://developer.mozilla.org/en-US/docs/Web/API/FetchEvent)
</details>

<details>
<summary>How do you handle errors in a service worker script?</summary>

### How do you handle errors in a service worker script?

**Definition:**

A thrown error or a rejected waitUntil promise fails that lifecycle step. A fetch handler that rejects respondWith fails that one request.

**How it works:**

- If the script has a syntax error, register() rejects. MDN says the file might be unavailable or contain a syntax error.
- If precache's addAll rejects inside install's waitUntil, the worker does not activate. The previous worker, if any, stays.
- Inside fetch, catch a network failure and return a cached fallback or Response.error(). An unhandled rejection of the respondWith promise is a failed load, not a skipped interception.
- Do not use synchronous XHR or Web Storage in the worker. Those APIs are unavailable because the worker is asynchronous.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
</details>

<details>
<summary>Discuss how service workers can be debugged.</summary>

### Discuss how service workers can be debugged.

**Definition:**

Debug the registration promise, the worker's state, and the cache contents. The worker is a separate script with no DOM.

**How it works:**

- Log the rejection from navigator.serviceWorker.register. A syntax error or a missing file shows up there.
- Read registration.installing, waiting, and active, and listen for statechange. Redundant means the worker was discarded.
- If install never reaches activated, the waitUntil promise rejected. A failed addAll is the usual cause.
- Firefox can run workers on HTTP only with DevTools open and the insecure-worker option enabled. Production still needs HTTPS or localhost. Offline reload checks whether the fetch handler actually answered.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — ServiceWorker.state](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorker/state)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
</details>

<details>
<summary>What is the 'activate' event in a service worker and what is it used for?</summary>

### What is the 'activate' event in a service worker and what is it used for?

**Definition:**

activate fires when the registration acquires a new active worker. It is the safe time to drop caches from the previous worker, because that worker is no longer running.

**How it works:**

- MDN deletes every cache name that is not on an allowlist, inside event.waitUntil. Functional events wait until that promise resolves.
- clients.claim() in this handler makes already-open clients controlled immediately, without a reload.
- Do the cleanup here, not in install. During install the old worker may still be serving those caches.
- The event does not bubble and is not cancelable.

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
<summary>Explain the concept of "cache busting" and why it's important for PWAs.</summary>

### Explain the concept of "cache busting" and why it's important for PWAs.

**Definition:**

Cache busting means a changed file is not served under a URL the cache still considers the same. For a service worker, sameness is the script bytes and the cache name.

**How it works:**

- The browser installs a new worker only when the downloaded script differs byte for byte. An unchanged service-worker.js does not become a new generation, so a new app shell listed in the old script is never precached.
- Version the cache name. activate deletes names that are not current, which is how the old shell leaves.
- Fingerprinted asset URLs make cache-first safe: a new bundle is a new request, so the old response is not a match.
- skipWaiting and clients.claim only speed up adoption of a worker that already installed. They do not detect a change you forgot to put in the script bytes.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

### Web App Manifest

<details>
<summary>What key properties does the Web App Manifest contain?</summary>

### What key properties does the Web App Manifest contain?

**Definition:**

A manifest is a JSON object. Members are optional in the spec, but Chromium's install rules require a specific set.

**How it works:**

- The example MDN ships has short_name, name, icons (192 and 512), start_url, display, theme_color, and background_color.
- Chromium installability wants name or short_name, icons at 192 and 512, start_url, display and/or display_override, and prefer_related_applications false or absent.
- Link it with `<link rel="manifest" href="manifest.json">`. The preferred type is application/manifest+json. Browsers also accept .json.
- name, background_color, and icons feed the splash screen on some systems. display standalone hides browser UI.

**Sources:**

- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>How do you define the start URL in a manifest file?</summary>

### How do you define the start URL in a manifest file?

**Definition:**

start_url is the manifest member the browser loads when the installed app is launched.

**How it works:**

- MDN's example sets `"start_url": "."`. It is relative to the manifest URL.
- Chromium requires start_url for installability. A multi-page app still needs the manifest link on every page.
- It is not the scope. scope limits which URLs the installed app treats as inside itself. start_url is the first page.
- A start_url outside the scope is not a usable launch target. Keep it inside the scope you declared.

**Examples:**

```json
{ "start_url": ".", "scope": "/", "display": "standalone" }
```

**Sources:**

- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>What are the guidelines for designing app icons for a PWA?</summary>

### What are the guidelines for designing app icons for a PWA?

**Definition:**

Chromium installability requires an icons entry that includes a 192px icon and a 512px icon.

**How it works:**

- Each icon has src, sizes, and type. MDN's example uses image/png.
- The install prompt shows the name and the icon from those members. On Android, description and screenshots are also shown when you provide them.
- The splash screen uses name, background_color, and icons.
- A single blurry icon fails the 192-and-512 rule even if the rest of the manifest is present.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
</details>

<details>
<summary>How does the web app manifest affect the appearance of the PWA on a device?</summary>

### How does the web app manifest affect the appearance of the PWA on a device?

**Definition:**

The manifest tells the OS how the installed app looks: name, icon, colors, and whether browser chrome is hidden.

**How it works:**

- display standalone looks like a platform app, without browser UI. display browser opens a normal tab. If a mode is unsupported, display falls back along a fixed sequence. display_override replaces that sequence.
- theme_color and background_color color the surrounding UI and the splash screen. The splash uses name, background_color, and icons.
- The install prompt uses name and icons. On Android it can also show description and screenshots.
- Without a manifest, some browsers can still install any site as an app, but they will not promote it or apply these members.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
</details>

<details>
<summary>Can you dynamically generate a web app manifest or must it be static?</summary>

### Can you dynamically generate a web app manifest or must it be static?

**Definition:**

The manifest is a JSON response the browser fetches from the link in the page. It does not have to be a file committed as static bytes, but the response has to be JSON the browser can parse.

**How it works:**

- `<link rel="manifest" href="...">` is the hook. A generated URL is fine if it returns the object. If the fetch needs credentials, set crossorigin="use-credentials" even for a same-origin URL.
- The type should be application/manifest+json. Browsers also accept application/json.
- Every page must point at a manifest for a multi-page app. A manifest that appears only after client-side script runs is late for the browser's install check.
- Chromium still requires the install members on whatever JSON you generated. Generating it does not relax name, icons, start_url, and display.

**Sources:**

- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

### User Engagement & Experience

<details>
<summary>How do push notifications enhance user engagement in a PWA?</summary>

### How do push notifications enhance user engagement in a PWA?

**Definition:**

Push lets a server wake the service worker and show a notification after the user has subscribed. The page does not have to be open.

**How it works:**

- An active service worker calls PushManager.subscribe(). The PushSubscription carries an endpoint and the encryption key. The endpoint is a capability URL: anyone who has it can push, so keep it secret.
- Incoming messages fire the push event. The worker can call registration.showNotification(). Firefox quotas pushes that do not display a notification; the quota refreshes on visit. Chrome has no such quota.
- Protect the subscribe endpoint against CSRF. MDN warns about that specifically.
- Push does not run without a worker, and the worker does not run without a secure context.

**Sources:**

- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>What strategies can you use to prompt the user to install your PWA?</summary>

### What strategies can you use to prompt the user to install your PWA?

**Definition:**

Meet the installability criteria and then either let the browser show its own icon or call prompt() on a saved beforeinstallprompt event.

**How it works:**

- Chromium promotes an app that is served over HTTPS or localhost and has the required manifest members. The prompt shows name and icons.
- beforeinstallprompt fires when the browser has decided the app is installable. preventDefault, keep the event, and call prompt() from your own button. Read result.outcome.
- That event is not supported on iOS. There the user installs from the Share menu.
- Do not show your button until the event fires. Before that the app is not installable yet.

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
<summary>How would you customize the prompt that asks users to add your PWA to their home screen?</summary>

### How would you customize the prompt that asks users to add your PWA to their home screen?

**Definition:**

You can defer the browser prompt and add context. You cannot redesign the browser's dialog itself, except for the fields the manifest already feeds it.

**How it works:**

- preventDefault on beforeinstallprompt stops the default UI. Your button later calls prompt().
- name and icons are always in the prompt. On Android, description and screenshots are shown too when the manifest has them.
- iOS has no beforeinstallprompt, so there is no custom button that opens the system prompt. The Share menu is the install path.
- The outcome is user choice. A dismissed prompt should not immediately call prompt() again.

**Sources:**

- [MDN — beforeinstallprompt](https://developer.mozilla.org/en-US/docs/Web/API/Window/beforeinstallprompt_event)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Can you tailor the UX of a PWA depending on whether it's run in a browser or as an installed app?</summary>

### Can you tailor the UX of a PWA depending on whether it's run in a browser or as an installed app?

**Definition:**

Yes. display standalone launches without browser UI, and a media query can see that mode. A tab is display browser.

**How it works:**

- The manifest display member selects standalone or browser. Unsupported modes fall back. display_override changes the fallback order.
- Match `(display-mode: standalone)` in CSS, or the equivalent script check, to drop browser-only chrome you added yourself.
- start_url is what launches. A tab visit can be a different page. Do not assume the installed app and the tab share nothing: they share origin storage and the same worker.
- iOS install does not use beforeinstallprompt, but a launched standalone app is still a display mode you can branch on.

**When to use:**

Branch when the installed window should hide a 'open in browser' hint. Keep one codebase. Do not ship a second app.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
</details>

<details>
<summary>What is the significance of having a responsive design in PWAs?</summary>

### What is the significance of having a responsive design in PWAs?

**Definition:**

An installed PWA is a window onto the same site. If the layout only works at one width, the home-screen icon opens a broken page.

**How it works:**

- display standalone removes browser UI, so the layout has less chrome and the same viewport duties. Icons and splash do not fix a layout.
- MDN's install story is progressive enhancement: a browser that cannot install the app still uses it as a site. That site has to be usable.
- start_url and every other page share the manifest link. A responsive shell is what both the tab and the standalone window render.
- Caching a desktop-only stylesheet does not make a phone layout. The cached CSS has to include the narrow layout.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

### Performance & Optimization

<details>
<summary>What tools can be used for performance auditing of a PWA?</summary>

### What tools can be used for performance auditing of a PWA?

**Definition:**

Audit the network waterfall and the worker, not a single score. The checks that MDN makes testable are installability, the cache strategy, and first paint.

**How it works:**

- First Contentful Paint is when the browser first renders text or an image, including a background image. It answers whether anything is happening.
- A precached shell is how a PWA paints that first content without the network. Cache-first on the HTML, CSS, and JS is the strategy MDN gives for the UI.
- Time to Interactive is a non-standardized metric: the point after the last long task followed by 5 seconds of network and main-thread inactivity. It is not a browser API you call.
- Confirm the worker reaches activated and that install's addAll did not reject. A worker that never activates cannot be the reason the second load is fast.

**Sources:**

- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
- [MDN — Time to Interactive](https://developer.mozilla.org/en-US/docs/Glossary/Time_to_interactive)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — ServiceWorker.state](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorker/state)
</details>

<details>
<summary>How do you optimize the loading time of your PWA?</summary>

### How do you optimize the loading time of your PWA?

**Definition:**

Serve the first paint from the cache and do not wait for the worker to boot for URLs you already know.

**How it works:**

- Cache-first on the precached shell returns HTML, CSS, and JS without a network round trip. MDN lists responsiveness as a reason to cache, separate from offline.
- The worker itself can delay the first load after idle, because the browser waits for it to start. InstallEvent.addRoutes() can tell the browser to fetch known URLs without waiting for that startup.
- A network-first document waits on the network even when a good shell is cached. Use that only for responses that must be fresh.
- Keep the precache list small. addAll fails the whole install if one URL fails, and a huge list delays activation.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>Discuss the impact of JavaScript bundling and minification on PWAs.</summary>

### Discuss the impact of JavaScript bundling and minification on PWAs.

**Definition:**

Bundling and minification shrink the script the first visit must download. They do not replace the cache, and they must change the URL or the worker will keep the old file.

**How it works:**

- Cache-first serves whatever was stored for that request URL. A new bundle at the same URL stays invisible until the cache entry is replaced. A fingerprinted name is a new request.
- The service worker script itself updates only when its bytes change. Import the new bundle URL from the new worker, and bump the cache name so activate drops the old one.
- Less JavaScript before first paint helps First Contentful Paint, which is the first text or image. A smaller bundle is less to parse on the main thread before that paint and before the page is idle.
- Minification does not make a worker able to touch the DOM. The worker stays a separate script.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
</details>

<details>
<summary>What role does image optimization play in PWAs?</summary>

### What role does image optimization play in PWAs?

**Definition:**

Images are often the first contentful paint and a large share of the precache. A smaller image paints sooner and fails addAll less often.

**How it works:**

- First Contentful Paint includes the first image, background images included. An above-the-fold hero that is huge delays that timestamp.
- Precache the images the shell needs and leave the rest to a later strategy. addAll fails installation if any one image 404s.
- Cache-first is appropriate for images that do not change for this version. A new image needs a new URL or a cache-name bump, or the old bytes stay.
- Do not precache every gallery image. MDN also allows caching later, on fetch or when the user asks to save something for offline.

**Sources:**

- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
</details>

<details>
<summary>How do you track and improve the First Contentful Paint (FCP) in PWAs?</summary>

### How do you track and improve the First Contentful Paint (FCP) in PWAs?

**Definition:**

First Contentful Paint is the time the browser first renders any text or image, including a background image. It answers whether anything is happening.

**How it works:**

- A precached shell can paint that text and CSS without the network. That is the responsiveness benefit MDN gives caching, not only the offline benefit.
- Do not block that paint on a network-first document or on a worker startup you could skip. addRoutes can fetch known URLs without waiting for the worker.
- Track it in the lab and in the field with the same definition. Improving it means the first text or image arrives sooner, not that a later metric moved.
- A splash screen from the manifest is not FCP. FCP is a render of DOM content.

**Sources:**

- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>What is Time to Interactive (TTI), and how do you optimize it for PWAs?</summary>

### What is Time to Interactive (TTI), and how do you optimize it for PWAs?

**Definition:**

Time to Interactive is a non-standardized progress metric: the moment the last long task finished and was followed by 5 seconds with no network activity and no main-thread activity.

**How it works:**

- MDN attributes the proposal to the Web Incubator Community Group in 2018. It is not an event you listen for.
- A long task on the main thread pushes that quiet window back. Less JavaScript before the first paint, and a worker that does not block the document, keep the main thread free.
- The service worker runs off the main thread and has no DOM. Moving cache and fetch work there does not by itself end long tasks in the page.
- Five quiet seconds is the definition. A page that keeps polling never reaches it.

**Sources:**

- [MDN — Time to Interactive](https://developer.mozilla.org/en-US/docs/Glossary/Time_to_interactive)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
</details>

<details>
<summary>How do you implement lazy loading in a PWA?</summary>

### How do you implement lazy loading in a PWA?

**Definition:**

Lazy loading fetches a subresource when it is about to be seen, instead of putting it in the install precache.

**How it works:**

- The precache is for the shell the first paint needs. MDN says you can also cache during fetch, or when the user asks to save something large, including with Background Fetch.
- An image that is not in the precache list is requested normally. A loading="lazy" image is that request, delayed. The fetch handler can still cache the response when it happens.
- Do not lazy-load the image that is the First Contentful Paint. That timestamp is the first text or image.
- If the user is offline before the lazy request, cache-first has nothing to return. The shell should still render.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
</details>

<details>
<summary>Why is it important to prioritize above-the-fold content for PWAs?</summary>

### Why is it important to prioritize above-the-fold content for PWAs?

**Definition:**

Above-the-fold content is what First Contentful Paint is measuring: the first text or image the user can see.

**How it works:**

- If that content waits on a network-first call or on a large script, FCP waits with it. Precache the HTML and CSS that produce it.
- Images in that region count as contentful paint, including backgrounds. A huge hero delays the timestamp it was meant to improve.
- The rest of the page can stay lazy. The shell cannot.
- A splash screen from the manifest is not a substitute. FCP is DOM content.

**Sources:**

- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
</details>

<details>
<summary>Discuss how to prevent janky animations and achieve smooth performance in a PWA.</summary>

### Discuss how to prevent janky animations and achieve smooth performance in a PWA.

**Definition:**

Jank is a long task on the main thread between frames. Time to Interactive literally waits until the last long task and then five quiet seconds.

**How it works:**

- Keep animation work off a script that also parses a large bundle. A smaller bundle is less main-thread time before the page can go quiet.
- The service worker cannot animate. It has no DOM. Do not try to drive frames from the worker.
- A fetch handler that does heavy synchronous work delays responses. The worker is asynchronous for a reason. MDN forbids synchronous XHR there.
- Prefer CSS for the motion the first paint needs, so FCP is not waiting on a script to insert the first image.

**Sources:**

- [MDN — Time to Interactive](https://developer.mozilla.org/en-US/docs/Glossary/Time_to_interactive)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
</details>

<details>
<summary>What strategies can be employed to minimize main-thread work in a PWA?</summary>

### What strategies can be employed to minimize main-thread work in a PWA?

**Definition:**

Main-thread work is what delays First Contentful Paint and what Time to Interactive calls a long task.

**How it works:**

- Ship less script before the first paint. The worker can intercept fetch off the main thread, but it cannot touch the DOM, so rendering stays on the page.
- addRoutes lets the browser fetch known URLs without waiting for the worker to start, which is main-thread-adjacent startup cost MDN calls out.
- Do not precache so much that install's waitUntil runs a huge download before the worker can activate. A failed or slow install does not help the page.
- Five seconds of main-thread quiet is part of the TTI definition. Polling and timers that never stop keep the page out of that window.

**Sources:**

- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
- [MDN — Time to Interactive](https://developer.mozilla.org/en-US/docs/Glossary/Time_to_interactive)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

### PWA Architecture and Application Structure

<details>
<summary>What architectural patterns are commonly used in PWAs?</summary>

### What architectural patterns are commonly used in PWAs?

**Definition:**

The architecture MDN describes is a page plus a service worker that proxies requests, plus a manifest for install. The page stays the UI.

**How it works:**

- The worker is an event-driven script: install, activate, fetch, push. It has no DOM.
- Caching strategies differ by resource. One worker can use cache-first for the shell and network-first for an inbox.
- Offline writes go to client storage and a later sync. Background Sync registers a one-shot tag. Periodic sync is a separate, more restricted API.
- Progressive enhancement is the install story: a browser that cannot install still gets the site.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Offline and background operation](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>How should you structure your files and directories for a PWA?</summary>

### How should you structure your files and directories for a PWA?

**Definition:**

Put the worker at a scope that covers the pages it should control, and put the manifest where every page can link it.

**How it works:**

- register(url, { scope }) controls pages under that scope. A script in a nested folder cannot control the parent path.
- The manifest link belongs on every page of a multi-page app. start_url is the launch page.
- Precache URLs are paths the install handler requests. They have to exist. One 404 fails addAll.
- Keep the worker file's URL stable. Its bytes are what the browser compares. The assets it caches should be fingerprinted.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
</details>

<details>
<summary>Can you explain the Model-View-ViewModel (MVVM) architecture in the context of PWAs?</summary>

### Can you explain the Model-View-ViewModel (MVVM) architecture in the context of PWAs?

**Definition:**

MVVM is a UI split: a model holds state, a view renders, a view-model adapts one to the other. A PWA does not require it. The worker is not the view-model.

**How it works:**

- The worker cannot touch the DOM, so it cannot be the view. It can cache and sync. The page remains the view.
- Offline state belongs in IndexedDB, which MDN describes as client-side storage for significant structured data, including files and blobs, with indexes. Web Storage is the smaller store, and it cannot be used inside the worker.
- A view-model that assumes the network is up will show an empty view offline. The model has to read the local store first.
- Installability and caching do not care which UI pattern the page uses.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
</details>

<details>
<summary>How do you manage state in a PWA?</summary>

### How do you manage state in a PWA?

**Definition:**

Separate the durable user data from the HTTP cache. The cache stores responses. IndexedDB stores structured records.

**How it works:**

- IndexedDB is for significant structured data, including blobs, with indexes for search. Web Storage is for smaller amounts, and the worker cannot use it.
- The Cache API holds Request/Response pairs. It is the right place for the shell and for GET responses. It is the wrong place for a draft the user is editing.
- A versioned cache name is state of the shell. Bump it when the shell changes and delete the old name in activate.
- Sync is a tag you register when the local store has something to send. The worker runs it later. The page should not pretend the POST already succeeded.

**Sources:**

- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — Offline and background operation](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation)
</details>

<details>
<summary>Discuss the role of front-end frameworks and libraries in PWA development.</summary>

### Discuss the role of front-end frameworks and libraries in PWA development.

**Definition:**

A framework does not install the worker or the manifest for you unless you add those files. The browser's rules stay the same.

**How it works:**

- The page still needs the manifest link, HTTPS, and a registering script. Chromium still wants the install members.
- The worker is a separate file. A bundler that fingerprints the worker URL on every build makes the browser treat it as a new worker only if the bytes change, which they will. Keep the registered URL stable and fingerprint the assets instead.
- Framework HTML still has a First Contentful Paint. Shipping the whole framework before any text delays it.
- Server-rendered HTML can be precached as the shell. The worker does not run the framework. It caches the response.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

<details>
<summary>How does a PWA handle dynamic content and user personalization?</summary>

### How does a PWA handle dynamic content and user personalization?

**Definition:**

Dynamic content should use network-first or network-only. Personalization that must not be stale does not belong in a cache-first shell.

**How it works:**

- MDN's network-first example is an inbox path: try the network, update the cache, fall back to the cache on failure. A product list that must be current stays network-only.
- The shell around that data can still be precached. The HTML frame and the JSON are different strategies.
- POST and user-specific writes are not cacheable. MDN says caching is never appropriate for POST.
- A personalized page cached under one URL will be shown to the next session on that device. Vary the cache, or do not cache that response.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

### Offline Capabilities

<details>
<summary>How do you design for offline-first in a PWA?</summary>

### How do you design for offline-first in a PWA?

**Definition:**

Offline-first means the shell renders from the cache, and user edits land in local structured storage until a sync can send them.

**How it works:**

- Precache the shell in install and answer navigations from that cache. MDN's cache-first path is the UI that does not change until the next worker.
- Drafts go to IndexedDB, not the Cache API. IndexedDB holds structured data and blobs. The Cache API holds responses.
- Register a background sync tag when the device is offline so the worker can retry. MDN's offline guide shows sync.register from the page once the worker is ready. The sync event is not a general timer.
- The user must see that the edit is local. A failed waitUntil asks the browser to retry with backoff. It is not a guarantee on every browser.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — Offline and background operation](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation)
</details>

<details>
<summary>What is the role of IndexedDB in PWAs?</summary>

### What is the role of IndexedDB in PWAs?

**Definition:**

IndexedDB is the client-side store for significant structured data, including files and blobs, with indexes for search. Web Storage is only for smaller amounts.

**How it works:**

- MDN contrasts it with Web Storage, which is less useful for larger structured data. The service worker cannot use Web Storage at all, because the worker is asynchronous and Web Storage is synchronous.
- Use it for drafts, queues of outgoing edits, and cached query results you need to query. Do not use it as a substitute for the Cache API's response storage.
- It lives in the origin. Clearing site data clears it. Do not treat it as a server.
- Indexes are how you search it without scanning every record. That is the point MDN states.

**Sources:**

- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>How can you synchronize local changes with a server once online?</summary>

### How can you synchronize local changes with a server once online?

**Definition:**

Queue the change locally and register a one-shot background sync. The worker sends it when the browser fires the sync event.

**How it works:**

- From the page, after the worker is ready: registration.sync.register(tag). MDN's offline guide uses that call.
- The worker listens for sync, checks event.tag, and passes the send to event.waitUntil. A rejection asks the browser to retry later with backoff.
- This is not Periodic Background Sync. Periodic sync is a separate API, needs the periodic-background-sync permission, and in Chrome requires an installed app plus an engagement score.
- The server must tolerate a repeat. A lost response can cause the same queued edit to be sent again.

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
<summary>Can you explain how to manage user-generated content in offline mode?</summary>

### Can you explain how to manage user-generated content in offline mode?

**Definition:**

Store the user's content in IndexedDB while offline, and keep a queue of what still has to be sent. Do not put the draft in the HTTP cache.

**How it works:**

- IndexedDB holds structured data and blobs, which covers a note and an attached file. The Cache API holds responses and is the wrong shape for an editable draft.
- Show the local copy immediately. Mark it unsynced. When the network returns, sync.register asks the worker to send the queue.
- If the user edits the same item twice before a successful send, the queue should hold the latest copy, or the server must apply them in order.
- Clearing site data deletes the local copy. Tell the user the device is the only copy until sync succeeds.

**Sources:**

- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Offline and background operation](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation)
</details>

<details>
<summary>What are the considerations for storing and retrieving data locally in a PWA?</summary>

### What are the considerations for storing and retrieving data locally in a PWA?

**Definition:**

Match the store to the data. Responses go in the Cache API. Structured records go in IndexedDB. Small tokens are the only fit for Web Storage, and not inside the worker.

**How it works:**

- The Cache API is persistent Request/Response storage available to the page and the worker. Precache in install, refresh in fetch, delete old names in activate.
- IndexedDB is for significant structured data and blobs, with indexes. Web Storage is for smaller data and cannot be used in the worker.
- The browser may evict caches. The user may clear them. A cache-first app needs a network fallback.
- Do not cache POST. Do not cache a response that is wrong when stale, such as a live product list. MDN says network-only is the strategy there.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
</details>

### Push Notifications

<details>
<summary>How do you register a service worker for push notifications?</summary>

### How do you register a service worker for push notifications?

**Definition:**

Push registration is PushManager.subscribe() on an active service worker. The page gets back a subscription whose endpoint the server stores.

**How it works:**

- The worker must already be active. subscribe() returns a PushSubscription with the endpoint and the keys the server needs to encrypt the payload.
- The endpoint is secret. MDN says knowledge of it is enough to send a message to your app.
- The worker handles push and can show a notification with showNotification(). A push that never shows a notification counts against Firefox's quota.
- Guard the request that saves the subscription. MDN warns about CSRF on this flow.

**Examples:**

```js
const reg = await navigator.serviceWorker.ready;
const sub = await reg.pushManager.subscribe({ userVisibleOnly: true });
```

**Sources:**

- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
</details>

<details>
<summary>What is the Web Push Protocol and how is it used in PWAs?</summary>

### What is the Web Push Protocol and how is it used in PWAs?

**Definition:**

Web Push is the server-to-browser delivery that lands in the service worker's push event. The Push API is the browser side of that delivery.

**How it works:**

- The server sends to the subscription endpoint, encrypted with the subscription's key. The browser starts the worker if needed and fires push.
- The worker reads PushMessageData and typically calls showNotification(). The page does not have to be loaded.
- pushsubscriptionchange fires when a subscription expires or is about to. Resubscribe there, or the endpoint you stored is dead.
- The endpoint must stay secret. Treat a leaked endpoint as a leaked send capability.

**Sources:**

- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
</details>

<details>
<summary>Can you explain the notification click event handling in a PWA?</summary>

### Can you explain the notification click event handling in a PWA?

**Definition:**

A notification click is handled in the worker, because the page may not be open. The worker focuses an existing client or opens one.

**How it works:**

- The push handler shows the notification. The click handler is a separate event on the worker. Use waitUntil so the worker stays alive while you focus or open a window.
- clients.matchAll finds open windows. If one matches, focus it. Otherwise open the start_url or a URL you put in the notification data.
- The page is not guaranteed to be there. Do not handle the click only in a page listener.
- A click that does nothing is a failed notification. The user opted in to see a result.

**Sources:**

- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>What are the best practices for sending push notifications to users?</summary>

### What are the best practices for sending push notifications to users?

**Definition:**

Ask permission in context, show a notification for what you send, and keep the endpoint secret.

**How it works:**

- subscribe only after the user has a reason. The Push API delivers even when the app is not loaded, which is also why a noisy sender is expensive. Firefox will quota silent pushes.
- userVisibleOnly subscriptions are the ones that display a notification. A silent push is what Firefox limits, and the limit refreshes when the site is visited. Chrome does not apply that limit.
- Handle pushsubscriptionchange or you will send to a dead endpoint.
- CSRF-protect the call that stores the subscription. The endpoint is enough to push.

**Sources:**

- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
</details>

<details>
<summary>How do you customize the notification appearance and behavior on different platforms?</summary>

### How do you customize the notification appearance and behavior on different platforms?

**Definition:**

The notification UI is the platform's. You choose title, body, icon, and data. You do not get one pixel-perfect dialog on every OS.

**How it works:**

- showNotification renders with the platform. An icon you pass should be a real image URL the device can fetch, the same care as the manifest icons.
- A click is handled in the worker, not in a page that may be closed. Put the target URL in the notification data.
- iOS install and push support differ from Chromium. Feature-detect showNotification and the PushManager before you promise notifications.
- Silent notifications are a Firefox quota problem. Prefer a visible notification.

**Sources:**

- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

### Scalability and Maintainability

<details>
<summary>How do you ensure that a PWA remains scalable as features grow?</summary>

### How do you ensure that a PWA remains scalable as features grow?

**Definition:**

Scale the site the way you scale a site. The worker stays a thin proxy. Do not put every feature's logic in the fetch handler.

**How it works:**

- Cache strategies stay per URL pattern: shell cache-first, fresh data network-first or network-only. One growing if-ladder in fetch is how interception becomes untestable.
- Structured data stays in IndexedDB. The Cache API stays responses. Mixing them makes eviction and migrations one blob.
- A new worker version is a new script plus a new cache name. activate deletes the old caches. That is the migration of the shell.
- Installability does not change because you added a route. The manifest and HTTPS rules stay.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Discuss the use of module bundlers in PWAs for better maintainability.</summary>

### Discuss the use of module bundlers in PWAs for better maintainability.

**Definition:**

A bundler should fingerprint assets and leave the service worker URL stable, or every build looks like a new worker for the wrong reason.

**How it works:**

- The browser compares worker bytes. A bundle hash inside the worker file does change those bytes, which is what you want when the precache list changed. A hash only in the worker's filename changes the URL you register.
- The precache list must be the filenames the bundler emitted. A stale list fails addAll or serves a missing file.
- Minification shrinks the main-thread script, which helps the first paint. It does not change fetch semantics.
- Source maps do not belong in the precache.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
</details>

<details>
<summary>What considerations must be taken into account for version control in PWAs?</summary>

### What considerations must be taken into account for version control in PWAs?

**Definition:**

Version the worker and the cache name together. The script bytes are the version the browser sees.

**How it works:**

- A new commit that changes the worker installs a waiting worker. Old pages keep the old one until they close, unless you skipWaiting.
- The cache name is the shell's version. activate deletes every other name. Forgetting the bump serves the previous precache forever.
- Asset filenames with hashes mean an old cache entry is simply not requested. That is safer than overwriting one URL.
- Do not force-update from the page on a timer. The browser already checks on navigation and at least every 24 hours when events fire.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
- [MDN — skipWaiting()](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/skipWaiting)
</details>

<details>
<summary>How do you handle data migrations in PWAs?</summary>

### How do you handle data migrations in PWAs?

**Definition:**

Shell migrations are a new cache name and a delete of the old names in activate. Data migrations are an IndexedDB version change.

**How it works:**

- activate is the point MDN gives for deleting caches the previous worker used. The old worker is gone, so those names are safe to drop.
- IndexedDB is the structured store. A schema change belongs in its versioned upgrade, not in the Cache API.
- Do not migrate by reading every cached response and hoping the shape matches. Responses are HTTP bodies. Records are IndexedDB.
- If the upgrade fails, keep the old database. A half-written store is worse than the old schema.

**Sources:**

- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

<details>
<summary>Can you implement feature toggles in PWAs?</summary>

### Can you implement feature toggles in PWAs?

**Definition:**

A feature toggle is a runtime flag. In a PWA it has to live where both the page and, if needed, the worker can read it, and a cached shell must not freeze the flag forever.

**How it works:**

- Do not precache a JSON flag with cache-first if the flag must change before the next worker. MDN's stale-while-revalidate skips JSON in its example for that kind of freshness, and network-only is the strategy when stale is wrong.
- IndexedDB or a network read can hold the flag for the page. The worker does not see the page's memory.
- A toggle that changes the precache list requires a new worker. The script bytes have to change.
- Default the flag off when the read fails, so offline does not turn a half-shipped feature on.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

### PWA and E-commerce

<details>
<summary>Why are PWAs well-suited for e-commerce platforms?</summary>

### Why are PWAs well-suited for e-commerce platforms?

**Definition:**

A storefront is a shell plus fresh product data. The shell can be installed and cached. The catalog often must not be.

**How it works:**

- Cache-first paints the UI offline, which is the responsiveness and offline benefit MDN describes. A product list that would frustrate users when stale stays network-only.
- Install from the browser or, where you package it, from a store via the installability rules. The same site still works as a website where install is unavailable.
- Push can tell a subscriber that an order changed, with the worker showing a notification. That requires an active worker and a secret endpoint.
- Payments are a POST or a Payment Request, not a cached GET.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [MDN — Payment Request API](https://developer.mozilla.org/en-US/docs/Web/API/Payment_Request_API)
</details>

<details>
<summary>How can a PWA improve conversion rates for an e-commerce site?</summary>

### How can a PWA improve conversion rates for an e-commerce site?

**Definition:**

The pages we have do not publish a conversion percentage. What they do say is that a cached shell paints without the network, and an installable app launches like a platform app.

**How it works:**

- First Contentful Paint is the first text or image. A precached document can produce that without waiting on the network.
- Install promotion requires the manifest and HTTPS. A browser that cannot install still gets the site, which is the progressive-enhancement path.
- A stale catalog is not a conversion feature. MDN says some data must stay network-only.
- Do not quote a before-and-after number the docs do not contain.

**Sources:**

- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>What are the unique challenges of developing PWAs for e-commerce?</summary>

### What are the unique challenges of developing PWAs for e-commerce?

**Definition:**

The hard parts are freshness, payments, and browsers that do not install the same way.

**How it works:**

- A product list and a price must not be cache-first. MDN's network-only strategy is for responses that are wrong when old.
- Checkout is a POST. Caching POST is never appropriate. Payment Request is a way to select a saved instrument, not a new payment network, and it is not something you cache.
- Offline carts belong in IndexedDB, then a sync when the network returns. Stock may have changed. The server has to reject a stale checkout.
- iOS has no beforeinstallprompt. Desktop Firefox does not install from a manifest. The site still has to sell in a tab.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Payment Request API](https://developer.mozilla.org/en-US/docs/Web/API/Payment_Request_API)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>How would you handle checkout and payment processing in an e-commerce PWA?</summary>

### How would you handle checkout and payment processing in an e-commerce PWA?

**Definition:**

Payment Request lets the user pick a saved way to pay and hands that choice to the merchant. It is not a new payment network, and it is not a cached response.

**How it works:**

- MDN: it provides a consistent experience for merchants and users. Cart abandonment is the problem it is aimed at, by avoiding a long custom form when a stored instrument exists.
- The actual charge still happens on your server. The API selects and returns the instrument details the merchant asked for.
- Do not intercept this flow with a cache-first fetch handler. A payment POST is in the set MDN says must not be cached.
- Feature-detect PaymentRequest. Where it is missing, show your own form. That is the same progressive enhancement as install.

**Sources:**

- [MDN — Payment Request API](https://developer.mozilla.org/en-US/docs/Web/API/Payment_Request_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Can you discuss strategies for integrating analytics in an e-commerce PWA?</summary>

### Can you discuss strategies for integrating analytics in an e-commerce PWA?

**Definition:**

Analytics is a network call. It must not block First Contentful Paint, and a failed send must not break checkout.

**How it works:**

- FCP is the first text or image. A script that waits on an analytics response before rendering delays that timestamp.
- Send after the shell is up. If the device is offline, queue the event in IndexedDB and replay it with a sync tag. Do not cache the analytics POST.
- A service worker can listen for fetch, but caching POST is never appropriate. Let the analytics request hit the network.
- Do not put secrets in the worker script. The script is served to every client.

**Sources:**

- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [MDN — Offline and background operation](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation)
</details>

### APIs and Integration

<details>
<summary>What are the common APIs used in developing a PWA?</summary>

### What are the common APIs used in developing a PWA?

**Definition:**

The APIs that define a PWA in these docs are the service worker, the Cache API, and the manifest. Push, sync, and device APIs are additions.

**How it works:**

- ServiceWorkerContainer.register, install, activate, and fetch are the worker. Cache Storage holds the responses.
- The manifest supplies name, icons, start_url, and display for install.
- PushManager, sync.register, and periodicSync.register cover notifications and deferred work. Geolocation and getUserMedia cover GPS and camera, both in secure contexts, both after a permission prompt.
- IndexedDB covers structured offline data. Web Storage does not run inside the worker.

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
<summary>How do PWAs integrate with native device features?</summary>

### How do PWAs integrate with native device features?

**Definition:**

Device features are web APIs on a secure origin, each behind a permission. The installed icon does not grant them.

**How it works:**

- getUserMedia prompts for a camera or microphone and resolves to a MediaStream, or rejects if the user denies it or no device matches. It is a secure-context API.
- Geolocation asks permission and then reports coordinates. It is also secure-context only.
- Push asks for a subscription. Notifications are shown from the worker.
- A browser that does not implement the API leaves you with the site without that feature. Detect it.

**Sources:**

- [MDN — MediaDevices.getUserMedia()](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia)
- [MDN — Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API)
- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>Discuss how a PWA can access a user's camera or GPS.</summary>

### Discuss how a PWA can access a user's camera or GPS.

**Definition:**

Camera access is getUserMedia. GPS is the Geolocation API. Both require a secure context and a user permission.

**How it works:**

- getUserMedia() prompts for a media input and returns a MediaStream. A video track can come from a camera. If the user denies permission, or nothing matches, the promise rejects.
- Geolocation's getCurrentPosition or watchPosition asks permission and then provides latitude and longitude. For privacy, the browser asks before reporting.
- Neither call belongs in the service worker's install handler. The worker has no DOM and these prompts belong to a user gesture on the page.
- HTTPS or localhost is required. An insecure page does not get the APIs.

**Sources:**

- [MDN — MediaDevices.getUserMedia()](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia)
- [MDN — Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>How do you handle cross-origin requests in a PWA?</summary>

### How do you handle cross-origin requests in a PWA?

**Definition:**

Cross-origin requests succeed only if the other origin says so. A service worker does not turn CORS off.

**How it works:**

- CORS is the mechanism by which a server lists origins, other than its own, from which the browser may load resources. The browser may send a preflight to check that the real request will be allowed.
- A fetch from the worker is still a fetch. A cross-origin response without the right headers fails the same way it fails on the page.
- cache.put of a response you were not allowed to read does not make it readable later.
- Credentials on a cross-origin request need the server to allow them explicitly. Do not use a wildcard origin with credentials.

**Sources:**

- [MDN — CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
- [MDN — FetchEvent](https://developer.mozilla.org/en-US/docs/Web/API/FetchEvent)
</details>

<details>
<summary>How can third-party services be integrated into a PWA?</summary>

### How can third-party services be integrated into a PWA?

**Definition:**

A third-party service is another origin. Load it only if that origin sends the CORS headers your fetch needs, and do not precache a script you do not control.

**How it works:**

- A cross-origin script or API without CORS fails. The worker cannot relax that.
- Putting a third-party URL in addAll makes your install depend on their uptime. One failure rejects the whole precache.
- Payment Request and push subscriptions are the integration points these docs actually specify. Each has its own permission and its own server.
- A tag manager that injects scripts delays First Contentful Paint. Load it after the shell.

**Sources:**

- [MDN — CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
- [MDN — Payment Request API](https://developer.mozilla.org/en-US/docs/Web/API/Payment_Request_API)
- [MDN — First Contentful Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_contentful_paint)
</details>

### Advanced PWA Topics

<details>
<summary>What are Progressive Enhancement and Graceful Degradation in the context of PWAs?</summary>

### What are Progressive Enhancement and Graceful Degradation in the context of PWAs?

**Definition:**

Progressive enhancement means the site works before the extra capability. Graceful degradation means a fancier path fails softly back to that site.

**How it works:**

- MDN's install guide says a browser that cannot install the PWA still uses it as a normal website. That is the enhancement: install, worker, and push are additions.
- Feature-detect serviceWorker, BeforeInstallPromptEvent, PushManager, and PaymentRequest. The missing ones skip that branch.
- A worker that fails to install must leave the network path working. Do not make the document depend on respondWith.
- iOS without beforeinstallprompt is the degradation case for the custom install button. The Share menu remains.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — beforeinstallprompt](https://developer.mozilla.org/en-US/docs/Web/API/Window/beforeinstallprompt_event)
</details>

<details>
<summary>How do you implement a periodic background sync in PWA?</summary>

### How do you implement a periodic background sync in PWA?

**Definition:**

Periodic background sync registers a tag the browser may fire on a schedule. It is not one-shot Background Sync, and it is heavily gated.

**How it works:**

- periodicSync.register(tag, { minInterval }) on an active registration. minInterval is a minimum, in milliseconds, not a promise.
- InvalidStateError if there is no active worker. NotAllowedError if the periodic-background-sync permission is denied. InvalidAccessError if the window is not a top-level window.
- In Chrome the permission is granted only to an installed app, and Chrome also uses an engagement score to decide whether and how often to fire.
- The worker handles the periodicsync event and does the fetch inside waitUntil. It still cannot touch the DOM.

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
<summary>Discuss the concept of Credential Management API in PWAs.</summary>

### Discuss the concept of Credential Management API in PWAs.

**Definition:**

The Credential Management API lets the page ask the browser for stored credentials instead of a custom password form, when the browser supports it.

**How it works:**

- Feature-detect with `"credentials" in navigator`, then credentials.get({ password: true }). If the property is missing, keep the old sign-in.
- This is a page API. The service worker does not show the prompt and has no DOM.
- It does not replace your session. You still send the credential to your server. Do not cache that POST.
- A missing API is the progressive-enhancement case, not an error.

**Sources:**

- [MDN — Navigator.credentials](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/credentials)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

<details>
<summary>What is the Payment Request API and how does it simplify transactions in a PWA?</summary>

### What is the Payment Request API and how does it simplify transactions in a PWA?

**Definition:**

The Payment Request API lets the user select a preferred way to pay and gives that information to the merchant. It is not a new payment method.

**How it works:**

- MDN describes a consistent checkout experience and ties it to cart abandonment caused by checkout friction.
- You request the methods you support. The browser shows the ones the user has. The charge is still yours to make on the server.
- Detect the API. Fall back to your form. Do not cache the payment request.
- A service worker must not answer the payment POST from the cache.

**Sources:**

- [MDN — Payment Request API](https://developer.mozilla.org/en-US/docs/Web/API/Payment_Request_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

<details>
<summary>How can a PWA be part of the Internet of Things (IoT)?</summary>

### How can a PWA be part of the Internet of Things (IoT)?

**Definition:**

A PWA does not become a device on a local network by being installable. The worker can sync and receive push. It cannot hold a DOM or a guaranteed background radio.

**How it works:**

- The service worker wakes for push and for sync. Periodic sync is gated and is not a realtime socket to a sensor.
- Geolocation and getUserMedia run on the page, in a secure context, after permission. They are not available as a silent worker loop.
- If a reading must be stored offline, put the structured sample in IndexedDB and sync it later. The Cache API is the wrong store for a sensor log.
- MDN's forward-looking note on the worker page mentions geo-fence as a future hook, not as a current API you can call.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — PeriodicSyncManager.register()](https://developer.mozilla.org/en-US/docs/Web/API/PeriodicSyncManager/register)
- [MDN — Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
</details>

### Platform-Specific Considerations

<details>
<summary>How do you optimize a PWA for iOS devices?</summary>

### How do you optimize a PWA for iOS devices?

**Definition:**

On iOS, install is the Share menu, not beforeinstallprompt, and older iOS only installs from Safari.

**How it works:**

- iOS 16.3 and earlier: Safari only. iOS 16.4 and later: Share in Safari, Chrome, Edge, Firefox, and Orion. There is no beforeinstallprompt, so do not wait for that event to show an install hint.
- The manifest still matters for name, icons, and display standalone once the user adds the app.
- HTTPS is required, same as everywhere. A service worker is still the offline path.
- Do not promise WebAPK behavior. That is the Android Chrome and Samsung Internet path.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Web app manifest](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Manifest)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

<details>
<summary>What are the current limitations for PWAs on the Apple App Store?</summary>

### What are the current limitations for PWAs on the Apple App Store?

**Definition:**

Apple's install path is not Chromium's. MDN does not describe a separate App Store review quota for PWAs. What it does say is how iOS installs from the browser.

**How it works:**

- Packaging for stores is per store. MDN points at PWABuilder and lists Play, Microsoft, Meta Quest, and the iOS App Store as packaging targets. The browser install rules are separate.
- On iOS the user installs from the Share menu, with the version limits above. beforeinstallprompt will not fire.
- A PWA that depends on an API iOS does not implement has to keep the website path. That is progressive enhancement.
- Do not invent an App Store rejection reason the page does not give.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>Discuss the differences between optimizing PWAs for Android vs. iOS.</summary>

### Discuss the differences between optimizing PWAs for Android vs. iOS.

**Definition:**

Android Chromium can install a WebAPK and fire beforeinstallprompt. iOS installs from the Share menu and does not fire that event.

**How it works:**

- WebAPKs, with a real launcher entry, are Chrome on devices with Google Mobile Services and Samsung Internet on Samsung devices. Other Android browsers add a shortcut.
- iOS 16.4 and later allows install from several browsers' Share menus. Earlier iOS is Safari only.
- description and screenshots in the install prompt are an Android behavior. Do not expect them on iOS.
- The manifest, HTTPS, and the service worker are shared. The prompt is not.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — beforeinstallprompt](https://developer.mozilla.org/en-US/docs/Web/API/Window/beforeinstallprompt_event)
</details>

<details>
<summary>How does the choice of web browser affect the behavior of a PWA?</summary>

### How does the choice of web browser affect the behavior of a PWA?

**Definition:**

The same origin can be an installed app, a shortcut, or only a tab, depending on the browser.

**How it works:**

- Desktop Chromium installs from a manifest. Desktop Safari from Sonoma can Add to Dock with or without one. Desktop Firefox does not install from a manifest. Some browsers can still install any site without promoting it.
- Android WebAPK versus a badged shortcut is the browser list above. iOS is the Share menu.
- Feature-detect. Do not branch on a user-agent string you will get wrong.
- The worker and the cache behave only where service workers exist, and only on a secure context.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

### Testing & Debugging PWAs

<details>
<summary>What strategies do you use for testing the service workers?</summary>

### What strategies do you use for testing the service workers?

**Definition:**

Test registration, the lifecycle, the cache, and a reload while offline.

**How it works:**

- register() must resolve. A syntax error rejects it. Then watch installing, waiting, and activated.
- Force an update by changing the worker bytes and reloading. The new one should sit in waiting until you skipWaiting or close the old clients.
- Confirm addAll URLs return 200. One failure fails install.
- In Firefox, HTTP workers require DevTools open and the option enabled. Test the real HTTPS path too.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — ServiceWorker.state](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorker/state)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
- [MDN — skipWaiting()](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/skipWaiting)
</details>

<details>
<summary>How do you simulate offline testing conditions for your PWA?</summary>

### How do you simulate offline testing conditions for your PWA?

**Definition:**

The browser's offline mode, or a failed network in the fetch handler, is the test. The worker should answer the shell from the cache.

**How it works:**

- Cache-first returns caches.match when the device has no network. If you never precached, the fallback is Response.error() or a failed navigation.
- DevTools can set the network to offline and reload. The document request is a fetch event with mode navigate.
- Do not only disable Wi-Fi on a machine that still has another interface. The browser's offline setting is the condition the fetch rejection sees.
- A POST still goes to the network and fails offline. That is correct. The draft should already be in IndexedDB.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — FetchEvent](https://developer.mozilla.org/en-US/docs/Web/API/FetchEvent)
- [MDN — IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
</details>

<details>
<summary>What are the main aspects to test before deploying a PWA?</summary>

### What are the main aspects to test before deploying a PWA?

**Definition:**

Before deploy, the worker installs, the shell loads offline, the manifest meets the install rules, and fresh data is not stuck in a cache.

**How it works:**

- HTTPS or localhost. Manifest link on every page. Chromium members: name or short_name, 192 and 512 icons, start_url, display.
- install's precache succeeds. activate drops old cache names. A second load offline paints the shell.
- Prices, inboxes, and POST are network-first or network-only.
- Push, if you ship it, has a secret endpoint and a visible notification.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — install event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/install_event)
- [MDN — activate event](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/activate_event)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
</details>

<details>
<summary>How can you debug a PWA on a mobile device?</summary>

### How can you debug a PWA on a mobile device?

**Definition:**

Debug the phone against the same HTTPS origin, and watch the worker from the desktop's remote inspector when the browser allows it.

**How it works:**

- Reproduce install from the Share menu on iOS or the install icon on Android. beforeinstallprompt will not appear on iOS.
- Offline on the device is the real fetch failure. A desktop offline toggle does not prove the phone's storage.
- A secure context is required. A certificate the phone does not trust will not register the worker.
- Log statechange. Redundant or a stuck installing state is the same bug as on desktop.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — ServiceWorker.state](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorker/state)
</details>

<details>
<summary>What are common issues encountered when testing push notifications and how do you address them?</summary>

### What are common issues encountered when testing push notifications and how do you address them?

**Definition:**

The usual failures are permission denied, a dead subscription, a silent push that Firefox quotas, and a click handler that assumes the page is open.

**How it works:**

- subscribe requires an active worker and a user gesture that can show a permission prompt. A rejected permission ends the flow.
- Store the endpoint securely and refresh it on pushsubscriptionchange. An old endpoint does not deliver.
- Show a notification. Firefox limits pushes that do not. The quota refreshes when the site is visited. Chrome does not use that quota.
- Handle the click in the worker with waitUntil, and focus or open a window. A page-only listener misses the click when the app is closed.

**Sources:**

- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
</details>

### PWA Case Studies and Real-life Applications

<details>
<summary>Can you provide examples of successful PWAs?</summary>

### Can you provide examples of successful PWAs?

**Definition:**

These docs do not name a winning company. They describe the properties a successful install has: a manifest, HTTPS, and a worker that can cache.

**How it works:**

- Chromium promotes an app with the required manifest members. The prompt shows name and icons.
- A cached shell loads without the network. That is the offline and responsiveness claim MDN makes.
- A browser that cannot install still has the website. Success includes that path, not only the icon.
- Do not attach a traffic number the page does not publish.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

<details>
<summary>Discuss how PWAs have impacted mobile traffic for major websites.</summary>

### Discuss how PWAs have impacted mobile traffic for major websites.

**Definition:**

MDN does not publish a before-and-after traffic figure for a major site. The mechanism it does describe is a cached shell and an installable manifest.

**How it works:**

- A repeat visit can paint from the cache, which is the responsiveness benefit, separate from offline.
- Install adds an icon and a standalone window. It does not by itself change the HTML.
- Where the browser will not install, traffic stays on the normal site. Firefox desktop and older iOS are in that set.
- Any percentage you have seen elsewhere is not in these pages. Do not repeat it as fact.

**Sources:**

- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
</details>

<details>
<summary>How has the user engagement changed pre and post PWA implementation for a notable company?</summary>

### How has the user engagement changed pre and post PWA implementation for a notable company?

**Definition:**

Again, these pages do not give an engagement delta for a named company. Engagement features they do specify are install, offline paint, and opt-in push.

**How it works:**

- Install is a prompt, not a forced home-screen icon. The user can refuse.
- Push requires a subscription and, to avoid Firefox's quota, a visible notification.
- Offline only helps if the shell was precached and the data strategy matches the resource.
- Report engagement from your own analytics after the fact. Do not borrow a case study the docs do not contain.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
</details>

<details>
<summary>What challenges did developers face when transitioning a traditional web app to a PWA?</summary>

### What challenges did developers face when transitioning a traditional web app to a PWA?

**Definition:**

The transition is adding a manifest, HTTPS if you did not have it, and a worker that caches on purpose. The site has to keep working where those fail.

**How it works:**

- Every page needs the manifest link. The worker must not depend on synchronous XHR or Web Storage.
- Choose a strategy per resource. Caching the whole origin, including POST and prices, is the bug MDN's strategy list exists to prevent.
- Updates wait until old pages close unless you skipWaiting. Users can sit on the old worker.
- iOS and Firefox will not follow a Chromium install QA script. Test those paths separately.

**Sources:**

- [MDN — Making PWAs installable](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable)
- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — PWA caching](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching)
- [MDN — skipWaiting()](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope/skipWaiting)
</details>

### The Future of PWAs

<details>
<summary>What future advancements do you anticipate in the field of PWAs?</summary>

### What future advancements do you anticipate in the field of PWAs?

**Definition:**

MDN already points at a few extensions: static routes so the worker need not start, periodic sync under strict limits, and push. Anything beyond that is not a promise in these pages.

**How it works:**

- InstallEvent.addRoutes() skips worker startup for URLs whose source you already know.
- Periodic background sync stays permissioned, and in Chrome it stays tied to an installed app and engagement.
- Push already wakes a worker to show a notification. A geo-fence is listed as a future use of the worker context, not as an API to ship.
- The durable part is unchanged: HTTPS, a manifest, and an explicit cache strategy.

**Sources:**

- [MDN — Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [MDN — PeriodicSyncManager.register()](https://developer.mozilla.org/en-US/docs/Web/API/PeriodicSyncManager/register)
- [MDN — Push API](https://developer.mozilla.org/en-US/docs/Web/API/Push_API)
</details>
