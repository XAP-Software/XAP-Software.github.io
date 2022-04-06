# How to expand/collapse elements of treegrid table from Syncfusion library in Vue.js? 

In case you need to expand/collapse the hierarchy elements of treegrid table in Vue.js using built-in methods from Syncfusion. In this article we show you how to expand/collapse all elements or elements on specific hierarchy level.

## expandAll()

Function expandAll() allows to expand all the collapsed nodes on every hierarchy level.

``` HTML
<ejs-treegrid ref="treegrid" :dataSource="data"></ejs-treegrid>
```
``` JavaScript
this.$refs.treegrid.expandAll()
```     
        
You can also expand the specific nodes by passing the array of collapsed nodes as argument to this method.

``` HTML
<ejs-treegrid ref="treegrid" :dataSource="data"></ejs-treegrid>
```
``` JavaScript
var node = this.$refs.treegrid.getNode(args.node);
console.log(node);
this.$refs.treegrid.expandAll([node.id]);
```

## collapseAll()

Function collapseAll() allows to collapse all the expanded nodes on every hierarchy level.

``` HTML
<ejs-treegrid ref="treegrid" :dataSource="data"></ejs-treegrid>
```
``` JavaScript
this.$refs.treegrid.collapseAll()
```     

You can also collapse the specific nodes by passing the array of expanded nodes as argument to this method.

``` HTML
<ejs-treegrid ref="treegrid" :dataSource="data" ></ejs-treegrid>
```
    
``` JavaScript
var node = this.$refs.treegrid.getNode(args.node);
console.log(node);
this.$refs.treegrid.collapseAll([node.id]);
```

## expandAtLevel()

Function expandAtLevel() allows to expand all the collapsed nodes on specific hierarchy level by passing the number of level as argument to this method.

``` HTML
<ejs-treegrid ref="treegrid" :dataSource="data" ></ejs-treegrid>
```
    
``` JavaScript
this.$refs.treegrid.expandAtLevel(1);
```

## collapseAtLevel()

Function collapseAtLevel() allows to collapse all the expanded nodes on specific hierarchy level by passing the number of level as argument to this method.

``` HTML
<ejs-treegrid ref="treegrid" :dataSource="data" ></ejs-treegrid>
```
    
``` JavaScript
this.$refs.treegrid.collapseAtLevel(1);
```



## You might also be interested in

- [some link](/link)
