# React Router

---

**Documentation**: [https://reacttraining.com/react-router/web/guides/philosophy](#)

Its about showing different pages to the user in **SPA** \(Single Page Application\):

It just renders different coponents based on url path.

For Web Development, we can just install **react-router-dom**  which uses **react-router** as its dependency.

### Using React-router in a project

Then we need  a Higher Order Components propvided by the package to add routing to the entire application:

Example using **BrowserRouter** in App.js

```
import { BroswerRouter } from 'react-router-dom';

...

render(){
    return(
        <BrowserRouter>
            <div className="App">
             ....
            </div>
        </BrowserRouter>
    )
}
```

### Route

We can then use it as follows:

```
import { Route } from 'react-router-dom';

...

<Route path="/" render{() => some JSX} />
<Route path="/new_post" render{() => some JSX} />
```

**Note**:

* Now if we visit /new\_post, it also matches "/" path,  to not run into this issue, we can use the **exact** keyword for the route to match perfectly with the address url.

* Example:

```
<Route path="/" exact render{() => some JSX} />
```

* Instead of doing the render manually, we can also just **pass in a component**

```
<Route path="/" exact component = "Posts" />
```

### 



