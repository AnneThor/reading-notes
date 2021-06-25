# Redux: Asynchronous Actions

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[Redux Tutorials: Async Logic & Data Fetching](#async-data-fetching)

[npm: Redux Thunk](https://github.com/reduxjs/redux-thunk)

[Digital Ocean: Understanding Async Redux Actions](#async-redux-thunk)

## Review, Research, and Discussion

### How granular should your reducers be?

Well, they should not be ambiguous and they need to maintain immutable state.

### Pro or Con – multiple reducers can “fire” when a commonly named action is dispatched

It is a pro if you want that functionality and a con if it happens unexpectedly. The ability to do that makes planning the app more simple, but I could see that getting out of hand if things weren't planned out properly and there was a team of devs trying to coordinate.

### Name a strategy for preventing the above

Just decide in advance if you're going to pursue that functionality or not and be explicit when you're drawing the wireframes for the app so there isn't anything left to chance.

## Vocabulary Terms

Term | Definition
---- | ----------
Store | The react store where the main state object of the program is maintained and where references to it are directed to different components in the app
Combined Reducers | This is when you have different reducers handling different "slices" of state, and then they rejoin in the store to make one unified state object

### [Async Data Fetching](https://redux.js.org/tutorials/fundamentals/part-6-async-logic)

Redux store is oblivious to async logic, it only synchronously dispatches actions and updates the state by calling the reducer functions, then notified the UI about changes.
- Do not put side effect causing behaviors inside a reducer
- Redux middleware are intentioned to handle side effect causing behavior

Middleware can do anything:
- can cause side effects
- can accept params that are not just a plain object (as long as a plain action object is what reaches the reducer)
- have access to `dispatch` and `getState`

#### Using Middleware to Enable Async Logic

Here's an example of fetching from an API
- This example is very specific and only does one thing
- Better to write all async logic ahead of time separate from middleware

```JSX
const fetchTodosMiddleware = storeAPI => next => action => {
  if (action.type === 'todos/fetchTodos') {
    // Make an API call to fetch todos from the server
    client.get('todos').then(todos => {
      // Dispatch an action with the todos we received
      storeAPI.dispatch({ type: 'todos/todosLoaded', payload: todos })
    })
  }

  return next(action)
}
```

What about passing a **function** to `dispatch` instead of an action object
- middleware could verify if it is function or object and then handle accordingly

```JSX
const asyncFunctionMiddleware = storeAPI => next => action => {
  // If the "action" is actually a function instead...
  if (typeof action === 'function') {
    // then call the function and pass `dispatch` and `getState` as arguments
    return action(storeAPI.dispatch, storeAPI.getState)
  }

  // Otherwise, it's a normal action - send it onwards
  return next(action)
}

// and then the middleware would be used like
const middlewareEnhancer = applyMiddleware(asyncFunctionMiddleware)
const store = createStore(rootReducer, middlewareEnhancer)

// Write a function that has `dispatch` and `getState` as arguments
const fetchSomeData = (dispatch, getState) => {
  // Make an async HTTP request
  client.get('todos').then(todos => {
    // Dispatch an action with the todos we received
    dispatch({ type: 'todos/todosLoaded', payload: todos })
    // Check the updated store state after dispatching
    const allTodos = getState().todos
    console.log('Number of todos after loading: ', allTodos.length)
  })
}

// Pass the _function_ we wrote to `dispatch`
store.dispatch(fetchSomeData)
// logs: 'Number of todos after loading: ###'
```

#### How the data flows in Async Redux activities

UI => Event Handler/Dispatch => Middleware => API (if needed) => Middleware/Dispatch => Store [ Reducer => State ] => UI

#### Redux Thunk Middleware

That is the official implementation of this that allows for writing functions that get `dispatch` and `getState` as arguments
- using this allows using logic without knowing what redux store we're using ahead of time
- available on npm as a package called redux-thunk, need to install that


Integrating `redux-thunk` into program

```JSX
import { createStore, applyMiddleware } from 'redux'
import thunkMiddleware from 'redux-thunk'
import { composeWithDevTools } from 'redux-devtools-extension'
import rootReducer from './reducer'

const composedEnhancer = composeWithDevTools(applyMiddleware(thunkMiddleware))

// The store now has the ability to accept thunk functions in `dispatch`
const store = createStore(rootReducer, composedEnhancer)
export default store
```

Dispatching an action immediately updates the store, so we can also call getState in the thunk to read the updated state value after we dispatch

#####

Updating/saving: we need to make the API call to the server with the initial data, wait for the server to send back a copy of the newly saved data, and THEN dispatch an action with that todo item
- Issue is that if we try to implement that as a thunk, the code that makes the API call doesn't know what the new item is supposed to be
- Solution is to write one function that accepts the new item content, then creates the thunk function so it can use the new data to make the API call
- Outer function should then return the thunk to pass to `dispatch` in the component

The pattern of wrapping something that will get passed to dispatch in a function is called the **action creator pattern**.

Thunk functions can be async or sync - they provide a way to use reusable logic that needs access to dispatch and getState

### [Async Redux Thunk](https://www.digitalocean.com/community/tutorials/redux-redux-thunk)

Redux's actions are sync by default; Redux Thunk is a popular library to handle adding middleware (which handles the async operations)

Redux thunk lets you call **action creators** that return a function instead of an action object; this function receives the store's dispatch method which then dispatches regular synchronous actions once the async ones are completed

Redux Thunk is installed at the index level of your app, making the middleware available throughout
