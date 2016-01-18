# usql.js
Microsoft U-SQL in js (node.js or browser)

# Roadmap

## I think U-SQL in js like...

### in-browser u-sql usage

```usql
//script.usqljs
// 1. Extract
@tab = EXTRACT webpage, user, duration //JS object members
       FROM "/path/to/ajax.json" // using Ajax
       USING JSON.parse; //JS functions

// 2. Process
@tab =
    SELECT webpage.substring(0, 5) AS tag, // using JS String.prototype.substring() Method
     webpage.split('/').filter(p => p == "admin").length > 0 AS admin, //using ECMAScript 6 lambda (or you can anonmyous function.)
     GetFirstPath(webpage) AS first //using JS UDF            
    FROM @tab;

@result =
    SELECT tag,
           COUNT( * ) AS count // same as @tab.length ...
    FROM @tab
         WHERE admin == false // JS Expression
    GROUP BY tag;
            
// 3. Output            
OUTPUT @result
TO "#mytable" // DOM Selector (absolute on document, replace child is default. to append, use APPEND keyword.)
USING JSON2Table;  //JS DOM Parse Callback
```

### node.js u-sql usage

```usql
//script.usqljs
// 1. Extract
@tab = EXTRACT webpage, user, duration //JS object members
       FROM "/path/to/data.csv" // using Ajax
       USING require('some-csv-parser').fromCSV; //CSV to JSON

// 2. Process
@tab =
    SELECT webpage.substring(0, 5) AS tag, // using JS String.prototype.substring() Method
     webpage.split('/').filter(p => p == "admin").length > 0 AS admin, //using ECMAScript 6 lambda (or you can anonmyous function.)
     GetFirstPath(webpage) AS first //using JS UDF            
    FROM @tab;

@result =
    SELECT tag,
           COUNT( * ) AS count // same as @tab.length ...
    FROM @tab
         WHERE admin == false // JS Expression
    GROUP BY tag;
            
// 3. Output            
OUTPUT @result
TO "/path/to/mydata.csv" // save to file
USING require('some-csv-parser').toCSV;  //JSON to CSV
```

## in browser script like

```html
<script id="getSomething" type="text/usql">
//... your U-SQL here or code over there.
</script>
<script type="text/javascript">
USQL.id('getSomething').execute(callback);//inline script execute
USQL(`
//... your U-SQL String here.
`).execute(callback); // returns usql object. but not implemented.
USQL.file('/path/to/ajax.usql'/*, 'callbackname' for JSONP*/); //plain async. "thanable" usql object will be returned.
var uobj = await USQL.fileAsync('/path/to/ajax.usql'/*, 'callbackname' for JSONP*/); //ECMA7 async
var result = await uobj.executeAsync(); //async execute
</script>
```

## in node.js like

```js
var usql = require('usql');
usql(`
//... your U-SQL String here.
`).execute(); // returns usql object. but not implemented.
usql.file('/path/to/my.usql').execute();//plain sync
var uobj = await usql.fileAsync('/path/to/my.usql'); //async file load
var result = await uobj.executeAsync(); //async execute
```

## or your ideas welcome.

# Target

- in-browser use
- node.js

# Prepare dependency with

## in-browser

- ECMA 5 or newer
- indexedDB
- Don't care about old browser.

## node.js

- ECMA 5 or newer
- ...

# TODO

- Make with standard from U-SQL specification.
- work with browser and node.js
- need your help 

#License

Apache License 2.0
