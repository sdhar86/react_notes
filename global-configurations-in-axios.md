# Global and Instance Configurations

---

### Global Config:

Example: 

```
axios.defaults.baseUrl = 'base_url';
```

Now we can leverage this in all calls and dont have to specify the base url 

```
axios.post('/posts', data).
 then(response => {
  console.log(response);
 })
```

**Application: **

* Comes in handy when passing authorization token 

**Example: **

```
axios.defaults.headers.common['Authorization'] = 'Auth Token';
```

### Instance Config:

We can create an instance like: 

```
import axios form 'axios';

const instance = axios.create({
   baseUrl: 'base_url';
});

instance.defaults.headers.common['Authorization'] = 'AUTH TOKEN';

export default instance; 
```

We can then use this instance like: 

```
import axios from '../instance file'
```

We can have multiple instances in one project with their own auth tokens. 

