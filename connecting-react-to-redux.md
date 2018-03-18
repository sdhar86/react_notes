# Connecting React to Redux

---

In a React App that uses Redux for state management we can have two kinds of components

**presentational components** just display data, we need a way to This is where

**container components **that access the store, and pass data from the store. -- they can specify behavior and pass data.

**Example Container Component:**

```js
class TodoApp extends Component {
    render () {
    const {
      todos,
      visibilityFilter
    } = this.props;

    const visibleTodos = getVisibleTodos(
      todos,
      visibilityFilter
    );

    return (
      <div>
        <input ref={node => {
          this.input = node;
        }} />
        <button onClick={() => {
          store.dispatch({
            type: 'ADD_TODO',
            text: this.input.value,
            id: nextTodoId++
          });
          this.input.value = '';
        }}>
          Add Todo
        </button>
        <TodoList
          todos={visibleTodos}
          onTodoClick={id =>
            store.dispatch({
              type: 'TOGGLE_TODO',
              id
            })
          } />
      .
      . // FilterLink stuff
      .
      </div>
    );
  }
}
```

**Example Presentation Components: **

```js
const TodoList = ({
  todos,
  onTodoClick
}) => (
  <ul>
    {todos.map(todo =>
      <Todo
        key={todo.id}
        {...todo}
        onClick={() => onTodoClick(todo.id)}
      />
    )}
  </ul>
)
```

and

```js
const Todo = ({
  onClick,
  completed,
  text
}) => (
  <li
    onClick={onClick}
    style={{
      textDecoration:
        completed ?
          'line-through' :
          'none'
    }}
  >
    {text}
  </li>
);
```

#### To Recap...

The`TodoApp`component renders a`TodoList`and passes it a function that can dispatch an action.

The`TodoList`component renders the`Todo`component, and passes an`onClick`prop which calls`onTodoClick()`.

The`Todo`component uses the`onClick`prop it receives and binds it to the list item's`onClick`. This way when it's called, the`onTodoClick()`is called, which in turn dispatches the action, which in turn updates the visibile todos, since the action updates the store.

### Passing the Store Down Explicitly via Props

For all Components to have access to the store, we can pass them down to all components as props

```js
ReactDOM.render(
  <TodoApp store={createStore(todoApp)} />,
  document.getElementById('root')
);
```

and then TodoApp has to pass them to it's child components:

```js
const TodoApp = ({ store }) => (
  <div>
    <AddTodo store={store} />
    <VisibleTodoList store={store} />
    <Footer store={store} />
  </div>
);
```

The problem with doing it this way is that the container components need to have the`store`instance to get`state`from it, dispatch actions, and subscribe to changes.

Now inside of each of the components inside of`TodoApp`we need to adjust our container components to take the`store`from the`props`in both`componentDidMount()`, and`render()`:

```js
class VisibleTodoList extends Component {
  componentDidMount() {
    const { store } = this.props;
    this.unsubscribe = store.subscribe(() =>
      this.forceUpdate()
    );
  }
  .
  . // Rest of component as before to `render()` method
  .

  render() {
    const props = this.props;
    const { store } = props;
    const state = store.getState();
    .
    .
    .
  }
}
```

### Passing the Store Down Implicitly via Context:

Instead of passing store around as props, there is an easier way: use React's context feature.

we define a provider HOC:

    class Provider extends Component {
      getChildContext() {
        return {
          store: this.props.store // This corresponds to the `store` passed in as a prop
        };
      }
      render() {
        return this.props.children;
      }
    }

    Provider.childContextTypes = {
      store: React.PropTypes.object
    }

and then:

```
ReactDOM.render(
  <Provider store={createStore(todoApp)}>
    <TodoApp />
  </Provider>,
  document.getElementById('root')
);
```

`Provider`component use React's context feature. This is what makes the store available to any component that it renders. Now the`store`will be available to any of the children and grandchildren of the components`Provider`renders \(in our example, this is`TodoApp`and all of the other components and containers  built inside of it!\)

Now the containers get store from context instead of props

```js
class VisibleTodoList extends Component {
  componentDidMount() {
    const { store } = this.context;
    this.unsubscribe = store.subscribe(() =>
      this.forceUpdate()
    );
  }
  .
  . // Rest of component as before to `render()` method
  .

  render() {
    const props = this.props;
    const { store } = this.context;
    const state = store.getState();
    .
    .
    .
  }
}
```

**Note**:

The`context`is opt-in for all components, so we have to speciicf`contextTypes.`If you don't specify this, the component won't receive the relevant context, so it is essential to declare them!

```
VisibleTodoList.contextTypes = {
  store: React.PropTypes.object
}
```

**Our functional components don't have**`this`**, so how will we give them**`context`**?**

It turns out that they receive context as a second argument \(after`props`\). We also need to add`contextTypes`for the component that specifies which context we want to receive \(in this case`store`from`Provider)`. Context can be passed down to any level, so you can think of it as creating a 'wormhole' to whatever component that uses it, however, you must remember to opt-in by declaring the contextTypes

```js
const AddTodo = (props, { store }) => {
  .
  . // Rest of component as before
  .
}

AddTodo.contextTypes = {
  store: React.PropTypes.object
}
```

**we do not have to implement Provider HOC, ourselves - we can import it from the 'react-redux' library. **

```
import { Provider } from 'react-redux';
```

Just like the`Provider`we wrote before, the`Provider`that comes with`react-redux`exposes the`store`as a prop on the context.

### Refactoring Entry point

With all the Configurations, the index file might get messy:

example:

```js
import 'babel-polyfill'
import React from 'react'
import { render } from 'react-dom'
import { Provider } from 'react-redux'
import { createStore } from 'redux'
import throttle from 'lodash/throttle'
import todoApp from './reducers'
import App from './components/App'
import { loadState, saveState } from './localStorage'

const persistedState = loadState()
const store = createStore(
  todoApp,
  persistedState
);

store.subscribe(throttle(() => {
  saveState({
    todos: store.getState().todos,
  })
}, 1000))

render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

We can extract the configuration of store in another file: - configureStrore.js

```js
import { createStore } from 'redux'
import throttle from 'lodash/throttle'
import todoApp from './reducers'
import { loadState, saveState } from './localStorage'

const configureStore = () => {
  const persistedState = loadState()
  const store = createStore(todoApp, persistedState)

  store.subscribe(throttle(() => {
    saveState({
      todos: store.getState().todos
    })
  }, 1000))

  return store
}

export default configureStore
```

We can extract out the Root component as well:

```js
import React, { PropTypes } from 'react';
import { Provider } from 'react-redux';
import App from './App';

const Root = ({ store }) => (
  <Provider store={store}>
    <App />
  </Provider>
);

Root.propTypes = {
  store: PropTypes.object.isRequired,
};

export default Root;
```

and then we can use then in index,js

```js
import 'babel-polyfill'
import React from 'react'
import { render } from 'react-dom'
import Root from './components/Root'
import configureStore from './configureStore'

const store = configureStore()
render(
  <Root store={store} />,
  document.getElementById('root')
);
```

With the extracted Root Component, it is **easy for us to add React Router to our projects: **

react-router 4.0.0 and up 

```js
import React, { PropTypes } from 'react';
import { Provider } from 'react-redux';
import App from './App';
import { BrowserRouter, Route } from 'react-router-dom'

const Root = ({ store }) => (
    <Provider store={store}>
        <BrowserRouter>
            <Route path="/" component={App} />
        </BrowserRouter>
    </Provider>
);
```

Old React router &lt; v4.0.0: 

```js
import React, { PropTypes } from 'react';
import { Provider } from 'react-redux';
import { Router, Route, browserHistory } from 'react-router';
import App from './App';

const Root = ({ store }) => (
  <Provider store={store}>
    <Router history={browserHistory}>
      <Route path="/" component={App} />
    </Router>
  </Provider>
);
.
.
.
```



