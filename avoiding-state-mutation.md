# Avoiding State Mutations

---

It is important that in Redux, we do not mutate states, we instead make a new copy of the state in reducers and make changes to the copy and return that copu in the reducers. 

### Dealing with Arrays: 

```js
// Adding to an array 

const addCounter = (list) => {
  // return list.concat([0]); // old way
  return [...list, 0]; // ES6 way
};

// removing from an array 

const removeCounter = (list, index) => {
  // Old way:
  //return list
  //  .slice(0, index)
  //  .concat(list.slice(index + 1));

  // ES6 way:
  return [
    ...list.slice(0, index),
    ...list.slice(index + 1)
  ];
};

// Updating an array 

const incrementCounter = (list, index) => {
  // Old way:
  // return list
  //  .slice(0, index)
  //  .concat([list[index] + 1])
  //  .concat(list.slice(index + 1));

  // ES6 way:
  return [
    ...list.slice(0, index),
    list[index] + 1,
    ...list.slice(index + 1)
  ];
};
```

### Dealing with Objects: 

```js
// changing one key of the object using Object.assign()

const toggleTodo = (todo) => {
  return Object.assign({}, todo, {
    completed: !todo.completed
  });
};


// using the spread operator

const toggleTodo = (todo) => {
  return {
    ...todo,
      completed: !todo.completed
  };
};

```



