# React: Component Based UI

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[Dan Abramov: Redux Fundamentals](#redux-fundamentals)

[FCC: Redux Guide](https://www.freecodecamp.org/news/understanding-redux-the-worlds-easiest-guide-to-beginning-redux-c695f45546f6/)

[Alex Bachuk: Testing Redux with Jest](https://medium.com/@netxm/testing-redux-reducers-with-jest-6653abbfe3e1)

[Redux Docs](https://redux.js.org/)

## Review, Research, and Discussion

## What are the advantages of storing tokens in “Cookies” vs “Local Storage”

Cookies persist, while local storage does not persist through a page refresh.

## Explain 3rd party cookies.

Cookies stored under a different domain than the user is currently using. They are used to track users across websites. Some good functionality are ability to share web tokens, some potentially bad functionality is ability to share data, track web usage, send annoying ads.

## How do pixel tags work?

Pixel tags are transparent gif images added to a web page that allow reading and placement of cookies. The resulting connection can include information about the user, browser, etc. They are invisible to the user, but are used by companies to track user activities.

## Vocabulary Terms

Term | Definition
---- | ----------
Cookies | Cookies are small blocks of data placed on the user's device during a session that carry information throughout the session. They enable web servers to have stateful information, show browsing activity, and allow data to persist through page refresh, etc. Authentication cookies are common to track if a user is logged in. Tracking cookies are used to log and record user data (usage, page viewing history, details of location/browser, etc).
Authorization | There are different ways to authenticate and track authentication of a user. Basic authorization is the authorization header obtained by successful login with a password. More complex methods use web tokens to create sessions to keep a user logged in for a given period of time and potentially across several web services.
Access Control | System of creating and assigning tasks to different roles that are given to users when they establish their accounts. This limits the ability of the user to interact with the web service to functions predefined in their given role.
Conditional Rendering | Limiting the display of portions of a webpage to depend on the result of some predicate.

### [Redux Fundamentals](https://egghead.io/lessons/react-redux-the-single-immutable-state-tree)

The idea is that the whole website is just one big JS object

You cannot change the state directly, every time you need to adjust it you must dispatch an action. State is read only. An action is a plain JS object, describing in a minimal way the necessary change in the application.

UI should be a pure function representation of the application state, Redux complements this with the idea that state mutations need to be described as pure functions. Take prev state, action and returns next state (that is the REDUCER). Redux is fast because just shallow updates are made to the state.

Convention is that if the reducer function finds something it doesn't understand it should return the current state (default state).

Redux uses "stores" to save data. Store has methods:
 `getState()` - returns current state
 `dispatch()` - fires the reducer that you have assigned to the store
 `subscribe()` - updates the 
