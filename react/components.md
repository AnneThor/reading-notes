# React: Component Composition
**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[freeCodeCamp: React Concepts to Know After the Basics](#react-concepts)

[React: Composition vs Inheritance](#composition-vs-inheritance)

[codeburst.io: Props.Children Intro](#props-children)

[Testing Library: React Testing Example](#react-testing-example)

## Review, Research, and Discussion

### Can a parent component access the state of a child component?

No, information flow in React is one directional. State is held in the parent component, but the child components can be passed state variables as well as functions that update the state variables (these functions live in the parent component and are passed as props).

### What can be passed along in a prop variable?

You can primitive values, React elements, or functions.

### How can a child component “know” the state of another component?

State (and all class methods and variables for that matter) are private to their components. There is no way for components to "know" anything about other components that is not passed to them as props. So that is how they must communicate.

## Vocabulary Terms

Term | Definition
---- | ----------
Component Props | Component props are the properties (values and functions) that are passed to the component at instantiation by their parent component
Component State | Component state is a special attribute that a component holds, this is immutable and can be passed to children components. Components can also have attributes that are not part of the "state" variable, these are just things that you would only need inside the given component.
Application State | That is where the application stores the data that is needs to reference to properly display/operate

### [React Concepts](https://www.freecodecamp.org/news/these-are-the-concepts-you-should-know-in-react-js-after-you-learn-the-basics-ee1d2f4b8030/)

#### Component Lifecycle

1. Render phase (pure and has no side effects, can be paused/aborted/restared by React)

- Mounting, constructor
- Updating: getDerivedStatedFromProps, shoudlComponentUpdate, render

2. Pre commit phase (cannot read the DOM)

- Updating: getSnapshotBeforeUpdate
- Mounting/Updating: react updates DOM and refs

3. Commit Phase (can work with DOM, run side effects, schedule updates)

- Mounting: componentDidMount
- Updating: componentDidUpdate
- Unmounting: componentWillUnmount

Stage of Lifcycle | Associated Methods
----------------- | ------------------
**Mounting** | Components run the constructor and initialize component state, render (which returns the JSX), now React "mounts" onto the DOM, componentDidMount (makes async calls to DB or directly manipulates DOM if needed)
**Updating** | This phase is triggered every time state/props change and components are perpetually here until they are unmounted, shouldComponentUpdate (compares old/new props and state to determine if update is needed, you can control re rendering by returning T/F), if there is a re rendering, getSnapshotBeforeUpdate then componentDidUpdate (which you can use to make any async calls or manipulate the DOM)
**Unmounting** | Last stage of lifestyle, clean up open connects like websockets or intervals
**Others** | forceUpdate (forces a rerender and should typically be avoided), getDerivedStateFromError (in the event of an error, this runs and you can update state to reflect an error occurred, this method should be used a lot!)


#### Higher Order Components (HOC)

"A higher-order component is a function that tkes a component and returns a new component", what they actually do is allow abstraction of shred logic between components into a single overarching component.

- A good use case is auth (since the logic will be needed in many components)
- The example is wrapping any component in an ```AuthWrapper``` that will either render the component (if loggedIn state is true) or render a message indicating that the user is not logged in

#### React State and setState()

When you use ```setState()``` React will re render the component every time unless you explicitly tell it not to do so using `shouldComponentUpdate`

- `setState()` is async, but it can take a callback function (which should allow you to be able to register the udpate if you're trying to console.log it right away or somethign similar to that)
- when you are using the current state to update the state, the best practice is to use a function

```JSX

this.state = { counter: 0 }

onClick = () => {
  this.setState((prevState, props) => {
    return ({ counter: prevState.counter + 1 })
  }, () => console.log("callback: " + this.state.counter)
  )
  console.log("after: ", this.state.counter)
}
```

In that above example you will see that "after: 0" and "callback: 1" (so you need to put the console.log inside the callback for it to immediately register).

Passing as functions is also beneficial becuase:

- you get a static copy of the state as it was when you invoked the function (therefore avoiding mutations that are possible through other `setState` calls elsewhere) 
- `setState` calls will be queued so that they run in order

#### React Context 

React context API gives you global context objects that can be given to any component (allowing you to share data without having to pass it all the way down through the DOM tree)

#### React is Fluid

React is changing and each release contains significant developments, some examples

- 16.3 deprecated some lifecycle methods
- 16.6 introduced async components
- 16.7 introduced hooks (which aim to replace class components entirely)

### [Props Children](https://codeburst.io/a-quick-intro-to-reacts-props-children-cb3d2fce4891)

```this.props.children``` or ```props.children``` (stateless functions)

What ```props.children``` does is display whatever you include between opening/closing tags when invoking a component. You would use this when you don't make a self closing tag, so then you can insert whatever code you want in between. Here is an example:

```JSX
const Picture = (props) => {
  return (
    <div>
      <img src={props.src}/>
      {props.children}
    </div>
  )
}

render () {
  return (
    <div className='container'>
      <Picture key={picture.id} src={picture.src}>
          //what is placed here is passed as props.children  
      </Picture>
    </div>
  )
}
```

So this is a bit like anonymous classes that you can extend to make them do anything you want. You can pretty easily put together various ```<Picture />``` elements in the above example with diffrent functionality while reusing the same code.

### [Composition vs Inheritance](https://reactjs.org/docs/composition-vs-inheritance.html)

Recommend using composition vs. inheritance to reuse code between components. Here are the common issues.

#### Containment 

Components don't know necessarily know their children ahead of time (esp for things like sidebars or dialog boxes); these shoudl use the special ```children``` prop to pass children elements directly into their output.

#### Specialization 

Sometimes components are "special cases" of other components (here is where you're likely to try to use inheritance) 

### [React Testing Example](https://testing-library.com/docs/react-testing-library/example-intro/)
