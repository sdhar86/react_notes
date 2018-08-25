# setState Usage Pattern:

---

### [https://reactjs.org/docs/react-component.html\#setstate](https://reactjs.org/docs/react-component.html#setstate) {#setstate}

### `setState()` {#setstate}

`setState()`**enqueues changes** to the component state and tells React that this component and its children need to be re-rendered with the updated state. This is the primary method you use to update the user interface in response to event handlers and server responses.

T**hink of**`setState()`**as a **_**request **_**rather than an immediate command to update the component**. For better perceived performance, **React may delay it,** and then update several components in a single pass. React does not guarantee that the state changes are applied immediately.

`setState()`**does not always immediately update the component. **It may batch or defer the update until later. This makes reading`this.state`right after calling`setState()`a potential pitfall. Instead, use`componentDidUpdate`or a`setState`callback \(`setState(updater, callback)`\), either of which are guaranteed to fire after the update has been applied.

`setState()`will always lead to a re-render unless`shouldComponentUpdate()`returns`false`. If mutable objects are being used and conditional rendering logic cannot be implemented in`shouldComponentUpdate()`, calling`setState()`only when the new state differs from the previous state will avoid unnecessary re-renders.

The first argument is an`updater`function with the signature:

```js
(prevState, props) => stateChange
```

`prevState`is a reference to the previous state. It should not be directly mutated. Instead, changes should be represented by building a new object based on the input from`prevState`and`props`. For instance, suppose we wanted to increment a value in state by`props.step`:

```js
this.setState((prevState, props) => {
  return {counter: prevState.counter + props.step};
});
```

Both`prevState`and`props`received by the updater function are guaranteed to be up-to-date. The output of the updater is shallowly merged with`prevState`.

The second parameter to`setState()`is an optional callback function that will be executed once`setState`is completed and the component is re-rendered. Generally we recommend using`componentDidUpdate()`for such logic instead.

You may optionally pass an object as the first argument to`setState()`instead of a function:

```js
setState(stateChange[, callback])
```

This performs a shallow merge of`stateChange`into the new state, e.g., to adjust a shopping cart item quantity:

```js
this.setState({quantity: 2})
```

This form of`setState()`is also asynchronous, and multiple calls during the same cycle may be batched together. For example, if you attempt to increment an item quantity more than once in the same cycle, that will result in the equivalent of:

```js
Object.assign(
  previousState,
  {quantity: state.quantity + 1},
  {quantity: state.quantity + 1},
  ...
)
```

Subsequent calls will override values from previous calls in the same cycle, so the quantity will only be incremented once. If the next state depends on the previous state, we recommend using the updater function form, instead:

```js
this.setState((prevState) => {
  return {quantity: prevState.quantity + 1};
});
```



