---
layout: post
title: How to deploy CAP project with SQLite DB and row data in *.csv files to SAP BTP Cloud Platform 
categories: [SAP BTP, CAP CDS]
author_github: negativename
author: Vladislav Kobenko
---

In case you need to deploy CAP project with SQLite DB and test data in `*.csv` files you need to modify 2 files.

## Modify package.json
Add in `requires` block information, that you are using sqlite in-memory database.

```json
    "requires": {
            "db": {
                "kind": "sqlite",
                "credentials": {
                    "database": ":memory:"
                }
            }
        },
```

After the `requires` block add the new block `feature` where set `in_memory_db` property as true.
```json
    "features": {
                "in_memory_db": true
            }
```

## Modify mta.yaml file
Now go to the `mta.yaml` and add the command `- cp -r db/data gen/srv/srv/data`. This command will allow you to deploy row data in `*.csv` files from your `db/` folder.
```yaml
    build-parameters:
      before-all:
      - builder: custom
        commands:
        - npm ci
        - npx -p @sap/cds-dk cds build --production
        - cp -r db/data gen/srv/srv/data
```
Now you can create mta archive and deploy it on SAP BTP.

