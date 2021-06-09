# React: Component Composition
**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[DigitalGuardian: What is Role Based Access Control](#role-based-access-control)

[React Cookies](https://www.npmjs.com/package/react-cookies)

[React Cookie Component](https://www.npmjs.com/package/react-cookie)

## Review, Research, and Discussion

### Why is the Context API useful?

Makes it possible to access state throughout the app without having to pass things up and down through many components.

### Can a component outside of a provider get its context?

Yes, that is the point. The provider component makes itself available to consumers that are set up to be able to access the state if they need to.

### What are some common use cases for using the Context API?

Dynamic styling, things like light/dark mode that may need to be implemented in various components, logged in state (which may control whether components do/can render), etc.

### Describe “Context Hell”

Refers to long lists of context nesting, actually probably it wouldn't trouble you too much in the actual dispersal and use of the state, it just is an ugly wrapper around a component. Best practice is probably to use this mechanism sparingly so that you're creating something another human could potentially understand.

## Vocabulary Terms

Term | Definition
---- | ----------
Global State | Similar to any global variable concept, variables (incl. functions) that are available to all components inside their scope
Global Context | The scope that you are giving the global state when you set it up (by wrapping a component).
Provider | That is the component that establishes the global state and "hooks" into the system that makes it accessible throughout the program
Consumer | Components that are set up to access the specified subset of "global state"; this property can be applied to any component, but it must be done explicitly for the component to have access to the global state variables

### [Role Based Access Control](https://digitalguardian.com/blog/what-role-based-access-control-rbac-examples-benefits-and-more)

Restricts access within a network to a user based on their role. The idea is to provided the minimum access that is necessary to fulfill job functions and to keep everything as uniform as possible (i.e. no special access roles).
- Theoretically users could permanently (or temporarily) have several roles
- Cuts down on paperwork, administration, need to change passwords upon roles changing, etc
