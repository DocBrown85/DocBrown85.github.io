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
  More informations can be found [here](https://nodejs.org/en/).
* **Express** is a software stack built over Node to ease the development of networked applications.
  More informations can be found [here](http://expressjs.com/).
* **MongoDB** is a non-relational database engine with dynamic schemas based on the JSON format.
  More informations can be found [here](https://www.mongodb.com/).

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

## Prerequisites

* The Node engine has to be properly installed and setup according to the used platform (Windows or Linux). Refer to the official
documentation for how to install and setup Node.
* The storage engine MongoDB can be either installed on the development environment or a cloud based solution can be used. I'm using
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

from application's root directory.

After answering some question about the application, a file named `package.json` will be created
in the root directory. This file is used, among other things, to keep trace of our application's name, starting point, and dependencies.

We then need to install Express and Mongoose packages:

* Express, as stated above, is the stack to build networked application with Node.
* Mongoose is a high-level stack to communicate with MongoDB databases.
  More informations can be found [here](http://mongoosejs.com/).

We install Express with the following command (from the root directory):

```
npm install --save express
```
while to install Mongoose we can execute the command (from the root directory):

```
npm install --save mongoose
```

## The Server

`server.js` is the entry point of the application, so its purpose is to include the packages we need, do the initial setup and open
a port to listen for incoming requests:

```javascript
// server.js

// call the packages we need
var express     = require('express');       // call express
var app         = express();                // define a new app with express
var bodyParser  = require('body-parser');
var mongoose    = require('mongoose');

// configure app to use bodyParser()
// this will let us get the data from a POST
app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());

// define application port
var port        = process.env.PORT || 3000;

// start listening for incoming connections
app.listen(port, function() {
  console.log('NodeJS TODO REST API server started on port: ' + port);
});

```
By executing `server.js` with Node:
```
node server.js
```
the following output is printed on the screen:
```
NodeJS TODO REST API server started on port: 3000
```
so the server is up and running.

## The Routes

Routes are a way to map URLs to functions in our applications. Recalling the above table, we want a specific pair of (URL, HTTP Method)
to map to a CRUD operation on our todo list.

We can specify this mapping using the `router` module of an Express application:

```javascript
//api/routes/todo-routes.js

'use strict'

var express     = require('express');

// ROUTES FOR OUR API
// =============================================================================

module.exports = function(app) {

  // get an instance of the express Router
  var router = express.Router();

  //
  // configure routes
  //

  // test route, to check if server is up and running
  router.route('/')
  .get(function(req, res) {
    res.json({ message: 'NodeJS TODO REST API' });
  });

  //
  // TODO REST ROUTES
  //

  // all URLs ending with '/todos' will be routed here
  router.route('/todos')
  .get(function(req, res) {       // GET requests will execute this function
    res.json({ message: 'GET Request on /todos!' });
  })
  .post(function(req, res) {      // POST requests will execute this function
    res.json({ message: 'POST Request on /todos!' });
  })
  .delete(function(req, res) {    // DELETE requests will execute this function
    res.json({ message: 'DELETE Request on /todos!' });
  });

  // all URLs ending with '/todos/:todoId' will be routed here
  router.route('/todos/:todoId')
  .get(function(req, res) {       // GET requests will execute this function
    res.json({ message: 'GET Request on /todos/' + req.params.todoId + '!' });
  })
  .post(function(req, res) {      // POST requests will execute this function
    res.json({ message: 'POST Request on /todos/' + req.params.todoId + '!' });
  })
  .delete(function(req, res) {    // DELETE requests will execute this function
    res.json({ message: 'DELETE Request on /todos/' + req.params.todoId + '!' });
  });

  // register routes: all of our routes will be prefixed with /api
  app.use('/api', router);

}
```
we then register our routes by requiring the `todo-routes.js` module from `server.js`:

```javascript
// server.js
...
// import the route module
var routes      = require('./api/routes/todo-routes.js');
...
// define application port
var port        = process.env.PORT || 3000;
...
// register todo routes
routes(app);
...
// start listening for incoming connections
app.listen(port, function() {
  console.log('NodeJS TODO REST API server started on port: ' + port);
});
```
If we run the code and start sending requests to the server, for example with POSTMan, we will get the corresponding message as the
result.
For example, by sending the following request:
```
GET 127.0.0.1/api/todos/1
```
we will receive:
```
{ message: 'GET Request on /todos/1' }
```
## The Model

The model for the Todo item is very simple: it has a text description and a flag to state if the todo is completed or not:

```javascript
// api/models/todo-model.js
'use strict';

var mongoose = require('mongoose');
var Schema = mongoose.Schema;

var TodoSchema = new Schema({
  text: {
    type: String,
    required: 'enter the name of the task'
  },
  done: {
    type: Boolean,
    default: false
  }
});

module.exports = mongoose.model('Todo', TodoSchema);
```
The above code defines a new Mongoose Schema with the fields required to model our todo items. We will use an instance of this object
to communicate with the MongoDB database to provide search and persistence capabilities for our todo list.

After defining the model, we need to require it from `server.js`, along with setting up the communication with the MongoDB database:

```javascript
// server.js
...
// setup database connection
mongoose.Promise = global.Promise;
mongoose.connect(
  'connection_string_to_mongo_db_database',
  {
    useMongoClient: true
  }
);
...
// import the Todo model
var Todo        = require('./api/models/todo-model.js');
// import the route module
var routes      = require('./api/routes/todo-routes.js');
...
// define application port
var port        = process.env.PORT || 3000;
...
// register todo routes
routes(app);
...
// start listening for incoming connections
app.listen(port, function() {
  console.log('NodeJS TODO REST API server started on port: ' + port);
});
```

## The Controller

In our architecture the controller is responsible for executing the needed actions on the Todo model to provide all functionalities
exposed from the API.

For example, by sending the following request:
```
GET 127.0.0.1/api/todos
```
we are required to provide the current list of todos. We can do this defining another Node module with the proper API:

```javascript
// api/controllers/todo-controller.js
'use strict';

var mongoose = require('mongoose'),

// build an instance of a Todo Schema
Todo = mongoose.model('Todo');

module.exports = {

    // function to read current todo list
    readAll: function(req, res) {

      // Use the instance of Todo Schema to retrieve the
      // current todo list from MongoDB database
      Todo.find(function(err, todos) {

        if (err) {

          res.status(400).send(err);

          return;

        }

        res.json(todos);

      });

    },

    // function to create a new Todo
    create: function(req, res) {
      // TODO
    },

    // function to get data about a Todo
    read: function(req, res) {
      // TODO
    },

    // function to updata data about a Todo
    update: function(req, res) {
      // TODO
    },

    // function to delete a Todo
    delete: function(req, res) {
      // TODO
    }

}
```

After defining the `readAll` function, we need to execute that function whenever a `GET` request is sent to the URL 
`127.0.0.1/api/todos`.

We do this by requiring the controller module from the router module and by registering the function to be called by the proper handler:

```javascript
// api/routes/todo-routes.js
'use strict'

var express     = require('express');

// require the controller
var todoList    = require('../controllers/todo-controller.js');

// ROUTES FOR OUR API
// =============================================================================

module.exports = function(app) {

  // get an instance of the express Router
  var router = express.Router();

  //
  // configure routes
  //

  // test route, to check if server is up and running
  router.route('/')
  .get(function(req, res) {
    res.json({ message: 'NodeJS TODO REST API' });
  });

  //
  // TODO REST ROUTES
  //

  // all URLs ending with '/todos' will be routed here
  router.route('/todos')
  .get(todoList.readAll)              // GET requests will execute this function
  .post(todoList.create)              // POST requests will execute this function

  // all URLs ending with '/todos/:todoId' will be routed here
  router.route('/todos/:todoId')
  .get(todoList.read)                 // GET requests will execute this function
  .put(todoList.update)               // PUT requests will execute this function
  .delete(todoList.delete);           // DELETE requests will execute this function

  // register routes: all of our routes will be prefixed with /api
  app.use('/api', router);

}
```
in the above, in addition to register the `readAll` handler for the route `GET 127.0.0.1/api/todos`, we registered handlers for all
functions exported by the controller to the related route. 

### Function to create a new Todo

Using a software like POSTMan we can send to our API a `POST 127.0.0.1/api/todos` with the body cointaining the following data:

```javascript
{
  "text": "Walk the Dog",
  "done": false	
}
```
to create a new todo item.

We can handle the request from the controller:

```javascript
// api/controllers/todo-controller.js
'use strict';

var mongoose = require('mongoose'),

// build an instance of a Todo Schema
Todo = mongoose.model('Todo');

module.exports = {

    // function to read current todo list
    readAll: function(req, res) {
      ...
    },

    // function to create a new Todo
    create: function(req, res) {
      
      // create a new instance of the Todo model
      // set todo data (from the request)
      var todo = {};
      todo.text = req.body.text;
      todo.done = req.body.done;

      // save todo and check for errors
      Todo.create(todo, function(err, todo) {

        if (err) {

          res.status(400).send(err);

          return;

        }

        // echo the new created todo
        res.json(todo);

      });
      
    },

    // function to get data about a Todo
    read: function(req, res) {
      // TODO
    },

    // function to updata data about a Todo
    update: function(req, res) {
      // TODO
    },

    // function to delete a Todo
    delete: function(req, res) {
      // TODO
    }

}
```
Now, sending a `POST 127.0.0.1/api/todos` with the body cointaining the following data:

```javascript
{
  "text": "Walk the Dog",
  "done": false	
}
```
to our API we will receive a response containing the fresh new todo item:

```javascript
{
  "text": "Walk the Dog",
  "done": false,
  __v: 0,
  _id: "5a5f4fba87adcc530ee3c51c"
}
```
note the `_id` automatically added by MongoDB upon creation of the new record.

### Function to get a Todo

By sending a `GET 127.0.0.1/api/todos/5a5f4fba87adcc530ee3c51c` we should be able to retrieve all data belonging to the todo 
item with id `5a5f4fba87adcc530ee3c51c`.

We can handle this request from the controller:

```javascript
// api/controllers/todo-controller.js
'use strict';

var mongoose = require('mongoose'),

// build an instance of a Todo Schema
Todo = mongoose.model('Todo');

module.exports = {

    // function to read current todo list
    readAll: function(req, res) {
      ...
    },

    // function to create a new Todo
    create: function(req, res) {
      ...
    },

    // function to get data about a Todo
    read: function(req, res) {
      
      // use the findById() function from Todo Schema to retrieve
      // a todo with the specified id
      Todo.findById(req.params.todoId, function(err, todo) {

        if (err) {

          res.status(400).send(err);

          return;

        }

        res.json(todo);

      });
      
    },

    // function to updata data about a Todo
    update: function(req, res) {
      // TODO
    },

    // function to delete a Todo
    delete: function(req, res) {
      // TODO
    }

}
```

### Function to update a Todo

By sending a `PUT 127.0.0.1/api/todos/5a5f4fba87adcc530ee3c51c` request with the following body:

```javascript
{
  _id: "5a5f4fba87adcc530ee3c51c",
  "text": "Walk the Dog",
  "done": true
}
```

we should be able to update specified data belonging to the todo item with id `5a5f4fba87adcc530ee3c51c`.

We can handle this request from the controller:

```javascript
// api/controllers/todo-controller.js
'use strict';

var mongoose = require('mongoose'),

// build an instance of a Todo Schema
Todo = mongoose.model('Todo');

module.exports = {

    // function to read current todo list
    readAll: function(req, res) {
      ...
    },

    // function to create a new Todo
    create: function(req, res) {
      ...
    },

    // function to get data about a Todo
    read: function(req, res) {
      ...
    },

    // function to updata data about a Todo
    update: function(req, res) {
      
      // use the findById() function from Todo Schema to retrieve
      // a todo with the specified id
      Todo.findById(req.params.todoId, function(err, todo) {

        if (err) {

          res.status(400).send(err);

          return;

        }

	// update todo informations according to what
	// is specified in the body of the request
        todo.text = req.body.text;
        todo.done = req.body.done;

	// save the updated todo item
        todo.save(function(err) {

          if (err) {

            res.status(400).send(err);

            return;

          }

          res.json({ message: 'Todo Updated!' });

        });

      });
      
    },

    // function to delete a Todo
    delete: function(req, res) {
      // TODO
    }

}
```

### Function to delete a Todo

By sending a `DELETE 127.0.0.1/api/todos/5a5f4fba87adcc530ee3c51c` we should be able to delete the todo item with id
`5a5f4fba87adcc530ee3c51c`.

We can handle this request from the controller:

```javascript
// api/controllers/todo-controller.js
'use strict';

var mongoose = require('mongoose'),

// build an instance of a Todo Schema
Todo = mongoose.model('Todo');

module.exports = {

    // function to read current todo list
    readAll: function(req, res) {
      ...
    },

    // function to create a new Todo
    create: function(req, res) {
      ...
    },

    // function to get data about a Todo
    read: function(req, res) {
      ...
    },

    // function to updata data about a Todo
    update: function(req, res) {
      ...
    },

    // function to delete a Todo
    delete: function(req, res) {
      
      // use the remove() function from Todo Schema to delete
      // todos that match the specified conditions
      Todo.remove(
	  {
            _id: req.params.todoId
	  },
	  function(err) {

	      if (err) {

	        res.status(400).send(err);

	        return;
	      }

	      res.json({ message: 'Todo Deleted!' });
	  }

	  );
      
     }

}
```
## The Full Code

The full code for the Simple Todo REST API with Node, Express and MongoDB is available
[here](https://github.com/DocBrown85/nodejs-todo-rest-api).
