# Lazy Loading

---

When we load the app, the entire bundle.js for all components is downloaded. The App can be more performant if we can only download what is needed. 

It is not useful for small apps

This also known as **code splitting. This only works for React router 4 and dependent on webpack configuration. It works for create-react-app.**



We can make a HOC known as **asyncComponent, **and put it in async\_component.js. 

```
import React, { Component } from 'react'; 

const asyncComponnet = (importComponent) => {
    return class extends Component{
       state = { 
          component: null
       }
       
       componentDidMount () {
          importComponent()
            .then(cmp => {
              this.setState({component: cmp.default});
            });
       }
       
       render(){
          const C = this.state.component; 
          return C ? <C {...this.props} /> : null;
       
       }
    
    }
    
}

export default asyncComponent;
```

Now we can use it like so: 

instead of 

```
import NewPost from './new-posts';
```

we can do 

```
import asyncComponent from '../hoc/asyncComponent';

const AsyncNewPost = asyncComponent(() => {
    return import('.../NewPost')
})

.....


{AsyncNewPost} instead of {NewPost}
```













