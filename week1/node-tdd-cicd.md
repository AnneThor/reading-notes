# Node Ecosystem, TDD, CI/CD

- **[Return to Main Page](https://annethor.github.io/reading-notes/)**

## 1. Describe (in plain English) what Array.map() does

In JavaScript, array.map() takes a function as a parameter and applies this function to every element in the array it is called on, returning a new array. The original array is not modified.

## 2. Describe (in plain English) what Array.reduce() does

In JavaScript, array.reduce() takes a function and an optional original value as parameters. This will return one value that is determined by the callback function and the initial value. The function passed in as a parameter is an accumulator function that determines how the array elements will be "reduced" or combined to come up with the final return result. The original array is not modified.

## 3. Provide code snippets showing how to use superagent() to fetch data from a URL and log the result:

Superagent is a package that provides processing to HTTP get results to make them easily accessible to the programmer. 

You must first install the package in your terminal:

```
npm i superagent
```

This will also add the dependency to your package.json (you can also do that manually if you want to have more control on the major/minor versions.

You will need to import the dependency into the file where you are using the superagent package.

**Example with normal Promise .then() syntax**

```
const superagent = require('superagent')

superagent.get('APIEndpoint')
  .then(data => {
    console.log(data.body)
    })
  .catch(err => {
    console.error(err)
    })
```
**Again with async / await syntax**

```
const superagent = require('superagent')

async function getData() => {
  try {
    let results = await superagent.get('APIEndpoint')
    console.log(results.body)
    } catch (err) {
    console.error(err)
  }
}
````

## 4. Explain promises as though you were mentoring a Code 301 level student
  
> JavaScript is single threaded - meaning that it can only execute one statement at time. Promises are callbacks that take "resolve" and "reject" as arguments. The promise will exeute and then the function control flow will follow the successful (resolve) condition or the unsuccessful (reject) condition. Using promises allows you to control the outcome of asynchronous processing by waiting until the process completes to handle the result.

> Anne Thor

## 5. Are all callback functions considered to be Asynchronous? Why or Why Not?

JavaScript is single threaded, so callbacks would not be asynchronous unless you specifically tell JS to behave this way. The point is that you gain more control over the UI and the functionality of the program if you use asynchronous functions in scenarios where functions could cause blocking.
