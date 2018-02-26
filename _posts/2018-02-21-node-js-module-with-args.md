---
layout: post
title: Node.js Module with Arguments
---

```
// module.js
module.exports = function(configuration) {
     
     var module = {};
     
     module.foo = function foo() {
 
         console.log(configuration);
 
     }
 
     var configuration;
 
     function bar(aConfiguration) {
         configuration = aConfiguration;
     }
 
     bar(configuration);
 
     return module;
 
}

// main.js
var configuration = {
     a: 1,
     b: 'a string value',
     c: {
         d: 1,
         e: {
             f: 'another string value'
         }
     }
 };
 
 var module = require('./module.js')(configuration);
 
 module.foo();
```
