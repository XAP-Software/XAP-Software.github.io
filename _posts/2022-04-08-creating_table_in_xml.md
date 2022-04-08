---
layout: post
title: How to create table in view.xml file in SAP UI5?
categories: [SAP UI5 & Open UI5]
author_github: negativename
author: Vladislav Kobenko
--- 

In case you need to create table in `*.view.xml` file in SAP UI5, you need to do few steps below.

## In *.view.xml
First of all we create table using `<Table>` tag in `*.view.xml` file. You can also add additional parameters, like `width`, `mode` and etc..

```xml
    <Table 
        id="Table" 
        items="{
                path: '/exampleODataService'
            }"
        >

        ...

    </Table>
```

Now we need to fill the front-end part of our table.
```xml
    <columns>
        <Column xmlns:mvc="sap.ui.core.mvc" xmlns:semantic="sap.f.semantic" xmlns="sap.m" id="exampleColumn1" hAlign=>
            <header>
                <Text xmlns="sap.m" text="ColumnName1" id="columnTitle1"/>
            </header>
        </Column>

        <Column xmlns:mvc="sap.ui.core.mvc" xmlns:semantic="sap.f.semantic" xmlns="sap.m" id="exampleColumn2">
            <header>
                <Text xmlns="sap.m" text="ColumnName2" id="columnTitle2"/>
            </header>
        </Column>
    </columns>
```
Here you can also add additional parameters like `align` of your text in `<Column>` tag.

And in the end we need to bind our `items` to existing OData service.
```xml
    <items>
        <ColumnListItem>
            <cells>
                <ObjectIdentifier title="{exampleID}" id="identifier1"/>
                <ObjectAttribute text="{exampleAttribute}" id="attribute1"/>
            </cells>
        </ColumnListItem>
    </items>
```
To bind `key` fields you can use `<ObjectIdentifier>` tag and for other fields you need to use `ObjectAttribute` tag.
