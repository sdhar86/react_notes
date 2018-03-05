# React Router

---

For Web Development, we cam just install **react-router-dom**  which uses **react-router** as its dependency

[https://reacttraining.com/react-router/web/guides/philosophy](https://reacttraining.com/react-router/web/guides/philosophy)

We can then use it as follows:

```
import { Route } from 'react-router-dom';

...

<Route path="/" render{() => some JSX} />
```

We can use the **exact** keyword for the route to match perfectly

```
<Route path="/" exact render{() => some JSX} />
```

Instead of doing the render manually, we can just pass in a component

```
<Route path="/" exact rcomponent = "Posts" />
```



