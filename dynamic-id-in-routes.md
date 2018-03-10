# Dynamic parameters in Routes

---

We can pass dynamic params as follows

```
<Route path="/:id" exact render{() => some JSX} />
```

But if I had

```
<Route path="/:id" exact render{() => some JSX} />
<Route path="/new_post" exact render{() => some JSX} />
```

:id will match both itself and /new\_post

so we need to change the order

```
<Route path="/new_post" exact render{() => some JSX} />
<Route path="/:id" exact render{() => some JSX} />
```

### Extracting the dynamic parameter in container

Using the router specific params: 

```
{this.props.match.params.id}
```

### Parsing Query Params: 

```
<Link to="/my-path?start=5">Go to Start</Link> 

 or 
 
 <Link 
    to={‌{
        pathname: '/my-path',
        search: '?start=5'
    }}
    >Go to Start</Link>
```

We can now do 

```
{props.location.search}
```

to get 

`?start=5`

You probably want to get the key-value pair, without the`?`  and the`=` . Here's a snippet which allows you to easily extract that information:

```
componentDidMount() {
    const query = new URLSearchParams(this.props.location.search);
    for (let param of query.entries()) {
        console.log(param); // yields ['start', '5']
    }
}
```

**`URLSearchParams` ** is a built-in object, shipping with vanilla JavaScript. It returns an object, which exposes the`entries()`  method.`entries()`  returns an Iterator - basically a construct which can be used in a`for...of...`  loop \(as shown above\).

When looping through`query.entries()` , you get **arrays **where the first element is the **key name **\(e.g.`start` \) and the second element is the assigned **value **\(e.g.`5` \).

### Parsing Fragment

```
<Link to="/my-path#start-position">Go to Start</Link> 

or 
<Link 
    to={‌{
        pathname: '/my-path',
        hash: 'start-position'
    }}
    >Go to Start</Link>
```



To parse, we can do : 

```
{props.location.hash}
```





