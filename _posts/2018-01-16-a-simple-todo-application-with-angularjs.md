---
layout: post
title: A Simple TODO Application with AngularJS
---

## Foreword

For the complete code of the TODO Application refer to the **The Full Code** section below.

## Introduction

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

## The TODO Application

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

This kind of application is useful to evaluate and learn the basic capabilities of a framework (be it a frontend or a backend
framework), and can serve as a benchmark to evaluate different frameworks when choosing a particular one.

In the following will be described the steps to build a really simple Todo Application with a focus on the AngularJS framework, although
we will use some other frameworks to build a better looking application.

The additional framework will be:

* *Bootstrap.js*: a frontend HTML, CSS, and JavaScript framework for developing responsive, mobile-first sites and applications,
                  see <https://getbootstrap.com/docs/3.3/getting-started/>.

* *Font Awesome Icons*: a CSS framework for fonts and icons, see <http://fontawesome.io/icons/>.

## Application Architecture

AngularJS enforces an MVC architecture, which basically says we have to split our application in a *Model*, a *View*, and a
*Controller*.

The *Model* is responsible for managing the data of the application. It responds to the request from the view and it also
responds to instructions from the controller to update itself.

The *View* is responsible for data presentation in a particular format, triggered by a controller's decision to present the data.

The *Controller* is responsible for responding to the user input and perform interactions on the data model objects.
The controller receives the input, it validates the input and then performs the business operation that modifies the state
of the data model.

## Code Organization

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

## Initial Setup Code and AngularJS Application Initialization

Let's start with some ready to use code which will setup the environment and will do some HTML scaffolding to create the basic structure
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

Apart from loading stylesheets and javascripts dependencies, the code does nothing but creating a simple three column layout with 
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
as argument to our controllers we will create later on.

To include `app.js` in our application we simply load it from a `<script>` tag from `index.html`:

```html
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
<body ng-app="TodoApp">
    ....
</body>
...
```

## The Model

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

The starting point for our service is the following code, which declares a factory named `localStorage` within the main application
module `TodoApp`:

```javascript
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

For example, the following code implements the `create` function, which simply store the given todo in the todo array.
Note the promise based interface:

```javascript
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
        _id: Date.now()
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

Using the same approach we can write `read`, `update` and `delete` functions, along with all other functions we may need that directly
manipulate application data.

The full code for the `localStorage` module is available at:

<https://github.com/DocBrown85/angularjs-todo-application/blob/master/services/persist/local-storage.js>

## The View

The view is responsible for presenting application data to the user and allow to interact with it. Since we are developing a CRUD
application, the view will be responsible for creating, updating and deleting our todos.

The layout of the application can be summarized as follows:

```
+----------------------------------------+
| this part will allow to add a new todo |
+----------------------------------------+
| this part will display current todo    |
| list, and will allow single entries to |
| be marked as completed or to remove    |
| them                                   |
+----------------------------------------+
| This part will allow to filter current |
| todo list according to todos state and |
| to remove completed ones               |
+----------------------------------------+
```

The above layout will be rendered as HTML using AngularJS builtin template engine.

The first step is to declare where our application template will be rendered in the view. We do this using the **ng-view** directive
in the HTML element we want to be the cointainer of the view:

```html
<!-- index.html -->
...
<div class="row">
    <div class="col-md-3"></div>
    <div id="application-container" class="col-md-6" data-ng-view>
        <!-- Application template will render here. -->
    </div>
    <div class="col-md-3"></div>
</div>
...
```

Then we have to set up a place for the template.

We have two options: 

* Store the template in a separate HTML file and load it with an HTTP call
* Embed the template in `index.html`

We choose the second option, so we set up a `<script>` tag to contain the application template:

```html
<!-- index.html -->
...
<script type="text/ng-template" id="todo-app-index.html">

</script>
...
```

The type of the `<script>` element must be specified as `text/ng-template`, and a cache name for the template must be assigned through
the element's `id`.

Next we have to setup the template itself. For the sake of brevity the following will focus on control elements directly involved in
user's interaction with the todo list, skipping over the boilerplate code to setup the application layout with Bootstrap.

### Sub-Template to insert a new Todo

The first control will allow the user to insert a new todo and to mark all current todos as completed or pending:

```html
<!-- index.html -->
...
<script type="text/ng-template" id="todo-app-index.html">
    ...
    <form id="todo-add-form" name="todo-add-form" data-ng-submit="createTodo()">
        ...
        <input type="checkbox" data-ng-model="allChecked" data-ng-click="markAll(allChecked)"/>
        ...
        <input type="text" name="todo-description-input" data-ng-model="newTodoDescription" required/>
        ...
    </form>
    
    ...
</script>
...
```
We are declaring a form with two controls:

* A checkbox to mark all current todos as completed or pending
* An input text to insert a new todo description

Whithin the form, we are using several AngularJS directives:

* The `ng-submit` directive specifies a function to run when the form is submitted, in the snippet above the `createTodo()` function
  which will be part of the controller.
* The `ng-model` directive binds the value of HTML controls (input, select, textarea) to application data.
  In this case we are bounding the "checked" status of the checkbox to the `allChecked` variable.
* The `ng-click` directive defines AngularJS code that will be executed when the element is being clicked.
  In our case, when the element is clicked, the function `markAll()` will be executed, passing the value of the variable `allChecked` as
  the argument.
* Again, we are using the `ng-model` directive to bind the value of the text element to the application variable `newTodoDescription`.

### Sub-Template to show current todo list and to update/delete single todo

The next task is to allow the template to display current todo list and to allow the user to update and delete displayed items.
We can do this with the HTML specified int the `todo-list div`:

```html
<!-- index.html -->
...
<script type="text/ng-template" id="todo-app-index.html">
    ...
    <form id="todo-add-form" name="todo-add-form" data-ng-submit="createTodo()">
        ...
    </form>
    <div id="todo-list">
        ...
        <div ng-repeat="todo in todos | orderBy:'text' | filter: statusFilter track by $index">
            ...
            <input type="checkbox" ng-model="todo.done" ng-change="toggleDone(todo)"/>
            ...
            <label ng-dblclick="editTodo(todo)" ng-class="{'done':todo.done}">
                {{todo.text}}
            </label>
            ...
            <i class="icon-delete fa fa-close" ng-dblclick="deleteTodo(todo)"></i>
            ...
            <form ng-submit="saveTodo(todo, 'submit')" ng-class="{hidden: todo != editedTodo}">
                ...
                <input type="text" ng-model="todo.text" ng-blur="saveTodo(todo, 'blur')" ng-trim="false">
                ...
            </form>
        </div>
        ...
    </div>
    ...
</script>
...
```
Basically we are iterating over current todo list and for each item we are generating a `div` element which contains a checkbox,
a label, an inline element to display an icon, and a form with an input text to change todo description.

Here the details:

The `ng-repeat` directive repeats a set of HTML, a given number of times.
The set of HTML will be repeated once per item in a collection, which must be an array or an object. Each instance of the repetition
is given its own scope, which consist of the current item.

In this case we are repeating for each item in `todos` variable, which is an array holding all todos we will create.

We are also using filters:

- `orderBy`, which allows to sort an array by an expression. In our case we are ordering by the 'text' property of the todo items.

- `filter`, which allows to filter a collection by a predicate. In our case we are filtering by an object (`statusFilter`) which
  will specify the property and its value we are filtering by.
  
For the checkbox we are using the `ng-model` directive to bind the value of the checkbox to a variable within the application scope.
Since we are using the `ng-repeat` directive, each instance of the repetition is given its own scope (which consist of the current
item), so we are bounding the value of the checkbox to the property "done" of the current todo item.
We are also binding the execution of the function `toggleDone()` (which receives the current item as the first argument) on `change`
events occurring to the checkbox.

Regarding the label we are binding the execution of the function `editTodo()` (which receives the current item as the first argument) on
`double click` events occurring to the label using the `ng-dblclick` directive.
We are also dynamically assigning the class "done" to the element on the basis of the value of the `done` property of the binded 
`todo` item.
The value of the label is displayed by evaluating the expression `{{todo.text}}` for the current todo item.

For the `form` element we are binding the execution of the `saveTodo()` function to `submit` events of the form with the 
`ng-submit` directive.
We are also dynamically assigning the class `hidden` to the form depending on the value of the expression `todo != editedTodo`,
where `todo` and `editedTodo` are variables in our application's scope.

Last, for the form's inner `input` element, we are binding the value of the textbox to a variable within the application scope.
Again, since we are using the `ng-repeat` directive, each instance of the repetition is given its own scope (which consist of the
current item), so we are bounding the value of the textbox to the property `text` of the current todo item.
With the `ng-blur` directive we are binding the execution of the `saveTodo()` function (which receives the current todo and the string
`'blur'` as arguments) when the input field loses focus (`onblur`).
With `ng-trim` directive we are disabling the automatic trimming of the input text, while with `set-focus` we are setting a custom
directive to set the focus on the element, which will be executed when the expression '`todo == editedTodo`' evaluates to `true`.

The full code for the `index.html` is available at:

<https://github.com/DocBrown85/angularjs-todo-application/blob/master/index.html>

## The Controller

The Controller is the glue between our application View and the Model. We can specify this relation configuring the `ngRoute` module
in the `config` phase of our application:

```javascript
// app.js
...
.config(function($routeProvider) { // providers & constants
    // do configuration
    
    var routeConfig = {
        templateUrl: 'todo-app-index.html',
        controller: 'todoController'
    };

    $routeProvider
        .when('/', routeConfig)
        .otherwise({
            redirectTo: '/'
        });
})
...
```
The `templateUrl` key specifies the template to use to render a particular page. In this case we are stating to use the template
we previously provided in the `<script>` tag.
The `controller` key specifies the controller to use to render a particular page. In this case we are stating to use the 
`todoController` we are going to write shortly.

The following code adds the `todoController` to the application:

```javascript
angular.module('TodoApp')
.controller('todoController', function($scope, $routeParams, $filter, store) {
    // TODO
});
```
The controller receives following injected arguments:

* `$scope` is the application object (the owner of variables and functions used throughout the application).
* `$routeParams` holds the params we previously configured for this route.
* `$filter` is an utility object to filter collections.
* `store` is the API to the storing engine we are using to persist todos.

Within the body of `todoController` we have to write all functionalities that tie together the view with the model.
For example, when designing the view, we provided the function `createTodo()` to add a new todo to the list. This 
function (and nearly all functions and variables we used in the view) need an implementation we can put in the controller:

```javascript
// controllers/todo-controller.js
angular.module('TodoApp')
.controller('todoController', function($scope, $routeParams, $filter, store) {
    
    $scope.newTodoDescription = "";
    
    $scope.createTodo = function() {

        var newTodo = {
            text: $scope.newTodoDescription,
            done: false
        };

        store
        .create(newTodo)
        .then(function() {
            $scope.newTodoDescription = "";
        });

    };
    
});
```
Note we are attaching all needed functions and variables to the `$scope` object: this way they will be available from the view
whenever `todoController` is used.

Using the same approach we can write all other functions and variables we needed while writing the view.

The full code for the `todoController` is available at:

<https://github.com/DocBrown85/angularjs-todo-application/blob/master/controllers/todo-controller.js>

## The Full Code

The full code for the TODO Application written with AngularJS framework is available here:

<https://github.com/DocBrown85/angularjs-todo-application.git>
