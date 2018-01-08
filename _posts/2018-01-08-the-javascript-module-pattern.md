---
layout: post
title: The JavaScript Module Pattern
---

## Motivation

The **Module** design pattern can be used to *emulate* the concept of *classes* in JavaScript in a way that can provide 
*private*/*public* access to functions and variables using a single object, preventing parts of an implementation to be accessed from
the global scope.

The pattern also provides a way to achieve the principle of *Single Responsibility* within an application, thus improving 
overall maintainability and making code more robust.

## Basics

The **Module** pattern relies on the concepts of *Immediatly Invoked Function Expression* and *Closure*.

* An *Immediatly Invoked Function Expression* (*IIFE*) is a JavaScript function that runs as soon as it is defined.
* A *Closure* is a function having access to the parent scope, even after the parent function has closed.

An example of *Immediatly Invoked Function Expression* is the following:

```javascript
(function() {

  // all variables and functions declared here are local to
  // function's scope, but still maintan access to globals

})();
```

The function defines a local scope which can define variables and functions that can operate on such variables, a *closure*, for
example:

```javascript
var Adder = (function() {

  // defines 'private' variable
  var counter = 0;
  
  // defines and returns a function that can access the
  // counter variable defined in the IIFE scope, which is
  // the parent scope of the returned function
  return function () {
    counter += 1;
  }

})();
```

The previous code defines an *Immediatly Invoked Function Expression* which declares a variable `counter` and returns a function
(which gets assigned to the `Adder` variable) that operates on `counter` by increasing its value by 1 every time it get called.

It's important to note that `counter` is not accessible from outside the scope of the function returned from the *IIFE*, thus emulating
*private* access to `counter`.
On the other hand, we can access the function returned as the result of the *IIFE*, thus emulating a *public* access to the function.

We are not limited to functions when returning from an *IIFE*, we can return for example an object which references the function we
want to have *public* access:

```javascript
var Adder = (function() {

  // defines 'private' variable
  var counter = 0;
  
  return {
  
    // defines a function that can access the
    // counter variable defined in the IIFE scope, which is
    // the parent scope of the returned function
    add: function () {
      counter += 1;
    }
  
  }
  
})();
```

By using an object we can add as many functions as we need in the public API:

```javascript
var Counter = (function() {

  // defines a 'private' variable to hold the counter value
  var counter = 0;
  
  return {
  
    add: function () {
      counter += 1;
    },
    
    sub: function () {
      counter -= 1;
    },
    
    get: function () {
      return counter;
    }
  
  }
  
})();
```

From outside the *IIFE* scope we can use the API as follows:

```javascript

// ...include the above definition of the Counter Module...

Counter.get(); // will return 0
Counter.add(); // counter = 1
Counter.add(); // counter = 2
Counter.add(); // counter = 3
Counter.get(); // will return 3
Counter.sub(); // counter = 2
Counter.sub(); // counter = 1
Counter.get(); // will return 1

```

Wrapping up: everything defined whithing the *IIFE* scope that is not returned will get a *private* access emulation, while what gets 
returned as the result of the *IIFE* will get *public* access emulation and will have access to private variables and functions.

## A Working Example

The following code demonstrates the above concepts in a full working example with *private* and *public* methods and variables:

```javascript
var myNamespace = (function () {
 
  var myPrivateVar;
  var myPrivateMethod;
 
  // A private counter variable
  myPrivateVar = 0;
 
  // A private function which logs any arguments
  myPrivateMethod = function( foo ) {
      console.log( foo );
  };
 
  // The public API of this module
  return {
 
    // A public variable
    myPublicVar: "foo",
 
    // A public function utilizing privates
    myPublicFunction: function( bar ) {
 
      // Increment our private counter
      myPrivateVar++;
 
      // Call our private method using bar
      myPrivateMethod( bar );
 
    }
  };
 
})();
```

## Further Readings

* <https://addyosmani.com/resources/essentialjsdesignpatterns/book/>
* <http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html>
