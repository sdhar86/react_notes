# Redux

---

A library that provides a central place to manage state. Redux provides data layer for a React App.

State management in a big appiication can be tricky, thats why we use Redux as a central manager of state

### Three Principles of Redux:

* **The entire state of the application will be represented by one JavaScript object. **All changes and mutations to the application are explicit. These mutations, which include the data and the UI state, are contained in a single object we call the **state**.

* The second principle of Redux is that **the **_**state tree **_**is read only**. Any time you want to change the state, you have to dispatch an **action**. An action is a plain JS object describing the change. Just like the state is the minimal representation of the data, the action is the minimal representation of the change to that data. The only requirement for an action is that it has a type property \(conventionally a String\)

* This is the 3rd and final principle of Redux: to describe state mutations you have to write a function that takes the previous state of the app and the action being dispatched, then returns the next state of the app. This function is called the **Reducer**

  * It is important that the function is **pure. **Pure functions are those whose return values depend only upon the values of their arguments. Pure functions don't have side effects like network or database calls. Pure functions also do not override the values of anything.

**Example Reducer: **

```js
const counter = (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
}
```

### Store:

The **store** binds together the 3 principles of Redux:

1. Holds the current application state object
2. Allows you to dispatch actions
3. When you create it, you need to specify the reducer that tells how state is updated with actions.

#### Store has 3 important methods:

1. `getState()`retrieves the current state of the Redux store. If we ran`console.log(store.getState())`with the code above, we could get`0`since it is the initial state of our application.

2. `dispatch()`is the most commonly used. It is how we dispatch actions to change the state of the application. If we run`store.dispatch( { type: 'INCREMENT' });`followed by`console.log(store.getState());`we will get`1`since

3. `subscribe()`registers a callback that the redux store will call any time an action has been dispatched so you can update the UI of your application to reflect the current application state.

```js
const { createStore } = Redux; // Redux CDN import syntax
// import { createStore } from 'redux' // npm module syntax

const store = createStore(counter);
```

### persistedState\(\)

Redux lets us pass persistedState as the second argument to the createStore functions, to setup the initial state values. 

```js
const persistedState = {
  todos: [{
    id: 0,
    text: 'Welcome Back!',
    completed: false
  }]
}

const store = createStore(
  todoApp,
  persistedState
)
```

**Note: **

We can persist state in localStorage

\#\#\#\#`localStorage.js`

```js
export const loadState = () => {
  try {
    const serializedState = localStorage.getItem('state');
    if (serializedState === null) {
      return undefined;
    }
    return JSON.parse(serializedState);
  } catch (err) {
    return undefined;
  }
};


export const saveState = (state) => {
  try {
    const serializedState = JSON.stringify(state);
    localStorage.setItem('state', serializedState);
  } catch (err) {
    // Ignore write errors.
  }
};
```

Then in index.js, 

In order to save our state any time the store changes, we will use the`store's subscribe()`method to add a listener that will be invoked on any state change, passing in the current state of the store into the`saveState`function:

```js
import { loadState, saveState } from './localStorage'

// note: we dont have to save the entire state, only the ones that matter. 
store.subscribe(() => {
  saveState(store.getState())
})
```

save State could be an expensive operation, and we save it everytime store changes... we can throttle it to save only once every 1s

```js
// top of index.js
import throttle from 'lodash/throttle'
.
.
.
store.subscribe(throttle(() => {
  saveState({
    todos: store.getState().todos
  })
}, 1000))
```

### Redux Overall Structure: ![](/assets/redux.png)

### Redux Example:

```JavaScript
const createStore = redux.createStore;

const initialState = {counter : 0}

// Reducer
const rootReducer = (state = initialState, action) => {
    if (action.type == 'INC_COUNTER') {
        return { ..state, counter: state.counter + 1 }
    }
    if (action.type == 'ADD_COUNTER') {
        return { ..state, counter: state.counter + action.value }
    }
    return state; 
}

// Store 
const store =  createStore(rootReducer);
console.log(store.getState()); // { counter: 0}

// Subscription 
// the subscription fires everytime action is dispatched and store is updated

store.subscribe(()  => {
    console.log('[Subscription]', store.setState()); 
})

// Dispatching Action
store.dispatch({type: INC_COUNTER });
console.log(store.getState()); // { counter: 1}


store.dispatch({type: ADD_COUNTER, value: 10 });
console.log(store.getState()); // { counter: 11}
```



