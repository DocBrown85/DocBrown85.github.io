---
layout: post
title: A Simple Todo REST API with Node, Express and MongoDB
---

## REST APIs and CRUD operations

REST is an acronym for **REpresentational** **State** **Transfer**. It is a software architecture to provide interoperability between
systems on the Internet. REST-compliant Web services allow to access and manipulate textual representations of Web resources using a 
uniform and predefined set of stateless operations.

Every resource within a RESTful service is identified with a URI, while requests to that URI are mapped to operations on that resource.
The most common architecture provides CRUD operations on resources by using the HTTP protocol, mapping each operation to a proper HTTP
method.

For example, suppose a service to provide CRUD operations on a Todo list such that clients can *Create*, *Read*, *Update* and *Delete*
items from that list.

A RESTful interface to such service might be the following:

| Operation                 | HTTP Verb     | URL                   |
| --------------------------|---------------| ----------------------|
| READ the whole Todo list  | GET           | /api/todos            |
| CREATE a Todo Item        | POST          | /api/todos            |
| READ a Todo Item          | GET           | /api/todos/:todoId    |
| UPDATE a Todo Item        | PUT           | /api/todos/:todoId    |
| DELETE a Todo Item        | DELETE        | /api/todos/:todoId    |

## The Node, Express, MongoDB Stack

Node, Express and MongoDB are software stacks used to build server-side applications with JavaScript:

* **Node** is an engine to run JavaScript from the command line, the same way we can run Python or BASH. It also provides an
  environment to build asynchronous and event driven applications.
* **Express** is a software stack built over Node to ease the development of networked applications.
* **MongoDB** is a non-relational database engine with dynamic schemas based on the JSON format.

When used together, Node, Express and MongoDB allow to build pure JavaScript applications using an asynchronous and event driven
approach.

## Application Overview

In the following we will develop a simple backend application to provide a RESTful service to manage a list of todo items with
Node and Express, with storage capability provided by a MongoDB database.

The APIs exposed by the service are summarized in the following table:

| Operation                 | HTTP Verb     | URL                   |
| --------------------------|---------------| ----------------------|
| READ the whole Todo list  | GET           | /api/todos            |
| CREATE a Todo Item        | POST          | /api/todos            |
| READ a Todo Item          | GET           | /api/todos/:todoId    |
| UPDATE a Todo Item        | PUT           | /api/todos/:todoId    |
| DELETE a Todo Item        | DELETE        | /api/todos/:todoId    |

## Prerequisite

The Node engine has to be properly installed and setup according to the used platform (Windows or Linux). Refer to the official
documentation for how to install and setup Node.

The storage engine MongoDB can be either installed on the development environment or a cloud based solution can be used. I'm using
a cloud based solution hosted by [MongoLab](https://mlab.com/).

## Application Initial Setup

Application's files will be organized with the following layout:

```
            nodejs-todo-rest-api
                    |
                    +--------------- api
                    |                 |
                    |                 +-------- controllers // directory to hold application controllers
                    |                 |
                    |                 +-------- models      // directory to hold application models
                    |                 |
                    |                 +-------- routes      // directory to hold application routes
                    |
                    +---------------- server.js             // application's entry point
                    
```

Supposing the existence of the previous structure, we initialize a new Node project with the command

```
npm init
```

from application's root directory. After answering some question about the application, a file named `package.json` will be created
in the root directory. This file is used, among other things, to keep trace of our application's name, starting point, and dependancies.

We then need to install Express and Mongoose packages:

* Express, as stated above, is the stack to build networked application with Node.
* Mongoose is a high-level stack to communicate with MongoDB databases.

We install Express with the following command (from the root directory):

```
npm install --save express
```
while to install Mongoose we can execute the command:

```
npm install --save mongoose
```

## The Server
Coming Soon.

## The Model

Coming Soon.

## The Controller

Coming Soon.

## The Routes

Coming Soon.

## The Full Code

The full code for the Simple Todo REST API with Node, Express and MongoDB is available
[here](https://github.com/DocBrown85/nodejs-todo-rest-api).
