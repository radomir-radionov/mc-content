# Web APIs — Auth & storage

AuthN/AuthZ, сеансы, JWT, OAuth, файлы cookie и хранилище.

[ ← Вернуться в хаб](./6_web_api.md)

### Related docs

- [Веб-API — Core](./6_web_api_core.md) · [Веб-API — HTTP](./6_web_api_http.md) · [Веб-API — стили API](./6_web_api_rest.md) · [Веб-API — аутентификация и хранение](./6_web_api_auth.md) · [Веб-API — Приложения](./6_web_api_apps.md) · [Веб-API — Безопасность](./6_web_api_security.md) · [Веб-API — Интервью](./6_web_api_interview.md)

### Sections

- [8. Auth](#8-auth)
- [9. Storage](#9-storage)

---

## 8. Auth

<details>
  <summary>Identification, authentication, authorization & HTTP Basic</summary>

### Identification, authentication, authorization & HTTP Basic

**Definition:** **Идентификация** присваивает имя (имя пользователя, `sub`). **Аутентификация** подтверждает это утверждение. **Авторизация** предоставляет права после успешного подтверждения.

**How it works:**

- Аналогия с билетом: имя на билете = идентификация; проверка паспорта = аутентификация; посадочный талон/класс салона = авторизация.
- HTTP **Basic**: сервер может отправлять `401` + `WWW-Authenticate: Basic realm="…"`; клиент повторяет попытку с `Authorization: Basic <base64(user:pass)>`. Base64 **не** шифрование — используется только через HTTPS. Неверные кредиты → `401`; допустимо, но не разрешено → `403`.
- Аутентификация приложения: учетные данные → файл cookie сеанса или токен позже. `Authorization` заголовки. «Войти с помощью Google» проходит аутентификацию у IdP; ваш сервер получает токен, а затем запускает собственный сеанс ([OAuth2 & OIDC](#oauth2--oidc-overview), [Sessions & JWT](#sessions--jwt)).

**When to use:**

Базовый для размещения информационных панелей за TLS. Sessions/JWT/OAuth для реальных продуктов.

#### Common pitfalls

Вызов Basic «зашифрован». Смешивание Basic с токенами сеанса, как если бы они были одной и той же схемой.

**Pluses and minuses:**

| +                                   | −                                                                              |
| ----------------------------------- | ------------------------------------------------------------------------------ |
| Basic прост и широко поддерживается | Полномочия по каждому запросу; CSRF-склонность против файлов cookie `SameSite` |
| Очистить 401 против 403             | Пароль в Base64 является обратимым                                             |

**Examples:**

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Basic realm="staging"

GET /admin HTTP/1.1
Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l
```

(`aladdin:opensesame` в Base64 — пример MDN, не секрет.)

**Thoughts:** _Идентифицировать → доказать (authn) → предоставить (authz). Базовый `user:pass` в заголовке, а не в сеансе._

**Sources:**

[MDN — HTTP-аутентификация](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication) · Узел: [идентификация против аутентификации](./8_nodejs_http.md#authentication-identification--authorization)

---

</details>

<details>
  <summary>OAuth2 & OIDC overview</summary>

### OAuth2 & OIDC overview

**Definition:** **OAuth 2.0** – это платформа авторизации, обеспечивающая делегированный доступ без обмена паролями. **OpenID Connect (OIDC)** — это уровень идентификации поверх OAuth2, который добавляет информацию о аутентифицированном пользователе (`id_token`).

**How it works:**

1. Приложение перенаправляет пользователя на **Сервер авторизации** (`/authorize?client_id=…&redirect_uri=…&scope=openid profile&code_challenge=…`).
2. Пользователь проходит аутентификацию и дает согласие.
3. Сервер перенаправляет обратно с **кодом авторизации** (краткосрочно, единоразово).
4. Приложение обменивается кодом в **конечной точке токена** (`POST /token`) с `client_secret` или **PKCE** `code_verifier` — никогда не раскрывайте секрет в приложениях, доступных только в браузере.
5. Ответ: **access_token** (вызов API), **refresh_token** (необязательно), **id_token** (OIDC — JWT с `sub`, `email`, `iss`).

| Токен                     | Цель                                                         |
| --- | --- |
| Токен доступа             | Учетные данные носителя для API серверов ресурсов            |
| Обновить токен            | Получите новый токен доступа без повторного входа в систему  |
| ID-токен (OIDC)           | Подтверждение аутентификации; JWT подписан эмитентом         |
| Концепция | ОАут2 |
| ------------              | ---------------------------------                            | --------------------------------                 |
| Основная цель             | Авторизация (области действия)                               | Аутентификация (кто является пользователем)      |
| Информация о пользователе | Через защищенную конечную точку с информацией о пользователе | `id_token` претензии + информация о пользователе |
| Типичное использование    | «Приложение может читать мой Календарь Google»               | «Войти через Google»                             |

**When to use:**

- **Войдите с помощью Google/GitHub** — код авторизации OIDC + PKCE.
- **Машина-машина** — поток учетных данных клиента (без пользователя).
- **Мобильный/родной** — PKCE обязателен; используйте системный браузер, а не встроенный WebView.

**Pluses and minuses:**

| +                                               | −                                                                  |
| ----------------------------------------------- | ------------------------------------------------------------------ |
| Никакого обмена паролями с третьими лицами      | Сложный; легко неправильно настроить                               |
| Стандартные потоки, библиотеки (Паспорт, Auth0) | Кража токена, если он хранится в localStorage                      |
| Мелкозернистые области                          | Обновление ротации токенов необходимо для обеспечения безопасности |

**Examples:**

```json
{
  "access_token": "eyJhbG…",
  "token_type": "Bearer",
  "expires_in": 3600,
  "refresh_token": "def502…",
  "id_token": "eyJhbG…"
}
```

**Thoughts:** OAuth2 сам по себе **не** аутентификация — используйте OIDC для входа. Храните токены в файлах cookie «HttpOnly»\*\* или в безопасном хранилище; подтвердить `iss`, `aud`, `exp` на JWT. Никогда не помещайте токены доступа в URL-адреса.

**Sources:**

[OAuth 2.0 RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749) · [OpenID Connect](https://openid.net/connect/) · [OAuth 2.0 для браузерных приложений (PKCE)](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-browser-based-apps)

---

</details>

<details>
  <summary>Sessions & JWT</summary>

### Sessions & JWT

**Definition:** **Сеансы** хранят состояние на стороне сервера; клиент хранит только файл cookie идентификатора сеанса. **JWT** кодирует утверждения в подписанном токене — проверка не имеет состояния, если вы доверяете подписи и сроку действия.

**How it works:**

- **Сессия** — вход → сервер создает запись сеанса (Redis/DB) → `Set-Cookie: sid=…; HttpOnly; Secure; SameSite=Lax` → каждый запрос отправляет cookie → сервер ищет сеанс.
- **JWT** — вход → сервер подписывает полезную нагрузку → клиент отправляет `Authorization: Bearer <token>` → сервер проверяет подпись HMAC/RSA + `exp` без поиска в базе данных (если не требуется черный список).

**When to use:** Сеанс для традиционных веб-приложений со страницами, отображаемыми на сервере. JWT для микросервисов и мобильных API. Гибрид: сеанс для Интернета + JWT для API.

**Pluses and minuses:**

| Подход                | +                                              | −                                                 |
| --------------------- | ---------------------------------------------- | ------------------------------------------------- |
| Сеансовый файл cookie | Простой отзыв (удаление сеанса)                | Состояние сервера, закрепленные сеансы в кластере |
| ДЖВТ                  | Без гражданства, масштабируется по горизонтали | Трудно отозвать; размер токена в каждом запросе   |
| JWT + обновление      | Короткий доступ, отзывное обновление           | Больше движущихся частей                          |

**Examples:**

```http
Set-Cookie: sid=abc123; HttpOnly; Secure; SameSite=Lax; Path=/
Authorization: Bearer eyJhbGciOiJIUzI1NiIs…
```

**Thoughts:**

JWT в `localStorage` XSS-уязвим — отдавайте предпочтение файлам cookie HttpOnly или памяти + обновление. Всегда используйте `Secure`, `HttpOnly`, `SameSite` о сеансовых файлах cookie.

Сессии легко отменить; JWT масштабируются без поиска — выбирайте отзыв или масштабирование, а не «JWT более безопасен».

**Sources:**

[jwt.io](https://jwt.io/) · [MDN — файлы cookie HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)

---

</details>

<details>
  <summary>Access token refresh</summary>

### Access token refresh

**Definition:** **Access token refresh** обновляет короткоживущий **access token** через долгоживущий **refresh token**, чтобы пользователь оставался в системе без повторного логина — обычно `POST /token` с `grant_type=refresh_token`.

**How it works:**

1. **Login** — Authorization Code + PKCE (SPA) или password/session.
2. **Issue** — IdP возвращает `access_token` + `refresh_token` (опционально `id_token`).
3. **API calls** — `Authorization: Bearer <access_token>` до `401` или `exp`.
4. **Refresh** — token endpoint с `refresh_token`; новая пара токенов.
5. **Rotation** — новый refresh при каждом use, старый invalidate — детект кражи.
6. **Retry** — один повтор failed request с новым access token.

| Storage   | Где refresh                                   | Notes                            |
| --------- | --------------------------------------------- | -------------------------------- |
| SPA + BFF | HttpOnly cookie на BFF; Route Handler refresh | Предпочтительно — secret не в JS |
| Mobile    | OS secure storage                             | PKCE; rotate refresh             |
| Pure SPA  | Access в memory; refresh в httpOnly cookie    | Не `localStorage`                |

**Silent refresh:** на `401` — очередь запросов, один refresh (single-flight), retry. SDK (Auth0, NextAuth) оборачивают это.

**When to use:** JWT access с TTL &lt; длины сессии. Обязательно с OAuth2/OIDC SPA. С [Secure BFF](./6_web_api_security.md#secure-bff-patterns), если браузер не должен держать refresh.

#### Common pitfalls

- Длинный access token «чтобы не refresh» — stolen bearer живёт до expiry.
- Refresh в `localStorage` — XSS крадёт долгоживущий credential.
- Без rotation — stolen refresh работает до revoke.
- Бесконечный retry при invalid refresh — logout и redirect на login.

**Pluses and minuses:**

| +                                  | −                                        |
| ---------------------------------- | ---------------------------------------- |
| Короткий access ограничивает ущерб | Rotation store, 401 handling             |
| Silent UX                          | Refresh endpoint — rate-limit и audit    |
| Rotation детектит replay           | Logout должен revoke refresh server-side |

**Examples:**

```http
POST /oauth/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token
&refresh_token=def502…
&client_id=spa-client
```

```ts
let refreshPromise: Promise<string> | null = null;

async function fetchWithRefresh(input: RequestInfo, init?: RequestInit) {
  let res = await fetch(input, {
    ...init,
    headers: { ...init?.headers, Authorization: `Bearer ${getAccessToken()}` },
  });
  if (res.status !== 401) return res;

  refreshPromise ??= refreshAccessToken().finally(() => {
    refreshPromise = null;
  });
  await refreshPromise;

  return fetch(input, {
    ...init,
    headers: { ...init?.headers, Authorization: `Bearer ${getAccessToken()}` },
  });
}
```

**Thoughts:** _Refresh — второй credential; защищайте сильнее access. BFF + HttpOnly cookie — default для web._

**See also:** [OAuth2 & OIDC](#oauth2--oidc-overview) · [Sessions & JWT](#sessions--jwt) · [Secure BFF patterns](./6_web_api_security.md#secure-bff-patterns)

**Sources:**

[OAuth.net — Refresh Token](https://oauth.net/2/grant-types/refresh-token/) · [OAuth.net — Refresh tokens](https://oauth.net/2/refresh-tokens/) · [OAuth 2.0 for Browser-Based Apps (PKCE)](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-browser-based-apps)

---

</details>

<a name="9-storage"></a>

## 9. Storage

<details>
  <summary>Cookies, sessionStorage & localStorage</summary>

### Cookies, sessionStorage & localStorage

**Definition:** Те же три клиентских хранилища, что и в [JS Browser — хранилище](./3_js_browser_extra.md#browser-storage) — файлы cookie также являются состоянием HTTP; Веб-хранилище — это ключ/значение только для JS.

**How it works:** Файлы cookie отправляются по соответствующим запросам (~ 4 КБ). `sessionStorage` / `localStorage` составляют ~5 МБ каждый, строки синхронизации, никогда не отправляются автоматически. Файлы cookie HttpOnly по-прежнему отправляются на сервер.

**When to use:** Авторизующийся сервер должен видеть → cookie. Небольшие настройки клиента → Веб-хранилище. Флаги HTTP: [Secure & HttpOnly cookie](#secure--httponly-cookies).

**Thoughts:** Не используйте файлы cookie в качестве общей клиентской базы данных.

**Sources:**

[MDN — API веб-хранилища](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API)

---

</details>

<details>
  <summary>IndexedDB</summary>

### IndexedDB

**Definition:** Асинхронная база данных объектов одного и того же происхождения для больших структурированных данных (индексы, транзакции, большие двоичные объекты) — полные примечания по выбору правильного хранилища в [JS Browser — хранилище](./3_js_browser_extra.md#browser-storage).

**How it works:** `indexedDB.open` → `onupgradeneeded` создавать хранилища объектов → `transaction` + `objectStore`. Квоты основаны на диске (не 5 МБ). Safari ITP может удалить IDB, записывающий сценарии, через 7 дней без взаимодействия.

**When to use:** Данные автономных приложений, файлы и все, что слишком велико или слишком часто для синхронизации с веб-хранилищем.

**Examples:**

```js
const request = indexedDB.open("myApp", 1);
request.onupgradeneeded = (e) => {
  e.target.result.createObjectStore("books", { keyPath: "id" });
};
```

**Sources:**

[MDN — IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)

---

</details>

<details>
  <summary>Web Storage API</summary>

### Web Storage API

**Definition:** `window.localStorage` / `sessionStorage` — строка KV в области происхождения; local сохраняется на всех вкладках/перезапусках, сеанс выполняется для каждой вкладки. Теория + подводные камни: [JS Browser — хранилище](./3_js_browser_extra.md#browser-storage).

**How it works:** API синхронизации; обертка записывает в `try/catch`. Объекты нуждаются `JSON.stringify`.

**When to use:** Тема, локаль, несекретное состояние пользовательского интерфейса.

**Examples:**

```js
try {
  localStorage.setItem("user", JSON.stringify({ id: 1 }));
  const user = JSON.parse(localStorage.getItem("user") ?? "null");
  sessionStorage.removeItem("tempFilter");
} catch {
  /* quota / disabled storage */
}
```

**Sources:**

[MDN — Window.localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)

---

</details>

<details>
  <summary>Secure & HttpOnly cookies</summary>

### Secure & HttpOnly cookies

**Definition:**

`Secure` ограничивает передачу файлов cookie HTTPS; `HttpOnly` блокирует доступ к JavaScript — и то, и другое усиливает безопасность сеанса.

| Флаг        | Защищает от                                                           |
| ----------- | --------------------------------------------------------------------- |
| Безопасный  | Сетевой снифинг по HTTP                                               |
| HttpOnly    | Файлы cookie сеанса чтения XSS                                        |
| Тот же сайт | CSRF (частичный); некоторое уменьшение количества кликджекинга/утечек |

**Examples:**

```http
Set-Cookie: sid=abc; Secure; HttpOnly; SameSite=Lax; Max-Age=86400
```

**Sources:**

[MDN — Set-Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie)

---

</details>

<a name="10-protocols"></a>
