# React: Props and State

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[CSS Tricks: Understanding `setState`](#set-state)

[React: Handling Events](#handle-events)

[React: Forms](#forms)

[React: State and Lifecycle](#state-and-lifecycle)

[React: Components and Props](#components-and-props)

[Thomas Lombart Blog: How to Test React Apps](#how-to-test-react)

## Review, Research, and Discussion

### Does a deployed React application require a server?

Yes, a deployed React application requires a server.

### Why do we prefer to test a React application at the behavior rather than the unit level?

Because we are testing that our react components are behaving in the ways we expect them to, if you decouple that and just test that the underlying functions are working properly you may have false positive/false negatives becuase you are not testing how the app is actually wired together.

### What does npm run build do?

```npm run build``` creates a ```build``` directory that is ready to be pushed to a server - it doesn't include all the testing and other extra files and it minifies your code so it will load faster.

### Describe the actual composition / architecture of a React application

If you run the command ```create-react-app``` this will create a project scaffolding based on node and javascript that is configured to handle files written in JS/JSX and use React. You will have the main index.html page that displays the SPA and that will link to a root node that imports your App.js file. So you develop from the App.js file and link other componenets there. 

## Vocabulary Terms

Term | Definition
---- | ----------
BDD | Behavior Driven Development, refers to iterating based on the expected/desired behavior of the application and encourages a collaborative approach
Acceptance Tests | Are written by non coders who are trying to be sure that the program meets the behavior goals they have in mind (vs. coders testing expected outputs)
Mounting | Mounting is when the DOM element is set up/loaded for the first time; unmounting will be when it is removed from the DOM
Build | Refers to the production build of the app where it is minified and all extra whitespaces removed to make loading as fast as possible (other non essential things like dev dependencies/tests are removed here as well)

### [Set State](https://css-tricks.com/understanding-react-setstate/)

React components render UI based on the state, when the state of the components change, the component UI changes

```setState()``` si the only way to update state after initial setup

This is done with a process called **reconciliation**. When the request is triggered React makes a new tree wtih the reactive elements in the component along with updated state. It will only update parts of the DOM that are different. 

If you overwrite state directly, you will bypass React's internal mechanism to refresh this part of the page.

When you call set state, you cannot call it repeatedly on the same property in the same function (that is because you will be referencing the original value in each subsequent call so only the last call will persist). You CAN pass functions to set state that is the workaround.

Bad example: (will return count = 2)

```JSX
let count = 1;
handleIncrement = () => {
  this.setState({ count: this.state.count + 1 })
  this.setState({ count: this.state.count + 1 })
  this.setState({ count: this.state.count + 1 })
}
```

Good example: (will return count = 4)

```JSX
let count = 1;
handleIncrement = () => {
  this.setState((prevState) => ({ count: prevState.count + 1 }))
  this.setState((prevState) => ({ count: prevState.count + 1 }))
  this.setState((prevState) => ({ count: prevState.count + 1 }))
}
```

#### Access Previous State Using Updater

An updater allows you to access teh current state nd to put it to use immediately to update other items

```JSX
changeCount = () => {
    this.setState(prevState) => {
        return { count: prevState.count -1 }
    }
}
```

This does not depend on the result of ```this.state``` but builds the ```count``` variable on itself 

**```setState()``` should be treated asynchronously, do not expect that the state has changed after calling it immediately!**

#### Main Points 

- You  must update state with ```setState()```
- You can pass an object or a function to ```setState()```
- Pass a function when you need to do multiple updates to state 
- Do not depend on ```setState()``` result immediately due to async nature (you should use updater function instead)

### [Handle Events](https://reactjs.org/docs/handling-events.html)

Similar to handling DOM elements, with some differences

- JSX uses camelCase; pass functions to event handler rather than a string 
- you cannot return ```false``` to prevent default behavior in React, must call ```preventDefault``` explicitly
- events in React are ``SyntheticEvent```s and work a ltitle different 

The way classes work in JS ES6 is that methods are NOT automatically bound, so you need to bind them before you pass them along 

#### Passing Aruments to Event Handlers

These two things are equivalent: 

```JSX
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

In both cases the ```e``` argumetn is passed after the id, with an arrow function you have to pass that explicitly, but if you bind it, it gets passed automatically

### [Forms](https://reactjs.org/docs/forms.html)

For html submits, the default behavior is browsing to a new page where the user submits the form; React works the same, but usually you add a JS function to access the data inside the form

**Controlled Components** 

HTML form elements typically maintain their own state based on user input; in React, mutable state is kept in the state property and updated with ```setState()```

Here's how to handle this for **input** tags

- Make React the single source of truth
- An input form form whose value is controlled by react state is called **controlled component**

This allows you to pass the values around and control them from other functions/elements 

Here's how to handle that for **textarea** tags

- In HTML ```<textarea>``` elemnts define the text by their children 
- In React the ```<textarea>``` elemnt instead uses a ```value``` attribute 

This basically works the same way, you can initiate it with some default text by having the state start off with some default message

Here's how to handle that for **<select>** tags

- In HTML they make a dropdown menu
- In React, you don't have a "selected" value that makes the default selection, instead you have a value attribute on the root select tag that handles that

So all that means is that you set the default selection by having the state begin with that value selected

Here's how to handle **MULTIPLE INPUTS**

- Add a ```name``` attribute to each element and let the handler function decide what to do with them by using ```event.target.name```

**Specifying the value on a controlled component prevents the user from changing the input unless you want them to do so**

### [State and Lifecycle](https://reactjs.org/docs/state-and-lifecycle.html)

#### State 

Fully controlled and private to the component

#### LifeCycle Methods to a Class 

**Note that you can have class attributes that are not part of state**

We need to free resources taken by components when they are destroyed.

Mounting = setting up the DOM element for the first time; Unmounting = clearing the elements when they are removed

```componentDidMount()``` runs after the component output has been rendered to the DOM 

```componentWillUnmount()``` cleans up afte ra component 

#### Using State Properly

1. Do not modify state directly
2. State updates may be asynchronous, and you cna handle that like this

```JSX
this.setState((state, props) => {
    counter: state.counter + props.increment
})
```

3. State updates are merged (meaning you can just update one attribute at a time)

**Data flows DOWN, each component owns it state and can pass it down, but they can not pass upwards**

### [Components and Props](https://reactjs.org/docs/components-and-props.html)

Components are like JS functions, they accept arbitrary inputs (props) and return react elements describing what to display

There are function and class components.

Always start custom components with a capital letter, React treats those starting with lowercase letters as DOM tags.

**Props are READ ONLY** - a component must never modify it's own props

**All React components must act like pure functions with respect to their props**

This means soemthing like:

```JSX
function withdraw(account, amount) {
    account.total -= amount;
}
```

is impure becuase it is changing it's own props.

### [How to Test React](https://thomaslombart.com/beginner-guide-testing-react-apps/)

This article is about using **react-testing-library** that is based on testing implementation details.

These tests are based on the **Arrange, Act, Assert** principles.

1. Setup code in the tests so everything is ready to be tested
2. Act -> implement the actions you are testing
3. Assert the outcomes 