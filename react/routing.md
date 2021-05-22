# React: Routing

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[blog.pshrmn: React Router v4 Tutorial](#react-router)

[React Training: Browser Router Example](#browser-router)

## Review, Research, and Discussion

### Do child components have direct access to props/state from the parent?

No, they do not. They have access to what they are passed and if they are passed functions, for example, they are able to modify the state of the parent component. If they are nested inside a parent component, then they would have access to the same things the parent component had access to.

### When a component “wraps” another component, how does the child component’s output get rendered?

```JSX
<Main>
  <Content />
</Main>
```

Those things will just render normally, Content is part of the ```props.children``` of the Main elmeent.

### Can a component, such as <Content />, which is a child also be used as a standalone component elsewhere in the application?

There is nothing explicit about that Component that makes it non reusable so long as it receives whatever it needs to receive in props to have enough data to properly render.

### What trick can a parent use to share all props with it’s children

Have them render inside it's own open/close tag.

## Vocabulary Terms

Term | Definition
---- | ----------
props.children | This refers to any elements that you include in the body of a React Component open/close tag set
composition | Refers to how to structure and build a React Component and how it will interact with props/state/other elements

### [React Router](https://blog.pshrmn.com/simple-react-router-v4-tutorial/)

Pieces of React router

1. The Router

Browser based projects should use `<BrowserRouter>` and `<HashRouter>` components

- Browser Router when you have a server that will handle dynamic requests
- Hash router is for static websites (only respond to files it knows about)

2. History

Each creates a history object which it uses to render current location and re-render when there are changes. Because the website is relying on this history object, every piece that needs to be re rendered must be inside a Router Component or this won't work.

3. Rendering a <Router>

Suggestion is to wrap the entire app in a <BrowserRouter> object since it expects to have only one chile like so

```JSX
import { BrowserRouter from 'react-router-dom' }

ReactDOM.render((
  <BrowserRouter>
    <App />
  </BrowserRouter>
  ), document.getElementById('root'))
```

4. The <App>

Header contains nav links, <Main> contains the rest of the content

5. Routes

<Route> component is the main building block of React Router, this is where any content that is coupled to location's pathname should be placed

- PATH: <Route> Objects expect a path prop (string description of pathname) - components inside this route will only render when the pathname is matched
- <Route path="..."> will match something like path="/path*/"
- <Route exact path="/path" will only match the exact path
- When matching paths, React does not care about params/queries

- MATCHING PATHS is done by regex, when there is a match, a match object will be created with the following properties

Property | Value
-------- | -----
`url` | the matched part of the current location's pathname
`path` | the route's path
`isExact` | `path === pathname`
`params` | an object containing values from the pathname that were captured by `path-to-regex`

- Creating routes: can be done anywhere inside the router, but makes sense to render them in the same place; can use `<Switch>` component to group <Route>s. <Switch> will iterate over children elements (routes) and only render the first one that matches the current pathname

6. Route Rendering

Here is what the `<Route>` has as props

- `component` A React Component. When a route with a component prop matches, the route will return a new element whose type is the provided React Component
- `render` will be called when a path matches
- `children` a function that returns a React element, unlike the two above, this always renders decoupled from path match; better not to use thi sin most cases

In most cases `component` or `render` should be used.

Here is the example:

```JSX
<Route path='/page' component={Page} />
const extraProps = { color: 'red' }
<Route path='/page' render={(props) => (
  <Page {...props} data={extraProps}/>
)}/>
<Route path='/page' children={(props) => (
  props.match
    ? <Page {...props}/>
    : <EmptyPage {...props}/>
)}/>
```

7. Main Element

Now to actually render our routes, we put them inside a switch in the main component like so:

```JSX
import { Switch, Route } from 'react-router-dom'
function Main() {
  return (
    <main>
      <Switch>
        <Route exact path='/' component={Home}/>
        <Route path='/roster' component={Roster}/>
        <Route path='/schedule' component={Schedule}/>
      </Switch>
    </main>
  );
}
```

8. How to handle params in routing

So within the routes, you can further define exact matches against matches with some extra info (params, etc)

```JSX
function Roster() {
  return (
    <Switch>
      <Route exact path='/roster' component={FullRoster}/>
      <Route path='/roster/:number' component={Player}/>
    </Switch>
  );
}
```

If you do it like this, it is cleaner than having ALL of that in the main switch

Params are stored like:

match.params.param, for example:

`/roster/6` will generate a params object `{ number: 6 }`

You can then make API calls to get the info you want using the params in the rendered link

9. Links

How to navigate between pages? Anchor elements cause the whole page to reload, so use <Link> components:

`<Link to={{ pathname: 'roster/7' }}>Player #7</Link>`

Currently the LINK pathname must be absolute

### [Browser Router](https://reactrouter.com/web/api)

Documentation for Browser Router!
