---
layout: post
title: How to setup NextJS FrontEnd Framework with some API? 
categories: [NextJS]
author_github: nevermore17
author: Verbin Kirill
---

Let's say we have some BackEnd server (in my case, Django + Django Rest Framework) and we want to generate `html`/`css` on NextJS

## 1. Create NextJS app
 ``` 
    npx create-next-app myapp
 ```
NextJS application uses a folder structure, such as:
```
myapp
|-- pages
|   |-- api
|   |   |-- hello.js
|   |
|   |-- _app.js
|   |-- index.js
|
|-- public
|   |-- favicon.ico
|  
|-- styles
|   |-- global.css
|   |-- Home.module.css
|
|--package.json

```
On this step I recommend to add to myapp folder two files named `.env.development` and `.env.production`. Depending on the start mode (dev or prod), NextJs adds values from `.end.<start_mode>` files to `process.en`v. This will make development and deployment easier.
``` env
# .env.development / .env.production

NEXT_PUBLIC_BACKENDSERVER=http://127.0.0.1
NEXT_PUBLIC_BACKENDPORT=8000
```
## 2. Create page articles

The paths for the application are made by using folder management in pages folder:
- pages/index.js - `/`
- page/some_page - `/some_page`
- page/some_page/some_page2.js - `/some_page/some_page2`
- page/[param].js - `/<param>`
- page/[param]/[param2] - `/<param>/<param2>`

We have to create `/articles` way. 
```js
// pages/articles.js

export default function Index() {
    return(
        <>
            Hello World!
        </>
    )
}
```
And run the app. Now we have "Hello World" on route `/articles` but we need to get articles from BackEnd server

In myapp BackEnd server we have two routes:
- `http://127.0.0.1:8000/api/article/`
- `http://127.0.0.1:8000/api/article/<int:pk>`

The page `/article/` only lists links to the article pages. 
The function `getServerSideProps` without params request articles from BackEnd server and return like page_props

```js
// pages/articles.js

export default function Home({articles}) {
    
    return (
        <>
            <h1>Articles!</h1>
            {articles.map((obj) => { return (<>
                <a href={`article/${obj.id}`}>{obj.title}</a><br/>
            </>)})}
        </>
    )

}

export async function getServerSideProps(){
    
    const res = await fetch(`${process.env.NEXT_PUBLIC_BACKENDSERVER}:${process.env.NEXT_PUBLIC_BACKENDPORT}/api/article/`)
    const res_json = await res.json()
    return { props: { articles: res_json.results } }
}
```
## 3. Create articles/\<id\> page

Create a folder articles and add to this folder [id].js

We have `/articles/<id>` way:

```js
// pages/articles/[id].js

export default function Index({article}) {

    return (
        <>
            <h1>Article: {article.title}</h1>
            <p>
                {article.text}
            </p>
            <p>
                {article.pub_date}
            </p>
            <a href='/article'>Back</a>
        </>
    )

}

export async function getServerSideProps({params}){
    
    const res = await fetch(`${process.env.NEXT_PUBLIC_BACKENDSERVER}:${process.env.NEXT_PUBLIC_BACKENDPORT}/api/article/${params.pk}`)
    console.log(`${process.env.NEXT_PUBLIC_BACKENDSERVER}:${process.env.NEXT_PUBLIC_BACKENDPORT}/api/article/${params.pk}`)
    const res_json = await res.json()
    return { props: { article: res_json } }
}
```

By passing `{params}` to `getServerSideProps`, we mean that a request should be made to receive data from the article





















