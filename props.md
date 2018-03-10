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

**Question**:

Do I have to accept props in functional component, or no?

```
const post = () => { here I get access to props.x  }
```

Or

```
const post = ( props ) => { here I get access to props.x }
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

We can also initiate state with constructor as  follows:

```
class NewPost extends Component { 
 constructor(props){
     super(props);
     this.state = { counter: 1};
 }


render () {
    return (
        <div>{this.state.counter}</div>
        );
    }
}
```

### Manipulating State:

React gives us a nice methiod to use: **setState**

Example:

To change the state defined in the previous example, we can just use:

```
this.setState({
 counter: 2
});
```

If the existing looked as follows:

```
state = {
 counter: 1 
 param_a: 2
}
```

The setState method would have left the **param\_a** alone. That is, it would still remain 2, and that part of the state wont be touched. This way, we can update the state partially. It basically merges the object defined in setState with existing state value.

Inside setState something like this  happens:

```
state = {a: 1, b: 2};

setState = (Obj) => {
 state = { ...state, ...Obj }    
}
```

 **Note: **

* the spread operator does a replace of a key, and not a merge
* **React setState does a shallow merge \( uses spread operators\).**
*  We can use the **lodash merge** does nested recursive merge \(deep merge\)
* \`undefined\` replaces value in ES6 spread operator - lodash merge does not 
  * if we have use spread operator to merge {b: undefined} the b param will be set to undefined, but lodash merge does not do this. 
* in a redux app we can use **replace** and **merge **actions- **replace** will use the spread operators, **merge** will use lodash merge



