# Props and State

---

**props** and **state** are CORE concepts of Components. Changes in props and/ or state trigger React to rerender your components and potentially update the DOM in the browser

### Props:

### props allow you to pass data from a parent \(wrapping\) component to a child \(embedded\) component.![](/assets/props.png)**Accessing Props inside functional and class based component:**

* Accessing _**title**_ in functional component:  _**props.title**_
* Accessing _**title**_ in class-based component: _**this.props.title**_

### Props Children property:

```
<Post title="First Post"> Hey Yo </Post>
```

We can now access the "**Hey Yo**" passed in the component using the reserved keyword _**children. **_

Inside the Component, _**props.children**_** **is "**Hey Yo**"

### State:

Whilst props allow you to pass data down the component tree \(and hence trigger an UI update\), state is used to change the component, well, state from within. Changes to state also trigger an UI update.

![](/assets/state.png)

Example:

```
 class NewPost extends Component { 
   state = {
     counter: 1
   };

  render () { 
   return (
    <div>{this.state.counter}</div>
    );
   }
  }
```

Here, the **NewPost** component contains **state** . **Only class-based components can define and use state** . You can of course pass the state down to functional components, but these then can't directly edit it. **state** simply is a property of the component class, you have to call it **state** though - the name is not optional. You can then access it via **this.state** in your class JSX code

Note:

* Whenever state changes, the component will re-render and reflect the new state.





