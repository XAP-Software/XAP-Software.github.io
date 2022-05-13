---
layout: post
title: How to set up replication from PostgreSQL to SAP HANA Cloud? 
categories: [SAP BTP, SAP HANA Cloud, PostgreSQL]
author_github: Kostya2702
author: Konstantin Loshkarev
---

As the application scales up, so does the need to handle a large number of errors. if you use axios to get data from api, then overflowing .catch is wrong, axios.interceptors are used for that.

## Import required elements

In `main.js`

```js
import App from './App';
import axios from "axios";
```

## Implementing axios interceptors

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

Response is returned if api call ends with code 200 or 201.
Otherwise, we handle each error as we need.