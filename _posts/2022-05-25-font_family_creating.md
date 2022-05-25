---
layout: post
title: How to create font-family with custom font?
categories: [CSS]
author_github: negativename
author: Vladislav Kobenko
---

If you need to create `font-family` using custom fonts from the Internet, in this article described two different ways, which can help you.

## Downloading fonts from the Internet

In the first method you need to download custom fonts from the Internet. Then create a new `.css` file, for example `fonts.woff.css`:

```css
@font-face {
    font-family: "KievitWeb";
    font-weight: 700;
    font-style: normal;
    src: local("KievitWebBold"), local("KievitWebBold"),
        url(data:application/font-woff;charset=utf-8;base64) format("woff"), url("../fonts/KievitWebBold.woff")
            format("truetype");
}
@font-face {
    font-family: "KievitWeb";
    font-weight: 400;
    font-style: normal;
    src: local("KievitWeb"), local("KievitWeb"),
        url(data:application/font-woff;charset=utf-8;base64) format("woff"), url("../fonts/KievitWeb.woff")
            format("truetype");
}
```

As you see, we have two different fonts: normal and bold, and we united two different fonts by one family. Now you can add this z in your main `.css` file:

```css
body {
    font-family: "KievitWeb", sans-serif;
```

## Using link on the font

Some fonts you are described by the link in the Internet. In this method we show you how to use `link` and create `font-family`.

```css
@font-face {
    font-family: "Open Sans";
    font-weight: 400;
    font-style: normal;
    src: url("https://fonts.googleapis.com/css2?family=Open+Sans&display=swap");
}
@font-face {
    font-family: "Open Sans";
    font-weight: 400;
    font-style: italic;
    src: url("https://fonts.googleapis.com/css2?family=Open+Sans:ital@1&display=swap");
}
@font-face {
    font-family: "Open Sans";
    font-weight: 700;
    font-style: normal;
    src: url("https://fonts.googleapis.com/css2?family=Open+Sans:wght@700&display=swap");
}
```

Now we united three different types of fonts to the one `font-family`. In the last step you need to add this `font-family` into your main `.css` file.
