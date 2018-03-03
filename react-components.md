# **Components and JSX**

---

### **Components and React: **

Components are the core building block of React apps.

A typical React app therefore could be depicted as a component tree - having one root component \("App"\) and then an potentially infinite amount of nested child components.

Each component needs to return/ render some JSX code - it defines which HTML code React should render to the real DOM in the end.

### **What is JSX?**

**JSX is NOT HTML** but it looks a lot like it. Differences can be seen when looking closely though \(for example className in JSX vs class in "normal HTML"\). JSX is just syntactic sugar for JavaScript, allowing you to write HTMLish code instead of nested React.createElement\(...\) calls.

Sample JSX:

```
return (
<div className = "App">
    <h1> React Component goes here.. </h1>
</div> 
):
```

Note:

* It looks like HTML a lot
* we use the keyword "className"  instead of the keyword "class" in JSX
* Inside the render\(\) method, **we typically have one root level &lt;div&gt;&lt;/div&gt;**

Example:

```
render() {
    return (
        <div>
          <p></p>
          <div> </div>
        </div>
    );
}
```

However, it is possible to return adjacent elements. We have to pass it in as a array and with keys \(just like dynamically generated lists have.\)

```
    return [
          <p key="1"></p>, 
          <div key="2"></div> 
   ]
```

### Using Higher Order Component to return adjacent elements: 

Let's make a Aux.js

```
import React from 'react';

const aux = (props) => props.children;

export default aux; 

```

then we can use the Aux Component

```
import Aux from ../hoc/Aux

return (
    <Aux>
        <p></p>
        <div></div>
    </Aux>
)
```

### Fragment:

In React 16.2 - Fragment is Introduced, so we do not have to implement the Aux HOC ourselves

Instead of :

```
import Aux from ../hoc/Aux;

...

return (
    <Aux>
        <p></p>
        <div></div>
    </Aux>
)
```

Now, we can just do :

```
return (
    <>
        <p></p>
        <div></div>
    </>
)
```



### **Component Types:**

### When creating components, you have the choice between two different ways:

1. **Functional components **\(also referred to as "presentational", "dumb" or "stateless" components\) 

```
 const cmp = () => { return some JSX}
```

1. **Class-based components **\(also referred to as "containers", "smart" or "stateful" components\)

```
class Cmp extends Component {
  render() { 
     return JSX
   }
 }
```

**Note:**

* Always try to use functional components as much as possible, as they are easy to maintain and good for code readability and understanding. 
* Only Class based components/Containers have states and has the avaibility of life cycle hooks

![](/assets/import.png)

### Dynamic content in JSX:

Just wrap the content in {} and Javascript will execute inside the curly braces.

