# JS Async APIs — Timers, fetch, and AbortController

Timers, `fetch` helpers, `AbortController`, `requestIdleCallback`, and JSON response parsing.

### Related docs

- [JS Event Loop](./3_js_async_event_loop.md) · [JS Async/Await](./3_js_async_await.md) · [JSON](./3_js_data_structures.md#json)
- [HTTP — Fetch & AbortController](./6_web_api_http.md#fetch-api-abortcontroller-and-formdata) · [JS Interview](./3_js_interview.md)

[← Back to JS hub](./3_js.md) · [Async hub](./3_js_async.md)

### Main concepts

| Area | Topics |
| ---- | ------ |
| **Browser** | Timers, `AbortController`, `requestIdleCallback` |
| **Data** | `res.json()` and JSON pointers |

### Sections

- [AbortController](#abortcontroller)
- [Related APIs](#related-apis)
- [Interview](./3_js_interview.md)


Each topic: **Definition** · **How it works** · **When to use** · **Pluses and minuses** · **Examples** · **Thoughts** · **Sources**.

---
<a name="abortcontroller"></a>

## AbortController

<details>
  <summary>AbortController</summary>

### AbortController

**Definition:** `AbortController` обеспечивает `signal` отменить `fetch` и другие асинхронные операции.

**How it works:**

- `const ac = new AbortController();` — `ac.signal` это `AbortSignal`.
- Проходить `{ signal: ac.signal }` to `fetch` (и другие API, которые его поддерживают).
- `ac.abort()` — прерывает связанные операции; `fetch` отвергает с `AbortError`.
- `signal.aborted` и `signal.reason` проверить состояние.
- `AbortSignal.timeout(ms)` — сокращение для прерывания по тайм-ауту (современные браузеры/Node).
- Составление: один сигнал может прервать несколько выборок; Реагировать на вызовы очистки `abort()` при размонтировании.

**When to use:**

| Ситуация            | Узор                                                 |
| ------------------- | ---------------------------------------------------- |
| Пользователь уходит | `abort()` in `useEffect` уборка                      |
| Отказ от поиска     | Отменить предыдущий запрос при новом нажатии клавиши |
| Запросить тайм-аут  | `AbortSignal.timeout(5000)` или ручной таймер        |
| Победитель гонки    | Прервать проигравших после `Promise.race`            |

#### Common pitfalls

- Лечение `AbortError` как настоящая неудача — часто ожидаемая в навигации.
- Не прерывать размонтирование компонента — setState выдает предупреждение о размонтированном компоненте.
- Контроллер одноразового использования — создайте новый по запросу.

**Pluses and minuses:**

| +                                                                        | −                                                                    |
| ------------------------------------------------------------------------ | -------------------------------------------------------------------- |
| Отменяет запросы в полете                                                | Не все API поддерживают `AbortSignal`                                |
| Компонуемый с таймаутами                                                 | Необходимо подключить сигнал к каждому вызову                        |
| Предотвращает обновление пользовательского интерфейса устаревших ответов | Прерванная выборка по-прежнему отклоняется — дескриптор `AbortError` |
| Стандарт в экосистеме выборки                                            | Старые библиотеки требуют отмены вручную                             |

**Examples:**

```js
const controller = new AbortController();
const timeout = setTimeout(() => controller.abort(), 5000);

try {
  const res = await fetch("/api/data", { signal: controller.signal });
  clearTimeout(timeout);
  const data = await res.json();
} catch (err) {
  if (err.name === "AbortError") console.log("Request aborted");
  else throw err;
}
```

**Thoughts:** Прерывание устаревшего поиска `fetch` уменьшает работу основного потока и гонки — помогает **INP**. [6_web_api.md — INP](./6_web_api_security.md#inp-interaction-to-next-paint).

**Sources:** [javascript.info/fetch-abort](https://javascript.info/fetch-abort) — полные сведения о выборке в [HTTP — AbortController & FormData](./6_web_api_http.md#fetch-api-abortcontroller-and-formdata)

</details>

## Related APIs

<details>
  <summary>Timers</summary>

### Timers

**Definition:** `setTimeout` выполняет обратный вызов один раз после задержки; `setInterval` повторяется через фиксированный интервал. Оба планируют **макрозадачи**. `clearTimeout` / `clearInterval` отменить ожидающие обратные вызовы.

**How it works:**

- `setTimeout(fn, ms)` — минимальная задержка `ms`; фактическая задержка ≥ `ms` (ограничено до ~4 мс в браузерах при глубине вложенности ≥5).
- `setInterval(fn, ms)` — повторяется до тех пор, пока `clearInterval`; интервал включает в себя время обратного вызова — реальный разрыв между запусками может сокращаться или суммироваться.
- **Вложенный `setTimeout`** — запланируйте следующий запуск в **конце** текущего: гарантирует минимальную паузу между финишами; можно регулировать задержку (отсрочку) на итерацию.
- Возвращаемое значение – числовой идентификатор (браузер) или `Timeout` объект (Узел) — перейти к `clear*`.
- Обратный вызов выполняется как макрозадача — после текущего кода синхронизации и всех микрозадач.
- `setTimeout(fn, 0)` переходит к следующему ходу макрозадачи — не немедленно.
- Планировщик удерживает ссылку на обратный вызов до тех пор, пока он не запустится/не будет очищен — отмените неиспользуемые таймеры, чтобы освободить закрытую память.

| API                    | Поведение                                                 |
| ---------------------- | --------------------------------------------------------- |
| `setTimeout(fn, ms)`   | Запустить один раз после `ms` (минимум ~4 мс в браузерах) |
| `setInterval(fn, ms)`  | Повторяйте каждые `ms` пока не будет очищено              |
| Вложенный `setTimeout` | Следующий тик запланирован после предыдущего завершения   |
| `clearTimeout(id)`     | Отменить `setTimeout`                                     |
| `clearInterval(id)`    | Отменить `setInterval`                                    |

**When to use:**

| Использование                                      | Вместо этого предпочитаю                          |
| -------------------------------------------------- | ------------------------------------------------- |
| Устранение дребезга/дроссель                       | `requestAnimationFrame` для визуальных обновлений |
| Отложенное обновление пользовательского интерфейса | `queueMicrotask` для макроса перед следующим      |
| Опрос                                              | WebSocket/SSE для отправки с сервера              |
| `sleep` в асинхронном коде                         | `await new Promise(r => setTimeout(r, ms))`       |

#### Common pitfalls

- `this` в обратном вызове таймера — используйте функцию стрелки или `.bind`.
- Забывчивость `clearInterval` об изменении маршрута SPA — утечки и устаревшие обновления.
- Предполагая точное время — нагрузка цикла событий добавляет дрожание.
- Остаток `setInterval` (опрос, автосохранение) регулярно крадет основной поток — хуже для **INP**, чем одноразовый `setTimeout`. [6_web_api.md — INP](./6_web_api_security.md#inp-interaction-to-next-paint).

**Pluses and minuses:**

| +                                                | −                                                             |
| ------------------------------------------------ | ------------------------------------------------------------- |
| Простая отсрочка и планирование                  | Неточно для анимации (используйте `rAF`)                      |
| Работает везде                                   | `setInterval` складывается, если обработчик работает медленно |
| Хорошо подходит для устранения дребезга/дросселя | Фиксация минимальной задержки удивляет вложенными таймерами   |
| Макрозадача — позволяет рисовать между           | В середине периода отмены обратного вызова в полете не будет  |

**Examples:**

```js
const id = setTimeout(() => console.log("later"), 1000);
clearTimeout(id);

const interval = setInterval(() => tick(), 500);
clearInterval(interval);

// Nested setTimeout — delay between *ends*, optional backoff
let delay = 1000;
let timerId = setTimeout(function tick() {
  // work...
  delay = overloaded ? delay * 2 : 1000;
  timerId = setTimeout(tick, delay);
}, delay);

// Async sleep helper
const sleep = (ms) => new Promise((r) => setTimeout(r, ms));
await sleep(1000);
```

**Sources:** [javascript.info/settimeout-setinterval](https://javascript.info/settimeout-setinterval)

</details>

<details>
  <summary>requestIdleCallback</summary>

### requestIdleCallback

**Definition:** API браузера, который запускает обратный вызов во время **периода простоя** (после работы с макетом/рисованием, когда у цикла событий есть свободное время). Это **не** микрозадача Promise и **не** `requestAnimationFrame`.

**How it works:**

- `requestIdleCallback(cb, { timeout })` — `cb` получает `IdleDeadline`: `timeRemaining()` (мс осталось в этом незанятом фрагменте) и `didTimeout`.
- Уступайте, когда `timeRemaining()` достигает ~0, чтобы браузер мог рисовать. Если ты пройдешь `timeout`, обратный вызов по-прежнему выполняется по истечении крайнего срока (`didTimeout === true`) даже если вкладка была занята — `timeRemaining()` тогда ~0.
- Без `timeout`, занятая страница **возможно, никогда** не позвонит вам.
- Недоступно в Node. Предпочитать `scheduler.yield()` / работа по частям, если вам нужна более широкая поддержка; РАФ для визуальных рамок.

**When to use:** Отложить некритичный JS; контрастирует с rAF (должен выполняться перед покраской) и микрозадачами (выполняется перед следующей макрозадачой **и** может задерживать покраску, если она никогда не истощается).

**Pluses and minuses:**

| +                                                             | −                                                                                               |
| ------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| Низкоприоритетная работа (аналитика, предварительная выборка) | Можно голодать без `timeout`                                                                    |
| Сотрудничает с рендерингом                                    | Не является заменой веб-работников при выполнении задач с интенсивным использованием процессора |

**Examples:**

```js
requestIdleCallback(
  (deadline) => {
    while (deadline.timeRemaining() > 0 && queue.length) {
      work(queue.pop());
    }
  },
  { timeout: 2000 },
);
```

**Thoughts:** _В режиме ожидания работа выполняется, когда браузер не рисует; бесконечная очередь микрозадач никогда не достигает состояния простоя или отрисовки._

**Sources:** [MDN — requestIdleCallback](https://developer.mozilla.org/en-US/docs/Web/API/Window/requestIdleCallback) · [IdleDeadline](https://developer.mozilla.org/en-US/docs/Web/API/IdleDeadline)

</details>

<details>
  <summary>JSON</summary>

### JSON

**Definition:** In async browser/Node code, JSON is almost always moved as **text** over the wire, then parsed into JS values with `JSON.parse` or `Response.json()`.

**How it works:**

- **Fetch response:** `const data = await res.json()` — reads body text and parses (throws on invalid JSON).
- **Request body:** `fetch(url, { method: "POST", headers: { "Content-Type": "application/json" }, body: JSON.stringify(payload) })`.
- `JSON.stringify` drops `undefined`, functions, and `Symbol` keys; `Date` becomes ISO strings — not round-tripped as `Date` without reviver.
- `JSON.parse(text, reviver)` can revive dates or filter keys; still no comments or trailing commas in strict JSON.
- Full syntax, `structuredClone`, and pitfalls → [JSON (Data Structures)](./3_js_data_structures.md#json). Config files with comments → [YAML](./10_build_ci.md#yaml).

**When to use:** REST APIs, `localStorage` snapshots, WebSocket text frames. Use `FormData` or binary for uploads.

**Examples:**

```js
const res = await fetch("/api/user");
if (!res.ok) throw new Error(res.statusText);
const user = await res.json();

await fetch("/api/user", {
  method: "PUT",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ name: "Ann" }),
});
```

**Pluses and minuses:**

| +                    | −                                      |
| -------------------- | -------------------------------------- |
| Universal wire format | No types, no `undefined`, no functions |
| Built into all engines | Parse throws on any invalid syntax   |

**Thoughts:** Fetch delivers JSON **text**; parsing is a separate step — handle `res.ok` and parse errors explicitly.

**Sources:** [MDN Response.json()](https://developer.mozilla.org/en-US/docs/Web/API/Response/json) · [JSON (Data Structures)](./3_js_data_structures.md#json)

</details>

---

Interview Q&A and coding tasks live in [JS — Interview](./3_js_interview.md).

