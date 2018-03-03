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

### Dynamic content in JSX:

Just wrap the content in {} and Javascript will execute inside the curly braces.

