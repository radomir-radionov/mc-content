# React — Redux interview

Вопросы по Redux. Ответы заполнены для полного списка вопросов.

[ ← Вернуться в хаб](./4_react.md)

### Related docs

- [React — Redux](./4_react_redux.md) · [Redux — Интервью](./4_react_redux_interview.md) · [React — Интервью](./4_react_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Вопросы с [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/).

### Redux Fundamentals

<details>
<summary>Что такое Redux and how is it used in web development?</summary>

### Что такое Redux and how is it used in web development?

**Definition:**

Redux — предсказуемый контейнер состояния, в первую очередь рассчитанный на JavaScript-приложения. Он эффективно хранит и обновляет состояние приложения и служит централизованным глобальным хранилищем.

**How it works:**

**Основные понятия**

**Store**

Store находится в центре Redux и выступает единым источником истины. Здесь лежит текущее состояние и возможность отправлять действия.

**Actions**

Actions — это полезная нагрузка данных, обычно объекты, которые описывают, что должно произойти. Их отправляют в store.

**Reducers**

Reducers — чистые функции, которые задают, как состояние меняется в ответ на действия. Каждый reducer обрабатывает свою часть состояния, и вместе они дают полное состояние.

**View**

Пользовательский интерфейс (UI), или то, что приложение показывает, в идеале напрямую выводится из текущего состояния. Они связаны через подписку и отрисовку.

**Поток данных**

Redux следует простому циклу, однонаправленному потоку данных, который согласуется со структурой action-reducer-store.

**Получение действия:**

Элементы UI, например кнопки, порождают действия.

**Роль диспетчера:**

Через store.dispatch(action) эти действия передаются в store.

**Выполнение действия и изменение состояния:**

По заданной логике reducers изменяют состояние и порождают новое дерево состояния.

**Уведомление подписчиков об изменении состояния:**

Подписанные части UI получают сигнал об изменении состояния и обновляются соответственно.

**Действие отрисовки:**

Обновлённое состояние запускает повторную отрисовку представления и приводит UI в соответствие с последним состоянием.

**Применение в разных стеках**

Хотя большинство сотрудников разработало Redux для React, библиотека не привязана к платформе. Адаптеры и соединители позволяют использовать её с рядом библиотек, прежде всего react-redux. Другие интеграции включают Angular, Vue.js и приложения на чистом JavaScript.

**Pluses and minuses:**

**Ключевые выгоды**

- **Большая предсказуемость:** последовательность изменений состояния задана заранее и контролируется.
- **Упрощённое управление состоянием:** каждая часть состояния хранится в одном месте.
- **Более прямой ход выполнения:** reducers задают изменения состояния, и изменения проще проследить. Отладка становится прямее и быстрее.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Можете ли вы describe the three principles that Redux is BASEd upon?</summary>

### Можете ли вы describe the three principles that Redux is BASEd upon?

**Definition:**

Рассмотрим три ключевых принципа, которые составляют основу Redux: единый источник истины, состояние только для чтения и изменения через чистые функции.

**How it works:**

**Единый источник истины**

Состояние всего приложения хранится в одном дереве или объекте внутри Redux store. Так проще понять, где и как состояние меняется, и от этого выше предсказуемость и управляемость.

**Основная выгода**

**Централизованный взгляд на состояние:** в любой момент можно получить полный снимок состояния приложения, и отладка и отслеживание потока данных становятся проще.

**Состояние только для чтения**

В устройстве Redux состояние никогда не меняют напрямую. Нужные изменения делают, отправляя actions, которые фиксируют ожидаемые изменения по пунктам.

**Основные выгоды**

**Ясность и контроль:** запрещая прямые изменения состояния, Redux делает упор на контролируемый и определённый механизм изменения состояния — action creator.

**Отладка с путешествием во времени:** поскольку actions записываются, можно вернуться к конкретным состояниям приложения для отладки.

**Изменения через чистые функции**

Изменения состояния в Redux ведут чистые функции, называемые reducers. Эти функции принимают текущее состояние и action и возвращают новое состояние, не изменяя предыдущее.

**Основные выгоды**

**Предсказуемый результат:** поскольку reducers детерминированы, их результат для любых заданных входных данных и action всегда один и тот же, и это даёт согласованность.

**Тестируемость и сопровождаемость:** чистые функции проще тестировать и понимать, поэтому кодовая база надёжнее и проще в сопровождении.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Что такое action in Redux?</summary>

### Что такое action in Redux?

**Definition:**

Сначала обзор actions в Redux, а затем код.

**How it works:**

**Actions в Redux**

В Redux action заключает в себе «полезную нагрузку» данных, предназначенную изменить состояние. Он также несёт описательный type, который задаёт характер изменения состояния.

Actions отправляют компоненты интерфейса, а обрабатывают reducers — чистые функции, и это обеспечивает предсказуемость.

**Устройство action**

Action в Redux создают как объектный литерал хотя бы с одним обязательным свойством type. При желании можно добавить данные, известные как «payload».

**Соображения**

**Инкапсуляция:** type и payload делают actions описательными и структурированными, и это упрощает чтение кода и отладку.

**Типобезопасность:** TypeScript (или похожий язык) для actions даёт строгую проверку типов в процессе разработки.

Для дополнительной надёжности и единообразия стоит рассмотреть action creators.

**Examples:**

**Типичная структура:**

```js
const myAction = {
  type: 'ADD_TODO',
  payload: {
    id: 1,
    text: 'Buy groceries'
  }
};
```

**Пример кода:** Action

**Вот код на TypeScript:**

```ts
// Defining the Action Interface
interface AddTodoAction {
  type: 'ADD_TODO';
  payload: { id: number; text: string };
}

// Creating an action
const addTodoAction: AddTodoAction = {
  type: 'ADD_TODO',
  payload: { id: 1, text: 'Buy groceries' }
};
```

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Как actions used to change the state in a Redux application?</summary>

### Как actions used to change the state in a Redux application?

**Definition:**

Redux следует строгому однонаправленному потоку данных: actions — единственный способ изменить состояние.

**How it works:**

**Цикл action и состояния**

**Создание action:** actions создают через Action Creators.

**Dispatch:** actions отправляют через store.dispatch(action). Здесь store — единый источник истины в приложении Redux, и он доставляет action в reducers.

**Прослушивание reducer:** reducers, чистые функции, слушают отправленные actions. Если найден подходящий action, reducer выполняет нужные изменения состояния и возвращает новое состояние.

**Обновление store:** обновлённое состояние сохраняется, и связанные компоненты UI получают уведомление для нужных обновлений.

**Что важно помнить**

**Состояние только для чтения:** прямые изменения состояния не допускаются. Вместо этого нужно отправить генератор action.

**Синхронные обновления:** reducers отвечают за синхронные обновления состояния. Любую асинхронную логику (например, вызовы API) обычно обрабатывает middleware вроде Redux Thunk.

**Единый источник истины:** состояние приложения хранится в одном store, и им проще управлять и его проще отлаживать.

**Обработка состояния через reducers**

Reducers, чистые функции, — основа процесса изменения состояния. Получив action и текущее состояние, они решают, как это состояние изменить. Когда они возвращают новое, обновлённое состояние, глобальный store заменяет текущее состояние этим новым.

**Examples:**

**Пример кода:** отправка action

**Вот код на JavaScript:**

```js
// Action Creator
function increment() {
  return { type: 'INCREMENT' };
}

// Dispatching the action
store.dispatch(increment());
```

В примере функция increment — это Action Creator, который создаёт action с type 'INCREMENT'. Метод store.dispatch затем отправляет этот action.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Что такое reducer in Redux and what role does it play?</summary>

### Что такое reducer in Redux and what role does it play?

**Definition:**

Reducer в Redux — чистая функция, которая отвечает за конкретные части состояния приложения. Она фиксирует изменения состояния и вычисляет новое состояние на их основе.

**How it works:**

**Ключевые свойства**

**Предсказуемость:** при одних и тех же входных данных reducer всегда даёт один и тот же результат, и управление состоянием становится предсказуемее.

**Неизменяемость:** reducers обрабатывают состояние неизменяемо, так что каждый переход даёт новый объект состояния. Это помогает избегать побочных эффектов и повышает эффективность библиотек UI и управления состоянием вроде React и их алгоритмов обнаружения изменений.

**Единственная обязанность:** каждый reducer направлен на управление конкретным срезом состояния приложения. Узкая область помогает держать кодовую базу в порядке и упрощает отладку и сопровождение.

**Методы и библиотеки**

Redux даёт служебные функции, такие как combineReducers(), чтобы управлять несколькими reducers, каждый из которых отвечает за свою часть состояния приложения. Библиотеки вроде immer и Redux Toolkit ещё упрощают написание reducers, особенно неизменяемые обновления состояния.

**Практическое применение**

Связь actions с reducers даёт модульный и ориентированный на данные подход к управлению состоянием. Например, нажатие кнопки может отправить action 'increment', который counterReducer обработает и поправит значение count.

**Examples:**

**Пример кода:** Reducer

**Вот код на JavaScript:**

```js
const counterReducer = (state = { count: 0 }, action) => {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
};
```

В этом примере counterReducer обеспечивает неизменяемость: при каждом отправленном action он создаёт и возвращает новый объект состояния.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Как Redux differ from local component state in React?</summary>

### Как Redux differ from local component state in React?

**Definition:**

Рассмотрим ключевые различия между локальным состоянием компонента и Redux при управлении состоянием в React.

**How it works:**

**Зачем глобальное управление состоянием**

**Глобальное управление состоянием:**

- Особенно полезно в больших приложениях, потому что упрощает доступ к состоянию и его обновление между компонентами.
- Упрощает обмен данными между компонентами на разных уровнях иерархии компонентов.

**Плюсы и ограничения локального состояния компонента**

**Плюсы**

**Простота:** локальное состояние внутри компонента быстро и легко завести.

**Локальные обновления:** изменения локального состояния остаются внутри компонента, и о них проще рассуждать и ими управлять.

**Ограничения**

**Дублирование состояния:** держать похожее состояние согласованным в нескольких компонентах может вести к расхождениям и ошибкам.

**Передача состояния и обработчиков событий:** приходится прокидывать состояние и обработчики событий во вложенные и связанные компоненты.

**Иерархический поток данных:** задаёт односторонний поток от родителя к ребёнку, и некоторые схемы из-за этого громоздко реализовать.

**Плюсы глобального управления состоянием с Redux**

**Централизованное состояние:** даёт единый источник истины и защищает от расхождений по всему приложению.

**Предсказуемые обновления состояния:** reducers обеспечивают контролируемые и согласованные изменения состояния.

**Обмен данными:** позволяет делиться данными между компонентами без прокидывания props.

**Отладка с путешествием во времени:** через инструменты вроде Redux DevTools можно вернуться и посмотреть состояние в любой момент жизненного цикла приложения.

**When to use:**

**Когда какой подход выбирать**

**Локальное состояние:** выбирайте локальное управление состоянием для более простого, в основном интерфейсного состояния или для небольших приложений с минимальными требованиями к состоянию и обмену данными.

**Redux (или другое глобальное управление состоянием):** выбирайте глобальную систему управления состоянием в крупных приложениях, при многоуровневых зависимостях состояния или когда по всему приложению критичны широкий обмен данными и согласованность.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Определите "store" in the context of Redux.</summary>

### Определите "store" in the context of Redux.

**Definition:**

В контексте Redux store — ключевой компонент, который действует как центральный узел данных.

**How it works:**

**Составные части store**

**State:** хранит текущее состояние данных, и оно только для чтения. Чтобы обновить его, отправляют actions.

**Reducers:** функции под конкретные actions отвечают за изменение состояния и объединяются через combineReducers().

**Listeners:** зарегистрированные колбэки получают уведомление, когда состояние обновляется.

**Механика store**

getState(): этот метод даёт доступ к текущему состоянию.

dispatch(action): обновления состояния проводят, передавая action, обычно объект со свойством type.

subscribe(listener): слушатели остаются в курсе изменений состояния, если их зарегистрировать.

replaceReducer(nextReducer): переданный reducer используют, чтобы обновить логику обработки состояния.

**Основные принципы store**

**Единый источник истины:** все данные приложения ведутся через один store, и это даёт согласованность.

**Состояние только для чтения:** случайные изменения состояния вне reducers не допускаются, чтобы поведение оставалось детерминированным.

**Настройка store**

Чтобы завести Redux store в приложении, используют функцию createStore() и передают reducers приложения. Затем можно:

Обращаться к инициализированному store через getState(), dispatch() и subscribe().

Динамически обновлять reducers store через replaceReducer(), когда это нужно.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Можете ли вы describe the concept of "single source of truth" in Redux?</summary>

### Можете ли вы describe the concept of "single source of truth" in Redux?

**Definition:**

Redux, предсказуемый контейнер состояния, придерживается принципа единого источника истины. Он устанавливает, что на всё приложение есть одно неизменяемое дерево состояния.

**Pluses and minuses:**

**Ключевые выгоды**

- **Согласованность:** все компоненты отражают последнее состояние, и расхождения данных меньше.
- **Простая отладка:** одна точка наблюдения за изменениями состояния улучшает отладку.
- **Сжатость и ясность:** центральное дерево состояния упрощает управление данными и их видимость.
- **Гарантия корректности:** параллельные обновления невозможны, и стабильность приложения выше.

**Возможные недостатки и решения**

**Последствия для производительности:** компонентам отдают только нужные части состояния, часто через selectors.

**Возможные узкие места:** используют подписки или другие механизмы для эффективного потока данных, учитывая компромиссы.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Как you create a Redux store?</summary>

### Как you create a Redux store?

**Definition:**

Создание Redux store состоит из нескольких ключевых шагов. Store служит центральным узлом управления состоянием во всём приложении.

**Redux Store можно создать любым из трёх способов:**

- Напрямую через createStore(reducer, [preloadedState], [enhancer])
- Через функцию, которая объединяет несколько reducers с помощью combineReducers(reducers)
- С middleware для выполнения логики в потоке данных через applyMiddleware(middleware1, middleware2, ...)

**How it works:**

**Шаги создания Redux Store**

**Определить reducer**

Сначала нужно определить хотя бы один reducer — функцию, которая задаёт форму и переходы состояния store.

Если reducers будет несколько, чтобы вести разные части дерева состояния, их нужно объединить.

**Создать функцию reducer**

**Импортировать дополнительные reducers**

Если reducer не один, их вместе ведут через combineReducers.

**Необязательно:** начальное состояние и middleware

Центральное состояние можно передать как начальное, чтобы reducers забрали свои участки.

Элемент applyMiddleware нужно вызвать с теми middleware, которые используются в приложении.

**Examples:**

**Вот пример кода:**

```js
// counterReducer.js
const initialState = { count: 0 };

const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      return state;
  }
};

export default counterReducer;
```

```js
// reducers/index.js
import { combineReducers } from 'redux';
import counterReducer from './counterReducer';

const reducers = combineReducers({
  counter: counterReducer,
  // Add more reducers here
});

export default reducers;
```

```js
// reducers/index.js
import { combineReducers } from 'redux';
import counterReducer from './counterReducer';
import userReducer from './userReducer';

const reducers = combineReducers({
  counter: counterReducer,
  user: userReducer,
});

export default reducers;
```

```js
// store.js
import { createStore, applyMiddleware, compose } from 'redux';
import reducers from './reducers';

const store = createStore(
  reducers,
  {
    counter: { count: 10 },
    // Additional initial states for more reducers
  },
  compose(applyMiddleware(middleware1, middleware2))
);

export default store;
```

**Полный пример**

**Вот полный пример со всеми шагами:**

```js
// counterReducer.js
const initialState = { count: 0 };

const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      return state;
  }
};

export default counterReducer;

// reducers/index.js
import { combineReducers } from 'redux';
import counterReducer from './counterReducer';
import userReducer from './userReducer';

const reducers = combineReducers({
  counter: counterReducer,
  user: userReducer,
});

export default reducers;

// store.js
import { createStore, applyMiddleware, compose } from 'redux';
import reducers from './reducers';

const store = createStore(
  reducers,
  {
    counter: { count: 10 },
    // Additional initial states for more reducers
  },
  compose(applyMiddleware(middleware1, middleware2))
);

export default store;
```

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Что такое meant by "immutable state," and why is it important in Redux?</summary>

### Что такое meant by "immutable state," and why is it important in Redux?

**Definition:**

**Неизменяемое состояние — ключевое понятие Redux:** состояние приложения нельзя менять напрямую. Изменения состояния идут через actions и reducers, состояние становится предсказуемее, и отладка легче.

**How it works:**

**Как неизменяемость держится в Javascript**

Объявление const в JavaScript не даёт неизменяемости свойств объекта, но гарантирует, что сама ссылка (объект состояния в контексте Redux) остаётся постоянной, и это согласуется с требованием неизменяемости в Redux.

**Способы обеспечить неизменяемость в Redux**

**Оператор расширения:** для поверхностного клонирования объектов состояния

**Глубокое клонирование:** для вложенных объектов и массивов. Однако для больших объектов состояния этот подход может быть неэффективен, и применять его стоит осмотрительно.

**Библиотеки неизменяемости**

Чтобы упростить работу с неизменяемыми данными, можно использовать библиотеки вроде:

**Immer:** даёт минималистичный API для управления неизменяемым состоянием

**Immutable.js:** даёт полную библиотеку структур данных, включая и примитивы, и коллекции. Она мощная, но её бывает трудно встроить в существующие кодовые базы из-за отличий API.

**Pluses and minuses:**

**Преимущества неизменяемого состояния в Redux**

- **История состояния:** сохраняя каждое изменение состояния, можно шагать вперёд и назад во времени, и это помогает отладке и возможностям вроде undo/redo.
- **Оптимизации производительности:** Redux использует проверку равенства ссылок. Если новое и старое состояния совпадают, компоненты избегают лишних повторных отрисовок.
- **Упрощённые обновления компонентов:** при неизменённых объектах компоненты могут не обновляться, пока не изменятся их входные данные.
- **Безопасность потоков:** в многопоточных приложениях неизменяемые объекты гарантируют, что изменения состояния не создают гонок.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Объясните the significance of the combineReducers function.</summary>

### Объясните the significance of the combineReducers function.

**Definition:**

Функция combineReducers в Redux играет центральную роль в устройстве больших веб-приложений.

**How it works:**

Каждый из импортированных reducers — срез общего дерева состояния. Когда отправляют action, он передаётся каждому из этих reducers. Они обрабатывают action, и обновляются соответствующие части дерева состояния.

**Pluses and minuses:**

**Выгоды combineReducers**

- **Модульность кода:** когда reducers разложены по меньшим модулям, ими даже проще делиться и переиспользовать их в разных частях приложения.
- **Удобство и ясность:** вместо одного непомерно большого reducer разработчики могут отдельно вести логически разные части приложения.
- **Выборочная обработка данных:** reducers могут действовать только в заданных функциональных областях и не трогать остальное состояние.
- **Эффективный поток данных:** каждый reducer «слушает» только ту часть состояния, которая ему нужна, и это потенциально улучшает производительность.

**Examples:**

**Пример кода:** combineReducers в деле

**Возьмём приложение для заметок:** у него могут быть три отдельных дерева состояния: notes, ui и user.

**Вот как можно применить combineReducers:**

```js
import { combineReducers } from 'redux';
import notesReducer from './notesReducer';
import uiReducer from './uiReducer';
import userReducer from './userReducer';

const rootReducer = combineReducers({
  notes: notesReducer,
  ui: uiReducer,
  user: userReducer
});

export default rootReducer;
```

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Что такое pure functions and side effects in the context of Redux?</summary>

### Что такое pure functions and side effects in the context of Redux?

**Definition:**

В контексте Redux actions меняют состояние через reducers, а middleware обрабатывают побочные эффекты. Функции reducer должны быть чистыми, чтобы изменения состояния были предсказуемыми.

**How it works:**

**Базовые понятия**

**Actions:** объекты, описывающие изменение состояния.

**Reducers:** чистые функции, задающие изменение состояния.

**State:** неизменяемый объект, доступный по всему приложению.

**Чистые функции**

Reducers должны быть чистыми, то есть их поведение должно быть полностью предсказуемым.

**Определение результата:** результат должен напрямую следовать из входа. Функция не должна основывать результат на внешнем состоянии или данных.

**Без побочных эффектов:** они не должны вызывать изменений вне своей области, например менять глобальные переменные или вызывать функции, которые делают операции ввода-вывода.

**Идемпотентность:** многократный вызов функции с тем же входом должен давать согласованные результаты и не провоцировать побочные эффекты.

**Redux использует принцип неизменяемости:** оказавшись на месте, actions или состояние не подвергаются изменению. Вместо этого они порождают «новое» состояние, которое целиком заменяет существующее.

**Побочные эффекты**

Побочные эффекты — это изменения, которые вызывает функция и которые не сводятся к её возвращаемому значению. Функции с побочными эффектами могут выполнять разные действия сверх своей явной цели. Некоторые побочные эффекты необходимы, но держать их обособленно важно для устойчивости кода и простоты отладки. Управление побочными эффектами в Redux обеспечивают middleware, такие как redux-thunk или redux-saga.

**Методы управления побочными эффектами**

Разные библиотеки именно для Redux, такие как Thunks или Sagas, дают механизмы, чтобы разбирать побочные эффекты структурно.

**Thunks**

Thunks — функции, которые могут порождать другие функции и тем самым допускают действия шире простого dispatch. redux-thunk — устоявшийся middleware, который даёт thunks в Redux store.

**Sagas**

Sagas построены на идее кооперативной конкурентности, чтобы упорядочить actions с побочными эффектами. В store на redux-saga с такими sagas работают через middleware applyMiddleware(sagaMiddleware). Это очень полезно для одновременных операций и сложных взаимных зависимостей, особенно с асинхронными процессами.

**Examples:**

**Пример кода:** нечистый reducer

**Это код на Python:**

```python
# Non-Pure Function

state = 0

def increment_counter():
    global state  # Modifies global state
    state += 1
    return state  # Returns a value different from the input

# Result: Output is unpredictable and changes external state
```

**Пример кода:** Thunk в деле

**Это код на JavaScript:**

```js
// Action Creator
const incrementAsync = () => {
  // Returning a function (a thunk)
  return (dispatch) => {
    // Asynchronous Operation
    setTimeout(() => {
      dispatch({ type: 'INCREMENT' }); // Dispatch a regular action after a delay
    }, 2000);
  };
};
```

**Пример кода:** Saga для проверки ввода

**Вот код на JavaScript:**

```js
import { takeLatest, call, put } from 'redux-saga/effects';
import { START_SUBMIT, SUCCESS_SUBMIT, FAILED_SUBMIT } from './actionTypes';
import { validateInput } from './api';

function* submitForm(action) {
  const {input} = action.payload;
  try {
    yield call(validateInput, input);
    yield put({ type: SUCCESS_SUBMIT });
  } catch (error) {
    yield put({ type: FAILED_SUBMIT, error });
  }
}

export default function* formSaga() {
  yield takeLatest(START_SUBMIT, submitForm);
}
```

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>


### Redux Core Concepts

<details>
<summary>Как you handle asynchronous actions in Redux?</summary>

### Как you handle asynchronous actions in Redux?

**Definition:**

Создатели действий Redux обычно производят немедленные синхронные actions. Чтобы обработать асинхронное поведение, применяют middleware, особенно redux-thunk.

С redux-thunk создатели действий могут возвращать функции вместо простых объектов action.

**How it works:**

**Как работает Redux-Thunk**

**Конфигурация:** при настройке Redux store используют applyMiddleware, чтобы включить redux-thunk.

**Отправка action:** когда создатель действия возвращает функцию, redux-thunk её перехватывает. Этой функции первым параметром передают dispatch.

**Асинхронное управление:** внутри функции можно вручную отправлять actions, часто чтобы обозначить начало и завершение асинхронного потока.

**Examples:**

**Пример кода:** thunk middleware в Redux

**Вот код на JavaScript:**

```js
// configureStore.js
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import rootReducer from './reducers';

const store = createStore(rootReducer, applyMiddleware(thunk));
export default store;

// actions.js
export const fetchData = () => {
  return async (dispatch) => {
    dispatch({ type: 'FETCH_DATA_REQUEST' });
    try {
      const data = await someAPICall();
      dispatch({ type: 'FETCH_DATA_SUCCESS', payload: data });
    } catch (err) {
      dispatch({ type: 'FETCH_DATA_FAILURE', error: err.message });
    }
  };
};

// reducer.js
const initialState = { data: null, loading: false, error: null };
const reducer = (state = initialState, action) => {
  switch (action.type) {
    case 'FETCH_DATA_REQUEST':
      return { ...state, loading: true, error: null };
    case 'FETCH_DATA_SUCCESS':
      return { ...state, data: action.payload, loading: false };
    case 'FETCH_DATA_FAILURE':
      return { ...state, loading: false, error: action.error };
    default:
      return state;
  }
};
```

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Что такое "selector" in Redux and what is its puRPOse?</summary>

### Что такое "selector" in Redux and what is its puRPOse?

**Definition:**

Selector в Redux — чистая функция, которая эффективно вычисляет производные данные из состояния Redux store.

**How it works:**

**Назначение**

**Главное назначение selectors — дать:**

**Преобразование данных:** selectors вычисляют производные данные, например производное состояние или сложные агрегации.

**Оптимизацию производительности:** они помогают избежать дорогих пересчётов и повторных отрисовок в компонентах за счёт кэширования результатов.

**Основные части**

**Срез состояния:** выбирает нужную часть состояния приложения.

**Преобразование данных:** вычисляет и обрабатывает срез состояния, чтобы получить производные данные.

**Механизм кэширования:** эффективно ведёт и обновляет кэш результата selector.

**Роль в архитектуре Redux**

**Источник входа:** selectors получают полный объект состояния из store как вход.

**Централизованный доступ к данным:** они служат основным источником данных для компонентов, подключённых к Redux.

**Эффективность и согласованность:** преобразуя и кэшируя данные состояния, selectors улучшают общую производительность приложения.

**Examples:**

**Пример кода:** selectors в Redux

**Вот код на JavaScript:**

```js
// Example State
const appState = {
  user: {
    name: "John Doe",
    email: "john.doe@example.com",
    age: 30,
    lastLogin: "2023-10-15T08:00:00.000Z",
  },
  products: {
    list: [
      { id: 1, name: "Product 1", price: 100, category: "electronics" },
      { id: 2, name: "Product 2", price: 150, category: "clothing" },
      { id: 3, name: "Product 3", price: 200, category: "electronics" },
    ],
  },
};

// Selectors
const getUser = (state) => state.user;
const getUserEmail = (state) => getUser(state).email;
const getElectronicsProducts = (state) => {
  return state.products.list.filter((product) =>
    product.category === "electronics"
  );
};

// Usage with Selector Functions
console.log(getUserEmail(appState)); // Output: john.doe@example.com
console.log(getElectronicsProducts(appState));
// Output: [ { id: 1, name: "Product 1", price: 100, category: "electronics" }, { id: 3, name: "Product 3", price: 200, category: "electronics" } ]

// Caching Mechanism: Example with Memoization
const getAllProducts = (state) => state.products.list;
const memoizedElectronicsSelector = createSelector(
  [getAllProducts],
  (products) => products.filter((product) => product.category === "electronics")
);

// Add a new product to trigger re-calculation
appState.products.list.push({ id: 4, name: "Product 4", price: 300, category: "electronics" });
// Re-run the memoized selector
console.log(memoizedElectronicsSelector(appState));
// Output: [ { id: 1, name: "Product 1", price: 100, category: "electronics" }, { id: 3, name: "Product 3", price: 200, category: "electronics" }, { id: 4, name: "Product 4", price: 300, category: "electronics" } ]
```

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Как Redux handle the flow of data and actions?</summary>

### Как Redux handle the flow of data and actions?

**Definition:**

В Redux идея однонаправленного потока данных обеспечивает согласованность данных за счёт односторонних каналов для состояния и actions. Рассмотрим подробнее:

**How it works:**

**Ключевые элементы**

**Actions:** это объекты с атрибутом type, они описывают, что в приложении должно измениться. Actions отправляют из компонентов, и их получают reducers.

**Reducers:** это чистые функции, которые задают, как состояние приложения должно преобразоваться в ответ на actions.

**Store:** это единый источник истины. Он держит дерево состояния приложения и отвечает за:

- Изменение состояния через reducers.
- Отправку состояния по мере того, как отправляются actions.

**Односторонний поток данных**

**Отправка action:** компоненты отправляют actions (через store.dispatch(action)).

**Получение reducer:** store передаёт action подходящим reducers.

**Изменения состояния:** reducer изменяет состояние на основе отправленного action.

**Подписка на состояние:** изменённое состояние рассылается подписанным компонентам.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Можете ли вы explain the difference between a container and Presentational component in Redux?</summary>

### Можете ли вы explain the difference between a container and Presentational component in Redux?

**Definition:**

Презентационный компонент получает данные и колбэки пропсами и рисует интерфейс. Контейнер читает хранилище и передаёт эти данные вниз. В руководстве по стилю сказано, что разделение всё ещё верно, а хуки теперь обычный способ читать хранилище.

**How it works:**

- **Презентационный**: Он не импортирует хранилище. Тот же компонент нарисует статичный просмотр, если пропсы передадите вы.
- **Контейнер**: `connect` был старым контейнером: обёртка подписывается и передаёт `stateProps` и `dispatchProps`. В руководстве по стилю сказано, что хуки ведут к вызову `useSelector` и `useDispatch` в компоненте, которому данные нужны.
- **Точечное чтение**: Руководство по стилю говорит подключать больше компонентов. Список выбирает id; каждая строка выбирает свой элемент. Это не один контейнер, который выбирает всё дерево.

**When to use:**

- Компонент только с пропсами берите, когда интерфейс переиспользуют или тестируют без хранилища.
- Вызывайте хуки в компоненте, который показывает данные, когда значение нужно в одном месте.

**Examples:**

```js
function TodoRow({ id }) {
  const todo = useSelector((state) => state.todos.entities[id]);
  const dispatch = useDispatch();
  return <button onClick={() => dispatch(todoToggled(id))}>{todo.text}</button>;
}
```

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [React-Redux hooks](https://react-redux.js.org/api/hooks)
- [connect()](https://react-redux.js.org/api/connect)
</details>

<details>
<summary>Что такое middleware in Redux and when would you use it?</summary>

### Что такое middleware in Redux and when would you use it?

**Definition:**

Промежуточный слой оборачивает `dispatch`. Он видит каждое действие до редьюсеров и может записать его, отложить, изменить или остановить. Побочные эффекты живут здесь, потому что редьюсеры должны оставаться чистыми.

**How it works:**

- **Форма**: Сигнатура в документации `applyMiddleware`: `({ getState, dispatch }) => next => action`. Вызовите `next(action)`, чтобы передать действие дальше.
- **По умолчанию**: `configureStore` добавляет thunk и проверки, которые в разработке ловят мутации и несериализуемые значения.
- **Свой слой**: Передайте `middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(myMiddleware)`. Голый массив отбрасывает значения по умолчанию.

**When to use:**

- Промежуточный слой берите для логов, асинхронной работы и слушателей.
- Не считайте в нём следующее состояние. Это работа редьюсера.

**Examples:**

```js
const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(logger),
});
```

**Sources:**

- [applyMiddleware](https://redux.js.org/api/applyMiddleware)
- [Redux fundamentals — async logic](https://redux.js.org/tutorials/fundamentals/part-6-async-logic)
- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
</details>

<details>
<summary>Как you access the Redux store in a React component?</summary>

### Как you access the Redux store in a React component?

**Definition:**

Оберните дерево в `Provider` с хранилищем. В функциональном компоненте `useSelector` читает состояние, а `useDispatch` возвращает `dispatch`. `useStore` возвращает объект хранилища; в документации хуков для состояния советуют `useSelector`.

**How it works:**

- **Provider**: В учебнике основ сказано, что хуки работают только под `Provider`. Руководство по стилю говорит, что код приложения не должен импортировать хранилище напрямую, кроме крайнего случая.
- **Чтение**: `useSelector(selectCount)` вызывает вашу функцию с состоянием и подписывает компонент.
- **Наследие**: `connect(mapStateToProps)` всё ещё подписывает обёртку. Он помечен устаревшим; `legacy_connect` сохраняет API без предупреждения в редакторе.

**Examples:**

```js
const count = useSelector(selectCount);
const dispatch = useDispatch();
```

**Sources:**

- [React-Redux getting started](https://react-redux.js.org/introduction/getting-started)
- [Hooks API](https://react-redux.js.org/api/hooks)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Что такое the puRPOse of the dispatch function in Redux?</summary>

### Что такое the puRPOse of the dispatch function in Redux?

**Definition:**

`dispatch` — единственный способ попросить хранилище измениться. Вы передаёте действие. Промежуточный слой может его перехватить. Затем корневой редьюсер возвращает следующее состояние, и подписчики выполняются.

**How it works:**

- **Обычные действия**: Обычное действие — простой объект с `type`. `createSlice` собирает таких создателей действий за вас.
- **Другие значения**: Thunk позволяет отправить функцию. Функция получает `dispatch` и `getState`. Редьюсеры эту функцию не видят.
- **Возврат**: В примере логгера `applyMiddleware` возвращается то, что вернул `next(action)`. Для обычного действия это само действие, если поздний слой его не изменил.

**Examples:**

```js
dispatch(increment());
dispatch(fetchUserById(123));
```

**Sources:**

- [createStore](https://redux.js.org/api/createStore)
- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
</details>

<details>
<summary>Какие arguments получает createStore в Redux?</summary>

### Какие arguments получает createStore в Redux?

**Definition:**

`createStore(reducer, preloadedState?, enhancer?)` создаёт хранилище. Redux помечает его устаревшим и просит использовать `configureStore` из Redux Toolkit. `legacy_createStore` — та же функция без пометки.

**How it works:**

- **reducer**: Корневой редьюсер `(state, action) => nextState`.
- **preloadedState**: Необязательное начальное состояние, чтобы гидратировать серверный рендер или восстановить сессию.
- **enhancer**: Необязательный усилитель хранилища. `applyMiddleware(...)` возвращает такой. `configureStore` принимает `reducer`, `preloadedState`, `middleware` и `enhancers` именованными полями.

**Examples:**

```js
const store = configureStore({
  reducer: { posts: postsReducer, users: usersReducer },
  preloadedState,
});
```

**Sources:**

- [createStore](https://redux.js.org/api/createStore)
- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
</details>

<details>
<summary>Опишите the subscribe function in Redux.</summary>

### Опишите the subscribe function in Redux.

**Definition:**

`subscribe` регистрирует слушателя, который выполняется после отправки действия, чтобы интерфейс прочитал новое состояние. React-Redux вызывает его за вас внутри `useSelector` и `connect`.

**How it works:**

- **Без нагрузки**: Слушателю не передают действие. Внутри вызовите `store.getState()`.
- **Кто вызывает**: Руководство по стилю говорит, что компоненты приложения не должны импортировать хранилище. Подпиской владеет `Provider`.
- **Тесты и промежуточный слой**: Тест или не-React-хост может подписаться, отправить действие и проверить `getState()`. У промежуточного слоя уже есть `getState`, и подписка ему не нужна, чтобы увидеть действие.

**Sources:**

- [createStore](https://redux.js.org/api/createStore)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>


### Working with Redux Actions

<details>
<summary>Как you structure action creators in Redux?</summary>

### Как you structure action creators in Redux?

**Definition:**

В Redux Toolkit создатели действий приходят из `createSlice`. Поле `name` слайса становится префиксом каждого `type`, а функция возвращает `{ type, payload }`.

**How it works:**

- **Редьюсеры случаев**: Каждый ключ в `reducers` становится создателем действия в `slice.actions` и обработчиком, который может использовать Immer.
- **События**: Руководство по стилю просит моделировать действия как события (`todoToggled`), а не как сеттеры (`setCompleted`).
- **Логика в редьюсере**: Передайте id в нагрузке. Следующее состояние задачи считайте в редьюсере, чтобы обработчик клика оставался тонким, а логику было легко тестировать.

**Examples:**

```js
const todosSlice = createSlice({
  name: "todos",
  initialState,
  reducers: {
    todoToggled(state, action) {
      const todo = state.find((item) => item.id === action.payload);
      if (todo) todo.completed = !todo.completed;
    },
  },
});
export const { todoToggled } = todosSlice.actions;
```

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Reusing reducer logic](https://redux.js.org/usage/structuring-reducers/reusing-reducer-logic)
</details>

<details>
<summary>Что такое action type and how is it used in Redux?</summary>

### Что такое action type and how is it used in Redux?

**Definition:**

`action.type` — строка, по которой переключается редьюсер. Redux требует только, чтобы она была задана. Типы Toolkit выглядят как `todos/todoToggled`.

**How it works:**

- **Сопоставление**: Редьюсер слайса обрабатывает типы, которые породил сам. `extraReducers` обрабатывает типы других слайсов или `createAsyncThunk`.
- **Префикс**: На странице повторного использования логики показан `counterB/incremented`, потому что имя слайса — `counterB`. Два слайса могут делить функции редьюсера и всё равно не ловить действия друг друга.
- **Имена**: Руководство по стилю предпочитает имя события, а не `setX`. Эта строка видна в журнале действий DevTools.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Reusing reducer logic](https://redux.js.org/usage/structuring-reducers/reusing-reducer-logic)
</details>

<details>
<summary>Можете ли вы give an example of a synchronous action creator?</summary>

### Можете ли вы give an example of a synchronous action creator?

**Definition:**

Синхронный создатель возвращает простой объект действия. Ничего не запрашивается. `createSlice` порождает эту функцию из ключа редьюсера.

**How it works:**

- **Порожденный**: `todoToggled(id)` возвращает `{ type: "todos/todoToggled", payload: id }`.
- **Отправка**: Компонент по клику вызывает `dispatch(todoToggled(id))`. Редьюсер обновляет эту задачу.
- **Написанный вручную**: Можно и самим вернуть `{ type, payload }`. Руководство по стилю говорит, что новый код должен идти через Toolkit, чтобы тип и редьюсер оставались вместе.

**Examples:**

```js
dispatch(todoToggled(id));
```

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Как you handle complex state logic in actions?</summary>

### Как you handle complex state logic in actions?

**Definition:**

Считайте в редьюсере, а не в действии. Руководство по стилю говорит, что редьюсеры легко тестировать и что подготовка следующего состояния снаружи ведёт к случайным мутациям.

**How it works:**

- **Нагрузка маленькая**: Шлите id. Редьюсер находит задачу и переключает `completed` через Immer.
- **Не подменяйте вслепую**: Руководство по стилю предостерегает от `return action.payload`, если нет причины, например формы, где отдельное действие на каждое поле было бы шумом.
- **prepare**: Когда клик должен выдумать id, сведите это к минимуму. Остальное обновление всё равно принадлежит редьюсеру.

**Examples:**

```js
todoToggled(state, action) {
  const todo = state.find((item) => item.id === action.payload);
  if (todo) todo.completed = !todo.completed;
}
```

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Объясните the concept of action creators returning a function for asynchronous operations.</summary>

### Объясните the concept of action creators returning a function for asynchronous operations.

**Definition:**

Thunk — это функция, которую вы отправляете. Промежуточный слой thunk вызывает её с `dispatch` и `getState` и не передаёт функцию редьюсерам. Redux Toolkit добавляет этот слой в `configureStore`.

**How it works:**

- **Зачем функция**: На странице основ показан слой, который при `typeof action === "function"` вызывает `action(dispatch, getState)`. Это и есть thunk.
- **createAsyncThunk**: Учебник essentials называет это обычным способом писать асинхронную логику. `createAsyncThunk` возвращает thunk, который шлёт `pending`, `fulfilled` и `rejected`.
- **Не в редьюсере**: Руководство по стилю запрещает AJAX, таймеры и `Date.now()` внутри редьюсеров. Thunk делает запрос, затем отправляет обычное действие с результатом.

**Examples:**

```js
const fetchUserById = createAsyncThunk("users/fetchByIdStatus", async (userId) => {
  const response = await userAPI.fetchById(userId);
  return response.data;
});
dispatch(fetchUserById(123));
```

**Sources:**

- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
- [Fundamentals — async logic](https://redux.js.org/tutorials/fundamentals/part-6-async-logic)
- [Redux Toolkit usage guide](https://redux-toolkit.js.org/usage/usage-guide)
</details>


### Reducers and State Management

<details>
<summary>Как reducers handle state changes in response to different action types?</summary>

### Как reducers handle state changes in response to different action types?

**Definition:**

Корневой редьюсер вызывает каждый редьюсер слайса с тем же действием. Слайс возвращает новое состояние только для типов, которые он обрабатывает, и текущее состояние для остальных.

**How it works:**

- **createSlice**: Ключи в `reducers` обрабатывают собственные типы слайса. `extraReducers` с `builder.addCase` обрабатывает действия жизненного цикла thunk.
- **Immer**: Присваивание `state.entities.push(...)` внутри `createSlice` идёт через черновик Immer. Настоящее состояние не мутируется. Руководство по стилю говорит, что `configureStore` в разработке проверяет мутации.
- **По умолчанию**: Если ни один случай не совпал, состояние слайса остаётся прежним. Сюда входит первый вызов, который берёт `initialState`, когда аргумент состояния не задан.

**Examples:**

```js
extraReducers: (builder) => {
  builder.addCase(fetchUserById.fulfilled, (state, action) => {
    state.entities.push(action.payload);
  });
}
```

**Sources:**

- [Redux Toolkit usage guide](https://redux-toolkit.js.org/usage/usage-guide)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Что такое meant by a "SLIce reducer"?</summary>

### Что такое meant by a "SLIce reducer"?

**Definition:**

Редьюсер слайса владеет одной веткой дерева состояния, например `state.todos`. `createSlice` строит этот редьюсер и его действия. `configureStore` объединяет слайсы, которые вы передали в `reducer`.

**How it works:**

- **Одна возможность**: Руководство по стилю кладёт `todosSlice.ts` в папку возможности и экспортирует из этого файла редьюсер и создателей действий.
- **Объединение**: Хранилище в getting started — это `reducer: { todos: todosReducer, posts: postsReducer }`. Каждый ключ — одна часть состояния.
- **Не корень**: Слайс не видит соседнее состояние. Реакции между возможностями идут через `extraReducers` или слушателя и используют действия, которые отправил другой слайс.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
</details>

<details>
<summary>Опишите how to properly update the state BASEd on an action.</summary>

### Опишите how to properly update the state BASEd on an action.

**Definition:**

Верните новое значение или присваивайте через Immer внутри `createSlice`. Не мутируйте объект состояния, который редьюсеры делят. Руководство по стилю называет мутацию самой частой ошибкой Redux.

**How it works:**

- **Черновик Immer**: В редьюсере слайса `todo.completed = !todo.completed` меняет черновик. Immer порождает следующее неизменяемое состояние.
- **Копия вручную**: Без Immer копируйте каждого предка, которого меняете: `{ ...state, items: state.items.map(...) }`.
- **Владение**: Читайте поля, которые знаете. Избегайте `return { ...state, ...action.payload }`, кроме обновления формы — это сознательное исключение в руководстве по стилю.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Как бы вы reset the state of a Redux store?</summary>

### Как бы вы reset the state of a Redux store?

**Definition:**

Слайс возвращает свой `initialState` на действие сброса. Чтобы сбросить всё дерево, корневой редьюсер возвращает `undefined` на это действие, и каждый слайс снова стартует со своего начального состояния.

**How it works:**

- **Один слайс**: Редьюсер случая может сделать `return initialState`. Это заменяет ветку.
- **Всё хранилище**: Оберните объединённый редьюсер. На `app/reset` вызовите его как `combined(undefined, action)`, чтобы каждый слайс увидел первый вызов.
- **Не мутируйте**: Присваивать поля обратно по одному легко ошибиться. Замена ветки начальным значением сохраняет форму, которую слайс объявил.

**Examples:**

```js
function rootReducer(state, action) {
  if (action.type === "app/reset") return combined(undefined, action);
  return combined(state, action);
}
```

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [createStore](https://redux.js.org/api/createStore)
</details>

<details>
<summary>Можете ли вы discuss reducer composition and its benefits?</summary>

### Можете ли вы discuss reducer composition and its benefits?

**Definition:**

Композиция значит, что редьюсер вызывает другие редьюсеры. `configureStore` объединяет редьюсеры слайсов. Редьюсер высшего порядка оборачивает один редьюсер, чтобы несколько копий жили в одном хранилище.

**How it works:**

- **Слайсы**: Каждый редьюсер возможности видит только свою ветку и полное действие. Корневое состояние — объект их результатов.
- **Повтор**: На странице повторного использования показан `makeCounterSlice(name)`, чтобы `counterA` и `counterB` делили случаи, но получали разные типы действий.
- **Польза**: Слайс тестируют без остального приложения. Форму состояния одной возможности можно сменить, не переписывая остальные.

**Examples:**

```js
const store = configureStore({
  reducer: {
    counterA: counterA.reducer,
    counterB: counterB.reducer,
  },
});
```

**Sources:**

- [Reusing reducer logic](https://redux.js.org/usage/structuring-reducers/reusing-reducer-logic)
- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
</details>

<details>
<summary>Что такое the significance of immutability in reducer functions?</summary>

### Что такое the significance of immutability in reducer functions?

**Definition:**

Редьюсеры не должны менять существующий объект состояния. `useSelector` и путешествие во времени в DevTools зависят от новых ссылок. Руководство по стилю говорит, что мутация — обычная причина, почему подключённый компонент не перерисовывается.

**How it works:**

- **Проверки**: `configureStore` в разработке включает проверку неизменяемости.
- **Immer**: Руководство по стилю рекомендует Immer, который использует `createSlice`. Присваивания в черновик разрешены, потому что настоящие данные не мутируются.
- **Копии**: Менять копию нормально. Менять аргумент или значение, которое вы замкнули, нельзя.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Как you handle initial state in Redux reducers?</summary>

### Как you handle initial state in Redux reducers?

**Definition:**

Передайте `initialState` в `createSlice`. Слайс использует его при создании хранилища и всякий раз, когда этот редьюсер вызван с `undefined`.

**How it works:**

- **Слайс**: Объект, который вы передаёте, — форма этой ветки: `{ entities: [], loading: "idle" }` в примере руководства.
- **Гидратация**: `preloadedState` у `configureStore` или `createStore` подменяет это начальное значение для страницы с сервера или восстановленной сессии.
- **Адаптер сущностей**: `booksAdapter.getInitialState()` возвращает `{ ids: [], entities: {} }`.

**Examples:**

```js
const usersSlice = createSlice({
  name: "users",
  initialState: { entities: [], loading: "idle" },
  reducers: {},
});
```

**Sources:**

- [Redux Toolkit usage guide](https://redux-toolkit.js.org/usage/usage-guide)
- [createEntityAdapter](https://redux-toolkit.js.org/api/createEntityAdapter)
- [createStore](https://redux.js.org/api/createStore)
</details>

<details>
<summary>Что такое common pitfalls when writing reducers?</summary>

### Что такое common pitfalls when writing reducers?

**Definition:**

Мутация состояния, ввод-вывод и доверие к тому, что `action.payload` уже является следующим состоянием, — это сбои, которые называет руководство по стилю.

**How it works:**

- **Мутация**: Добавление в настоящий массив ломает DevTools и может пропустить отрисовку. Используйте слайс, чтобы Immer копировал за вас, и оставьте проверки разработки.
- **Побочные эффекты**: Никакого `fetch`, таймеров или `Math.random()` в редьюсере. Это делает thunk или слушатель, а затем отправляет обычное действие.
- **Слепая запись**: `return action.payload` сохранит всё, что отправили, включая объект не того вида. Читайте поля, которые ожидаете.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>


### Redux Middleware and Side Effects

<details>
<summary>Что такое Redux-thunk and how does it work?</summary>

### Что такое Redux-thunk and how does it work?

**Definition:**

redux-thunk учит `dispatch` принимать функцию. `configureStore` включает его по умолчанию. Функция может сделать `await` запроса и затем отправить обычные действия.

**How it works:**

- **Вызов**: Если отправленное значение — функция, слой вызывает её с `dispatch` и `getState` и не вызывает редьюсеры с этой функцией.
- **Помощник Toolkit**: `createAsyncThunk("users/fetchByIdStatus", async (userId) => ...)` шлёт pending, затем fulfilled с возвращённым значением или rejected, если обещание упало.
- **Редьюсер**: Обрабатывайте `fetchUserById.fulfilled` в `extraReducers`. Редьюсер по-прежнему видит только обычные действия.

**Examples:**

```js
dispatch(fetchUserById(123));
```

**Sources:**

- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
- [Fundamentals — async logic](https://redux.js.org/tutorials/fundamentals/part-6-async-logic)
</details>

<details>
<summary>Можете ли вы explain the puRPOse of Redux-Saga and how it compares to Redux-thunk?</summary>

### Можете ли вы explain the puRPOse of Redux-Saga and how it compares to Redux-thunk?

**Definition:**

redux-saga запускает функции-генераторы в промежуточном слое в ответ на действия. redux-thunk запускает функцию, которую вы отправили. Учебник essentials говорит, что thunk — обычный подход, и `configureStore` ставит thunk сам.

**How it works:**

- **Saga**: На странице начала используют `takeEvery` или `takeLatest`, затем `yield call(Api.fetchUser, id)` и `yield put({ type, user })`. `takeLatest` отменяет более ранний ещё идущий запрос.
- **Подключение**: `createSagaMiddleware()`, `getDefaultMiddleware().concat(sagaMiddleware)`, затем `sagaMiddleware.run(mySaga)`.
- **Thunk**: Thunk — асинхронная функция, замкнутая на работу. `takeLatest` нет, пока вы его не добавите. RTK Query — то, что учебник essentials рекомендует, когда работа — это загрузка данных, а не длинный процесс.

**When to use:**

- Для запросов берите thunk или RTK Query.
- Saga берите, когда нужно отменять или выстраивать долгие процессы и зависимость от redux-saga уже есть.

**Examples:**

```js
const sagaMiddleware = createSagaMiddleware();
const store = configureStore({
  reducer,
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(sagaMiddleware),
});
sagaMiddleware.run(mySaga);
```

**Sources:**

- [redux-saga getting started](https://redux-saga.js.org/docs/introduction/GettingStarted)
- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
</details>

<details>
<summary>Что такое "side effect" and how do middlewares handle them in Redux?</summary>

### Что такое "side effect" and how do middlewares handle them in Redux?

**Definition:**

Побочный эффект — работа, видимая вне возвращаемого значения: HTTP, таймеры, логи, случайные id. Редьюсеры не должны этого делать. Промежуточный слой выполняет работу, затем отправляет обычные действия.

**How it works:**

- **Примеры**: Страница основ перечисляет логи, файлы, таймеры, HTTP, мутацию вне состояния и `Date.now()` или `Math.random()`.
- **Где**: Thunk, saga или `createListenerMiddleware` могут сделать `fetch` и затем `dispatch`. В документации слушателя сказано делать `prepend`, чтобы он шёл до проверки сериализуемости, потому что действия слушателя могут содержать функции.
- **Правило редьюсера**: Путешествие во времени перезапускает редьюсеры. Эффект внутри одного повторился бы при отладке.

**Sources:**

- [Fundamentals — async logic](https://redux.js.org/tutorials/fundamentals/part-6-async-logic)
- [createListenerMiddleware](https://redux-toolkit.js.org/api/createListenerMiddleware)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Как you write a custom middleware for Redux?</summary>

### Как you write a custom middleware for Redux?

**Definition:**

Напишите `({ getState, dispatch }) => (next) => (action) => { ... }` и верните результат `next(action)`, если вы не собирались остановить действие.

**How it works:**

- **Цепочка**: В документации `applyMiddleware` сказано, что каждый слой получает API хранилища и возвращает функцию от `next`. Последний `next` — настоящий `dispatch` хранилища.
- **Логгер**: Описанный логгер печатает действие, вызывает `next(action)`, затем печатает `getState()`.
- **Установка**: В Toolkit — `getDefaultMiddleware().concat(logger)`. Не передавайте список без вызова `getDefaultMiddleware`, если вам всё ещё нужны thunk и проверки.

**Examples:**

```js
function logger({ getState }) {
  return (next) => (action) => {
    console.log("will dispatch", action);
    const result = next(action);
    console.log("state after dispatch", getState());
    return result;
  };
}
```

**Sources:**

- [applyMiddleware](https://redux.js.org/api/applyMiddleware)
</details>

<details>
<summary>Обсудите how middlewares can enhance the CAPabilities of a Redux store.</summary>

### Обсудите how middlewares can enhance the CAPabilities of a Redux store.

**Definition:**

Промежуточный слой — это как хранилище получает возможности, не кладя их в редьюсеры: асинхронные функции, проверки в разработке, слушатели и журнал каждого действия.

**How it works:**

- **Встроенное**: `configureStore` включает DevTools и добавляет thunk плюс проверки неизменяемости и сериализуемости.
- **Асинхронность**: Thunk и saga позже отправляют обычные действия. Промежуточный слой RTK Query, добавленный через `.concat(api.middleware)`, следит за временем жизни кэша.
- **Слушатели**: `startListening` запускает эффект, когда совпал создатель действия, и может `dispatch`, `delay` и `fork` из API слушателя.

**Sources:**

- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
- [createListenerMiddleware](https://redux-toolkit.js.org/api/createListenerMiddleware)
</details>


### Redux and UI Integration

<details>
<summary>Как you connect a React component to a Redux store?</summary>

### Как you connect a React component to a Redux store?

**Definition:**

Нарисуйте `Provider store={store}` в корне. Затем вызывайте `useSelector` и `useDispatch` в компоненте. `connect` всё ещё работает и в React-Redux 9 помечен устаревшим.

**How it works:**

- **Хуки**: Счётчик в getting started читает `selectCount` и отправляет `increment` из обработчика клика.
- **connect**: `connect(mapStateToProps, mapDispatchToProps)(Component)` возвращает обёртку. Компонент, который вы передали, он не меняет.
- **Одно хранилище**: Руководство по стилю говорит, что у приложения должно быть одно хранилище, переданное через `Provider`.

**Examples:**

```js
export function Counter() {
  const count = useSelector(selectCount);
  const dispatch = useDispatch();
  return <button onClick={() => dispatch(increment())}>{count}</button>;
}
```

**Sources:**

- [React-Redux getting started](https://react-redux.js.org/introduction/getting-started)
- [connect()](https://react-redux.js.org/api/connect)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Что такое the advantages of using React-Redux's connect function?</summary>

### Что такое the advantages of using React-Redux's connect function?

**Definition:**

connect подписывает обёртку и передаёт данные хранилища пропсами, поэтому внутренний компонент может не знать о Redux. Текущая документация помечает его устаревшим и рекомендует `useSelector` и `useDispatch`.

**How it works:**

- **Граница пропсов**: Руководство по стилю говорит, что обёртка была полезна для презентационных компонентов и для их тестов с обычными пропсами.
- **Цена**: Та же страница говорит, что `connect` трудно проследить и трудно типизировать из-за `mapState`, `mapDispatch` и слияния пропсов. Хуки принимают один селектор.
- **Если оставляете**: Импортируйте `legacy_connect as connect`, чтобы убрать зачёркивание. Если всё ещё используете его, предпочитайте объектную форму `mapDispatch`.

**When to use:**

- Оставляйте `connect`, когда классовый компонент или существующая обёртка ещё не могут перейти на хуки.
- В новых функциональных компонентах используйте хуки.

**Sources:**

- [connect()](https://react-redux.js.org/api/connect)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Можете ли вы describe the MapStateToProps and MapDispatchToProps functions?</summary>

### Можете ли вы describe the MapStateToProps and MapDispatchToProps functions?

**Definition:**

Это первые два аргумента `connect`. `mapStateToProps(state, ownProps?)` возвращает пропсы из состояния. `mapDispatchToProps` возвращает колбэки, которые отправляют действия, или объект создателей действий.

**How it works:**

- **Состояние**: Функция с одним аргументом выполняется, когда меняется хранилище. Функция с двумя аргументами выполняется ещё и когда меняются собственные пропсы обёртки, и получает их как `ownProps`.
- **Отправка**: Если его опустить, компонент получает `dispatch`. Руководство по стилю говорит передавать объект создателей действий, а не функцию, если вы всё ещё используете `connect`.
- **Перерисовка**: В документации сказано, что возвращённые пропсы состояния решают, перерисуется ли обёрнутый компонент.

**Examples:**

```js
connect(
  (state, ownProps) => ({ todo: state.todos[ownProps.id] }),
  { todoToggled },
)(TodoRow);
```

**Sources:**

- [connect()](https://react-redux.js.org/api/connect)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Объясните the Provider component from React-Redux.</summary>

### Объясните the Provider component from React-Redux.

**Definition:**

Provider кладёт хранилище в контекст React. `useSelector`, `useDispatch` и `connect` читают это хранилище. Без него они не видят результат `configureStore`.

**How it works:**

- **Один раз**: Руководство по стилю говорит: одно хранилище на приложение, обычно в `store.js`, переданное в `Provider` в корне.
- **Хуки**: Учебник основ вводит `Provider` до `useSelector` и `useDispatch`.
- **RTK Query**: В обзоре упомянут `ApiProvider`, только если хранилища ещё нет. Обычное приложение использует существующий `Provider` и добавляет редьюсер и промежуточный слой API в это хранилище.

**Examples:**

```jsx
root.render(
  <Provider store={store}>
    <App />
  </Provider>,
);
```

**Sources:**

- [React-Redux getting started](https://react-redux.js.org/introduction/getting-started)
- [Redux style guide](https://redux.js.org/style-guide/)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
</details>

<details>
<summary>Как you use hooks like useDispatch and useSelector in React functional components?</summary>

### Как you use hooks like useDispatch and useSelector in React functional components?

**Definition:**

Вызовите `useSelector` с функцией от состояния к значению. Один раз вызовите `useDispatch`, чтобы получить `dispatch`. Оба требуют `Provider`.

**How it works:**

- **Выбор**: По возможности возвращайте примитив или устойчивую ссылку. Если возвращаете новый объект, передайте `shallowEqual`, иначе компонент рисуется на каждую отправку.
- **Несколько вызовов**: Руководство по стилю говорит вызывать `useSelector` много раз для маленьких значений, а не один селектор, который возвращает большой объект.
- **Отправка**: `dispatch(increment())` в обработчике клика. Для запроса отправьте thunk из `createAsyncThunk`.

**Examples:**

```js
const count = useSelector(selectCount);
const dispatch = useDispatch();
dispatch(increment());
```

**Sources:**

- [Hooks API](https://react-redux.js.org/api/hooks)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Какие performance considerations учитывать при connecting components к Redux?</summary>

### Какие performance considerations учитывать при connecting components к Redux?

**Definition:**

Компонент перерисовывается, когда выбранное значение меняется по проверке равенства. Выбор большого нового объекта или выбор высоко в дереве заставляет рисовать больше интерфейса, чем требует изменение.

**How it works:**

- **Много подписчиков**: Пример из руководства по стилю: список выбирает id пользователей, а каждый `UserRow` выбирает одного пользователя. Обновление одного пользователя не обязано рисовать каждую строку.
- **Равенство**: `useSelector` использует строгое равенство, если вы не передали `shallowEqual`. Селектор, который возвращает `{ a, b }`, каждый раз даёт новый объект.
- **Нормализованное состояние**: Страница нормализации говорит, что вложенное обновление копирует предков и будит каждый селектор, который их читал. Поиск по id трогает меньший кусок.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Hooks API](https://react-redux.js.org/api/hooks)
- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
</details>

<details>
<summary>Как you optimize the performance of Redux-connected components?</summary>

### Как you optimize the performance of Redux-connected components?

**Definition:**

Выбирайте наименьшее значение, которое можете, в компоненте, который его рисует. Производные данные мемоизируйте через `createSelector`, который Redux Toolkit реэкспортирует.

**How it works:**

- **Где**: Руководство по стилю говорит, что больше подписок лучше, чем один родитель, который выбирает всю коллекцию и передаёт её вниз.
- **Равенство**: Передавайте `shallowEqual` из `react-redux`, когда селектор обязан вернуть объект или массив.
- **Производные данные**: Руководство по стилю говорит не хранить вычисленные значения в хранилище и мемоизировать их через Reselect, чтобы фильтр списка не считался на посторонних действиях.

**Examples:**

```js
const selected = useSelector(selectorReturningObject, shallowEqual);
```

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Hooks API](https://react-redux.js.org/api/hooks)
</details>


### Advanced Redux Patterns

<details>
<summary>Что такое Higher Order Reducers (HORs) and how are they used?</summary>

### Что такое Higher Order Reducers (HORs) and how are they used?

**Definition:**

Редьюсер высшего порядка принимает редьюсер и возвращает редьюсер. `combineReducers` — один из них. Схему используют, чтобы одну логику применить ко многим ключам состояния.

**How it works:**

- **Фильтр**: В документации показаны `createNamedWrapperReducer` и `createFilteredReducer`: они пропускают внутренний редьюсер, если действие не для этой копии.
- **Toolkit**: `makeCounterSlice(name)` вызывает `createSlice` с этим именем, поэтому порождённые типы — `counterB/incremented` и не обновляют `counterA`.
- **Общие случаи**: Определите функции случаев один раз и передайте их в каждый `createSlice`, если несколько слайсов должны делить поведение и сохранить свои типы.

**Examples:**

```js
function makeCounterSlice(name) {
  return createSlice({
    name,
    initialState: 0,
    reducers: { incremented: (state) => state + 1 },
  });
}
```

**Sources:**

- [Reusing reducer logic](https://redux.js.org/usage/structuring-reducers/reusing-reducer-logic)
</details>

<details>
<summary>Можете ли вы implement a feature toggle mechanism using Redux?</summary>

### Можете ли вы implement a feature toggle mechanism using Redux?

**Definition:**

Храните флаг в слайсе и читайте его через `useSelector`. Переключение — обычное действие. Булевым значением владеет редьюсер, чтобы DevTools показал изменение.

**How it works:**

- **Состояние**: Держите `{ flags: { newCheckout: false } }` в сериализуемых простых данных, как требует руководство по стилю.
- **Обновление**: Событие вроде `flagToggled` получает имя флага и переключает это поле внутри слайса.
- **Интерфейс**: Компонент выбирает `state.flags.newCheckout` и решает, какого потомка рисовать. Не читайте хранилище из случайного модуля.

**Examples:**

```js
flagToggled(state, action) {
  const name = action.payload;
  state.flags[name] = !state.flags[name];
}
```

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Hooks API](https://react-redux.js.org/api/hooks)
</details>

<details>
<summary>Как you normalize a state SHApe in Redux?</summary>

### Как you normalize a state SHApe in Redux?

**Definition:**

Храните каждый тип сущности как поиск по id плюс массив id. На странице нормализации сказано, что `createEntityAdapter` использует для этого `{ ids, entities }`.

**How it works:**

- **Форма**: Элементы живут один раз, под своим id. Другие записи хранят id, а не вложенные копии.
- **Адаптер**: `createEntityAdapter` даёт `getInitialState`, `addOne`, `setAll` и селекторы вроде `selectAll`.
- **Вложенные ответы**: На странице сказано, что API часто возвращают вложенные данные. Нормализуйте их до хранилища. Normalizr умеет это преобразование; его больше не сопровождают активно.

**Examples:**

```js
const booksAdapter = createEntityAdapter({
  selectId: (book) => book.bookId,
});
bookAdded: booksAdapter.addOne,
```

**Sources:**

- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
- [createEntityAdapter](https://redux-toolkit.js.org/api/createEntityAdapter)
</details>

<details>
<summary>Что такое Redux "SLIce" and how is it used?</summary>

### Что такое Redux "SLIce" and how is it used?

**Definition:**

Слайс — это редьюсер, действия и начальное состояние одной возможности, созданные через `createSlice` и повешенные на хранилище под одним ключом.

**How it works:**

- **Файл**: Руководство по стилю называет один файл слайса схемой ducks и предпочитает её отдельным папкам `actions/` и `reducers/`.
- **Хранилище**: `configureStore({ reducer: { todos: todosReducer } })` запускает этот редьюсер для `state.todos`.
- **Экспорт**: Экспортируйте `todosSlice.reducer` и `todosSlice.actions`. Компоненты отправляют действия и выбирают из этой ветки.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
</details>

<details>
<summary>Объясните the "Ducks" pattern in Redux.</summary>

### Объясните the "Ducks" pattern in Redux.

**Definition:**

Ducks значит, что один модуль владеет типами действий, создателями действий и редьюсером возможности. В текущем Redux этот модуль — файл `createSlice`.

**How it works:**

- **Рядом**: Дерево в руководстве по стилю — `features/todos/todosSlice.ts` рядом с `Todos.tsx`, а не глобальная папка действий.
- **Префикс**: Имя слайса — префикс типов. Ту же изоляцию старые файлы ducks получали рукописными константами.
- **Папка приложения**: `app/store.ts` только настраивает хранилище. Редьюсеров возможностей в нём нет.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>


### Redux State Normalization

<details>
<summary>Почему is state normalization recommended in Redux?</summary>

### Почему is state normalization recommended in Redux?

**Definition:**

Вложенные копии устаревают, требуют глубокого кода редьюсера и меняют ссылки предков, из-за чего посторонние вызовы `useSelector` перерисовываются.

**How it works:**

- **Одно место**: Страница нормализации говорит, что элемент, заданный один раз, обновляется один раз.
- **Проще обновления**: Поиск по id заменяет глубокий неизменяемый путь.
- **Отрисовка**: Смена комментария должна копировать запись комментария, а не каждый пост, который его показывал.

**Sources:**

- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
</details>

<details>
<summary>Как you structure a normalized state?</summary>

### Как you structure a normalized state?

**Definition:**

У каждого типа сущности есть `ids` и `entities`, или учебные имена со страницы `allIds` и `byId`. Связанные таблицы могут лежать под ключом `entities` рядом с состоянием `ui`.

**How it works:**

- **Поиск**: `entities[id]` — запись. `ids` — порядок.
- **Связи**: Пост хранит `author` и `comments` как id. Объект автора не вложен в каждый пост.
- **Адаптер**: `getInitialState()` начинает с `{ ids: [], entities: {} }`. `selectId` говорит адаптеру, какое поле является id, если это не `id`.

**Sources:**

- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
- [createEntityAdapter](https://redux-toolkit.js.org/api/createEntityAdapter)
</details>

<details>
<summary>Что такое the benefits of state denormalization?</summary>

### Что такое the benefits of state denormalization?

**Definition:**

Денормализация значит собрать вложенный объект, который хочет интерфейс, в момент чтения. Хранилище остаётся плоским; селектор соединяет id с записями.

**How it works:**

- **Модель чтения**: Экран поста нуждается в имени автора. Селектор читает `users.entities[post.author]`, вместо того чтобы хранить это имя на каждом посте.
- **Мемоизация**: Руководство по стилю говорит мемоизировать это соединение через `createSelector`, чтобы оно считалось, когда меняется пост или автор, а не на каждое действие.
- **Модель записи**: Обновления всё ещё бьют в одну таблицу. Вы не ищете по дереву каждую копию пользователя.

**Sources:**

- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Можете ли вы explain how to manage entity relations in a normalized Redux state?</summary>

### Можете ли вы explain how to manage entity relations in a normalized Redux state?

**Definition:**

Храните id на сущности, когда всегда идёте в одну сторону. Используйте таблицу соединения, когда связь многие-ко-многим или у неё есть свои поля.

**How it works:**

- **Один ко многим**: Автор на странице хранит `bookIds`. Книги живут в `books.byId`.
- **Соединение**: `authorBook` хранит `authorId`, `bookId` и `role`. Поиск идёт по этой таблице.
- **Индекс позже**: Добавляйте `bookIdsByAuthorId`, только если измеренное чтение медленное, и обновляйте индекс теми же действиями, которые меняют строки соединения.

**Sources:**

- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
</details>


### Redux Ecosystem and Utilities

<details>
<summary>Что такое immutable.js and how can it be used with Redux?</summary>

### Что такое immutable.js and how can it be used with Redux?

**Definition:**

Immutable.js — библиотека устойчивых коллекций. Рекомендация руководства по стилю для Redux — Immer внутри `createSlice` и простые объекты в хранилище.

**How it works:**

- **Сериализуемое**: Руководство по стилю говорит не класть Map, Set или экземпляры классов в состояние или действия. DevTools и интерфейс зависят от простых данных.
- **Immer**: `createSlice` уже позволяет писать мутирующий синтаксис по черновику. Оборачивать хранилище в другой тип коллекций не нужно.
- **Если оно уже есть**: Преобразуйте в простые объекты на границе до `dispatch`. Не выбирайте структуру Immutable из нового слайса.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Обсудите the Redux DevTools extension and its features.</summary>

### Обсудите the Redux DevTools extension and its features.

**Definition:**

DevTools записывает действия и состояние, чтобы можно было смотреть журнал и двигаться по нему. `configureStore` включает это. Оно работает, когда состояние и действия остаются сериализуемыми, а редьюсеры — чистыми.

**How it works:**

- **Настройка**: На странице getting started сказано, что `configureStore` включает расширение Redux DevTools. Усилитель вручную подключать не нужно.
- **Путешествие во времени**: Руководство по стилю говорит, что DevTools заново запускает редьюсеры от ранних действий. Побочный эффект в редьюсере повторился бы. Мутация делает историю лживой.
- **Что видно**: В этом журнале видны типы действий слайсов и `createAsyncThunk` (`pending`, `fulfilled`, `rejected`).

**Sources:**

- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Как reselect and memoized selectors work in Redux?</summary>

### Как reselect and memoized selectors work in Redux?

**Definition:**

Мемоизированный селектор кэширует результат и пересчитывает его, когда меняются входы. Redux Toolkit реэкспортирует `createSelector` из Reselect для производных данных.

**How it works:**

- **Зачем**: Руководство по стилю говорит, что вывод данных в селекторах делает хранилище меньше и убирает лишние поля, которые нужно синхронизировать.
- **Входы**: Входные селекторы читают слайсы. Функция результата их соединяет. Если ссылки входов те же, возвращается кэш.
- **Селекторы сущностей**: `adapter.getSelectors()` возвращает мемоизированные селекторы для `selectAll` и других поисков, в области функции, которую вы передали.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [createEntityAdapter](https://redux-toolkit.js.org/api/createEntityAdapter)
</details>

<details>
<summary>Что такое common utilities and libraries used in conjunction with Redux?</summary>

### Что такое common utilities and libraries used in conjunction with Redux?

**Definition:**

Redux Toolkit — набор, который руководство по стилю просит использовать: `configureStore`, `createSlice`, Immer, thunk и Reselect. RTK Query закрывает загрузку. React-Redux соединяет React.

**How it works:**

- **Ядро**: Хранилище всё ещё Redux. Toolkit его настраивает и пишет редьюсеры.
- **Интерфейс**: `Provider`, `useSelector` и `useDispatch` — это React-Redux. `connect` остаётся для старых компонентов.
- **Данные**: `createAsyncThunk` для жизненного цикла одного запроса. `createApi`, когда нужны ключи кэша, флаги загрузки и хуки, порождённые из конечных точек.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
- [React-Redux getting started](https://react-redux.js.org/introduction/getting-started)
</details>

<details>
<summary>Опишите the significance of the Redux Toolkit and how it helps Redux development.</summary>

### Опишите the significance of the Redux Toolkit and how it helps Redux development.

**Definition:**

Redux Toolkit — поддерживаемый способ писать Redux. Он объединяет редьюсеры, добавляет thunk и DevTools и использует Immer, чтобы неизменяемые обновления не копировать руками.

**How it works:**

- **Хранилище**: `configureStore({ reducer: { todos, posts } })` заменяет ручные `combineReducers`, `applyMiddleware` и настройку DevTools.
- **Логика**: `createSlice` порождает типы и создателей действий и позволяет редьюсерам присваивать в черновик.
- **Не обязан**: Руководство по стилю говорит, что RTK можно не использовать, и что именно он даёт рекомендуемые значения по умолчанию.

**Examples:**

```js
const store = configureStore({
  reducer: { todos: todosReducer, posts: postsReducer },
});
```

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
</details>


### Testing Redux Applications

<details>
<summary>Как you test Redux reducers?</summary>

### Как you test Redux reducers?

**Definition:**

Вызовите редьюсер с состоянием и действием и проверьте результат. Руководство по стилю говорит, что поэтому логика должна жить в редьюсере.

**How it works:**

- **Чистая функция**: `const result = reducer(testState, action)` не нуждается в хранилище, DOM и сети.
- **Слайс**: Отправьте результат порождённого создателя: `todosReducer(initialState, todoToggled(id))`.
- **Immer**: Проверяйте возвращённое состояние. Не проверяйте, что входной объект изменился; проверка в настоящем хранилище провалила бы реальную мутацию.

**Examples:**

```js
const next = todosReducer(initialState, todoToggled("1"));
expect(next.entities["1"].completed).toBe(true);
```

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Что такое the recommended approach to test Redux actions?</summary>

### Что такое the recommended approach to test Redux actions?

**Definition:**

Проверяйте простой объект, который возвращает синхронный создатель. Для thunk запустите его с поддельными `dispatch` и `getState` или отправьте через настоящее `configureStore` и проверьте действия, которые хранилище записало.

**How it works:**

- **Синхронно**: `todoToggled("1")` должен быть равен `{ type: "todos/todoToggled", payload: "1" }`.
- **Thunk**: Страница essentials говорит, что `createAsyncThunk` шлёт pending, затем fulfilled или rejected. Проверьте эти типы и нагрузку.
- **Лучше тесты редьюсера**: Руководство по стилю говорит, что важное поведение — следующее состояние, и тест редьюсера его уже покрывает.

**Sources:**

- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Как you test a Redux-connected React component?</summary>

### Как you test a Redux-connected React component?

**Definition:**

Нарисуйте его под `Provider` с хранилищем, у которого `preloadedState` — нужный случай. Проверьте текст, затем кликните и проверьте хранилище или следующую отрисовку.

**How it works:**

- **Хранилище**: `configureStore` принимает `preloadedState`, тот же параметр, которым `createStore` гидратирует состояние.
- **Хуки**: Компонент вызывает `useSelector` и `useDispatch`, как в бою, поэтому тесту нужен тот же `Provider`.
- **Презентационный**: Руководство по стилю отмечает, что потомка только с пропсами можно нарисовать без хранилища. Так делают, когда тест про разметку, а не про подписку.

**Examples:**

```jsx
render(
  <Provider store={store}>
    <Counter />
  </Provider>,
);
```

**Sources:**

- [createStore](https://redux.js.org/api/createStore)
- [React-Redux getting started](https://react-redux.js.org/introduction/getting-started)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Опишите the process of testing asynchronous action creators.</summary>

### Опишите the process of testing asynchronous action creators.

**Definition:**

Отправьте thunk в тестовое хранилище и дождитесь, пока обработается действие fulfilled или rejected. Подмените HTTP-клиент, который вызывает функция нагрузки.

**How it works:**

- **Жизненный цикл**: `createAsyncThunk` шлёт `pending`, затем `fulfilled` с возвращёнными данными или `rejected` при ошибке. В руководстве обработчик fulfilled показан в `extraReducers`.
- **Проверка состояния**: Когда обещание завершилось, `store.getState()` должен показать загруженную сущность или поле ошибки. Так вместе проверяются редьюсер и thunk.
- **Без таймеров в редьюсерах**: Подменяйте API на границе. Не кладите часы внутрь слайса.

**Sources:**

- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
- [Redux Toolkit usage guide](https://redux-toolkit.js.org/usage/usage-guide)
</details>

<details>
<summary>Какие инструменты can you use to facilitate testing in Redux applications?</summary>

### Какие инструменты can you use to facilitate testing in Redux applications?

**Definition:**

Используйте хранилище из `configureStore`, редьюсер как простую функцию и DevTools во время разработки. Поведение RTK Query видно как действия и состояние кэша в этом хранилище.

**How it works:**

- **Модуль**: Тестам редьюсера не нужна библиотека кроме проверки. Передайте `todoToggled(id)`.
- **Связка**: `Provider` плюс средство тестов интерфейса рисует подключённый компонент. `preloadedState` задаёт фикстуру.
- **DevTools**: `configureStore` включает расширение, чтобы сбойный поток можно было разобрать как список действий.

**Sources:**

- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>


### Redux with Other Frameworks and Libraries

<details>
<summary>Как you integrate Redux with a framework other than React, like Angular or Vue?</summary>

### Как you integrate Redux with a framework other than React, like Angular or Vue?

**Definition:**

Хранилище не зависит от React. Любой интерфейс может вызывать `getState`, `dispatch` и `subscribe`. React-Redux — только привязка к React.

**How it works:**

- **Хранилище**: `configureStore` возвращает это хранилище. Руководство по стилю говорит: одно хранилище, и код интерфейса не должен его импортировать, кроме границы.
- **Подписка**: `createStore` описывает `subscribe`, чтобы вид обновился после `dispatch`. Адаптер Vue или Angular делает эту подписку за вас; слайсы вы пишете так же.
- **RTK Query**: В обзоре сказано, что ядро не зависит от интерфейса и что есть примеры для других библиотек интерфейса. Хуки React идут из `@reduxjs/toolkit/query/react`.

**Sources:**

- [createStore](https://redux.js.org/api/createStore)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Можно ли Redux be used with server-side rendering frameworks like Next.js?</summary>

### Можно ли Redux be used with server-side rendering frameworks like Next.js?

**Definition:**

Да. Рисуйте на сервере с хранилищем, сериализуйте `getState()` и передайте этот объект как `preloadedState`, когда создаёте хранилище в браузере.

**How it works:**

- **preloadedState**: И `createStore`, и `configureStore` принимают его, чтобы универсальное приложение гидратировалось с сервера.
- **Один запрос, одно хранилище**: На сервере создавайте хранилище на запрос. Не делите одно хранилище уровня модуля между пользователями.
- **Простые данные**: Правило сериализуемого состояния из руководства по стилю делает `getState()` безопасным для вставки в HTML.

**Examples:**

```js
const store = configureStore({ reducer, preloadedState });
```

**Sources:**

- [createStore](https://redux.js.org/api/createStore)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Обсудите the use of Redux with React Native.</summary>

### Обсудите the use of Redux with React Native.

**Definition:**

React Native рисует через React, поэтому те же `Provider`, `useSelector` и `useDispatch`. Хранилище всё так же создаётся через `configureStore`.

**How it works:**

- **Привязка интерфейса**: Хуки из getting started React-Redux не зависят от DOM. Они зависят от `Provider`.
- **Сохранение**: Перезагрузка приложения стирает память. Сохраните сериализуемое состояние сами и верните его как `preloadedState`, если продукту это нужно.
- **Эффекты**: Вызовы нативных API остаются в thunk или слушателях, не в редьюсерах.

**Sources:**

- [React-Redux getting started](https://react-redux.js.org/introduction/getting-started)
- [Redux style guide](https://redux.js.org/style-guide/)
- [createStore](https://redux.js.org/api/createStore)
</details>

<details>
<summary>Какие adaptations нужны для использования Redux вне UI environment (например, в Node.js backend)?</summary>

### Какие adaptations нужны для использования Redux вне UI environment (например, в Node.js backend)?

**Definition:**

Создайте хранилище, отправляйте действия и читайте `getState()`. `Provider` не нужен. Не держите одно глобальное хранилище на все запросы, если эти запросы не должны видеть данные друг друга.

**How it works:**

- **Те же редьюсеры**: Слайсы остаются чистыми. Node может их импортировать и вызвать `configureStore`.
- **Подписка**: Используйте `subscribe`, когда не-React-вызывающему нужно реагировать. Иначе отправьте действие и один раз прочитайте `getState()`.
- **Изоляция**: Хранилище — объект. Двум запросам нужны два хранилища, либо одна очередь, в которой пользователи не пересекаются.

**Sources:**

- [createStore](https://redux.js.org/api/createStore)
- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
</details>


### Scaling Redux for Large Applications

<details>
<summary>Как you orGAnize your files and folders in a large Redux application?</summary>

### Как you orGAnize your files and folders in a large Redux application?

**Definition:**

Используйте папки возможностей. Кладите слайс рядом с компонентами, которые его используют. Настройку хранилища держите в `app/`.

**How it works:**

- **Раскладка**: Руководство по стилю показывает `app/store.ts`, `features/todos/todosSlice.ts` и `features/todos/Todos.tsx`.
- **Не по типу**: Корневая папка `actions/` размазывает одну возможность по каталогам. Руководство говорит, что так труднее сопровождать.
- **Общее**: `common/` — для хуков и помощников, которые не являются возможностью. Код возможности остаётся в своей папке.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Какие стратегии can be used to scale Redux actions and reducers?</summary>

### Какие стратегии can be used to scale Redux actions and reducers?

**Definition:**

Один слайс на возможность, типы действий с префиксом и общие редьюсеры случаев только через фабрику. Хранилище пусть объединяет их само.

**How it works:**

- **Префикс**: `makeCounterSlice("counterB")` даёт `counterB/incremented`, поэтому отправка не обновляет каждый счётчик.
- **Дополнительные редьюсеры**: Слайс слушает действие другой возможности в `extraReducers`, вместо того чтобы импортировать её состояние и писать в него.
- **Нормализация**: Большие списки остаются в адаптерах сущностей, чтобы одно обновление не переписывало вложенное дерево.

**Sources:**

- [Reusing reducer logic](https://redux.js.org/usage/structuring-reducers/reusing-reducer-logic)
- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
</details>

<details>
<summary>Можете ли вы explain the concept of "domain-driven design" in the context of a Redux state tree?</summary>

### Можете ли вы explain the concept of "domain-driven design" in the context of a Redux state tree?

**Definition:**

Группируйте дерево состояния по предметной возможности, а не по техническому типу. Каждая папка возможности владеет своей частью дерева.

**How it works:**

- **Границы**: `state.todos` и `state.posts` совпадают с возможностями. Компонент задач выбирает задачи и не лезет во внутренности другого слайса.
- **События**: Действия в стиле событий из руководства (`orderAdded`) называют, что случилось в предмете, а не сеттер одного поля.
- **Общее ядро**: Таблицы сущностей, нужные нескольким возможностям, могут жить под `entities`, что страница нормализации показывает рядом с `ui`.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
</details>

<details>
<summary>Как feature modules and code splitting affect Redux state management?</summary>

### Как feature modules and code splitting affect Redux state management?

**Definition:**

Редьюсер можно добавить после старта через `replaceReducer`. Ключ состояния возможности появляется, когда этот редьюсер смонтирован. Селекторы должны терпеть отсутствующую ветку до этого момента.

**How it works:**

- **replaceReducer**: В примере настройки хранилища `store.replaceReducer` вызывается при горячей перезагрузке. Тот же метод подставляет объединённый редьюсер, в который вошла только что загруженная возможность.
- **Начальное состояние**: Новый слайс при первом вызове получает `undefined` и использует свой `initialState`.
- **Не импортируйте хранилище везде**: Руководство по стилю говорит, что код возможности должен отправлять действия через хуки. Динамическая вставка происходит рядом с настройкой хранилища.

**Sources:**

- [Configuring your store](https://redux.js.org/usage/configuring-your-store)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Обсудите strategies for managing complex application state with Redux.</summary>

### Обсудите strategies for managing complex application state with Redux.

**Definition:**

Делите по возможностям, нормализуйте сущности, держите производные данные в селекторах и моделируйте асинхронный статус явным полем.

**How it works:**

- **Конечные состояния**: Руководство по стилю показывает `idle | loading | success | failure` как размеченное объединение, чтобы `user` и `error` не были заданы вместе.
- **Сущности**: `createEntityAdapter` держит большие коллекции. Флаги интерфейса остаются в отдельном слайсе.
- **Эффекты**: Загрузка идёт через `createAsyncThunk` или RTK Query, не через редьюсер.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [createEntityAdapter](https://redux-toolkit.js.org/api/createEntityAdapter)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
</details>


### Redux Performance Optimization

<details>
<summary>Как you prevent unnecessary renders in Redux-connected components?</summary>

### Как you prevent unnecessary renders in Redux-connected components?

**Definition:**

Подписывайте каждый компонент на значение, которое он рисует. Возвращайте из `useSelector` устойчивый результат и мемоизируйте соединения.

**How it works:**

- **Точечно**: Разделение список/строка из руководства значит, что один изменённый пользователь не рисует строки всего списка.
- **Несколько хуков**: Несколько вызовов `useSelector`, каждый из которых возвращает число или строку, лучше одного селектора, который строит новый объект.
- **shallowEqual**: Когда объект необходим, передайте `shallowEqual` вторым аргументом или как `equalityFn`.

**Examples:**

```js
const count = useSelector(selectCount);
const todo = useSelector((state) => state.todos.entities[id]);
```

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Hooks API](https://react-redux.js.org/api/hooks)
</details>

<details>
<summary>Какие техники are available to reduce the size of the Redux store?</summary>

### Какие техники are available to reduce the size of the Redux store?

**Definition:**

Не храните то, что может посчитать селектор. Не храните серверный кэш руками, если RTK Query уже его ведёт. Держите сущности плоскими, чтобы не повторять вложенные копии.

**How it works:**

- **Выводить**: Руководство по стилю говорит, что отфильтрованные списки и суммы принадлежат мемоизированным селекторам.
- **Нормализовать**: Одна запись пользователя вместо копии внутри каждого поста.
- **Кэш**: RTK Query хранит результаты конечных точек под своим `reducerPath` и удаляет их по времени жизни кэша, вместо того чтобы каждый экран дописывал свой массив.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
</details>

<details>
<summary>Как you optimize Redux action dispatch and payload size?</summary>

### Как you optimize Redux action dispatch and payload size?

**Definition:**

Отправляйте одно событие с id или с полями, которые изменились. Не кладите в действие следующее дерево состояния, функции или экземпляры классов.

**How it works:**

- **Маленькая нагрузка**: Предпочтительный в руководстве `todoToggled` несёт id. Редьюсер берёт задачу из состояния.
- **Сериализуемое**: Обещания, функции и экземпляры классов противоречат проверке сериализуемости. Промежуточный слой может держать функцию, только если остановит действие до редьюсеров.
- **Одна отправка**: Событие `orderAdded` лучше двух отправок-сеттеров, каждая из которых читает `getState()` и добавляет единицу.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Что такое best practices for ensuring a Scalable Redux state management with large datasets or frequent updates?</summary>

### Что такое best practices for ensuring a Scalable Redux state management with large datasets or frequent updates?

**Definition:**

Нормализуйте коллекцию, обновляйте одну сущность на действие и выбирайте по id, чтобы частые тики не рисовали весь список.

**How it works:**

- **Адаптер**: `updateOne` и `setAll` из `createEntityAdapter` пишут форму `{ ids, entities }`.
- **Селекторы**: Компонент строки выбирает `entities[id]`. Родитель выбирает только `ids`.
- **Склеивание**: Если сокет шлёт много раз в секунду, отправляйте последний снимок с коротким интервалом, а не раз на сообщение. Редьюсер всё равно применяет одно обычное действие.

**Sources:**

- [createEntityAdapter](https://redux-toolkit.js.org/api/createEntityAdapter)
- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>


### Redux and Forms

<details>
<summary>Как you manage form state with Redux?</summary>

### Как you manage form state with Redux?

**Definition:**

Держите большинство нажатий в компоненте. Кладите значение в Redux, когда другая часть приложения должна прочитать его до отправки, или когда оно нужно в DevTools.

**How it works:**

- **По умолчанию локально**: Полю, которое нужно только его форме, не нужно действие на каждую клавишу. Руководство по стилю хочет логику в редьюсерах, а не поток сеттеров.
- **Если оно в хранилище**: Руководство по стилю допускает обновление через расширение для формы, где отдельное действие на поле было бы шумом. Лучше один `draftSaved` с полями, которые вы действительно храните.
- **Отправка**: Thunk отправки читает черновик или получает его аргументом, отправляет и кладёт сущность сервера в адаптер.

**When to use:**

- Состояние компонента для формы одного экрана.
- Redux, когда черновик должен пережить переход или его читает другая смонтированная возможность.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Обсудите the use of libraries like Redux Form or React Final Form with Redux.</summary>

### Обсудите the use of libraries like Redux Form or React Final Form with Redux.

**Definition:**

Эти библиотеки держат состояние полей в своём дереве. Redux не нужна копия каждого нажатия. При отправке отправьте thunk со значениями, которые даёт библиотека формы.

**How it works:**

- **Граница**: Библиотека формы вызывает вашу функцию отправки с простыми значениями. Эта функция отправляет `createAsyncThunk` или мутацию RTK Query.
- **Не зеркальте**: Запись каждого изменения и в Redux, и в форму дублирует состояние. «Единый источник» в руководстве — на факт, а не «каждая клавиша живёт в хранилище».
- **Результат сервера**: Сохранённую сущность храните в адаптере. Фокус ввода и флаги «не тронуто» оставьте библиотеке формы.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
</details>

<details>
<summary>Что такое the trade-offs of keeping form state in the Redux store?</summary>

### Что такое the trade-offs of keeping form state in the Redux store?

**Definition:**

Черновик виден в DevTools и любому подписчику. Каждое нажатие — действие, поэтому журнал шумный и больше компонентов может рисоваться.

**How it works:**

- **Выигрыш**: Другой экран может прочитать черновик. Обновление страницы может восстановить его из `preloadedState`, если вы его сохранили.
- **Цена**: Руководство по стилю предупреждает, что действия-сеттеры заливают журнал. Селектор, который читает весь черновик, перерисовывает подписчиков на каждый символ.
- **Середина**: Храните черновик, когда пользователь покидает поле или уходит со страницы, а не на каждое событие `input`.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>


### Redux and Authentication

<details>
<summary>Как you manage user authentication and session state in Redux?</summary>

### Как you manage user authentication and session state in Redux?

**Definition:**

Храните сериализуемые факты сессии, которые нужно рисовать: id пользователя, статус и, может быть, несекретное отображаемое имя. Вход и выход ведите thunk-ами, которые затем отправляют обычные действия.

**How it works:**

- **Объединение статусов**: Форма `idle | loading | success | failure` из руководства подходит для `user` и `error` без невозможных сочетаний.
- **Запрос**: `createAsyncThunk` выполняет запрос входа. `fulfilled` записывает пользователя. `rejected` записывает ошибку.
- **Сброс**: Выход отправляет действие, которое возвращает слайс сессии к `initialState`, и корневой сброс, если другие слайсы держат данные этого пользователя.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
</details>

<details>
<summary>Какие соображения should be made for security when storing tokens or sensitive information in Redux?</summary>

### Какие соображения should be made for security when storing tokens or sensitive information in Redux?

**Definition:**

Хранилище читает любой код страницы и DevTools. Токен там не секрет от XSS. Предпочтительнее cookie HttpOnly, которую JavaScript не читает.

**How it works:**

- **Что хранить**: Храните `{ id, name, status }`. Не кладите долгоживущие токены доступа в действие: история DevTools из руководства хранит действия.
- **Сериализуемое намеренно**: Всё в состоянии можно сериализовать в журнал или в нагрузку серверного рендера. Не кладите секреты в `preloadedState`.
- **Выход**: Сбросьте слайс сессии, чтобы следующий пользователь на той же странице не прочитал предыдущий объект `user`.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [createStore](https://redux.js.org/api/createStore)
</details>

<details>
<summary>Как you manage login and loGout workflows with Redux actions and reducers?</summary>

### Как you manage login and loGout workflows with Redux actions and reducers?

**Definition:**

Вход — это thunk, который отправляет действие с результатом. Выход — синхронное действие, которое восстанавливает `initialState` и при необходимости сбрасывает остальное корневое состояние.

**How it works:**

- **Вход**: `extraReducers` обрабатывает `login.pending`, `login.fulfilled` и `login.rejected` через объединение статусов из руководства по стилю.
- **Выход**: Редьюсер сессии возвращает `initialState`. Корневая обёртка может передать `undefined` в объединённый редьюсер, чтобы слайсы возможностей сбросили сущности этого пользователя.
- **Интерфейс**: `useDispatch` запускает thunk из формы. `useSelector` читает `status`, чтобы отключить кнопку, пока `loading`.

**Examples:**

```js
builder.addCase(login.fulfilled, (state, action) => {
  return { status: "success", user: action.payload, error: null };
});
```

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
</details>


### Migrating to Redux and State Hydration

<details>
<summary>Как you approach migrating a leGAcy application state management to Redux?</summary>

### Как you approach migrating a leGAcy application state management to Redux?

**Definition:**

Добавьте одно хранилище в корень и переносите по одной возможности в слайс. Остальное состояние оставьте на месте, пока чтения этой возможности не пойдут через `useSelector`.

**How it works:**

- **Сначала хранилище**: `configureStore` и `Provider` могут обернуть приложение до переноса возможностей. Пустой `reducer: {}` бесполезен; начните с первого слайса.
- **Одна возможность**: Напишите `createSlice` для этой возможности, отправляйте действия из её событий и удалите старые локальные обновления тех же фактов.
- **Правила**: По мере переноса применяйте руководство по стилю: без мутаций, без эффектов в редьюсерах, папки возможностей.

**Sources:**

- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Можете ли вы explain the concept of "state hydration" in the context of SSR and Redux?</summary>

### Можете ли вы explain the concept of "state hydration" in the context of SSR and Redux?

**Definition:**

Гидратация здесь значит создать браузерное хранилище с состоянием, которое сервер уже нарисовал. Этот объект — `preloadedState`.

**How it works:**

- **Сервер**: Сервер создаёт хранилище, отправляет нужные загрузки, рисует и сериализует `getState()`.
- **Браузер**: `configureStore({ reducer, preloadedState })` стартует с этого объекта, чтобы первая отрисовка клиента совпала с HTML.
- **Ограничение**: Эту границу пересекают только простые сериализуемые данные. Руководство по стилю запрещает функции, Map и экземпляры классов в состоянии.

**Sources:**

- [createStore](https://redux.js.org/api/createStore)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Какие challenges могут возникнуть при интеграции Redux в existing application и как их решить?</summary>

### Какие challenges могут возникнуть при интеграции Redux в existing application и как их решить?

**Definition:**

Два источника правды и второе хранилище — обычные поломки. Держите одно хранилище и переносите одну возможность, пока её старое состояние не исчезнет.

**How it works:**

- **Одно хранилище**: Руководство по стилю говорит, что у обычного приложения одно хранилище в `Provider`. Второй `configureStore` не обновит первое дерево.
- **Дублирование**: Если и состояние React, и Redux хранят список задач, они разойдутся. Когда слайс есть, компонент должен выбирать его, а не копировать в `useState`.
- **Эффекты**: Старый код, который запрашивал данные во время отрисовки, переезжает в thunk или RTK Query. Редьюсеры остаются чистыми, чтобы DevTools мог их проиграть.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
</details>


### Redux Future and Community

<details>
<summary>Опишите the current trends and community practices around Redux.</summary>

### Опишите the current trends and community practices around Redux.

**Definition:**

Текущая практика из руководства по стилю — Redux Toolkit, слайсы возможностей, хуки вместо `connect` и RTK Query для серверного кэша.

**How it works:**

- **Toolkit**: `configureStore` и `createSlice` — путь, который показывают документы getting started.
- **Хуки**: React-Redux помечает `connect` устаревшим с версии 9.3.0 и указывает на `useSelector` и `useDispatch`.
- **Загрузка данных**: Обзор RTK Query говорит, что загрузка и кэш — другая задача, чем состояние клиента, и что `createApi` — встроенный ответ.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [connect()](https://react-redux.js.org/api/connect)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
</details>

<details>
<summary>Как Redux evolved to fit into the modern React ecosystem with hooks?</summary>

### Как Redux evolved to fit into the modern React ecosystem with hooks?

**Definition:**

React-Redux добавил `useSelector` и `useDispatch`, которые читают хранилище без обёртки `connect`. Затем Redux Toolkit стал тем, как эти хуки преподают.

**How it works:**

- **От HOC к хукам**: Руководство по стилю говорит, что хуки заставляют компонент запрашивать свои данные и что `connect` труднее типизировать.
- **Всё ещё одно направление**: Компонент отправляет действие; редьюсеры возвращают состояние; селекторы его читают. Хуки не добавили второй путь записи.
- **Устаревание**: `connect` всё ещё выполняется. Новые компоненты используют хуки или `legacy_connect`, если обёртка должна остаться.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Hooks API](https://react-redux.js.org/api/hooks)
- [connect()](https://react-redux.js.org/api/connect)
</details>

<details>
<summary>Каково, по вашему мнению, будущее Redux в контексте emerging state management libraries?</summary>

### Каково, по вашему мнению, будущее Redux в контексте emerging state management libraries?

**Definition:**

Redux остаётся предсказуемым хранилищем клиентского состояния, которое делят много возможностей. RTK Query и руководство по стилю уже считают серверный кэш отдельной работой, которая может жить рядом с этим хранилищем.

**How it works:**

- **Состояние клиента**: Слайсы, события и DevTools остаются причиной держать общее хранилище.
- **Состояние сервера**: Обзор RTK Query говорит, что сообщество отделило «загрузку данных» от «управления состоянием» и что `createApi` закрывает загрузку поверх Redux.
- **Меньше шаблона**: Хранилище в getting started — вызов `configureStore`, а не рукописные константы и switch.

**Sources:**

- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>


### Advanced State Modeling in Redux

<details>
<summary>Как you model complex state dependencies and computations in Redux?</summary>

### Как you model complex state dependencies and computations in Redux?

**Definition:**

Храните входы. Зависимое значение считайте в мемоизированном селекторе. Используйте объединение статусов, когда допустимые сочетания ограничены.

**How it works:**

- **Селекторы**: `createSelector` пересчитывает, когда меняются входы. Руководство по стилю предпочитает это хранению производного числа.
- **Объединения**: Состояние пользователя в руководстве — `idle`, `loading`, `success` или `failure`, поэтому успех не стоит рядом с ошибкой.
- **Связи**: Нормализованные id плюс селектор собирают граф. Редьюсер не держит вторую вложенную копию в синхроне.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
</details>

<details>
<summary>Обсудите your approach to type checking and ensuring state consistency.</summary>

### Обсудите your approach to type checking and ensuring state consistency.

**Definition:**

Типизируйте состояние слайса, включая размеченное объединение для статуса. Пусть `configureStore` выведет корневое состояние. Доверьте редьюсеру игнорировать невозможные переходы.

**How it works:**

- **Объединение**: Руководство по стилю типизирует `UserState` так, что `status: "success"` значит `user: User` и `error: null`.
- **PayloadAction**: Редьюсеры случаев принимают `PayloadAction<User>`, чтобы поле нагрузки проверялось в месте отправки.
- **Корень**: `ReturnType<typeof store.getState>` — тип корневого состояния для селекторов, включая `getSelectors<RootState>`.

**Examples:**

```ts
type UserState =
  | { status: "idle"; user: null; error: null }
  | { status: "loading"; user: null; error: null }
  | { status: "success"; user: User; error: null }
  | { status: "failure"; user: null; error: string };
```

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [createEntityAdapter](https://redux-toolkit.js.org/api/createEntityAdapter)
</details>

<details>
<summary>Как you tackle transactional updates in Redux?</summary>

### Как you tackle transactional updates in Redux?

**Definition:**

Одно действие может обновить все поля, которые должны смениться вместе, внутри одного вызова редьюсера. Подписчики видят следующее состояние только после того, как редьюсер вернулся.

**How it works:**

- **Одно событие**: В руководстве `orderAdded` несёт пиццу и колу вместе, а не два сеттера.
- **Несколько слайсов**: Если два слайса должны сдвинуться вместе, обработайте один и тот же тип действия в обоих `extraReducers`. Оба выполняются в этой отправке до того, как слушатели рисуют.
- **Не транзакция базы**: Если вызов сервера не удался, отправьте следующее действие, которое вернёт прежние простые значения. Редьюсер сам не откатывается.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Какие стратегии would you use for undo/redo functionality in a Redux application?</summary>

### Какие стратегии would you use for undo/redo functionality in a Redux application?

**Definition:**

Держите `past`, `present` и `future` для ветки, которую можно отменить. Действие отмены кладёт `present` в `future` и восстанавливает последнюю запись `past`. Путешествие во времени в DevTools — отдельный инструмент отладки.

**How it works:**

- **Область**: Отменяйте слайс документа, а не всё хранилище. Сессия и статус запроса не должны отматываться.
- **Действия**: `changeCommitted` кладёт предыдущее настоящее. `undo` и `redo` только двигают эти массивы. Это обычные редьюсеры слайса.
- **Предел**: Ограничьте `past`, чтобы хранилище не держало каждое нажатие. Руководство по стилю и так предостерегает от безграничных производных копий.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>


### Redux Interoperability

<details>
<summary>Как Redux interact with WebSockets or external data sources?</summary>

### Как Redux interact with WebSockets or external data sources?

**Definition:**

Сокетом владеет слушатель или thunk. Входящие сообщения отправляют обычные действия. Редьюсер хранит сериализуемую нагрузку.

**How it works:**

- **Слушатель**: `createListenerMiddleware` может начать работу, когда действие совпало, сделать `dispatch` и `fork`. Поставьте его через `prepend`, чтобы функции не попали в проверку сериализуемости.
- **RTK Query**: В обзоре сказано, что параметры жизненного цикла записи кэша могут стримить обновления по websocket после первой загрузки.
- **Редьюсер**: Колбэк сокета не должен мутировать состояние. Он отправляет `eventReceived` с простым объектом.

**Sources:**

- [createListenerMiddleware](https://redux-toolkit.js.org/api/createListenerMiddleware)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Обсудите the integration of GraphQL in a Redux-managed application.</summary>

### Обсудите the integration of GraphQL in a Redux-managed application.

**Definition:**

Результаты GraphQL — это состояние сервера. RTK Query может вызвать конечную точку GraphQL через `fetchBaseQuery` или свой `baseQuery`, а обзор отмечает генерацию кода из схем GraphQL.

**How it works:**

- **Конечная точка**: `createApi` задаёт операцию. Точка входа React порождает хук вроде `useGetPokemonByNameQuery` для компонентов.
- **Хранилище**: Добавьте `pokemonApi.reducer` и `pokemonApi.middleware` в `configureStore`. `setupListeners(store.dispatch)` включает повторную загрузку при переподключении.
- **Состояние клиента**: Черновики и флаги интерфейса оставьте в слайсах. Не копируйте кэш GraphQL ещё и в рукописный слайс сущностей без причины.

**Sources:**

- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
</details>

<details>
<summary>Как бы вы manage local vs. server state in a Redux application?</summary>

### Как бы вы manage local vs. server state in a Redux application?

**Definition:**

Записи сервера живут в адаптере сущностей или в кэше RTK Query. Локальное состояние интерфейса живёт в слайсе или в компоненте. Не смешивайте статус запроса и черновик в один бесформенный объект.

**How it works:**

- **Сервер**: Обзор RTK Query говорит, что загрузка и время жизни кэша — другая задача, и `createApi` сделан для неё.
- **Локальное**: Страница нормализации кладёт `ui` рядом с `entities`. Флаг модального окна не принадлежит записи пользователя.
- **Один факт**: Если оба слоя хранят пользователя, они разойдутся. После успешного запроса источником выберите серверный кэш.

**Sources:**

- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
</details>


### Redux API Requests and Caching

<details>
<summary>Как you structure API requests in Redux actions?</summary>

### Как you structure API requests in Redux actions?

**Definition:**

Для базового URL и конечных точек предпочтителен `createApi`. Для разового вызова `createAsyncThunk` шлёт `pending`, `fulfilled` и `rejected`, а слайс обрабатывает их в `extraReducers`.

**How it works:**

- **Thunk**: Создатель нагрузки делает `await` клиента и возвращает данные. Сам редьюсер он не вызывает.
- **Обработчики**: `builder.addCase(fetchUserById.fulfilled, ...)` записывает результат. `pending` ставит `loading`.
- **RTK Query**: Учебник essentials говорит, что RTK Query может убрать рукописные thunk и редьюсеры для загрузки. `fetchBaseQuery({ baseUrl })` — обычный базовый запрос.

**Examples:**

```js
export const pokemonApi = createApi({
  reducerPath: "pokemonApi",
  baseQuery: fetchBaseQuery({ baseUrl: "https://pokeapi.co/api/v2/" }),
  endpoints: (build) => ({
    getPokemonByName: build.query({ query: (name) => `pokemon/${name}` }),
  }),
});
```

**Sources:**

- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
</details>

<details>
<summary>Обсудите methods for caching API responses in the Redux store.</summary>

### Обсудите methods for caching API responses in the Redux store.

**Definition:**

RTK Query кэширует результат на конечную точку и аргумент, а компонент читает `data` и `isLoading` из порождённого хука. Рукописный слайс — более старый образец, который учебник essentials показывает снизу.

**How it works:**

- **createApi**: Один слайс API на базовый URL — правило из обзора. Редьюсер и промежуточный слой оба должны быть зарегистрированы.
- **Время жизни**: Запись кэша может загрузиться снова при монтировании или переподключении, если вызвать `setupListeners(store.dispatch)`.
- **Вручную**: Слайс, который хранит `entities` из выполненного thunk, — это кэш, который вы сами должны инвалидировать. Делайте так, только когда для этого ресурса не используете `createApi`.

**Examples:**

```js
[pokemonApi.reducerPath]: pokemonApi.reducer,
middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(pokemonApi.middleware),
```

**Sources:**

- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
</details>

<details>
<summary>Какие patterns или libraries вы используете для RESTful или GraphQL API interactions с Redux?</summary>

### Какие patterns или libraries вы используете для RESTful или GraphQL API interactions с Redux?

**Definition:**

Для обоих используйте `createApi` из RTK Query. Конечные точки REST используют `fetchBaseQuery` и путь. GraphQL использует функцию `query`, которая отправляет операцию, или код, порождённый из схемы, — обзор говорит, что он есть в раннем виде.

**How it works:**

- **REST**: `build.query({ query: (name) => \`pokemon/${name}\` })` делает GET к базовому URL. Мутации используют `build.mutation`.
- **Хуки**: Импортируйте `createApi` из `@reduxjs/toolkit/query/react`, чтобы конечная точка экспортировала `useGetPokemonByNameQuery`.
- **Запасной путь**: Один REST-вызов может остаться `createAsyncThunk` плюс `extraReducers`, когда кэш не нужен.

**Sources:**

- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
</details>
