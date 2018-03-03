# Two Way Binding/Controlled Component

---

```
import React, {Component} from 'react';

class SearchBar extends Component {
  constructor(props){
   super(props)
   
   this.state = {term: ''}
  }
  
   
  render(){
    return (
       <div>
         <input onChange={(event) =>  this.setState({term: event.target.value })} />
         Value: {this.state.term}
       </div>
     );
  }   
}

export default SearchBar
```

In here the value makes the state Change , but Really State should change the value, not the other way around: 

```

class SearchBar extends Component {
  constructor(props){
   super(props)
   
   this.state = {term: ''}
  }
  
   
  render(){
    return (
       <div>
         <input 
         value = {this.state.term}
         onChange={(event) =>  this.setState({term: event.target.value })} 
         />
       </div>
     );
  }   
}

export default SearchBar
```

After the update, the **onChange** event actually changes the state and not the event, the changed state in term re renders the DOM and shows the change in the value of the input component. 

We can now have a starting value for the input: 

```
constructor(props){
    super(props)
    this.state = {term: 'Starting Value...'}
}
```





