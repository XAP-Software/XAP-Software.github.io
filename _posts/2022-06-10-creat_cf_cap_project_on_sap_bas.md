---
layout: post
title: How to create cf cap project on SAP Business Application Studio?
categories: [SAP CAP & SAP Business Application Studio]
author_github: nevermore17
author: Verbin Kirill
---

In this blog, I’ll explain how to create a cap project in SAP Business Application Studio.

## Create Development space

Open SAP Business Application Studio and create new Dev Space.

![Step 1](../article_images/create_cf_cap_project_on_sap_bas/1.png)

Enter a name for the workspace and select the type `Full Stack Cloud Application`.

![Step 2](../article_images/create_cf_cap_project_on_sap_bas/2.png)

Now we create our dev space and we can start creating our application.

![Step 3](../article_images/create_cf_cap_project_on_sap_bas/3.png)

## Create simple CAP Project

Let's create a project from the template.

![Step 4](../article_images/create_cf_cap_project_on_sap_bas/4.png)

Select the CAP project.

![Step 5](../article_images/create_cf_cap_project_on_sap_bas/5.png)

We will deploy the application on SAP BTP and HANA Cloud so we need to select the appropriate settings.

![Step 6](../article_images/create_cf_cap_project_on_sap_bas/6.png)

We need to create files `service.cds` and `service.js` in folder `srv/` and file `schema.cds` with simple code in folder `db/` for CAP project.

![File structure](../article_images/create_cf_cap_project_on_sap_bas/files.png)

```js
// db/schema.cds

using {cuid} from '@sap/cds/common';

namespace db;

entity Foo : cuid {
    title: String default 'Hellow world';
    num: Integer default 0;
}
```

```js
// srv/service.cds

using { db as db } from '../db/schema';

service MyService {

    entity Foo as projection on db.Foo;

}
```

## Creating Fiori elements application

Select Create MTA Module from Template.

![Step 7](../article_images/create_cf_cap_project_on_sap_bas/7-10.png)

Select 'Create Approuter'.

![Step 8](../article_images/create_cf_cap_project_on_sap_bas/8.png)

Enter the name and agree that you are planning to add UI.

![Step 9](../article_images/create_cf_cap_project_on_sap_bas/9.png)

Select 'Create MTA Module from Template'.

![Step 10](../article_images/create_cf_cap_project_on_sap_bas/7-10.png)

Select `Create SAP Fiori application`.

![Step 11](../article_images/create_cf_cap_project_on_sap_bas/11.png)

In this step we will create front-end application for our Full Stack Application. We can create SAP UI5 application or Fiori application from the template. For example, we choose Fiori elements List report.

![Step 12](../article_images/create_cf_cap_project_on_sap_bas/12.png)

Let's choose Local CAP project, CAP project location and our service which we will use for our front end application.

![Step 13](../article_images/create_cf_cap_project_on_sap_bas/13.png)

Select `Main entity`.

![Step 14](../article_images/create_cf_cap_project_on_sap_bas/14.png)

Config our application and add deployment configuration and FLP configuration.

![Step 15](../article_images/create_cf_cap_project_on_sap_bas/15.png)

Select `Local CAP Project API`.

![Step 16](../article_images/create_cf_cap_project_on_sap_bas/16.png)

Configurate FLP.

![Step 17](../article_images/create_cf_cap_project_on_sap_bas/17.png)

## Deploy CAP project to SAP BTP.

Login to our API endpoint using `cf`.

![Step 18](../article_images/create_cf_cap_project_on_sap_bas/20.png)

Build our project to MTA archive.

![Step 18](../article_images/create_cf_cap_project_on_sap_bas/18.png)

Deploy MTA archive.

![Step 18](../article_images/create_cf_cap_project_on_sap_bas/21.png)
