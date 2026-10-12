# React — Core

Модель библиотеки, рендеринг, JSX и состояние.

[ ← Вернуться в центр](./4_react.md)

### Related docs

- [React — Core](./4_react_core.md) · [React — Hooks](./4_react_hooks.md) · [React — Components](./4_react_components.md) · [React — APIs](./4_react_apis.md) · [React — React 19](./4_react_server.md) · [React — Ecosystem](./4_react_ecosystem.md) · [React — Интервью](./4_react_interview.md)

### Sections

- [1. Library overview](#1-library-overview)
- [2. Rendering model](#2-rendering-model)
- [3. Describing the UI](#3-describing-the-ui)
- [4. State & interactivity](#4-state-interactivity)

---

## 1. Library overview

### Library overview

<details>
  <summary>React overview</summary>

### React overview

**Definition:** React — это **библиотека** JavaScript для создания пользовательских интерфейсов из повторно используемых компонентов. Он управляет только уровнем представления — маршрутизация, выборка данных и глобальные соглашения поступают из фреймворков (Next.js, React Router) или сторонних библиотек.

**When to use:** SPA, дизайн-системы, виджеты (`createRoot` на узле), полный стек Next.js/React Router, React Native/Expo.

**Pluses and minuses:**

| +                                                                        | −                                                                                           |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------- |
| Модель компонентов масштабируется от виджетов до полноценных приложений  | Батарейки в комплект не входят — нужен маршрутизатор, уровень данных, инструмент для сборки |
| Огромная экосистема, пул найма, инструменты (DevTools, Compiler)         | Кривая обучения: перехватчики, параллельный рендеринг, ментальная модель RSC                |
| Декларативный пользовательский интерфейс + согласование виртуального DOM | Размер пакета для небольших вставок по сравнению с ванильным JS                             |
| Кроссплатформенность: веб (React DOM), нативный (React Native)           | Частая смена экосистем (шаблоны, мета-структуры)                                            |
| Сильная интеграция TypeScript                                            | Класс → перехватывает наследие миграции в старых базах кода                                 |

**Examples:**

```tsx
import { useState } from "react";
import { createRoot } from "react-dom/client";

function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount((c) => c + 1)}>{count}</button>;
}

createRoot(document.getElementById("root")!).render(<Counter />);
```

**Thoughts:** React — это **не** фреймворк. Инверсия управления: вы вызываете React (`createRoot`, `render`); фреймворки называют ваши файловые соглашения. Meta (теперь Linux Foundation) поддерживает React; производственные приложения обычно поставляются через Vite, Next.js или Expo, а не через Create React App (устарело).

**Sources:**

[react.dev](https://react.dev/)

Реагировать против ReactDOM

</details>

<details>
  <summary>React vs ReactDOM</summary>

### React vs ReactDOM

**Definition:** `react` = ядро ​​(компоненты, крючки, согласование). `react-dom` = веб-рендерер (монтирование, фиксация, порталы). Отдельные пакеты npm, **одна и та же версия**.

**How it works:**

Веб-установка — это **два** пакета, **три** `react-dom` пути импорта (не три пакета npm):

| | Роль |
| | ------------------ | ------------------------------------------------------------ |
| | `react` | Платформо-независимый пользовательский интерфейс |
| | `react-dom` | Веб-рендерер (заменяет `react-native` на мобильном телефоне) |
| | `react-dom/client` | `createRoot`, `hydrateRoot` |
| | `react-dom` | `createPortal`, `flushSync`, `useFormStatus` |
| | `react-dom/server` | `renderToString` / потоковое ССР |

`react-dom` запускает **первый** рендеринг и **фиксацию**; `react` запускает **рендеринг** + `setState`. Браузер рисует.

**When to use:**

Всегда `react` + `react-dom` в сети. Родной меняет рендерер. Реализация реформ в сфере безопасности через структуру, а не вручную `react-dom/server`.

#### Common pitfalls

Лечение `/server` в качестве третьего пакета; импорт `react-dom` в каждом компоненте; несовпадающие версии; `react` без рендерера; `react-native` в веб-приложении.

**Examples:**

```tsx
import { createRoot } from "react-dom/client";
import { createPortal } from "react-dom";
createRoot(document.getElementById("root")!).render(<App />);
createPortal(<Modal />, document.body);
```

**Thoughts:** _Два пакета для Интернета; три_ `react-dom` _пути._ `react` _описывает пользовательский интерфейс; рендерер фиксирует это._

**See also:** [Конвейер рендеринга](#render-pipeline) · [Гидрация](./4_react_server.md#гидратация)

**Sources:**

[react.dev/reference/react-dom](https://react.dev/reference/react-dom)

Библиотека против фреймворка

</details>

<details>
  <summary>Library vs framework</summary>

### Library vs framework

**Definition:**

**Библиотека** решает конкретную проблему (рендеринг пользовательского интерфейса); вы управляете архитектурой. **Структура** меняет управление — она определяет структуру проекта, маршрутизацию и шаблоны данных.

| | Библиотека (React) | Фреймворк (Next.js) |
| | ------------------ | -------------------------- | ----------------------------------------- |
| | Контроль | Вы составляете стопку | Структура предписывает соглашения |
| | Маршрутизация | Принесите свой собственный | Встроенная файловая маршрутизация |
| | Получение данных | Принесите свой собственный | Серверные компоненты, `fetch` кэширование |

**Examples:**

```tsx
// Library — you wire routing yourself
import { BrowserRouter, Routes, Route } from "react-router-dom";

// Framework — Next.js owns file-based routes and data loading
// app/page.tsx is a Server Component by convention
```

**Thoughts:** Ответ на собеседовании: React — это библиотека. Режим фреймворка Next.js/React Router v7 — это полнофункциональные фреймворки React, построенные на его основе.

**Sources:**

[react.dev/learn/start-a-new-react-project](https://react.dev/learn/start-a-new-react-project)

Философия

</details>

<details>
  <summary>Philosophy</summary>

### Philosophy

**Definition:**

Основные принципы проектирования, определяющие структуру приложений React — декларативный пользовательский интерфейс, композиция и однонаправленный поток данных.

| Принцип                              | Значение                                                                                                          |
| ------------------------------------ | ----------------------------------------------------------------------------------------------------------------- |
| **Декларативный**                    | Опишите, как должен выглядеть пользовательский интерфейс для каждого состояния; React обрабатывает обновления DOM |
| **На основе компонентов**            | UI = дерево многоразовых компонуемых модулей                                                                      |
| **Однонаправленный поток данных**    | Реквизит стекает вниз; события текут вверх                                                                        |
| **Композиция важнее наследования**   | Создавайте пользовательские интерфейсы, вкладывая компоненты, а не иерархии классов                               |
| **Узнай один раз, пиши куда угодно** | Одни и те же навыки работы с компонентами для веб- и нативной версии                                              |

**How it works:**

- **2013** — React с открытым исходным кодом; JSX, виртуальный DOM
- **v16 (2017)** — Перезапись оптоволокна; границы ошибок
- **v16.8 (2019)** — Крюки; компоненты класса необязательны
- **v18 (2022)** — Параллельный рендеринг, `createRoot`, автоматическая пакетная обработка, улучшения саспенса
- **v19 (2024)** — Действия, `useActionState`, `useOptimistic`, Реагирующий компилятор, `ref` как опора
- **v19.2** — `useEffectEvent`, API действий, улучшения стека владельцев

**Examples:**

```tsx
function LikeButton({
  liked,
  onToggle,
}: {
  liked: boolean;
  onToggle: () => void;
}) {
  return <button onClick={onToggle}>{liked ? "Unlike" : "Like"}</button>;
}
```

Реквизит вниз, событие вверх — родитель владеет `liked`.

**See also:**

[Rules of React](#rules-of-react-catalog) · [SOLID in React](#solid-in-react) · [Composition patterns](#composition-patterns)

Rules of React catalog

**Sources:** [react.dev/blog/2024/12/05/react-19](https://react.dev/blog/2024/12/05/react-19)

</details>

### Rules of React

Rules of React catalog

<details>
  <summary>Rules of React catalog</summary>

### Rules of React catalog

**Definition:** **Rules of React** — идиомы, которые React считает **правилами, а не стилем**. Нарушение → баги; Compiler и Concurrent не исправят нарушения за вас.

**How it works:**

| Rule                                                                    |
| ----------------------------------------------------------------------- |
| [Components and Hooks must be pure](#components-and-hooks-must-be-pure) |
| [React calls Components and Hooks](#react-calls-components-and-hooks)   |
| [Rules of Hooks](#rules-of-hooks)                                       |

Поддержка: [`<StrictMode>`](./4_react_components.md#strictmode) и `eslint-plugin-react-hooks`. Подробнее о рендере: [Purity](#purity).

**When to use:** Всегда — в каждом компоненте и custom hook.

**Sources:**

[react.dev/reference/rules](https://react.dev/reference/rules)

Components and Hooks must be pure

</details>

<details>
  <summary>Components and Hooks must be pure</summary>

### Components and Hooks must be pure

**Definition:** Компоненты и хуки должны быть **чистыми** относительно входов одного рендера — предсказуемый UI, отладка и безопасная автоматическая оптимизация.

**How it works:**

| Принцип                      | Смысл                                                     |
| ---------------------------- | --------------------------------------------------------- |
| **Идемпотентность**          | Те же props, state, context → тот же JSX                  |
| **Эффекты вне render**       | Нет `fetch` и подписок в теле render — handlers и effects |
| **Неизменяемые props/state** | Не мутировать; копия и `setState`                         |
| **Аргументы хуков**          | Не менять значения после передачи в Hook                  |
| **После JSX**                | Не мутировать значение, уже попавшее в JSX                |

**When to use:** Каждый путь рендера в компонентах и `use*`.

**Sources:**

[react.dev/reference/rules/components-and-hooks-must-be-pure](https://react.dev/reference/rules/components-and-hooks-must-be-pure)

React calls Components and Hooks

</details>

<details>
  <summary>React calls Components and Hooks</summary>

### React calls Components and Hooks

**Definition:** **React** решает, когда рендерить компоненты и хуки — вы описываете UI в JSX.

**How it works:**

| Правило                               | Смысл                                          |
| ------------------------------------- | ---------------------------------------------- |
| **Не вызывать компонент как функцию** | `<Article />`, не `Article()`                  |
| **Не передавать хуки как значения**   | Не прокидывать `useState` или hook через props |

**Examples:**

```tsx
return (
  <Layout>
    <Article />
  </Layout>
); // ✅
return <Layout>{Article()}</Layout>; // 🔴
```

**Sources:**

[react.dev/reference/rules/react-calls-components-and-hooks](https://react.dev/reference/rules/react-calls-components-and-hooks)

Rules of Hooks

</details>

<details>
  <summary>Rules of Hooks</summary>

### Rules of Hooks

**Definition:** Хуки — функции с жёсткими правилами места вызова; нарушение ломает связанный список хуков компонента.

**How it works:**

| Правило                     | Смысл                                                         |
| --------------------------- | ------------------------------------------------------------- |
| **Только верхний уровень**  | Не в циклах, условиях, вложенных функциях — до early `return` |
| **Только из React-функций** | Компоненты и custom `use*`                                    |

Каталог хуков: [React — Hooks](./4_react_hooks.md).

**Sources:**

[react.dev/reference/rules/rules-of-hooks](https://react.dev/reference/rules/rules-of-hooks)

SOLID in React

</details>

<details>
  <summary>SOLID in React</summary>

### SOLID in React

**Definition:** **SOLID** соответствует **компонентам, хукам, свойствам и композиции**, а не наследованию классов. React уже опирается на **O** и **D** («композиция важнее наследования»).

**How it works:**

Каждая буква соответствует **Examples** ниже — ищите `// good` и `// bad`:

1. **S** — data hook **или** UI-компонент — не оба в одном файле.
2. **O** — расширять через `variant`, `children`, обёртки — не копировать `DangerButton`.
3. **L** — если `Button` принимает `disabled`, каждая обёртка должна его уважать.
4. **I** — узкие props — `Modal` это shell с `children`, а не 20 props.
5. **D** — передавать `UserClient` в `useUsers()` — без `fetch('/api/...')` внутри хука.

**When to use:**

Крупные компоненты, UI-киты, обзоры, интервью. Пропустить церемонию на 10-строчном виджете.

#### Common pitfalls

Over-SRP (15 однолинейных компонентов); бесконечный `variant` перечисления; ДИП для одного прототипа `fetch`. Руководства, а не законы.

**Examples:**

```tsx
type User = { id: string; name: string };
type UserClient = { list(): Promise<User[]> };

// S — good: hook loads data; component renders UI
function useUsers(client: UserClient) {
  const [users, setUsers] = useState<User[]>([]);
  useEffect(() => {
    void client.list().then(setUsers);
  }, [client]);
  return users;
}

function UserList({ users }: { users: User[] }) {
  return (
    <ul>
      {users.map((u) => (
        <li key={u.id}>{u.name}</li>
      ))}
    </ul>
  );
}

// S — bad: one component fetches, formats, handles errors, and renders
// function UserPage() {
//   const [users, setUsers] = useState<User[]>([]);
//   useEffect(() => {
//     fetch("/api/users")
//       .then((r) => r.json())
//       .then(setUsers);
//   }, []);
//   return (
//     <ul>
//       {users.map((u) => (
//         <li key={u.id}>{u.name.toUpperCase()}</li>
//       ))}
//     </ul>
//   );
// }

// O — good: extend Button with variant — no forked components
type ButtonProps = React.ComponentProps<"button"> & {
  variant?: "primary" | "danger";
};

function Button({ variant = "primary", children, ...props }: ButtonProps) {
  return (
    <button className={styles[variant]} {...props}>
      {children}
    </button>
  );
}

// O — bad: copy-paste PrimaryButton / DangerButton — edit both when padding changes
// function DangerButton(props: ButtonProps) {
//   return <button className="btn-danger" {...props} />;
// }

// L — good: disabled actually disables click
function IconButton({
  onClick,
  disabled,
  children,
}: {
  onClick?: () => void;
  disabled?: boolean;
  children: React.ReactNode;
}) {
  return (
    <button type="button" onClick={onClick} disabled={disabled}>
      {children}
    </button>
  );
}

// L — bad: ignores disabled — looks disabled but still fires onClick
// function IconButton({ onClick, disabled, children }) {
//   return (
//     <button type="button" onClick={onClick} aria-disabled={disabled}>
//       {children}
//     </button>
//   );
// }

// I — good: Modal shell — content via children
function Modal({
  open,
  onClose,
  children,
}: {
  open: boolean;
  onClose: () => void;
  children: React.ReactNode;
}) {
  if (!open) return null;
  return (
    <dialog open>
      <button type="button" onClick={onClose}>
        Close
      </button>
      {children}
    </dialog>
  );
}

// I — bad: god Modal — callers forced to pass props they never use
// function Modal({ open, title, body, footer, onConfirm, confirmLabel, ...rest }) { ... }

// D — good: inject API client — swap fake in tests
function UsersPanel({ client }: { client: UserClient }) {
  const users = useUsers(client);
  return <UserList users={users} />;
}

// D — bad: hook hardcodes fetch — hard to test, tied to one URL
// function useUsers() {
//   const [users, setUsers] = useState<User[]>([]);
//   useEffect(() => {
//     void fetch("/api/users")
//       .then((r) => r.json())
//       .then(setUsers);
//   }, []);
//   return users;
// }
```

**Thoughts:** _Небольшие специализированные подразделения, расширяемые за счет состава, вводящие данные — React создан для S, O и D._

**See also:** [Философия](#philosophy) · [Шаблоны композиции](#composition-patterns) · [SOLID (overview)](./7_special_engineering.md#solid-overview)

**Sources:**

[react.dev/learn](https://react.dev/learn) · [patterns.dev/react](https://www.patterns.dev/react/) · [SOLID (overview)](./7_special_engineering.md#solid-overview) · [JS — SOLID in Classes](./3_js_core.md#solid-in-classes) · [JS — SOLID in Functions](./3_js_core.md#solid-in-functions) · [SOLID in Next.js](./4_nextjs_core.md#solid-in-nextjs) · [SOLID in Express](./8_nodejs_http.md#solid-in-express)

---

</details>

## 2. Rendering model

### Rendering model

<details>
  <summary>Render pipeline</summary>

### Render pipeline

**Definition:** Каждое обновление — это **Триггер → Рендеринг → Фиксация**, затем браузер **рисует**. React «рендеринг» = вызов ваших функций, а не рисование пикселей.

**How it works:**

```
Trigger   setState / parent / context / root.render()
   ↓
Render    call components → element tree → diff (memory only; may repeat)
   ↓
Commit    DOM mutations → useInsertionEffect → useLayoutEffect (blocks paint)
   ↓
Paint     browser layout + pixels
   ↓
Effects   useEffect after paint
```

| Кто         | Владеет                                           |
| ----------- | ------------------------------------------------- |
| `react`     | Оказывать, `setState` / крючки                    |
| `react-dom` | Начальный триггер (`createRoot`), коммит, порталы |
| Браузер     | Краска                                            |

Пакеты и пути импорта: [React vs ReactDOM](#react-vs-reactdom). Параллельно: рендеринг может быть приостановлен/перезапущен; **commit никогда не делает пауз** — [Fiber](#react-fiber).

**When to use:**

Всегда. Знайте это для «что происходит в setState?» интервью.

#### Common pitfalls

Рендеринг ≠ обновление DOM; побочные эффекты при рендере; `useLayoutEffect` для всего; смешивание рендеринга React с конвейером HTML/CSS браузера → [1_html.md](./1_html.md).

**Examples:**

```tsx
function Tooltip({ text }: { text: string }) {
  const ref = useRef<HTMLDivElement>(null);
  useLayoutEffect(() => {
    /* measure DOM before paint */
  }, [text]);
  useEffect(() => {
    document.title = text;
  }, [text]); // after paint
  return <div ref={ref}>{text}</div>;
}
```

**Thoughts:** _Расписания триггеров → вычисления рендеринга (могут повторяться) → фиксация изменений один раз → рисование →_ `useEffect`_._

**See also:** [React vs ReactDOM](#react-vs-reactdom) · [Виртуальный DOM и согласование](#virtual-dom--reconciliation) · [Параллельный рендеринг](#concurrent-rendering) · [Пакетная обработка](#batching)

**Sources:**

[react.dev/learn/render-and-commit](https://react.dev/learn/render-and-commit) · [react.dev/reference/react/useLayoutEffect](https://react.dev/reference/react/useLayoutEffect)

Виртуальный DOM и сверка

</details>

<details>
  <summary>Virtual DOM & reconciliation</summary>

### Virtual DOM & reconciliation

**Definition:** React строит дерево из **элементов React** — простых объектов JS, описывающих пользовательский интерфейс (`{ type, props, key }`). **Сверка** – это алгоритм, который сравнивает новое дерево элементов с предыдущим и планирует минимальные изменения DOM. В просторечии это дерево называется «Виртуальным DOM», но оно **не** является полным клоном DOM браузера.

**How it works:**

1. **Рендеринг** — компоненты запускаются и возвращают **новое** дерево элементов (каждый раз неизменяемые снимки).
2. **Diff** – средство согласования проверяет новое и предыдущее дерево и решает: обновить реквизиты, повторно использовать узел DOM или снести и перестроить.
3. **Зафиксировать** — `react-dom` применяет записанные операции DOM (см. [рендеринг конвейер](#render-pipeline))

**Что такое элемент React** (сравнение единиц измерения):

```tsx
// JSX compiles to createElement — produces a plain object, not a DOM node
<h1 className="title">Hello</h1>
// ≈ { type: 'h1', props: { className: 'title', children: 'Hello' }, key: null }
```

См. [Элемент против компонента](#element-vs-comComponent) — компоненты **возвращают** элементы; при сверке сравниваются **элементы**.

**Правила сверки** (эвристика различий, а не глубокое равенство):

| Сравнить                                                    | Результат                                                                                                          |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **Другой** `type` (`div` → `span`, or `Counter` → `Timer`)  | Уничтожить старое поддерево, построить новое с нуля — состояние ниже потеряно                                      |
| **Тот же ДОМ** `type` (`<div>` → `<div>`)                   | Сохранить узел DOM, обновлять только измененные реквизиты                                                          |
| **Тот же компонент** `type` (`<Counter />` → `<Counter />`) | Сохранение экземпляра, обновление реквизитов, рекурсия к дочерним элементам                                        |
| **Списки**                                                  | Сопоставьте братьев и сестер по `key` при рендеринге — без стабильных ключей React возвращается к порядку индексов |

```tsx
// Same type → update text node only, <input> value preserved
return <h1>Hello, {name}</h1>;

// Different type → old <h1> removed, new <p> inserted
return condition ? <h1>Title</h1> : <p>Body</p>;

// Keys — stable id, not index on sortable lists
{
  items.map((item) => <li key={item.id}>{item.name}</li>);
}
```

Компонент может повторно отрисовывать и создавать дерево элементов, **идентичное** предыдущему — при сверке изменений не обнаруживается, а **commit пропускает мутации DOM**. Родительские повторные рендеринги часто повторно запускают дочерние функции, не затрагивая DOM.

Публичная модель представляет собой различие в дереве элементов.Внутренне, начиная с React 16, **[Fiber](#react-fiber)** реализует согласование в виде прерываемых рабочих единиц (связанный список волоконных узлов) — обеспечивает одновременный рендеринг, приоритеты и приостановку. Вы не взаимодействуете с Fiber напрямую.

**When to use:**

Всегда — так работает внутри React. Вы не соглашаетесь.

#### Common pitfalls

- **Shadow DOM ≠ Virtual DOM** — Shadow DOM — это примитив инкапсуляции браузера для веб-компонентов; Дерево React находится на уровне JS.
- **Глубокое сравнение реквизитов в голове** — React сравнивает **тип элемента + ключ**, а не `JSON.stringify(props)`
- **Индекс как** `key` в переупорядочиваемых/фильтруемых списках — повторное использование неправильного экземпляра, устаревшее состояние → [Ключи](#keys)
- **При условии, что все** `setState` **попадание в DOM** — рендеринг выполняется всегда (обычно); DOM обновляется только тогда, когда diff находит изменения
- `key` **при перемонтировании компонента** — намеренный сброс (`<Form key={userId} />`), но удивительно, если случайно

**Pluses and minuses:**

| +                                                        | −                                                                                  |
| -------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Декларативные обновления — пишите `UI = f(state)`        | Вводящее в заблуждение имя — не полный клон DOM для каждого узла                   |
| Повторно использует узлы DOM при совпадении типа и ключа | Diff — это эвристика O(n), а не оптимальная: огромные списки требуют виртуализации |
| Та же модель работает для React Native                   | «VDOM быстрее настоящего DOM» — миф: браузеры уже делают частичные обновления      |

**Thoughts:** _Сверка различий в деревьях элементов по типу и ключу — реквизиты того же типа обновляются на месте; различные типы реконструкций; спискам нужны стабильные ключи._

**See also:** [Конвейер рендеринга](#render-pipeline) · [Элемент против компонента](#element-vs-comComponent) · [Ключи](#keys) · [React Fiber](#react-fiber) · [Пакетная обработка](#batching)

**Sources:**

[react.dev/learn/preserving-and-resetting-state](https://react.dev/learn/preserving-and-resetting-state) · [react.dev/learn/rendering-lists](https://react.dev/learn/rendering-lists)

Реагирующее волокно

</details>

<details>
  <summary>React Fiber</summary>

### React Fiber

**Definition:** **Fiber** — это средство примирения React (начиная с версии 16). Одно **волокно** = одна **единица работы** для экземпляра компонента или хост-узла (`div`, текст). Это **виртуальный стек** — React может приостанавливать, возобновлять, повторно использовать или выбросить его.

**How it works:**

~60 кадров в секунду ≈ 16 мс на кадр. Fiber разделяет рендеринг на **куски** и **планирует** срочную работу в первую очередь, чтобы ввод текста и анимация не блокировались.

**Последовательность** (одно обновление):

```
1. Bootstrap   createRoot → FiberRootNode + HostRootFiber (#root)
2. Queue       setState → update on a fiber → Scheduler assigns a **lane**
3. Render      clone current → WIP; DFS work loop (memory only, no DOM)
                 beginWork ↓  (call component, reconcile children, set flags)
                 completeWork ↑ (bubble flags onto the effect list)
                 between fibers: shouldYield()? → end chunk (pause) or continue
                 urgent lane mid-WIP? → discard WIP, restart
4. Commit      sync (never pauses): Placement / Update / Deletion → flip current
               useLayoutEffect → browser paint → useEffect
```

Рендер ≠ краска. Экран меняется только при **commit**. Примиритель (`react`) ходит по волокнам; рендерер (`react-dom`) пишет ДОМ.

|        | текущий             | незавершенное производство                            |
| ------ | ------------------- | ----------------------------------------------------- |
| Дерево | На экране           | Черновик этого рендеринга                             |
| Ссылка | `alternate` близнец | Клон; использовать повторно, если `type` + `key` матч |

Связанный список (`child` = первый ребенок, `sibling` = следующий, `return` = родительский), поэтому React всегда знает **следующий** файбер без стека вызовов JS — именно так работает пауза/возобновление.

```
App
 └─ child → Header ── sibling → Main
                                 └─ child → ItemA ── sibling → ItemB
```

Пройдите: Приложение → Заголовок → Главное → ItemA → ItemB, затем `completeWork` резервное копирование. Состояние хуков живёт в `memoizedState` на волокне (один и тот же порядок вызовов на каждом рендере).

Одно волокно (или короткий пакет), обработанное ранее `shouldYield()`. React **завершает текущую функцию**, а затем завершает выполнение — никогда в середине компонента. Типичный срез ~5 мс; следующий кадр возобновляется на следующем волокне. **Коммит никогда не приводит к результату.**

```
[browser input]  [React chunk → yield]  [paint]
[next frame: resume from next fiber]
```

Чанки **не** используют дополнительные ядра ЦП и **не** создают собственный тяжелый JS (`array.filter` на 100 тыс. элементов) неблокирующий — нарезается только сверка.

Планировщик приказывает _когда_ запускается обратный вызов. Тег **Дорожки**, _который_ обновляется.

| Высокий → низкий | Триггер                 | Заметки                                                         |
| ---------------- | ----------------------- | --------------------------------------------------------------- |
| Синхронизировать | нажмите, `flushSync`    | Совершить как можно скорее                                      |
| Ввод             | `onChange`, перетащить  | Продолжайте печатать плавно                                     |
| По умолчанию     | нормальный `setState`   | Стандарт                                                        |
| Переход          | `startTransition`       | прерываемый; WIP можно выбросить                                |
| Простой          | предварительная выборка | Самый низкий; голодная работа **истекает** и получает повышение |

Та же полоса → **пакетно**. Выше полоса во время нижнего рендера → выбросить WIP, перезапустить.

**When to use:**

Интервью «Как React остается отзывчивым»; дорогой пользовательский интерфейс вкладок/списков через переходы. Потребности `createRoot` (Реакт 18+).

#### Common pitfalls

- Fiber ≠ параллельный рендеринг — движок vs `createRoot` + переходы
- Условные перехваты сдвигают список перехватчиков волокна.
- Не читай `__reactFiber$` / внутренности из кода приложения

**Pluses and minuses:**

| +                                          | −                                                                 |
| ------------------------------------------ | ----------------------------------------------------------------- |
| Прерываемый рендеринг, приоритеты, саспенс | Внутренние органы нестабильны; commit по-прежнему блокирует поток |

**Examples:**

```tsx
onClick={() => setOpen(true)}; // urgent lane
startTransition(() => setTab('analytics')); // transition lane
```

**Thoughts:** _Рендер готовит работу по частям; commit применяет его за один атомарный проход._

**See also:** [Конвейер рендеринга](#render-pipeline) · [Виртуальный DOM и согласование](#virtual-dom--reconciliation) · [Параллельный рендеринг](#concurrent-rendering) · [Пакетная обработка](#batching)

**Sources:**

[github.com/acdlite/react-fiber-architecture](https://github.com/acdlite/react-fiber-architecture) · [react.dev/learn/render-and-commit](https://react.dev/learn/render-and-commit) · [react.dev/reference/react/useTransition](https://react.dev/reference/react/useTransition) · [Документация по React Scheduler](https://scheduler-explorer.vercel.app/)

Параллельный рендеринг

</details>

<details>
  <summary>Concurrent rendering</summary>

### Concurrent rendering

**Definition:** React может подготовить несколько версий пользовательского интерфейса в памяти, **прерывать** рендеринг с низким приоритетом и **сначала фиксировать срочные обновления**. Построен на [Fiber](#react-fiber) — рендер можно прерывать; коммит нет.

**How it works:**

1. **Срочное обновление** (введите текст, щелкните мышью) — рендеринг и фиксация выполняются немедленно на полосе с высоким приоритетом.
2. **Обновление перехода** (`startTransition`) — отмечен низкий приоритет; React может продолжать показывать предыдущий пользовательский интерфейс при подготовке нового дерева в фоновом режиме.
3. Если новое срочное обновление поступает в середине перехода, React **отменяет** устаревшую работу по переходу и отдает приоритет срочному рендерингу.
4. По-прежнему **однопоточная** — кооперативная работа, а не веб-воркеры.

| Срочно                                 | Отложенный (переходный)                                                      |
| -------------------------------------- | ---------------------------------------------------------------------------- |
| Вводимое значение, нажатие кнопок      | Фильтр большого списка, переключение вкладок, перерисовка диаграммы          |
| Пользователь должен немедленно увидеть | Краткий устаревший пользовательский интерфейс приемлем (`isPending` спиннер) |

**When to use:**

Переключение вкладок, фильтрация больших списков, обновление диаграмм при вводе пользователем данных, границы приостановки для асинхронных данных.

Каждый `setState` — сначала измерьте; Обновления по умолчанию уже достаточно быстры для большинства пользовательских интерфейсов.

#### Common pitfalls

- Запаковка всех обновлений в `startTransition` - ненужный; только отложить дорогостоящий повторный рендеринг
- Ожидается параллельный JS — выход Fiber только в основном потоке
- Забывчивость `createRoot` - наследие `ReactDOM.render` не включает полные параллельные функции

**Examples:**

```tsx
const [isPending, startTransition] = useTransition();

function handleChange(e: React.ChangeEvent<HTMLInputElement>) {
  setInput(e.target.value); // urgent — input stays in sync
  startTransition(() => setFiltered(heavyFilter(e.target.value))); // deferred
}
```

**Thoughts:** _Подготовьте пользовательский интерфейс в фоновом режиме, сначала выполните срочную работу; устаревший пользовательский интерфейс на короткое время — это компромисс._

**See also:** [React Fiber](#react-fiber) · [Конвейер рендеринга](#render-pipeline) · [Пакетная обработка](#batching)

**Sources:**

[react.dev/reference/react/useTransition](https://react.dev/reference/react/useTransition)

Пакетирование

</details>

<details>
  <summary>Batching</summary>

### Batching

**Definition:** React группирует несколько обновлений состояния в **один** цикл рендеринга/фиксации вместо повторного рендеринга после каждого `setState`.

**How it works:**

1. Каждый `setState` / `dispatch` ставит обновление в волокно компонента
2. React обрабатывает всю очередь перед следующим рендерингом — несколько обновлений → **один** проход рендеринга.
3. Внутри этого рендеринга состояние отражает все обновления в очереди (функциональные средства обновления запускаются по порядку).
4. Реагируйте на 18+ пакетов внутри промисов, `setTimeout`, а также собственные обработчики событий (с `createRoot`)

| Версия          | Поведение                                                                                     |
| --------------- | --------------------------------------------------------------------------------------------- |
| Реагировать 17  | Пакетируется только внутри обработчиков событий React                                         |
| Реагировать 18+ | Автоматическое пакетирование обещаний, `setTimeout`, собственные обработчики (с `createRoot`) |

`flushSync(() => setState(...))` вызывает немедленный рендеринг + фиксацию DOM — используйте, когда вам необходимо прочитать DOM сразу после обновления (редко).

**When to use:** По умолчанию — всегда включено. Вы редко об этом думаете.

**Examples:**

```tsx
function handleClick() {
  setCount((c) => c + 1);
  setFlag((f) => !f);
  // React 18+: one re-render, not two
}

// Pitfall — same snapshot twice (not a batching issue)
setCount(count + 1);
setCount(count + 1); // still +1 total — use c => c + 1
```

**Thoughts:** _ Пакетная обработка уменьшает количество рендеров; Функциональные средства обновления исправляют устаревшую математику моментальных снимков._

**See also:** [Состояние как снимок](#state-as-snapshot) · [Конвейер рендеринга](#render-pipeline)

**Sources:**

[react.dev/learn/queueing-a-series-of-state-updates](https://react.dev/learn/queueing-a-series-of-state-updates)

Ключи

</details>

<details>
  <summary>Keys</summary>

### Keys

**Definition:** `key` — это специальный атрибут React, который придает **стабильную идентичность** дочернему элементу среди его братьев и сестер при рендеринге. Согласование сопоставляет элементы списка по `key`, а не по индексу массива.

**How it works:**

1. React ходит по родственным волокнам по порядку
2. При повторном рендеринге он пытается сопоставить каждого нового дочернего элемента с предыдущим дочерним элементом с помощью **того же ключа**.
3. Соответствие найдено → **повторно использовать** волокно + узел DOM, обновить реквизиты.
4. Нет соответствия → смонтировать новый; оставшиеся старые ключи → размонтировать
5. `key` **не** передается в качестве реквизита вашему компоненту

**When to use:**

Каждый предмет в `array.map()`. Используйте стабильные идентификаторы из данных (`todo.id`). Также используйте `key` на компоненте для **принудительного перемонтирования** при изменении идентификатора (`<Form key={userId} />`).

| Ключевой выбор                       | ОК, когда                                                                                     |
| ------------------------------------ | --------------------------------------------------------------------------------------------- |
| База данных `id`                     | По умолчанию — изменение порядка, фильтрация, вставка                                         |
| Индекс массива                       | Статический список — никогда не переупорядочивается, не фильтруется и не добавляется в начало |
| `Math.random()` / `uuid()` за рендер | **Никогда** — новый ключ при каждом рендеринге, полное перемонтирование                       |

#### Common pitfalls

- Индекс как ключ в сортируемых/фильтруемых списках — неправильное повторное использование экземпляра.
- Случайный ключ для каждого рендеринга — ухудшает производительность и состояние
- Изменение `key` **перемонтирует** компонент — сбрасывает внутреннее состояние (используйте намеренно)

**Examples:**

```tsx
// Good — stable id from database
{
  todos.map((todo) => <Todo key={todo.id} todo={todo} />);
}

// Bad — reorder [A,B] → [B,A]: React reuses wrong instances, stale state
{
  todos.map((todo, i) => <Todo key={i} todo={todo} />);
}
```

**Thoughts:** _Ключ = идентификатор во всех рендерах, а не позиция в списке._

**See also:** [Виртуальный DOM и согласование](#virtual-dom--reconciliation) · [Keys](#keys)

**Sources:**

[react.dev/learn/rendering-lists](https://react.dev/learn/rendering-lists)

Строгий режим

</details>

<details>
  <summary>Strict Mode</summary>

### Strict Mode

**Definition:** `<StrictMode>` — это оболочка, предназначенная только для разработчиков, которая выполняет **дополнительные проверки** для раннего выявления ошибок. Он **не** отображает видимый пользовательский интерфейс и оказывает **нулевое** влияние на производство.

**How it works:**

| Проверить                        | Что происходит                                                                           |
| -------------------------------- | ---------------------------------------------------------------------------------------- |
| **Двойной рендеринг**            | Функция компонента, вызываемая дважды — улавливает побочные эффекты нечистого рендеринга |
| **Двойной эффект**               | `useEffect` запускает mount → очистка → mount — обнаруживает отсутствующую очистку       |
| **Устаревшие API**               | Предупреждает об устаревших шаблонах (`findDOMNode`, старый контекст, ссылки на строки)  |
| **Небезопасные жизненные циклы** | Предупреждения компонентов класса (устаревшие версии)                                    |

**When to use:**

Сворачивать `<App />` в основе развития. Оставьте включенным — это научит вас писать гибкие эффекты и чистые рендеры.

В качестве исправления двойной выборки — исправьте очистку эффектов или логику дедупликации вместо удаления строгого режима.

#### Common pitfalls

- Отключение строгого режима, поскольку эффекты запускаются дважды — из-за ошибки отсутствует очистка, а не строгий режим.
- Размещение побочных эффектов в теле рендеринга — двойной рендеринг отображает их → [Правила React](#rules-of-react-catalog)
- Предполагая, что производство ведет себя как разработка - проверки строгого режима удалены в сборках продукта.

**Examples:**

```tsx
<StrictMode>
  <App />
</StrictMode>
```

**Thoughts:** _Строгий режим проводит стресс-тестирование ваших компонентов — если он сломается в процессе разработки, значит, он уже хрупкий._

**See also:** [Правила React](#rules-of-react-catalog) · [Чистота](#purity) · [Конвейер рендеринга](#render-pipeline)

**Sources:**

[react.dev/reference/react/StrictMode](https://react.dev/reference/react/StrictMode)

Элемент против компонента

</details>

<details>
  <summary>Element vs component</summary>

### Element vs component

**Definition:** **Элемент React** — это простой объект (`{ type, props, key }`), описывающее, что визуализировать. **Компонент** — это функция или класс, который **возвращает** элементы, когда React вызывает его.

**How it works:**

1. Вы пишете JSX → компилируется в `React.createElement(type, props, ...children)`
2. `<Welcome name="Ada" />` создает элемент `{ type: Welcome, props: { name: 'Ada' } }`
3. Примиритель видит `type: Welcome` → **звонит** `Welcome({ name: 'Ada' })`
4. Компонент возвращает больше элементов (например. `<h1>`) → рекурсия продолжается до тех пор, пока не будут размещены элементы DOM.
5. Новое дерево элементов при каждом рендеринге; **экземпляры** компонента (волокно + состояние) сохраняются

| | Элемент | Компонент |
| | ----------- | ------------------------------ | --------------------------------- |
| | Что | Неизменяемое описание (снимок) | Многоразовая функция/класс |
| | Создано | `createElement` / JSX | Вы определяете это |
| | За рендер | Новый объект каждый раз | Та же функция, вызванная повторно |
| | Государство | Нет | Крючки живут на оптоволокне |

**When to use:**

Элементы – это то, что отличает примирение. Компоненты — это то, как вы организуете логику. Заглавная буква = компонент (`<Welcome />`); нижний регистр = DOM (`<div />`).

#### Common pitfalls

- Сбивает с толку `<Welcome />` с узлом DOM — это инструкция для вызова `Welcome`
- Вызов `Welcome()` как функция — [Правила React](#rules-of-react-catalog): только JSX, поэтому React владеет вызовом
- Строчные JSX (`<welcome />`) — React рассматривает его как неизвестный HTML-тег, а не ваш компонент.
- Мутирующие объекты элементов — всегда создавайте новые элементы через JSX при каждом рендеринге.

**Examples:**

```tsx
// Component (function) — React CALLS this
function Welcome({ name }: { name: string }) {
  return <h1>Hello, {name}</h1>; // returns an element
}

// Element — description object, not a live DOM node
const element = <Welcome name="Ada" />;
// ≈ { type: Welcome, props: { name: 'Ada' } }
```

**Thoughts:** _Компоненты являются функциями; элементы — это их выходные снимки — при сверке сравниваются элементы._

**See also:** [Правила React](#rules-of-react-catalog) · [Виртуальный DOM и согласование](#virtual-dom--reconciliation) · [Компоненты](#comComponents)

**Sources:**

[react.dev/learn/ваш-первый-компонент](https://react.dev/learn/ваш-первый-компонент)

---

</details>

## 3. Describing the UI

### Describing the UI

<details>
  <summary>Components</summary>

### Components

**Definition:** Компонент React — это функция JavaScript (или класс — устаревший), которая возвращает JSX. Имена **должны** начинаться с заглавной буквы, чтобы React отличал их от тегов HTML.

**When to use:**

Каждая часть пользовательского интерфейса. Отдавайте предпочтение небольшим, сфокусированным компонентам, расположенным вверх.

#### Pitfalls

1. Имена в нижнем регистре отображаются как HTML-теги: `<profile />` → Элемент DOM, а не ваш компонент
2. **Никогда не вставляйте определения компонентов** внутри другого компонента — приводит к перемонтированию каждого родительского рендеринга.

```tsx
// Bad — Profile recreated every Gallery render
function Gallery() {
  function Profile() {
    return <p>...</p>;
  }
  return <Profile />;
}

// Good — define at module scope
function Profile() {
  return <p>...</p>;
}
function Gallery() {
  return <Profile />;
}
```

**Pluses and minuses:**

| +                                                       | −                                           |
| ------------------------------------------------------- | ------------------------------------------- |
| Разметка, логика, стили совместного размещения          | Глубокие деревья → бурение без планирования |
| Многоразовое использование с любой степенью детализации | Чрезмерная фрагментация добавляет шаблон    |
| Тестируется изолированно                                |                                             |

**Examples:**

```tsx
function VideoList({ videos }: { videos: Video[] }) {
  return (
    <section>
      {videos.map((v) => (
        <Video key={v.id} video={v} />
      ))}
    </section>
  );
}
```

**Thoughts:**

_Компоненты — это функции: заглавная буква = компонент React, строчная = HTML._

Единая ответственность — если компонент растет, разделите его. Страницы/виджеты/функции в терминах FSD (см. [Раздел 9](./4_react_ecosystem.md#9-ecosystem--architecture)).

**See also:** [Элемент против компонента](#элемент-против-компонента) · [Чистота](#purity)

**Sources:**

[react.dev/learn/ваш-первый-компонент](https://react.dev/learn/ваш-первый-компонент)

JSX

</details>

<details>
  <summary>JSX</summary>

### JSX

**Definition:** JSX — это расширение синтаксиса, которое компилируется в `React.createElement(type, props, ...children)`. Он позволяет вам писать HTML-подобную разметку внутри JavaScript.

**How it works:**

- Один корневой элемент или `<>...</>` Фрагмент
- Закрыть все теги (`<img />`, `<br />`)
- DOM-атрибуты CamelCase: `className`, `onClick`, `tabIndex`
- Теги с заглавной буквы = компоненты; нижний регистр = HTML

**Pluses and minuses:**

| +                                            | −                                            |
| -------------------------------------------- | -------------------------------------------- |
| Читабельный код пользовательского интерфейса | Требуется этап сборки (Babel, SWC, esbuild)  |
| Типобезопасность с TypeScript                | `class` → `className`, `for` → `htmlFor`     |
| Выражения через `{}`                         | Должен возвращать один корень (или фрагмент) |

**Examples:**

```tsx
const name = "Ada";
return (
  <>
    <h1 className="title">{name}</h1>
    {items.length > 0 && <List items={items} />}
  </>
);
```

**Thoughts:** JSX — это синтаксический сахар. `<>...</>` компилируется в `Fragment`. Условный рендеринг: предпочесть `condition ? <A /> : <B />` or `!!condition && <A />` - следить за `0` ложный рендеринг.

**Sources:**

[react.dev/learn/writing-markup-with-jsx](https://react.dev/learn/writing-markup-with-jsx)

Фрагменты

</details>

<details>
  <summary>Fragments</summary>

### Fragments

**Definition:** `Fragment` (или сокращенно `<>...</>`) группирует несколько дочерних элементов без добавления дополнительного узла DOM.

**How it works:** JSX требует одного корневого элемента. `Fragment` удовлетворяет этому правилу, ничего не отображая в DOM — дочерние узлы появляются как братья и сестры других узлов родителя. `<>...</>` компилируется в `<React.Fragment>`; используйте длинную форму, когда вам нужно `key` в самом фрагменте (например, элементы списка с ключами, которые возвращают несколько элементов).

**When to use:** Строки таблицы (`<tr>` пары), элементы списка с несколькими узлами, оболочки макета, где дополнительная оболочка может нарушить CSS.

**Pluses and minuses:**

| +                                                              | −                                                          |
| -------------------------------------------------------------- | ---------------------------------------------------------- |
| Избегает оболочки `<div>` это нарушает макет (гибкий/сетчатый) | Невозможно установить `className` или события на фрагменте |
| Сохраняет семантический HTML в чистоте                         | стенография `<>` не принимает `key`                        |
| Требуется для возврата нескольких корней из компонента         |                                                            |

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

// Keyed fragment in a list
items.map((item) => (
  <Fragment key={item.id}>
    <dt>{item.term}</dt>
    <dd>{item.definition}</dd>
  </Fragment>
));
```

**Sources:**

[react.dev/reference/react/Fragment](https://react.dev/reference/react/Fragment)

Реквизит

</details>

<details>
  <summary>Props</summary>

### Props

**Definition:** Свойства — это входные данные **только для чтения**, передаваемые от родительского элемента к дочернему, как и аргументы функции. Ребенок не может их мутировать; только родитель может изменить реквизиты путем повторного рендеринга с новыми значениями.

**How it works:**

1. Родительский рендеринг → передает снимок реквизита дочернему элементу: `<Child count={count} onSave={handleSave} />`
2. Дочерний элемент получает реквизиты для **только этого рендеринга** — заморожен для этого вызова.
3. Дочерние запросы изменяются с помощью **callback prop** (`onIncrement`) — родитель владеет состоянием и обновлениями
4. Односторонний поток: данные **вниз**, события **вверх**

**When to use:**

Передавайте данные, конфигурацию и обработчики событий вниз по дереву. Поднимите состояние, когда братьям и сестрам нужны общие данные → [Поднятие состояния](#lifting-state-up).

#### Common pitfalls

- Мутация реквизита внутри дочернего элемента (`props.items.push(...)`) — прерывает односторонний поток
- `{...props}` на элементах DOM — утечка недопустимых атрибутов (`isLoading`, `variant`)
- Создание нового объекта/функции при каждом рендеринге — может сломаться. `memo` дети (`onClick={() => ...}`)

**Examples:**

```tsx
function Button({ label, onClick, ...rest }: ButtonProps) {
  return (
    <button onClick={onClick} {...rest}>
      {label}
    </button>
  );
}
```

**Thoughts:** _Входит реквизит, обратные вызовы выходят — родительский объект владеет изменяемым состоянием._

**See also:** [Состояние vs реквизит](#state-vs-props) · [Шаблоны композиции](#composition-patterns)

**Sources:**

[react.dev/learn/passing-props-to-a-comComponent](https://react.dev/learn/passing-props-to-a-comComponent)

Чистота

</details>

<details>
  <summary>Purity</summary>

### Purity

**Definition:**

Компоненты React должны быть **чистыми** во время рендеринга: те же входные данные → тот же вывод. Полный контракт: [Components and Hooks must be pure](#components-and-hooks-must-be-pure) · [Rules of React catalog](#rules-of-react-catalog).

| Разрешено                          | Запрещено во время рендеринга                              |
| ---------------------------------- | ---------------------------------------------------------- |
| Создание локальных переменных      | `fetch()` побочные эффекты                                 |
| Локальная мутация _новых_ объектов | Изменить реквизиты, состояние, контекст, ссылки            |
| Вычисление производных значений    | Подпишитесь на внешние магазины без `useSyncExternalStore` |

**Побочные эффекты** относятся к обработчикам событий или эффектам, а не к телу рендеринга.

**When to use:**

Рендер каждого компонента должен быть чистым. Получение отображаемых значений при рендеринге; скачать/подписаться в `useEffect` или обработчики событий.

#### Common pitfalls

- `fetch` в рендеринге — запускает каждый повторный рендеринг; использовать `useEffect` или уровень данных платформы
- [Strict Mode](#strict-mode) двойные вызовы рендеринга в разработке — обнажает нечистый код
- Чтение внешних изменяемых глобальных переменных во время рендеринга — вывод меняется без изменения реквизита/состояния.

**Examples:**

```tsx
// Bad — impure render
function ItemList({ items }: { items: Item[] }) {
  items.push({ id: "new" }); // mutates prop!
  return (
    <ul>
      {items.map((i) => (
        <li key={i.id}>{i.name}</li>
      ))}
    </ul>
  );
}

// Good — local mutation of new data is fine
function FriendList({ friends }: { friends: string[] }) {
  const sorted = [...friends].sort(); // new array, local
  return (
    <ul>
      {sorted.map((f) => (
        <li key={f}>{f}</li>
      ))}
    </ul>
  );
}
```

**Thoughts:** _Рендеринг — это чистая функция реквизита, состояния и контекста._

**See also:** [Правила React](#rules-of-react-catalog) · [Конвейер рендеринга](#render-pipeline)

**Sources:**

[react.dev/learn/keeping-comComponents-pure](https://react.dev/learn/keeping-comComponents-pure)

Условный рендеринг и списки

</details>

<details>
  <summary>Conditional rendering & lists</summary>

### Conditional rendering & lists

**Definition:** JSX — это JavaScript — используйте `if`троичный, `&&`, и `map()` чтобы решить, что визуализировать. Никакого специального синтаксиса шаблона.

**How it works:** Выражения внутри `{ }` оценить узлы React, `null`, `undefined`, `false` (ничего не отображать) или массивы узлов. Рано `return` до того, как JSX часто наиболее понятен для сложных ветвей.

**When to use:**

| Узор                        | Вариант использования                                               |
| --------------------------- | ------------------------------------------------------------------- |
| `if` + ранний `return`      | Множественные ветки, состояния загрузки/ошибки                      |
| `condition ? <A /> : <B />` | Две встроенные ветки                                                |
| `condition && <A />`        | Одиночное ответвление — **нет**, если может быть левая сторона. `0` |
| `items.map()`               | Списки — всегда обеспечивайте стабильную `key` → [Ключи](#ключи)    |

#### Common pitfalls

- `{count && <Badge />}` когда `count === 0` — рендерит `0` на экране; использовать `count > 0 &&`
- Вложенные тройки — трудно читать; извлечь компонент или использовать ранний возврат
- `map` без `key` — предупреждения о сверке и ошибки при повторном заказе

**Examples:**

```tsx
if (loading) return <Spinner />;
if (error) return <Error message={error.message} />;

{
  isLoggedIn ? <Dashboard /> : <Login />;
}
{
  count > 0 && <Badge count={count} />;
} // not: {count && ...} when count can be 0
{
  items.map((item) => <Row key={item.id} item={item} />);
}
```

**Thoughts:** _у JSX нет_ `v-if` _— используйте выражения JavaScript и ранние возвраты._

**See also:** [Ключи](#keys) · [Деревья компонентов](#compent-trees)

**Sources:**

[react.dev/learn/conditional-rendering](https://react.dev/learn/conditional-rendering)

Деревья компонентов

</details>

<details>
  <summary>Component trees</summary>

### Component trees

**Definition:** React моделирует пользовательский интерфейс как деревья. Два связанных представления: **дерево рендеринга** (родительские/дочерние компоненты среды выполнения) и **дерево модулей** (статические `import` график для объединения).

**How it works:**

- Каждый экземпляр компонента представляет собой **узел**; родитель отображает дочерних элементов как вложенный JSX
- React сопоставляет состояние с **положением в дереве** — тот же тип компонента в том же месте → состояние сохраняется.
- Другой тип в одном слоте → старое поддерево **размонтировано**, состояние потеряно
- Условный рендеринг меняет форму дерева → может непреднамеренно сбросить состояние.

| Дерево        | Что это показывает                         | Используется для                |
| ------------- | ------------------------------------------ | ------------------------------- |
| **Рендеринг** | Кто кого визуализирует во время выполнения | Состояние, примирение, контекст |
| **Модуль**    | Зависимости импорта файлов                 | Разделение кода, анализ пакетов |

**When to use:**

Понимайте дерево рендеринга при отладке сброса состояния, области контекста и размещения границ ошибок.

#### Common pitfalls

- Обмен `<A />` для `<B />` в той же позиции — разрушает состояние А
- Предполагается, что состояние следует за именем переменной — имеет значение положение в дереве, а не метка JSX.
- Вложение одного и того же компонента в разные ветки — отдельные экземпляры состояния.

**Examples:**

```tsx
// Same position → state preserved across re-renders
<Profile userId={id} />;

// Different type at same slot → state reset
{
  showA ? <CounterA /> : <CounterB />;
}

// Same variable name, different position — still different state slots
{
  cond ? (
    <Panel>
      <Counter />
    </Panel>
  ) : (
    <Counter />
  );
}
```

**Thoughts:** _Состояние привязано к положению в дереве, а не к тому, как вы назвали переменную._

**See also:** [Виртуальный DOM и согласование](#virtual-dom--согласование) · [Ключи](#keys)

**Sources:**

[react.dev/learn/understanding-your-ui-as-a-tree](https://react.dev/learn/understanding-your-ui-as-a-tree)

Шаблоны композиции

</details>

<details>
  <summary>Composition patterns</summary>

### Composition patterns

**Definition:**

Создавайте сложные пользовательские интерфейсы, вставляя компоненты и передавая `children`.

| Узор                          | Статус           | Использование                                   |
| ----------------------------- | ---------------- | ----------------------------------------------- |
| **Дети**                      | Предпочтительный | Оболочки макетов, общие оболочки                |
| **Слоты** (именные реквизиты) | Общий            | `<Card header={...} footer={...} />`            |
| **НОС**                       | Наследие         | `withRouter`, `connect` — избегать в новом коде |
| **Рендеринг реквизита**       | Наследие         | `<DataProvider render={data => ...} />`         |

**How it works:** Родитель передает фрагменты пользовательского интерфейса как `children` или именованные реквизиты — дочерний элемент отображает слоты, не зная источника данных родителя. Позволяет избежать сверления опор для макетных оболочек.

**When to use:**

| Узор                                   | Предпочитаю, когда                                                 |
| -------------------------------------- | ------------------------------------------------------------------ |
| `children`                             | Общая оболочка (Карточка, Макет, Модальная оболочка)               |
| Именованные слоты (`header`, `footer`) | Фиксированные регионы разных типов                                 |
| Контекст                               | Глубокое дерево, стабильные общие данные (тема, аутентификация)    |
| HOC / рендеринг реквизита              | Только устаревшие библиотеки — в большинстве случаев заменяют хуки |

#### Common pitfalls

- Проп-сверление через 5+ слоев — состояние подъема, контекста или совместного размещения.
- Адская обертка HOC — трудно отследить реквизиты; предпочитаю крючки + композиция
- `children` как функция для всего — рендеринг шаблона реквизита; использовать только при необходимости

**Examples:**

```tsx
function Card({
  children,
  header,
}: {
  children: React.ReactNode;
  header?: React.ReactNode;
}) {
  return (
    <article>
      {header && <header>{header}</header>}
      <main>{children}</main>
    </article>
  );
}
```

**Thoughts:** _Составляйте пользовательский интерфейс по принципу вложенного HTML — слоты важнее наследования._

**See also:** [SOLID в React](#solid-in-react) · [Props](#props) · [Подъём состояния](#lifting-state-up)

**Sources:**

[patterns.dev/react](https://www.patterns.dev/react/)

---

</details>

## 4. State & interactivity

### State & interactivity

<details>
  <summary>State</summary>

### State

**Definition:** Состояние — это память компонента — данные, которые изменяются со временем в ответ на ввод пользователя или асинхронные результаты. React хранит состояние вне вашей функции и предоставляет вам снимок для каждого рендеринга.

**How it works:**

1. React хранит состояние в **волокне** компонента — вне тела вашей функции.
2. Каждый рендер, `useState` возвращает **текущий снимок** для этого рендеринга
3. `setState` планирует обновление — не изменяет снимок, который вы держите
4. Повторный рендеринг → функция запускается снова с новым снимком.

**When to use:**

Любой изменяющийся пользовательский интерфейс: формы, переключатели, выборка данных, выбор.

#### Common pitfalls

- Ожидание сохранения локальных переменных (`let count = 0` в теле функции — сбрасывает каждый рендер)
- Дублирование состояния, которое может быть получено из реквизита или другого состояния.
- Мутирующие государственные объекты на месте (`items.push(x); setItems(items)`) — та же ссылка, React может пропустить обновление

**Pluses and minuses:**

| +                                                        | −                                                                  |
| -------------------------------------------------------- | ------------------------------------------------------------------ |
| Локальная инкапсуляция                                   | Легко создать избыточное/дублированное состояние                   |
| Запускает целевой повторный рендеринг                    | Асинхронная пакетная обработка может удивить (устаревшие закрытия) |
| `useState` / `useReducer` охватывают большинство случаев | Глобальному государству нужен контекст или внешнее хранилище       |

**Examples:**

```tsx
const [count, setCount] = useState(0);
// Functional update avoids stale closure
setCount((c) => c + 1);
```

**Thoughts:** _Государство живет за счет оптоволокна; при каждом рендеринге используется замороженный снимок._

**See also:** [Состояние как снимок](#state-as-snapshot) · [Пакетная обработка](#batching) · [Реактивное программирование](./7_special_architecture.md#reactive-programming--reactivity)

**Sources:**

[react.dev/learn/state-a-comComponents-memory](https://react.dev/learn/state-a-comComponents-memory)

Государство против реквизита

</details>

<details>
  <summary>State vs props</summary>

### State vs props

**Definition:** **Props** — это входные данные родительского объекта, доступные только для чтения; **состояние** — это данные, которыми **владеет** компонент (или поднятый предок) и которые обновляются через установщик.

**How it works:**

Родитель удерживает состояние → передает **снимок** в качестве реквизита → дочерний элемент отображает и вызывает обратный вызов → родительский элемент обновляет состояние → дочерний элемент повторно визуализируется с новыми реквизитами. Ребенок никогда не мутирует реквизит напрямую.

| | Реквизит | Государство |
| | ------------- | ------------------------------------ | -------------------------------------------------------- |
| | Источник | Родитель | Местный (`useState`) или глобальный (контекст/хранилище) |
| | Мутабельность | Только чтение для детей | Обновлено через установщик |
| | Изменения | Родитель повторно отображает ребенка | Я или ребенок запускает обновление |
| | Вопрос | «Какие данные я получу?» | «Что я помню?» |

**When to use:** Реквизит для конфигурации и потока данных. Состояние интерактивного пользовательского интерфейса, которое меняется со временем. Если это нужно двум братьям и сестрам → поднимите состояние.

**Examples:**

```tsx
function Parent() {
  const [count, setCount] = useState(0);
  return (
    <Child
      label="Clicks"
      count={count}
      onIncrement={() => setCount((c) => c + 1)}
    />
  );
}
```

**Thoughts:** _Реквизит на месте, события на исходе — государство живет там, где оно принадлежит._

**See also:** [Props](#props) · [Поднятие состояния](#lifting-state-up)

**Sources:**

[react.dev/learn/passing-props-to-a-comComponent](https://react.dev/learn/passing-props-to-a-comComponent)

Состояние как снимок

</details>

<details>
  <summary>State as snapshot</summary>

### State as snapshot

**Definition:** Вызов `setState` планирует повторный рендеринг со **следующим** состоянием — он не меняет текущее состояние на месте. Обработчики событий закрываются во время **моментального снимка времени рендеринга**, а не в режиме реального времени.

**How it works:**

1. Рендеринг с помощью `n = 0` → обработчик создан, закрывается `n = 0`
2. `setN(n + 1)` обновление очередей на основе снимка `0` → следующий `n` is `1`
3. Два `setN(n + 1)` в одном обработчике оба читают `0` → еще `1`, нет `2`
4. `setN(prev => prev + 1)` функциональные средства обновления очередей — каждый видит предыдущий результат в очереди
5. [Пакетная обработка](#batching) обрабатывает все обновления в очереди перед следующим рендерингом.

**When to use:** Всегда помните об этом при отладке счетчиков, асинхронных обработчиков и устаревших ошибок закрытия.

**Examples:**

```tsx
function Counter() {
  const [n, setN] = useState(0);
  function handleClick() {
    setN(n + 1);
    setN(n + 1);
    // After one click: n becomes 1, not 2 — both use snapshot n=0
    setN((prev) => prev + 1);
    setN((prev) => prev + 1);
    // After one click: n becomes 2 — functional updates queue correctly
  }
}
```

**Thoughts:** _setState планирует будущее; текущие значения рендеринга заморожены._

**See also:** [Пакетная обработка](#пакетная обработка) · [Состояние](#состояние)

**Sources:**

[react.dev/learn/state-as-a-snapshot](https://react.dev/learn/state-as-a-snapshot)

Принципы структурирования государства

</details>

<details>
  <summary>Principles for structuring state</summary>

### Principles for structuring state

**Definition:**

Рекомендации по выбору того, что хранить в состоянии, чтобы обновления оставались предсказуемыми, а компоненты — простыми.

1. **Состояние, связанное с группой** — переменные слияния всегда обновляются вместе.
2. **Избегайте противоречий**. `isSending` + `isSent` оба правда
3. **Избегайте избыточности** — не храните то, что можно вычислить во время рендеринга.
4. **Избегайте дублирования** — одни и те же данные в двух переменных состояния смещаются.
5. **Избегайте глубокой вложенности** — отдавайте предпочтение плоскому состоянию + идентификаторы.

**When to use:** Прежде чем добавлять `useState`, спросите: можно ли это вычислить? Дублируется ли он где-нибудь еще? Будут ли противоречить два флага?

**Examples:**

```tsx
// Bad — redundant
const [firstName, setFirstName] = useState("");
const [lastName, setLastName] = useState("");
const [fullName, setFullName] = useState(""); // derive instead!

// Good
const fullName = `${firstName} ${lastName}`;
```

**Thoughts:** _Минимизируйте поверхность состояния — по возможности извлекайте ее во время рендеринга._

**See also:** [Состояние](#state) · [Чистота](#purity)

**Sources:**

[react.dev/learn/choosing-the-state-structure](https://react.dev/learn/choosing-the-state-structure)

Поднятие состояния

</details>

<details>
  <summary>Lifting state up</summary>

### Lifting state up

**Definition:** Переместить общее состояние к **ближайшему общему предку** компонентов, которым оно необходимо; передавать данные и обратные вызовы в качестве реквизита.

**How it works:**

1. Определите братьев и сестер, которым нужны одни и те же данные (входные данные поиска + отфильтрованный список)
2. Найдите ближайшего общего родителя.
3. Двигайтесь `useState` этому родителю
4. Пройти `value` + `onChange` (или производные данные) в качестве реквизита

Состояние используется только одним компонентом — сохраняйте локальное. Prop drilling → композиция (`children`), внешнее хранилище или context в [React — Hooks](./4_react_hooks.md).

**When to use:** Два или более брата и сестры обмениваются данными или должны оставаться синхронизированными.

**Examples:**

```tsx
function Page() {
  const [query, setQuery] = useState("");
  return (
    <>
      <SearchInput value={query} onChange={setQuery} />
      <FilteredList query={query} />
    </>
  );
}
```

**Thoughts:** _Общее состояние живет у наименьшего общего предка._

**See also:** [Состояние vs реквизит](#state-vs-props) · [Принципы структуры состояния](#principles-for-structuring-state)

**Sources:**

[react.dev/learn/sharing-state-between-comComponents](https://react.dev/learn/sharing-state-between-comComponents)

Контролируемые и неконтролируемые входы

</details>

<details>
  <summary>Controlled vs uncontrolled inputs</summary>

### Controlled vs uncontrolled inputs

**Definition:**

**Управляемые** входы получают `value` из состояния React; **неконтролируемые** входы сохраняют значение в DOM и считываются через ссылки.

| | Контролируется | Неконтролируемый |
| | ----------------- | ----------------------------------------- | ---------------------------------------------------- |
| | Источник значения | Состояние реакции | ДОМ (`ref.current.value`) |
| | Когда | По умолчанию для форм в приложениях React | Ввод файлов, интеграция библиотек, отличных от React |
| | API | `value` + `onChange` | `ref` + `defaultValue` |

**Examples:**

```tsx
// Controlled
<input value={email} onChange={e => setEmail(e.target.value)} />

// Uncontrolled
<input ref={inputRef} defaultValue="" />
```

**Thoughts:** Действия формы React 19 сокращают количество шаблонов для форм неконтролируемого стиля с мутациями сервера.

**Sources:**

[react.dev/reference/react-dom/comComponents/input](https://react.dev/reference/react-dom/comComponents/input)

State normalization

</details>

<details>
  <summary>State normalization</summary>

### State normalization

**Definition:** Хранить коллекции как `{ ids: Id[], entities: Record<Id, Entity> }` вместо вложенных массивов для поиска O(1) и целевых обновлений.

**How it works:** `ids` сохраняет порядок рендеринга; `entities` идентификатор карты → объект. Обновите одну сущность по идентификатору, не сканируя весь массив. Селекторы извлекают отфильтрованные/отсортированные представления из нормализованного хранилища.

**When to use:** Большие списки, частые обновления каждого элемента, нормализованные кеши Redux/Apollo. Пропустите небольшие статические списки.

**Pluses and minuses:**

| +                                             | −                                                |
| --------------------------------------------- | ------------------------------------------------ |
| Быстрый поиск по идентификатору               | Больше настроек, чем простые массивы             |
| Обновить один объект без сканирования массива | Денормализованным представлениям нужны селекторы |
| Меньше ненужных повторных рендерингов         |                                                  |

**Examples:**

```tsx
// Instead of users: User[]
type UsersState = {
  userIds: number[];
  usersById: Record<number, User>;
};

// Read
const user = state.usersById[2];

// Update one user
state.usersById[2] = { ...state.usersById[2], name: "Ivan" };

// Render list
state.userIds.map((id) => <User key={id} user={state.usersById[id]} />);
```

**Thoughts:**

_Идентификаторы для заказа, карта для поиска — обновляйте одну строку, не трогая весь список._

Инструментарий Redux `createEntityAdapter`, Apollo/Relay автоматически нормализует кеш GraphQL. TanStack Query сохраняет форму сервера как есть — нормализовать в `select` если необходимо. Не имеет отношения к SQL 1NF–3NF — то есть [9_database.md — Нормальные формы](./9_database_core.md#normal-forms-1nf3nf-bcnf).

**Sources:**

[redux-toolkit.js.org/api/createEntityAdapter](https://redux-toolkit.js.org/api/createEntityAdapter)

---

</details>
