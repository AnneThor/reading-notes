# Express REST API 

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Name 3 real world use cases where you’d want to change the request with custom middleware

## True or false: The route handler is middleware?

## In what ways can a middleware function end the process and send data to the browser?

## At what point in the request lifecycle can you “inject” middleware?

## What can cause express to error with “Request headers sent twice, cannot start a second response”

## Document the following Vocabulary Terms

Term | Definition
----- | -----------
Middleware | 
Request Object | 
Response Object | 
Application Middleware | 
Routing Middleware | 
Test Driven Development | 
Behavioral Testing | 

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
- static methods/properties: not available to class instances, you would have to call them on the object itself (example below to clarify)
```
class Point {
  constructor(x,y) {
    this.x = x;
    this.y = y;
  }
 }
```


Simple structure:
```
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
    }
}

let square = new Rectangle(4,4);
```
You can alternately use [class expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/class)
```
const MyClass = class [className] [extends otherClassName] {
    // class body
};
```
Why use expresions:
- you can omit the class name 
- you can redfine the class without throwing a SyntaxError


### [Express Routing](https://expressjs.com/en/guide/routing.html)

### [ExpressJS 4.0 Router](https://scotch.io/tutorials/learn-to-use-the-new-router-in-expressjs-4)
