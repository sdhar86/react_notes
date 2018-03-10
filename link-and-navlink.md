# Link and NavLink

---

[Link Docs](https://reacttraining.com/react-router/web/api/Link)

[NavLink Docs](https://reacttraining.com/react-router/web/api/NavLink)

We can use these to navigate around our application without reloading the page. These just re-renders component.

**Link: **

Provides declarative, accessible navigation around your application.

```
import { Link } from 'react-router-dom'

<Link to="/about">About</Link>
```

When we use Link and pass in a pathname, it buids an **absolute path** for us , That is it will always add  the passed in path to the root path.

```
<Link to={{pathname: /new_post }}> New Post </Link>
```

This is create **example.com/new\_post** no matter where the Link Router was placed, but if we want a **relative path, **we can use

```
<Link to={{pathname: this.props.match.url + '/new_post' }}> New Post </Link>
```

so, if we now call from /posts path - Link will take us to **example.com/posts/new\_post**

**Note**

* We can also wrap a component with Link element

**Example:**

```
<Link to={'/' + post.id} key={post.id}>
 <Post title = {post.title}
   author={post.author}
 />
</Link>
```

**NavLink: **

A special version of the &lt;Link/&gt; that will add styling attributes to the rendered element when it matches the current URL.

```
import { NavLink } from 'react-router-dom'

<NavLink to="/about">About</NavLink>
```

This adds a class="active" on navigation link - now in UI which Link is active by adding css

We can also change the classname with **activeClassName **attribute

```
<NavLink to={{ pathname: '/new_post' activeClassName='awesome'}}> Click here Amigo! </NavLink>
```

it will now add class="awesome" to active links

We also should use **exact **to target specific links.

```
activeStyle ={{
    color: 'red'
    textDecoration: 'underline
}}
```



