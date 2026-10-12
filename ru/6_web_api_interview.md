# Web APIs — Interview

Вопросы и ответы на собеседовании по HTTP и веб-API.

[ ← Вернуться в хаб](./6_web_api.md)

### Related docs

- [Веб-API — Core](./6_web_api_core.md) · [Веб-API — HTTP](./6_web_api_http.md) · [Веб-API — стили API](./6_web_api_rest.md) · [Веб-API — аутентификация и хранение](./6_web_api_auth.md) · [Веб-API — Приложения](./6_web_api_apps.md) · [Веб-API — Безопасность](./6_web_api_security.md) · [Веб-API — Performance](./6_web_api_performance.md) · [Веб-API — Интервью](./6_web_api_interview.md)

### Sections

- [Interview Practice](#interview-practice)
- [Questions & Answers](#questions-answers)

---

## Interview Practice

<details>
  <summary>Interview Practice</summary>

### Interview Practice

Вопросы от [0_interview.md](0_interview.md). Попробуйте ответить, прежде чем открывать решения.

### Question index

| Тема                                                  | Ответ            |
|  -----------------------------------------------------  |  ----------------  |
| OSI и уровни TCP/IP                                   | Вопросы и ответы |
| LAN vs Интернет / пакеты / как проверить              | Вопросы и ответы |
| TCP против UDP                                        | Вопросы и ответы |
| IPv4 против IPv6 / AAAA / Happy Eyeballs              | Вопросы и ответы |
| Анатомия URL (схема, запрос, хэш)                     | Вопросы и ответы |
| Что такое API? REST, SOAP и RPC (tRPC) | Вопросы и ответы |
| XML против HTML против JSON                           | Вопросы и ответы |
| HTTP-методы + идемпотентность                         | Вопросы и ответы |
| Коды состояния                                        | Вопросы и ответы |
| Управление версиями и нумерацией страниц REST         | Вопросы и ответы |
| GET против POST                                       | Вопросы и ответы |
| Что происходит на `fetch`                             | Вопросы и ответы |
| Кэширование заголовков                                | Вопросы и ответы |
| КОРС                                                  | Вопросы и ответы |
| OAuth2 против OIDC                                    | Вопросы и ответы |
| JWT против сессии                                     | Вопросы и ответы |
| смягчение последствий CSRF                            | Вопросы и ответы |
| Кликджекинг / CSP                                     | Вопросы и ответы |
| Ограничение скорости (429)             | Вопросы и ответы |
| Получить контроллер прерывания                        | Вопросы и ответы |
| WebSocket против опроса                               | Вопросы и ответы |
| Клиент-сервер / зачем бэкенд                          | Вопросы и ответы |
| Типы веб-приложений (MPA/SPA/CSR/SSR/SSG/PWA)         | Вопросы и ответы |
| Что такое ПВА?                                        | Вопросы и ответы |
| Что такое ФКП?                                        | Вопросы и ответы |
| Что такое ЛКП?                         | Вопросы и ответы |
| Платежи/вебхуки                                       | Вопросы и ответы |
| ИЯФ против ФИД                                        | Вопросы и ответы |
| ТТФБ                                                  | Вопросы и ответы |
| Core Web Vitals (лабораторные и полевые исследования) | Вопросы и ответы |
| Оптимизация API и клиентской части                    | Вопросы и ответы |

---

</details>

## Questions & Answers

<details>
  <summary>OSI vs TCP/IP layers?</summary>

### OSI vs TCP/IP layers?

**Definition:** Обе модели описывают, как данные перемещаются по сетям. **OSI** имеет 7 уровней; практическая модель **TCP/IP** имеет 4. Полные примечания: [Модели OSI и TCP/IP](./6_web_api_apps.md#osi--tcpip-models).

**How it works:**

| ОСИ (7)                           | TCP/IP (4)     | Примеры                   |
|  ---------------------------------  |  --------------  |  -------------------------  |
| Приложение, Презентация, Сессия   | **Приложение** | HTTP, DNS, WebSocket, TLS |
| Транспорт                         | **Транспорт**  | TCP, UDP |
| Сеть | **Интернет** | IP, маршрутизация |
| Канал передачи данных, физический | **Ссылка**     | Ethernet, Wi-Fi           |

HTTP находится на **прикладном уровне** — он предполагает TCP (надежная доставка) и IP (маршрутизация) ниже него.

**When to use:** Ответ на собеседовании: «HTTP — это прикладной уровень поверх TCP». TLS шифрует при представлении/сеансе в терминах OSI, но обычно выполняется между HTTP и TCP.

**Sources:** [Модели OSI и TCP/IP](./6_web_api_apps.md#osi--tcpip-models) · [MDN — Как работает Интернет](https://developer.mozilla.org/en-US/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work)

---

</details>

<details>
  <summary>LAN vs Internet — как computers обмениваются data?</summary>

### LAN vs Internet — как computers обмениваются data?

**Definition:** Одни и те же пакеты TCP/IP в домашней локальной сети и в общедоступном Интернете. В локальной сети используются частные адреса и локальный шлюз; Интернет — это общедоступные IP-адреса с глобальной маршрутизацией. DNS сопоставляет имя с IP-адресом; TCP/TLS, а затем HTTP говорят с этим IP.

**How it works:** Файл или страница → разбивается на пакеты → каждый пакет пересылается на целевой IP → конечное ядро ​​демультиплексирует по **порту** в службу → байты собираются заново. Осмотрите с помощью `ping`, `ip addr`/`ifconfig`, `ss`/`netstat`; захватить с помощью tcpdump/Wireshark. Полные примечания: [Как работает Интернет](./6_web_api_core.md#internet) · [Порты, сокеты и сетевые службы](./6_web_api_apps.md#ports-sockets--network-services).

**When to use:** Интервью: «Что происходит, когда я ввожу URL?» Отвечайте DNS → TCP → TLS → HTTP, а не «пакеты идут в DNS, а DNS их перезаписывает».

**Thoughts:** _Адресация и NAT различаются; пакетная модель этого не делает._

**Sources:** [Как работает Интернет](./6_web_api_core.md#internet)

---

</details>

<details>
  <summary>TCP vs UDP?</summary>

### TCP vs UDP?

**Definition:** Выбор транспортного уровня: TCP = надежный упорядоченный поток после трехстороннего подтверждения; UDP = датаграммы без установления соединения.

**Interview answer:** HTTP/1.1 и HTTP/2 используют TCP (целостность HTML/CSS/JS). Потоковая передача, DNS, WebRTC часто используют UDP (задержку). HTTP/3 — это QUIC **по UDP**, но при этом надежный. Стоимость рукопожатия → `preconnect` /повторное использование соединений. Полный блок: [TCP vs UDP](#tcp-vs-udp).

---

</details>

<details>
  <summary>IPv4 vs IPv6 / AAAA / Happy Eyeballs?</summary>

### IPv4 vs IPv6 / AAAA / Happy Eyeballs?

**Definition:** IPv4 — 32-битный (`192.0.2.1`); IPv6 — это 128-битные гекстеты (`2001:db8::1`). DNS `A` vs `AAAA`. Браузеры с двумя стеками используют Happy Eyeballs, поэтому IPv6 проверяется без зависания, если он сломан.

**How it works:** Сжать нули с помощью `::` один раз. В URL-адресах переносятся литералы: `https://[2001:db8::1]:443/`. Префикс `/64` это одна локальная сеть. петлевая проверка `::1`. Документы: `2001:db8::/32`, `3fff::/20`. Публичный GUA находится в `2000::/3`. Включите сайт: слушайте IPv6 + статический адрес + AAAA.

**When to use:** «Зачем WebRTC нужно ВКЛЮЧИТЬ IPv4» (NAT); «почему мобильные пользователи выбирают IPv6»; «Почему плохой AAAA вредит сетям, поддерживающим только IPv6».

**Thoughts:** Двойной стек = A + AAAA + сервер, который фактически отвечает на оба.

**Sources:** [IPv4 и IPv6](./6_web_api_apps.md#ipv4--ipv6)

---

</details>

<details>
  <summary>URL anatomy (scheme, query, hash)?</summary>

### URL anatomy (scheme, query, hash)?

**Definition:** URL-адрес определяет ресурс и называет метод доступа. Форма: `scheme://[userinfo@]host[:port]/path?query#fragment`.

**How it works:** Схема выбирает протокол (`https`, `mailto`). Хост ищется DNS. Порты по умолчанию (80/443) опущены. `?` запускает пары запросов (`&`-раздельные). `#` это клиентский фрагмент, а не HTTP-запрос. Относительные URL-адреса наследуют текущий источник.

**When to use:** Адресная строка; `<a href>`; REST-пути против `?page=2`; на странице `#id`.

**Thoughts:** Запрос — это дополнительный ввод данных на сервер; hash никогда не покидает браузер.

**Sources:** [URI, URL и URN](./6_web_api_core.md#uri-url--urn)

---

</details>

<details>
  <summary>Что такое API? REST vs SOAP vs RPC?</summary>

### Что такое API? REST vs SOAP vs RPC?

**Definition:** API — это набор функций, которые одна программа предоставляет другим. В веб-серверах обычными стилями являются REST, SOAP и RPC. tRPC — это TypeScript RPC.

| Стиль | Определение | Плюсы | Минусы |
| -----  |  ---------------------------------------------------------------------------  |  ------------------------------  |
| ОТДЫХ | Стиль общения: необходимые данные находятся в запросе (URL + HTTP-глагол + тело) | Общий, HTTP-собственный, читаемый | Избыточная/недостаточная выборка (помогает BFF) |
|МЫЛО | Формат структурированного обмена данными (конверт XML) | Транспортно-независимые, структурированные | Многословно, хуже на клиенте, чем REST |
| РПК | Запрос содержит действие _и_ его данные (`method` + `params` + `id`) | Структура, JSON, хорошие пакеты | Логика на клиенте; нет HTTP-кеша |
| тРПК | Процедуры TS, выведенные из `AppRouter`; запрос GET/мутация POST | Сквозные типы, без кодогенерации | ТС-к-ТС; не является API общедоступного ресурса |

**When to use:** REST для общедоступного CRUD и кеширования. SOAP для устаревших предприятий. JSON-RPC, если вам нужны вызовы/пакеты процедур. tRPC, когда клиент и сервер используют TypeScript.

**Thoughts:** Полное сравнение: [REST, SOAP и RPC](./6_web_api_rest.md#5-rest-vs-soap-vs-rpc) · [tRPC](./6_web_api_rest.md#6-trpc) · [XML](./6_web_api_rest.md#xml). Не рассматривайте SOAP как «противоположность REST». Успех/ошибка JSON-RPC 2.0 являются взаимоисключающими. tRPC — это RPC, а не REST — имена процедур несут в себе смысл.

**Sources:** [Doka — Что такое API](https://doka.guide/tools/api/) · [Спецификация JSON-RPC](https://www.jsonrpc.org/specification) · [tRPC — концепции](https://trpc.io/docs/concepts)

---

</details>

<details>
  <summary>XML vs HTML vs JSON?</summary>

### XML vs HTML vs JSON?

**Definition:** XML — это _синтаксис_ разметки, для которого вы изобретаете теги; HTML — это фиксированный словарь для страниц; JSON — это типизированный формат данных. SOAP _использует_ XML; REST обычно использует JSON.

**How it works:** XML требует одного корня, закрытых тегов, атрибутов в кавычках, необязательно. `<?xml …?>` наверху. Пространства имен (`xmlns`) разделить одно и то же местное имя на разные словари. Анализаторы HTML прощают ошибки; XML и JSON — нет.

**When to use:** XML для файлов SOAP, RSS, SVG, устаревшего предприятия. JSON для новых API. HTML для документов в браузере.

**Thoughts:** Та же древовидная разметка, что и HTML, но для данных, а не для рендеринга.

**Sources:** [XML](./6_web_api_rest.md#xml)

---

</details>

<details>
  <summary>HTTP methods и idempotency?</summary>

### HTTP methods и idempotency?

**Definition:** Методы HTTP выражают намерение. **Безопасные** методы не меняют состояние сервера; **идемпотентные** методы при повторении дают тот же эффект.

| Метод    | Идемпотент | Сейф | Типичное использование        |
|  --------  |  ----------  |  ----  |  -----------------------------  |
| ПОЛУЧИТЬ | да         | да   | Читать ресурс                 |
| ГОЛОВА   | да         | да   | Только заголовки |
| ОПЦИИ | да  | да  | Предполетная подготовка CORS, возможности |
| ПУТЬ     | да         | нет  | Полная замена                 |
| УДАЛИТЬ  | да         | нет  | Удалить (2-й звонок, май 404) |
| ПОСТ     | нет        | нет  | Создание, действия            |
| ПАТЧ     | нет\*      | нет  | Частичное обновление          |

Идемпотентность PATCH зависит от реализации сервера. `TRACE` является безопасным и идемпотентным (обратная петля); `CONNECT` ни то, ни другое (туннель). Полный список: [Протокол HTTP](./6_web_api_http.md#http-protocol).

**How it works:** Повторение `PUT /users/1` с тем же телом покидает то же состояние. Повторение `POST /users` может создать несколько пользователей. При повторных попытках в случае сбоя сети следует использовать идемпотентные методы или **ключи идемпотентности** в POST.

**When to use:** Используйте PUT для обновления по идентификатору; POST для создания; DELETE можно повторить, если сервер считает отсутствующий ресурс успешным (204/404).

**Sources:** [MDN — методы HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) · [MDN — идемпотент](https://developer.mozilla.org/en-US/docs/Glossary/Idempotent)

---

</details>

<details>
  <summary>Что такое web server?</summary>

### Что такое web server?

**Definition:** Программа, которая принимает HTTP-запросы, обрабатывает их (файл, кэш, прокси или приложение) и возвращает HTTP-ответы.

**How it works:** Сообщения HTTP/1.1 = начальная строка + заголовки + пустая строка + необязательное тело. На производственных серверах добавляются модули: TLS, gzip/brotli, обратный прокси, кэш. Nginx/Apache/Caddy часто располагаются перед Node.

**When to use:** `npx serve` / `http-server` для локальных статических файлов; Nginx для TLS + статический + `/api/` прокси.

**Thoughts:** Полный блок: [Веб-сервер](./6_web_api_http.md#web-server) · Сжатие: [Content-Encoding](./6_web_api_http.md#http-compression-content-encoding) · TLS: [HTTPS-рукопожатие](./6_web_api_http.md#ssltls--https-handshake).

**Sources:** [MDN — HTTP-сообщения](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Messages) · [Doka — Веб-сервер](https://doka.guide/tools/web-server/)

---

</details>

<details>
  <summary>Распространённые HTTP status codes?</summary>

### Распространённые HTTP status codes?

**Definition:** Трехзначные коды сообщают клиенту класс результата и порядок действий.

| Код | Значение | Когда |
| ------------------------------------------- | ----------------------------- | ------------------------------------- |
|**200** ОК | Успех с телом | ПОЛУЧИТЬ, ПОСТАВИТЬ, ПАТЧИТЬ успех |
| **201** Создано | Ресурс создан | Успешная POST + `Location` заголовок |
| **204** Нет содержимого | Успех, пустое тело | УДАЛИТЬ успех |
| **301** Переехал навсегда | URL-адрес изменен навсегда | SEO-перенаправления |
| **302/307** Временное перенаправление | Временный переезд | Перенаправление входа, POST→GET |
|**304** Не изменено | Кэшированная копия все еще действительна | `If-None-Match` / `If-Modified-Since` |
| **400** Неверный запрос | Неверный ввод данных клиента | Неверный JSON |
| **401** Несанкционированный | Требуется или не удалась авторизация | Отсутствует/недействительный токен |
| **403** Запрещено | Аутентифицировано, но не разрешено | Проверка роли не удалась |
|**404** Не найден | Ресурс не существует | Неправильный идентификатор |
| **409** Конфликт | Государственный конфликт | Дублирующийся адрес электронной почты |
| **422** Необработано | Проверка не удалась | Ошибки схемы |
|**429** Слишком много запросов | Скорость ограничена | См. раздел ограничения скорости |
| **500** Внутренняя ошибка сервера | Ошибка сервера | Необработанное исключение |
| **502/503/504** Шлюз/Недоступен/Время ожидания | Восходящий/нисходящий/перегруженный | Балансировщик нагрузки, развертывание |

**When to use:** Возвращает **401**, если учетные данные отсутствуют/недействительны; **403**, когда пользователь известен, но у него нет разрешения. Используйте **201** + `Location` при создании.

**Sources:** [MDN — коды состояния HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

---

</details>

<details>
  <summary>REST versioning и pagination?</summary>

### REST versioning и pagination?

Полные примечания см. в разделе **REST → Управление версиями и нумерацией страниц**.

**Short answer:** Версия по URL-пути (`/v2/`) or `Accept` заголовок. Разбивка на страницы с помощью **курсора** для каналов (эффективно) или **смещения** для номеров страниц (просто). Возвращаться `nextCursor` / `Link` заголовки и привязки `limit` (максимум 100).

**Sources:** [RFC 8288 — Веб-ссылки] (https://datatracker.ietf.org/doc/html/rfc8288)

---

</details>

<details>
  <summary>CORS — как это работает?</summary>

### CORS — как это работает?

**Definition:** `Cache-Control` и валидаторы (`ETag`, `Last-Modified`) управлять браузером и кешированием CDN.

**How it works:**

| Директива      | Эффект                                                               |
| -------------- | -------------------------------------------------------------------- |
| `max-age=3600` | Свежий 3600 секунд                                                   |
| `no-cache`     | Перед использованием необходимо пройти повторную проверку на сервере |
| `no-store`     | Не хранить вообще (конфиденциальные данные)                          |
| `private`      | Только браузер, без общего CDN                                       |
| `public`       | CDN может кэшировать                                                 |
| `immutable`    | Никогда не проверять повторно (хешированные имена файлов)            |

Условный запрос: клиент отправляет `If-None-Match: "etag"` → сервер возвращает **304** с пустым телом, если оно не изменилось.

**When to use:** Статические ресурсы с хешем в имени файла → `max-age=31536000, immutable`. Пользовательские данные API → `private, no-store` или короткий `max-age`. Кэш уровня приложения (запрос RTK) является отдельным — см. [Кэширование HTTP](./6_web_api_rest.md#7-http).

**Thoughts:** `max-age` _это свежесть;_ `no-cache` _означает повторную проверку;_ `no-store` _означает не писать в кеш._

**Sources:** [MDN — HTTP-кэширование](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

---

</details>

<details>
  <summary>CORS — как это работает?</summary>

### CORS — как это работает?

**Definition:** Браузеры применяют **Политику одного и того же происхождения** — JS не может читать ответы из разных источников, если сервер не разрешит это через заголовки CORS.

**How it works:**

1. Тот же источник → нет проверки CORS. Перекрестное происхождение → браузер отправляет `Origin`.
2. Простой запрос отправляется на сервер; JS читает тело, только если `Access-Control-Allow-Origin` матчи.
3. Непростое → `OPTIONS` предполетная подготовка (`Access-Control-Request-Method` / `Headers`); тогда реальный запрос.
4. Необходимы учетные данные `Allow-Credentials: true` и **конкретное** происхождение — не `*`.

**When to use:** SPA + API на другом источнике; У Curl/Postman **нет** CORS. Полные заголовки и подводные камни → [CORS](./6_web_api_security.md#cors).

**Thoughts:** _Сервер часто все еще получает запрос; браузер скрывает ответ со страницы JS без сопоставления_ `Access-Control-*` _заголовки._

| Плюсы                                                         | Минусы                                           |
| ------------------------------------------------------------- | ------------------------------------------------ |
| Защищает пользователей от вредоносных сайтов, читающих данные | Неправильная конфигурация блокирует законные SPA |
| Явное согласие сервера                                        | Легко переразрешить `*` в разработке             |

**Sources:** [MDN — CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)

---

</details>

<details>
  <summary>JWT vs session cookie?</summary>

### JWT vs session cookie?

**Definition:** OAuth2 делегирует **авторизацию** (доступ к ресурсам). OIDC добавляет **аутентификацию** (кто вошел в систему) через `id_token`.

**How it works:** Код авторизации + PKCE для SPA → код обмена на `access_token` (API) + `id_token` (идентичность JWT с `sub`, `email`).

**When to use:** «Войти через Google» = OIDC. «Приложение читает мой календарь» = области действия OAuth2. Никогда не используйте неявный поток для новых приложений.

**Sources:** [OpenID Connect](https://openid.net/connect/)

---

</details>

<details>
  <summary>CSRF — как смягчить?</summary>

### CSRF — как смягчить?

**Definition:** Сессии сохраняют состояние на сервере; JWT — это автономные подписанные токены, проверенные без поиска.


| | Сессия                      | ДЖВТ                                        |
| | --------  |  ---------------------------  |
| | Хранение | Сервер (Redis/БД)           | Клиент (cookie/заголовок)                   |
| | Отзыв    | Удаление сеанса — мгновенно | Черный список или короткий TTL + обновление |
| | Масштаб | Требуется хранилище общих сессий | Проверка без гражданства |
| | XSS-риск | HttpOnly cookie безопаснее  | localStorage опасен                         |

**How it works:** Сеанс — файл cookie содержит непрозрачный идентификатор; сервер загружает пользователя. JWT — сервер проверяет подпись и `exp` по каждому запросу.

**When to use:** Сеанс для монолитных веб-приложений. JWT для микросервисов и мобильных устройств. Если это возможно, отдавайте предпочтение **HttpOnly cookie**.

**Sources:** [jwt.io](https://jwt.io/) · [OWASP — Управление сеансами](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)

---

</details>

<details>
  <summary>Clickjacking — как смягчить?</summary>

### Clickjacking — как смягчить?

**Definition:** CSRF подделывает аутентифицированные запросы, используя файлы cookie жертвы. AuthN (действительный сеанс) — это не то же самое, что авторизация этой мутации.

**Mitigations:** Токен CSRF (токен синхронизатора), `SameSite=Lax/Strict` файлы cookie, требуют пользовательских заголовков для мутаций API, никогда не меняют состояние при GET. `HttpOnly` не останавливает CSRF.

**When to use:** Сеанс cookie + форма POST → требуется токен. JWT в `Authorization` только заголовок → не уязвим к CSRF.

**Sources:** [OWASP — CSRF](https://owasp.org/www-community/attacks/csrf)

---

</details>

<details>
  <summary>Rate limiting — что такое 429?</summary>

### Rate limiting — что такое 429?

**Definition:** Пользователь нажимает на приманку, в то время как копия вашего сайта в рамке получает клик.

**Mitigations:** CSP `frame-ancestors 'none'` or `'self'`; `X-Frame-Options: DENY` or `SAMEORIGIN`. Не используйте `ALLOW-FROM` (устаревший). XFO — это только HTTP-заголовок.

**Sources:** [MDN — X-Frame-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options)

---

</details>

<details>
  <summary>AbortController с fetch?</summary>

### AbortController с fetch?

**Definition:** **429 Слишком много запросов**, когда клиент превышает допустимую скорость. Сервер должен отправить `**Retry-After`\*\*.

**How it works:** Счетчик на ключ IP/API в Redis или Edge Proxy. Клиент экспоненциально отступает из-за дрожания.

**Sources:** [MDN — 429](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429)

---

</details>

<details>
  <summary>AbortController с fetch?</summary>

### AbortController с fetch?

**Definition:** `AbortController` отменяет полет `fetch` — полезно для таймаутов, устранения дребезга поиска и очистки React.

**Examples:**

```js
const ac = new AbortController();
fetch("/api/search?q=ab", { signal: ac.signal });
ac.abort(); // throws AbortError in catch
```

**Sources:** [MDN — AbortController](https://developer.mozilla.org/en-US/docs/Web/API/AbortController)

---

</details>

<details>
  <summary>What happens when the frontend calls fetch?</summary>

### What happens when the frontend calls fetch?

**Definition:** Методы HTTP выражают намерение — GET считывает ресурсы; POST создает или запускает действия.


| | ПОЛУЧИТЬ                     | ПОСТ                                            |
| | --------------  |  ----------------------------  |
| | **Цель**       | Получить данные              | Создать ресурс/отправить действие |
| | **Безопасно** | Да (без изменения состояния сервера) | Нет |
| | **Идемпотент** | Да                           | Нет (при повторении могут возникнуть дубликаты) |
| | **Тело**       | Обычно нет (параметры в URL) | Часто JSON/тело формы                           |
| | **Кэшируемый** | Да (по умолчанию)            | Нет                                             |
| | **Пример**     | `GET /api/users/1`           | `POST /api/users`                               |

**Thoughts:** _GET = чтение (с возможностью закладки, кэширования); POST = запись/действие (по умолчанию не идемпотентно)._

**See also:** [Сопоставление CRUD и REST](./6_web_api_rest.md#rest-principles-and-contract) · [PUT vs POST](./6_web_api_http.md#put-vs-post)

**Sources:** [MDN — методы HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)

---

</details>

<details>
  <summary>Что происходит, когда frontend вызывает `fetch`?</summary>

### Что происходит, когда frontend вызывает `fetch`?

**Definition:** `fetch(url, options)` инициирует HTTP-запрос от браузера и возвращает `Promise<Response>`.

**How it works:**

```
1. JS calls fetch('/api/users')
2. Browser resolves URL → DNS → TCP/TLS connection
3. HTTP request sent (method, headers, optional body)
4. Server processes → HTTP response (status, headers, body stream)
5. Promise resolves with Response (4xx/5xx do NOT reject — check res.ok)
6. res.json() parses body
7. Browser checks CORS — blocks JS reading cross-origin response without Access-Control-Allow-Origin
```

**Frontend ↔ API contract:** Клиент ожидает форму JSON; сервер должен соответствовать кодам состояния и `Content-Type`.

**Thoughts:** _Сетевые ошибки отклоняются; Ошибок HTTP нет — всегда проверяйте_ `res.ok` _и проверьте форму JSON._

**See also:** [Fetch API](#fetch-api) · [CORS](./6_web_api_security.md#cors)

**Sources:** [MDN — Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

---

</details>

<details>
  <summary>Client–server — зачем backend?</summary>

### Client–server — зачем backend?

**Definition:** Различные шаблоны для сервер-клиент или двунаправленных обновлений.

| Узор           | Плюсы                            | Минусы                                      |
|  --------------  |  --------------------------------  |  -------------------------------------------  |
| Короткий опрос | Простой                          | Бесполезно, высокая задержка                |
| Длинный опрос  | Почти в режиме реального времени | Накладные расходы на соединение за ожидание |
| ССЕ | Сервер передает HTTP | Только в одну сторону |
| Вебсокет       | Полный дуплекс, низкая задержка  | Управление соединениями, прокси             |

**When to use:** Чат/игры → WebSocket. Живые уведомления только для чтения → SSE. Редкие обновления → опрос в порядке.

**Sources:** [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket) · [MDN — SSE](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events)

---

</details>

<details>
  <summary>Типы web apps (MPA vs SPA vs CSR/SSR/SSG)?</summary>

### Типы web apps (MPA vs SPA vs CSR/SSR/SSG)?

**Definition:** Клиентские (браузерные) запросы; сервер применяет бизнес-логику и использует БД. Пользовательский интерфейс — HTML/CSS/JS; для общих долговременных данных требуется серверная часть.

**How it works:** Классический MPA перезагрузил всю страницу. AJAX сначала возвращал фрагменты HTML. Современные API возвращают JSON; клиент красит. HTTP-команды + URL-адреса ресурсов = REST. Раздувание полезной нагрузки → BFF/GraphQL.

**See:** [Клиент-серверная архитектура](./6_web_api_core.md#client-server-architecture) · [Эволюция веб-приложений](#evolution-of-web-apps) · [0_system_design.md](./0_system_design_core.md#clientserver-architecture-why-a-backend)

**Sources:** [Doka — Как работают веб-приложения](https://doka.guide/tools/web-app-works/)

---

</details>

<details>
  <summary>Типы web apps (MPA vs SPA vs CSR/SSR/SSG)?</summary>

### Типы web apps (MPA vs SPA vs CSR/SSR/SSG)?

**Definition:** MPA = множество HTML-страниц, полная перезагрузка ссылок. SPA = одна страница, экраны принадлежат JS. CSR/SSR/SSG = **когда** создается первый HTML. PWA добавляет установку/офлайн (манифест + сервисный работник).


| | Когда существует HTML        | Навигация                        |
| | ---------------  |  ----------------------------  |
| | МПА статический | Файлы на диске               | Полная перезагрузка |
| | МПА динамический | Шаблон сервера по запросу | Полная перезагрузка |
| | КСО             | После JS в браузере          | Мгновенный                       |
| | ССР             | Серверный HTML, затем гидрат | Мгновенно после загрузки         |
| | ССГ             | Время сборки                 | Перезагрузка или увлажненное СПА |

**Thoughts:** Не рассматривайте PWA как отдельную архитектуру. Установка или оффлайн: [PWA](./6_web_api_apps.md#pwa-progressive-web-app). Таксономия: [Типы веб-приложений](./6_web_api_apps.md#12-types-of-web-apps).

**Sources:** [Doka — Типы веб-приложений](https://doka.guide/tools/web-app-types/)

---

</details>

<details>
  <summary>Что такое PWA?</summary>

### Что такое PWA?

**Definition:** Три ответа: (1) **Каждый день** — устанавливаемый сайт, который работает автономно, как нативное приложение. (2) **Официально** — прогрессивное улучшение веб-API для обеспечения пользовательского опыта собственного класса. (3) **Технический** — соответствует правилам установки данного браузера (они различаются).

**How it works:** Запрос на установку Chromium: HTTPS/`localhost` + манифест веб-приложения (`name`/`short_name`значки, `start_url`, `display`). **Service Worker не требуется устанавливать** (раньше так было). Программное обеспечение по-прежнему требуется для автономной/push-синхронизации/фоновой синхронизации. Safari/Firefox обычно пропускают пользовательский интерфейс установки Chromium; Apple говорит **Веб-приложения для главного экрана**.

**Pros (interview):** одна кодовая база; главный экран + оффлайн + нажатие; не требуется магазин; малый (API браузера); SEO/URL-адреса; песочница + HTTPS + запросы разрешений; HTML/CSS/JS.

**Cons:** нет большого каталога PWA; фрагментированные API; iOS WebKit в каждом браузере.

**Thoughts:** Установка/оффлайн не являются обязательными для _официального_ определения. См. [PWA](./6_web_api_apps.md#pwa-progressive-web-app).

**Sources:** [Doka — PWA](https://doka.guide/tools/pwa/) · [web.dev — критерии установки](https://web.dev/articles/install-criteria)

---

</details>

<details>
  <summary>Что такое FCP?</summary>

### Что такое FCP?

**Definition:** Первая отрисовка контента — время до появления первого текста, изображения, SVG или непустого холста. Пустой экран закончился, а не «готов».

**How it works:** Поддержка Web Vital (не Core, не рейтинг). Хорошо ≤ 1,8 с р75; плохой > 3 с. Медленный FCP обычно подразумевает медленный LCP (общий TTFB + блокирующий CSS). ИЯФ все еще может опоздать.

**Thoughts:** Исправлена ​​первая отрисовка с TTFB + ресурсами, блокирующими рендеринг; еще измеряю LCP и INP.

**See also:** [Первая отрисовка содержимого (FCP)](./1_html_performance.md#first-contentful-paint-fcp)

---

</details>

<details>
  <summary>Что такое LCP?</summary>

### Что такое LCP?

**Definition:** Наибольшая отрисовка контента — время, пока не будет отрисовано самое большое содержимое области просмотра (обычно главное изображение или большой текст). Браузер выбирает элемент.

**How it works:** Core Web Vital. Хорошо ≤ 2,5 с р75; плохой > 4 с. Четыре среза: TTFB, задержка загрузки ресурса, продолжительность загрузки ресурса, задержка рендеринга элемента. Не лениво загружайте героя; использовать `fetchpriority="high"` на 1–2 ресурсах.

**Thoughts:** Откройте для себя ресурс LCP в HTML как можно раньше; затем сократите TTFB, байты и CSS/JS, блокирующие рендеринг.

**See also:** [LCP (Largest Contentful Paint)](./1_html_performance.md#lcp-largest-contentful-paint)

---

</details>

<details>
  <summary>Как принять payment без хранения card numbers?</summary>

### Как принять payment без хранения card numbers?

**Definition:** **INP** — это основной веб-важный показатель оперативности реагирования (с **12 марта 2024 г.**). **FID** измерял только время ожидания **первого** взаимодействия до _запуска_ обработчика (а не работы обработчика или следующей отрисовки).

**How it works:** INP = щелчок/нажатие/клавиша → следующая краска для (почти) худшего взаимодействия за посещение (один выброс на 50). Фазы: задержка ввода + обработка + представление. Хорошо ≤ 200 мс p75; плохое > 500 мс. Маяк без пользователя сообщает **TBT**, а не INP.

**Thoughts:** Вырезать основной поток JS; сначала покажи пользовательский интерфейс, затем выдай (`scheduler.yield()` + `setTimeout` отступать).

**See also:** [INP (Взаимодействие со следующей отрисовкой)](./1_html_performance.md#inp-interaction-to-next-paint)

---

</details>

<details>
  <summary>Как you take a payment without storing card numbers?</summary>

### Как you take a payment without storing card numbers?

**Definition:** Поставщик (Stripe) токенизирует карту в своем iframe. Ваш сервер никогда не видит PAN. На самом деле это **подписанный вебхук**, а не обратный вызов браузера.

**How it works:** Оформление заказа → клиент платит → `checkout.session.completed` вебхук с `Stripe-Signature` → подтвердить → отметить заказ как оплаченный. Используйте ключи идемпотентности при создании.

**Thoughts:** Клиентский SDK для UX; вебхук для реестра.

**See also:** [Платежи, аналитика и сторонние SDK](./6_web_api_security.md#15-integrations)

---

</details>

<details>
  <summary>Lab Lighthouse зелёный, Search Console красный — почему?</summary>

### Lab Lighthouse зелёный, Search Console красный — почему?

**Definition:** Маяк **лаборатория** (один синтетический заход). Search Console Core Web Vitals – это **поле** CrUX на **p75** в течение **28 дней**. В рейтинге используется поле, а не Lighthouse.

**How it works:** В Lighthouse по умолчанию нет пользователя → **INP** отсутствует; **TBT** — это всего лишь прокси. Поле **CLS** включает прокрутку и поздние сдвиги. Поле **LCP** использует реальные области просмотра и теплые кэши. После исправления дождитесь скользящего окна. Отладка в DevTools; корабль на базе РУМ/`web-vitals`.

**Thoughts:** Лаборатория = воспроизводить. Поле p75 = пользователи и рейтинг.

**See also:** [Основные веб-показатели](./1_html_performance.md#core-web-vitals)

---

</details>

<details>
  <summary>Что такое TTFB and why does it matter?</summary>

### Что такое TTFB and why does it matter?

**Definition:** Время от начала навигации до первого байта ответа. Диагностика, а не Core Web Vital. Хорошо ≤ 0,8 с; плохое > 1,8 с.

**How it works:** Перенаправления + запуск ПО + DNS + TLS + происхождение до первого байта. Нет первого байта → нечего анализировать → ожидание FCP/LCP. Низкий TTFB еще не гарантирует хорошую LCP.

**Thoughts:** Исправлена ​​сеть (CDN, меньше перенаправлений) и источник (поток/SSG/ISR, `Server-Timing`). Затем оптимизируйте покраску.

**See also:** [TTFB](./1_html_performance.md#ttfb-time-to-first-byte)

---

</details>

<details>
  <summary>Как you optimize API and client performance?</summary>

### Как you optimize API and client performance?

**Answer:** Сначала измерения (RUM, APM/traces, Lighthouse, `Server-Timing`), затем самое медленное звено на пользовательском пути. **API:** pagination, индексы, убрать N+1, кэш (HTTP/Redis/CDN), compression, async для тяжёлых задач, меньше round trip (BFF/batch). **Клиент:** меньше бандл (split/lazy), меньше работы main thread (memo, virtualization, отложить third-party), Core Web Vitals (LCP images, INP long tasks, CLS layout). После каждого изменения — p95/p99 и field vitals.

**See also:** [API and client performance optimization](./6_web_api_performance.md#api-and-client-performance-optimization) · [§10 Performance](./4_react_ecosystem.md#10-performance) · [TTFB](./1_html_performance.md#ttfb-time-to-first-byte)

---

</details>

<details>
  <summary>Task 1 — Спроектируйте REST API для books</summary>

### Task 1 — Спроектируйте REST API для books

</details>

<details>
  <summary>Task 2 — Разберите Cache-Control scenario</summary>

### Task 2 — Разберите Cache-Control scenario

Получение списка конечных точек для CRUD на `/books` с методами и кодами состояния.

Контур

`GET /books` 200, `GET /books/:id` 200/404, `POST /books` 201, `PUT /books/:id` 200/404, `DELETE /books/:id` 204/404

---

</details>

<details>
  <summary>Общие</summary>

### Общие

Ответ имеет `Cache-Control: max-age=3600, public`. Когда произойдет повторная загрузка браузера?

Ответ

Может использоваться из кэша без повторной проверки в течение 3600 секунд; разрешены общие кэши (CDN).

---

</details>

<details>
  <summary>Junior — General (HTTP / REST)</summary>

### Junior — General (HTTP / REST)

// 12 Что такое ОТДЫХ? Какие методы запроса у нас есть? Что такое коды состояния?

---

</details>

<details>
  <summary>Middle — Общие</summary>

### Middle — Общие

1. Опишите пирамиду тестирования.
2. Какие типы автоматизированных тестов вы написали? Какие библиотеки и инструменты вы предпочитаете?
3. Что такое модульные тесты? Какое место они занимают в пирамиде тестирования?
4. Что такое покрытие кода? Требуется ли 100% покрытие?
5. Как запретить браузеру отправлять кэшированные HTTP-ответы?
6. Что такое XSS (межсайтовый скриптинг)?

---

</details>

<details>
  <summary>Общие</summary>

### Общие

1. Опишите пирамиду тестирования.
2. Какие типы автоматизированных тестов вы написали? Какие библиотеки и инструменты вы предпочитаете?
3. Что такое модульные тесты? Какое место они занимают в пирамиде тестирования?
4. Что такое покрытие кода? Требуется ли 100% покрытие?
5. Как запретить браузеру отправлять кэшированные HTTP-ответы?
6. Что такое XSS (межсайтовый скриптинг)?
   7.Объясните Observer и Pub/Sub — различия и примеры браузера/фреймворка.
7. Какова цель `self.addEventListener('fetch', …)` в сервисном работнике?
8. Что такое цикл событий? Объясните микрозадачи и макрозадачи.

---

</details>

<details>
  <summary>Общие вопросы</summary>

### Общие вопросы

1. Объяснить концепции функционального программирования.
2. Что такое TDD/BDD?
3. Подробно объясните, как работает HTTPS. Что находится в сертификате TLS и как подпись ЦС подтверждает открытый ключ сервера?
4. Какой технологический стек вы бы выбрали для клонирования известного проекта и почему?
5. Как поддерживать устаревшую кодовую базу при выпуске изменений?
   6.Если вы знаете несколько фреймворков — какой бы вы выбрали для следующего проекта и почему?
6. Что такое двигатель V8?

---

</details>
 frameworks — какой бы вы выбрали для следующего проекта и почему?
7. Что такое двигатель V8?

---

</details>
