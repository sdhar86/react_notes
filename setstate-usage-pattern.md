# setState Usage Pattern:

---

we should change the previousState with  this.setState\(\) as follows: 

```
this.setState((prevState, props) => {
    return {
        counter = prevState.counter + 1;
    }
});

```

We do this because **this.setState is executed asynchronously in React** and if multiple places fire this.setState, we could introduce bugs. By using **prevState**, we can guarantee that it was not mutated from anywhere else while this setState is being fired.





