# React — Redux

Redux Toolkit, глобальное состояние клиента и RTK Query.

[ ← Вернуться в центр](./4_react.md)

### Related docs

- [React — Core](./4_react_core.md) · [React — Hooks](./4_react_hooks.md) · [React — Components](./4_react_components.md) · [React — APIs](./4_react_apis.md) · [React — React 19](./4_react_server.md) · [React — Ecosystem](./4_react_ecosystem.md) · [React — Redux](./4_react_redux.md) · [React — TanStack](./4_react_tanstack.md) · [React — Интервью](./4_react_interview.md)

### Sections

- [Principles](#principles)
- [Flux and Redux](#flux-and-redux)
- [Immutability](#immutability)
- [Redux Toolkit](#redux-toolkit)
- [Redux vs Redux Toolkit](#redux-vs-redux-toolkit)
- [Optimizations](#optimizations)
- [Async logic](#async-logic)
- [When to use Redux](#when-to-use-redux)
- [State management comparison](#state-management)
- [RTK Query](#rtk-query)

---

## Principles

<details>
  <summary>Redux core principles</summary>

### Redux core principles

**Definition:** Redux — это **паттерн и библиотека** для **предсказуемого управления глобальным состоянием** приложения. UI диспатчит **actions** (простые объекты «что произошло»); **reducers** (чистые функции) вычисляют следующее состояние; единый **store** хранит текущее дерево состояния.

**How it works:**

Три правила, делающие обновления предсказуемыми:

| Rule                       | Meaning                                                                                                       |
| -------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **Single source of truth** | Один объект store хранит всё состояние приложения                                                             |
| **State is read-only**     | Изменить состояние можно только через `dispatch(action)` — компоненты никогда не мутируют store напрямую      |
| **Reducers are pure**      | `(state, action) => newState` — одинаковый вход всегда даёт одинаковый выход; без side effects внутри reducer |

**One-way data flow:**

1. UI читает состояние через **selectors** (или `useSelector`)
2. Событие пользователя → `dispatch(action)` (часто через action creators)
3. Store запускает reducers → сохраняется новое состояние
4. Подписчики (React-Redux) перерисовывают компоненты, у которых изменился выбранный срез

Ключевые термины:

| Term         | Role                                                                       |
| ------------ | -------------------------------------------------------------------------- |
| **Action**   | `{ type: 'domain/event', payload?: … }` — описывает _что произошло_        |
| **Reducer**  | Вычисляет _как_ меняется состояние для данного action                      |
| **Store**    | `configureStore({ reducer })` — хранит state, даёт `dispatch` / `getState` |
| **Dispatch** | Единственный способ инициировать обновление                                |
| **Selector** | `(state) => slice` — читает фрагмент без дублирования lookup-логики        |

Reducers обновляют состояние **иммутабельно** — копия, изменение копии, возврат. См. [Immutability in Redux](#immutability-in-redux). Redux Toolkit использует [Immer](https://immerjs.github.io/immer/): в slice reducers можно писать `state.count++`, а на выходе всё равно immutable update.

**When to use:**

Общее состояние, нужное многим далёким компонентам, с нетривиальной логикой обновления. **Не** для каждого приложения — часто хватает локального `useState`, Context или server-state библиотек.

**Pluses and minuses:**

| +                                             | −                                                       |
| --------------------------------------------- | ------------------------------------------------------- |
| Предсказуемые, тестируемые обновления         | Больше концепций и boilerplate, чем у Context           |
| Time-travel в DevTools                        | Дополнительный слой — actions/reducers между UI и state |
| Чёткое разделение: событие vs изменение state | Избыточно для маленьких приложений с малым shared state |

**Examples:**

```tsx
const increment = () => ({ type: "counter/increment" });

const counterReducer = (state = { value: 0 }, action) => {
  if (action.type === "counter/increment") {
    return { ...state, value: state.value + 1 };
  }
  return state;
};

const store = configureStore({ reducer: counterReducer });
store.dispatch(increment()); // { value: 1 }
```

**Thoughts:** _Redux разделяет «что произошло» (action) и «как меняется state» (reducer) — это и делает большие приложения отлаживаемыми._

**See also:** [Flux vs Redux](#flux-vs-redux) · [Why async is outside reducers](#why-async-is-outside-reducers) · [Redux Toolkit](#redux-toolkit-essentials)

**Sources:**

[redux.js.org — Overview and Concepts](https://redux.js.org/tutorials/essentials/part-1-overview-concepts) · [redux-toolkit.js.org](https://redux-toolkit.js.org/)

Flux vs Redux

</details>

## Flux and Redux

<details>
  <summary>Flux vs Redux</summary>

### Flux vs Redux

**Definition:** **Flux** — **архитектурный паттерн** Facebook для однонаправленного потока данных в React. **Redux** взял идеи Flux, упростил их и добавил строгие правила — особенно **единый store** и **чистые reducers**.

**How it works:**

Оба описывают обновления plain **action** объектами и держат логику мутаций вне UI. Поток один: событие → action → обновление state → re-render.

| | Flux | Redux |
| | ---------------- | ----------------------------------------------- |
| | **Stores** | Несколько stores (по домену) | **Один** глобальный store |
| | **Dispatcher** | Центральный хаб маршрутизирует actions в stores | **Без dispatcher** — `store.dispatch` вызывает reducers напрямую |
| | **Update logic** | Stores могут обновлять себя сами | **Только чистые reducers** — `(state, action) => newState` |
| | **Data shape** | Часто мутирующие обновления | **Обязательная иммутабельность** (копия, не мутация) |
| | **Composition** | Event emitters + много слушателей | Чистые функции — легко тестировать и композировать |

Модель Flux: `(state, action) => state`. Redux сохраняет форму, но убирает слой Dispatcher — reducers это функции, а не event emitters.

Типичный поток Flux:

```
View → Action → Dispatcher → Store(s) → View
```

Поток Redux:

```
View → dispatch(action) → reducer(s) → store state → View
```

Redux также заимствует у **Elm** паттерн updater `(action, state) => state` и строже требует immutability, чем большинство Flux-реализаций.

**When to use:**

Изучайте Flux для legacy-кодовых баз и собеседований. **Для новых приложений — Redux (через Redux Toolkit)**; Flux с нуля сегодня почти не пишут — Redux это эволюция и стандартизация.

**Pluses and minuses:**

| +                                                   | −                                                             |
| --------------------------------------------------- | ------------------------------------------------------------- |
| Flux: простая модель, несколько bounded stores      | Несколько stores → сложнее координировать cross-feature state |
| Redux: один store, DevTools, предсказуемые reducers | Один store может разрастись без slices/normalization          |

**Examples:**

```tsx
const action = { type: "todos/todoAdded", payload: "Buy milk" };

function todosReducer(state = [], action) {
  if (action.type === "todos/todoAdded") {
    return [...state, { text: action.payload, done: false }];
  }
  return state;
}
```

**Thoughts:** _Redux — это «упрощённый Flux»: тот же однонаправленный поток, меньше движущихся частей._

**See also:** [Redux core principles](#redux-core-principles) · [Redux vs Redux Toolkit](#redux-vs-redux-toolkit)

**Sources:**

[redux.js.org — Prior Art: Flux](https://redux.js.org/understanding/history-and-design/prior-art#flux) · [redux.js.org — Overview](https://redux.js.org/tutorials/essentials/part-1-overview-concepts)

Immutability in Redux

</details>

## Immutability

<details>
  <summary>Immutability in Redux</summary>

### Immutability in Redux

**Definition:** **Immutable updates** — вы никогда не меняете существующий state на месте; возвращаете **новый** объект/массив (или Immer создаёт его). Redux **требует** этого, чтобы change detection, DevTools и re-render в React работали корректно.

**How it works:**

Зачем Redux это требует:

| Mechanism            | Why immutability matters                                                                                                         |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `combineReducers`    | Сравнивает старый и новый slice **по ссылке** (`===`). Мутация на месте → та же ссылка → Redux считает, что ничего не изменилось |
| `useSelector`        | Re-render только когда **ссылка** результата selector изменилась. Мутируете state → та же ссылка → UI не обновляется             |
| DevTools time-travel | Воспроизводит чистые шаги `(state, action) => newState` — мутации ломают историю                                                 |
| RTK dev middleware   | `configureStore` в dev бросает ошибку, если reducer мутирует state                                                               |

Shallow vs deep equality:

- Redux использует **shallow** проверки (`a === b`) — быстро
- Deep compare всего дерева на каждый dispatch слишком дорог
- Если мутировать `state.user.name`, но вернуть тот же объект `state`, **подписчики не увидят изменение**

Ручное immutable-обновление:

```tsx
// ❌ Мутирует существующий state
state.todos.push(newTodo);

// ✅ Новый массив верхнего уровня
return {
  ...state,
  todos: [...state.todos, newTodo],
};

// ✅ Вложенность: копировать каждый изменённый уровень
return {
  ...state,
  user: { ...state.user, name: "Ada" },
};
```

С RTK + Immer (рекомендуется):

```tsx
reducers: {
  todoAdded(state, action) {
    state.todos.push(action.payload); // Immer draft → immutable результат
  },
},
```

**Common pitfalls:**

- Spread только верхнего уровня, но мутация вложенных объектов
- Вернуть **новый** объект из reducer, когда ничего не менялось → лишние re-render (Immer возвращает исходный draft, если не трогали)
- Selector с `state.items.filter(...)` на **каждый** вызов → новая ссылка массива на каждый dispatch → постоянные re-render

**When to use:** Всегда в reducers. Предпочитайте RTK `createSlice` + Immer вместо ручных spreads для вложенных данных.

**Pluses and minuses:**

| +                                                  | −                                               |
| -------------------------------------------------- | ----------------------------------------------- |
| Предсказуемые обновления, дешёвый change detection | Многословно без Immer                           |
| Безопаснее рассуждать о потоке данных              | Легко ошибиться с глубокой вложенностью вручную |

**Thoughts:** _Случайная мутация была главным классом багов Redux — RTK частично существует, чтобы это убрать._

**See also:** [Redux performance optimizations](#redux-performance-optimizations) · [Redux Toolkit essentials](#redux-toolkit-essentials)

**Sources:**

[redux.js.org — Immutable Data](https://redux.js.org/faq/immutable-data) · [redux-toolkit.js.org — Writing Reducers with Immer](https://redux-toolkit.js.org/usage/immer-reducers)

Redux Toolkit essentials

</details>

## Redux Toolkit

<details>
  <summary>Redux Toolkit essentials</summary>

### Redux Toolkit essentials

**Definition:** [Redux Toolkit](https://redux-toolkit.js.org/) (RTK) — **официальный рекомендуемый** способ писать Redux-логику: настройка store, slices, async-хелперы и normalized entities с хорошими дефолтами и dev-проверками.

**How it works:**

| API                   | Purpose                                                                                                                                     |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `configureStore`      | Создаёт store — автоматически комбинирует slice reducers, добавляет thunk middleware, DevTools, проверки immutability/serializability в dev |
| `createSlice`         | `name`, `initialState`, case reducers; генерирует action creators и типы                                                                    |
| `createAsyncThunk`    | Async с lifecycle actions `pending` / `fulfilled` / `rejected`                                                                              |
| `createEntityAdapter` | Нормализованная форма `{ ids, entities }` + CRUD-хелперы                                                                                    |
| RTK Query `createApi` | Кэш сервера, loading, invalidation — [TanStack Query vs RTK Query](./4_react_tanstack.md#tanstack-query-vs-rtk-query)                       |

Reducers в `createSlice` работают через Immer — можно писать в «мутирующем» стиле:

```tsx
import {
  configureStore,
  createSlice,
  type PayloadAction,
} from "@reduxjs/toolkit";

const counterSlice = createSlice({
  name: "counter",
  initialState: { value: 0 },
  reducers: {
    incremented(state) {
      state.value += 1;
    },
    addByAmount(state, action: PayloadAction<number>) {
      state.value += action.payload;
    },
  },
});

export const { incremented, addByAmount } = counterSlice.actions;

export const store = configureStore({
  reducer: { counter: counterSlice.reducer },
});
```

**When to use:** Любое новое Redux-приложение — не пишите вручную `switch` reducers и store setup без веской причины.

**Pluses and minuses:**

| +                                      | −                                               |
| -------------------------------------- | ----------------------------------------------- |
| Меньше boilerplate и ошибок            | Всё ещё модель Redux (actions, reducers, store) |
| Thunk + DevTools + проверки из коробки | Размер бандла vs Zustand для крошечных apps     |
| Immer упрощает вложенные обновления    | Нужно освоить API RTK поверх концепций Redux    |

**Thoughts:** _RTK — не другая библиотека, а стандартный toolkit для Redux в 2024+._

**See also:** [Redux vs Redux Toolkit](#redux-vs-redux-toolkit) · [Redux core principles](#redux-core-principles) · [createAsyncThunk](#createasyncthunk)

**Sources:**

[redux-toolkit.js.org](https://redux-toolkit.js.org/) · [Usage Guide — Store Setup](https://redux-toolkit.js.org/usage/usage-guide#store-setup)

Redux vs Redux Toolkit

</details>

## Redux vs Redux Toolkit

<details>
  <summary>Redux vs Redux Toolkit</summary>

### Redux vs Redux Toolkit

**Definition:** **Redux** (пакет `redux`) — небольшой набор примитивов: `createStore`, `combineReducers`, `applyMiddleware`. **Redux Toolkit** (`@reduxjs/toolkit`) оборачивает Redux API, нужные в продакшене. Это всё ещё Redux (один store, actions, reducers), но с меньшим boilerplate и встроенными проверками.

**How it works:**

| Concern      | Redux core (legacy, вручную)               | Redux Toolkit (рекомендуется)                               |
| ------------ | ------------------------------------------ | ----------------------------------------------------------- |
| Store setup  | `createStore` + middleware вручную         | `configureStore` — thunk, DevTools, dev checks по умолчанию |
| Actions      | Константы + action creators на каждую фичу | `createSlice` генерирует creators и `type`                  |
| Reducers     | `switch` + ручные spreads                  | `createSlice` + Immer — безопасно `state.x = y`             |
| Async        | Ручные thunks + loading actions            | `createAsyncThunk` lifecycle                                |
| Server cache | Ручные thunks + reducers                   | RTK Query `createApi`                                       |
| Bugs         | Случайная мутация — главная ошибка Redux   | Dev middleware ловит мутации и non-serializable values      |

Legacy-паттерн:

```tsx
const ADD_TODO = "ADD_TODO";
export const addTodo = (text) => ({ type: ADD_TODO, payload: text });

function todosReducer(state = [], action) {
  switch (action.type) {
    case ADD_TODO:
      return [...state, { text: action.payload, done: false }];
    default:
      return state;
  }
}

const store = createStore(todosReducer, applyMiddleware(thunk));
```

Та же фича с RTK:

```tsx
const todosSlice = createSlice({
  name: "todos",
  initialState: [],
  reducers: {
    todoAdded(state, action) {
      state.push({ text: action.payload, done: false });
    },
  },
});

export const { todoAdded } = todosSlice.actions;
export const store = configureStore({ reducer: { todos: todosSlice.reducer } });
```

**What stays the same:** один store, plain actions, чистые immutable reducers, thunks для side effects, React-Redux, DevTools.

**What RTK adds:** меньше кода, auto action types, Immer, entity helpers, listener middleware, опциональный RTK Query.

**When to use:**

| Situation                             | Choice                                                                      |
| ------------------------------------- | --------------------------------------------------------------------------- |
| **Любой новый Redux-код**             | Redux Toolkit — официальная рекомендация                                    |
| **Существующее app на `createStore`** | Мигрировать store на `configureStore`, reducers на `createSlice` постепенно |
| **Учиться, как работает Redux**       | Читать core concepts, писать с RTK на практике                              |
| **Только raw `redux`**                | Устарело для новых проектов — API реэкспортируется из `@reduxjs/toolkit`    |

**Pluses and minuses:**

| +                                             | −                                            |
| --------------------------------------------- | -------------------------------------------- |
| RTK: меньше файлов и ошибок, лучше TypeScript | Чуть большая зависимость, чем bare `redux`   |
| Core: минимальный, без мнений                 | Многословно; легко случайно мутировать state |

**Thoughts:** _RTK — это способ использовать Redux сегодня, а не конкурирующая библиотека._

**See also:** [Redux Toolkit essentials](#redux-toolkit-essentials) · [Flux vs Redux](#flux-vs-redux)

**Sources:**

[redux-toolkit.js.org — Why RTK is Redux Today](https://redux-toolkit.js.org/introduction/why-rtk-is-redux-today) · [redux.js.org](https://redux.js.org/)

Redux performance optimizations

</details>

## Optimizations

<details>
  <summary>Redux performance optimizations</summary>

### Redux performance optimizations

**Definition:** Производительность Redux — это в основном **меньше работы на dispatch**: храните минимальный state, **вычисляйте** производные значения через мемоизированные **selectors**, избегайте лишних re-render из-за нестабильных ссылок.

**How it works:**

**1. Минимальный state, derive остальное**

Храните только source of truth; filtered lists, totals и флаги — в selectors, не в reducers и не в дублирующих полях state.

```tsx
// ❌ Дублирование derived data в store
{ todos: [...], completedCount: 3, visibleTodos: [...] }

// ✅ Хранить todos; фильтровать в selector
const selectVisibleTodos = (state) =>
  state.todos.filter((t) => !t.isArchived);
```

**2. `useSelector` + reference equality**

React-Redux сравнивает предыдущий и следующий результат selector через `===`. Та же ссылка → без re-render.

| Pattern                                     | Result                                              |
| ------------------------------------------- | --------------------------------------------------- |
| Примитив (`state.counter.value`)            | Re-render только при изменении значения             |
| Стабильная ссылка на slice                  | Re-render при замене объекта slice                  |
| `state.items.filter(...)` inline в selector | **Новый массив каждый dispatch** → re-render всегда |
| Мемоизация `createSelector`                 | Пересчёт только при изменении входов                |

**3. Мемоизированные selectors (`createSelector`)**

RTK реэкспортирует `createSelector` из Reselect — кэширует последний результат, пока входные selectors не изменились:

```tsx
import { createSelector } from "@reduxjs/toolkit";

const selectTodos = (state: RootState) => state.todos;
const selectFilter = (state: RootState) => state.filters.status;

export const selectFilteredTodos = createSelector(
  [selectTodos, selectFilter],
  (todos, status) => todos.filter((t) => t.status === status),
);

const todos = useAppSelector(selectFilteredTodos);
```

`createEntityAdapter.getSelectors()` даёт мемоизированные lookup для нормализованного `{ ids, entities }`.

**4. Привычки на уровне компонентов**

- Дробите selectors — не подписывайтесь на весь store, если нужен только `state.ui.theme`
- Стабильные props для `React.memo` — нестабильные inline functions/objects ломают memo
- Server data → RTK Query / TanStack Query
- Общая настройка re-render в React → [Diagnosing re-renders](./4_react_ecosystem.md#diagnosing-re-renders)

**When to use:** Сначала профилируйте (React DevTools Profiler, Redux DevTools). Добавляйте `createSelector`, когда selector делает нетривиальную работу или возвращает новые объекты/массивы. Нормализуйте большие списки через `createEntityAdapter`.

**Pluses and minuses:**

| +                                                       | −                                            |
| ------------------------------------------------------- | -------------------------------------------- |
| Selectors собирают lookup-логику и убирают дублирование | Over-memo для простого `state.x` — шум       |
| Normalized entities — O(1) обновления по id             | Стоимость нормализации для маленьких списков |

**Thoughts:** _Чаще всего проблема Redux perf — «selector возвращает новую ссылку каждый раз» или «компонент подписан на слишком много state», а не медленные reducers._

**See also:** [Immutability in Redux](#immutability-in-redux) · [Redux Toolkit essentials](#redux-toolkit-essentials) · [When to use Redux](#when-to-use-redux)

**Sources:**

[redux.js.org — Deriving Data with Selectors](https://redux.js.org/usage/deriving-data-selectors) · [react-redux.js.org — useSelector](https://react-redux.js.org/api/hooks#useselector)

createAsyncThunk

</details>

## Async logic

<details>
  <summary>Why async is outside reducers</summary>

### Why async is outside reducers

**Definition:** Обновление Redux **store синхронно** — `dispatch` сразу запускает reducers и возвращается. **Reducers должны оставаться чистыми и синхронными**: без `fetch`, `setTimeout`, случайных значений и других side effects. Async-работа идёт **вне** reducers; результаты приходят обычными actions.

**How it works:**

```
Клик пользователя
  → dispatch(thunk)           // middleware перехватывает
  → thunk выполняет async      // fetch, таймеры и т.д.
  → dispatch(plain actions)    // { type: '…/fulfilled', payload }
  → reducers обновляют state   // sync, pure
  → UI перерисовывается
```

Зачем reducers синхронные:

| Reason               | Detail                                                                                  |
| -------------------- | --------------------------------------------------------------------------------------- |
| **Predictability**   | При `(state, action)` выход всегда один — легко тестировать и воспроизводить в DevTools |
| **Time-travel**      | DevTools перезапускает шаги reducer; async внутри reducer ломает replay                 |
| **Middleware chain** | Thunks, логирование, аналитика — _между_ dispatch и reducer                             |

`configureStore` по умолчанию добавляет **redux-thunk**. **Thunk** — функция `(dispatch, getState) => …`, которая может вызывать async-код и диспатчить несколько actions.

**When to use:** Любой сетевой запрос, debounced save, цепочки dispatch или «прочитать state и решить, что fetch-ить».

**Examples:**

```tsx
const fetchTodos = () => async (dispatch) => {
  dispatch({ type: "todos/loading" });
  const res = await fetch("/api/todos");
  dispatch({ type: "todos/loaded", payload: await res.json() });
};
```

**Thoughts:** _Redux синхронен в ядре; «async Redux» — это async **вокруг** store, не внутри reducers._

**See also:** [Working with async in Redux](#working-with-async-in-redux) · [createAsyncThunk](#createasyncthunk) · [RTK Query](./4_react_tanstack.md#tanstack-query-vs-rtk-query) · [Data fetching](./4_react_ecosystem.md#data-fetching)

**Sources:**

[redux-toolkit.js.org — Async Logic](https://redux-toolkit.js.org/usage/usage-guide#async-logic-and-data-fetching) · [redux.js.org — Reducers must be pure](https://redux.js.org/tutorials/essentials/part-1-overview-concepts#reducers)

createAsyncThunk

</details>

<details>
  <summary>createAsyncThunk</summary>

### createAsyncThunk

**Definition:** Хелпер RTK, оборачивающий promise-returning функцию и автоматически генерирующий action types **`pending`**, **`fulfilled`**, **`rejected`** — не нужно вручную писать loading/error для каждого запроса.

**How it works:**

```tsx
import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";

export const fetchUserById = createAsyncThunk(
  "users/fetchById",
  async (userId: number, { rejectWithValue }) => {
    const res = await fetch(`/api/users/${userId}`);
    if (!res.ok) return rejectWithValue(await res.json());
    return res.json();
  },
);

const usersSlice = createSlice({
  name: "users",
  initialState: {
    entities: [],
    status: "idle" as "idle" | "loading" | "failed",
  },
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(fetchUserById.pending, (state) => {
        state.status = "loading";
      })
      .addCase(fetchUserById.fulfilled, (state, action) => {
        state.status = "idle";
        state.entities.push(action.payload);
      })
      .addCase(fetchUserById.rejected, (state) => {
        state.status = "failed";
      });
  },
});

// В компоненте: dispatch(fetchUserById(42))
```

Payload creator получает `(arg, thunkAPI)` с `dispatch`, `getState`, `signal` (AbortController) и `rejectWithValue` для типизированных ошибок.

| Tool                    | Best for                                                         |
| ----------------------- | ---------------------------------------------------------------- |
| **createAsyncThunk**    | Разовый или slice-owned async (сохранение формы, загрузка по id) |
| **RTK Query**           | CRUD + кэш + invalidation для многих endpoints                   |
| **Listener middleware** | «Когда action A — подождать B, затем dispatch C»                 |

**When to use:** Async, который **не** полноценный server-cache слой — мутации, многошаговые flow, browser APIs. Для стандартного REST/GraphQL кэширования предпочтительнее RTK Query.

**Pluses and minuses:**

| +                                          | −                                                         |
| ------------------------------------------ | --------------------------------------------------------- |
| Стандартный паттерн loading/error/success  | Больше boilerplate, чем RTK Query для простых GET-списков |
| Работает с `extraReducers` в разных slices | Легко дублировать fetch-логику без RTK Query              |

**Thoughts:** _createAsyncThunk — для actions; RTK Query — когда главная задача «fetch, cache, refetch»._

**See also:** [Working with async in Redux](#working-with-async-in-redux) · [Why async is outside reducers](#why-async-is-outside-reducers) · [TanStack Query vs RTK Query](./4_react_tanstack.md#tanstack-query-vs-rtk-query)

**Sources:**

[redux-toolkit.js.org/api/createAsyncThunk](https://redux-toolkit.js.org/api/createAsyncThunk)

Working with async in Redux

</details>

<details>
  <summary>Working with async in Redux</summary>

### Working with async in Redux

**Definition:** Практический гайд по **side effects в Redux**: выбор async-инструмента, dispatch из компонентов, loading/error, чистые reducers. Async всегда в **middleware** (thunks, RTK Query, listeners) — никогда внутри reducers.

**How it works:**

**1. Выбор API**

| Task                                                         | Tool                                    | Why                                           |
| ------------------------------------------------------------ | --------------------------------------- | --------------------------------------------- |
| Загрузка/кэш/refetch server data (REST/GraphQL)              | **RTK Query** `createApi`               | Хуки, кэш, tags, loading из коробки           |
| Один запрос с `pending` / `fulfilled` / `rejected`           | **`createAsyncThunk`**                  | Стандартный lifecycle без ручных action types |
| Цепочки dispatch, `getState`, browser APIs                   | **Manual thunk** или `createAsyncThunk` | Императивный контроль между шагами            |
| Реакция на actions (debounce, «после login — fetch profile») | **`createListenerMiddleware`**          | Side effects вне компонентов; легче sagas     |
| Next.js RSC / route loaders                                  | **Server `fetch`** сначала              | Client cache — только для live-обновлений     |

**2. Полный flow (`createAsyncThunk`)**

```tsx
export const saveTodo = createAsyncThunk(
  "todos/save",
  async (text: string, { rejectWithValue, signal }) => {
    const res = await fetch("/api/todos", {
      method: "POST",
      body: JSON.stringify({ text }),
      signal,
    });
    if (!res.ok) return rejectWithValue("Save failed");
    return res.json();
  },
);

extraReducers: ((builder) => {
  builder
    .addCase(saveTodo.pending, (state) => {
      state.saving = true;
    })
    .addCase(saveTodo.fulfilled, (state, action) => {
      state.saving = false;
      state.items.push(action.payload);
    })
    .addCase(saveTodo.rejected, (state, action) => {
      state.saving = false;
      state.error = action.payload as string;
    });
},
  function AddTodo() {
    const dispatch = useAppDispatch();
    const saving = useAppSelector((s) => s.todos.saving);

    return (
      <button disabled={saving} onClick={() => dispatch(saveTodo("Buy milk"))}>
        {saving ? "Saving…" : "Add"}
      </button>
    );
  });
```

**3. RTK Query (предпочтительно для server cache)**

```tsx
export const todosApi = createApi({
  reducerPath: "todosApi",
  baseQuery: fetchBaseQuery({ baseUrl: "/api" }),
  endpoints: (build) => ({
    getTodos: build.query<Todo[], void>({ query: () => "todos" }),
    addTodo: build.mutation<Todo, string>({
      query: (text) => ({ url: "todos", method: "POST", body: { text } }),
      invalidatesTags: ["Todo"],
    }),
  }),
});

const { data, isLoading } = useGetTodosQuery();
const [addTodo] = useAddTodoMutation();
```

Зарегистрируйте `todosApi.reducer` + `todosApi.middleware` в `configureStore`.

**4. Правила**

- **Не** `await fetch` внутри reducers
- **Не** дублировать server cache в slices, если RTK Query уже владеет данными
- **Используйте** `rejectWithValue` для ошибок API в state
- **Используйте** `signal` из `thunkAPI` для отмены при смене маршрута / unmount
- **Dispatch** thunks на события пользователя или вход на маршрут — избегайте `useEffect` + fetch, если thunk/query уже моделирует запрос

**When to use:** Любой сетевой вызов, отложенное сохранение, многошаговый wizard, cross-slice workflow. Начните с RTK Query для CRUD; `createAsyncThunk` — для кастомных flow.

**Pluses and minuses:**

| +                                                | −                                                     |
| ------------------------------------------------ | ----------------------------------------------------- |
| Один предсказуемый паттерн loading/error/success | Больше файлов, чем `useEffect` + `fetch` в компоненте |
| DevTools показывает каждый async action          | Неверный выбор (thunk vs Query) добавляет boilerplate |

**Thoughts:** _Async Redux = dispatch thunk/query, middleware делает I/O, reducers применяют plain results._

**See also:** [Why async is outside reducers](#why-async-is-outside-reducers) · [createAsyncThunk](#createasyncthunk) · [TanStack Query vs RTK Query](./4_react_tanstack.md#tanstack-query-vs-rtk-query) · [Data fetching](./4_react_ecosystem.md#data-fetching)

**Sources:**

[redux-toolkit.js.org — Async Logic and Data Fetching](https://redux-toolkit.js.org/usage/usage-guide#async-logic-and-data-fetching) · [redux.js.org — Side Effects](https://redux.js.org/tutorials/essentials/part-5-async-logic)

</details>

## When to use Redux

<details>
  <summary>When to use Redux</summary>

### When to use Redux

**Definition:** Когда глобальный Redux state оправдывает сложность vs более лёгкие инструменты (`useState`, Context, Zustand, TanStack Query).

**How it works:**

Redux уместен, когда **несколько** условий выполняются:

| Signal                   | Example                                                             |
| ------------------------ | ------------------------------------------------------------------- |
| **Large shared state**   | Корзина, auth session, многошаговый wizard на многих маршрутах      |
| **Frequent updates**     | Live dashboard, collaborative editor, real-time счётчики            |
| **Complex update rules** | Нормализованные entities, cross-slice invalidation, middleware      |
| **Team scale**           | Много разработчиков, общие паттерны, DevTools, тестируемые reducers |

| State type                                    | Prefer                                                         |
| --------------------------------------------- | -------------------------------------------------------------- |
| Server/async JSON                             | TanStack Query или **RTK Query** (не ручной `useEffect` fetch) |
| Local UI (один компонент)                     | `useState` / `useReducer`                                      |
| Редкие globals (theme, locale)                | Context                                                        |
| Средний shared client state, минимум ceremony | Zustand                                                        |
| Большой client state + DevTools + структура   | **Redux Toolkit**                                              |

**When to skip:**

- В основном server data с малым client-owned state → Query + local state
- Прототип или маленькое app → Context или Zustand
- API cache в Redux **без** RTK Query → обычно неверный выбор

**Pluses and minuses:**

| +                                                    | −                                  |
| ---------------------------------------------------- | ---------------------------------- |
| Одно место для inspect и debug client state          | Кривая обучения и структура файлов |
| Проверено в масштабе (middleware, entities, codegen) | Медленнее старт, чем Zustand       |

**Thoughts:** _«You Might Not Need Redux» по-прежнему верно — берите, когда боль от shared state реальна, а не заранее._

**See also:** [State management libraries](#state-management-libraries) · [Redux core principles](#redux-core-principles)

**Sources:**

[redux.js.org — When Should I Use Redux?](https://redux.js.org/tutorials/essentials/part-1-overview-concepts#when-should-i-use-redux) · [redux-toolkit.js.org](https://redux-toolkit.js.org/)

State management libraries

</details>

## State management

<details>
  <summary>State management libraries</summary>

### State management libraries

**Definition:** Краткое сравнение инструментов состояния в React — Redux Toolkit для **shared client state**; кэш сервера — отдельная задача.

| Library                  | Best for                                                                |
| ------------------------ | ----------------------------------------------------------------------- |
| **Redux Toolkit**        | Большие команды, DevTools, normalized entities, RTK Query в одном store |
| **Zustand**              | Минимум boilerplate без ceremony actions/reducers                       |
| **Jotai / Recoil**       | Мелкозернистые atomic subscriptions                                     |
| **TanStack Query**       | Server/async state — автономный кэш                                     |
| **RTK Query**            | Server cache внутри Redux store                                         |
| **Context + useReducer** | Маленькие apps, theme/locale                                            |

| | Flux | Redux |
| | ------- | --------------------------- |
| | Stores | Несколько | Одно хранилище |
| | Updates | Stores обновляют себя | Только чистые reducers |
| | Flow | Action → Dispatcher → Store | Action → Reducer → Store |

**When to use:** Полный гайд — [When to use Redux](#when-to-use-redux).

**Examples:**

```tsx
// Redux Toolkit — см. createSlice выше
import { create } from "zustand";
const useStore = create((set) => ({
  count: 0,
  inc: () => set((s) => ({ count: s.count + 1 })),
}));
```

**Thoughts:** _Server state и UI state — разные задачи — не кладите fetch cache в Redux без нужды в RTK Query._

**See also:** [TanStack Query vs RTK Query](./4_react_tanstack.md#tanstack-query-vs-rtk-query) · [Data fetching](./4_react_ecosystem.md#data-fetching)

**Sources:**

[redux-toolkit.js.org](https://redux-toolkit.js.org/)

TanStack Query vs RTK Query

</details>

## RTK Query

<details>
  <summary>TanStack Query vs RTK Query</summary>

### TanStack Query vs RTK Query

**Definition:** Оба кэшируют **server state** в памяти. TanStack Query — автономный; RTK Query живёт в Redux store через `createApi`.

Каноническое сравнение → [React — TanStack](./4_react_tanstack.md#tanstack-query-vs-rtk-query).

**When to use RTK Query:** Приложение уже на Redux Toolkit — один store, DevTools, `invalidatesTags`, generated hooks из endpoints.

**See also:** [State management libraries](#state-management-libraries) · [createAsyncThunk](#createasyncthunk) · [Working with async in Redux](#working-with-async-in-redux)

**Sources:**

[redux-toolkit.js.org/rtk-query](https://redux-toolkit.js.org/rtk-query/overview) · [tanstack.com/query](https://tanstack.com/query/latest)

</details>
