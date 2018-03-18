# Reducer Composition

---

Every time a function does too many things, it's best to break them up into other functions that each address only one concern.

In Redux, we can break reducers apart - we call that **reducer composition**

Different reducers specify how different parts of the state tree are updated in response to actions. Since reducers are normal JS functions, they can call other reducers to delegate & abstract away updates to the state.

Reducer composition can be applied many times. While there's a single top-level reducer managing the overall state of the app, it's encouraged to have reducers call each other as needed to manage the state tree.

### `combineReducers()`

Let's say We have two reducers - **visibilityFilter\(\) **and **todos\(\)**

We will use reducer composition to create a new reducer that calls existing reducers to manage their parts of the state, then combine the parts into a single state object.

```js
const todoApp = (state = {}, action) => {
  return {
     // Call the `todos()` reducer from last section
     todos: todos(
      state.todos,
      action
    ),
    visibilityFilter: visibilityFilter(
      state.visibilityFilter,
      action
    )
  };
};
```

Since reducer composition is so common in Redux, there's a helper function`combineReducers()`

```js
const { combineReducers } = Redux; // CDN Redux import

const todoApp = combineReducers({
  todos: todos,
  visibilityFilter: visibilityFilter
});
```

**By convention, the state keys should be named after the reducers that manage them. **Because of this, we can use ES6 object literal shorthand notation to accomplish the same thing like this:

```js
const todoApp = combineReducers({
  todos,
  visibilityFilter
});
```

**Note: **

* we pass in the **root reduce**r \(the top most level reducer\) to the store, when we create the store.





