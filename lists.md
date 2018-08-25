# Lists and Conditional Patterns:

---

### Conditional rendering based on state

```
return (
  { this.state.show? 
    <div>
      more Jsx
    </div> : null
  }
);
```

All we write is javaScript - so we can use a ternary expression as show above.

For readability, we can also extract the content out

```
let content = null;

if (this.state.show) {
  content = (
    some Jsx
  );
}

return (
  {content}
 );
```

### Dynamic List Generation

```
persons = [
  { name: 'Bob' , age: 21, key: 1  }, 
  { name: 'Alice', age: 24, key: 2}, 
  { name: 'Don', age: 31, key: 3} 
 ]


{this.persons.map(person => {
    return <Person name={person.name} age={person.age} key={person.key} />
})}
```

Note: key is needed in React when rendering a list to help it update efficiently. **Key should be unique**

### Passing event from a generated List

```
deletePersonHandler = () => {
    const persons = [...this.state.persons];
    persons.splice(toDeleteIndex, 1);
    this.setState({persons: persons});
}

render() {

  {this.persons.map((person, index) => {
      return <Person 
        name={person.name} 
        age={person.age} 
        key={person.key}
        click={() => this.deletePersonHandler(index)} // this is actually a closure
       />
  })}

  OR 

{this.persons.map((person, index) => {
      return <Person 
        name={person.name} 
        age={person.age} 
        key={person.key}
        click={this.deletePersonHandler.bind(this,index)}
       />
  })}






}
```

```
this.deletePersonHandler.bind(this,index) is a factory (bind)

and () => deletePersonHandler(index) returns a function - we are still passing a reference. Therefore,
 a closure.
```

Note: In deletePersonHandler\(\) we made a copy of persons and then removed the index passed. This operation must be **immutable **

JS is not immutable, so we explicitely try to make it immutable so we save snashots of state.

Java is immutable, ruby is not for example.

Note:  to Update the array in state immutably, we could use the  ES6  spread operator

```
nameChangedHandler = (updatedName, index) => {
    const person = {
        ...this.state.persons[index]
    }; // this spreads person out and puts in a new hash 

    person.name = updatedName;
    const persons = [...this.state.persons];
    persons[index] = person;
    this.setState({persons: persons});

 }
```

We could have also used older **Object.assign\(\) **method instead of newer ES6 spread operator

```
const person = Object.assign({}, this.state.persons[index]); // shallow copy excludes prototyes.
```

To do : simulate Prototypical Inheritence and see shallow copy of Object.assign

