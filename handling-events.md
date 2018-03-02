# Event Handling

---

[React Doc on Handling Events](https://reactjs.org/docs/handling-events.html)

**Example of event changing state in a class based component: **

Example:

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
    <div>{this.state.name}</div>
    <button onClick={ this.switchNameHandler }> Switch Name </button>
    );
   }
  }
```

Note

* in normal html, we would use onclick , but in JSX it is onClick \(for other events see, docs\)
* Do not call the function inside the event 
  * **Correct**: onClick={ this.switchNameHandler } . We just want to pass a reference 
  * **Wrong** : onClick={ this.switchNameHandler\(\) } . This would execute immidiately when React renders to the DOM.



