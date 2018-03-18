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



### ![](/assets/redux.png)Redux Basic

---

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



