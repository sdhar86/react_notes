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



