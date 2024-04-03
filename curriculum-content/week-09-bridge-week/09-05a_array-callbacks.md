
# Arrays and Callback Functions

## Objectives

- Introduce the **term: callback function**
- Understand how **functions can be passed as arguments**, like any other variable
- **Implement our own functions using callbacks**, to more intuitively understand how callback functions work.
- Learn to use **built-in Array methods**, which accept callbacks 
  - Start with `.forEach()` and `.map()`
  - This is the foundation to learn additional methods, like `.filter()`, `.find()`, `.reduce()`, etc.
- Be better **prepared for React**, where callback functions and Array built-ins are used extensively


## Callback functions

> NOTE: We will be using callback functions extensively with React!

When we talk about callback functions, we're talking about passing a function as an argument to a function. We've done this before!

```js
router.get('/', (req, res) => {
  //Router Code...
});
```

We are giving that req/res function to the `.get()` function, and asking express to call the function for us, whenever a GET request is made. 

We can imagine express has some code internally like:

```js
function get(route, callbackFn) {
    // wait until the event is triggered 
    waitForSomeoneToRequest(route);

    // Then call the function we were given
    callbackFn();
}
```

## Example: call a function twice for me

Let try to write our own function, using callbacks:

```js
// Give me a function, and I'll call it twice!
function callMeTwice(callbackFn) {
    callbackFn();
    callbackFn();
}

function sayHey() {
    console.log('Hey 👋');
}

// Pass the sayHey function as an argument
// to callMeTwice
callMeTwice(sayHey);
// -> "Hey 👋"
// -> "Hey 👋"
```

Let's look at some alternate syntaxes here:

```js
// Using an arrow function, assigned to a variable
const sayHey = () => {
    console.log('Hey 👋');
}
callMeTwice(sayHey);

// Or, instead of assigning the function to a variable,
// pass the function directly:
callMeTwice(() => {
    console.log('Hey 👋');
})

// As a one-liner:
callMeTwice(() => console.log('Hey 👋'));
```

We'll see all of these different styles in different situations. Get to know them!

## Example: forEach

How can we use callback functions to help us work with arrays? Imagine a `forEach` function that loops through and array, and calls a function for each item:

```js
let pets = ['tarantula', 'iguana', 'trained kangaroo'];

forEach(pets, (pet) => {
    console.log(`I've never owned a pet ${pet}!`)
});
// -> "I've never owned a pet tarantula!"
// -> "I've never owned a pet iguana!"
// -> "I've never owned a pet trained kangaroo!"
```

How would we write that function?

```js
function forEach(list, callbackFn) {
    // Loop through the list
    for (let item of list) {
        // Call the function we were given
        // and pass the item in as an argument 
        callbackFn(item);
    }
}
```

In fact, this function is already built into javascript: https://www.w3schools.com/jsref/jsref_foreach.asp

```js
pets.forEach((pet) => {
    console.log(`I've never owned a pet ${pet}!`)
});
```

## Example: map

What if we want to transform each value in an array, and make a new array?

```js
let greetings = ['Hello', 'Hey', 'Yo'];

let rudeGreetings = [];

for (let greet of greetings) {
    let rude = greet.toUpperCase() + '.'; // "Hello" --> "HELLO."
    rudeGreetings.push(rude);
}

console.log(rudeGreetings);
// -> ['HELLO.', 'HEY.', 'YO.']
```

This is a very common pattern, and something we've seen before. Can we use callback functions, to simplify?

> INSTRUCTOR NOTE: Let students attempt to write a `map()` function on their own, then solve.

```js
function map(list, callbackFn) {
    let results = [];

    for (let item of list) {
        // Call the provided function with the item
        let newItem = callbackFn(item);
        // Save the return value in our `results` array
        results.push(newItem);
    }

    // Return the results
    return results;
}

map(greetings, (greet) => {
    return greet.toUpperCase() + '.';
});

// Or, as a one-liner
map(greetings, (greet) => greet.toUpperCase() + '.');

// Built-in JS syntax
// https://www.w3schools.com/jsref/jsref_map.asp
greetings.map((greet) => greet.toUpperCase() + '.)
```

## Example: Filter

https://www.w3schools.com/jsref/jsref_filter.asp

## Example: Reduce

Advanced!

https://www.w3schools.com/jsref/jsref_reduce.asp

## Example: setTimeout

https://www.w3schools.com/jsref/met_win_settimeout.asp
