# React: Hooks API

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[React: Context](#react-context)

[Start it up: Snackbars in React: An Exercise in Hooks and Context](#snackbars-in-react)

[Github: Awesome React Context](https://github.com/diegohaz/awesome-react-context)

## Review, Research, and Discussion

### Describe use cases for `useMemo()` and `useReducer()`

Point of `useMemo` is to make the browser faster - if you have some piece of information that is displayed using some computationally expensive process, useMemo can make this happen only when the dependencies have changed (and not on every single render). Point here is to minimize how often expensive operations are called.

Point of `useReducer` is to provide a simple control flow for complicated logic and to help handle scenarios where current state depends on previous state.

I would say that `useMemo` is intended to isolate and minimize expensive processes vs. `useReducer` being a way to simplify control flow.

### Why do custom hooks need the use prefix?

You can actually technically write them without the prefix, but the point is to make sure they are adherent to the React rules for hooks and to be sure that they are called appropriately.

### What do custom hooks usually do?

Custom hooks are used to update state/re render components in response to the expected user interaction with the website. This can be fine tuned to the user experience and can make re rendering/state management less complex.

### Using any list of custom hooks, research and name one that you think will be useful in your applications

There are a lot of interesting custom hooks, I guess my next stop will be `react-use-form-state` since my next task at hand will be to revisit the previous lab and get the form 100% properly working.

### Describe how a hook that fetches API data might work

I got mine working in a way that makes sense to me, I have a separate suite of functions that handle the logic to interact with the API. Then I have hooks inside the component files that use these functions when needed and I can put them inside of functions ("custom  hooks") that allow me to manipulate when they are called without a lot of clutter.


## Vocabulary Terms

Term | Definition
---- | ----------
reducer | In general, a reducer function is usually something that takes multiple inputs and returns one value, not sure what you're looking for me to say in this context, I guess that is somewhat analogous to using a switch statement to distill multiple possible options into one outcome

### **[React Context](https://reactjs.org/docs/context.html)**

Context offers a way to pass data through the component tree laterally. Idea is to use them for things that are relevant everywhere

- Currently authenticated user
- Theme
- Preferred language

Using Context, you can avoid having to explicitly pass through components

```JSX
// Context lets us pass a value deep into the component tree
// without explicitly threading it through every component.
// Create a context for the current theme (with "light" as the default).
const ThemeContext = React.createContext('light');

class App extends React.Component {
  render() {
    // Use a Provider to pass the current theme to the tree below.
    // Any component can read it, no matter how deep it is.
    // In this example, we're passing "dark" as the current value.
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

// A component in the middle doesn't have to
// pass the theme down explicitly anymore.
function Toolbar() {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class ThemedButton extends React.Component {
  // Assign a contextType to read the current theme context.
  // React will find the closest theme Provider above and use its value.
  // In this example, the current theme is "dark".
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}
```

This is really only intended for use for global concerns, if you're just needing to pass through several items, you could consider **passing the entire component** as props.

You can also pass multiple children along with a prop like so

```JSX

function Page(props) {
  const user = props.user;
  const content = <Feed user={user} />;
  const topBar = (
    <NavigationBar>
      <Link href={user.permalink}>
        <Avatar user={user} size={props.avatarSize} />
      </Link>
    </NavigationBar>
  );
  return (
    <PageLayout
      topBar={topBar}
      content={content}
    />
  );
}

```

### **[Snackbars in React](https://medium.com/swlh/snackbars-in-react-an-exercise-in-hooks-and-context-299b43fd2a2b)**

A snackbar is just one of those little updates that briefly pops up in response to a user activity.

Idea is to implement them using a hooks

```JSX

// this is the idealized implementation

const { addAlert } = useSnackBars()
...
addAlert('Your profile is updated!')

```

State of visible snackbars can be localized to a single centralized component. Management of that state needs to be **globally** accessible. That is what context lets you do

```JSX

// this is a naive, draft example
// Context provider is now responsible for displaying the local state and exposing an API for globally managing them

export const snackBarContext = createContext();

export function SnackBarProvider({ children }) {
  const [alerts, setAlerts] = useState([])

  return (
    <SnackBarContext.Provider value={{ setAlerts }}>
      {children}
      {alerts.map((alert) => <SnackBar key={alert}>{alert}</SnackBar>)}
    </SnackBarContext.Provider>
    )
}

```

Now we just need a consumer for this provider. This can be implemented by wrapping the hook in `useContext` React hook:

```JSX

import { useContext } from 'react'

import { SnackBarContext } from 'components/snack-bar-provider'

const useSnackBars = () => useContext(SnackBarContext)

export default useSnackBars

```

Remaining issue is that setAlerts is exposed, but we want something higher level and keep that one private

```JSX

export const SnackBarContext = createContext()

const AUTO_DISMISS = 5000

export function SnackBarProvider({ children }) {
  const [alerts, setAlerts] = useState([])

  const activeAlertIds = alerts.join(',')
  useEffect(() => {
    if (activeAlertIds.length > 0) {
      const timer = setTimeout(() => setAlerts((alerts) => alerts.slice(0, alerts.length - 1)), AUTO_DISMISS)
      return () => clearTimeout(timer)
    }
  }, [activeAlertIds])

  const addAlert = (alert) => setAlerts((alerts) => [alert, ...alerts])

  const value = { addAlert }

  return (
    <SnackBarContext.Provider value={value}>
      {children}
      {alerts.map((alert) => <SnackBar key={alert}>{alert}</SnackBar>)}
    </SnackBarContext.Provider>
  )
}

```
