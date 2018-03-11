# Programmatic Navigation

---

```
this.props.history.push{{pathname: '/' + id}}
```

We can use this to go to another page after a http request comes back with dat or form has been successfully submitted etc.

### Redirecting User

From inside **Switch**

```
<Switch>
 <Route path="/new-post" component={NewPost} />
 <Route path="/posts" component={Posts} />
 <Redirect from="/" to="/posts" />
</Switch>
```

Outside the **Switch** Statement **from cannot be specified, only to is needed.**

We can **conditionally redirect** based on a state

```
let redirect = null;

if (this.state.submitted) {
    redirect = <Redirect to="/posts">
}

...

return(
    {redirect}
    ..

 )
```

We can also use **replace **which works same as **Redirect**

```
this.props.history.replace{{pathname: '/' + id}}
```

**Note**

* With Push , if we hit the back button in the browser, we go back to the last page
* With Replace or Redirect, we do not, instead we go the page before last's because in redirect or replace, the last component was never rendered.

### Navigation Guards

We can use Redirect to add guards that will only let people in for who are authenticated. 

```
  {this.state.auth ? <Route path="/new_post component={NewPost}"> : null}
```

or in a lifecycle hook: 

```
componentDidMount () {
    if user not authenticaed
     // use history.replace  to kick him out
}
```



