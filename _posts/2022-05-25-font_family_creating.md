---
layout: post
title: How to create font-family with custom font?
categories: [CSS]
author_github: negativename
author: Vladislav Kobenko
---

If you need to create a `font-family` using custom fonts from the Internet, this article describes two different ways which can help you.

## Downloading fonts from the Internet

Using first method, you need to download custom fonts from the Internet. Then create a new `.css` file, for example `fonts.woff.css`:

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

Some fonts are described by a link to the Internet. In this method, we show you how to use `link` and create `font-family`.

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

Thus, we united three different types of fonts and put them into one `font-family`. The last step is adding this `font-family` into your main `.css` file.
