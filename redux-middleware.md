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

import ReduxPromise from 'redux-promise';
...

const createStoreWithMiddleware = applyMiddleware()(createStore);
...

ReactDOM.render(
    <Provider store={createStoreWithMiddleware(reducers)}>
      <App />
    <Provider>  
  ,document.querySelector('.container')
);
```

In this case the redux promise waits for a promise to be the resolved or rejected before passing to the reducers. 















