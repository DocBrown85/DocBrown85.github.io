---
layout: post
title: A Simple TODO Application with AngularJS
---

**Foreword**

This is a long ongoing description not yet completed. For the complete code of the TODO Application refer to the **The Code** section
below.

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

**Step 0: Initial Setup Code and AngularJS Application Initialization**

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

**Step 1: The View**

Coming soon.

**Step 2: The Controller**

Coming soon.

**Step 3: The Model**

Coming soon.

**The Code**

The full code for the TODO Application written with AngularJS framework is available here:
<https://github.com/DocBrown85/angularjs-todo-application.git>


