---
layout: post
title: How to implement Axios interceptors? 
categories: [JavaScript, Axios]
author_github: Kostya2702
author: Konstantin Loshkarev
---

As the application scales up, the need for error handling increases. If you use axios to get data from api, then overflowing .catch is wrong, axios.interceptors should be used to handle errors.

## Import required exports

In `main.js`

```js
import App from './App';
import axios from "axios";
```

## Implement axios interceptors

in `main.js`

```js
axios.interceptors.response.use(
  response => { 
    return Promise.resolve(response)
  }, 
  error => {
    if (error.response.status) {
      switch (error.response.status) {
        case 403:
          router.replace({
            path: "/login",
            query: { redirect: router.currentRoute.fullPath }
          });
          break;

        case 501:
          alert('Select date period')
          break;
      
        case 502:
          alert('There are no orders for the selected date');
          break;

        case 504:
          alert(Promise.reject(error.response.data));
          break;
      }
      return Promise.reject(error.response);
    }
});
```

A response is returned only if API call ends with OK codes 200 or 201.
Otherwise, we handle each error as we need.