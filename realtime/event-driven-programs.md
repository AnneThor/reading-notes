# Event Driven Programming

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[Digital Ocean: Event Driven Programming in Node](#event-driven-programming-node)

[Node: Events Documentation](#node-documentation)

## Review, Research, and Discussion

### Why is access control important?

Access control is important to limit the control users have over changing the content of a webpage or accessing unauthorized information.

### Describe an application that would need access control.

Most applications that have user signup and signin capabilities require access control. For example, I have sold things on ebay since I was a teenager. In my interactions with this app I have been a simple buyer and a seller. As a user it is not desirable for me to have access to updating the listings of other users, so my role should be restricting to updating/editing records that myself have created (likely there are multiple layers of capability nested therein). At the same time, as a seller I want the sales portfolio section of my account to be limited to my private view.

### What is a role used for?

Roles are pre defined access levels that are used to determine which capabilities different users will have. Roles are generally assigned when the users are created and are meant to be inflexible (to avoid having an infinite number of hugely nuanced capability sets). The goal is to give users the minimal capabilites they need to meaningfully interact with the software.

### Why is role based access control more scalable than discretionary or mandatory access control?

It is scalable because the idea is to predefine role sets and be inflexible with them - as you scale up to a greater number of users, the roles and associated capability sets remain the same.

## Vocabulary Terms

Term | Definition
---- | ----------
Authorization | Permissions needed by the user to access certain content (Basic authorization are login credentials, bearer authorization is a token given to a user after successful login that is used to restrict/direct traffic in a website)
Role Based Access Control | Roles are assigned to users, each role has a pre defined set of minimal capabilties needed for successful interaction with the program
Capabilities | Powers such as read, create, update or delete that are assigned to various roles in RBAC; users can have multiple capabilities and multiple roles can have access to the same capabilites

### [Event Driven Programming Node](https://www.digitalocean.com/community/tutorials/nodejs-event-driven-programming)

Event driven programming makes uses an **Event Handler** (i.e. a callback function that happens when an event is triggered) and a **Main Loop** that listens for event triggers and calls associated events.

#### EventEmitter 

EventEmitter is a native Node.js module that allows to integrate event driven programming (npm packages for faster event emitters exist if needed, EventEmitter2 and EventEmitter3). EventEmitter is accessed through the ```events``` module and needs to be imported and instantiated:

``` JavaScript
const EventEmitter = require('events').EventEmitter;
const myEventEmitter = new EventEmitter;
```

Then you can write functions using your event emitter:

``` JavaScript
const EventEmitter = require('events').EventEmitter;
const chatRoomEvents = new EventEmitter;

function userJoined(username) {
  // assuming a function exists to alert all users
  alertAllUsers('Use ' + username + ' has joined the chat.');
 }

// Run the userJoined function when a 'userJoined' event is triggered 
chatRoomEvents.on('userJoined', userJoined);

// We need the chat room to emit the event to be sure that it is 
// called when someone joins the room
function login(username) {
  chatRoomEvents.emit('userJoined', username);
  }
```

#### Removing Listeners


Reasons to want to remove an event listener from an event: 

- Performance reasons (event no longer needed)
- Avoid memory leaks (event listener references an object that is no longer needed but prevents garbage clean up from collecting it)

To remove event listeners you can:

- ```removeListener``` or ```removeAllListeners```
- Note that you must pass a reference to the exact function that you wish to remove when using these, so it is best practice to name your event handlers and declare them before registering the event listener (as opposed to leaving them anonymous)

Here's an example to make that clear:

``` JavaScript
const EventEmitter = require('events').EventEmitter;
const chatRoomEvents = new EventEmitter;

function displayMessage(message){
  document.write(message);
}

function userJoined(username){
  chatRoomEvents.on('message', displayMessage);
}

chatRoomEvents.on('userJoined', userJoined);

// to remove the displayMessage function from the message's event list
chatRoomEvents.removeListener('message', displayMessage);
```

#### EventEmitters and OOP

Objects are responsible for themselves and interacting with other objects; idea is that Event-driven programming can reverse the communication flow between objects, creating a system where outjects emit events and other objects that are told to listen can respond accordingly (making the object behavior now entirely self contained).

### [Node Documentation](https://nodejs.org/api/events.html)

All objects that emit events are instances of EventEmitter class; they expost an eventEmitter.on() function that allows one or more functions to be attached to named events emitted by the object

When the EventEmitter object emits an event, all functions attached to the event are called synchronously, any values returned by the called listeners are ignored and discarded

**Be mindful with arrow functions, if you use them when you are making event listeners, ```this``` keyword will no longer reference the ```EventEmitter``` instance**

#### Asynchronous vs Synchronous

```EventEmitter``` calls the events synchronously in teh order they were registered to avoid race conditions/logic errors

- When appropriate, listener events can switch to async using ```setImmediate()``` or ```process.nextTick()```

#### Handling events only once 

Default is to calls the listener every time the event is emitted; you can make it once using ```EventEmitter.once()```

#### Error Events 

If an error happens, error will be emitted and will stop the process, so you need to write error handlers
