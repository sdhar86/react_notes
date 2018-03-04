# Axios

---

[https://github.com/axios/axios](https://github.com/axios/axios)

```
 npm install axios
```

**Example to get Data : **

```
// Make a request for a user with a given ID
axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

Similar calls can be made to post, put  or delete

Play ground to play with JSPN API calls : [https://jsonplaceholder.typicode.com/](https://jsonplaceholder.typicode.com/)

### **API calls in React**

A good place to get Side-Effects \(call api\) is **componentDidMount\(\) **lifecycle hook in a Component.

**Example: **

```
componentDidMount () { 
  axios.get('url'). 
   then(response = > {
     this.setState({state: response.data});
   });
 }
```

There are other places where data can be called too! 

**Warning: **

Fetching Data on **ComponentDidUpdate\(\) **can cause **infinite loops**. So , we need to write a guard for that. 

**Example: **

```
componentDidUpdate () {
    if we did not already fetch data for id : id {
        axios.get('url').
         then(res => {
             this.setState({state = response.data});
         })
    }
}
```



### Handling Errors

**Example: **

```
componentDidMount () { 
  axios.get('url'). 
   then(response = > {
     this.setState({state: response.data});
   }).
   catch(error => {
     this.setState({error: true});
   })
 }
```









