---
layout: post
title: How to add sitemap and robots.txt to your NextJS project?
categories: [NextJS, TS]
author_github: nevermore17
author: Verbin Kirill
---

Use `next-sitemap` npm module for generation a sitemap to your NextJS project.
This allows generate the sitemap for your project with server-side props based pages.

### Install module

```bash
npm install next-sitemap --dev
```

### Configurate package.json file

Add `postbuild` config to `scripts`

```json
...
"scripts": {
    "dev": "next dev",
    "build": "next build",
    "postbuild": "next-sitemap",
    "start": "next start",
    "export": "next export",
    "lint": "next lint"
  },
...
```

### Create configuration files

You need to create `next-sitemap.js` in your project folder

```js
//  ./next-sitemap.js

module.exports = {
  siteUrl: process.env.NEXT_PUBLIC_BACKENDSERVER || 'https://example.com', // url from your env file
    generateRobotsTxt: true, // create robots.txt file
    exclude: ['/server-sitemap-index.xml'], // sitemap file for side props based pages
  robotsTxtOptions: {
    additionalSitemaps: [
      `${process.env.NEXT_PUBLIC_BACKENDSERVER}/server-sitemap-index.xml`, // sitemap url for side props based pages
  robotsTxtOptions: {
    ],
  },
};
```

### Create dynamic pages sitemap generation

Create `./pages/server-sitemap-index.xml/index.tsx`

```tsx
import { getServerSideSitemap, ISitemapField} from 'next-sitemap'
import { GetServerSideProps } from 'next'

export const getServerSideProps: GetServerSideProps = async (ctx) => {
     var urls: ISitemapField[] = []

     const be_urls = fetch( `some user\`s req to be` ) // get page data from BE

     be_urls.map(url => {
        urls.push({loc: url, lastmod: "lastmod" ,changefreq: "changefreq", priority: "priority" })
     })

```
