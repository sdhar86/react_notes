# Connecting React to Redux

---

In a React App that uses Redux for state management we can have two kinds of components

**presentational components** just display data, we need a way to This is where

**container components **that access the store, and pass data from the store. -- they can specify behavior and pass data.

Example Container Component: 

```
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

In index.js

```
import  {createStore} from 'redux'; 
import {Provider } from 'react-redux';
import { render } from 'react-dom';

import App from './app'
import combinedReducer from '../reducer/index';


const store = createStore(reducer);

... 


const render(
  <Provider store={store}>
      <App />
  </Provider>,
  document.getElementById('root')
);
```

Now store is available in all children components of App

