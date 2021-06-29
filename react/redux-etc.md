# Redux: Additional Resources

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[Redux Toolkit: Official Docs](https://redux-toolkit.js.org/)

[Redux Toolkit: Tutorial](#redux-toolkit-tutorial)

## Review, Research, and Discussion

### What’s the best practice for “pre-loading” data into the store (on application start) in a Redux application?

Use a reducer with thunk middleware to grab the data and wrap it in a useEffect with an empty array to signal not to repeat the action/rendering. This will load the data only once in this way at the start of your app. You can access/reload portions later on through other functions.

### When using a thunk/async action that dispatches the actual action, which do you export from your reducer?

You are exporting a function from the reducer.

## Vocabulary Terms

Term | Definition
---- | ----------
Middleware | Middleware are functions that you pass as params to a higher order function to manipulate the data in the end form. This can be done as a workaround to functions that accept a limited number of parameters.
Thunk | An anonymous function `() => result`

### [Redux Toolkit Tutorial](https://redux-toolkit.js.org/tutorials/overview)
