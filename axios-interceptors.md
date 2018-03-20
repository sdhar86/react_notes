# Axios Interceptors

---

### [https://github.com/axios/axios](https://github.com/axios/axios)

### Adding Interceptors

We can add **interceptors** to add functionality to apps globally.

**Example: **

```
axios.interceptors.request.use(request => {
    console.log(request);
    return request: 
})
```

**Note: **

* We must return request, or wherever we are using axios won't work properly.
* We can also use interceptors to handle errors **globally**

```
axios.interceptors.request.use(request => {
    console.log(request);
    return request: 
}, error => {
    console.log(error);
    return Promise.reject(error);
});
```

* We must return Promise.reject so , we can also handle the error locally. 

**Interceptors can also be added for response**

```
axios.interceptors.response.use(response => {
    console.log(request);
    return response: 
})
```

**To capture api errors, we must use interceptor on response. **

**Removing Interceptors**

```
var myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```



