# JS — Additional

Международный, RegExp, сигналы (TC39) и `eval`. Основы повседневного языка находятся в разделе «Ядро → Язык».

[ ← Назад в JS-хаб](./3_js.md)

### Related docs

- [Modules](./3_js_modules.md) · [Export and Import](./3_js_import_export.md) · [Dynamic imports](./3_js_dynamic_import.md) · [Язык JS](./3_js_core.md) · [Переменные](./3_js_variables.md) · [Типы данных](./3_js_data_types.md) · [Структуры данных](./3_js_data_structures.md) · [Операторы](./3_js_operators.md) · [Циклы](./3_js_loops.md) · [Массивы JS](./3_js_array.md) · [Функции JS](./3_js_functions.md) · [Objects](./3_js_objects.md) · [Classes](./3_js_classes.md) · [JS Async](./3_js_async.md) · [JS Interview](./3_js_interview.md)
- [Информатика](./7_special_cs.md)

### Main concepts

| Площадь           | Темы                              |
| ----------------- | --------------------------------- |
| **Международный** | Форматирование с учетом локали    |
| **регэксп**       | Узоры и флаги                     |
| **Сигналы**       | Реактивные клетки TC39 (стадия 1) |
| **Избегать**      | `eval`                            |

### Sections

> Теория `<details>` блоки следуют: **Определение** · **Как это работает** · **Когда использовать** · **Плюсы и минусы** · **Примеры** · **Мысли** · **Источники**. **Определение** обязательно; другие разделы являются необязательными.

- [Intl](#intl)
- [RegExp](#regexp)
- [Signals](#signals)
- [Avoid](#avoid)
- [Интервью](./3_js_interview.md)

Родственные темы: [Modules](./3_js_modules.md) · [Export and Import](./3_js_import_export.md) · [Dynamic imports](./3_js_dynamic_import.md) · [Язык](./3_js_core.md) · [Операторы](./3_js_operators.md) · [Циклы](./3_js_loops.md).

<a name="intl"></a>

## Intl

<details>
  <summary>Intl API</summary>

### Intl API

**Definition:** Форматирование дат, чисел, правил сортировки и множественного числа с учетом локали — встроено в механизмы JS.

**How it works:**

- Создайте форматтер один раз: `new Intl.NumberFormat(locale, options)`.
- Вызов `.format(value)` по значению — кэширует данные локали внутри себя.
- `Intl.Collator` для сортировки строк с учетом локали; `Intl.DateTimeFormat` для свиданий.

**When to use:** Отображение валюты, дата в локали пользователя, сортировка имен по языковым правилам.

**Pluses and minuses:**

| +                                | −                                             |
| -------------------------------- | --------------------------------------------- |
| Исправьте правила локали         | Размер комплекта Н/Д (встроенный)             |
| Лучше, чем ручное форматирование | Подробный API опций                           |
| С учетом часового пояса/локали   | Должна быть передана правильная строка локали |

**Examples:**

```js
new Intl.NumberFormat("de-DE", { style: "currency", currency: "EUR" }).format(
  99.9,
);
// "99,90 €"

new Intl.DateTimeFormat("en-US", { dateStyle: "medium" }).format(new Date());
```

**Sources:** [javascript.info/intl](https://javascript.info/intl)

</details>

<a name="regexp"></a>

## RegExp

<details>
  <summary>RegExp (advanced basics)</summary>

### RegExp (advanced basics)

**Definition:** Регулярные выражения соответствуют шаблонам в строках с помощью `/pattern/flags` or `new RegExp(pattern, flags)`.

**How it works:**

- Флаги: `g` глобальный, `i` нечувствителен к регистру, `m` многострочный, `s` точкаВсе, `u` юникод.
- Методы: `.test(str)`, `.exec(str)` (с группами захвата), `str.match`, `str.replace`, `str.split`.
- Группы захвата `(...)` и не захватывающий `(?:...)`.

**When to use:** Проверка ввода (по электронной почте), извлечение токенов, замена шаблонов. Предпочитайте специальные парсеры для сложных форматов (HTML, JSON).

**Pluses and minuses:**

| +                    | −                              |
| -------------------- | ------------------------------ |
| Мощный анализ текста | Трудно читать/обслуживать      |
| Встроенный, быстрый  | Катастрофический риск возврата |

| Краевые случаи Unicode |

**Examples:**

```js
/^\w+@\w+\.\w+$/.test("a@b.co"); // basic email shape
"a1b2".replace(/\d/g, "#"); // "a#b#"
const m = /(\d+)-(\d+)/.exec("2024-07"); // groups
```

**Sources:** [javascript.info/regular-expressions](https://javascript.info/regular-expressions)

</details>

<a name="signals"></a>

## Signals

<details>
  <summary>Signals (TC39)</summary>

<a name="signals"></a>

### Signals (TC39)

**Definition:** **Сигнал** — это первоклассная ячейка состояния, которая уведомляет иждивенцев об изменениях. Вы читаете с `.get()`, напиши с `.set()`, и движок строит DAG, кто от кого зависит. Предложение TC39 стандартизирует **график + автоматическое отслеживание**, а не интерфейс приложения. `effect()` API. 1 этап с апреля 2024 года.

**How it works:**

1. **`Signal.State`** — записываемая ячейка. **`Signal.Computed`** — ленивая формула. **`effect`** _нет_ в предложении; фреймворки строят это на `Signal.subtle.Watcher`.
2. **Автоматическое отслеживание:** локально для потока. `computing`. Во время выполнения вычисляемого обратного вызова каждый `.get()` регистрирует этот узел как источник. Вложенный вычисляемый B, который читает `light` владеет преимуществом; родительский элемент A зависит только от B. Списки источников перестраивают каждую оценку (**динамические зависимости**).
3. **Четыре цвета:** `clean` (кэшированный, свежий), `checked` (предок _могло_ измениться), `dirty` (определенно устаревший или никогда не читанный), `computing` (выполняется обратный вызов).
4. **Нажмите (`set`):** if `equals` (по умолчанию `Object.is`) говорит без изменений, стоп. Еще отметьте прямые раковины `dirty`, дальше тонет `checked` (никогда не понижайте версию `dirty`), поставить Наблюдателей `pending`, вызов `notify` **синхронно**. Перерасчета пока нет.
5. **Потянуть (`get`):** if `clean`, вернуть кеш. В противном случае идите крайний левый крайний `dirty` источник, пересчитать, сравнить. Без изменений → `checked` потомки уходят `clean` (волна замирает). Изменено → раскрасить их `dirty` и продолжить. Это **без сбоев**.
6. Ошибки кэшируются как значения (повторно выбрасываются до тех пор, пока не изменится источник). Рекурсивное самочтение является ошибкой. В течение `notify` двигатель **заморожен** — нет чтения/записи; `untrack` это не поднимает.
7. **`Signal.subtle`:** `Watcher`, `untrack`, `watched`/`unwatched` (лень подписываться на `resize` и т. д.), самоанализ DevTools. Наблюдатели поддерживают источники в живых — пропали без вести `unwatch()` утечки.

```
light (State) → canGo (Computed) → message (Computed) → effect (framework)
set() paints left → right.  get() recomputes right ← left, only if asked.
```

| Цвет        | Значение                         | Следующий `.get()`                          |
| ----------- | -------------------------------- | ------------------------------------------- |
| `clean`     | кэш-это правда                   | вернуть кэш                                 |
| `checked`   | возможно, устаревший (косвенный) | источники прогулок; может оставаться чистым |
| `dirty`     | должен пересчитать               | запустите обратный вызов, затем `equals`    |
| `computing` | обратный вызов выполняется       | чтение себя → ошибка                        |

Императив `let light = "red"` плюс руководство `render()` пары передаются в DOM и пересчитываются, даже если видимый результат не изменился. Pub/sub исправляет вопрос «кому сказать», но вы все равно подписываетесь вручную, и `render` должен знать, это зависит от `light` даже если его волнует только `message`. Сигналы делают график автоматическим.

**When to use:**

Детализированное состояние пользовательского интерфейса (Angular `signal()`, Вуэ `ref()`, Твердый `createSignal()`, Стройная 5 `$state()`, Preact, Qwik, MobX). Общие реактивные _модели_ во всех платформах, если примитив поставляется. Не для одноразовых событий (щелчок, сокет) — используйте события. Не RxJS: сигнал — это одно **текущее** значение, а не поток во времени. **`AbortSignal` не имеет отношения.**

#### Common pitfalls

Лечение `.get`/`.set` как Вью `.value` «компат» — авторы намеренно этого избежали. Планирование работы внутри `notify`. Забывчивость `unwatch()`. Чтение реальной зависимости внутри `untrack` (график устарел). Предполагается один глобальный планировщик эффектов.

**Pluses and minuses:**

| +                                                              | −                                                           |
| -------------------------------------------------------------- | ----------------------------------------------------------- |
| Автоматические подписки, без публикации/подписки вручную       | Еще не на языке (Этап 1)                                    |
| Ленивый + запомненный; `equals` останавливает напрасную работу | Планирование (когда запускать эффекты) зависит от платформы |
| DAG без сбоев и наивные штормы уведомлений                     | `untrack` / замороженный `notify` легко злоупотребить       |

**Examples:**

```js
const light = new Signal.State("red");
const canGo = new Signal.Computed(() => light.get() === "green");
const message = new Signal.Computed(() => (canGo.get() ? "go" : "stop"));

light.get(); // "red"
canGo.get(); // false  — first pull: callback runs, sources = [light]
message.get(); // "stop"

light.set("yellow");
// push: light changed → canGo dirty, message checked. No callbacks yet.
message.get();
// pull: canGo is dirty → recompute false (same as cache) → equals true
// → message stays "stop", DOM would not need a write
```

```js
const showDetails = new Signal.State(false);
const name = new Signal.State("Pumpkin");
const details = new Signal.State("Halloween classic");

const label = new Signal.Computed(() => {
  if (!showDetails.get()) return name.get();
  return `${name.get()}: ${details.get()}`;
});

label.get(); // "Pumpkin" — sources: showDetails, name
details.set("new"); // label does not recompute: details is not a source
showDetails.set(true);
label.get(); // "Pumpkin: new" — now sources include details
```

```js
const light = new Signal.State("red");
const cars = new Signal.Computed(() => light.get() === "green");
const pedestrians = new Signal.Computed(() => light.get() !== "green");
const board = new Signal.Computed(
  () => `cars:${cars.get()} walk:${pedestrians.get()}`,
);

light.set("green");
board.get();
// Naive pub/sub: board runs twice (cars then pedestrians).
// Signals: both parents dirty; board pulled once after they settle.
```

```js
const list = new Signal.State([1, 2, 3], {
  equals: (prev, next) => prev.length === next.length,
});
list.set([9, 8, 7]); // length 3 === 3 → no dirty paint
list.set([1]); // length changed → sinks dirty
```

```js
let pending = false;
const watcher = new Signal.subtle.Watcher(() => {
  if (pending) return;
  pending = true;
  queueMicrotask(() => {
    pending = false;
    for (const s of watcher.getPending()) s.get();
    watcher.watch(); // re-arm; otherwise next set() is missed
  });
});

function effect(callback) {
  let cleanup;
  const computed = new Signal.Computed(() => {
    cleanup?.();
    cleanup = callback();
  });
  watcher.watch(computed);
  computed.get();
  return () => {
    cleanup?.();
    watcher.unwatch(computed);
  };
}

effect(() => {
  element.innerText = message.get();
});
```

`notify` работает **внутри** `set()` — только расписание, никогда не пересчитывайте. После микрозадачи `getPending()` это живой грязный список.

```js
const debugMode = new Signal.State(false);
const items = new Signal.State([1, 2]);
const total = new Signal.Computed(() => {
  const n = items.get();
  if (Signal.subtle.untrack(() => debugMode.get())) {
    console.log("recompute total");
  }
  return n.length;
});
// debugMode flips do not dirty total — by design
```

**Thoughts:** `set()` только раскрашивает график; `get()` это единственное место, где происходит работа; эффекты остаются в рамках, потому что **когда** повторно запускать не является одним размером.

**Sources:** [README proposal-signals](https://github.com/proposal-signals/proposal-signals) · [signal-polyfill](https://github.com/proposal-signals/signal-polyfill) · Doka — Signals (Русаков, 31 августа 2026 г.)

</details>

<a name="avoid"></a>

## Avoid

<details>
  <summary>eval (anti-pattern)</summary>

### eval (anti-pattern)

**Definition:** `eval(string)` executes a string as code in the current lexical scope.

**How it works:**

- Parses and runs string as JS in current scope — can access local variables.
- Prevents JIT optimization; same security risks as `new Function` with user input.

**When to use:** Do not use with user input. Prefer `JSON.parse`, template engines, or proper parsers.

**Examples:**

```js
const expr = "2 + 2";
eval(expr); // 4 — same scope as caller; never pass user input

// Safer dynamic math: dedicated parser library, not eval
JSON.parse('{"a":1}'); // data only, not code
```

**Pluses and minuses:**

| +                      | −                                            |
| ---------------------- | -------------------------------------------- |
| Rare dynamic DSL cases | Security vulnerability (XSS → RCE)           |
|                        | Breaks JIT optimization in surrounding scope |
|                        | Hard to debug and audit                      |

**Sources:** [javascript.info/eval](https://javascript.info/eval) · [`new Function` (avoid)](./3_js_functions.md#new-function-avoid)

</details>
