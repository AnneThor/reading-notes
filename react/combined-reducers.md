# Redux: Combined Reducers

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[Video: Multiple Reducers](#multiple-reducers)

[Redux: Using `combineReducers`](#combineReducers)

[Redux: combine reducers](#combine-reducers)

## Review, Research, and Discussion

## Why choose Redux instead of the Context API for global state?

It is a more directed state management system. Context API is good for global variables such as display ones that you do not want to have to pass specifically through many components just to share an unchanged value. The state management system is to allow different components to update state directly without convoluting logic by passing the functions up and down via many components.

## What is the purpose of a reducer?

Reducers are intended to update the state but to maintain it as immutable (meaning you're always replacing state with a new copy of itself and not overwriting variables directly).

## What does an action contain?

An action contains the action keyword (that will match in a switch statement) and the payload (so whatever information you may need to pass as params to the function that does the action).

## Why do we need to copy the state in a reducer?

Because state is immutable and we do not update it directly, we are always making a copy and saving the new copy. That allows us to preserve "history" and helps to avoid race conditions and undesirable side effects.

## Vocabulary Terms

Term | Definition
---- | ----------
Immutable State | means we do not overwrite state, we replace it with a new copy with the shallow changes we make in any small operation
Time Travel in Redux | allows you to visit previous state (since state is immutable you can access previous iterations)
Action Creator | That is a function that assigns a new action
Reducer | The function that actually updates the state
Dispatch | Method that sends an action, using the observer pattern, we are signaling to components subscribed to that action that they should fire the internal corresponding logic

### [Multiple Reducers](https://www.youtube.com/watch?v=gBER4Or86hE)

- Do not ever update state directly
- Reducers MUST return something, so always return state as the default

### [combineReducers](https://redux.js.org/recipes/structuring-reducers/using-combinereducers/)

This is simply a HOF that takes an object full of slice reducer functions and returns a new reducer function
- `combineReducers` is a utility function to simplify the most common use case of using slice Reducers
- `combineReducers` does not call every every redcuer when dispatching an action, but it does call each slice reducer to reassemble the tree

`createStore` takes `preloadedState` as second argument that is primarily intended to initialize state with stored information (from local storage or elsewhere)

###[Combine Reducers](https://redux.js.org/api/combinereducers/)

Point of `combineReducers` is to turn an object whose values are smaller reducing functions into a single function to pass to `createStore`
- Inside state, keys are functioned as the names you pass into `combineReducer` i.e.

```JSX
combineReducers({ todos: myTodosReducer, counter: myCounterReducer })
//shapes state to { todos, counter }
```

There are some rules about the reducers that you pass in:

1. For actions that aren't recognized you need a default that just returns `state`
2. Must never return `undefined`
3. You must handle the case that your reducers receive `undefined` or the `combineReducers` function will throw

**Important points**
- This was just designed to handle the most general case, you can write your own `combineReducers` functions to handle more complicated logic
- `combineReducers` can be called at any level in the hierarchy, not required to occur at the top
