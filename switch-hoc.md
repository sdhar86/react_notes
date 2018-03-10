# Switch

---

### **Switch**

We can tell React to load only one Path

```
import { Switch } from 'react-router-dom';

... 

<Switch>
  <Route path="/new_post" exact render{() => some JSX} />
  <Route path="/:id" exact render{() => some JSX} />
</Switch>
```



