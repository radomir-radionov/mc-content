# React — Redux

Redux Toolkit, global client state, and RTK Query.

[← Back to hub](./4_react.md)

### Related docs

- [React — Core](./4_react_core.md) · [React — Hooks](./4_react_hooks.md) · [React — Components](./4_react_components.md) · [React — APIs](./4_react_apis.md) · [React — React 19](./4_react_server.md) · [React — Ecosystem](./4_react_ecosystem.md) · [React — Redux](./4_react_redux.md) · [React — TanStack](./4_react_tanstack.md) · [React — Interview](./4_react_interview.md)

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

**Definition:** Redux is a **pattern and library** for managing **global application state** in a predictable way. The UI dispatches **actions** (plain objects describing what happened); **reducers** (pure functions) compute the next state; a single **store** holds the current state tree.

**How it works:**

Three rules that make updates predictable:

| Rule                       | Meaning                                                                                                        |
| -------------------------- | -------------------------------------------------------------------------------------------------------------- |
| **Single source of truth** | One store object holds the entire app state                                                                    |
| **State is read-only**     | The only way to change state is `dispatch(action)` — components never mutate the store directly                |
| **Reducers are pure**      | `(state, action) => newState` — same inputs always produce the same output; no side effects inside the reducer |

**One-way data flow:**

1. UI reads state via **selectors** (or `useSelector`)
2. User event → `dispatch(action)` (often via action creators)
3. Store runs reducers → new state saved
4. Subscribers (React-Redux) re-render components whose selected state changed

Core terms:

| Term         | Role                                                                         |
| ------------ | ---------------------------------------------------------------------------- |
| **Action**   | `{ type: 'domain/event', payload?: … }` — describes _what happened_          |
| **Reducer**  | Computes _how_ state changes for a given action                              |
| **Store**    | `configureStore({ reducer })` — holds state, exposes `dispatch` / `getState` |
| **Dispatch** | Only way to trigger a state update                                           |
| **Selector** | `(state) => slice` — reads one piece of state without repeating lookup logic |

Reducers must update **immutably** — copy existing state, change the copy, return it. See [Immutability in Redux](#immutability-in-redux). Redux Toolkit uses [Immer](https://immerjs.github.io/immer/) so slice reducers can write `state.count++` while still producing immutable updates.

**When to use:**

Shared state that many distant components need, with non-trivial update logic. **Not** for every app — local `useState`, Context, or server-state libraries often suffice.

**Pluses and minuses:**

| +                                       | −                                                   |
| --------------------------------------- | --------------------------------------------------- |
| Predictable, testable updates           | More concepts and boilerplate than Context          |
| DevTools time-travel debugging          | Indirection — actions/reducers between UI and state |
| Clear separation: event vs state change | Overkill for small apps with little shared state    |

**Examples:**

```tsx
// Action creator
const increment = () => ({ type: "counter/increment" });

// Reducer
const counterReducer = (state = { value: 0 }, action) => {
  if (action.type === "counter/increment") {
    return { ...state, value: state.value + 1 };
  }
  return state;
};

// Store + dispatch
const store = configureStore({ reducer: counterReducer });
store.dispatch(increment()); // { value: 1 }
```

**Thoughts:** _Redux separates "what happened" (action) from "how state changes" (reducer) — that split is what makes large apps debuggable._

**See also:** [Flux vs Redux](#flux-vs-redux) · [Why async is outside reducers](#why-async-is-outside-reducers) · [Redux Toolkit](#redux-toolkit-essentials)

**Sources:**

[redux.js.org — Overview and Concepts](https://redux.js.org/tutorials/essentials/part-1-overview-concepts) · [redux-toolkit.js.org](https://redux-toolkit.js.org/)

Flux vs Redux

</details>

## Flux and Redux

<details>
  <summary>Flux vs Redux</summary>

### Flux vs Redux

**Definition:** **Flux** is Facebook's **architecture pattern** for unidirectional data flow in React apps. **Redux** took Flux's core ideas, simplified them, and added stricter rules — especially a **single store** and **pure reducers**.

**How it works:**

Both describe updates as plain **action** objects and keep mutation logic out of UI code. The flow is the same idea: event → action → update state → re-render.

|                  | Flux                                      | Redux                                                        |
| ---------------- | ----------------------------------------- | ------------------------------------------------------------ |
| **Stores**       | Multiple stores (one per domain)          | **One** global store                                         |
| **Dispatcher**   | Central hub routes every action to stores | **No dispatcher** — `store.dispatch` calls reducers directly |
| **Update logic** | Stores can update themselves              | **Pure reducers only** — `(state, action) => newState`       |
| **Data shape**   | Mutable updates were common               | **Immutable updates required** (copy, don't mutate)          |
| **Composition**  | Event emitters + multiple listeners       | Pure functions — easy to test and compose                    |

Flux mental model: `(state, action) => state`. Redux keeps that shape but drops the Dispatcher layer because reducers are plain functions, not event emitters.

Typical Flux flow:

```
View → Action → Dispatcher → Store(s) → View
```

Redux flow:

```
View → dispatch(action) → reducer(s) → store state → View
```

Redux also borrows from **Elm**'s updater pattern `(action, state) => state` and enforces immutability more strictly than most Flux implementations.

**When to use:**

Learn Flux to read legacy codebases and interviews. **Use Redux (via Redux Toolkit) for new apps** — Flux itself is rarely implemented from scratch today; Redux is the evolved, standardized version.

**Pluses and minuses:**

| +                                                  | −                                                          |
| -------------------------------------------------- | ---------------------------------------------------------- |
| Flux: simple mental model, multiple bounded stores | Multiple stores → harder to coordinate cross-feature state |
| Redux: one store, DevTools, predictable reducers   | Single store can grow large without slices/normalization   |

**Examples:**

```tsx
// Flux-style action (both use this)
const action = { type: "todos/todoAdded", payload: "Buy milk" };

// Redux: one reducer handles one slice of the single store
function todosReducer(state = [], action) {
  if (action.type === "todos/todoAdded") {
    return [...state, { text: action.payload, done: false }];
  }
  return state;
}
```

**Thoughts:** _Redux is "Flux simplified" — same unidirectional flow, fewer moving parts._

**See also:** [Redux core principles](#redux-core-principles) · [Redux vs Redux Toolkit](#redux-vs-redux-toolkit)

**Sources:**

[redux.js.org — Prior Art: Flux](https://redux.js.org/understanding/history-and-design/prior-art#flux) · [redux.js.org — Overview](https://redux.js.org/tutorials/essentials/part-1-overview-concepts)

Immutability in Redux

</details>

## Immutability

<details>
  <summary>Immutability in Redux</summary>

### Immutability in Redux

**Definition:** **Immutable updates** mean you never change existing state in place — you return a **new** object/array (or let Immer produce one). Redux **requires** this so change detection, DevTools, and React re-renders work correctly.

**How it works:**

Why Redux cares:

| Mechanism            | Why immutability matters                                                                                              |
| -------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `combineReducers`    | Compares old vs new slice **by reference** (`===`). Mutate in place → same reference → Redux thinks nothing changed   |
| `useSelector`        | Re-renders only when selector result **reference** changes. Mutate state → selector returns same reference → UI stuck |
| DevTools time-travel | Replays pure `(state, action) => newState` steps — mutations break history                                            |
| RTK dev middleware   | `configureStore` throws in dev if a reducer mutates state accidentally                                                |

Shallow vs deep equality:

- Redux uses **shallow** checks (`a === b`) — fast
- Deep compare of entire trees would be too expensive on every dispatch
- Therefore: if you mutate `state.user.name` but return the same `state` object, **no subscriber sees the update**

Hand-written immutable update:

```tsx
// ❌ Mutates existing state
state.todos.push(newTodo);

// ✅ New top-level array, new object inside
return {
  ...state,
  todos: [...state.todos, newTodo],
};

// ✅ Nested: copy every level that changes
return {
  ...state,
  user: { ...state.user, name: "Ada" },
};
```

With RTK + Immer (recommended):

```tsx
reducers: {
  todoAdded(state, action) {
    state.todos.push(action.payload); // Immer drafts → immutable result
  },
},
```

**Common pitfalls:**

- Spread only the top level but mutate nested objects
- Return a **new** object from reducer when nothing changed → unnecessary re-renders (Immer returns original draft when untouched)
- Selector that does `state.items.filter(...)` **every call** → new array reference every dispatch → constant re-renders

**When to use:** Always in reducers. Prefer RTK `createSlice` + Immer over manual spreads for nested data.

**Pluses and minuses:**

| +                                           | −                                           |
| ------------------------------------------- | ------------------------------------------- |
| Predictable updates, cheap change detection | Verbose without Immer                       |
| Safer reasoning about data flow             | Easy to get wrong with deep nesting by hand |

**Thoughts:** _Accidental mutation was the #1 Redux bug class — RTK exists partly to eliminate it._

**See also:** [Redux performance optimizations](#redux-performance-optimizations) · [Redux Toolkit essentials](#redux-toolkit-essentials)

**Sources:**

[redux.js.org — Immutable Data](https://redux.js.org/faq/immutable-data) · [redux-toolkit.js.org — Writing Reducers with Immer](https://redux-toolkit.js.org/usage/immer-reducers)

Redux Toolkit essentials

</details>

## Redux Toolkit

<details>
  <summary>Redux Toolkit essentials</summary>

### Redux Toolkit essentials

**Definition:** [Redux Toolkit](https://redux-toolkit.js.org/) (RTK) is the **official, recommended** way to write Redux logic — store setup, slices, async helpers, and normalized entity utilities with good defaults and dev checks built in.

**How it works:**

| API                   | Purpose                                                                                                                       |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `configureStore`      | Creates the store — auto-combines slice reducers, adds thunk middleware, DevTools, immutability/serializability checks in dev |
| `createSlice`         | Defines `name`, `initialState`, and case reducers; generates action creators + action types                                   |
| `createAsyncThunk`    | Async work with `pending` / `fulfilled` / `rejected` lifecycle actions                                                        |
| `createEntityAdapter` | Normalized `{ ids, entities }` shape + CRUD helpers for lists                                                                 |
| RTK Query `createApi` | Server cache, loading flags, invalidation — [TanStack Query vs RTK Query](./4_react_tanstack.md#tanstack-query-vs-rtk-query)  |

`createSlice` reducers run inside Immer — you can write "mutating" style safely:

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
      state.value += 1; // Immer produces immutable update
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

**When to use:** Any new Redux app — do not hand-write `switch` reducers and manual store setup unless you have a specific reason.

**Pluses and minuses:**

| +                                             | −                                                   |
| --------------------------------------------- | --------------------------------------------------- |
| Less boilerplate, fewer mistakes              | Still Redux mental model (actions, reducers, store) |
| Thunk + DevTools + checks included by default | Bundle size vs Zustand for tiny apps                |
| Immer simplifies nested updates               | Learning RTK APIs on top of Redux concepts          |

**Thoughts:** _RTK is not a different library — it is the standard toolkit for Redux in 2024+._

**See also:** [Redux vs Redux Toolkit](#redux-vs-redux-toolkit) · [Redux core principles](#redux-core-principles) · [createAsyncThunk](#createasyncthunk)

**Sources:**

[redux-toolkit.js.org](https://redux-toolkit.js.org/) · [Usage Guide — Store Setup](https://redux-toolkit.js.org/usage/usage-guide#store-setup)

Redux vs Redux Toolkit

</details>

## Redux vs Redux Toolkit

<details>
  <summary>Redux vs Redux Toolkit</summary>

### Redux vs Redux Toolkit

**Definition:** **Redux** (core `redux` package) is a small set of primitives: `createStore`, `combineReducers`, `applyMiddleware`. **Redux Toolkit** (`@reduxjs/toolkit`) wraps Redux with the APIs you actually need in production — it is still Redux (one store, actions, reducers), with less boilerplate and built-in safety checks.

**How it works:**

| Concern      | Redux core (legacy hand-written)                                        | Redux Toolkit (recommended)                                            |
| ------------ | ----------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Store setup  | `createStore(rootReducer, middleware)` — wire thunk + DevTools yourself | `configureStore({ reducer })` — thunk, DevTools, dev checks by default |
| Actions      | Hand-written constants + action creators per feature                    | `createSlice` generates creators and `type` strings                    |
| Reducers     | `switch (action.type)` + manual spreads                                 | `createSlice` reducers with Immer — `state.x = y` safely               |
| Async        | Manual thunks + loading actions                                         | `createAsyncThunk` lifecycle actions                                   |
| Server cache | Manual thunks + reducers                                                | RTK Query `createApi`                                                  |
| Bugs         | Accidental mutation was #1 Redux bug                                    | Dev middleware catches mutations + non-serializable values             |

Legacy hand-written pattern:

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

Same feature with RTK:

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

**What stays the same:** single store, dispatched plain actions, pure immutable reducers, thunks for side effects, React-Redux `useSelector` / `useDispatch`, DevTools.

**What RTK adds:** less code, auto action types, Immer updates, normalized entity helpers, listener middleware, optional RTK Query.

**When to use:**

| Situation                         | Choice                                                                          |
| --------------------------------- | ------------------------------------------------------------------------------- |
| **Any new Redux code**            | Redux Toolkit — official recommendation                                         |
| **Existing app on `createStore`** | Migrate store to `configureStore`, reducers to `createSlice` incrementally      |
| **Learning how Redux works**      | Read core concepts, then write with RTK in practice                             |
| **Raw `redux` only**              | Obsolete for new projects — APIs are re-exported from `@reduxjs/toolkit` anyway |

**Pluses and minuses:**

| +                                                   | −                                            |
| --------------------------------------------------- | -------------------------------------------- |
| RTK: fewer files, fewer mistakes, better TypeScript | Slightly larger dependency than bare `redux` |
| Core: minimal, unopinionated                        | Verbose; easy to mutate state by mistake     |

**Thoughts:** _RTK is how you use Redux today — not a competing library._

**See also:** [Redux Toolkit essentials](#redux-toolkit-essentials) · [Flux vs Redux](#flux-vs-redux)

**Sources:**

[redux-toolkit.js.org — Why RTK is Redux Today](https://redux-toolkit.js.org/introduction/why-rtk-is-redux-today) · [redux.js.org](https://redux.js.org/)

Redux performance optimizations

</details>

## Optimizations

<details>
  <summary>Redux performance optimizations</summary>

### Redux performance optimizations

**Definition:** Redux performance is mostly about **doing less work per dispatch**: keep stored state minimal, **derive** computed values with memoized **selectors**, and avoid unnecessary React re-renders from unstable references.

**How it works:**

**1. Minimal state, derive the rest**

Store the source of truth only; compute filtered lists, totals, and flags in selectors — not in reducers or extra state fields.

```tsx
// ❌ Duplicate derived data in store
{ todos: [...], completedCount: 3, visibleTodos: [...] }

// ✅ Store todos; derive in selector
const selectVisibleTodos = (state) =>
  state.todos.filter((t) => !t.isArchived);
```

**2. `useSelector` + reference equality**

React-Redux compares the **previous** and **next** selector result with `===`. Same reference → skip re-render.

| Pattern                                      | Result                                              |
| -------------------------------------------- | --------------------------------------------------- |
| Select primitive (`state.counter.value`)     | Re-renders only when value changes                  |
| Select stable slice reference                | Re-renders when that slice object is replaced       |
| `state.items.filter(...)` inline in selector | **New array every dispatch** → re-render every time |
| `createSelector` memoization                 | Recomputes only when inputs change                  |

**3. Memoized selectors (`createSelector`)**

RTK re-exports Reselect's `createSelector` — caches the last result until input selectors change:

```tsx
import { createSelector } from "@reduxjs/toolkit";

const selectTodos = (state: RootState) => state.todos;
const selectFilter = (state: RootState) => state.filters.status;

export const selectFilteredTodos = createSelector(
  [selectTodos, selectFilter],
  (todos, status) => todos.filter((t) => t.status === status),
);

// Component
const todos = useAppSelector(selectFilteredTodos);
```

`createEntityAdapter.getSelectors()` generates memoized lookups for normalized `{ ids, entities }` stores.

**4. Component-level habits**

- Split selectors — don't subscribe the whole store if you only need `state.ui.theme`
- Pass stable props to memoized children (`React.memo`) — unstable inline functions/objects defeat memo
- Server data → RTK Query / TanStack Query (avoids manual cache + refetch logic in Redux)
- General React re-render tuning → [Diagnosing re-renders](./4_react_ecosystem.md#diagnosing-re-renders)

**When to use:** Profile first (React DevTools Profiler, Redux DevTools). Add `createSelector` when selectors do non-trivial work or return new objects/arrays. Normalize large lists with `createEntityAdapter`.

**Pluses and minuses:**

| +                                                      | −                                                  |
| ------------------------------------------------------ | -------------------------------------------------- |
| Selectors colocate lookup logic and cut duplicate code | Over-memoizing simple `state.x` lookups adds noise |
| Normalized entities make updates O(1) by id            | Normalization upfront cost for small lists         |

**Thoughts:** _Most Redux perf issues are "selector returns new reference every time" or "component subscribes to too much state" — not slow reducers._

**See also:** [Immutability in Redux](#immutability-in-redux) · [Redux Toolkit essentials](#redux-toolkit-essentials) · [When to use Redux](#when-to-use-redux)

**Sources:**

[redux.js.org — Deriving Data with Selectors](https://redux.js.org/usage/deriving-data-selectors) · [react-redux.js.org — useSelector](https://react-redux.js.org/api/hooks#useselector)

createAsyncThunk

</details>

## Async logic

<details>
  <summary>Why async is outside reducers</summary>

### Why async is outside reducers

**Definition:** A Redux **store update is synchronous** — `dispatch` runs reducers immediately and returns. **Reducers must stay pure and sync**: no `fetch`, `setTimeout`, random values, or other side effects. Async work happens **outside** reducers; results arrive as plain actions.

**How it works:**

```
User click
  → dispatch(thunk)           // middleware intercepts
  → thunk runs async work       // fetch, timers, etc.
  → dispatch(plain actions)     // { type: '…/fulfilled', payload }
  → reducers update state       // sync, pure
  → UI re-renders
```

Why keep reducers sync:

| Reason               | Detail                                                                                   |
| -------------------- | ---------------------------------------------------------------------------------------- |
| **Predictability**   | Given `(state, action)`, output is always the same — easy to test and replay in DevTools |
| **Time-travel**      | DevTools can re-run reducer steps; async inside reducers breaks replay                   |
| **Middleware chain** | Thunks, logging, analytics sit _between_ dispatch and reducer                            |

`configureStore` adds **redux-thunk** by default. A **thunk** is a function `(dispatch, getState) => …` that can call async code and dispatch multiple actions.

**When to use:** Any network call, debounced save, chained dispatches, or "read state then decide what to fetch" logic.

**Examples:**

```tsx
// Manual thunk (works, but prefer createAsyncThunk for request lifecycle)
const fetchTodos = () => async (dispatch) => {
  dispatch({ type: "todos/loading" });
  const res = await fetch("/api/todos");
  dispatch({ type: "todos/loaded", payload: await res.json() });
};
```

**Thoughts:** _Redux is sync at the core; "async Redux" means async **around** the store, not inside reducers._

**See also:** [Working with async in Redux](#working-with-async-in-redux) · [createAsyncThunk](#createasyncthunk) · [TanStack Query vs RTK Query](./4_react_tanstack.md#tanstack-query-vs-rtk-query) · [Data fetching](./4_react_ecosystem.md#data-fetching)

**Sources:**

[redux-toolkit.js.org — Async Logic](https://redux-toolkit.js.org/usage/usage-guide#async-logic-and-data-fetching) · [redux.js.org — Reducers must be pure](https://redux.js.org/tutorials/essentials/part-1-overview-concepts#reducers)

createAsyncThunk

</details>

<details>
  <summary>createAsyncThunk</summary>

### createAsyncThunk

**Definition:** RTK helper that wraps a promise-returning function and auto-generates **`pending`**, **`fulfilled`**, and **`rejected`** action types — so you don't hand-write loading/error flags for every request.

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

// In a component: dispatch(fetchUserById(42))
```

Payload creator receives `(arg, thunkAPI)` with `dispatch`, `getState`, `signal` (AbortController), and `rejectWithValue` for typed errors.

| Tool                    | Best for                                                     |
| ----------------------- | ------------------------------------------------------------ |
| **createAsyncThunk**    | One-off or slice-owned async (save form, load detail by id)  |
| **RTK Query**           | CRUD + cache + invalidation across many endpoints            |
| **Listener middleware** | "When action A fires, wait for B, then dispatch C" workflows |

**When to use:** Async logic that is **not** a full server-cache layer — mutations, multi-step flows, browser APIs. For standard REST/GraphQL caching prefer RTK Query.

**Pluses and minuses:**

| +                                        | −                                                    |
| ---------------------------------------- | ---------------------------------------------------- |
| Standard loading/error/success pattern   | More boilerplate than RTK Query for simple GET lists |
| Works with `extraReducers` across slices | Easy to duplicate fetch logic without RTK Query      |

**Thoughts:** _Use createAsyncThunk for actions; use RTK Query when the main job is "fetch, cache, refetch"._

**See also:** [Working with async in Redux](#working-with-async-in-redux) · [Why async is outside reducers](#why-async-is-outside-reducers) · [TanStack Query vs RTK Query](./4_react_tanstack.md#tanstack-query-vs-rtk-query)

**Sources:**

[redux-toolkit.js.org/api/createAsyncThunk](https://redux-toolkit.js.org/api/createAsyncThunk)

Working with async in Redux

</details>

<details>
  <summary>Working with async in Redux</summary>

### Working with async in Redux

**Definition:** Practical guide for **side effects in Redux**: pick the right async tool, dispatch from components, handle loading/error, and keep reducers pure. Async always lives in **middleware** (thunks, RTK Query, listeners) — never inside reducers.

**How it works:**

**1. Choose the right API**

| Task                                                      | Tool                                   | Why                                                  |
| --------------------------------------------------------- | -------------------------------------- | ---------------------------------------------------- |
| Load/cache/refetch server data (REST/GraphQL)             | **RTK Query** `createApi`              | Hooks, cache, tags, loading flags built in           |
| One request with `pending` / `fulfilled` / `rejected`     | **`createAsyncThunk`**                 | Standard lifecycle without hand-written action types |
| Chain dispatches, read `getState`, browser APIs           | **Manual thunk** or `createAsyncThunk` | Imperative control between async steps               |
| React to actions (debounce, "after login, fetch profile") | **`createListenerMiddleware`**         | Side effects outside components; lighter than sagas  |
| Next.js RSC / route loaders                               | **Server `fetch`** first               | Add client cache only for live updates               |

**2. End-to-end flow (`createAsyncThunk`)**

```tsx
// 1) Define thunk in slice file
export const saveTodo = createAsyncThunk(
  "todos/save",
  async (text: string, { rejectWithValue, signal }) => {
    const res = await fetch("/api/todos", {
      method: "POST",
      body: JSON.stringify({ text }),
      signal, // aborts if dispatch is cancelled
    });
    if (!res.ok) return rejectWithValue("Save failed");
    return res.json();
  },
);

// 2) Handle lifecycle in extraReducers
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
  // 3) Dispatch from component (not useEffect + fetch)
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

**3. RTK Query (preferred for server cache)**

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

// Component — no manual loading state in slice
const { data, isLoading } = useGetTodosQuery();
const [addTodo] = useAddTodoMutation();
```

Register `todosApi.reducer` + `todosApi.middleware` in `configureStore`.

**4. Rules of thumb**

- **Don't** `await fetch` inside reducers
- **Don't** duplicate server cache in slices if RTK Query already owns it
- **Do** use `rejectWithValue` for API errors you want in state
- **Do** use `signal` from `thunkAPI` for cancellation on route change / unmount
- **Do** dispatch thunks on user events or route enter — avoid `useEffect` + fetch when a thunk/query already models the request

**When to use:** Any network call, delayed save, multi-step wizard, or cross-slice workflow. Start with RTK Query for CRUD; drop to `createAsyncThunk` for custom flows.

**Pluses and minuses:**

| +                                                 | −                                                    |
| ------------------------------------------------- | ---------------------------------------------------- |
| One predictable pattern for loading/error/success | More files than `useEffect` + `fetch` in a component |
| DevTools shows every async action                 | Wrong tool (thunk vs Query) adds boilerplate         |

**Thoughts:** _Async Redux = dispatch a thunk/query, let middleware do I/O, let reducers apply plain results._

**See also:** [Why async is outside reducers](#why-async-is-outside-reducers) · [createAsyncThunk](#createasyncthunk) · [TanStack Query vs RTK Query](./4_react_tanstack.md#tanstack-query-vs-rtk-query) · [Data fetching](./4_react_ecosystem.md#data-fetching)

**Sources:**

[redux-toolkit.js.org — Async Logic and Data Fetching](https://redux-toolkit.js.org/usage/usage-guide#async-logic-and-data-fetching) · [redux.js.org — Side Effects](https://redux.js.org/tutorials/essentials/part-5-async-logic)

</details>

## When to use Redux

<details>
  <summary>When to use Redux</summary>

### When to use Redux

**Definition:** Decision guide for when global Redux state earns its complexity vs lighter tools (`useState`, Context, Zustand, TanStack Query).

**How it works:**

Redux fits best when **several** of these are true:

| Signal                   | Example                                                                 |
| ------------------------ | ----------------------------------------------------------------------- |
| **Large shared state**   | Cart, auth session, multi-step wizard used across many routes           |
| **Frequent updates**     | Live dashboard, collaborative editor, real-time counters                |
| **Complex update rules** | Normalized entities, cross-slice invalidation, middleware pipelines     |
| **Team scale**           | Many developers need the same patterns, DevTools, and testable reducers |

| State type                                       | Prefer                                                         |
| ------------------------------------------------ | -------------------------------------------------------------- |
| Server/async JSON                                | TanStack Query or **RTK Query** (not manual `useEffect` fetch) |
| Local UI (one component)                         | `useState` / `useReducer`                                      |
| Rare globals (theme, locale)                     | Context                                                        |
| Medium shared client state, minimal ceremony     | Zustand                                                        |
| Large client state + DevTools + strict structure | **Redux Toolkit**                                              |

**When to skip:**

- Mostly server data with little client-owned state → Query + local state
- Prototype or small app → Context or Zustand
- Putting API cache in Redux **without** RTK Query → usually the wrong tool

**Pluses and minuses:**

| +                                               | −                                 |
| ----------------------------------------------- | --------------------------------- |
| One place to inspect and debug client state     | Learning curve and file structure |
| Proven at scale (middleware, entities, codegen) | Slower to start than Zustand      |

**Thoughts:** _"You Might Not Need Redux" is still valid — reach for it when shared state pain is real, not preemptively._

**See also:** [State management libraries](#state-management-libraries) · [Redux core principles](#redux-core-principles)

**Sources:**

[redux.js.org — When Should I Use Redux?](https://redux.js.org/tutorials/essentials/part-1-overview-concepts#when-should-i-use-redux) · [redux-toolkit.js.org](https://redux-toolkit.js.org/)

State management libraries

</details>

## State management

<details>
  <summary>State management libraries</summary>

### State management libraries

**Definition:** Quick comparison of React state tools — Redux Toolkit is one option for **shared client state**; server cache is a separate problem.

| Library                  | Best for                                                                 |
| ------------------------ | ------------------------------------------------------------------------ |
| **Redux Toolkit**        | Large teams, DevTools, normalized entities, RTK Query in one store       |
| **Zustand**              | Minimal boilerplate, slice-like stores without actions/reducers ceremony |
| **Jotai / Recoil**       | Fine-grained atomic subscriptions                                        |
| **TanStack Query**       | Server/async state — standalone cache                                    |
| **RTK Query**            | Server cache inside the Redux store                                      |
| **Context + useReducer** | Small apps, theme/locale                                                 |

|         | Flux                        | Redux                    |
| ------- | --------------------------- | ------------------------ |
| Stores  | Multiple                    | Single store             |
| Updates | Stores self-update          | Pure reducers only       |
| Flow    | Action → Dispatcher → Store | Action → Reducer → Store |

**When to use:** See [When to use Redux](#when-to-use-redux) for the full decision guide.

**Examples:**

```tsx
// Redux Toolkit — see createSlice above
// Zustand — minimal global store
import { create } from "zustand";
const useStore = create((set) => ({
  count: 0,
  inc: () => set((s) => ({ count: s.count + 1 })),
}));
```

**Thoughts:** _Server state and UI state are different problems — don't put fetch cache in Redux unless you need RTK Query._

**See also:** [TanStack Query vs RTK Query](./4_react_tanstack.md#tanstack-query-vs-rtk-query) · [Data fetching](./4_react_ecosystem.md#data-fetching)

**Sources:**

[redux-toolkit.js.org](https://redux-toolkit.js.org/)

TanStack Query vs RTK Query

</details>

## RTK Query

<details>
  <summary>TanStack Query vs RTK Query</summary>

### TanStack Query vs RTK Query

**Definition:** Both cache **server state** in memory. TanStack Query is standalone; RTK Query lives in the Redux store via `createApi`.

Canonical comparison → [React — TanStack](./4_react_tanstack.md#tanstack-query-vs-rtk-query).

**When to use RTK Query:** App already on Redux Toolkit — one store, DevTools, `invalidatesTags`, generated hooks from endpoints.

**See also:** [State management libraries](#state-management-libraries) · [createAsyncThunk](#createasyncthunk) · [Working with async in Redux](#working-with-async-in-redux)

**Sources:**

[redux-toolkit.js.org/rtk-query](https://redux-toolkit.js.org/rtk-query/overview) · [tanstack.com/query](https://tanstack.com/query/latest)

</details>
