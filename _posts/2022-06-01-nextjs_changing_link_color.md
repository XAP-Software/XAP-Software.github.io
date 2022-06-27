---
layout: post
title: How to change link color by hover in Next.js/React.js?
categories: [Next.js, React.js]
author_github: negativename
author: Vladislav Kobenko
---

If you want to change link color by hover using `Next.js` or `React.js`, this article will help you a lot.

## Creating a hover handler

First of all you need to import `useState` hook from `React.js` library. Then create a state for status of hover. You need to set the default state as not hovered.

```jsx
import { useState } from "react";

const [isHover, setHover] = useState(false);
```

Let's create a hover handler. The state will change to opposite when the link was hovered.

```jsx
const toggleHover = () => {
    setHover(!isHoverBlog);
};
```

## Changing style of a link dynamically

Now let's move on to the main part. Here you need to create a flag for changing the color of the link.

```jsx
return (
    <p>Some info</p>

    {(() => {
        let linkStyle;
        if (isHover)
            linkStyle = { color: "#999" };
        else linkStyle = { color: "#0ad1d8" };
        return (
                <a
                    style={linkStyle}
                    onMouseEnter={toggleHover}
                    onMouseLeave={toggleHover}
                >
                    Test link...
                </a>
        );
    })()}

    <p>Some other info</p>
);
```

Using the property `onMouseEnter` we call for a hover handler. This handler changes `isHover` state and the link gets a new color. We call a hover handler one more time in `onMouseLeave`. That's why link changes color back. As you saw how to use inline styles, you can change the link color yourself.

That would be nice if you could make your code a little bit more expressive.
