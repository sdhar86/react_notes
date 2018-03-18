# Redux Promise MiddleWare

---

```js
npm i redux-promise-middleware -s
```

Import the middleware and include it in`applyMiddleware`when creating the Redux store:

```js
import promiseMiddleware from 'redux-promise-middleware'

composeStoreWithMiddleware = applyMiddleware(
  promiseMiddleware(),
)(createStore)
```

## Use

Dispatch a promise as the value of the`payload`property of the action.

```js
const foo = () => ({
  type: 'FOO',
  payload: new Promise(),
})
```

A pending action is immediately dispatched.

```js
{
  type: 'FOO_PENDING'
}
```

Once the promise is settled, a second action will be dispatched. If the promise is resolved a fulfilled action is dispatched.

```js
{
  type: 'FOO_FULFILLED'
  payload: {
    ...
  }
}
```

On the other hand, if the promise is rejected, a rejected action is dispatched.

```js
{
  type: 'FOO_REJECTED'
  error: true,
  payload: {
    ...
  }
}
```

If you need to send extra information not included in the`payload`property, you can use the`meta`property.

```js
const foo = () => ({
  type: 'FOO',
  payload: new Promise(),
  meta: {
    ...
  },
})
```

# Use with Reducers

Handling actions dispatched by Redux promise middleware is simple by default.

```js
const FOO_TYPE = 'FOO';

// Dispatch the action
const fooActionCreator = () => ({
  type: FOO_TYPE
  payload: Promise.resolve('foo')
});

// Handle the action
const fooReducer = (state = {}, action) => {
  switch(action.type) {
    case `${FOO_TYPE}_PENDING`:
      return;

    case `${FOO_TYPE}_FULFILLED`:
      return {
        isFulfilled: true,
        data: action.payload
      };

    case `${FOO_TYPE}_REJECTED`:
      return {
        isRejected: true,
        error: action.payload
      };

    default: return state;
  }
}
```



### Promise Suffixes

Optionally, the default promise suffixes can be imported from this module. This can be useful in your reducers.

```js
import { PENDING, FULFILLED, REJECTED } from 'redux-promise-middleware';

```

## Large Applications

In a large application with many async actions, having many reducers with this same structure can grow redundant.

To keep your reducers [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself), you might see value in using a solution like [type-to-reducer](https://github.com/tomatau/type-to-reducer).

```js
import typeToReducer from 'type-to-reducer';

const BAR_TYPE = 'BAR';

// Dispatch the action
const barActionCreator = () => ({
  type: BAR_TYPE
  payload: Promise.resolve('bar')
});

// Handle the action
const barReducer = typeToReducer({
  [BAR_TYPE]: {
    PENDING: () => ({
      // ...
    }),
    REJECTED: (state, action) => ({
      isRejected: true,
      error: action.payload
    }),
    FULFILLED: (state, action) => ({
      isFulfilled: true,
      data: action.payload
    })
  }
}, {});
```











