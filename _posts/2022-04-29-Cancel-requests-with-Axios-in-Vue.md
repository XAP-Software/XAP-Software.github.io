---
layout: post
title: How to cancel a request with Axios in Vue.js? 
categories: [Vue.js, Axios]
author_github: Kostya2702
author: Konstantin Loshkarev
---

You need to cancel a request using Axios in Vue.js. For example, the request takes a long time and you need to make another request. If it sounds familiar, this article is for you.

## Implementation of the cancellation of the previous request

```js
// Variable declaration globally
let controller;

export default {
  components: { 
    DatePicker,
  },
  data() {
    return {
      data: null,
      loading: false,
    };
  },
  methods: {
    search: function() {

      // Creating a new instance of the  AbortController class
      controller = new AbortController();

      // Сondition to cancel the request at loading time. On the first pass or after canceling the download, the condition will not be satisfied
      if (this.loading){
        controller.abort();

        // Toggle flag to stop download
        this.loading = false;
      }

      // Download start
      this.loading = true;

      axios.get(getDataFunc('someURL'), {
        // Getting a reference to the associated AbortSignal object
        signal: controller.signal,
      })
      .then(response => {
        this.data = [];
        // Your code for to get data

        // After getting data toggle the flag so that there is no interruption while receiving new data
        this.loading = false;
      }
      .catch(error => {
          return alert(error);
      });
    }
  }
}

```

## Implementation of the button in the template

```html
<template>
    <div id="app">
        <div style="display: inline-block; margin:10px">
            <b-button variant="outline-warning" size="sm" @click='search'>
                <b-spinner v-if="loading" small type="grow"></b-spinner>
                Create request
            </b-button>
        </div>
    </div>
</template>
```

