---
layout: post
title: How to create switch statement inside rendering method in Next.js?
categories: [Next.js]
author_github: negativename
author: Vladislav Kobenko
---

In case you want to create switch statement in rendering method in `Next.js` like this:

```jsx
return (
        <div>
            <div>
                // ...some info...
            </div>

           { switch(...) {} }

            <div>
                // ...some info...
            </div>
        </div>
    );
```

You can use one of the following methods.

## Creating self-invoking function

The first method is to use self-invoking function:

```jsx
return (
        <div>
            <div>
                // ...some info...
            </div>

           {(() => {
               switch(param) {
                    case 'A':
                       return (<p>Some info</p>)
                    case 'B':
                       return (<span>Another info</span>)
               } 
           })()}

            <div>
                // ...some info...
            </div>
        </div>
    );
```

Now your `switch` statement will invoke like `JS` expression. 

## Creating method/component outside of rendering method

An another way is to create method or component with `switch` statement outside of rendering method. For example:

```jsx
// write this code outside of rendering method

const test = (param) => {
    switch (param){
        case 'A':
            return(
                <p>Some info</p>
            )
        case 'B':
            return(
                <span>Another info</span>
            ) 
    }
}
```

And in the rendering method you should to invoke your `test` method:

```jsx
return (
        <div>
            <div>
                // ...some info...
            </div>

           { test(param) }

            <div>
                // ...some info...
            </div>
        </div>
    );
```


It would be nice if we could make our code a bit more concise.