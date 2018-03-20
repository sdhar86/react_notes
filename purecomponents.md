# PureComponent

---

[https://60devs.com/pure-component-in-react.html](https://60devs.com/pure-component-in-react.html)

Instead of extending **Component** in React, we can extend **PureComponent. **In doing so, it automatically does the change in state in the **shouldComponentUpdate\(\) **lifecycle hook and returns false if there is no state change.

When **shouldComponentUpdate\(\) **returns false, the render\(\) functions are not executed. This results in performance gains.

Note:

* Just because render\(\) function is executed, doesnt mean the real DOM is updated. React is smart enough to know that it should only touch the real DOM only if something changes.

Example:

```
import React , {PureComponent} from 'react';

class Books extents PureComponent {

// shouldComponentUpdate() is now automatically implemented


}
```



