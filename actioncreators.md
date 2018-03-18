# Action Creators

---

### Lifecycle of an action: 

![](/assets/action_lifecycle.png)

An Action creator is a function that returns an action. Which will eventually be dispatched and flow through all the reducers. Action creators are typically kept separate from components and reducers in order to help with maintainability.

Using Action Creator and **bindActionCreator**, the mapDispatchtoProps can be simplified even more. 

Example: 

We can make a action creator: 

```js
export function selectBook(book) {
    type: BOOK_SELECTED
    book: {title: 'Book 2'}
}
```

then, in the component

```js
import { connect } from 'react-redux';
import { selectBook } from 'actions/index';
import { bindActionCreators } from 'redux';

...

function mapDispatchToProps(dispatch){
  return bindActionCreators({selectBook: selectBook}, dispatch)
}
```

 







