# Refactoring

[Starter Repo](https://github.com/PrimeAcademy/refactoring-lecture)

## Objectives

- Introduce **language and concepts** which will be useful on the job:
  - Refactoring
  - Single Responsibility Principle
  - Technical Debt
- Build comfort with **code mechanics** and organization
  - eg. How are we able to accomplish the same task using functions vs. inline code? How are variables passed around as values or function arguments.
- Be able to **restructure and modify code**, without breaking its behavior

## Overview


**What is Refactoring?**

Refactoring === Changing code, without changing behavior

**Why Refactor?**

- Keep code clean and maintainable. 
- Code naturally accumulates ["cruft"](https://en.wikipedia.org/wiki/Cruft). 
  - Over time, this slows down progress on features. We call this [Technical Debt](https://en.wikipedia.org/wiki/Technical_debt)
  - Regularly cleaning up and refactoring code reduces technical debt
  
For us, refactoring will be **an exercise to build comfort manipulating code** and to **understand how code works**. 

## Organizing code with functions

Functions form the **primary unit for organizing code** behavior. We can put as many lines of code as we want into a function, or we can split up function into many small functions.

How do we decide how _big_ a function should be?

The **Single Responsibility Principle:**

> Every function should have a **single job**

An example of a function with _several jobs._

```js
function loadBananas() {
  // JOB 1: Request banana data from the server
  axios({
    url: '/bananas',
    method: 'GET'
  })
    .then((response) => {
      // JOB 2: Render the list of bananas to the DOM 
      let list = document.getElementById('bananaList')
      
      //empty list
      list.innerHTML = '';
      for (let banana of response) {
        list.innerHTML += `
          <li>${banana}</li>
        `;
      }
    })
    .catch((error) => {
      // JOB 3: Handle errors from the server, and let the user know what happened
      console.log('Yes, we have no bananas', error);
      document.getElementById('errorMessage').innerText = 'There was a problem loading bananas. Please try again later.';
    });
}
```

We can split this up into 3 function, each one with a single job:

```js
// My job is to get bananas from the server
function loadBananas() {
    axios({
        url: '/bananas',
        method: 'GET'
    })
        .then((response) => {
            // I will rely on another function to render the bananas
            renderBananas(response);
        })
        .catch((error) => {
            // And another function to handle errors
            handleError('Yes, we have no bananas', error);
        });
}

// My job is to render bananas
// THINK: what data does this function need to do it's job?
//        define that data as an argument!
function renderBananas(bananas) {
    let list = document.getElementById('bananaList')

    //empty list
    list.innerHTML = '';

    for (let banana of response) {
        list.innerHTML += `
            <li>${banana}</li>
        `;
    }
}

// My job is to handle errors
// THINK: how can we use arguments
//        to make this function more reusable?
//        Adding `displayMessage` allows this function to be used
//        for non-banana-related purposes, too!
function handleError(displayMessage, err) {
    console.log(displayMessage, error);

    document.getElementById('errorMessage').innerText = 'There was a problem loading bananas. Please try again later.';
}
```

We accomplished a few things here:

- Code is better organized: it's more clear what each piece does
- Code is reusable: we can use that `handleError` function for any type of error
- Code is easy to change: eg, if we need to change how errors are displayed, we only need to touch one function.

That last piece is critical for addressing **Technical Debt**. Disorganized and duplicated code is hard to change -- we need to touch code in lots of different places, and we're likely to introduce bugs.

Another definition for the Single Responsibility Principle is:

> Every function should have only **one reason to change.**

The first version of `loadBananas()` might need to change because:

1. We want to add a delete button to each banana on the DOM
2. Or, we want display error messages differently
3. Or, the `/bananas` url changes to `/api/bananas`

The new version would only need to change in that last case.

## Practice: Refactor the Bonus Calculator

https://github.com/PrimeAcademy/bonus-calculator-refactor

----

## Other Types of Refactoring

There are _many_ ways to reorganize code. 

See [refactoring.guru](https://refactoring.guru/refactoring/techniques) for an in-depth discussion of refactoring techniques.


**Extracting and merging:**

- Extract: Move code from one function into a new function, and call that function 
  - Merge: combine code from two functions into one
- Extract: Move code from one module (file) to another, and `require` that module
  - Merge: combine two modules into one
- Extract: move a value into a variable
  - Merge: "inline the value"
      ```js
      // eg.
      learn('refactoring');

      // Extract
      let topic = 'refactoring';
      learn(topic);

      // Merge:
      learn('refactoring')
      ```

**Organizing Data**

- Combine multiple variables into an object/array
  - Or, the inverse: extract variables from an object/array
- Using global state vs. passing data around as arguments

**Renaming**

- Variables
- Arguments
- Files
- Anything else: jQuery selectors, DB tables, etc.
