# Component Lifecycle

---

From React Doc:

> We can declare special methods on the component class to run some code when a component mounts and unmounts:
>
> These methods are called “lifecycle hooks”.

**Note:  Lifecycle Hooks are only available in Containers, a.k.a Class Based Components**

Doc: [https://reactjs.org/docs/react-component.html](https://reactjs.org/docs/react-component.html)

Cheatsheet:

[https://gist.github.com/bvaughn/923dffb2cd9504ee440791fade8db5f9](https://gist.github.com/bvaughn/923dffb2cd9504ee440791fade8db5f9)

### Component Lifecycle - Creation:

![](/assets/component_lifecycle_creation.png)Note:

* **Side-Effect** here means things that can change state . 
  * E.g. Calling a API that returns data and changes state 
* **componentDidMount\(\)** is commonly used for Side-Effects
* When child components are rendered, they will have lifecycles of their own

### Component Lifecycle - Deletion:

There also is one Lifecycle method which gets executed **right before **a Component is **removed **from the DOM:

`componentWillUnmount()`

### Component Lifecycle - Update \(triggered by parent\):

![](/assets/component_lifecycle_update_from_parent.png)

Note:

* Inside **shouldComponentUpdate\(\),** we can return true or false and component will continue updating, or won't based on what is returned. We can therefore use this hook to make our application more perfomant by checking incoming props to existing state and deciding whether or not we want to go ahead with the updates.This is the basis of **PureComponents.**

To do : add example here

* **componentWillUpdate\(\) **is a good place to cause side-effects because now we know that the **shouldComponentUpdate\(\) **has returned true and we are going ahead with the update. 

### Component lifecycle - Update \(triggered by internal this.setState call\)

### ![](/assets/lifecycle_internal.png)

### `render()` {#render}

**The`render()`method is required.**

When called, it should examine`this.props`and`this.state`and return one of the following types:

* **React elements.**
* **String and numbers.**
* **Portals**
* `null - ` Renders nothing.
* **Booleans - **Render nothing.

The`render()`function **should be pure**, meaning that it does not modify component state, it returns the same result each time it’s invoked, and it does not directly interact with the browser. If you need to interact with the browser, perform your work in`componentDidMount()`or the other lifecycle methods instead. Keeping`render()`pure makes components easier to think about.  


### `constructor()`

The constructor for a React component is called before it is mounted. When implementing the constructor for a`React.Component`subclass, you **should call`super(props)`**before any other statement. Otherwise,`this.props`will be undefined in the constructor, which can lead to bugs.

**Avoid introducing any side-effects or subscriptions in the constructor. For those use cases, use`componentDidMount()`instead.**

**The constructor is the right place to initialize state.** To do so, just assign an object to`this.state`; don’t try to call`setState()`from the constructor. The constructor is also often used to bind event handlers to the class instance.

  




