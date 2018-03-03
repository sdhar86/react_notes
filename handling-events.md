# Handling Events and Passing Event Handlers

---

[React Doc on Handling Events](https://reactjs.org/docs/handling-events.html)

### **Example of event changing state in a class based component: **

```
 class NewPost extends Component { 
   state = {
     name: "Roman"
   };

   switchNameHandler = () => {
     this.setState{
      name: "Vlad"
     }
   }

  render () { 
   return (
    <div>
     <div>{this.state.name}</div>
     <button onClick={ this.switchNameHandler }> Switch Name </button>
    </div>
    );
   }
  }
```

Note

* in normal html, we would use onclick , but in JSX it is onClick \(for other events see, docs\)
* Do not call the function inside the event 
  * **Correct**: onClick={ this.switchNameHandler } . We just want to pass a reference 
  * **Wrong** : onClick={ this.switchNameHandler\(\) } . This would execute immidiately when React renders to the DOM.
* Right after changing the State, the DOM would be updated to reflect the new state. 

### Passing Method Reference between Components:

Let's say there is a functional component **Person** inside **NewPost**

```
 class NewPost extends Component { 
   state = {
     name: "Roman"
   };

   switchNameHandler = () => {
     this.setState{
      name: "Vlad"
     }
   }

  render () { 
   return (
    <div>
     <div>{this.state.name}</div>
     <Person click={this.switchNameHandler} />
    </div> 
    );
   }
  }
```

here we referenced **switchNameHandler **in the property click. Then inside the Person component, we can use the click handler as follows:

```
import React from 'react';

const person = (props) => {
  return (
    <div>
      <p onClick={props.click}> Click here </p>
    </div>
    )
 }

  export default person;
```

**Passing value to function from the event **

We can change the handler to:

```
switchNameHandler = (name) => {
    this.setState{
        name: name
    }
}
```

There are two ways of passing the data:

1. **With bind - binds the this reference outside the function to that of inside **

```
onClick={ this.switchNameHandler.bind(this, "Sergei") }
```

1. **Executing an anoymous ES6 arrow function**

```
  onClick={ () => this.switchNameHandler("Sandeep") }
```

When using Arrow function in one line,  the return keyword is automatically added ,

so this gets returned:

```
this.switchNameHandler("Sandeep")
```

So, the switcNameHandler\(\) function does not get executed, instead it gets returned. Since the anonymous fat arrow function gets executed and returns it.

