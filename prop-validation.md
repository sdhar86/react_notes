# Prop Validation \(PropTypes\):

---

We use prop types

```
npm install --save prop-types;
```

We use it as follows:

```
class Person extends Component {
    ...

    render(){

    }

    ...
 }   

Person.propTypes = {
    click: PropTypes.func,
    name: PropTypes.string,
    age: PropTypes.number,

}

export default Person;
```

Here we define what props should be and if we pass in a wrong prop, we get a ''invalid prop type" error message.

Available PropType Checks:

[https://reactjs.org/docs/typechecking-with-proptypes.html](https://reactjs.org/docs/typechecking-with-proptypes.html)

