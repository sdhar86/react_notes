# Router Props and WithRouter Higher Order Component

---

### React Router Special Props

React Router adds special props to get information or interact with our components

![](/assets/special_router_props.png)

### WithRouter HOC

But the Nested Containers in a route loaded Container won't have the special Router props. Here we can use the **withRouter HOC **to get access to the special router component. 

**Example**

```
import { WithRouter } from 'react-router-dom';

...
export default withRouter(post);
```

Now the Post component will have Router props in nested components 

We can also simply pass the props down:

```
<Post {..this.props} />
```





