# Selectors

---

[https://www.saltycrane.com/blog/2017/05/what-are-redux-selectors-why-use-them/](https://www.saltycrane.com/blog/2017/05/what-are-redux-selectors-why-use-them/)

Selectors are functions that take Redux state as an argument and return some data to pass to the component. Redux state can be thought of like a database and selectors like SELECT queries to get useful data from the database. A good example is a filtered list. **They are normally colocated near the reducers**

Reason to have selector:

* to avoid duplicated data in Redux.

* Performing data transformations in the component makes them more coupled to the Redux state and less generic/reusable. Also, as Dan Abramov points out, it makes sense to keep selectors near reducers because they operate on the same state. If the state schema changes, it is easier to update the selectors than to update the components

Simple example:

```js
const getDataType = state => state.editor.dataType;
```

They are typically used in`mapStateToProps`in`react-redux`'s`connect`:

Without using selectors

```js
export default connect(
  (state) => ({
    dataType: state.editor.dataType,
  })
)(MyComponent);
```

Wit using Selectors;

```js
export default connect(
  (state) => ({
    dataType: getDataType(state),
  })
)(MyComponent);
```

# Performance and Reselect

---

`mapStateToProps`gets called a lot so performing expensive calculations there is not good. This is where the[`reselect`](https://github.com/reactjs/reselect)library comes in. Selectors created with`reselect's createSelector`will memoize to avoid unnecessary recalculations

[https://redux.js.org/recipes/computing-derived-data](https://redux.js.org/recipes/computing-derived-data)

Let's say we had this container:

### `containers/VisibleTodoList.js` {#containersvisibletodolist.js}

```js
import { connect } from 'react-redux'
import { toggleTodo } from '../actions'
import TodoList from '../components/TodoList'
 
const getVisibleTodos = (todos, filter) => {
  switch (filter) {
    case 'SHOW_ALL':
      return todos
    case 'SHOW_COMPLETED':
      return todos.filter(t => t.completed)
    case 'SHOW_ACTIVE':
      return todos.filter(t => !t.completed)
  }
}
 
const mapStateToProps = state => {
  return {
    todos: getVisibleTodos(state.todos, state.visibilityFilter)
  }
}
 
const mapDispatchToProps = dispatch => {
  return {
    onTodoClick: id => {
      dispatch(toggleTodo(id))
    }
  }
}
 
const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)
 
export default VisibleTodoList
```

In the above example,`mapStateToProps`calls`getVisibleTodos`to calculate`todos.`This works great, but there is a drawback:`todos`is calculated every time the component is updated. If the state tree is large, or the calculation expensive, repeating the calculation on every update may cause performance problems. Reselect can help to avoid these unnecessary recalculations.

## Creating a Memoized Selector {#creating-a-memoized-selector}

We would like to replace`getVisibleTodos`with a memoized selector that recalculates`todos`when the value of`state.todos`or`state.visibilityFilter`changes, but not when changes occur in other \(unrelated\) parts of the state tree.

Reselect provides a function`createSelector`for creating memoized selectors.`createSelector`takes an array of input-selectors and a transform function as its arguments. If the Redux state tree is changed in a way that causes the value of an input-selector to change, the selector will call its transform function with the values of the input-selectors as arguments and return the result. If the values of the input-selectors are the same as the previous call to the selector, it will return the previously computed value instead of calling the transform function.

![](/assets/reselect_1.png)

![](/assets/reselct _2.png)

### `selectors/index.js` {#selectorsindex.js}

```js
import { createSelector } from 'reselect'
 
const getVisibilityFilter = state => state.visibilityFilter
const getTodos = state => state.todos
 
export const getVisibleTodos = createSelector(
  [getVisibilityFilter, getTodos],
  (visibilityFilter, todos) => {
    switch (visibilityFilter) {
      case 'SHOW_ALL':
        return todos
      case 'SHOW_COMPLETED':
        return todos.filter(t => t.completed)
      case 'SHOW_ACTIVE':
        return todos.filter(t => !t.completed)
    }
  }
)
```

In the example above,`getVisibilityFilter`and`getTodos`are input-selectors. They are created as ordinary non-memoized selector functions because they do not transform the data they select.`getVisibleTodos`on the other hand is a memoized selector. It takes`getVisibilityFilter`and`getTodos`as input-selectors, and a transform function that calculates the filtered todos list.

## Composing Selectors {#composing-selectors}

A memoized selector can itself be an input-selector to another memoized selector. Here is`getVisibleTodos`being used as an input-selector to a selector that further filters the todos by keyword

```js
const getKeyword = state => state.keyword
 
const getVisibleTodosFilteredByKeyword = createSelector(
  [getVisibleTodos, getKeyword],
  (visibleTodos, keyword) =>
    visibleTodos.filter(todo => todo.text.indexOf(keyword) > -1)
)
```

## Connecting a Selector to the Redux Store {#connecting-a-selector-to-the-redux-store}

you can call selectors as regular functions inside`mapStateToProps()`:

`containers/VisibleTodoList.js`

```js
import { connect } from 'react-redux'
import { toggleTodo } from '../actions'
import TodoList from '../components/TodoList'
import { getVisibleTodos } from '../selectors'
 
const mapStateToProps = state => {
  return {
    todos: getVisibleTodos(state)
  }
}
 
const mapDispatchToProps = dispatch => {
  return {
    onTodoClick: id => {
      dispatch(toggleTodo(id))
    }
  }
}
 
const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)
 
export default VisibleTodoList
```

## Accessing React Props in Selectors {#accessing-react-props-in-selectors}

So far we have only seen selectors receive the Redux store state as an argument, but a selector can receive props too.

Here is an`App`component that renders three`VisibleTodoList`components, each of which has a`listId`prop:

### `components/App.js` {#componentsapp.js}

```js
import React from 'react'
import Footer from './Footer'
import AddTodo from '../containers/AddTodo'
import VisibleTodoList from '../containers/VisibleTodoList'
 
const App = () => (
  <div>
    <VisibleTodoList listId="1" />
    <VisibleTodoList listId="2" />
    <VisibleTodoList listId="3" />
  </div>
)
```

### `selectors/todoSelectors.js` {#selectorstodoselectors.js}

```js
import { createSelector } from 'reselect'
 
const getVisibilityFilter = (state, props) =>
  state.todoLists[props.listId].visibilityFilter
 
const getTodos = (state, props) => state.todoLists[props.listId].todos
 
const getVisibleTodos = createSelector(
  [getVisibilityFilter, getTodos],
  (visibilityFilter, todos) => {
    switch (visibilityFilter) {
      case 'SHOW_COMPLETED':
        return todos.filter(todo => todo.completed)
      case 'SHOW_ACTIVE':
        return todos.filter(todo => !todo.completed)
      default:
        return todos
    }
  }
)
 
export default getVisibleTodos
```

`props`can be passed to`getVisibleTodos`from`mapStateToProps`:

```js
const mapStateToProps = (state, props) => {
  return {
    todos: getVisibleTodos(state, props)
  }
}
```

**But there is a problem!**

Using the`getVisibleTodos`selector with multiple instances of the`visibleTodoList`container will not correctly memoize:

### `containers/VisibleTodoList.js` {#containersvisibletodolist.js-2}

```js
import { connect } from 'react-redux'
import { toggleTodo } from '../actions'
import TodoList from '../components/TodoList'
import { getVisibleTodos } from '../selectors'
 
const mapStateToProps = (state, props) => {
  return {
    // WARNING: THE FOLLOWING SELECTOR DOES NOT CORRECTLY MEMOIZE
    todos: getVisibleTodos(state, props)
  }
}
 
const mapDispatchToProps = dispatch => {
  return {
    onTodoClick: id => {
      dispatch(toggleTodo(id))
    }
  }
}
 
const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)
 
export default VisibleTodoList
```

A selector created with`createSelector`only returns the cached value when its set of arguments is the same as its previous set of arguments. If we alternate between rendering`<VisibleTodoList listId="1" />`and`<VisibleTodoList listId="2" />`, the shared selector will alternate between receiving`{listId: 1}`and`{listId: 2}`as its`props`argument. This will cause the arguments to be different on each call, so the selector will always recompute instead of returning the cached value. We'll see how to overcome this limitation in the next section.

## Sharing Selectors Across Multiple Components {#sharing-selectors-across-multiple-components}

**Note**: The examples in this section require React Redux v4.3.0 or greater

In order to share a selector across multiple`VisibleTodoList`components **and **retain memoization, each instance of the component needs its own private copy of the selector.

Let's create a function named`makeGetVisibleTodos`that returns a new copy of the`getVisibleTodos`selector each time it is called:

### `selectors/todoSelectors.js` {#selectorstodoselectors.js-1}

```js
import { createSelector } from 'reselect'
 
const getVisibilityFilter = (state, props) =>
  state.todoLists[props.listId].visibilityFilter
 
const getTodos = (state, props) => state.todoLists[props.listId].todos
 
const makeGetVisibleTodos = () => {
  return createSelector(
    [getVisibilityFilter, getTodos],
    (visibilityFilter, todos) => {
      switch (visibilityFilter) {
        case 'SHOW_COMPLETED':
          return todos.filter(todo => todo.completed)
        case 'SHOW_ACTIVE':
          return todos.filter(todo => !todo.completed)
        default:
          return todos
      }
    }
  )
}
 
export default makeGetVisibleTodos
```

We also need a way to give each instance of a container access to its own private selector. The`mapStateToProps`argument of`connect`can help with this.

**If the**`mapStateToProps`**argument supplied to**`connect`**returns a function instead of an object, it will be used to create an individual**`mapStateToProps`**function for each instance of the container.**

In the example below`makeMapStateToProps`creates a new`getVisibleTodos`selector, and returns a`mapStateToProps`function that has exclusive access to the new selector:

```js
const makeMapStateToProps = () => {
  const getVisibleTodos = makeGetVisibleTodos()
  const mapStateToProps = (state, props) => {
    return {
      todos: getVisibleTodos(state, props)
    }
  }
  return mapStateToProps
}
```

If we pass`makeMapStateToProps`to`connect`, each instance of the`VisibleTodosList`container will get its own`mapStateToProps`function with a private`getVisibleTodos`selector. Memoization will now work correctly regardless of the render order of the`VisibleTodoList`containers.

### `containers/VisibleTodoList.js` {#containersvisibletodolist.js-3}

```js
import { connect } from 'react-redux'
import { toggleTodo } from '../actions'
import TodoList from '../components/TodoList'
import { makeGetVisibleTodos } from '../selectors'
 
const makeMapStateToProps = () => {
  const getVisibleTodos = makeGetVisibleTodos()
  const mapStateToProps = (state, props) => {
    return {
      todos: getVisibleTodos(state, props)
    }
  }
  return mapStateToProps
}
 
const mapDispatchToProps = dispatch => {
  return {
    onTodoClick: id => {
      dispatch(toggleTodo(id))
    }
  }
}
 
const VisibleTodoList = connect(
  makeMapStateToProps,
  mapDispatchToProps
)(TodoList)
 
export default VisibleTodoList
```



