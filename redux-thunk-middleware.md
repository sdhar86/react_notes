# Redux Thunk Middleware

---

It is another way of dealing with asynchrnous action creators. **It gives us direct control over the dispatch method**.

![](/assets/dispatch.png)

```
npm install --save redux-thunk
```

Then, to enable Redux Thunk, use[`applyMiddleware()`](http://redux.js.org/docs/api/applyMiddleware.html):

```js
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import rootReducer from './reducers/index';

// Note: this API requires redux@>=3.1.0
const store = createStore(
  rootReducer,
  applyMiddleware(thunk)
);
```

**Usage**:

actions/index.js:

```js
import axios from 'axios';

export function fetchUsers() {
    const resquest = axios.get('some api url');

    return(dispatch) => {
        request.then((data) => {
            dispatch({ type: 'FETCHED_PROFILES', payload: data })
        });
    }
}
```

![](/assets/thunk_1.png)

we can also add two more action types to have a granular control over UI during the fetch prpocess

```js
import axios from 'axios';

export function fetchUsers() {
    const resquest = axios.get('some api url');

    dispatch({ type: 'FETCH_PROFILES_START' })

    return(dispatch) => {
        request.then((data) => {
            dispatch({ type: 'FETCH_PROFILES_SUCCESS', payload: data })
        }).catch((err) => {
            dispatch({ type: 'FETCH_PROFILES_FAILURE', payload: err.message })
        });
    }
}
```



ES6 Way 

Synchronous Action Creator example: 

```js
export const subtract = (value) => {
    return {
        type: 'SUBTRACT',
        value: value
    };
}
```

Real life ES6 example: 

```js
const convertCardToToken = () => (dispatch, getState) =>  {
  const state = getState();

  const creditCardObj = reducers.getCreditCard(state);
  const {number, cvc, exp_month, exp_year} = creditCardObj;

  return WebUtil.convertCardToToken(number, cvc, exp_month, exp_year).then(
    response => {
      console.log("successfully converted card to token", response);
      dispatch({
        type: K.UPDATE_CREDIT_CARD,
        payload: {
          stripeToken: response.id
        }
      });
      return response;
    },
    error => {
      throw helpers.makeErrorPrettyObject("Credit Card Error", error);
    }
  )
};
```











