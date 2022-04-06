---
layout: post
title: What the difference between tx.run(CRUD Operation) and simple CRUD Operation?
categories: [CAP CDS]
---

Transaction management in CAP deals with (ACID) database transactions, principle / context propagation on service-to-service calls and tenant isolation.

## What is a transaction?

In the context of databases and data storage systems, a transaction is any operation that is treated as a single unit of work, which either completes fully or does not complete at all, and leaves the storage system in a consistent state.

## ACID

ACID (Atomicity, Consistency, Isolation, Durability) is a set of properties of a database transaction with an intention to guarantee validity of data.
ACID transactions ensure the highest possible data reliability and integrity. They ensure that your data never falls into an inconsistent state because of an operation that only partially completes.


## Root Transactions

Whenever an instance of cds.Service processes inbound messages or requests, for example in response to a call like that:
```js
await db.read('Books')
``` 
… the core framework automatically cares for…

 - Principle propagation and tenant isolation
 - Starting database transactions if applicable
 - Finally calling commit or rollback
 - Acquiring and returning physical connections from/to connection pools

## Manual Transactions

You can use `JavaScript srv.tx()` as documented below to start and commit transactions manually.

## srv.tx (context?, fn?) → tx<srv>

Use `JavaScript cds.tx()` to start new app-controlled transactions manually, most commonly for database services as in this example:
```js
let db = await cds.connect.to('db')
let tx = db.tx()
try {
  await tx.run (SELECT.from(Foo))
  await tx.create (Foo, {...})
  await tx.read (Foo)
  await tx.commit()
} catch(e) {
  await tx.rollback(e)
}
```

Arguments:
 - context – an optional context object
 - fn – an optional function to run

## You might also be interested in

- [Transaction Management - capair](https://cap.cloud.sap/docs/node.js/transactions#automatic-transactions)
- [ACID - wikipedia](https://en.wikipedia.org/wiki/ACID)
- [What is ACID Transactions - databricks](https://databricks.com/glossary/acid-transactions#:~:text=ACID%20is%20an%20acronym%20that,operations%20are%20called%20transactional%20systems.)
- [Significance of ACID vs BASE vs CAP Philosophy in Data Science - analytics vidhya](https://medium.com/analytics-vidhya/significance-of-acid-vs-base-vs-cap-philosophy-in-data-science-2cd1f78200ce)
