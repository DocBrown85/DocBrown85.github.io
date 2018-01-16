---
layout: post
title: A Simple TODO Application with AngularJS
---

**Foreword**

This is a long ongoing description not yet completed. For the complete code of the TODO Application refer to the **The Full Code**
section below.

**Introduction**

AngularJS is a JavaScript Web Application framework developed at Google. It extends standard HTML with **directives** which add
functionalities and expressiveness to plain HTML.

Another important feature of AngularJS is *two-way binding*, a useful concept which is best demonstrated with the following example:

```html
<!DOCTYPE html>
<html>
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
<body>

<div ng-app="">
 
<p>Input something in the input box:</p>
<p>Name: <input type="text" ng-model="name"></p>
Name:<label ng-bind="name"></label>

</div>

</body>
</html>
```

By viewing the previous snippet in a browser we can edit the value of the input text which is automatically displayed in the label
without any additional code. The heavy lifting work is done by AngularJS, we only have to use two directives: *ng-model* and *ng-bind*.

**The TODO Application**

The TODO Application is a simple application whose purpose is to allow the user to manage a list of things to do, such as:

* Buy the milk
* Call Kyra
* Walk the dog

It is simple in its purpose, but allows to understand the basic operations nearly all applications need to do over the data they manage.
These operations go under the name of CRUD operations, an acronym for:

* Create
* Read
* Update
* Delete

Basically nearly all applications need to *create* some data, *read* it from some kind of storage, *update* it as the user need
and *delete* it when no longer useful.

This kind of application is useful to evaluate and learn the basic capabilities of a framework (be it a frontend or a backend framework),
and can serve as a benchmark to evaluate different frameworks when choosing a particular one.

In the following will be described the steps to build a really simple Todo Application with a focus on the AngularJS framework, although
we will use some other frameworks to build a better looking application.

The additional framework will be:

* *Bootstrap.js*: a frontend HTML, CSS, and JavaScript framework for developing responsive, mobile-first sites and applications,
                  see <https://getbootstrap.com/docs/3.3/getting-started/>.
* *Font Awesome Icons*: a CSS framework for fonts and icons, see <http://fontawesome.io/icons/>.

**Application Architecture**

AngularJS enforces an MVC architecture, which basically says we have to split our application in a *Model*, a *View*, and a *Controller*.

The *Model* is responsible for managing the data of the application. It responds to the request from the view and it also
responds to instructions from the controller to update itself.

The *View* means presentation of data in a particular format, triggered by a controller's decision to present the data.

The *Controller* is responsible for responding to the user input and perform interactions on the data model objects.
The controller receives the input, it validates the input and then performs the business operation that modifies the state
of the data model.

**Code Organization**

The application code is organized in the following way:

```
    application-root-folder
        |
        +---- index.html    // application main view and user's entry point
        |
        +---- app.js        // application main AngularJS module
        |
        +---- css           // folder for application specific css styles
        |
        +---- controllers   // folder for AngularJS controllers needed for the application
        |
        +---- directives    // folder for AngularJS directives needed for the application
        |
        +---- services      // folder for AngularJS services needed for the application
        
```

**Initial Setup Code and AngularJS Application Initialization**

Let's start some ready to use code which will setup the environment and will do some HTML scaffolding to create the basic structure
of the page we will use to build the application:

```html
<!-- index.html -->
<!doctype html>
<html lang="en">
<head>

    <!-- Set Page Encoding -->
    <meta charset="UTF-8">

    <!-- Take Control of Viewport -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <!-- Set Page Title -->
    <title>AngularJS Todo App</title>

    <!-- Font Awesome Icons Stylesheet -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
    
    <!-- Bootstrap Stylesheet -->
    <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">

    <!-- Application Stylesheet -->
    <!-- TODO -->
</head>
<body>
    <!--
        Use Bootstrap styles to build a three column layout.
        The application will render in the middle column.
    -->
    <div class="row">
        <div class="col-md-3"></div>
        <div class="col-md-6" id="application-container">
            <!--
                The Application will render here.
            -->
        </div>
        <div class="col-md-3"></div>
    </div>
    
    <!-- Angular Library, also bring in jQuery replacement jQlite -->
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.1/angular.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.6.1/angular-route.js"></script>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.1/angular-animate.js"></script>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.1/angular-sanitize.js"></script>

    <!-- Boostrap Library for AngularJS -->
    <script src="https://angular-ui.github.io/bootstrap/ui-bootstrap-tpls-2.5.0.js"></script>
    
    <!-- Todo Application Code -->
    <!-- TODO -->
    
</body>
</html>
```

Apart from loading stylesheets and javascripts dependancies, the code does nothing but creating a simple three column layout with 
Bootstrap.

Starting from the above code, the first thing to do is to create an AngularJS application and bind it to the HTML element it will live
into.

To create a new application we use the following code:

```javascript
// app.js
//
// Define the AngularJS application main module
// along with its dependencies: the ngRoute module.
//
angular.module('TodoApp', ['ngRoute'])
//
// When an AngularJS app is started, it has a certain “boot-order”.
// First the "config" step.
//
.config(function($routeProvider) { // providers & constants
    // do configuration
})
//
// Then the "run" step.
//
.run(function() { // services & constants
    // do initialization
});
```

The call to `angular.module('TodoApp', ['ngRoute'])` creates a new AngularJS module named `TodoApp` to hold our application's
facilities.

We are also stating the built-in module `ngRoute` will be a dependancy of our application, so it will be injected by AngularJS core
as argument to our services and controllers we will create later on.

To include `app.js` in our application we simply load it from a `<script>` tag from `index.html`:

```
<!-- index.html -->
...
<body>
...
    <!-- Todo Application Code -->
    <script src="app.js"></script>
...
</body>
```

Then we have to bind the application `TodoApp` to the HTML element it will live in. We do this with AngularJS directive
`ng-app`. In our case we want to bind the application to the `<body>` tag of the HTML page:

```html
<!-- index.html -->
...
<body data-ng-app="TodoApp">
    ....
</body>
...
```

**The Model**

As stated in the previous section about the *MVC* architecture, the *Model* responsible for managing the data of the application.
Since we are designing an application whose purpose is to allow the user to manage a list of things to do, we can have a high level
representation of our data as a list of items.

This list of items must allow CRUD operations on its data, so we need to provide the model with the related functionalities to 
manipulate data.

Moreover we would like to wrap our data and related functionalities into a module we can plug into our application as the single point
of responsibility about managing our todo list. We want our model to be a module, or a service in AngularJS jargon, of the application.

AngularJS has several ways to create modules, we will use the *factory* way.

To create a factory we use the following code:

```javascript
angular.module('TodoApp')
// Attach the new factory to the TodoApp, so we can reference it as we need it
.factory('factoryName', function() {
    // Factory code
    
    var api = {
    
        // Here we will declare the module APIs.
    
    };
    
    return api;
});
```

We want our factory to have the following functionalities:

* model the data (our todo list)
* allow CRUD operations on data
* allow to persist data between "sessions"

The above requirements form the basis of most *Model*s which also take care of data persistance, be it a remote API or a storage engine
of any other type.

Since writing our model as an interface to a remote API would require to set up the API itself, we will write the model to persist
data using browser's local storage.
In addition, the APIs will have a *promise based* interface: this way, from a usage perspective, it won't be much different from a
real world remote API backed up by a remote server.

The starting point for our service is the following code, which declares a factory named `localStorage` whithin the main application
module `TodoApp`:

```
// services/persist/local-storage.js
angular.module('TodoApp')
//
// Create a factory to hold the local storage persistance API.
// This API will store/retrieve todo list in/from browser local
// storage.
//
// The API is promise-based to be fully compatible with a real world
// backended API, so we need the $q module, which is a promise library
// packed into angularjs.
//
// $filter is an AngularJS builtin module to manipulate arrays.
//
.factory('localStorage', function($q, $filter) {
    
    //
    // The object cointaining the exposed API for the local storage
    // persistance engine.
    //
    var store = {

        // We are modelling the todo list as an array of JSON objects.
        todos: [],
        
        //
        // APIs to allow CRUD operations.
        //
        
        create: function (todo) {
        
        },

        read: function () {

        },

        update: function (todo) {

        },

        delete: function (todo) {
        
        },

        //
        // Low Level functions to get/store the todo list from the
        // local storage.
        //
        // They shouldn't be used outside the module, hence the trailing '_' to
        // mark them as 'private'.
        //
        _getFromLocalStorage: function() {
            
        },

        _saveToLocalStorage: function (todos) {
            
        }

    };

    return store;

});
```

The following code implements the `create` function, which simply store the given todo in the todo array. Note the promise based
interface:

```
// services/persist/local-storage.js
...
//
// APIs to allow CRUD operations.
//

create: function (todo) {

    // Create a new promise, which will be returned
    // to the user as the result of this function
    // and will be resolved once the creation is 
    // completed
    var deferred = $q.defer();

    // Extend the todo we are going to create with
    // a unique id, to allow search and update
    // operations
    todo = angular.extend(todo, {
        id: Date.now()
    });

    // Store the todo in our todo list
    store.todos.push(todo);

    // Save the todo list in browser's local storage
    store._saveToLocalStorage(store.todos);

    // Resolve the promise before returning it
    deferred.resolve(store.todos);

    // Return a promise to emulate a real world API
    return deferred.promise;

},

...
```

Using the same approach we can write `read`, `update`, `delete` functions, along with some other utility function we need for
the application that directly manipulates application data.

The full code for the `localStorage` module is available at:

<https://github.com/DocBrown85/angularjs-todo-application/blob/master/services/persist/local-storage.js>

**The View**

Coming soon.

**The Controller**

Coming soon.

**The Full Code**

The full code for the TODO Application written with AngularJS framework is available here:
<https://github.com/DocBrown85/angularjs-todo-application.git>
