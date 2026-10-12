# React — Redux interview

Redux interview questions. Answers are filled for the full question list.

[← Back to hub](./4_react.md)

### Related docs

- [React — Redux](./4_react_redux.md) · [Redux — Interview](./4_react_redux_interview.md) · [React — Interview](./4_react_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/).

### Redux Fundamentals

<details>
<summary>What is Redux and how is it used in web development?</summary>

### What is Redux and how is it used in web development?

**Definition:**

Redux is a predictable state container principally designed for JavaScript applications. It efficiently manages and updates the app state, serving as a centralized global store.

**How it works:**

**Core Concepts**

**Store**

The store is at Redux's nucleus and acts as a single source of truth. Here, you'll find the current state alongside the ability to dispatch actions.

**Actions**

Actions are payloads of data - typically objects - that describe what needs to happen. These are dispatched to the store.

**Reducers**

Reducers are pure functions that specify how the state changes in response to actions. Each reducer handles a particular part of the state and combines to produce the full state.

**View**

The user interface (UI) or the output your app produces, is ideally a direct derivative of the current state. Both are connected through subscription and rendering.

**Data Flow**

Redux follows a straightforward loop, or unidirectional data flow, that tallies with its action-reducer-store structure.

**Acquiring Action:**

The UI elements, like buttons, yield actions.

**Dispatcher Role:**

Through store.dispatch(action), these actions are transmitted to the store.

**Action Execution and State Mutation:**

With defined logic, reducers modify the state, originating a new state tree.

**State Change Subscription notification:**

Subscribed UI segments receive alerts about the state change and update correspondingly.

**Render Action:**

The updated state triggers view re-renders, aligning the UI with the latest state.

**Application Across Tech Stacks**

While most associates developed Redux for React, the library is platform-agnostic. Adaptors and connectors permit its use with a spectrum of libraries, chiefly react-redux. Other integrations include Angular, Vue.js, and vanilla JavaScript applications.

**Pluses and minuses:**

**Key Benefits**

- **Increased Predictability:** The sequence of state alterations is preset and controlled.
- **Simplified State Management:** Every part of the state is stored in one location.
- **Streamlined Code Execution:** Reducers define state modifications, making it easier to trace changes. Debugging becomes more straightforward and time-efficient.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Can you describe the three principles that Redux is based upon?</summary>

### Can you describe the three principles that Redux is based upon?

**Definition:**

Let's look at the three key principles forming the backbone of Redux: Single Source of Truth, State is Read-Only, and Changes with Pure Functions.

**How it works:**

**Single Source of Truth**

The state of your whole application is stored in a single tree or object within the Redux store. This makes it easier to identify where and how the state is changing, leading to better predictability and manageability.

**Core Benefit**

**Centralized View of State:** You can get a comprehensive snapshot of your app's state at any point in time, making debugging and tracking data flow easier.

**State is Read-Only**

In a Redux setup, the state is never modified directly. Any required changes are implemented by dispatching actions capturing the point-by-point changes expected in the state.

**Core Benefits**

**Clarity and Control:** By preventing direct state alterations, Redux emphasizes a controlled and defined mechanism for state mutation, which is the action creator.

**Time-Travel Debugging:** Because actions are recorded, you have the option to revert to specific application states for debugging.

**Changes with Pure Functions**

The state changes in Redux are driven by pure functions called reducers. These functions take the current state and an action as input, and then return a new state without altering the previous state.

**Core Benefits**

**Predictable Outcomes:** Since reducers are deterministic, their output for any given input and action will always be the same, ensuring consistency.

**Testability and Maintainability:** Pure functions are easier to test and understand, making your codebase more reliable and maintainable.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>What is an action in Redux?</summary>

### What is an action in Redux?

**Definition:**

Let's start with an overview of actions in Redux before jumping into the code.

**How it works:**

**Actions in Redux**

In Redux, an action encapsulates a "payload" of data meant to change the state. It also carries a descriptive type, defining the nature of the state change.

Actions are dispatched by the front-end components and processed by reducers, which are pure functions, ensuring predictability.

**Anatomy of an Action**

An action in Redux is created as an object literal with at least one mandatory property, type. Optionally, you can have additional data known as the "payload".

**Considerations**

**Encapsulation:** The type and payload ensure actions are descriptive and structured, simplifying code readability and debugging.

**Type Safety:** Utilizing TypeScript (or a similar language) for actions brings robust type checking to the development process.

For added safety and consistency, consider using action creators.

**Examples:**

Here's a typical structure:

```js
const myAction = {
  type: "ADD_TODO",
  payload: {
    id: 1,
    text: "Buy groceries",
  },
};
```

**Code Example:** Action

Here's the TypeScript code:

```ts
// Defining the Action Interface
interface AddTodoAction {
  type: "ADD_TODO";
  payload: { id: number; text: string };
}

// Creating an action
const addTodoAction: AddTodoAction = {
  type: "ADD_TODO",
  payload: { id: 1, text: "Buy groceries" },
};
```

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>How are actions used to change the state in a Redux application?</summary>

### How are actions used to change the state in a Redux application?

**Definition:**

Redux follows a strict unidirectional data flow pattern, which means that actions are the only way to modify state.

**How it works:**

**The Cycle of Action and State**

**Action Creation:** You use Action Creators to create actions.

**Dispatch:** Actions are dispatched using store.dispatch(action). Here, the store is the single source of truth in a Redux app and is responsible for action delivery to reducers.

**Reducer Listening:** Reducers, which are pure functions, listen for dispatched actions. If a matching action is identified, the reducer performs the necessary state modifications and returns the new state.

**Store Update:** The updated state is then stored, and any bound UI components are notified for necessary updates.

**Key Points to Remember**

State is read-only: Direct changes to state are not allowed. Instead, an action generator must be dispatched.

**Synchronous Updates:** Reducers are responsible for synchronous state updates. Any asynchronous logic (like API calls) is typically handled by middleware like Redux Thunk.

**Single Source of Truth:** The application's state is maintained in a single store, making it easier to manage and debug.

**Handling State with Reducers**

Reducers, pure functions, are the cornerstone in the state modification process. When they receive an action and the current state, they decide how to modify that state. Upon returning a new, updated state, the global store replaces the current state with this new state.

**Examples:**

**Code Example:** Dispatching an Action

Here is the JavaScript code:

```js
// Action Creator
function increment() {
  return { type: "INCREMENT" };
}

// Dispatching the action
store.dispatch(increment());
```

In the example, the increment function is an Action Creator that creates an action with type 'INCREMENT'. The store.dispatch method then dispatches this action.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>What is a reducer in Redux and what role does it play?</summary>

### What is a reducer in Redux and what role does it play?

**Definition:**

A reducer in Redux is a pure function responsible for managing specific parts of your application state. It captures state changes and computes the new state based on those changes.

**How it works:**

**Key Characteristics**

**Predictability:** Given the same input, a reducer will always produce the same output, making state management more predictable.

**Immutability:** Reducers handle state immutably, ensuring that every state transition results in a new state object. This helps to avoid side effects and enhances the efficiency of UI and state management libraries like React and their change detection algorithms.

**Single Responsibility:** Each reducer is geared towards managing a particular slice of application state. This limited scope aids in keeping the codebase organized and makes it easier to debug and maintain.

**Methods and Libraries**

Redux provides utility functions, such as combineReducers(), to help manage multiple reducers, each responsible for a distinct part of the application state. Libraries like immer and Redux Toolkit further simplify the process of writing reducers, especially for handling immutable state updates.

**Practical Application**

Associating actions with reducers allows for a modular and data-driven approach to state management. For instance, clicking a button might dispatch an 'increment' action, processed by the counterReducer to adjust the count value.

**Examples:**

**Code Example:** Reducer

Here is the JavaScript code:

```js
const counterReducer = (state = { count: 0 }, action) => {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    default:
      return state;
  }
};
```

In this example, counterReducer ensures immutability by creating and returning a new state object with each action dispatched.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>How does Redux differ from local component state in React?</summary>

### How does Redux differ from local component state in React?

**Definition:**

Let's look at the key differences between using Local Component State and Redux for managing state in React.

**How it works:**

**Why Use Global State Management**

Managing state globally:

- Is especially useful for larger applications, as it streamlines access and updates to state across components.
- Simplifies data sharing between components located at different levels of component hierarchy.

**Advantages and Limitations of Local Component State**

**Advantages**

**Simplicity:** It's quick and easy to set up local state within a component.

**Scoped Updates:** Changes to local state are contained within the component, making it easier to manage and reason about.

**Limitations**

**State Duplication:** Keeping similar state in sync across multiple components can lead to inconsistencies and bugs.

**Passing State and Event Handlers:** Necessitates drilling down state and event handlers to nested and related components.

**Hierarchical Data Flow:** Forces a one-way, parent-to-child data flow, making certain patterns unwieldy to implement.

**Advantages of Global State Management with Redux**

**Centralized State:** Provides a single source of truth, which safeguards against inconsistencies across the application.

**Predictable State Updates:** Leveraging reducers ensures controlled and consistent state modifications.

**Data Sharing:** Facilitates data sharing between components without the need for prop drilling.

**Time-Travel Debugging:** Through tools like Redux DevTools, it makes it possible to revisit and review the state at any point in time during the application's lifecycle.

**When to use:**

**When to Choose Which Approach**

**Local State:** Opt for local state management for simpler, predominantly UI-related state or for small-scale applications with minimal state requirements or data sharing needs.

**Redux (or Another Global State Management):** Choose a global state management system when working with larger-scale applications, multi-level state dependencies, or when extensive data sharing and consistency across the application is critical.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Define "store" in the context of Redux.</summary>

### Define "store" in the context of Redux.

**Definition:**

In the context of Redux, the store is a key component that acts as a central data hub.

**How it works:**

**Store Components**

**State:** Maintains the current data state, which is read-only. To update it, you dispatch actions.

**Reducers:** Action-specific functions are responsible for modifying the state and are combined using combineReducers().

**Listeners:** Registered callbacks are notified whenever the state is updated.

**Store Mechanics**

getState(): Use this method to access the current state.

dispatch(action): Enforce state updates by providing an action, typically an object with a type property.

subscribe(listener): Keep listeners updated on state changes by registering them.

replaceReducer(nextReducer): Use the provided reducer to update state-handling logic.

**Store Core Principles**

**Single Source of Truth:** All application data is managed through a single store, ensuring consistency.

**State is Read-Only:** Prevent accidental state modifications outside reducers to maintain a deterministic behavior.

**Store Setup**

To establish a Redux store in your application, use the createStore() function and pass in your app's reducers. Then, you can:

Access the initialized store using getState(), dispatch(), and subscribe().

Update the store's reducers dynamically with replaceReducer() when necessary.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Can you describe the concept of "single source of truth" in Redux?</summary>

### Can you describe the concept of "single source of truth" in Redux?

**Definition:**

Redux, a predictable state container, adheres to the single source of truth principle. It stipulates that there is a single immutable state tree shared across the entire application.

**Pluses and minuses:**

**Key Benefits**

- **Consistency:** All components reflect the latest state, minimizing data inconsistencies.
- **Easy Debugging:** A single point to monitor state changes improves debugging.
- **Conciseness and Clarity:** The central state tree simplifies data management and visibility.
- **Correctness Guarantee:** No parallel updates are possible, leading to improved application stability.

**Potential Drawbacks and Solutions**

**Performance Implications:** Extract only necessary parts of the state for components, often accomplished through selectors.

**Possible Bottlenecks:** Employ subscriptions or alternative mechanisms for efficient data flow while recognizing the trade-offs.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>How do you create a Redux store?</summary>

### How do you create a Redux store?

**Definition:**

Creating a Redux store involves several key steps. The store serves as a centralized hub for state management across the application.

You can create a Redux Store in any of the three ways:

- Directly using createStore(reducer, [preloadedState], [enhancer])
- Via a function that combines multiple reducers using combineReducers(reducers)
- With Middlewares for executing logic in the data flow using applyMiddleware(middleware1, middleware2, ...)

**How it works:**

**Steps to Create a Redux Store**

**Define the Reducer**

First, you have to define at least one reducer, a function that determines the shape and state transitions of the store.

If you are going to use multiple reducers, to manage different parts of the state tree, you will need to combine them.

**Create a Reducer Function**

**Import Additional Reducers**

If you have more than one reducer, use combineReducers to manage them together.

**Optional:** Add Initial State and Middlewares

A central state can be provided as initial state, for the reducers to pick up their specific sections.

The element applyMiddleware should be called with any middlewares you're employing in your application.

**Examples:**

Here is the example code:

```js
// counterReducer.js
const initialState = { count: 0 };

const counterReducer = (state = initialState, action) => {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + 1 };
    case "DECREMENT":
      return { count: state.count - 1 };
    default:
      return state;
  }
};

export default counterReducer;
```

```js
// reducers/index.js
import { combineReducers } from "redux";
import counterReducer from "./counterReducer";

const reducers = combineReducers({
  counter: counterReducer,
  // Add more reducers here
});

export default reducers;
```

```js
// reducers/index.js
import { combineReducers } from "redux";
import counterReducer from "./counterReducer";
import userReducer from "./userReducer";

const reducers = combineReducers({
  counter: counterReducer,
  user: userReducer,
});

export default reducers;
```

```js
// store.js
import { createStore, applyMiddleware, compose } from "redux";
import reducers from "./reducers";

const store = createStore(
  reducers,
  {
    counter: { count: 10 },
    // Additional initial states for more reducers
  },
  compose(applyMiddleware(middleware1, middleware2)),
);

export default store;
```

**Complete Example**

Here is the complete example with all the steps:

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
<summary>What is meant by "immutable state," and why is it important in Redux?</summary>

### What is meant by "immutable state," and why is it important in Redux?

**Definition:**

Immutable State is a key concept in Redux, which ensures that the state of an application cannot be changed directly. State alterations are achieved through actions and reducers, making the state more predictable and facilitating better debugging.

**How it works:**

**Persisting Immutability in Javascript**

The const declaration in JavaScript, while not offering immutability for object properties, does ensure that the reference itself (the state object in the context of Redux) remains constant, aligning with Redux's immutability requirement.

**Ways to Ensure Immutability in Redux**

**Spread Operator:** For shallow cloning of state objects

**Deep Clone:** For nested objects and arrays. However, this approach might be inefficient for large state objects and should be used judiciously.

**Immutability Libraries**

To simplify working with immutable data, you can use libraries such as:

**Immer:** Offers a minimalistic API for immutable state management

**Immutable.js:** Provides a complete data structure library, including both primitive and collection data types. Though powerful, it can be challenging to integrate with existing codebases due to its API differences.

**Pluses and minuses:**

**Advantages of Immutable State in Redux**

- **State History:** By preserving each state change, you can step forward or backward in time, aiding debugging and enabling features like undo/redo.
- **Performance Optimizations:** Redux utilizes reference equality checks. If the new and old states are the same, components avoid unnecessary re-renders.
- **Simplified Component Updates:** With unchanged objects, components can avoid updating unless their inputs change.
- **Threading Safety:** In multi-threaded applications, immutable objects guarantee that state changes don't create race conditions.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Explain the significance of the combineReducers function.</summary>

### Explain the significance of the combineReducers function.

**Definition:**

The combineReducers function in Redux plays a central role in structuring large web applications.

**How it works:**

Each of the imported reducers is a slice of the overall state tree. When an action is dispatched, it is then passed to each of these reducers. They process the action, and the relevant parts of the state tree get updated.

**Pluses and minuses:**

**Benefits of combineReducers**

- **Code Modularity:** By organizing reducers into smaller modules, it even becomes easier to share and reuse them across different parts of the application.
- **Convenience and Clarity:** Instead of dealing with a single overwhelmingly large reducer, developers can manage logically distinct parts of the application separately.
- **Selective Data Handling:** Reducers can act in specified functional areas only, keeping the rest of the state untouched.
- **Efficient Data Flow:** Each reducer only 'listens' to that part of the state that is relevant, potentially improving performance.

**Examples:**

**Code Example:** combineReducers in Action

Consider a note-taking app, which might have three distinct state trees: notes, ui, and user.

Here is how combineReducers can be employed:

```js
import { combineReducers } from "redux";
import notesReducer from "./notesReducer";
import uiReducer from "./uiReducer";
import userReducer from "./userReducer";

const rootReducer = combineReducers({
  notes: notesReducer,
  ui: uiReducer,
  user: userReducer,
});

export default rootReducer;
```

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>What are pure functions and side effects in the context of Redux?</summary>

### What are pure functions and side effects in the context of Redux?

**Definition:**

In the context of Redux, actions alter state through reducers, and middlewares handle side effects. It's essential that reducer functions be pure to ensure predictability in state changes.

**How it works:**

**Fundamental Concepts**

**Actions:** Objects describing a state change.

**Reducers:** Pure functions defining the state change.

**State:** An immutable object accessible throughout the app.

**Pure Functions**

Reducers must be pure, that is, their behavior should be entirely predictable.

**Determination of Output:** Output should be a direct result of input. The function should not base its output on any external state or data.

**No Side Effects:** They should not cause alterations outside their scope, like modifying global variables or invoking functions that perform I/O operations.

**Idempotence:** Executing the function multiple times with the same input should provide consistent outcomes and not provoke side effects.

**Redux utilizes the principle of immutability:** once in place, actions or state don't undergo alteration. Instead, they generate a 'new' state that replaces the existing one in its entirety.

**Side Effects**

Side effects allude to alterations induced by a function that aren't limited to its return value. Side-effectful functions can engage in various actions that extend beyond their obvious purpose. While some side effects are necessary, keeping them compartmentalized is crucial to maintaining code stability and ease of debugging. Side-effect management in Redux is facilitated by middlewares, like redux-thunk or redux-saga.

**Side Effect Management Methods**

Various Redux-specific libraries, such as Thunks or Sagas, provide mechanisms to tackle side effects in a structured way.

**Thunks**

Thunks are functions that can generate other functions, permitting activities that extend past straightforward dispatch. redux-thunk is a well-established middleware that empowers thunks in a Redux store.

**Sagas**

Sagas are designed on the concept of cooperative concurrency to streamline actions with side effects. In a redux-saga powered store, you interact with such sagas using the middleware applyMiddleware(sagaMiddleware). This is immensely useful for simultaneous operations and complex co-dependencies, especially with asynchronous processes.

**Examples:**

**Code Example:** Impure Reducer

This is the Python code:

```python
# Non-Pure Function

state = 0

def increment_counter():
    global state  # Modifies global state
    state += 1
    return state  # Returns a value different from the input

# Result: Output is unpredictable and changes external state
```

**Code Example:** Thunk in Action

This is the JavaScript Code:

```js
// Action Creator
const incrementAsync = () => {
  // Returning a function (a thunk)
  return (dispatch) => {
    // Asynchronous Operation
    setTimeout(() => {
      dispatch({ type: "INCREMENT" }); // Dispatch a regular action after a delay
    }, 2000);
  };
};
```

**Code Example:** Saga for Input Validation

Here is the JavaScript code:

```js
import { takeLatest, call, put } from "redux-saga/effects";
import { START_SUBMIT, SUCCESS_SUBMIT, FAILED_SUBMIT } from "./actionTypes";
import { validateInput } from "./api";

function* submitForm(action) {
  const { input } = action.payload;
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
<summary>How do you handle asynchronous actions in Redux?</summary>

### How do you handle asynchronous actions in Redux?

**Definition:**

Redux' action creators typically produce immediate synchronous actions. To handle asynchronous behavior, middleware, especially redux-thunk, is employed.

With redux-thunk, action creators can return functions instead of plain action objects.

**How it works:**

**How Redux-Thunk Works**

**Configuration:** When setting up the Redux store, use applyMiddleware to incorporate redux-thunk.

**Action Dispatch:** When an action creator returns a function, redux-thunk intercepts it. This function is then given dispatch as its first parameter.

**Async Control:** Inside the function, you can manually dispatch actions, often used to denote the start and completion of an asynchronous flow.

**Examples:**

**Code Example:** Using Thunk Middleware in Redux

Here is the JavaScript code:

```js
// configureStore.js
import { createStore, applyMiddleware } from "redux";
import thunk from "redux-thunk";
import rootReducer from "./reducers";

const store = createStore(rootReducer, applyMiddleware(thunk));
export default store;

// actions.js
export const fetchData = () => {
  return async (dispatch) => {
    dispatch({ type: "FETCH_DATA_REQUEST" });
    try {
      const data = await someAPICall();
      dispatch({ type: "FETCH_DATA_SUCCESS", payload: data });
    } catch (err) {
      dispatch({ type: "FETCH_DATA_FAILURE", error: err.message });
    }
  };
};

// reducer.js
const initialState = { data: null, loading: false, error: null };
const reducer = (state = initialState, action) => {
  switch (action.type) {
    case "FETCH_DATA_REQUEST":
      return { ...state, loading: true, error: null };
    case "FETCH_DATA_SUCCESS":
      return { ...state, data: action.payload, loading: false };
    case "FETCH_DATA_FAILURE":
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
<summary>What is a "selector" in Redux and what is its purpose?</summary>

### What is a "selector" in Redux and what is its purpose?

**Definition:**

A selector in Redux is a pure function that efficiently computes derived data from the Redux store state.

**How it works:**

**Purpose**

The key purpose of selectors is to offer:

**Data Transformation:** Selectors calculate computed data, such as derived state or complex aggregations.

**Performance Optimization:** They help prevent expensive recalculations and re-renders in components by caching results.

**Core Segments**

**State Slice:** Selects a relevant part of the application state.

**Data Transformation:** Computes and processes the state slice to generate derived data.

**Caching Mechanism:** Efficiently handles and updates the selector's result cache.

**Role in Redux Architecture**

**Input Source:** Selectors receive the complete state object from the store as their input.

**Centralized Data Access:** They serve as the primary data source for Redux-connected components.

**Efficiency and Consistency:** By transforming and caching state data, selectors improve overall application performance.

**Examples:**

**Code Example:** Selectors in Redux

Here is the JavaScript code:

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
  return state.products.list.filter(
    (product) => product.category === "electronics",
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
  (products) =>
    products.filter((product) => product.category === "electronics"),
);

// Add a new product to trigger re-calculation
appState.products.list.push({
  id: 4,
  name: "Product 4",
  price: 300,
  category: "electronics",
});
// Re-run the memoized selector
console.log(memoizedElectronicsSelector(appState));
// Output: [ { id: 1, name: "Product 1", price: 100, category: "electronics" }, { id: 3, name: "Product 3", price: 200, category: "electronics" }, { id: 4, name: "Product 4", price: 300, category: "electronics" } ]
```

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>How does Redux handle the flow of data and actions?</summary>

### How does Redux handle the flow of data and actions?

**Definition:**

In Redux, the unidirectional data flow concept ensures data consistency by using one-way channels for state and actions. Let's look at a more detailed overview:

**How it works:**

**Key Elements**

**Actions:** These are objects with a type attribute, describing what needs to change in the application. Actions are dispatched from components and received by reducers.

**Reducers:** These are pure functions that specify how the application's state should transform in response to actions.

**Store:** It's a single source of truth. It holds the application's state tree and is responsible for:

- State modification through reducers.
- State dispatch as actions are sent.

**One-Way Data Flow**

**Action Dispatch:** Components dispatch actions (through store.dispatch(action)).

**Reducer Receipt:** The store sends the action to the appropriate reducers.

**State Changes:** The reducer modifies the state based on the dispatched action.

**State Subscription:** The modified state is broadcast to subscribed components.

**Sources:**

- [Devinterview.io — Redux](https://devinterview.io/questions/web-and-mobile-development/redux-interview-questions/)

</details>

<details>
<summary>Can you explain the difference between a Container and Presentational component in Redux?</summary>

### Can you explain the difference between a Container and Presentational component in Redux?

**Definition:**

A presentational component receives data and callbacks as props and renders UI. A container reads the store and passes that data down. The style guide says the split is still valid, and hooks are now the default way to read the store.

**How it works:**

- **Presentational**: It does not import the store. The same component can render a static preview if you pass props yourself.
- **Container**: `connect` was the old container: a wrapper that subscribes and passes `stateProps` and `dispatchProps`. The style guide says hooks lead you to call `useSelector` and `useDispatch` in the component that needs the data.
- **Granular reads**: The style guide says to connect more components. A list selects ids; each row selects its own item. That is the opposite of one container selecting the whole tree.

**When to use:**

- Use a props-only component when the UI is reused or tested without a store.
- Call the hooks in the component that displays the data when the value is used in one place.

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
<summary>What is middleware in Redux and when would you use it?</summary>

### What is middleware in Redux and when would you use it?

**Definition:**

Middleware wraps `dispatch`. It sees every action before the reducers, and it may log, delay, change, or stop that action. It is also where side effects belong, because reducers must stay pure.

**How it works:**

- **Shape**: The signature in the `applyMiddleware` docs is `({ getState, dispatch }) => next => action`. Call `next(action)` to pass it on.
- **Defaults**: `configureStore` adds thunk middleware and checks that catch mutations and non-serializable values in development.
- **Add your own**: Pass `middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(myMiddleware)`. Returning a bare array drops the defaults.

**When to use:**

- Use middleware for logs, async work, and listeners.
- Do not use it to compute the next state. That belongs in the reducer.

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
<summary>How do you access the Redux store in a React component?</summary>

### How do you access the Redux store in a React component?

**Definition:**

Wrap the tree in `Provider` with the store. In a function component, `useSelector` reads state and `useDispatch` returns `dispatch`. `useStore` returns the store object; the hooks docs say to prefer `useSelector` for state.

**How it works:**

- **Provider**: The fundamentals tutorial says the hooks work only under `Provider`. The style guide says app code should not import the store directly except as a last resort.
- **Read**: `useSelector(selectCount)` runs your function with the state and subscribes the component.
- **Legacy**: `connect(mapStateToProps)` still subscribes a wrapper. It is marked deprecated; `legacy_connect` keeps the API without the editor warning.

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
<summary>What is the purpose of the dispatch function in Redux?</summary>

### What is the purpose of the dispatch function in Redux?

**Definition:**

`dispatch` is the only way to ask the store to change. You pass an action. Middleware may intercept it. Then the root reducer returns the next state and subscribers run.

**How it works:**

- **Plain actions**: A normal action is a plain object with a `type`. `createSlice` builds those action creators for you.
- **Other values**: Thunk middleware lets you dispatch a function. The function receives `dispatch` and `getState`. Reducers never see that function.
- **Return**: The `applyMiddleware` logger example returns whatever `next(action)` returns. For a plain action that is the action, unless a later middleware changed it.

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
<summary>What arguments does createStore receive in Redux?</summary>

### What arguments does createStore receive in Redux?

**Definition:**

`createStore(reducer, preloadedState?, enhancer?)` creates the store. Redux marks it deprecated and tells you to use `configureStore` from Redux Toolkit. `legacy_createStore` is the same function without the deprecation mark.

**How it works:**

- **reducer**: The root reducer `(state, action) => nextState`.
- **preloadedState**: Optional initial state, used to hydrate a server render or restore a session.
- **enhancer**: Optional store enhancer. `applyMiddleware(...)` returns one. `configureStore` takes `reducer`, `preloadedState`, `middleware`, and `enhancers` as named options instead.

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
<summary>Describe the subscribe function in Redux.</summary>

### Describe the subscribe function in Redux.

**Definition:**

`subscribe` registers a listener that runs after an action is dispatched, so the UI can read the new state. React-Redux calls it for you inside `useSelector` and `connect`.

**How it works:**

- **No payload**: The listener is not given the action. Call `store.getState()` inside it.
- **Who should call it**: The style guide says application components should not import the store. The `Provider` owns the subscription.
- **Tests and middleware**: A test or a non-React host can subscribe, dispatch, and assert `getState()`. Middleware already has `getState` and does not need a subscription to see the action.

**Sources:**

- [createStore](https://redux.js.org/api/createStore)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

### Working with Redux Actions

<details>
<summary>How do you structure action creators in Redux?</summary>

### How do you structure action creators in Redux?

**Definition:**

With Redux Toolkit, action creators come from `createSlice`. The slice `name` prefixes each `type`, and the function returns `{ type, payload }`.

**How it works:**

- **Case reducers**: Each key under `reducers` becomes an action creator on `slice.actions` and a handler that may use Immer.
- **Events**: The style guide says to model actions as events (`todoToggled`) rather than setters (`setCompleted`).
- **Logic lives in the reducer**: Pass the id in the payload. Compute the next todo in the reducer so the click handler stays thin and the logic is easy to test.

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
<summary>What is an action type and how is it used in Redux?</summary>

### What is an action type and how is it used in Redux?

**Definition:**

`action.type` is a string the reducer switches on. Redux only requires that it is defined. Toolkit types look like `todos/todoToggled`.

**How it works:**

- **Match**: The slice reducer handles the types it generated. `extraReducers` handles types from other slices or from `createAsyncThunk`.
- **Prefix**: The reusing-logic page shows `counterB/incremented` because the slice name is `counterB`. Two slices can share reducer functions and still not catch each other’s actions.
- **Naming**: The style guide prefers an event name over `setX`. The string is what shows up in the DevTools action log.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Reusing reducer logic](https://redux.js.org/usage/structuring-reducers/reusing-reducer-logic)
</details>

<details>
<summary>Can you give an example of a synchronous action creator?</summary>

### Can you give an example of a synchronous action creator?

**Definition:**

A synchronous creator returns a plain action object. Nothing is fetched. `createSlice` generates that function from the reducer key.

**How it works:**

- **Generated**: `todoToggled(id)` returns `{ type: "todos/todoToggled", payload: id }`.
- **Dispatch**: The component calls `dispatch(todoToggled(id))` on click. The reducer updates that todo.
- **Hand-written**: You can still return `{ type, payload }` yourself. The style guide says new code should go through Toolkit so the type and the reducer stay together.

**Examples:**

```js
dispatch(todoToggled(id));
```

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>How do you handle complex state logic in actions?</summary>

### How do you handle complex state logic in actions?

**Definition:**

Put the calculation in the reducer, not in the action. The style guide says reducers are easy to test and that preparing the next state outside them leads to accidental mutations.

**How it works:**

- **Payload stays small**: Send the id. The reducer finds the todo and flips `completed` with Immer.
- **Avoid blind replace**: The style guide warns against `return action.payload` unless you have a reason, such as a form where one field-per-action would be noise.
- **Prepare**: When the click must invent an id, keep that to a minimum. The rest of the update still belongs in the reducer.

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
<summary>Explain the concept of action creators returning a function for asynchronous operations.</summary>

### Explain the concept of action creators returning a function for asynchronous operations.

**Definition:**

A thunk is a function you dispatch. The thunk middleware calls it with `dispatch` and `getState` and does not pass the function to reducers. Redux Toolkit adds this middleware in `configureStore`.

**How it works:**

- **Why a function**: The fundamentals page shows a middleware that, when `typeof action === "function"`, calls `action(dispatch, getState)`. That is the thunk pattern.
- **createAsyncThunk**: The essentials tutorial says this is the standard way to write async logic. `createAsyncThunk` returns a thunk that dispatches `pending`, `fulfilled`, and `rejected`.
- **Not in the reducer**: The style guide forbids AJAX, timers, and `Date.now()` inside reducers. The thunk performs the request, then dispatches a plain result action.

**Examples:**

```js
const fetchUserById = createAsyncThunk(
  "users/fetchByIdStatus",
  async (userId) => {
    const response = await userAPI.fetchById(userId);
    return response.data;
  },
);
dispatch(fetchUserById(123));
```

**Sources:**

- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
- [Fundamentals — async logic](https://redux.js.org/tutorials/fundamentals/part-6-async-logic)
- [Redux Toolkit usage guide](https://redux-toolkit.js.org/usage/usage-guide)
</details>

### Reducers and State Management

<details>
<summary>How do reducers handle state changes in response to different action types?</summary>

### How do reducers handle state changes in response to different action types?

**Definition:**

The root reducer calls each slice reducer with the same action. A slice returns a new state only for types it handles, and returns the current state for the rest.

**How it works:**

- **createSlice**: Keys under `reducers` handle the slice’s own types. `extraReducers` with `builder.addCase` handles thunk lifecycle actions.
- **Immer**: Assigning `state.entities.push(...)` inside `createSlice` is drafted by Immer. The real state is not mutated. The style guide says `configureStore` checks for mutations in development.
- **Default**: If no case matches, the slice state stays as it was. That includes the first call, which uses `initialState` when the state argument is undefined.

**Examples:**

```js
extraReducers: (builder) => {
  builder.addCase(fetchUserById.fulfilled, (state, action) => {
    state.entities.push(action.payload);
  });
};
```

**Sources:**

- [Redux Toolkit usage guide](https://redux-toolkit.js.org/usage/usage-guide)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>What is meant by a "slice reducer"?</summary>

### What is meant by a "slice reducer"?

**Definition:**

A slice reducer owns one branch of the state tree, such as `state.todos`. `createSlice` builds that reducer and its actions. `configureStore` combines the slices you pass in `reducer`.

**How it works:**

- **One feature**: The style guide puts `todosSlice.ts` in a feature folder and exports the reducer and the action creators from that file.
- **Combine**: The getting-started store is `reducer: { todos: todosReducer, posts: postsReducer }`. Each key is one slice of state.
- **Not the root**: The slice does not see sibling state. Cross-feature reactions go through `extraReducers` or a listener, using actions the other slice dispatched.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
</details>

<details>
<summary>Describe how to properly update the state based on an action.</summary>

### Describe how to properly update the state based on an action.

**Definition:**

Return a new value or assign through Immer inside `createSlice`. Do not mutate a state object that reducers share. The style guide calls mutation the most common Redux bug.

**How it works:**

- **Immer draft**: In a slice reducer, `todo.completed = !todo.completed` updates a draft. Immer produces the next immutable state.
- **Copy by hand**: Without Immer, copy every ancestor you change: `{ ...state, items: state.items.map(...) }`.
- **Ownership**: Read fields you know. Avoid `return { ...state, ...action.payload }` unless a form update is the deliberate exception the style guide allows.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>How would you reset the state of a Redux store?</summary>

### How would you reset the state of a Redux store?

**Definition:**

A slice returns its `initialState` for a reset action. To reset the whole tree, the root reducer returns `undefined` for that action so every slice runs again with its initial state.

**How it works:**

- **One slice**: A case reducer can `return initialState`. That replaces the branch.
- **Whole store**: Wrap the combined reducer. On `app/reset`, call it as `combined(undefined, action)` so each slice sees a first-time call.
- **Do not mutate**: Assigning fields back one by one is easy to get wrong. Replacing the branch with the initial value keeps the shape the slice declared.

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
<summary>Can you discuss reducer composition and its benefits?</summary>

### Can you discuss reducer composition and its benefits?

**Definition:**

Composition means a reducer calls other reducers. `configureStore` combines slice reducers. A higher-order reducer wraps one reducer so several copies can live in one store.

**How it works:**

- **Slices**: Each feature reducer sees only its branch and the full action. The root state is the object of their results.
- **Reuse**: The reusing-logic page shows `makeCounterSlice(name)` so `counterA` and `counterB` share the same cases but get different action types.
- **Benefit**: You test a slice without the rest of the app. You can change one feature’s state shape without rewriting the others.

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
<summary>What is the significance of immutability in reducer functions?</summary>

### What is the significance of immutability in reducer functions?

**Definition:**

Reducers must not change the existing state object. `useSelector` and DevTools time travel depend on new references. The style guide says mutation is the usual reason a connected component does not re-render.

**How it works:**

- **Checks**: `configureStore` includes an immutability check in development.
- **Immer**: The style guide recommends Immer, which `createSlice` uses. Draft assignments are allowed because the real data is not mutated.
- **Copies**: Changing a copy is normal. Changing an argument or a value you closed over is not.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>How do you handle initial state in Redux reducers?</summary>

### How do you handle initial state in Redux reducers?

**Definition:**

Pass `initialState` to `createSlice`. The slice uses it when the store is created and whenever that reducer is called with `undefined`.

**How it works:**

- **Slice**: The object you pass is the shape of that branch: `{ entities: [], loading: "idle" }` in the usage-guide example.
- **Hydration**: `preloadedState` on `configureStore` or `createStore` overrides that initial value for a server-rendered page or a restored session.
- **Entity adapter**: `booksAdapter.getInitialState()` returns `{ ids: [], entities: {} }`.

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
<summary>What are common pitfalls when writing reducers?</summary>

### What are common pitfalls when writing reducers?

**Definition:**

Mutating state, doing I/O, and trusting `action.payload` to be the next state are the failures the style guide calls out.

**How it works:**

- **Mutation**: Pushing into the real array breaks DevTools and can skip a render. Use a slice so Immer copies for you, and leave the development checks on.
- **Side effects**: No `fetch`, timers, or `Math.random()` in the reducer. A thunk or listener does that work and then dispatches a plain action.
- **Blind write**: `return action.payload` will store whatever was dispatched, including the wrong kind of object. Read the fields you expect.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

### Redux Middleware and Side Effects

<details>
<summary>What is redux-thunk and how does it work?</summary>

### What is redux-thunk and how does it work?

**Definition:**

redux-thunk teaches `dispatch` to accept a function. `configureStore` includes it by default. The function may `await` a request and then dispatch plain actions.

**How it works:**

- **Call**: If the dispatched value is a function, the middleware invokes it with `dispatch` and `getState` and does not call the reducers with that function.
- **Toolkit helper**: `createAsyncThunk("users/fetchByIdStatus", async (userId) => ...)` dispatches pending, then fulfilled with the return value, or rejected if the promise fails.
- **Reducer**: Handle `fetchUserById.fulfilled` in `extraReducers`. The reducer still only sees plain actions.

**Examples:**

```js
dispatch(fetchUserById(123));
```

**Sources:**

- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
- [Fundamentals — async logic](https://redux.js.org/tutorials/fundamentals/part-6-async-logic)
</details>

<details>
<summary>Can you explain the purpose of redux-saga and how it compares to redux-thunk?</summary>

### Can you explain the purpose of redux-saga and how it compares to redux-thunk?

**Definition:**

redux-saga runs generator functions in middleware in response to actions. redux-thunk runs a function you dispatched. The essentials tutorial says thunks are the standard approach, and `configureStore` installs thunk for you.

**How it works:**

- **Saga**: The getting-started page uses `takeEvery` or `takeLatest`, then `yield call(Api.fetchUser, id)` and `yield put({ type, user })`. `takeLatest` cancels an older in-flight fetch.
- **Wire-up**: `createSagaMiddleware()`, `getDefaultMiddleware().concat(sagaMiddleware)`, then `sagaMiddleware.run(mySaga)`.
- **Thunk**: A thunk is an async function closed over the work. There is no `takeLatest` unless you add it. RTK Query is what the essentials tutorial recommends when the work is data fetching rather than a long process.

**When to use:**

- Use thunks or RTK Query for requests.
- Use a saga when you need to cancel or sequence long-running processes and you already depend on redux-saga.

**Examples:**

```js
const sagaMiddleware = createSagaMiddleware();
const store = configureStore({
  reducer,
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(sagaMiddleware),
});
sagaMiddleware.run(mySaga);
```

**Sources:**

- [redux-saga getting started](https://redux-saga.js.org/docs/introduction/GettingStarted)
- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
</details>

<details>
<summary>What is a "side effect" and how do middlewares handle them in Redux?</summary>

### What is a "side effect" and how do middlewares handle them in Redux?

**Definition:**

A side effect is work visible outside the return value: HTTP, timers, logging, random ids. Reducers must not do it. Middleware runs that work, then dispatches plain actions.

**How it works:**

- **Examples**: The fundamentals page lists logging, files, timers, HTTP, mutating outside state, and `Date.now()` or `Math.random()`.
- **Where**: A thunk, a saga, or `createListenerMiddleware` can `fetch` and then `dispatch`. The listener docs say to `prepend` it so it runs before the serializability check, because listener actions may contain functions.
- **Reducer rule**: Time-travel re-runs reducers. An effect inside one would repeat during debugging.

**Sources:**

- [Fundamentals — async logic](https://redux.js.org/tutorials/fundamentals/part-6-async-logic)
- [createListenerMiddleware](https://redux-toolkit.js.org/api/createListenerMiddleware)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>How do you write a custom middleware for Redux?</summary>

### How do you write a custom middleware for Redux?

**Definition:**

Write `({ getState, dispatch }) => (next) => (action) => { ... }` and return the result of `next(action)` unless you mean to stop the action.

**How it works:**

- **Chain**: The `applyMiddleware` docs say each layer receives the store API and returns a function of `next`. The last `next` is the store’s real `dispatch`.
- **Logger**: The documented logger prints the action, calls `next(action)`, then prints `getState()`.
- **Install**: With Toolkit, `getDefaultMiddleware().concat(logger)`. Do not pass the middleware list without calling `getDefaultMiddleware` if you still want thunk and the checks.

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
<summary>Discuss how middlewares can enhance the capabilities of a Redux store.</summary>

### Discuss how middlewares can enhance the capabilities of a Redux store.

**Definition:**

Middleware is how the store grows features without putting them in reducers: async functions, dev checks, listeners, and a log of every action.

**How it works:**

- **Built in**: `configureStore` enables DevTools and adds thunk plus immutability and serializability checks.
- **Async**: Thunks and sagas dispatch later plain actions. RTK Query’s middleware, added with `.concat(api.middleware)`, tracks cache lifetimes.
- **Listeners**: `startListening` runs an effect when an action creator matches, and can `dispatch`, `delay`, and `fork` from the listener API.

**Sources:**

- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
- [createListenerMiddleware](https://redux-toolkit.js.org/api/createListenerMiddleware)
</details>

### Redux and UI Integration

<details>
<summary>How do you connect a React component to a Redux store?</summary>

### How do you connect a React component to a Redux store?

**Definition:**

Render `Provider store={store}` at the root. Then call `useSelector` and `useDispatch` in the component. `connect` still works and is deprecated in React-Redux 9.

**How it works:**

- **Hooks**: The getting-started counter reads `selectCount` and dispatches `increment` from the click handler.
- **connect**: `connect(mapStateToProps, mapDispatchToProps)(Component)` returns a wrapper. It does not change the component you passed.
- **One store**: The style guide says an app should have a single store, passed through `Provider`.

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
<summary>What are the advantages of using React-Redux's connect function?</summary>

### What are the advantages of using React-Redux's connect function?

**Definition:**

connect subscribes a wrapper and passes store data as props, so the inner component can stay unaware of Redux. The current docs mark it deprecated and recommend `useSelector` and `useDispatch`.

**How it works:**

- **Props boundary**: The style guide says the wrapper was useful for presentational components and for testing them with plain props.
- **Cost**: The same page says `connect` is hard to follow and hard to type because of `mapState`, `mapDispatch`, and merged props. Hooks take one selector.
- **If you keep it**: Import `legacy_connect as connect` to skip the strikethrough. Prefer the object form of `mapDispatch` when you still use it.

**When to use:**

- Keep `connect` when a class component or an existing wrapper cannot move to hooks yet.
- Use hooks in new function components.

**Sources:**

- [connect()](https://react-redux.js.org/api/connect)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Can you describe the mapStateToProps and mapDispatchToProps functions?</summary>

### Can you describe the mapStateToProps and mapDispatchToProps functions?

**Definition:**

They are the first two arguments to `connect`. `mapStateToProps(state, ownProps?)` returns props from state. `mapDispatchToProps` returns callbacks that dispatch, or an object of action creators.

**How it works:**

- **State**: A one-argument function runs when the store changes. A two-argument function also runs when the wrapper’s own props change, and receives those props as `ownProps`.
- **Dispatch**: If you omit it, the component receives `dispatch`. The style guide says to pass an object of action creators instead of a function when you still use `connect`.
- **Re-render**: The docs say the returned state props decide whether the wrapped component re-renders.

**Examples:**

```js
connect((state, ownProps) => ({ todo: state.todos[ownProps.id] }), {
  todoToggled,
})(TodoRow);
```

**Sources:**

- [connect()](https://react-redux.js.org/api/connect)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Explain the Provider component from React-Redux.</summary>

### Explain the Provider component from React-Redux.

**Definition:**

Provider puts the store on React context. `useSelector`, `useDispatch`, and `connect` read that store. Without it they cannot see your `configureStore` result.

**How it works:**

- **Once**: The style guide says one store per app, usually in `store.js`, passed into `Provider` at the root.
- **Hooks**: The fundamentals tutorial introduces `Provider` before `useSelector` and `useDispatch`.
- **RTK Query**: The overview mentions an `ApiProvider` only if you do not already have a store. A normal app uses the existing `Provider` and adds the API reducer and middleware to that store.

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
<summary>How do you use hooks like useDispatch and useSelector in React functional components?</summary>

### How do you use hooks like useDispatch and useSelector in React functional components?

**Definition:**

Call `useSelector` with a function from state to a value. Call `useDispatch` once to get `dispatch`. Both require `Provider`.

**How it works:**

- **Select**: Return a primitive or a stable reference when you can. If you return a new object, pass `shallowEqual` or the component renders on every dispatch.
- **Several calls**: The style guide says to call `useSelector` many times for small values instead of one selector that returns a large object.
- **Dispatch**: `dispatch(increment())` in the click handler. For a request, dispatch the thunk from `createAsyncThunk`.

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
<summary>What performance considerations should be taken into account when connecting components to Redux?</summary>

### What performance considerations should be taken into account when connecting components to Redux?

**Definition:**

A component re-renders when its selected value changes by the equality check. Selecting a big new object, or selecting high in the tree, makes more of the UI render than the change required.

**How it works:**

- **Many subscribers**: The style guide’s example: the list selects user ids, and each `UserRow` selects one user. An update to one user does not have to render every row.
- **Equality**: `useSelector` uses strict equality unless you pass `shallowEqual`. A selector that returns `{ a, b }` is a new object every time.
- **Normalized state**: The normalizing page says a nested update copies ancestors and wakes every selector that read those ancestors. An id lookup touches a smaller piece.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Hooks API](https://react-redux.js.org/api/hooks)
- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
</details>

<details>
<summary>How can you optimize the performance of Redux-connected components?</summary>

### How can you optimize the performance of Redux-connected components?

**Definition:**

Select the smallest value you can, in the component that renders it. Memoize derived data with `createSelector`, which Redux Toolkit re-exports.

**How it works:**

- **Where**: The style guide says more subscriptions beat one parent that selects a whole collection and passes it down.
- **Equality**: Pass `shallowEqual` from `react-redux` when the selector must return an object or array.
- **Derived data**: The style guide says to keep calculated values out of the store and memoize them with Reselect so a list filter does not run on unrelated actions.

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
<summary>What are Higher Order Reducers (HORs) and how are they used?</summary>

### What are Higher Order Reducers (HORs) and how are they used?

**Definition:**

A higher-order reducer takes a reducer and returns a reducer. `combineReducers` is one. You use the pattern to reuse one piece of logic for many state keys.

**How it works:**

- **Filter**: The docs show `createNamedWrapperReducer` and `createFilteredReducer`, which skip the inner reducer unless the action is meant for that copy.
- **Toolkit**: `makeCounterSlice(name)` calls `createSlice` with that name, so the generated types are `counterB/incremented` and do not update `counterA`.
- **Shared cases**: Define the case functions once and pass them into each `createSlice` if several slices should share behavior and keep their own types.

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
<summary>Can you implement a feature toggle mechanism using Redux?</summary>

### Can you implement a feature toggle mechanism using Redux?

**Definition:**

Store the flag in a slice and read it with `useSelector`. Toggling it is a normal action. The reducer owns the boolean so DevTools can show the change.

**How it works:**

- **State**: Keep `{ flags: { newCheckout: false } }` in serializable plain data, which the style guide requires.
- **Update**: An event such as `flagToggled` receives the flag name and flips that field inside the slice.
- **UI**: The component selects `state.flags.newCheckout` and chooses which child to render. Do not read the store from a random module.

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
<summary>How do you normalize a state shape in Redux?</summary>

### How do you normalize a state shape in Redux?

**Definition:**

Store each entity type as a lookup by id plus an array of ids. The normalizing page says `createEntityAdapter` uses `{ ids, entities }` for that.

**How it works:**

- **Shape**: Items live once, under their id. Other records store ids, not nested copies.
- **Adapter**: `createEntityAdapter` gives `getInitialState`, `addOne`, `setAll`, and selectors such as `selectAll`.
- **Nested responses**: The page says APIs often return nested data. Normalize before it enters the store. Normalizr can do that transform; it is no longer actively maintained.

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
<summary>What is a Redux "slice" and how is it used?</summary>

### What is a Redux "slice" and how is it used?

**Definition:**

A slice is the reducer, actions, and initial state for one feature, created with `createSlice` and mounted on the store under one key.

**How it works:**

- **File**: The style guide calls the single slice file the ducks pattern and prefers it to separate `actions/` and `reducers/` folders.
- **Store**: `configureStore({ reducer: { todos: todosReducer } })` runs that reducer for `state.todos`.
- **Exports**: Export `todosSlice.reducer` and `todosSlice.actions`. Components dispatch the actions and select from that branch.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
</details>

<details>
<summary>Explain the "Ducks" pattern in Redux.</summary>

### Explain the "Ducks" pattern in Redux.

**Definition:**

Ducks means one module owns a feature’s action types, action creators, and reducer. In current Redux that module is a `createSlice` file.

**How it works:**

- **Colocate**: The style guide’s tree is `features/todos/todosSlice.ts` next to `Todos.tsx`, not a global actions folder.
- **Prefix**: The slice name prefixes types, which is the same isolation older ducks files got from hand-written constants.
- **App folder**: `app/store.ts` only configures the store. It does not contain feature reducers.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

### Redux State Normalization

<details>
<summary>Why is state normalization recommended in Redux?</summary>

### Why is state normalization recommended in Redux?

**Definition:**

Nested copies go stale, force deep reducer code, and change ancestor references so unrelated `useSelector` calls re-render.

**How it works:**

- **One place**: The normalizing page says an item defined once is updated once.
- **Simpler updates**: Lookup by id replaces a deep immutable path.
- **Renders**: Changing a comment should copy the comment record, not every post that displayed it.

**Sources:**

- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
</details>

<details>
<summary>How do you structure a normalized state?</summary>

### How do you structure a normalized state?

**Definition:**

Each entity type gets `ids` and `entities`, or the page’s teaching names `allIds` and `byId`. Relational tables can sit under an `entities` key beside `ui` state.

**How it works:**

- **Lookup**: `entities[id]` is the record. `ids` is the order.
- **Relations**: A post stores `author` and `comments` as ids. The author object is not nested inside every post.
- **Adapter**: `getInitialState()` starts at `{ ids: [], entities: {} }`. `selectId` tells the adapter which field is the id when it is not `id`.

**Sources:**

- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
- [createEntityAdapter](https://redux-toolkit.js.org/api/createEntityAdapter)
</details>

<details>
<summary>What are the benefits of state denormalization?</summary>

### What are the benefits of state denormalization?

**Definition:**

Denormalization means assembling the nested object the UI wants at read time. The store stays flat; a selector joins ids to records.

**How it works:**

- **Read model**: A post view needs the author name. The selector reads `users.entities[post.author]` instead of storing that name on every post.
- **Memoize**: The style guide says to memoize that join with `createSelector` so it runs when the post or the author changes, not on every action.
- **Write model**: Updates still hit one table. You do not search the tree for every copy of the user.

**Sources:**

- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Can you explain how to manage entity relations in a normalized Redux state?</summary>

### Can you explain how to manage entity relations in a normalized Redux state?

**Definition:**

Store ids on the entity when you always navigate one way. Use a join table when the relationship is many-to-many or has its own fields.

**How it works:**

- **One to many**: The page’s author stores `bookIds`. Books live in `books.byId`.
- **Join**: `authorBook` rows hold `authorId`, `bookId`, and `role`. Lookups scan that table.
- **Index later**: Add `bookIdsByAuthorId` only if a measured read is slow, and update that index from the same actions that change the join rows.

**Sources:**

- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
</details>

### Redux Ecosystem and Utilities

<details>
<summary>What is Immutable.js and how can it be used with Redux?</summary>

### What is Immutable.js and how can it be used with Redux?

**Definition:**

Immutable.js is a library of persistent collections. The style guide’s recommendation for Redux is Immer inside `createSlice`, and plain objects in the store.

**How it works:**

- **Serializable**: The style guide says not to put Maps, Sets, or class instances in state or actions. DevTools and the UI depend on plain data.
- **Immer**: `createSlice` already lets you write mutating syntax against a draft. You do not wrap the store in another collection type.
- **If it is already there**: Convert to plain objects at the edge before `dispatch`. Do not select an Immutable structure from a new slice.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Discuss the Redux DevTools extension and its features.</summary>

### Discuss the Redux DevTools extension and its features.

**Definition:**

DevTools records actions and state so you can inspect the log and move through it. `configureStore` turns that on. It works when state and actions stay serializable and reducers stay pure.

**How it works:**

- **Setup**: The getting-started page says `configureStore` enables the Redux DevTools Extension. You do not wire the enhancer by hand.
- **Time travel**: The style guide says DevTools re-runs reducers from earlier actions. A side effect in a reducer would repeat. A mutation makes the history lie.
- **What you see**: Action types from slices and from `createAsyncThunk` (`pending`, `fulfilled`, `rejected`) show up in that log.

**Sources:**

- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>How do reselect and memoized selectors work in Redux?</summary>

### How do reselect and memoized selectors work in Redux?

**Definition:**

A memoized selector caches its result and recomputes when its inputs change. Redux Toolkit re-exports Reselect’s `createSelector` for derived data.

**How it works:**

- **Why**: The style guide says deriving data in selectors keeps the store smaller and avoids extra fields you must keep in sync.
- **Inputs**: Input selectors read slices. The result function joins them. If the inputs are the same references, the cached result is returned.
- **Entity selectors**: `adapter.getSelectors()` returns memoized selectors for `selectAll` and the other lookups, scoped by the function you pass.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [createEntityAdapter](https://redux-toolkit.js.org/api/createEntityAdapter)
</details>

<details>
<summary>What are common utilities and libraries used in conjunction with Redux?</summary>

### What are common utilities and libraries used in conjunction with Redux?

**Definition:**

Redux Toolkit is the set the style guide tells you to use: `configureStore`, `createSlice`, Immer, thunk, and Reselect. RTK Query covers fetching. React-Redux connects React.

**How it works:**

- **Core**: The store is still Redux. Toolkit configures it and writes the reducers.
- **UI**: `Provider`, `useSelector`, and `useDispatch` are React-Redux. `connect` remains for older components.
- **Data**: `createAsyncThunk` for a single request lifecycle. `createApi` when you want cache keys, loading flags, and hooks generated from endpoints.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
- [React-Redux getting started](https://react-redux.js.org/introduction/getting-started)
</details>

<details>
<summary>Describe the significance of the Redux Toolkit and how it helps Redux development.</summary>

### Describe the significance of the Redux Toolkit and how it helps Redux development.

**Definition:**

Redux Toolkit is the supported way to write Redux. It combines reducers, adds thunk and DevTools, and uses Immer so immutable updates are not hand-copied.

**How it works:**

- **Store**: `configureStore({ reducer: { todos, posts } })` replaces manual `combineReducers`, `applyMiddleware`, and DevTools setup.
- **Logic**: `createSlice` generates action types and creators and lets reducers assign to a draft.
- **Not required**: The style guide says you may skip RTK, and that using it is how you get the recommended defaults.

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
<summary>How do you test Redux reducers?</summary>

### How do you test Redux reducers?

**Definition:**

Call the reducer with a state and an action and assert the result. The style guide says that is why logic should live in the reducer.

**How it works:**

- **Pure**: `const result = reducer(testState, action)` needs no store, no DOM, and no network.
- **Slice**: Dispatch the generated action creator’s result: `todosReducer(initialState, todoToggled(id))`.
- **Immer**: Assert the returned state. Do not assert that the input state object changed; the check middleware would fail a real mutation.

**Examples:**

```js
const next = todosReducer(initialState, todoToggled("1"));
expect(next.entities["1"].completed).toBe(true);
```

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>What is the recommended approach to test Redux actions?</summary>

### What is the recommended approach to test Redux actions?

**Definition:**

Assert the plain object a sync creator returns. For a thunk, run it with a fake `dispatch` and `getState`, or dispatch it through a real `configureStore` and assert the actions the store recorded.

**How it works:**

- **Sync**: `todoToggled("1")` should equal `{ type: "todos/todoToggled", payload: "1" }`.
- **Thunk**: The essentials page says `createAsyncThunk` dispatches pending and then fulfilled or rejected. Assert those types and the payload.
- **Prefer reducer tests**: The style guide says the behavior that matters is the next state, which the reducer test already covers.

**Sources:**

- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>How can you test a Redux-connected React component?</summary>

### How can you test a Redux-connected React component?

**Definition:**

Render it under `Provider` with a store whose `preloadedState` is the case you care about. Assert the text, then click and assert the store or the next render.

**How it works:**

- **Store**: `configureStore` accepts `preloadedState`, the same option `createStore` uses to hydrate state.
- **Hooks**: The component calls `useSelector` and `useDispatch` as in production, so the test needs the same `Provider`.
- **Presentational**: The style guide notes that a props-only child can be rendered without a store. Use that when the test is about markup, not about the subscription.

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
<summary>Describe the process of testing asynchronous action creators.</summary>

### Describe the process of testing asynchronous action creators.

**Definition:**

Dispatch the thunk to a test store and wait until the fulfilled or rejected action is processed. Stub the HTTP client the payload function calls.

**How it works:**

- **Lifecycle**: `createAsyncThunk` dispatches `pending`, then `fulfilled` with the returned data, or `rejected` on failure. The usage guide shows the fulfilled handler in `extraReducers`.
- **Assert state**: After the promise settles, `store.getState()` should show the loaded entity or the error field. That tests the reducer and the thunk together.
- **No timers in reducers**: Fake the API at the boundary. Do not put the clock inside the slice.

**Sources:**

- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
- [Redux Toolkit usage guide](https://redux-toolkit.js.org/usage/usage-guide)
</details>

<details>
<summary>What tools can you use to facilitate testing in Redux applications?</summary>

### What tools can you use to facilitate testing in Redux applications?

**Definition:**

Use the store from `configureStore`, the reducer as a plain function, and DevTools while you develop. RTK Query’s behavior shows up as the actions and cache state in that store.

**How it works:**

- **Unit**: Reducer tests need no library beyond the assertion. Pass `todoToggled(id)` in.
- **Integration**: A `Provider` plus the UI test runner renders the connected component. `preloadedState` sets the fixture.
- **DevTools**: `configureStore` enables the extension so a failing flow can be inspected as an action list.

**Sources:**

- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

### Redux with Other Frameworks and Libraries

<details>
<summary>How do you integrate Redux with a framework other than React, like Angular or Vue?</summary>

### How do you integrate Redux with a framework other than React, like Angular or Vue?

**Definition:**

The store does not depend on React. Any UI can call `getState`, `dispatch`, and `subscribe`. React-Redux is only the React binding.

**How it works:**

- **Store**: `configureStore` returns that store. The style guide says one store, and UI code should not import it except at the boundary.
- **Subscribe**: `createStore` documents `subscribe` so a view can refresh after `dispatch`. A Vue or Angular adapter does that subscription for you; you still write slices the same way.
- **RTK Query**: The overview says the core is UI-agnostic and that there are examples for other UI libraries. The React hooks come from `@reduxjs/toolkit/query/react`.

**Sources:**

- [createStore](https://redux.js.org/api/createStore)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Can Redux be used with server-side rendering frameworks like Next.js?</summary>

### Can Redux be used with server-side rendering frameworks like Next.js?

**Definition:**

Yes. Render on the server with a store, serialize `getState()`, and pass that object as `preloadedState` when you create the store in the browser.

**How it works:**

- **preloadedState**: Both `createStore` and `configureStore` accept it so a universal app can hydrate from the server.
- **One request, one store**: Create the store per request on the server. Do not share one module-level store across users.
- **Plain data**: The style guide’s serializable-state rule is what makes `getState()` safe to embed in HTML.

**Examples:**

```js
const store = configureStore({ reducer, preloadedState });
```

**Sources:**

- [createStore](https://redux.js.org/api/createStore)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Discuss the use of Redux with React Native.</summary>

### Discuss the use of Redux with React Native.

**Definition:**

React Native renders with React, so the same `Provider`, `useSelector`, and `useDispatch` apply. The store is still created with `configureStore`.

**How it works:**

- **UI binding**: The React-Redux getting-started hooks do not depend on the DOM. They depend on `Provider`.
- **Persistence**: Reloading the app drops memory. Save serializable state yourself and pass it back as `preloadedState` if the product needs that.
- **Effects**: Calls to native APIs stay in thunks or listeners, not in reducers.

**Sources:**

- [React-Redux getting started](https://react-redux.js.org/introduction/getting-started)
- [Redux style guide](https://redux.js.org/style-guide/)
- [createStore](https://redux.js.org/api/createStore)
</details>

<details>
<summary>What adaptations are needed to use Redux outside of a UI environment (e.g., in a Node.js backend)?</summary>

### What adaptations are needed to use Redux outside of a UI environment (e.g., in a Node.js backend)?

**Definition:**

Create the store, dispatch actions, and read `getState()`. Skip `Provider`. Do not keep one global store for every request if those requests must not see each other’s data.

**How it works:**

- **Same reducers**: Slices stay pure. Node can import them and run `configureStore`.
- **Subscribe**: Use `subscribe` when a non-React caller needs to react. Otherwise dispatch and then read `getState()` once.
- **Isolation**: A store is an object. Two requests need two stores, or a single-threaded batch that never overlaps users.

**Sources:**

- [createStore](https://redux.js.org/api/createStore)
- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
</details>

### Scaling Redux for Large Applications

<details>
<summary>How do you organize your files and folders in a large Redux application?</summary>

### How do you organize your files and folders in a large Redux application?

**Definition:**

Use feature folders. Put the slice next to the components that use it. Keep store setup in `app/`.

**How it works:**

- **Layout**: The style guide shows `app/store.ts`, `features/todos/todosSlice.ts`, and `features/todos/Todos.tsx`.
- **Not by type**: A root `actions/` folder splits one feature across directories. The guide says that is harder to maintain.
- **Common**: `common/` is for hooks and helpers that are not a feature. Feature code stays in its folder.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>What strategies can be used to scale Redux actions and reducers?</summary>

### What strategies can be used to scale Redux actions and reducers?

**Definition:**

One slice per feature, prefixed action types, and shared case reducers only through a factory. Let the store combine them.

**How it works:**

- **Prefix**: `makeCounterSlice("counterB")` yields `counterB/incremented`, so a dispatch does not update every counter.
- **Extra reducers**: A slice listens to another feature’s action in `extraReducers` instead of importing that feature’s state and writing it.
- **Normalize**: Large lists stay in entity adapters so one update does not rewrite a nested tree.

**Sources:**

- [Reusing reducer logic](https://redux.js.org/usage/structuring-reducers/reusing-reducer-logic)
- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
</details>

<details>
<summary>Can you explain the concept of "domain-driven design" in the context of a Redux state tree?</summary>

### Can you explain the concept of "domain-driven design" in the context of a Redux state tree?

**Definition:**

Group the state tree by domain feature, not by technical type. Each feature folder owns its slice of the tree.

**How it works:**

- **Boundaries**: `state.todos` and `state.posts` match the features. A component in todos selects todos and does not reach into another slice’s internals.
- **Events**: The style guide’s event-style actions (`orderAdded`) name what happened in the domain, not a setter for one field.
- **Shared kernel**: Entity tables used by several features can live under `entities`, which the normalizing page shows beside `ui`.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
</details>

<details>
<summary>How do feature modules and code splitting affect Redux state management?</summary>

### How do feature modules and code splitting affect Redux state management?

**Definition:**

You can add a reducer after startup with `replaceReducer`. The state key for a feature appears when that reducer is mounted. Selectors must tolerate a missing branch until then.

**How it works:**

- **replaceReducer**: The configuring-your-store example calls `store.replaceReducer` on hot reload. The same method swaps in a combined reducer that includes a newly loaded feature.
- **Initial state**: The new slice receives `undefined` on its first call and uses its `initialState`.
- **Do not import the store everywhere**: The style guide says feature code should dispatch through hooks. The dynamic inject happens next to store setup.

**Sources:**

- [Configuring your store](https://redux.js.org/usage/configuring-your-store)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Discuss strategies for managing complex application state with Redux.</summary>

### Discuss strategies for managing complex application state with Redux.

**Definition:**

Split by feature, normalize entities, keep derived data in selectors, and model async status as an explicit field.

**How it works:**

- **Finite states**: The style guide shows `idle | loading | success | failure` as a discriminated union so `user` and `error` are not both set.
- **Entities**: `createEntityAdapter` holds the large collections. UI flags stay in a separate slice.
- **Effects**: Fetching goes through `createAsyncThunk` or RTK Query, not through the reducer.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [createEntityAdapter](https://redux-toolkit.js.org/api/createEntityAdapter)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
</details>

### Redux Performance Optimization

<details>
<summary>How do you prevent unnecessary renders in Redux-connected components?</summary>

### How do you prevent unnecessary renders in Redux-connected components?

**Definition:**

Subscribe each component to the value it paints. Return a stable result from `useSelector`, and memoize joins.

**How it works:**

- **Granular**: The style guide’s list/row split means one changed user does not render the whole list’s rows.
- **Multiple hooks**: Several `useSelector` calls that each return a number or string beat one selector that builds a new object.
- **shallowEqual**: When an object is required, pass `shallowEqual` as the second argument or as `equalityFn`.

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
<summary>What techniques are available to reduce the size of the Redux store?</summary>

### What techniques are available to reduce the size of the Redux store?

**Definition:**

Do not store what a selector can compute. Do not store server caches by hand if RTK Query already tracks them. Keep entities flat so you do not repeat nested copies.

**How it works:**

- **Derive**: The style guide says filtered lists and totals belong in memoized selectors.
- **Normalize**: One user record instead of a copy inside every post.
- **Cache**: RTK Query stores endpoint results under its `reducerPath` and drops them according to the cache lifetime, instead of every screen appending its own array.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
</details>

<details>
<summary>How do you optimize Redux action dispatch and payload size?</summary>

### How do you optimize Redux action dispatch and payload size?

**Definition:**

Dispatch one event with the id or the fields that changed. Do not put the next state tree, functions, or class instances in the action.

**How it works:**

- **Small payload**: The style guide’s preferred `todoToggled` carries an id. The reducer loads the todo from state.
- **Serializable**: Promises, functions, and class instances are rejected by the intent of the serializability check. A middleware may hold a function only if it stops the action before reducers.
- **One dispatch**: An event `orderAdded` beats two setter dispatches that each read `getState()` and add one.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>What are best practices for ensuring a scalable Redux state management with large datasets or frequent updates?</summary>

### What are best practices for ensuring a scalable Redux state management with large datasets or frequent updates?

**Definition:**

Normalize the collection, update one entity per action, and select by id so frequent ticks do not render the whole list.

**How it works:**

- **Adapter**: `updateOne` and `setAll` from `createEntityAdapter` write the `{ ids, entities }` shape.
- **Selectors**: A row component selects `entities[id]`. The parent selects `ids` only.
- **Coalesce**: If a socket emits many times a second, dispatch the latest snapshot on a short interval instead of once per message. The reducer still applies one plain action.

**Sources:**

- [createEntityAdapter](https://redux-toolkit.js.org/api/createEntityAdapter)
- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

### Redux and Forms

<details>
<summary>How do you manage form state with Redux?</summary>

### How do you manage form state with Redux?

**Definition:**

Keep most keystrokes in the component. Put a value in Redux when another part of the app must read it before submit, or when you need it in DevTools.

**How it works:**

- **Local by default**: A field used only by its form does not need an action per key. The style guide wants logic in reducers, not a stream of setters.
- **If it is in the store**: The style guide allows a spread update for a form, where one action per field would be noise. Prefer one `draftSaved` with the fields you actually store.
- **Submit**: The submit thunk reads the draft or receives it as the argument, posts it, and stores the server entity in an adapter.

**When to use:**

- Component state for a screen-local form.
- Redux when the draft must survive navigation or be read by another mounted feature.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Discuss the use of libraries like Redux Form or React Final Form with Redux.</summary>

### Discuss the use of libraries like Redux Form or React Final Form with Redux.

**Definition:**

Those libraries keep field state in their own tree. Redux does not need a copy of every keystroke. On submit, dispatch a thunk with the values the form library gives you.

**How it works:**

- **Boundary**: The form library calls your submit function with plain values. That function dispatches `createAsyncThunk` or an RTK Query mutation.
- **Do not mirror**: Writing each change into Redux and into the form duplicates state. The style guide’s “single source” is per fact, not “every key lives in the store”.
- **Server result**: Store the saved entity with the adapter. Leave input focus and untouched flags in the form library.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
</details>

<details>
<summary>What are the trade-offs of keeping form state in the Redux store?</summary>

### What are the trade-offs of keeping form state in the Redux store?

**Definition:**

The draft is visible in DevTools and to any subscriber. Every keystroke is an action, so the log is noisy and more components can render.

**How it works:**

- **Gain**: Another screen can read the draft. A refresh can restore it from `preloadedState` if you persisted it.
- **Cost**: The style guide warns that setter actions flood the log. A selector that reads the whole draft re-renders subscribers on each character.
- **Middle**: Store the draft when the user leaves the field or navigates, not on each `input` event.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

### Redux and Authentication

<details>
<summary>How do you manage user authentication and session state in Redux?</summary>

### How do you manage user authentication and session state in Redux?

**Definition:**

**Store the serializable session facts you must render:** user id, status, and maybe a non-secret display name. Drive login and logout with thunks that then dispatch plain actions.

**How it works:**

- **Status union**: The style guide’s `idle | loading | success | failure` shape fits `user` and `error` without impossible combinations.
- **Request**: `createAsyncThunk` performs the login request. `fulfilled` writes the user. `rejected` writes the error.
- **Reset**: Logout dispatches an action that returns the session slice to `initialState`, and a root reset if other slices hold that user’s data.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
</details>

<details>
<summary>What considerations should be made for security when storing tokens or sensitive information in Redux?</summary>

### What considerations should be made for security when storing tokens or sensitive information in Redux?

**Definition:**

The store is readable by any code in the page and by DevTools. A token there is not secret from XSS. Prefer an HttpOnly cookie the JavaScript never reads.

**How it works:**

- **What to store**: Store `{ id, name, status }`. Avoid putting long-lived access tokens in an action, because the style guide’s DevTools history keeps actions.
- **Serializable on purpose**: Anything in state can be serialized into a log or a server-rendered payload. Do not put secrets in `preloadedState`.
- **Logout**: Reset the session slice so a later user on the same page does not read the previous `user` object.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [createStore](https://redux.js.org/api/createStore)
</details>

<details>
<summary>How do you manage login and logout workflows with Redux actions and reducers?</summary>

### How do you manage login and logout workflows with Redux actions and reducers?

**Definition:**

Login is a thunk that dispatches a result action. Logout is a sync action that restores `initialState` and, when needed, resets the rest of the root state.

**How it works:**

- **Login**: `extraReducers` handles `login.pending`, `login.fulfilled`, and `login.rejected` using the status union from the style guide.
- **Logout**: The session reducer returns `initialState`. A root wrapper can pass `undefined` into the combined reducer so feature slices drop that user’s entities.
- **UI**: `useDispatch` starts the thunk from the form. `useSelector` reads `status` to disable the button while `loading`.

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
<summary>How do you approach migrating a legacy application state management to Redux?</summary>

### How do you approach migrating a legacy application state management to Redux?

**Definition:**

Add one store at the root and move one feature into a slice at a time. Leave the rest of the state where it is until that feature’s reads go through `useSelector`.

**How it works:**

- **Store first**: `configureStore` and `Provider` can wrap the app before any feature moves. Empty `reducer: {}` is not useful; start with the first slice.
- **One feature**: Write `createSlice` for that feature, dispatch from its events, and delete the old local updates for those same facts.
- **Rules**: Apply the style guide as you move: no mutations, no effects in reducers, feature folders.

**Sources:**

- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Can you explain the concept of "state hydration" in the context of SSR and Redux?</summary>

### Can you explain the concept of "state hydration" in the context of SSR and Redux?

**Definition:**

Hydration here means creating the browser store with the state the server already rendered. That object is `preloadedState`.

**How it works:**

- **Server**: The server creates a store, dispatches the loads it needs, renders, and serializes `getState()`.
- **Browser**: `configureStore({ reducer, preloadedState })` starts from that object so the first client render matches the HTML.
- **Constraint**: Only plain serializable data can cross that boundary. The style guide forbids functions, Maps, and class instances in state.

**Sources:**

- [createStore](https://redux.js.org/api/createStore)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>What challenges might arise when integrating Redux into an existing application and how would you address them?</summary>

### What challenges might arise when integrating Redux into an existing application and how would you address them?

**Definition:**

Two sources of truth and a second store are the usual breaks. Keep one store, and move one feature until its old state is gone.

**How it works:**

- **One store**: The style guide says a standard app has a single store in `Provider`. A second `configureStore` will not update the first tree.
- **Duplication**: If React state and Redux both store the todo list, they will diverge. After the slice exists, the component should select it, not copy it into `useState`.
- **Effects**: Old code that fetched inside render moves to a thunk or RTK Query. Reducers stay pure so DevTools can replay them.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
</details>

### Redux Future and Community

<details>
<summary>Describe the current trends and community practices around Redux.</summary>

### Describe the current trends and community practices around Redux.

**Definition:**

The style guide’s current practice is Redux Toolkit, feature slices, hooks instead of `connect`, and RTK Query for server cache.

**How it works:**

- **Toolkit**: `configureStore` and `createSlice` are the path the getting-started docs show.
- **Hooks**: React-Redux marks `connect` deprecated as of 9.3.0 and points to `useSelector` and `useDispatch`.
- **Data fetching**: The RTK Query overview says fetching and caching is a different problem from client state, and that `createApi` is the built-in answer.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [connect()](https://react-redux.js.org/api/connect)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
</details>

<details>
<summary>How has Redux evolved to fit into the modern React ecosystem with hooks?</summary>

### How has Redux evolved to fit into the modern React ecosystem with hooks?

**Definition:**

React-Redux added `useSelector` and `useDispatch`, which read the store without a `connect` wrapper. Redux Toolkit then became the way those hooks are taught.

**How it works:**

- **From HOC to hooks**: The style guide says hooks make the component request its own data, and that `connect` is harder to type.
- **Still one direction**: The component dispatches; reducers return state; selectors read it. Hooks did not add a second write path.
- **Deprecation**: `connect` still runs. New components use the hooks, or `legacy_connect` if a wrapper must remain.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Hooks API](https://react-redux.js.org/api/hooks)
- [connect()](https://react-redux.js.org/api/connect)
</details>

<details>
<summary>What do you believe is the future of Redux given the context of emerging state management libraries?</summary>

### What do you believe is the future of Redux given the context of emerging state management libraries?

**Definition:**

Redux stays the predictable store for client state that many features share. RTK Query and the style guide already treat server cache as a separate job that can live beside that store.

**How it works:**

- **Client state**: Slices, events, and DevTools remain the reason to keep a shared store.
- **Server state**: The RTK Query overview says the community split “data fetching” from “state management”, and that `createApi` covers the fetching side on top of Redux.
- **Less boilerplate**: The getting-started store is a `configureStore` call, not hand-written constants and switches.

**Sources:**

- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
- [Redux Toolkit getting started](https://redux-toolkit.js.org/introduction/getting-started)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

### Advanced State Modeling in Redux

<details>
<summary>How do you model complex state dependencies and computations in Redux?</summary>

### How do you model complex state dependencies and computations in Redux?

**Definition:**

Store the inputs. Compute the dependent value in a memoized selector. Use a status union when the legal combinations are limited.

**How it works:**

- **Selectors**: `createSelector` recomputes when its inputs change. The style guide prefers this to storing the derived number.
- **Unions**: The style guide’s user state is `idle`, `loading`, `success`, or `failure`, so a success state is not paired with an error.
- **Relations**: Normalized ids plus a selector assemble the graph. The reducer does not keep a second nested copy in sync.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
</details>

<details>
<summary>Discuss your approach to type checking and ensuring state consistency.</summary>

### Discuss your approach to type checking and ensuring state consistency.

**Definition:**

Type the slice state, including a discriminated union for status. Let `configureStore` infer the root state. Trust the reducer to ignore impossible transitions.

**How it works:**

- **Union**: The style guide types `UserState` so `status: "success"` implies `user: User` and `error: null`.
- **PayloadAction**: Case reducers take `PayloadAction<User>` so the payload field is checked at the dispatch site.
- **Root**: `ReturnType<typeof store.getState>` is the root state type used to type selectors, including `getSelectors<RootState>`.

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
<summary>How do you tackle transactional updates in Redux?</summary>

### How do you tackle transactional updates in Redux?

**Definition:**

One action can update every field that must change together, inside one reducer call. Subscribers see the next state only after that reducer returns.

**How it works:**

- **One event**: The style guide’s `orderAdded` carries the pizza and the coke together instead of two setters.
- **Several slices**: If two slices must move together, handle the same action type in both `extraReducers`. Both run in that dispatch before listeners paint.
- **Not a database transaction**: If the server call fails, dispatch a follow-up action that restores the previous plain values. The reducer does not roll back by itself.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>What strategies would you use for undo/redo functionality in a Redux application?</summary>

### What strategies would you use for undo/redo functionality in a Redux application?

**Definition:**

Keep `past`, `present`, and `future` for the branch you can undo. An undo action moves `present` onto `future` and restores the last `past` entry. DevTools time travel is a separate debug tool.

**How it works:**

- **Scope**: Undo the document slice, not the whole store. Session and request status should not rewind.
- **Actions**: `changeCommitted` pushes the previous present. `undo` and `redo` only move those arrays. They are ordinary slice reducers.
- **Limit**: Cap `past` so the store does not keep every keystroke. The style guide already warns against unbounded derived copies.

**Sources:**

- [Redux style guide](https://redux.js.org/style-guide/)
</details>

### Redux Interoperability

<details>
<summary>How does Redux interact with WebSockets or external data sources?</summary>

### How does Redux interact with WebSockets or external data sources?

**Definition:**

A listener or a thunk owns the socket. Incoming messages dispatch plain actions. The reducer stores the serializable payload.

**How it works:**

- **Listener**: `createListenerMiddleware` can start work when an action matches, `dispatch`, and `fork`. Prepend it so function values do not hit the serializability check.
- **RTK Query**: The overview says cache entry lifecycle options can stream updates over a websocket after the initial fetch.
- **Reducer**: The socket callback must not mutate state. It dispatches `eventReceived` with a plain object.

**Sources:**

- [createListenerMiddleware](https://redux-toolkit.js.org/api/createListenerMiddleware)
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
- [Redux style guide](https://redux.js.org/style-guide/)
</details>

<details>
<summary>Discuss the integration of GraphQL in a Redux-managed application.</summary>

### Discuss the integration of GraphQL in a Redux-managed application.

**Definition:**

GraphQL results are server state. RTK Query can call a GraphQL endpoint through `fetchBaseQuery` or a custom `baseQuery`, and the overview notes code generation from GraphQL schemas.

**How it works:**

- **Endpoint**: `createApi` defines the operation. The React entry point generates a hook such as `useGetPokemonByNameQuery` for components.
- **Store**: Add `pokemonApi.reducer` and `pokemonApi.middleware` to `configureStore`. `setupListeners(store.dispatch)` enables refetch on reconnect.
- **Client state**: Leave drafts and UI flags in slices. Do not also copy the GraphQL cache into a hand-written entity slice unless you have a reason.

**Sources:**

- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
</details>

<details>
<summary>How would you manage local vs. server state in a Redux application?</summary>

### How would you manage local vs. server state in a Redux application?

**Definition:**

Server records live in an entity adapter or in RTK Query’s cache. Local UI state lives in a slice or in the component. Do not mix a request status and a draft into one unstructured object.

**How it works:**

- **Server**: The RTK Query overview says fetching and cache lifetime are a different problem, and `createApi` is built for that.
- **Local**: The normalizing page puts `ui` next to `entities`. A modal flag does not belong on the user record.
- **One fact**: If both layers store the user, they will disagree. Pick the server cache as the source after a successful query.

**Sources:**

- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
- [Normalizing state shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)
</details>

### Redux API Requests and Caching

<details>
<summary>How do you structure API requests in Redux actions?</summary>

### How do you structure API requests in Redux actions?

**Definition:**

Prefer `createApi` for a base URL and endpoints. For a one-off call, `createAsyncThunk` dispatches `pending`, `fulfilled`, and `rejected`, and the slice handles those in `extraReducers`.

**How it works:**

- **Thunk**: The payload creator `await`s the client and returns data. It does not call the reducer itself.
- **Handlers**: `builder.addCase(fetchUserById.fulfilled, ...)` writes the result. `pending` sets `loading`.
- **RTK Query**: The essentials tutorial says RTK Query can remove hand-written thunks and reducers for fetching. `fetchBaseQuery({ baseUrl })` is the usual base query.

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
<summary>Discuss methods for caching API responses in the Redux store.</summary>

### Discuss methods for caching API responses in the Redux store.

**Definition:**

RTK Query caches a result per endpoint and argument, and the component reads `data` and `isLoading` from the generated hook. A hand-written slice is the older pattern the essentials tutorial shows underneath.

**How it works:**

- **createApi**: One API slice per base URL is the rule of thumb in the overview. The reducer and middleware must both be registered.
- **Lifetime**: The cache entry can refetch on mount or reconnect when you call `setupListeners(store.dispatch)`.
- **Manual**: A slice that stores `entities` from a fulfilled thunk is a cache you must invalidate yourself. Do that only when you are not using `createApi` for that resource.

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
<summary>What patterns or libraries do you use to manage RESTful or GraphQL API interactions with Redux?</summary>

### What patterns or libraries do you use to manage RESTful or GraphQL API interactions with Redux?

**Definition:**

Use RTK Query’s `createApi` for both. REST endpoints use `fetchBaseQuery` and a path. GraphQL uses a `query` function that posts the operation, or generated code from the schema, which the overview says exists in early form.

**How it works:**

- **REST**: `build.query({ query: (name) => \`pokemon/${name}\` })`issues GET against the base URL. Mutations use`build.mutation`.
- **Hooks**: Import `createApi` from `@reduxjs/toolkit/query/react` so the endpoint exports `useGetPokemonByNameQuery`.
- **Fallback**: A single REST call can stay a `createAsyncThunk` plus `extraReducers` when you do not want a cache.

**Sources:**

- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)
- [Essentials — async logic](https://redux.js.org/tutorials/essentials/part-5-async-logic)
</details>
