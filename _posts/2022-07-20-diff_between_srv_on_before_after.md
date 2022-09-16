---
layout: post
title: What is the difference between srv .on() .before() and .after() handlers in CAP CDS?
categories: [CAP CDS]
author_github: negativename
author: Vladislav Kobenko
---

## srv.before() handler

This handler is called before the srv.on() handler. It is usually used for adding user's input validation, for example:

```js
this.before (['CREATE', 'UPDATE'],'Record', (req)=>{
  const record = req.data
  if (record.spentTime <= 0)  throw 'Spent time must be positive number'
})
```

There are another four things that you need to know about `.before()` handler:

1. If you have more than one `.before()` handler, then they will be executed in order of their registration. 
2. If your handler executes asynchronous task, you need to return a promise.
3. Execution of the `.on()` handlers starts, when all promises will be resolved.
4. If your `.before()` handlers are running in asynchronous way, you can't react on side-effects.

## srv.on() handler

These handlers are run in a chain and every handler can terminate it. `.on()` handler helps you to create custom handlers replacing standard ones.

Please find below some examples for handling requests:

```js
srv.on('READ','Records', async (req)=> req.reply(await cds.tx(req).run(...))
srv.on('READ','Records', (req)=> req.reply(...))
srv.on('READ','Records', (req)=> cds.tx(req).run(...))
srv.on('READ','Records', (req)=> [ ... ])
srv.on('READ','Records', ()=> SELECT.from(Records))
srv.on('READ','Records', (req,next)=>{
  if (...) return SELECT.from(Records) 
  else return next()  
})
```

## srv.after() handler

The `.after()` handler is used to modify the response. It runs on the results, which are returned by the standard or custom `.on()` handler.

There are a few things you should know about this handler:

1. Otherwise than the `.on()` handler, the `.after()` handler has two parameters - `(result, req)`.
2. You should use `req` parameter, if you need to reflect on an inbound request
3. This method uses only for modificate the result. You can't replace the result, like this: 
```js
  this.after('READ','Records', (records)=>{
  return ...something...
})
```

That would be nice if you could make your code a little bit more expressive.
