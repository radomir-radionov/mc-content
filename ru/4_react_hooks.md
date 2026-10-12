# React — Hooks

Крюки опорные и аварийные люки.

[ ← Вернуться в центр](./4_react.md)

### Related docs

- [React — Core](./4_react_core.md) · [React — Hooks](./4_react_hooks.md) · [React — Components](./4_react_components.md) · [React — APIs](./4_react_apis.md) · [React — React 19](./4_react_server.md) · [React — Ecosystem](./4_react_ecosystem.md) · [React — Интервью](./4_react_interview.md)

### Sections

- [5. Hooks reference](#5-hooks-reference)
- [6. Escape hatches](#6-escape-hatches)

---

## 5. Hooks reference

### Overview

Каталог встроенных крючков

<details>
  <summary>Built-in hooks catalog</summary>

### Built-in hooks catalog

**Definition:** Указатель встроенных перехватчиков React, сгруппированных по ответственности — состояние, эффекты, ссылки, производительность и API React 19.

**How it works:**

1. **Изменяемые данные пользовательского интерфейса?** → `useState` or `useReducer`
2. **Синхронизировать с внешней системой?** → `useEffect` / `useLayoutEffect`
3. **DOM или значение без повторного рендеринга?** → `useRef`
4. **Общие данные без детализации?** → `useContext`
5. **Отложить дорогостоящий рендеринг?** → `useTransition` / `useDeferredValue`
6. **Вычисление кэша или стабильный обратный вызов?** → `useMemo` / `useCallback` (сначала профиль)

| Категория              | Крючки                                                                      |
| ---------------------- | --------------------------------------------------------------------------- |
| **Штат**               | `useState`, `useReducer`, `useActionState - 19`, `useOptimistic - 19`       |
| **Контекст**           | `useContext`, `use() - 19`                                                  |
| **Ссылка**             | `useRef`, `useImperativeHandle`, `ref as prop - 19`                         |
| **Эффект**             | `useEffect`, `useLayoutEffect`, `useInsertionEffect`, `useEffectEvent - 19` |
| **Производительность** | `useMemo`, `useCallback`, `useTransition`, `useDeferredValue`               |
| **Другое**             | `useId`, `useSyncExternalStore`, `useDebugValue`, `useFormStatus - 19`      |

**Examples:**

```tsx
const [n, setN] = useState(0); // UI data
const ref = useRef<HTMLInputElement>(null); // no re-render
useEffect(() => {
  document.title = String(n);
}, [n]); // after paint
```

**Sources:**

[react.dev/reference/react/hooks](https://react.dev/reference/react/hooks)

Правила крючков

</details>

<details>
  <summary>Rules of Hooks</summary>

### Rules of Hooks

**Definition:** Два инвариантных правила, которые обеспечивают единообразие состояния связанного списка React при рендеринге. Часть [Rules of Hooks](./4_react_core.md#rules-of-hooks) на Core (см. [Rules of React catalog](./4_react_core.md#rules-of-react-catalog)).

**How it works:**

React хранит перехватчики на волокне в виде **связанного списка** — сначала `useState` это слот 0, первый `useEffect` это слот 1 и т. д. Каждый рендер проходит по списку в **одном и том же порядке**. Условные перехваты смещают слоты → состояние привязывается к неправильному перехватчику → сбои или тихие ошибки.

1. Вызывайте хуки только на **верхнем уровне**, а не внутри условий, циклов, вложенных функций и т. д. `try`/`catch`/`finally`, или после раннего `return`
2. Вызывайте перехватчики только из **компонентов функций React** или **пользовательских перехватчиков** (`use`\*)

**When to use:** Всегда. Давать возможность `eslint-plugin-react-hooks` (`rules-of-hooks` + `exhaustive-deps`). Продолжение интервью: _ «Если мне нужен хук только при входе в систему, я все равно вызываю его при каждом рендеринге и ставлю `if (loggedIn)` внутри эффекта — я никогда не пропускаю вызов»._

**Examples:**

```tsx
// Bad — conditional hook
if (loggedIn) useEffect(() => {}, []);

// Good — condition inside effect
useEffect(() => {
  if (loggedIn) subscribe();
}, [loggedIn]);
```

**Thoughts:** _Одинаковые хуки, тот же порядок, при каждом рендеринге._

**See also:** [Rules of Hooks (Core)](./4_react_core.md#rules-of-hooks) · [Custom hooks](#custom-hooks) · [React Fiber](./4_react_core.md#react-fiber)

**Sources:**

[react.dev/reference/rules/rules-of-hooks](https://react.dev/reference/rules/rules-of-hooks)

useState

</details>

### State

<details>
  <summary>useState</summary>

### useState

**Definition:** `useState(initial)` объявляет переменную состояния и `set` функция. Установщик планирует создание **следующего снимка** — он не изменяет значение в текущем рендеринге.

**How it works:**

- React сохраняет значение в волокне. Первый рендер использует `initial`; позже игнорирует `initial`.
- Передайте **функцию** в качестве начальной (`useState(createTodos)`, нет `createTodos()`) для **lazy init** — React вызывает его только при первом монтировании. В строгом режиме (dev) инициализатор может запускаться дважды; держите его **чистым**.
- `setX(next)` or `setX(prev => next)`. Программы обновления ставятся в очередь и применяются по порядку при следующем рендеринге — это необходимо, когда несколько обновлений происходят в **одном событии** (`setN(n => n + 1)` дважды → +2).
- `set` **не** изменяет локальную переменную в работающем обработчике: после `setCount(1)`, `count` остается старым снимком до следующего рендеринга.
- Если `Object.is(next, current)`, React **пропускает** повторный рендеринг. Мутация тогда `setItems(items)` это не операция.
- `set` идентичность **стабильна** (можно исключить из описания эффектов). React [batches](./4_react_core.md#batching) обновляется после обработчиков событий.
- Чтобы **сохранить функцию** в состоянии, оберните ее: `useState(() => fn)` / `setFn(() => otherFn)` — в противном случае React рассматривает его как инициализатор/обновитель.

_"Состояние — это снимок каждого рендеринга. Я обновляю неизменно, поэтому `Object.is` видит новую ссылку. Я использую средство обновления, когда следующее значение зависит от предыдущего в том же событии»._

**When to use:**

Локальный пользовательский интерфейс — переключатели, ввод, выбор, модальные окна. **Не** производные данные отображения (вычисления при рендеринге) или кэш сервера (запрос/RSC).

| API                     | Использование                                       |
| ----------------------- | --------------------------------------------------- |
| `useState(initial)`     | Дешевая первоначальная стоимость                    |
| `useState(createTodos)` | Дорогой init — передать функцию, нет `()`           |
| `setN(n => n + 1)`      | Следующее состояние от предыдущего в том же событии |
| `setN(count + 1)`       | Прекрасно, если у вас уже есть снимок               |

#### Common pitfalls

`onClick={fn()}` (вызовы во время рендеринга → «Слишком много повторных рендеров»). Ведение журнала `count` сразу после `setCount` все еще показывает старое значение. `useState(expensive())` пересчитывает `expensive` каждый рендер.

**Pluses and minuses:**

| +                                            | −                                                        |
| -------------------------------------------- | -------------------------------------------------------- |
| Простая ментальная модель, стабильный сеттер | Легко изменять объекты и пропускать обновления           |
| Ленивая инициализация + очередь обновлений   | Множество связанных полей → `useReducer` яснее           |
| В пакете с другими `set`в одном мероприятии  | Функции хранения требуют дополнительных `() =>` упаковка |

**Examples:**

```tsx
const [user, setUser] = useState<User | null>(null);
setUser((u) => (u ? { ...u, name: "Ada" } : null));

// Interview trap — same reference, UI may not update
// items.push(x); setItems(items);
setItems((prev) => [...prev, x]);
```

**Thoughts:** _`set` ставит в очередь следующий снимок; переменные текущего рендеринга никогда не меняются._

**See also:** [Состояние как снимок](./4_react_core.md#state-as-snapshot) · [useReducer](#usereducer-1) · [Пакетная обработка](./4_react_core.md#batching)

**Sources:**

[react.dev/reference/react/useState](https://react.dev/reference/react/useState)

useReducer

</details>

<details>
  <summary>useReducer</summary>

### useReducer

**Definition:** Государственный крюк, управляемый `(state, action) => newState`. То же хранилище волокон, что и `useState`, но **все переходы** выполняются через одну функцию. `dispatch` **стабилен** — передайте его без `useCallback`.

**How it works:**

`dispatch(action)` ставит обновление в очередь; при следующем рендеринге запускается React `reducer(prev, action)` (чистый). Ленивая инициализация: `useReducer(reducer, initArg, initFn)` — `initFn(initArg)` запускается один раз. Предпочитаю бросаться на неизвестные действия, чтобы опечатки не были громкими.

_“`useReducer` когда обновления являются конечным автоматом. `useState` когда это одно значение. `dispatch` стабилен, поэтому детям не нужны запоминаемые обратные вызовы, чтобы просто позвонить»._

**When to use:** Многополевые формы, мастера, «следующее состояние зависит от нескольких полей». **Ни** ни одного логического значения/счетчика.

**Pluses and minuses:**

| +                                              | −                                 |
| ---------------------------------------------- | --------------------------------- |
| Централизованные переходы, легко тестируемые   | Шаблон для крошечного государства |
| Стабильный `dispatch`                          | Оверкилл против двоих `useState`s |
| Та же идея, что и у Redux, локально для дерева | Все еще не глобальный магазин     |

**Examples:**

```tsx
type Action = { type: "increment" } | { type: "set"; value: number };

function reducer(state: { count: number }, action: Action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "set":
      return { count: action.value };
    default: {
      const _exhaustive: never = action;
      return _exhaustive;
    }
  }
}

const [state, dispatch] = useReducer(reducer, { count: 0 });
dispatch({ type: "increment" });
```

**See also:** [useReducer в разделе 4](#usereducer) · [useState](#usestate)

**Sources:**

[react.dev/reference/react/useReducer](https://react.dev/reference/react/useReducer)

useActionState - 19

</details>

<details>
  <summary>useActionState - 19</summary>

### useActionState - 19

**Definition:** Wraps an action function; returns `[state, formAction, isPending]`. React 19 replacement for the old `useFormState` name.

**How it works:** Action signature `(previousState, payload) => nextState | Promise<nextState>`. For forms, payload is `FormData`. `isPending` is true while the Promise resolves. Pass `formAction` to `<form action={formAction}>`.

**When to use:** Forms and mutations where you need result state (error message, success text) + pending flag without manual `useState`.

**Examples:**

```tsx
const [message, formAction, isPending] = useActionState(
  async (_prev, formData: FormData) => {
    const err = await updateUser(formData);
    return err ?? "Saved";
  },
  null,
);

return (
  <form action={formAction}>
    <button disabled={isPending}>Save</button>
    {message && <p>{message}</p>}
  </form>
);
```

**See also:** [Actions](./4_react_server.md#actions) · [useFormStatus - 19](#useformstatus-19)

**Sources:**

[react.dev/reference/react/useActionState](https://react.dev/reference/react/useActionState)

useOptimistic - 19

</details>

<details>
  <summary>useOptimistic - 19</summary>

### useOptimistic - 19

**Definition:** Shows **optimistic UI** immediately while an async action is in flight; **reverts** to real state when the action completes or fails.

**How it works:** `useOptimistic(realState, updateFn)` returns `[optimisticState, addOptimistic]`. Call `addOptimistic` inside an action before `await` — UI updates instantly. When the action finishes and real state updates, optimistic layer discards.

**When to use:** Like counts, cart quantity, message "sent" status, reordering lists before server confirms.

**Examples:**

```tsx
const [optimisticCount, addOptimistic] = useOptimistic(
  count,
  (c, delta: number) => c + delta,
);

async function handleLike() {
  addOptimistic(1);
  await saveLike();
}
```

**See also:** [Actions](./4_react_server.md#actions) · [useActionState - 19](#useactionstate-19)

**Sources:**

[react.dev/reference/react/useOptimistic](https://react.dev/reference/react/useOptimistic)

использоватьконтекст

</details>

### Context

<details>
  <summary>useContext</summary>

### useContext

**Definition:** Считывает ближайшее значение поставщика, указанное выше в дереве. Компонент **перерисовывается всякий раз, когда это значение изменяется** (`Object.is`).

**How it works:**

`createContext(default)` — значение по умолчанию используется только в том случае, если поставщика **нет**. `useContext(Ctx)` направляет волокна вверх. Реакция 19: `<ThemeContext value={theme}>` (no `.Provider` необходимый). Контекст — это **не** высокочастотное хранилище: каждый потребитель выполняет повторный рендеринг при изменении значения.

_"Контекст для редких, древовидных значений. Если многие компоненты перерисовываются при каждом нажатии клавиши, значение слишком горячее — разделите или используйте хранилище"._

**When to use:**

Тема, локаль, сеанс аутентификации, DI клиента. **Не** корзина для покупок, которая обновляется при каждом клике — используйте Zustand/Redux или поднимите состояние.

#### Common pitfalls

`value={{ user, theme }}` создает новый объект при каждом родительском рендеринге → все потребители обновляются. Разделите контексты или запомните. Не используйте контекст для данных сервера (Query/RSC).

**Pluses and minuses:**

| +                                         | −                                                             |
| ----------------------------------------- | ------------------------------------------------------------- |
| Избегает винтового бурения                | Все потребители повторно выполняют рендеринг `value` изменить |
| Встроенная, без дополнительной библиотеки | Легко поместить слишком много в один объект                   |

**Examples:**

```tsx
const ThemeContext = createContext<"light" | "dark">("light");

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return <button className={theme}>Click</button>;
}

<ThemeContext value={theme}>
  <App />
</ThemeContext>;
```

**See also:** [Реквизит](./4_react_core.md#props) · [Подъём состояния](./4_react_core.md#lifting-state-up)

**Sources:**

[react.dev/reference/react/useContext](https://react.dev/reference/react/useContext)

use() - 19

</details>

<details>
  <summary>use() - 19</summary>

### use() - 19

**Definition:** Reads a **Promise** or **Context** value during render. For Promises, **suspends** (throws to Suspense) until resolved.

**How it works:**

- `use(promise)` — if pending, component suspends → nearest `<Suspense>` shows fallback; on resolve, retries render with value
- `use(context)` — React 19 reads context without `useContext` in some cases (check docs)
- Cache the Promise **outside** render — don't `use(fetch())` inline every render

**When to use:** Server-passed promises, async RSC boundaries, reading context in loops (React 19).

**Examples:**

```tsx
function Message({ messagePromise }: { messagePromise: Promise<Message> }) {
  const message = use(messagePromise);
  return <p>{message.text}</p>;
}
```

**See also:** [Streaming & Suspense](./4_react_server.md#streaming--suspense) · [useContext](#usecontext)

**Sources:**

[react.dev/reference/react/use](https://react.dev/reference/react/use)

использованиеЭффект

</details>

### Effects

<details>
  <summary>useEffect</summary>

### useEffect

**Definition:** Объявляет **синхронизацию** с внешней системой (сеть, DOM API, подписки, сторонние виджеты). React запускает установку после фиксации компонента; это **не** класс `componentDidMount` клон. Возврат `undefined`. Эффекты выполняются **только на клиенте** — никогда во время SSR.

**How it works:**

1. После фиксации запускается React. `setup`. `setup` может вернуть **очистку**.
2. При следующем коммите, если **любая** зависимость изменилась (`Object.is`), React запускает **очистку со старыми значениями**, а затем **настройку с использованием новых значений**.
3. При размонтировании React запускает очистку.
4. Зависимости должны представлять собой **встроенный массив** каждого **реактивного** значения, используемого при настройке (свойства, состояние и локальные значения из тела компонента). Опустите массив → запускайте после **каждого** коммита. `[]` → только после **первого** коммита (плюс дополнительный цикл строгого режима в разработке).
5. Очистка **не** «только при размонтировании». Он также запускается перед повторным запуском эффекта при изменении глубины.
6. **Строгий режим (для разработчиков):** дополнительная настройка → очистка → настройка при монтировании. Производство монтируется один раз. Если вы видите двойное получение/подписку, ваша очистка должна отменить настройку (отключиться, `abort()`, чистый интервал).
7. Время по умолчанию: после **paint**, чтобы браузер мог сначала отобразить пользовательский интерфейс. Визуальный размер/положение, которое будет мерцать → [`useLayoutEffect`](#uselayouteffect).

| Вы пишете             | При запуске установки                                                    |
| --------------------- | ------------------------------------------------------------------------ |
| нет второго аргумента | После каждого коммита                                                    |
| `[]`                  | После первого коммита (переподключение только в случае перемонтирования) |
| `[roomId, url]`       | После первого коммита, а затем, когда эти значения изменяются            |

Отсутствуют deps → **устаревшее закрытие** (эффект видит старое `count` / `user`). Дополнительные зависимости объекта/функции, созданные при рендеринге → эффект каждый раз запускается повторно (`{} !== {}`). Исправление: зависьте от примитивов или **создавайте объект/функцию внутри** эффекта (предпочтительнее, чем заключать все в `useCallback`).

- «Я использую `useEffect` для подключения к чему-то, чем не владеет React. В массиве зависимостей перечислены реактивные значения, которые должны перезапустить это соединение. Очистка отключается, поэтому строгий режим и изменения реквизита не просачиваются. Если я только получаю данные пользовательского интерфейса или обрабатываю клик, я не использую эффект».\*

**When to use:**

Подписаться на WebSocket/`matchMedia`, запустите таймер, прикрепите виджет, не относящийся к React, синхронизируйте `document.title`, запишите «просмотренная страница». Извлечение **может** жить здесь с флагом игнорирования / `AbortController`, но документы React предпочитают RSC, загрузчики или TanStack Query.

Производные значения (`fullName = first + last`), реагирование на клик (обработчик событий), сброс состояния (`key={userId}`), уведомление родителя о клике (вызов обратного вызова в обработчике), связывание `setA` затем `setB` от А.

#### Common pitfalls

- Никакой уборки `setInterval` / слушатели → Двойные галочки в строгом режиме, утечки при размонтировании.
- Объект/функция в деталях воссоздается при каждом рендеринге → цикле или повторном подключении шторма.
- `useEffect(() => { setX(f(y)); }, [y])` когда `f(y)` можно вычислить в рендере.
- Внесение `useLayoutEffect` — Блокирует краску.
- Очистка рассматривается как «только размонтирование» — она также запускается, когда `roomId` изменения.

**Pluses and minuses:**

| +                                                  | −                                                             |
| -------------------------------------------------- | ------------------------------------------------------------- |
| Очистка пар с настройкой (без утечек сабвуферов)   | Чрезмерно используется как «свалка жизненного цикла»          |
| Deps делает повторную синхронизацию явной          | Устаревшие замыкания/бесконечные циклы, если значения неверны |
| В поверхностях строгого режима отсутствует очистка | Только для клиента; fetch-in-effect требует обработки гонок   |

**Examples:**

```tsx
useEffect(() => {
  const conn = createConnection(serverUrl, roomId);
  conn.connect();
  return () => conn.disconnect();
}, [serverUrl, roomId]);
```

**Пример — выборка без гонок** (шаблон документации: флаг игнорирования):

```tsx
useEffect(() => {
  let ignore = false;
  setBio(null);
  fetchBio(person).then((result) => {
    if (!ignore) setBio(result);
  });
  return () => {
    ignore = true;
  };
}, [person]);
```

**Не** выполнять обратный вызов эффекта `async` — возвращенный промис не является очисткой. Используйте внутренний `async` функция или IIFE; прервать/игнорировать в возвращенной очистке.

**Thoughts:** _Эффекты синхронизируются с внешним миром. Deps = когда выполнять повторную синхронизацию. Очистка = отменить последнюю синхронизацию._

**See also:** [Возможно, вам не понадобится эффект](#вам-может-не-нужен-эффект) · [useLayoutEffect](#uselayouteffect) · [useEffectEvent](#useeffectevent-react-192) · [Строгий режим](./4_react_core.md#strict-mode)

**Sources:**

[react.dev/reference/react/useEffect](https://react.dev/reference/react/useEffect) · [Возможно, вам не понадобится эффект](https://react.dev/learn/you-might-not-need-an-effect)

использоватьLayoutEffect

</details>

<details>
  <summary>useLayoutEffect</summary>

### useLayoutEffect

**Definition:** Та же подпись, что и `useEffect` но срабатывает **синхронно после мутаций DOM, перед отрисовкой браузера**.

**How it works:**

| Крюк                 | Когда                           | Блокирует краску? |
| -------------------- | ------------------------------- | ----------------- |
| `useInsertionEffect` | До мутации DOM                  | Да                |
| `useLayoutEffect`    | После мутации DOM, до отрисовки | Да                |
| `useEffect`          | После покраски                  | Нет               |

**When to use:**

Измерьте геометрию DOM, синхронизируйте положение прокрутки, наведите фокус без вспышки, расположите всплывающие подсказки. По умолчанию `useEffect` для получения/подписок.

#### Common pitfalls

Получение данных в `useLayoutEffect` — Блокирует краску. Разбивка макета из-за повторяющихся циклов измерения/записи.

**Examples:**

```tsx
useLayoutEffect(() => {
  const height = ref.current?.getBoundingClientRect().height;
  setTooltipTop(height);
}, [items]);
```

**Thoughts:** _Используйте, когда пользователь не должен видеть неправильный кадр._

**See also:** [Конвейер рендеринга](./4_react_core.md#render-pipeline) · [useEffect](#useeffect) · [useInsertionEffect](#useinsertioneffect)

**Sources:**

[react.dev/reference/react/useLayoutEffect](https://react.dev/reference/react/useLayoutEffect)

использоватьInsertionEffect

</details>

<details>
  <summary>useInsertionEffect</summary>

### useInsertionEffect

**Definition:** Хук эффекта, который запускается **синхронно после рендеринга, до того, как React применит мутации DOM** — самый ранний из трёх хуков эффектов.

**How it works:** Заказ на фиксацию: рендер → `useInsertionEffect` → Мутации DOM → `useLayoutEffect` → краска → `useEffect`. React запускает эффекты вставки, поэтому библиотеки внедрения стилей могут вставлять `<style>` теги перед тем, как браузер прочитает макет из обновленного DOM. Очистка выполняется перед следующим эффектом вставки на том же компоненте.

**When to use:** Библиотеки CSS-in-JS (Emotion, стилизованные компоненты, среда выполнения ванильного экстракта). **Нет** для побочных эффектов на уровне приложения, получения данных или измерения DOM — используйте `useLayoutEffect` or `useEffect`.

**Pluses and minuses:**

| +                                             | −                                                                |
| --------------------------------------------- | ---------------------------------------------------------------- |
| Правильный порядок внедрения CSS-in-JS        | Блокирует макет при неправильном использовании в коде приложения |
| Выделенный API — без борьбы `useLayoutEffect` | Невозможно прочитать макет из DOM (запускается до мутаций)       |
| Очистка пар с удалением стилей                | Почти никогда не требуется в компонентах приложений              |

**Examples:**

```tsx
useInsertionEffect(() => {
  insertStyles(key, styles);
  return () => removeStyles(key);
}, [key, styles]);
```

**Thoughts:** Разработчикам приложений практически никогда не следует использовать этот крючок. Если вы не создаете библиотеку стилей, предпочитайте `useLayoutEffect` (измерить DOM) или `useEffect` (все остальное).

**Sources:**

[react.dev/reference/react/useInsertionEffect](https://react.dev/reference/react/useInsertionEffect)

useEffectEvent - 19

</details>

<details>
  <summary>useEffectEvent - 19</summary>

### useEffectEvent - 19

**Definition:** `useEffectEvent(fn)` creates a stable function that always sees the latest props/state without being an effect dependency.

**How it works:** Put _reactive_ values (`roomId`) in the effect deps so the effect re-subscribes when they change. Put _event_ logic (`muted`, `theme`) inside `useEffectEvent` so those values stay fresh without reconnecting.

**When to use:** Chat reconnect on `roomId` only; analytics that read latest user without re-binding listeners.

**Examples:**

```tsx
const onConnected = useEffectEvent(() => {
  showNotification("Connected!", theme);
});

useEffect(() => {
  const conn = createConnection(roomId);
  conn.on("connected", onConnected);
  return () => conn.disconnect();
}, [roomId]);
```

**Sources:**

[react.dev/reference/react/useEffectEvent](https://react.dev/reference/react/useEffectEvent)

использоватьRef

</details>

### Ref

<details>
  <summary>useRef</summary>

### useRef

**Definition:** Изменяемый `{ current }` box, который **выдерживает повторный рендеринг**. Письмо `.current` **не** планирует рендеринг.

**How it works:**

Одна и та же идентичность объекта при каждом рендеринге (в отличие от `createRef()` в теле функции). После фиксации React устанавливает `ref.current` к узлу DOM, когда вы передаете `ref={el}` на главном элементе. Чтение/запись `.current` в **эффектах и ​​обработчиках событий**, а не во время рендеринга (за исключением однократной отложенной инициализации `.current`).

_“`useRef` является переменной экземпляра. `useState` предназначен для всего, что пользователь должен видеть. Я не читаю рефы во время рендера»._

**When to use:**

| Дело                            | Узор                                                                    |
| ------------------------------- | ----------------------------------------------------------------------- |
| Доступ к DOM                    | `inputRef.current?.focus()` в обработчике кликов или эффекте            |
| Поля экземпляра                 | Идентификатор таймера, WebSocket, предыдущая опора (`prev.current = x`) |
| Избегайте повторного рендеринга | Значение, которое пользовательский интерфейс не отображает              |

#### Common pitfalls

Использование ссылки для отображаемых данных. `createRef()` внутри функционального компонента (новый объект при каждом рендеринге).

**Pluses and minuses:**

| +                                                         | −                                                                  |
| --------------------------------------------------------- | ------------------------------------------------------------------ |
| Никаких дополнительных рендеров для таймеров/DOM          | Изменение `.current` не будет обновлять пользовательский интерфейс |
| Стабильный объект — можно пропустить в некоторых разделах | Не замена государству                                              |

**Examples:**

```tsx
const inputRef = useRef<HTMLInputElement>(null);
useEffect(() => {
  inputRef.current?.focus();
}, []);

const prevCount = useRef<number>(undefined);
useEffect(() => {
  prevCount.current = count;
}, [count]);
```

**See also:** [ref as prop - 19](#ref-as-prop-19) · [useRef](#useref)

**Sources:**

[react.dev/reference/react/useRef](https://react.dev/reference/react/useRef)

использоватьImperativeHandle

</details>

<details>
  <summary>useImperativeHandle</summary>

### useImperativeHandle

**Definition:** Настраивает **значение экземпляра**, которое родительский элемент получает при присоединении `ref` для вашего компонента — вместо узла DOM вы предоставляете курируемый объект API.

**How it works:** Сопряжение с `ref` (React 19 prop или `forwardRef`). При фиксации React вызывает вашу фабрику `() => ({ focus, scroll, ... })` и присваивает результат `ref.current`. Необязательный массив зависимостей управляет моментом воссоздания дескриптора. Внутренние ссылки DOM остаются конфиденциальными через `useRef` внутри ребенка.

**When to use:** Сфокусируйте ввод внутри компонента составного поля, выставьте `play()`/`pause()` в обертке видео интегрируйте виджеты, не относящиеся к React. **Предпочитаю** декларативные реквизиты (`isOpen`, `onSubmit`) и `autoFocus` первый.

**Pluses and minuses:**

| +                                                                                           | −                                                                            |
| ------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| Инкапсулирует императивные операции DOM в стабильном API                                    | Нарушает декларативную модель React                                          |
| Полезно для ловушек фокуса, медиаплееров, виджетов карт                                     | Сложнее тестировать и рассуждать, чем о реквизите                            |
| Ограничивает то, что родитель может вызывать (по сравнению с необработанной ссылкой на DOM) | Злоупотребление, когда `autoFocus` или реквизита обратного вызова достаточно |

**Examples:**

```tsx
type InputHandle = { focus: () => void; clear: () => void };

function TextField({ ref, label }: { ref?: Ref<InputHandle>; label: string }) {
  const inputRef = useRef<HTMLInputElement>(null);
  useImperativeHandle(
    ref,
    () => ({
      focus: () => inputRef.current?.focus(),
      clear: () => {
        if (inputRef.current) inputRef.current.value = "";
      },
    }),
    [],
  );
  return (
    <>
      <label>{label}</label>
      <input ref={inputRef} />
    </>
  );
}
```

**Sources:**

[react.dev/reference/react/useImperativeHandle](https://react.dev/reference/react/useImperativeHandle)

ref as prop - 19

</details>

<details>
  <summary>ref as prop - 19</summary>

### ref as prop - 19

**Definition:** In React 19, function components accept `ref` as a **regular prop** — no `forwardRef` wrapper required.

**How it works:** Pass `ref` to a native element or to a child that forwards it. `forwardRef` still works for backward compatibility.

**When to use:** Composite inputs, focusable wrappers, libraries that need imperative handles.

**Examples:**

```tsx
function MyInput({
  ref,
  ...props
}: { ref?: Ref<HTMLInputElement> } & InputHTMLAttributes<HTMLInputElement>) {
  return <input ref={ref} {...props} />;
}
```

**See also:** [useRef](#useref) · [useImperativeHandle](#useimperativehandle)

**Sources:**

[react.dev/blog/2024/12/05/react-19](https://react.dev/blog/2024/12/05/react-19)

useMemo

</details>

### Performance

<details>
  <summary>useMemo</summary>

### useMemo

**Definition:** Caches a **computed value** between renders when dependencies are unchanged.

**How it works:** Deps compared with `Object.is`. Treat the cache as a **hint**, not a store — React may discard it (HMR, suspend). Profile before memoizing expensive calculations.

**When to use:** Proven-slow derived lists; stable object props for `memo` children.

**Examples:**

```tsx
const sorted = useMemo(() => items.slice().sort(compare), [items]);
```

**See also:** [useCallback](#usecallback) · [React Compiler](./4_react_server.md#react-compiler)

**Sources:**

[react.dev/reference/react/useMemo](https://react.dev/reference/react/useMemo)

useCallback

</details>

<details>
  <summary>useCallback</summary>

### useCallback

**Definition:** Caches a **function identity** between renders. `useCallback(fn, deps)` is `useMemo(() => fn, deps)` — React does **not** call `fn`.

**When to use:** Stable handlers for `memo` children; functions returned from custom hooks.

**Examples:**

```tsx
const onSave = useCallback(() => save(user), [user]);
```

**See also:** [useMemo](#usememo) · [useEffect](#useeffect)

**Sources:**

[react.dev/reference/react/useCallback](https://react.dev/reference/react/useCallback)

useTransition

</details>

<details>
  <summary>useTransition</summary>

### useTransition

**Definition:** Marks a state update as **non-urgent** so urgent updates (typing, clicks) stay responsive.

**How it works:** `const [isPending, startTransition] = useTransition()` — wrap expensive `setState` in `startTransition(() => ...)`.

**Examples:**

```tsx
const [isPending, startTransition] = useTransition();
startTransition(() => setResults(filter(query)));
```

**See also:** [useDeferredValue](#usedeferredvalue) · [Concurrent rendering](./4_react_core.md#concurrent-rendering)

**Sources:**

[react.dev/reference/react/useTransition](https://react.dev/reference/react/useTransition)

useDeferredValue

</details>

<details>
  <summary>useDeferredValue</summary>

### useDeferredValue

**Definition:** Returns a **lagging copy** of a value so heavy child renders can defer behind fast parent updates.

**How it works:** Pair with `memo` on the child. Requires `createRoot`.

**Examples:**

```tsx
const deferredQuery = useDeferredValue(query);
<SearchResults query={deferredQuery} />;
```

**See also:** [useTransition](#usetransition)

**Sources:**

[react.dev/reference/react/useDeferredValue](https://react.dev/reference/react/useDeferredValue)

идентификатор использования

</details>

### Other

<details>
  <summary>useId</summary>

### useId

**Definition:** Генерирует **стабильный уникальный идентификатор** для каждого экземпляра компонента, который согласуется между рендерингом сервера и гидратацией клиента.

**How it works:** React назначает идентификаторы, используя внутренний счетчик с префиксом `:r` (e.g. `:r1:`). Один и тот же экземпляр компонента получает один и тот же идентификатор на сервере и клиенте, что позволяет избежать несоответствий гидратации. Идентификаторы уникальны среди одноуровневых элементов в дереве, а не глобально уникальны, как UUID. Вызов `useId()` один раз для каждого логического идентификатора; получить связанные идентификаторы с суффиксами (`${id}-description`).

**When to use:** `htmlFor` / `id` на входах, `aria-labelledby`, `aria-describedby`, связывая сообщения об ошибках с полями.

**Pluses and minuses:**

| +                                                     | −                                                                         |
| ----------------------------------------------------- | ------------------------------------------------------------------------- |
| ССР/безопасность для гидратации — нет `Math.random()` | Не для списка `key` реквизит                                              |
| Метки пар, входные данные, атрибуты ARIA              | Идентификаторы содержат `:` — при необходимости экранируйте селекторы CSS |
| Один вызов перехватчика на группу полей формы         |                                                                           |

**Examples:**

```tsx
function Field({ label }: { label: string }) {
  const id = useId();
  const errorId = `${id}-error`;
  return (
    <>
      <label htmlFor={id}>{label}</label>
      <input id={id} aria-describedby={errorId} />
      <span id={errorId} role="alert">
        Required
      </span>
    </>
  );
}
```

**Thoughts:** **не** используйте `useId` для `key` в списках — ключи должны быть стабильными на протяжении всего существования списка, а не для каждого слота рендеринга. **Не** разрезайте и не хешируйте идентификатор.

**Sources:**

[react.dev/reference/react/useId](https://react.dev/reference/react/useId)

использоватьSyncExternalStore

</details>

<details>
  <summary>useSyncExternalStore</summary>

### useSyncExternalStore

**Definition:** Подписывается на **внешний источник данных** вне состояния React и считывает снимок безопасным способом при **параллельном рендеринге** (без разрывов).

**How it works:** Вы предоставляете три функции: `subscribe(onStoreChange)` регистрирует обратный вызов, который вызывает React при изменении хранилища; `getSnapshot()` возвращает текущее значение (должно быть неизменяемым или кэшированным — та же ссылка, если данные не изменились); `getServerSnapshot()` возвращает значение во время SSR (обязательно, если хранилище отличается на сервере и на клиенте, например `window`). React повторно отображает подписанные компоненты при изменении снимка. Авторы библиотек (Redux, Zustand) используют это внутри себя; код приложения использует перехватчик библиотеки.

**When to use:** Глобальные клиентские магазины (Redux, Zustand), `window.matchMedia`, `navigator.onLine`, стороннее состояние, не связанное с React. Требуется, когда библиотека интегрируется с параллельными функциями React 18+.

**Pluses and minuses:**

| +                                                      | −                                                                         |
| ------------------------------------------------------ | ------------------------------------------------------------------------- |
| Предотвращает разрыв во время одновременных обновлений | Подробный API — предпочитаете хуки Zustand/Redux в приложениях            |
| Официальный шаблон подписки на API браузера            | `getSnapshot` должен быть ссылочно стабильным, когда данные не изменяются |
| Безопасность SSR с `getServerSnapshot`                 | Излишество для простого `useState`                                        |

**Examples:**

```tsx
function useWindowWidth() {
  return useSyncExternalStore(
    (onChange) => {
      window.addEventListener("resize", onChange);
      return () => window.removeEventListener("resize", onChange);
    },
    () => window.innerWidth,
    () => 0, // SSR fallback
  );
}
```

**Thoughts:** `getSnapshot` не должен возвращать новую ссылку на объект/массив при каждом вызове, если данные фактически не изменились — React будет бесконечно перерисовываться. Предпочитайте существующие библиотеки, а не интеграцию собственного магазина.

**Sources:**

[react.dev/reference/react/useSyncExternalStore](https://react.dev/reference/react/useSyncExternalStore)

использоватьDebugValue

</details>

<details>
  <summary>useDebugValue</summary>

### useDebugValue

**Definition:** Добавляет **собственную метку** для настраиваемого перехватчика в React DevTools, видимую при проверке перехватчика в дереве компонентов.

**How it works:** Вызов `useDebugValue(value)` or `useDebugValue(value, formatFn)` внутри специального крючка. React DevTools показывает отформатированное значение рядом с именем хука. Тело хука по-прежнему работает в рабочей среде, но в оптимизированных производственных сборках отображение DevTools удалено — чтобы вычисления значений были дешевыми. Необязательный `formatFn` откладывает дорогостоящее форматирование до тех пор, пока DevTools фактически не проверит перехватчик.

**When to use:** Пользовательские перехватчики, возвращающие непрозрачные объекты — показать `"Online"` / `"Offline"`, количество товаров в корзине, роль авторизации. Пропустите тривиальные однострочные зацепки.

**Pluses and minuses:**

| +                                                                          | −                                                                                             |
| -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| Ускоренная отладка общих перехватчиков (`useAuth`, `useCart`)              | Преимущество только для разработчиков — никакой пользовательской ценности во время выполнения |
| Дополнительный ленивый форматтер позволяет избежать затрат на производство | Работает только внутри пользовательских хуков, а не компонентов                               |
| Краткий обзор состояния документов                                         |                                                                                               |

**Examples:**

```tsx
function useOnlineStatus() {
  const online = useSyncExternalStore(
    subscribe,
    () => navigator.onLine,
    () => true,
  );
  useDebugValue(online ? "Online" : "Offline");
  return online;
}

function useFriendStatus(friendId: string) {
  const status = useFriendStatusImpl(friendId);
  useDebugValue(status, (s) => (s === "online" ? "🟢 Online" : "⚫ Offline"));
  return status;
}
```

**Sources:**

[react.dev/reference/react/useDebugValue](https://react.dev/reference/react/useDebugValue)

useFormStatus - 19

</details>

<details>
  <summary>useFormStatus - 19</summary>

### useFormStatus - 19

**Definition:** `react-dom` hook — read **pending** state of the nearest parent `<form>` from a **child** component.

**How it works:** Must be called inside a descendant of `<form>`. Returns `{ pending, data, method, action }`. `pending` is true while the form's action Promise is in flight.

**When to use:** Reusable `SubmitButton` without prop drilling `isPending`.

**Examples:**

```tsx
function SubmitButton({ children }: { children: React.ReactNode }) {
  const { pending } = useFormStatus();
  return (
    <button type="submit" disabled={pending}>
      {children}
    </button>
  );
}
```

**See also:** [useActionState - 19](#useactionstate-19) · [Actions](./4_react_server.md#actions)

**Sources:**

[react.dev/reference/react-dom/hooks/useFormStatus](https://react.dev/reference/react-dom/hooks/useFormStatus)

Пользовательские крючки

</details>

### Reuse & guidance

<details>
  <summary>Custom hooks</summary>

### Custom hooks

**Definition:** Функция, имя которой начинается с `use` и это **вызывает другие хуки**. Это не функция React, выходящая за рамки соглашения об именах — ESLint использует `use` префикс для обеспечения соблюдения [Правил хуков](#rules-of-hooks).

**How it works:**

Каждый **сайт вызова** получает свое собственное изолированное состояние (два `useOnlineStatus()` звонки → две подписки). Хуки разделяют **логику**, а не одноэлементное хранилище, если только вы не разместите хранилище позади него. `useSyncExternalStore` /контекст специально. Возвращайте запоминаемые обратные вызовы из перехватчиков, которые предоставляют функции, чтобы потребители могли от них зависеть.

Одинаковый `useState` + `useEffect` комбо появляется в 2+ компонентах. **Не** для совместного использования JSX (составляющие компоненты) или для помощников, которые никогда не вызывают перехватчики.

_"Пользовательские перехватчики заменяют большинство HOC для многократного использования. Они не разделяют состояние между компонентами, если я не помещаю общее состояние в контекст или внешнее хранилище"._

**Pluses and minuses:**

| +                                                      | −                                     |
| ------------------------------------------------------ | ------------------------------------- |
| Повторное использование логики без адской оболочки HOC | `use` требуется префикс               |
| Испытайте крючок изолированно                          | Чрезмерная экстракция добавляет хмель |

**Thoughts:**

_Извлекайте поведение, а не разметку. Каждый вызов имеет свое состояние._

**Пример —** `useClickOutside`**:**

```tsx
function useClickOutside<T extends HTMLElement>(
  ref: RefObject<T | null>,
  onOutside: () => void,
) {
  useEffect(() => {
    function handle(e: MouseEvent) {
      if (ref.current && !ref.current.contains(e.target as Node)) {
        onOutside();
      }
    }
    document.addEventListener("mousedown", handle);
    return () => document.removeEventListener("mousedown", handle);
  }, [ref, onOutside]);
}
```

**Sources:**

[react.dev/learn/reusing-logic-with-custom-hooks](https://react.dev/learn/reusing-logic-with-custom-hooks)

Возможно, вам не нужен эффект

</details>

<details>
  <summary>You might not need an effect</summary>

### You might not need an effect

**Definition:** Многие разработчики шаблонов стремятся `useEffect` лучше обрабатываются при рендеринге, обработчиках событий или загрузчиках данных платформы. Эффекты предназначены для **синхронизации с внешними системами**, а не для преобразования данных или реагирования на пользовательские события.

**How it works:**

Спросите: «Нужно ли это запускать _потому что компонент был отображен_ и трогать что-то за пределами React?» Если нет → вероятно, нет эффекта.

| Нужна                                     | Делайте вместо эффекта                                     |
| ----------------------------------------- | ---------------------------------------------------------- |
| Преобразование данных для отображения     | Вычисления во время рендеринга                             |
| Ответ на событие пользователя             | Обработчик событий                                         |
| Кэш дорогой калькулятор                   | `useMemo`                                                  |
| Сбросить все состояние при смене опоры    | `key={id}` на компоненте                                   |
| Уведомить родителя об изменении состояния | Вызов родительского обратного вызова в обработчике событий |
| Получить данные                           | Загрузчик фреймворка, RSC, TanStack Query                  |
| Обновления состояния цепочки              | Один обработчик событий с несколькими `setState`           |

Подписки, таймеры, сторонние виджеты, вход в аналитику, синхронизация `document.title` (или используйте метаданные платформы).

**Examples:**

```tsx
// Bad — effect for derived data
useEffect(() => setFullName(first + " " + last), [first, last]);
// Good — compute during render
const fullName = `${first} ${last}`;
```

**Thoughts:** _Эффекты синхронизируются с внешним миром, а не с вашим собственным состоянием._

**See also:** [useEffect](#useeffect) · [Чистота](./4_react_core.md#purity)

**Sources:**

[react.dev/learn/you-might-not-need-an-effect](https://react.dev/learn/you-might-not-need-an-effect)

---

</details>

## 6. Escape hatches

### Escape hatches

Порталы

<details>
  <summary>Portals</summary>

### Portals

**Definition:** `createPortal(child, domNode)` визуализирует дочерние элементы React в **другой узел DOM**, сохраняя их в том же дереве компонентов React для контекста и всплытия событий.

**How it works:** React по-прежнему управляет дочерними элементами портала в виртуальном дереве — обработчики контекста, состояния и событий работают нормально. DOM-узлы монтируются под `domNode` (часто `document.body`). События перемещаются по **дереву React** (родительские обработчики по-прежнему срабатывают), а не только по дереву DOM. Используйте для пользовательского интерфейса, который должен избегать CSS. `overflow: hidden`, `z-index` стекирование контекстов или `position` ограничения предков.

**When to use:** Модальные диалоги, всплывающие уведомления, плавающие меню, полноэкранные наложения. В сочетании с ловушкой фокусировки и `aria-modal` для доступности.

**Pluses and minuses:**

| +                                                                                              | −                                                                                                                    |
| ---------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| Модальные окна, всплывающие подсказки, раскрывающиеся списки отображаются над страницей Chrome | Нужна выделенная точка монтирования в HTML (`#modal-root`)                                                           |
| Сохраняет контекст React из родителя                                                           | SSR: убедитесь, что цель портала существует, или отображайте значение null до тех пор, пока оно не будет установлено |
| Всплеск событий следует иерархии React                                                         | Управление фокусом и a11y требуют дополнительной работы (ловушка фокуса)                                             |

**Examples:**

```tsx
function Modal({
  children,
  onClose,
}: {
  children: ReactNode;
  onClose: () => void;
}) {
  return createPortal(
    <div role="dialog" aria-modal="true" className="overlay" onClick={onClose}>
      <div className="modal" onClick={(e) => e.stopPropagation()}>
        {children}
      </div>
    </div>,
    document.getElementById("modal-root")!,
  );
}
```

**Thoughts:** Дочерние элементы портала по-прежнему участвуют в согласовании — отключение родительского элемента отключает портал. Для SSR отложите рендеринг портала до тех пор, пока `useEffect` или используйте структуру, которая это обрабатывает.

**Sources:**

[react.dev/reference/react-dom/createPortal](https://react.dev/reference/react-dom/createPortal)

Границы ошибок

</details>

<details>
  <summary>Error boundaries</summary>

### Error boundaries

**Definition:**

Компоненты класса, реализующие `static getDerivedStateFromError` + `componentDidCatch` которые улавливают ошибки рендеринга в дочерних элементах и ​​отображают резервный пользовательский интерфейс.

| Уловы                   | НЕ ловит                             |
| ----------------------- | ------------------------------------ |
| Ошибки рендеринга       | Ошибки обработчика событий           |
| Ошибки жизненного цикла | Асинхронный (`setTimeout`, обещает)  |
| Ошибки конструктора     | Ошибки рендеринга на стороне сервера |

| Ошибки в самой границе |

**How it works:**

1. Дочерний объект выбрасывает во время рендеринга → React приближается к ближайшей границе ошибки
2. `getDerivedStateFromError` возвращает состояние → перерисовка границ с резервным пользовательским интерфейсом
3. `componentDidCatch(error, info)` запускается для регистрации (здесь побочные эффекты в порядке)
4. Братья, сестры и родители **за** границами продолжают работать

**When to use:**

Обертывание сегментов маршрута, рискованные сторонние виджеты, ленивые загружаемые фрагменты. Сопряжение с входом в систему `componentDidCatch`.

Ошибки обработчика событий (используйте `try/catch`), асинхронные ошибки (обработка в обещании), замена пользовательского интерфейса проверки формы.

**Examples:**

```tsx
class ErrorBoundary extends React.Component<
  { fallback: ReactNode; children: ReactNode },
  { hasError: boolean }
> {
  state = { hasError: false };
  static getDerivedStateFromError() {
    return { hasError: true };
  }
  render() {
    return this.state.hasError ? this.props.fallback : this.props.children;
  }
}
```

**Thoughts:**

Эквивалента хука пока нет — используйте `react-error-boundary` библиотека или класс. React 19 не меняет этого.

_Границы ошибок улавливают сбои во время рендеринга в дочерних элементах, а не ошибки событий или асинхронные ошибки._

**Sources:**

[react.dev/reference/react/Component#ching-rendering-errors-with-an-error-boundary](https://react.dev/reference/react/Component#ching-rendering-errors-with-an-error-boundary)

React.lazy + Приостановка

</details>

<details>
  <summary>React.lazy + Suspense</summary>

### React.lazy + Suspense

**Definition:** `lazy(() => import('./Page'))` **разбивает код** компонента — загружается только при первом рендеринге. `<Suspense fallback={...}>` показывает резервный пользовательский интерфейс во время загрузки ленивого фрагмента.

**How it works:**

1. `const Chart = lazy(() => import('./Chart'))` **еще** не загружается. Он регистрирует функцию **load**. React **не** вызывает `load` до тех пор, пока он не **попытается визуализировать** этот компонент в первый раз.
2. `{show && <Chart />}` - пока `show` ложно, `<Chart />` отсутствует в дереве → **нет сети**.
3. Когда `show` становится истинным, React выполняет рендеринг `<Chart />`. Загрузчик работает `import('./Chart')` — сборщик генерирует **отдельный фрагмент JS**; браузер извлекает этот файл.
4. Пока Промис не разрешится, React **выдает** это Промис (тот же канал, что и Приостановка). Ближайший `<Suspense fallback={...}>` ловит и показывает `fallback`.
5. Когда обещание разрешается, React читает `.default` как компонент, **кэширует** и Promise, и модуль — `load` не вызывается повторно — тогда **повторяет** рендеринг с реальным компонентом.
6. Если обещание **отклоняется**, React выдает ошибку до ближайшей **границы ошибки** (не в приостановке).

**When to use:**

Страница настроек, панель администратора, обширная библиотека диаграмм, пакеты для конкретных локали. Фреймворки (Next.js) автоматически разделяются по маршрутам.

#### Common pitfalls

Предполагая `lazy()` извлекается во время оценки модуля — он ожидает **первого рендеринга**. Отсутствует саспенс над ленивым элементом. Потребность в именованном экспорте `.then(m => ({ default: m.Foo }))`.

**Pluses and minuses:**

| +                                                               | −                                                            |
| --------------------------------------------------------------- | ------------------------------------------------------------ |
| Меньший первоначальный пакет, более быстрый TTI                 | Загрузка флэш-памяти, если резервный вариант не предусмотрен |
| Разделение на основе маршрутов является автоматическим шаблоном | Вложенные ленивцы без планирования → водопад                 |
| Интегрируется с Suspense для данных + код                       | Требуется граница ошибок при сбоях в сети                    |

**Examples:**

```tsx
const Settings = lazy(() => import("./Settings"));

function App({ openSettings }: { openSettings: boolean }) {
  return (
    <ErrorBoundary fallback={<p>Failed to load</p>}>
      <Suspense fallback={<Spinner />}>
        {openSettings ? <Settings /> : <Home />}
      </Suspense>
    </ErrorBoundary>
  );
}
```

**Thoughts:**

_Чанк загружается при первом рендеринге ленивого компонента; Приостановка показывает запасной вариант, пока_ `import()` _ожидается; После этого React кэширует модуль._

`lazy` поддерживает только экспорт по умолчанию. Для именованного экспорта: `lazy(() => import('./Foo').then(m => ({ default: m.Foo })))`.

**Sources:**

[react.dev/reference/react/lazy](https://react.dev/reference/react/lazy) · [Приостановка](https://react.dev/reference/react/Suspense)

---

</details>
