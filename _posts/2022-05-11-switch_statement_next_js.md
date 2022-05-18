---
layout: post
title: How to create switch statement inside rendering method in Next.js?
categories: [Next.js]
author_github: negativename
author: Vladislav Kobenko
---

In case you want to create a switch statement in rendering method in `Next.js` similar to the one below you can use on of the methods described in this post.

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

## Creating self-invoking function

The first method is to use a self-invoking function:

```jsx
return (
    <div>
        <div>// ...some info...</div>

        {(() => {
            switch (param) {
                case "A":
                    return <p>Some info</p>;
                case "B":
                    return <span>Another info</span>;
            }
        })()}

        <div>// ...some info...</div>
    </div>
);
```

Now your `switch` statement will invoke the `JS` expression.

## Creating method/component outside of rendering method

Another way is to create a method or component with the `switch` statement outside of the rendering method. For example:

```jsx
// write this code outside of rendering method

const test = (param) => {
    switch (param) {
        case "A":
            return <p>Some info</p>;
        case "B":
            return <span>Another info</span>;
    }
};
```

Then in the rendering method you can invoke your `test` method:

```jsx
return (
    <div>
        <div>// ...some info...</div>

        {test(param)}

        <div>// ...some info...</div>
    </div>
);
```

It would be nice if we could make our code a bit more concise.
