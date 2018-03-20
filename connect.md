# Generating Containers with`connect()`from React Redux

---

Now that we have`react-redux`, we are using its`Provider`component to pass`store`down via context.

Our container components are similar: they need to re-render when the store's state changes, they need to unsubscribe from the store when they unmount, and they take the current state from the Redux`store`and use it to render the presentational components with some props that they calculate.

They also need to specify the`contextTypes`to get the store from the context.

To understand/implement** connect\(\) **we need two functions:

### mapStateToProps\(\)

The name describes exactly what it does - it takes the Redux store's state, and changes them to props for the component.

Example:

```js
const mapStateToProps = (state) => {
  return {
    todos: getVisibleTodos (
      state.todos,
      state.visibilityFilter
    )
  };
}
```

the todos is now available as a props in the React Component.

## mapDispatchToProps\(\)

Like the earlier function, the name is very descriptive again ..it maps the store's`dispatch()`method and returns the props that use the dispatch method to dispatch actions.

Example:

```js
const mapDispatchToProps = (dispatch) => {
  return {
    onTodoClick: (id) => {
      dispatch({
        type: 'TOGGLE_TODO',
        id
      })
    }
  };
};
```

Now, whenever onTodoClick is called, it dispatches an action that flows through all the reducers.

### connect\(\) ing it up:

Together, these two new functions describe a container component so well that instead of writing it we can generate it by using the`connect()`function provided by`react-redux`

```js
import { connect } from 'react-redux';

...

const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)
```

Instead of creating a`VisibleTodoList`class, we can declare a variable and use the`connect()`method to obtain it. We'll pass in`mapStateToProps`as the first argument, and`mapDispatchToProps`as the second. As this is a curried function, we must call it again, passing in the presentational component that we want it to wrap and pass the props, thereby connecting to the redux store \(in this case,`TodoLis`\)

### mapDispatchToProps shorthand

Often, don't need to write`mapDispatchToProps,`and you can pass this map in object instead.

```js
const VisibleTodoList = withRouter(connect(
  mapStateToProps,
  { onTodoClick: toggleTodo }
)(TodoList));
```



