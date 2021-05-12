# React: Component Based UI

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[React: Hello World](#hello-world)

[React: Introduction to JSX](#jsx)

[React: Rendering Elements](#rendering-elements)

## Review, Research, and Discussion

### Name 5 Javascript UI Frameworks (other than React)

Ext JS by Sencha: specialized for managing large amounts of data, paid framework

Angular: free open source by Google, uses TypeScript, full support from Google behind it

Vue: free and open source, template syntax with component based architecture, has good documentation and can be scaled from use as a library to use as a framework

Ember: free and open source, focus on convention and limiting developer decisions  

Svelte 3: unique in that it shifts the work into the compile step (as opposed to in the browser)

### What’s the difference between a framework and a library?

The difference between a framework and a library is **inversion of control**. When you are using a library, you direct when to call the functions of the library and how they will integrate with the rest of your code. When you use a framework, there is **inversion of control**, meaning that the framework controls the project flow and you plug your concerns into it.


## Vocabulary Terms

Term | Definition
---- | ----------
Rendering | Rendering is the compiling of your JSX components into DOM nodes that the browser can parse/display on a screen
Templates | Templates are generic classes or other base unites that can be used for unique units of code; in relation to front end frameworks, templates are general scaffolding of a webpage like header/body/footer combinations (similar to how you can make them in EJS and then extend them as needed)
State | State is where data is held in a React Component Class; the idea is to have state immutable and to be held in as few places as possible to avoid side effects and keep access to state restricted to only parts of the program that truly need to access it.

### [Hello World](https://reactjs.org/docs/hello-world.html)

Elements and Components are the building blocks in React. React is a JavaScript library.

### [JSX](https://reactjs.org/docs/introducing-jsx.html)

JSX combines markup and JavaScript so that React can separate concerns into components (loosely couple units that combine event handling and state changes).

You don't have to use JSX with React, you can use regular JavaScript as wanted/needed.

ANY valid JS can go within the curly braces in JSX.

After compiling JSX becomes regular JS, so you can use JSX inside conditionals and return it in functions.

You can specify attributes with JSX, but remember to either use quotes around literals or curly braces around JSX (don't use both).

Security measures: everything is escaped before it is rendered, so nothing can be added in XSS (cross-site scripting) attacks.

During compilation Babel transforms React.createElement to normally formatted HTML elements.

There is equivalency between these three things, which are "React Elements":

```JSX
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);

const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);

// Note: this structure is simplified
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

### [Rendering Elements](https://reactjs.org/docs/rendering-elements.html)

Elements are the smallest building blocks of React apps and they describe what you want to show on the screen.

React elements are plain objects and are inexpensive.

Elements are NOT components, components are built from elements.

In practice, most React apps only call ReactDOM.render() once and the rest of the behavior is handled in **stateful components**.

React only updates as necessary and compares/looks for changes in the state (so you don't re-render entire pages)

React makes a virtual DOM and uses diffing to update only the parts that have changed due to changes in the underlying state.
