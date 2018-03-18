# Generating Containers with`connect()`from React Redux

---

Now that we have`react-redux`, we are using its`Provider`component to pass`store`down via context.

Our container components are similar: they need to re-render when the store's state changes, they need to unsubscribe from the store when they unmount, and they take the current state from the Redux`store`and use it to render the presentational components with some props that they calculate.

They also need to specify the`contextTypes`to get the store from the context.



