# "Ref" and the DOM

---

According to React docs:

> In the typical React dataflow, props are the only way that parent components interact with their children. To modify a child, you re-render it with new props. However, there are a few cases where you need to imperatively modify a child outside of the typical dataflow. The child to be modified could be an instance of a React component, or it could be a DOM element. For both of these cases, React provides an escape hatch.

Note:

* **Do not over use refs **
* A few good uses are; 
  * Managing focus, doing text selection and managing media playback 
  * Triggering imperative animations
  * Integrating with third-party DOM libraries

Example from React Doc:

    class CustomTextInput extends React.Component {
      constructor(props) {
        super(props);
        this.focusTextInput = this.focusTextInput.bind(this);
      }

      focusTextInput() {
        // Explicitly focus the text input using the raw DOM API
        this.textInput.focus();
      }

      render() {
        // Use the `ref` callback to store a reference to the text input DOM
        // element in an instance field (for example, this.textInput).
        return (
          <div>
            <input
              type="text"
              ref={(input) => { this.textInput = input; }} />
            <input
              type="button"
              value="Focus the text input"
              onClick={this.focusTextInput}
            />
          </div>
        );
      }
    }

The line below:

`ref={(input) => { this.textInput = input; }}`

helps us create a reference to the input element. This line creates a property named **textInput **for the class while the render function is executed. So, the prop **textInput **will be available in all lifecycle functions after render\(\). E.g. in **componentDidMount\(\) lifecycle hook.**

We are using that reference to focus the input in **focusTextInput\(\)** method

Note:

* **Ref can only be used in Component based classes. **
* **Ref to get values from inputs is Jquery way of doing things, the data of form inputs should already be in our state, In general we use ref when we get lazy. **
* **We should try to not get get data using refs \(in general\) that should already be in state. **



