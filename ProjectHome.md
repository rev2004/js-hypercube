JsHypercube is a light-weight OLAP database written in JavaScript.  It is useful for any application involving the aggregation of metrics for purposes of dynamic charting.  Datasets can be "sliced and diced" in real-time, with low-latency.   Plot this on a graph, and amaze your boss.

Here's an example of it in action:

```
var data = [
	{"time":1331773202,"facts":{"name":"Super Mario Bros. 2","platform":"Nintendo","staring":"Mario"},"measures":{"rentals":73,"sales":9,"revenue":359.91}},
{"time":1331841602,"facts":{"name":"Metroid","platform":"Nintendo","staring":"Samus"},"measures":{"rentals":43,"sales":6,"revenue":239.94}}
 // ... etc
];

// each fact record in the data-set has a unix-time. add standardized local-time facts
ps.Cube.transforms.dateLocal(data);

// turn our fact records into a cube
var cube	= ps.Cube.deserialize(data, ['rentals', 'sales', 'revenue'])

// run some interesting queries
console.info('Total Rentals', cube.sum().rentals);
console.info('Revenue at 6pm for Super Nintendo games', '$' + cube.slice({hour: 18, platform: 'Super Nintendo'}).sum(2).revenue);
console.info('Avg rentals per hour for games staring Mario', cube.slice({staring: 'Mario'}).avg(24, 2).rentals + ' units');

```

There are four major parts to this library:

  * ps - the Prescreen namespace and small base library

  * ps.Cube - maintains a set of cells, an index, query methods, and aggregation methods

  * ps.Cell - a discrete record in the database, maintains a "fact set", a "measure set" and a unix-time code.

  * ps.FactIndex - internal, and can be ignored. Its an indexing mechanism used by the query engine to drastically improve performance

  * ps.Cube.transforms - filters hyper.Cell instances unix-time code


This library requires jQuery, but you could easily replace jQuery if you create $.each, $.map, $.extend, $.isArray, $.isPlainObject, $.isFunction.  This will likely be done in a future release.  If you would like to take this project on, please contact me.

**Highly Recommended:** encourage users to use Google Chrome. IE can be slow for large datasets.

I'd like to dedicate this library to Ralph Kimball.  This book is an excellent read:
http://www.amazon.com/The-Data-Warehouse-Toolkit-Dimensional/dp/0471200247