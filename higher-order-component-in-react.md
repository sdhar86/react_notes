# Higher Order Components in React

---

Example:

Objective: We want to included specific css classes in a component

### Composition:

[https://reactjs.org/docs/composition-vs-inheritance.html](https://reactjs.org/docs/composition-vs-inheritance.html)

Let's define a WithClass Containment

```
import React from 'react'; 

const withClass = (props) => {
    <div className={props.classes}>
        {props.childern}
    </div>
}

export default withClass
```

Now we can use WithClass in App.js

```
import classes from './App.css'
import WithClass from './hoc/WithClass'

...

return (
    <WithClass classes={classes.App}
      some JSX
    </WithClass
);
```

### Higher Order Component:

Concretely,**a higher-order component is a function that takes a component and returns a new component.**

[https://reactjs.org/docs/higher-order-components.html](https://reactjs.org/docs/higher-order-components.html)

```
import React from 'react';

const withClass = (WrappedComponent, className) = {
    return(props) => (
        <div className={className}>
            <WrappedComponent {...props} />
        </div
    )
}

export default withClass;
```

Note:

* withClass is not a functional component, it is just a function, but it returns a functional component 
* withClass does not have to return a functional component , it can return a class based component too 
* we use the** {...props} **in** WrappedComponent **to **pass along unknown props**

Now we can use it as follows in App.js:

```
import withClass from 'hoc/withClass';
import classes from './App.css'

...


return (
    <>
     some JSX
    </>
)

...

export default withClass(App, classes.App)
```

**HOC for Stateful Component:**

```
import React, {Component} from 'react';

const withClass = (WrappedComponent, className) = {
    return class extends Component {
       render(){
           return(
               <div className={className}>
                    <WrappedComponent {...this.props} />
                </div>
          );
       }
    }
}

export default withClass;
```



