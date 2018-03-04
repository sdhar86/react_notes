# Async Await with Lifecycle Hooks

---

**Example**: 

```
async componentDidMount() {
  const res = await fetch('https://example.com')
  const something = await res.json()
  this.setState({something})
}
```

[https://www.dalejefferson.com/blog/async-await-with-react-lifecycle-methods/](https://www.dalejefferson.com/blog/async-await-with-react-lifecycle-methods/)

