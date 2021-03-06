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

**The**`render()`**method is required.**

When called, it should examine`this.props`and`this.state`and return one of the following types:

* **React elements.**
* **String and numbers.**
* **Portals \*\* **
* `null -` Renders nothing.
* **Booleans - **Render nothing.

The`render()`function **should be pure**, meaning that it does not modify component state, it returns the same result each time it’s invoked, and it does not directly interact with the browser. If you need to interact with the browser, perform your work in`componentDidMount()`or the other lifecycle methods instead. Keeping`render()`pure makes components easier to think about.

### `constructor()`

The constructor for a React component is called before it is mounted. When implementing the constructor for a`React.Component`subclass, you **should call**`super(props)`before any other statement. Otherwise,`this.props`will be undefined in the constructor, which can lead to bugs.

**Avoid introducing any side-effects or subscriptions\*\*  in the constructor. For those use cases, use**`componentDidMount()`**instead.**

**The constructor is the right place to initialize state.** To do so, just assign an object to`this.state`; don’t try to call`setState()`from the constructor. The constructor is also often used to bind event handlers to the class instance.

### `componentWillMount()` {#componentwillmount}

`componentWillMount()`is invoked just before mounting occurs. It is called before`render()`, therefore calling`setState()`synchronously in this method **will not trigger an extra rendering**. Generally, we recommend using the`constructor()`instead.

**Avoid introducing any side-effects or subscriptions in this method. For those use cases, use**`componentDidMount()`**instead.**

This is the only lifecycle hook called on server rendering. \*\*

### `componentDidMount()` {#componentdidmount}

`componentDidMount()`is invoked immediately after a component is mounted. Initialization that requires DOM nodes should go here. **If you need to load data from a remote endpoint, this is a good place to instantiate the network request.**

This method is a good place to set up any subscriptions. If you do that, don’t forget to unsubscribe in`componentWillUnmount()`.

Calling`setState()`in this method will trigger an extra rendering, but it will happen before the browser updates the screen. This guarantees that even though the`render()`will be called twice in this case, the user won’t see the intermediate state. Use this pattern with caution because it often causes performance issues. It can, however, be necessary for cases like modals and tooltips when you need to measure a DOM node before rendering something that depends on its size or position.

### `componentWillReceiveProps()` {#componentwillreceiveprops}

`componentWillReceiveProps()`is invoked before a mounted component receives new props. If you need to update the state in response to prop changes \(for example, to reset it\), you may compare`this.props`and`nextProps`and perform state transitions using`this.setState()`in this method.

Note that React will call this method even if the props have not changed, so make sure to compare the current and next values if you only want to handle changes. This may occur when the parent component causes your component to re-render.

**React doesn’t call**`componentWillReceiveProps()`**with initial props during **[**mounting**](https://reactjs.org/docs/react-component.html#mounting)**. It only calls this method if some of component’s props may update.** Calling`this.setState()`generally doesn’t trigger`componentWillReceiveProps()`.

### `shouldComponentUpdate()`

Use`shouldComponentUpdate()`to let React know if a component’s output is not affected by the current change in state or props. The default behavior is to re-render on every state change, and in the vast majority of cases you should rely on the default behavior.

`shouldComponentUpdate()`is invoked before rendering when new props or state are being received.** Defaults to**`true`. This method is not called for the initial render or when`forceUpdate()`is used.

**Returning**`false`**does not prevent child components from re-rendering when **_**their **_**state changes.**

Currently, if`shouldComponentUpdate()`returns`false`, then[`componentWillUpdate()`](https://reactjs.org/docs/react-component.html#componentwillupdate),[`render()`](https://reactjs.org/docs/react-component.html#render), and[`componentDidUpdate()`](https://reactjs.org/docs/react-component.html#componentdidupdate)will not be invoked. Note that in the future React may treat`shouldComponentUpdate()`as a hint rather than a strict directive, and returning`false`may still result in a re-rendering of the component.

If you determine a specific component is slow after profiling, you may change it to inherit from[`React.PureComponent`](https://reactjs.org/docs/react-api.html#reactpurecomponent)which implements`shouldComponentUpdate()`with a shallow prop and state comparison. If you are confident you want to write it by hand, you may compare`this.props`with`nextProps`and`this.state`with`nextState`and return`false`to tell React the update can be skipped.

**We do not recommend doing deep equality checks or using**`JSON.stringify()`**in**`shouldComponentUpdate()`**. It is very inefficient and will harm performance.**

### `componentWillUpdate()` {#componentwillupdate}

`componentWillUpdate()`is invoked just before rendering when new props or state are being received. Use this as an opportunity to perform preparation before an update occurs. This method is not called for the initial render.

**Note that you cannot call**`this.setState()`**here; nor should you do anything else \(e.g. dispatch a Redux action\) that would trigger an update to a React component before**`componentWillUpdate()`**returns.**

If you need to update`state`in response to`props`changes, use`componentWillReceiveProps()`instead.

### `componentDidUpdate()` {#componentdidupdate}

  
`componentDidUpdate()`is invoked immediately after updating occurs. This method is not called for the initial render.

**Use this as an opportunity to operate on the DOM when the component has been updated. This is also a good place to do network requests as long as you compare the current props to previous props \(e.g. a network request may not be necessary if the props have not changed\).**

### `componentWillUnmount()` {#componentwillunmount}

is invoked immediately before a component is unmounted and destroyed. Perform any necessary cleanup in this method, such as invalidating timers, canceling network requests, or cleaning up any subscriptions that were created in `componentDidMount().`

### `componentDidCatch()` {#componentdidcatch}

Error boundaries are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of the component tree that crashed. Error boundaries catch errors during rendering, in lifecycle methods, and in constructors of the whole tree below them.

A class component becomes an error boundary if it defines this lifecycle method. **Calling`setState()`in it lets you capture an unhandled JavaScript error in the below tree and display a fallback UI. Only use error boundaries for recovering from unexpected exceptions; don’t try to use them for control flow.**

  








