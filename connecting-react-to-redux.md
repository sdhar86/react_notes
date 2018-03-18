# Connecting React to Redux

---

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





