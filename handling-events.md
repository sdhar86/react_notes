# Event Handling

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



