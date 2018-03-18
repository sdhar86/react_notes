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

### Normalized the State Shape

We currently represent the`todos`in the state tree as an array of`todo`objects. However, in the real app we would probably have more than a single array, and`todos`with the same`id`s in different arrays might get out of sync.

#### `todos.js`Before

```js
const todos = (state = [], action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        todo(undefined, action),
      ];
    case 'TOGGLE_TODO':
      return state.map(t =>
        todo(t, action)
      );
    default:
      return state;
  }
};
```

### Refactoring`todos.js`

We should treat our state as a database, so we are going to keep`todos`in an object indexed by`id`.

We will start by renaming the reducer to`byID`. Now, rather than adding a new item at the end or mapping over every item, we will change the value in the lookup table.

Now both`TOGGLE_TODO`and`ADD_TODO`have the same logic. We want to return a new lookup table where the value of`action.id`is going to be the result of calling the reducer on the previous`action.id`value and the`action`.

```
const byId = (state = {}, action) => {
  switch (action.type) {
    case 'ADD_TODO':
    case 'TOGGLE_TODO':
      return {
        ...state,
        [action.id]: todo(state[action.id], action),
      };
    default:
      return state;
  }
};
```

  
This is still reducer composition, but with an object instead of an array.

## **NOTE**: We are using the Object Spread operator \(`...state`\). This is not a part of ES6, so we need to install the`transform-object-rest-spread`Babel plugin, and add it to our`.babelrc`file in order for this to work.

Anytime the`byId`reducer receives an action, it's going to return a copy of its mapping between the`id`s and the actual todos with an updated`todo`for the current action.

Now we'll add another reducer that keeps track of all the added`id`s.

### Adding an`allIds`Reducer

Now that we keep the todos themselves in the`byId`map, we will have the state of this reducer be an array of`id`s.

This reducer will switch on the action's type, and the only action I care about is`'ADD_TODO'`because if a new todo is added, we want to return a new array of`id`s with the new`id`as the last item.

For any other actions, we just need to return the current state \(which is the current array of`id`s\).

```js
const allIds = (state = [], action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return [...state, action.id];
    default:
      return state;
  }
};
```

We still need to export the single reducer from the`todos.js`file, so we use`combineReducers()`again to combine the`byId`and the`allIds`reducers.

  


---

_Note_: You can use combined reducers as many times as you like. You don't have to only use it on the top-level reducer. In fact, it's very common that as your app grows, you'll use`combineReducers`in several places.







