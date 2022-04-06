# How to get date and time in datetime format string JS from DateTimePicker SAP UI5? 

In case you need to get date and time in datetime format string JS from DateTimePicker of SAP UI5.

## In SAP UI5 Controller.js
First of all you need to add Date time Formatter variable in your controller file.

``` JS
    var oDateTimeFormat = sap.ui.core.format.DateFormat.getDateTimeInstance({
                    pattern: "d MMM yyyy, Hms",
                    strictParsing: true
                });
```

After that you need to parse your value, that you are getting from sap.m.DateTimePicker to a JS string.
``` JS
    oDateTimeFormat.parse(dateValue).toISOString();
```

Now you can use this string to send Filter to your OData service. For example:
``` JS
    oFilter.push(new Filter("DateTime", FilterOperator.GE, new Date(oDateTimeFormat.parse(dateValue)).toISOString()));
```

## You might also be interested in

- [some link](/link)
