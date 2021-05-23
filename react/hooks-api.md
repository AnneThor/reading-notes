# React: Hooks API

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[Dan Abramov: Making Sense of React Hooks](#react-hooks)

[React: Using the State Hook](#state-hook)

[React: Hooks API Reference](#hooks-api-reference)

[React: Hooks at a Glance](#hooks-overview)

[React: Using the Effect Hook](#effect-hook)

## Review, Research, and Discussion

### Why do we not need more .html pages in a multi-page React app?

There is one single homepage that is rendering the div with the "root" id, which corresponds to the App.js file of your React app. This is like a shell that contains all the nested parts of your app, so the different components rendering are what controls what is actually getting displayed in the html document.

### If we wanted a component to show up on every page, where would we put it and why?

- Outside the `<BrowserRouter/>`
No, putting it outside the `<BrowserRouter/>` will not have the desired effect and will be separate from the conditional rendering logic that is implicit to BrowserRouter.

- Inside the `<BrowserRouter />`, outside a `<Route />`
Yes, putting it here will include it on every page, it will just be non conditional on the route that is clicked

- Inside a `<Route />`
Inside a route will make the rendering of the component conditional on the link that is currently clicked.

### What does props.children contain?

All elements that are inside the open/closing tags of the parent element.

## Vocabulary Terms

Term | Definition
---- | ----------
Composition | Refers to how to structure and build a React Component and how it will interact with props/state/other elements
Children / Child Components | Components that are nested within an open/closing tag of a react component
Hash Routing | Hash routing is for pages that ONLY render internal pages (will only respond to files they know about)
Link Routing | Routing that is by the html path (handles dynamic requests)

### **[React Hooks](https://medium.com/@dan_abramov/making-sense-of-react-hooks-fdbde8803889)**

Issue without hooks is there is difficulty in separating concerns due to the way information flows unidirectionally in React. Resultant problems are:

- Huge components
- Duplicated logic
- Complex patterns (like render props and higher order components)

Hooks offer a solution to these issues. Hooks apply explicit data flow and composition inside a component and avoid unnecessary nesting.

Hooks does not cause bloating and should result in ultimately smaller minified code due to reduction in class components.

#### What are Hooks?

There are lots of ways to reuse logic in React Apps, problem with components is that they have to render something.

Hooks let you use React features (like state) from a function by doing a simple function call. React has the built in lifecycle hooks. Hooks are regular JS functions, you can combine built in Hooks provided by React with "custom Hooks."

Goal of hooks is to make components truly declarative even if they contain state and side effects. Here is an example that uses the window with and re renders the component if the viewport changes:

```JSX
function MyResponsiveComponent() {
  const width = useWindowWidth(); // the custom hook
  return (
    <p>Window width is {width}</p>
    )
}
```

How to use this? Use the *React local state* to keep the current window with and use a *side effect* to set that state when the window resizes:

```JSX
import { useState, useEffect } from 'react';

function useWindowWith() {
  const [width, setWidth] = useState(window.innerWidth)

  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);
    window.addEventListener('resize', handleResize);
    return () => {
      window.removeEventListener('resize', handleResize)
    };
  });

  return width;
}
```

So you can see this is built off the built in hooks (useState, useEffect) and that each time a hook is called it gets isolated local state within the currently executing component (does not disturb top down data flow).

#### Relation to Classes

For hooks to work React needs to provide a way for functions to declare state and side effects, that is what `useState` and `useSideEffect` do.

#### How Hooks are Working

Under the hood, hooks are essentially kept in a list, and that exists per component. When a hook is called, React moves to the next hook in the list. The order is the same on every render, so the component has the correct state for each call.

### **[State Hook](https://reactjs.org/docs/hooks-state.html)**

Hooks do not work inside class components, you can only use them in function components.

**What is a hook?**

A hook is a special function that lets you "hook into" React features, for example `useState` is a hook that lets you add react state to function components

**When to use a hook?**

If you write a function component and you need to add some state to it, you used to have to convert it to a class, now you can just use the hook

#### **Declaring a State Variable**

In a function component we have no `this` so we instead use the `useState` hook to access state

```JSX
import React, {useState} from 'react'

function Example() {
  // declare a new state variable
  const [count, setCount] = useState(0)
}
```

- This declares a state variable `count`; normally variables "disappear" when the function exits, but state variables are preserved by React
- The only argument to the `useState()` hook is the initial state, the state does not need to be an object; it can be just a number or a string or whatever you need
- If you wanted two values, call `useState()` twice
- `useState` returns a pair of values: the current state and the function that updates it.

This state variable persists through re-renders, so when you update it, it will remain current

#### Reading state

To display in a class component: `{this.state.count}`

To display in a function component: `{count}`

#### Updating State

In a class component: `{this.setState({ count: this.state.count + 1 })}`

In a function component: `{setCount(count+1)}`

**Note: in comparison to `this.setState` using the hooks version always REPLACES the previous value rather than merging it, so just be careful if you're using objects that you use a spread or something so you don't lose data**

```JSX
const [state, setState] = useState({});
setState(prevState => {
  // Object.assign would also work
  return { ...prevState, ...updatedValue }
  })
```

### **[Hooks Overview](https://reactjs.org/docs/hooks-overview.html)**

React assumes that if you are calling `useState` many times that you will do so in the same order during every render

#### Effect Hook

This comes into play for things like data fetching, subscriptions or manually changing the DOM from components, these things are called SIDE EFFECTS because they can affect other components and cannot be done during rendering.

By default React runs `useEffect` after every render including the first one and they can take an optional clean up

#### Rules of Hooks

Hooks are JS functions, but they have 2 additional rules.

1. Only call Hooks at the top level (i.e. do not put them into loops or nested conditions)

2. Only call Hooks from inside React function components (not from regular JS functions)

#### Custom Hooks

If you want to reuse stateful logic between components you can create custom hooks. You are passing the logic to different components but they are NOT sharing state - they will have individually updated local state that uses the same logic to update.

### **[Hooks API Reference](https://reactjs.org/docs/hooks-reference.html)**

`useEffect`: by default effects run after every completed render, but you can choose to run them only when certain values have changed
- often, effects need a cleanup function to prevent memory leaks before they are re rendered
- `useEffect` fires after layout and paint; `useLayoutEffect` can be used instead to prevent unintended visual consequences
- accept a second param, which is an array of values the effect depends upon; if you pass an empty array that says the effect doesnt' depend on ANY values and so it never needs to re run

### **[Effect Hook](https://reactjs.org/docs/hooks-effect.html)**

Lets you perform side effects in function components

Example:

```JSX
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

Side effects are things like: setting up subscription, data fetching, manually changing the DOM

#### Effects Without Cleanup

Things like network requests, manual DOM mutations, logging do not require cleanup (can be run immediately and disregarded)

Before using lifecycle hooks you would have to use the same logic twice to indicate something before a render, and then again after if the value was changed, i.e.

```JSX
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  componentDidMount() {
    document.title = `You clicked ${this.state.count} times`;
  }
  componentDidUpdate() {
    document.title = `You clicked ${this.state.count} times`;
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

The first example here shows you to do with with hooks, which removes the repetition.

- `useEffect` tells React that your component needs to do something after render; React will remember the function you passed and perform it after DOM updates
- `useEffect` is called INSIDE the component so it can access local state; closures are supported
- `useEffect` will run after EVERY render (but can be customized); React guarantees the DOM has been updated by the time it runs the effects

You pass your effect as a function to `useEffect`

**IMPORTANT**
`useEffect` does NOT stop the browser from updating the screen, this makes the app feel more responsive and it assumes that these things don't need to be run synchronously. If they DO need to run sync you can use `useLayoutEffect` instead.

#### Effects with Cleanup

Things like setting up a subscription to an external data source (need to clean up to avoid memory leak)

With classes you would do something like subscribe in the `componentDidMount` and unsubscribe in the `componentWillUnmount`

This is simplified without the need to mirror logic using hooks:

```JSX
import React, { useState, useEffect } from 'react';

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    // Specify how to clean up after this effect:
    return function cleanup() {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

If your effect returns a function, React will run that when it is time to clean up; the return function is an optional parameter
- Every effect may return a function that cleans up after it
- React performs the cleanup when the component unmounts; but effects run for every render not just once, so React ALSO cleans up from the previous render before re running the effect (you can opt out of that)
- You do not have to return a named function in the cleanup, it can be an arrow function or whatever you want
