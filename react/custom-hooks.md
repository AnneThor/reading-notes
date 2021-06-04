# React: Custom Hooks

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[Kendo React: Custom React Hooks](#custom-react-hooks)

[React: Hooks with Async/Await](#async-await-hooks)

[React: useReducer](#use-reducer)

[React: Build Your Own Hooks](#customized-hooks)

## Review, Research, and Discussion

### What does a component’s lifecycle refer to?

The period of time when it gets add to the DOM to when it gets removed from the DOM is the lifecycle.

### Why do you sometimes need to “wrap” functions in useCallback when called from within useEffect

This helps you avoid an infinite loop scenario where the inner function is triggering a re rendering of the component and then is itself rebuilt over and over.

It can be formatted like

```JSX
function someComponent() {
  const innerFunction = useCallback() => {
    // function does something
  }

  useEffect(() => {
    innerFunction();
    // the effect calls inner function, so calls it as dependency
    // that is to ensure the most recent version of innerFunction (and whatever data it uses) is referenced
    }, [innerFuction])
}
```
### Why are functional components preferred over class components?

They have a more simple format that requires less boilerplate coding and they are streamlined to make accessing and updating state more intuitive. They are also just plain JS functions that return JSX

### What is wrong with the following code?

Re rendering the component over and over again because the for loop is outside of `useEffect`

## Vocabulary Terms

Term | Definition
---- | ----------
State Hook | a hook that updates the state values
Effect Hook | a hook that controls when and how often components re render
Reducer Hook | Similar to effect hooks, but these are mostly used to manage complex state or situations where the nextState depends on prevState

### **[Custom React Hooks](https://www.telerik.com/kendo-react-ui/react-hooks-guide/#toc-custom-react-hooks)**

Hooks are just functions, anything that is a function can be a hook

#### Revisiting the Effects Hook

`useEffect` runs AFTER the component renders (after the first render, and after as many other renders as you indicate)

```JSX
// example without Cleanup
useEffect(() => {
  document.title = `You clicked ${count} times`;
  })

// if you need cleanup that will occur AFTER your effect, but BEFORE any other effects
// example - you need to unsubscribe as part of the cleanup process
// param at the end is optional to update only on changes to that variable
useEffect(() => {
  console.log("Subscribe to something!")
  return function cleanup() {
    console.log("Unsubscribe to Something!")
  }
}, [props.something.id])
```

#### Custom hooks

Are methods that are named with a prepended `use` tag
- HOOKS can call HOOKS => our custom hook can definitely call `useEffect`

Here's how to update that last example
```JSX
const useDocumentTitle = (title) => {
  useEffect(() => {
    document.title = title;
    }, [title])
}

// you could import this into another file like so
import useDocumentTitle from 'hooks/document-title'

function someFunction() {
  useDocumentTitle(`You clicked ${count} times`)
}
```

### **[Async Await Hooks](https://dev.to/vinodchauhan7/react-hooks-with-async-await-1n9g)**

When using `useEffect` you cannot use the `async` keyword because you'll cause a race condition

### **[Use Reducer](https://reactjs.org/docs/hooks-reference.html#usereducer)**

`useReducer` is an alternative to `useState` it accepts a reducer of type `(state, action) => newState` and returns the current state paired with a `dispatch` method

Generally preferred when
- complex state logic involving multiple sub values
- next state depends on previous one

**Note: React guarantees stability, therefore you can omit `useEffect` or `useCallback` from the dependency list**

Here is a counter example using `useReducer`

```JSX
const initialState = { count: 0 }

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 }
    case 'decrement':
      return { count: state.count - 1 }
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return(
    <>
      Count: { state.count }
      <button onClick={ () => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={ () => dispatch({ type: 'increment' })}>+</button>
    </>
    )
}
```

Specifying the initial state:

```JSX
const [state, dispatch] = useReducer {
  reducer,
  { count: initialCount }
}
```

You can initialize the state "lazily" by passing an `init` function as the 3rd argument to `useReducer`

Extracting the logic for calculating initial value can help for resetting in response to later actions

```JSX
function init(initialCount) {
  return { count: initialCount }
}

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    case 'reset':
      return init(action.payload);
    default:
      throw new Error();
  }
}

function Counter({initialCount}) {
  const [state, dispatch] = useReducer(reducer, initialCount, init);
  return (
    <>
      Count: {state.count}
      <button
        onClick={() => dispatch({type: 'reset', payload: initialCount})}>
        Reset
      </button>
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```

If React sees you returning the same value as previously, it won't re render unless prompted to by some other concern

### **[Customized Hooks](https://reactjs.org/docs/hooks-custom.html)**

Two components using the same custom hook do NOT share state
- each CALL to hook gets isolated state, we can call `useState` and `useEffect` many times in one component and they will remain totally independent
