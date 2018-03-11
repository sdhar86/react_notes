# 404 Page

---

We can catch unknown routes with

```
<Switch>
  <Route path="/posts" component={Posts} />
  <Route path="/new-posts" component={NewPosts} />
  <Route render={() => <h1> Not found - Go home you are drunk</h1>} />
</Switch>
```

if we specify a** Route component without a path in the end**, it catches everything.  We can use that to make 404 pages.

We can also use redirect for this

at the very end..

```
<Redirect from="/" to="/posts" />
```

It will catch everythig and send the user back to /posts

