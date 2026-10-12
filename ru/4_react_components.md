# React — Components

Встроенные компоненты React из `react`: Fragment, Profiler, StrictMode, Suspense, Activity и ViewTransition.

[ ← Вернуться в центр](./4_react.md)

### Related docs

- [React — Core](./4_react_core.md) · [React — Hooks](./4_react_hooks.md) · [React — Components](./4_react_components.md) · [React — APIs](./4_react_apis.md) · [React — React 19](./4_react_server.md) · [React — Ecosystem](./4_react_ecosystem.md) · [React — Интервью](./4_react_interview.md)

### Sections

- [5. Built-in components](#5-built-in-components)

---

## 5. Built-in components

### Built-in components

Built-in components catalog

<details>
  <summary>Built-in components catalog</summary>

### Built-in components catalog

**Definition:** Специальные JSX-компоненты из `react`, которые управляют рендерингом, загрузкой UI, dev-проверками и (в React 19+) видимостью и view transitions — не обычные виджеты.

**How it works:**

| Компонент                                       | Роль                                         |
| ----------------------------------------------- | -------------------------------------------- |
| [`<Fragment>`](#fragment)                       | Группировка без узла DOM                     |
| [`<Profiler>`](#profiler)                       | Замер времени рендера поддерева              |
| [`<StrictMode>`](#strictmode)                   | Дополнительные dev-проверки                  |
| [`<Suspense>`](#suspense)                       | Fallback, пока дети в suspend                |
| [`<Activity - 19>`](#activity---19)             | Скрыть/показать с сохранением state          |
| [`<ViewTransition - 19>`](#viewtransition---19) | Анимация enter/exit с Transitions + Suspense |

**When to use:** Используйте их раньше кастомных лоадеров и visibility-хаков — они встроены в Fiber, concurrent rendering и Suspense.

**Sources:**

[react.dev/reference/react/components](https://react.dev/reference/react/components)

Fragment

</details>

<details>
  <summary>Fragment</summary>

### Fragment

**Definition:** `Fragment` (или `<>...</>`) группирует несколько детей без лишнего узла DOM.

**How it works:** JSX требует один корневой элемент. `Fragment` даёт корень без DOM — дети остаются соседями других узлов родителя. `<>...</>` компилируется в `<React.Fragment>`; полная форма нужна для `key` на фрагменте (например, пары элементов в списке).

**When to use:** Строки таблицы, несколько узлов в `map`, layout где обёртка `<div>` ломает flex/grid.

**Pluses and minuses:**

| +                     | −                                                   |
| --------------------- | --------------------------------------------------- |
| Без лишнего `<div>`   | Нельзя повесить `className` или события на fragment |
| Чистая семантика HTML | `<>` не принимает `key`                             |

**Examples:**

```tsx
function Row({ user }: { user: User }) {
  return (
    <>
      <td>{user.name}</td>
      <td>{user.email}</td>
    </>
  );
}

items.map((item) => (
  <Fragment key={item.id}>
    <dt>{item.term}</dt>
    <dd>{item.definition}</dd>
  </Fragment>
));
```

**See also:** [JSX](./4_react_core.md#jsx) · [Keys](./4_react_core.md#keys)

**Sources:**

[react.dev/reference/react/Fragment](https://react.dev/reference/react/Fragment)

Profiler

</details>

<details>
  <summary>Profiler</summary>

### Profiler

**Definition:** `<Profiler>` оборачивает поддерево; React вызывает `onRender` с метриками на каждом commit, где обновилось это поддерево.

**How it works:**

- **`id`** — имя региона в колбэке и DevTools
- **`onRender`** — timing на каждый commit
- В production профилирование по умолчанию выключено (нужен profiling build для prod-метрик)

**When to use:** После React DevTools Profiler, чтобы сузить медленную ветку; логи в staging. Не включать постоянно в prod без profiling build.

**Examples:**

```tsx
<Profiler id="App" onRender={onRender}>
  <App />
</Profiler>
```

**See also:** [Performance](./4_react_ecosystem.md#performance) · [React Fiber](./4_react_core.md#react-fiber)

**Sources:**

[react.dev/reference/react/Profiler](https://react.dev/reference/react/Profiler)

StrictMode

</details>

<details>
  <summary>StrictMode</summary>

### StrictMode

**Definition:** `<StrictMode>` — dev-only обёртка с **дополнительными проверками**. Не рисует UI; в production не влияет на поведение.

**How it works:**

| Проверка            | Что происходит                          |
| ------------------- | --------------------------------------- |
| **Double render**   | Функция компонента вызывается дважды    |
| **Double effects**  | mount → cleanup → mount для `useEffect` |
| **Deprecated APIs** | Предупреждения о устаревших API         |

**When to use:** Обернуть корень `<App />` в development. При двойном effect — чинить cleanup, а не отключать Strict Mode.

**Examples:**

```tsx
<StrictMode>
  <App />
</StrictMode>
```

**See also:** [Rules of React catalog](./4_react_core.md#rules-of-react-catalog) · [Components and Hooks must be pure](./4_react_core.md#components-and-hooks-must-be-pure)

**Sources:**

[react.dev/reference/react/StrictMode](https://react.dev/reference/react/StrictMode)

Suspense

</details>

<details>
  <summary>Suspense</summary>

### Suspense

**Definition:** `<Suspense fallback={…}>` показывает **fallback**, пока ребёнок suspend (бросает promise) — при `lazy()`, `use()` на Promise или async-рендере на клиенте.

**How it works:**

1. Ребёнок suspend → ближайший `<Suspense>` рисует `fallback`
2. После resolve promise React повторяет рендер и подставляет UI
3. Вложенные границы дают прогрессивную загрузку
4. SSR/RSC — [Streaming & Suspense](./4_react_server.md#streaming--suspense)

**When to use:** Code-split маршруты, async-данные через `use(promise)`, когда один медленный блок не должен блокировать всю страницу.

**Examples:**

```tsx
<Suspense fallback={<ChartSkeleton />}>
  <Chart />
</Suspense>
```

**See also:** [`use() - 19`](./4_react_hooks.md#use---19) · [Streaming & Suspense](./4_react_server.md#streaming--suspense)

**Sources:**

[react.dev/reference/react/Suspense](https://react.dev/reference/react/Suspense)

Activity - 19

</details>

<details>
  <summary>Activity - 19</summary>

### Activity - 19

**Definition:** `<Activity mode="visible" | "hidden">` **скрывает** поддерево (`display: none`), **сохраняя state** — в отличие от unmount.

**How it works:**

- **`visible`** — обычный рендер
- **`hidden`** — UI скрыт, state в памяти; Effects снимаются и восстанавливаются при показе

**When to use:** Сайдбары, вкладки, формы при переключении видимости; с [`<ViewTransition - 19>`](#viewtransition---19) для анимации.

**Examples:**

```tsx
<Activity mode={isSidebarOpen ? "visible" : "hidden"}>
  <Sidebar />
</Activity>
```

**See also:** [ViewTransition - 19](#viewtransition---19)

**Sources:**

[react.dev/reference/react/Activity](https://react.dev/reference/react/Activity)

ViewTransition - 19

</details>

<details>
  <summary>ViewTransition - 19</summary>

### ViewTransition - 19

**Definition:** `<ViewTransition>` анимирует появление/исчезновение поддерева через View Transitions API в связке с Transitions и Suspense.

**How it works:**

- Обновления внутри `startTransition`
- **`enter` / `exit`** — анимация при mount/unmount или смене `mode` у [`<Activity - 19>`](#activity---19)

**When to use:** Переходы панелей/маршрутов, контент внутри Suspense с плавным enter/exit.

**Examples:**

```tsx
<Activity mode={show ? "visible" : "hidden"}>
  <ViewTransition enter="auto" exit="auto">
    <Counter />
  </ViewTransition>
</Activity>
```

**See also:** [Activity - 19](#activity---19) · [Suspense](#suspense) · [`useTransition`](./4_react_hooks.md#usetransition)

**Sources:**

[react.dev/reference/react/ViewTransition](https://react.dev/reference/react/ViewTransition)

---

</details>
