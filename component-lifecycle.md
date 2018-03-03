# Component Lifecycle

---

From React Doc:

> We can declare special methods on the component class to run some code when a component mounts and unmounts:
>
> These methods are called “lifecycle hooks”.

**Note:  Lifecycle Hooks are only available in Containers, a.k.a Class Based Components**

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
* **componentWillUpdate\(\) **is a good place to cause side-effects because now we know that the **shouldComponentUpdate\(\) **has returned true and we are going ahead with the update. 



### Component lifecycle - Update \(triggered by internal this.setState call\)

![](/assets/lifecycle_internal.png)







