# Express REST API

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Name 3 real world use cases where you’d want to change the request with custom middleware

1. Adding timestamps to make a log of activity

2. Checking login creditials of user

3. Taking information from params/query string to request info from server

## True or false: The route handler is middleware?

Handler functions are callbacks on the request to the API endpoint and they can be chained. If there is just one callback that sends the final response, then I would not call it "middleware", but if there are multiple in a chain, then, yes they are "middleware."

## In what ways can a middleware function end the process and send data to the browser?

Middleware shouldn't be ending the process unless it is an error, in that case the error would be passed to the ('next') object and the rest of the chain of middleware would skip. The request would port down to the middleware handlers. The middleware can modify successful routes as well, directing them to differnet processes based on user input.

## At what point in the request lifecycle can you “inject” middleware?

You apply the middleware before the request reaches the server. Then server responds based on your fully modified request object with the response.

## What can cause express to error with “Request headers sent twice, cannot start a second response”

## Document the following Vocabulary Terms

Term | Definition
----- | -----------
Middleware | callback functions on API endpoints
Request Object | values passed by client browser to server in WRRC cycle
Response Object | values passed by server to client in WRRC cycle
Application Middleware | middleware that is run on every request in an app
Routing Middleware | middleware that is specific to certain routes in an app
Test Driven Development | system of development where functionality is identified first, then tests are written to demonstrate the appropriate functionality is performing as expected, then the actual methods are written to satisfy the behavior outlined in the tests (so methods must conform to test expectations and not vice versa)
Behavioral Testing | mixes user stories into TDD process encourain management and development teams to work together on the development process; tests are written out in a more generalized context and focuses on: what to test and to what extent, how much testing to focus on at a time, how to understand success/failure of tests

## Notes for Required Readings

- [MDN: Classes in Javascript](#mdn-es6-classes)
- [Express Docs: Routing](#express-routing)
- [ExpressJS 4.0 Router](#expressjs-4.0-router)

### [MDN ES6 Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)

Classes are templates for creating objects. They are simply functions with special syntax/declarations.

Important principles:

- Encapsulation
- **Remember they do not get hoisted! So don't just put them anywhere**
- body of a class is written in strict mode

Classes contain:

- constuctor: function that initializes an object, 1 per class (uses super() constructor to call parent class)
- prototype methods: getters/setters/other functionalities
- generator methods: looks like this means returning a set of things (i.e. return all sides of a polygon object)
- binding with prototype/static methods: if you pass this to a prototype method it is only available to the insantiated obejct; if you pass it to the static method it won't be available to the instance
- instance properties: must be defined inside of class methods
- static properties: must be defined outside of class methods
- public fields: declare them outside of the constructor, makes them required properties for all instances of the class, do not need initial values
- private fields: declared outside of constructor with a ```#property = x``` format; these can only be read/written inside class body 
- static methods/properties: not available to class instances, you would have to call them on the object itself (example below to clarify)

``` JavaScript
class Point {
  constructor(x,y) {
    this.x = x;
    this.y = y;
  }
  static displayName = "Point";
 }

const p1 = new Point(1,5);
p1.displayName; //undefined 
console.log(Point.displayName) //"Point"
//didn't copy over whole example, but the same logic applies to functions/methods
```

Simple structure:

``` JavaScript
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
    }
}

let square = new Rectangle(4,4);
```

You can alternately use [class expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/class)

``` JavaScript
const MyClass = class [className] [extends otherClassName] {
    // class body
};
```

Why use expresions:

- you can omit the class name 
- you can redfine the class without throwing a SyntaxError

#### Sub classing with Extends

Extends is used in the class declarations to create child classes; if there is a constructor in the subclass the first call is super (and pass super any required parameters)

Note: you cannot extend regular objects, that is done by ```Object.setPropertyOf()```

You can call the methods of the parent class using syntax ```super.method()``` inside the child class

#### Abstract Classes/Mixins

These are templates for classes, multiple inheritance is not allowed in JS, functionality must come from superclass

### [Express Routing](https://expressjs.com/en/guide/routing.html)

Routing is how an applications endpoints respond to requests. Routing methods specify a callback function (handler function). Routes can have multiple callback functions (middleware), so important to implement ```next()``` to move along the chain.

Express supports routes correspoding to all HTTP verbs. Here are two things:

- ```app.use()``` will apply middleware to any request
- ```app.all('/someUrl')``` will apply to all matching the url
- route paths can use strings, string patterns, or regex (uses [path-to-regexp](https://www.npmjs.com/package/path-to-regexp))
- **note: query strings are not part of route path**

#### Route Params

Named URL segments that populate the ```req.params``` object

You can use hyphen and dot in some interesting ways:

- ```/flights/:from-:to``` will interpet flights/LAX-SFO
- ```/plantae/:genus.:species``` will interpet /plantae/Prunus.persica
- you can append regex to the strings to have more control, i.e. ```/user/:userId(\d+)```

#### Response Methods

You have a selection of response methods, if you do not call one in the route handler, the client request will not terminate

Method | Functionality | Method | Functionality
------ | ------------- | ------ | -------------
```res.download()``` | prompt file download  | ```res.end()``` | ends response process
```res.json()``` | sends JSON | ```res.jsonp()``` |  sends json with JSONP support
```res.redirect()``` | redirects | ```res.render()``` | renders view template
```res.send()``` | sends response | ```res.sendFile()``` | sends file
```res.sendStatus()```  | sets status code and sends string representation to ```res.body```

#### ```app.route()```

Use this when you want to chain types of verbs to teh same endpoint:

``` JavaScript
  app.route(/someEndpoint)
    .get()
    .post()
    .put()....
```

#### ```express.Router()```

Creates modular, mountable route handlers that you can import into your apps

### [ExpressJS 4.0 Router](https://scotch.io/tutorials/learn-to-use-the-new-router-in-expressjs-4)

What exactly is the Express router? It is a mini express application, it is just the routing.

You make the router, apply the routes to it, and then call ```app.use('/', router)```; it is possible to make multiple instances of the router and apply them accordingly.

The order that you place your middleware is important, they get called in order.

Express has ```.param()``` middleware - this creates middleware that will run for a certain route parameter like so:

``` JavaScript
  router.param('nameOfParam', function(req,res,next){
    //do something
    next();
  })
```

This is useful to validate data comin in or validate token to authenticate a user

```app.route()``` is like a shortcut to call the express router