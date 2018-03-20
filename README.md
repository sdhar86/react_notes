# Installing Gitbook

```
npm i -g gitbook-cli
gitbook install
gitbook serve
```

# React Notes

Know before hand:

* This and bind 
* Constructors
* Classes in JavaScript
* Object.assign 
* Spread Operators Es6
* Fat arrow ES6
* Async and Await
* Promises
* Immutability
* Higher Order Functions

# Thunks

---

A [thunk](https://en.wikipedia.org/wiki/Thunk) is a function that wraps an expression to delay its evaluation.

```js
// calculation of 1 + 2 is immediate
// x === 3
let x = 1 + 2;

// calculation of 1 + 2 is delayed
// foo can be called later to perform the calculation
// foo is a thunk!
let foo = () => 1 + 2;
```

* generators
* Fetch
  * [https://scotch.io/tutorials/how-to-use-the-javascript-fetch-api-to-get-data](https://scotch.io/tutorials/how-to-use-the-javascript-fetch-api-to-get-data)
* Closure

# Currying

---

Currying is a process to reduce functions of more than one argument to functions of one argument with the help of lambda calculus.

```
f(n, m) --> f'(n)(m)
```

**Example**:

```js
multiply = (n, m) => (n * m)
multiply(3, 4) === 12 // true

curryedMultiply = (n) => ( (m) => multiply(n, m) )
triple = curryedMultiply(3)
triple(4) === 12 // true
```

### Named and Default Exports 

[https://til.hashrocket.com/posts/xrtndhi1hi-default-and-named-exports-from-the-same-module](https://til.hashrocket.com/posts/xrtndhi1hi-default-and-named-exports-from-the-same-module)



