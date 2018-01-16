---
layout: post
title: A Simple TODO Application with AngularJS
---

**Introduction**

AngularJS is a JavaScript Web Application framework developed at Google. It extends standard HTML with **directives** which add
functionalities and expressiveness to plain HTML.

Another important feature of AngularJS is *two-way binding*, a useful concept which is best demonstrated with the following example:

```javascript
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

**Step 1: The View**

Coming soon.

**Step 2: The Controller**

Coming soon.

**Step 3: The Model**

Coming soon.






