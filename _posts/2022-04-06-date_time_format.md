---
layout: post
title: How to get date and time in datetime format string JS from DateTimePicker SAP UI5? 
categories: [SAP UI5 & Open UI5]
---

In case you need to get date and time in datetime format string JS from DateTimePicker of SAP UI5.

## In SAP UI5 Controller.js
First of all you need to add Date time Formatter variable in your controller file.

```js
    var oDateTimeFormat = sap.ui.core.format.DateFormat.getDateTimeInstance({
                    pattern: "d MMM yyyy, Hms",
                    strictParsing: true
                });
```

After that you need to parse your value, that you are getting from `sap.m.DateTimePicker` to a JS string.
```js
    oDateTimeFormat.parse(dateValue).toISOString();
```

Now you can use this string to send Filter to your OData service. For example:
```js
    oFilter.push(new Filter("DateTime", FilterOperator.GE, new Date(oDateTimeFormat.parse(dateValue)).toISOString()));
```

**Author**: [negativename](https://github.com/negativename)
