---
layout: post
title: How to set up your CAP project with PostgreSQL database?
categories: [CAP POSTGRESQL]
author_github: nevermore17
author: Kirill Verbin
---

In case you need to create CAP project with PostgreSQL database, this article will help you understand the initial setup.

## cds-pg

CAP projects cannot work natively with PostgreSQL. To connect to such database you need to install community-driven `cds-pg` and `cds-dbm`

```console
$ npm install cds-pg cds-dbm
```

Database connect in `package.json`

```json

"cds": {
        "requires": {
            "db": {
                "kind": "database"
              },
              "database": {
                "dialect": "plain",
                "impl": "cds-pg",
                "model": [
                  "srv"
                ]
              }
        },

```

Data for connecting to the database is written in `default-env.json`

```json

{
    "VCAP_SERVICES": {
      "postgres": [
        {
          "name": "postgres",
          "label": "postgres",
          "tags": ["plain", "database"],
          "credentials": {
            "host": "localhost",
            "port": 5432,
            "database": "database",
            "user": "user",
            "password": "password",
            "schema":"schema"
          }
        }
      ]
    }
  }

```

Now we should define our data model and services.

## cds-dpm 

Use `cds-dpm` to deploy database artifacts to the PostgreSQL database.

```console
$ npx cds-dpm deploy
```

