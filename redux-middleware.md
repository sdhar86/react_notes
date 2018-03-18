# Redux MiddleWare

---

### Action Lifecycle with Middleware![](/assets/middleware.png)

### Another Data flow diagram:

### ![](/assets/middleware 2.png)

### Middlewares are stackable

### ![](/assets/middleware_stack.png)

### Applying Middlewares:

```js
import {createStore, applyMiddleWare } from 'redux';

import someMiddleWare from 'middleware_file_path';
...

composeStoreWithMiddleware = applyMiddleware(
  someMiddleWare
)(createStore)
...

ReactDOM.render(
    <Provider store={createStoreWithMiddleware(reducers)}>
      <App />
    <Provider>  
  ,document.querySelector('.container')
);
```

### Applying multiple middlewares:

```js
import { createStore, applyMiddleware } from 'redux';
import promise from 'redux-promise';
import createLogger from 'redux-logger';
import todoApp from './reducers';

const configureStore = () => {
  const middlewares = [promise];
  if (process.env.NODE_ENV !== 'production') {
    middlewares.push(createLogger());
    // Note: you can supply options to `createLogger()`
  }

  return createStore(
    todoApp,
    applyMiddleware(...middlewares)
  );
};

export default configureStore;
```

### 





