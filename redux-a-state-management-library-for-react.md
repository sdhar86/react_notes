# Redux

---

A library that provides a central place to manage state. Redux provides data layer for a React App.

State management in a big appiication can be tricky, thats why we use Redux as a central manager of state

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



