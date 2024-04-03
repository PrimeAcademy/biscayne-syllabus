# Generator Functions
~ 30 minutes
## function()
What does a function do, anyway?
Anything!

What is its limitation?
It doesn't have any memory of itself. 
It can only `return` one thing!


## function* myGenerator ()

Run...Stop...Run
 
ES6 has a new way to write functions -- they're called Generator Functions.

ES6 generators may be paused in the middle, one or many times, and resumed later, allowing other code to run during these paused periods. A generator function 'remembers' where it was, and variables inside it until it has finished.

Generators are functions which can be exited and later re-entered. Their context (variable bindings) will be saved across re-entrances.


They have a special keyword `yield` -
A generator function can `yield` many different things. As it is called, it will pause and exit the function when it reaches a `yield`. When called again, it will remember where it was last time you called it, and then continue until it gets to a new `yield`, a `return`, or the end of the code block!

```
function* myGenerator() {
    yield true;
    yield 100
    yield 'Hello!'
    yield [1, 2, 3]
    yield { key: value }
}
```

When executed, the generator will pause at the yield and wait until it's called again!

```
//create an instance
const goDogGo = myGenerator();

// call generator with .next()
console.log(goDogGo.next());
console.log(goDogGo.next());
console.log(goDogGo.next());
console.log(goDogGo.next());
console.log(goDogGo.next());
console.log(goDogGo.next());
console.log(goDogGo.next());
console.log(goDogGo.next());
```

Inifinate loops are normally a bad thing. In a generator function, we can use them to repeat over our yields. Since a generator function pauses at each yield, we won't crash the browser.

```
function* getSwitch() {
    while(true) {
        yield 'on';
        yield 'off';
    }
}

const toggle = getSwitch();
console.log(toggle.next().value); // 'on'
console.log(toggle.next().value); // 'off'
console.log(toggle.next().value); // 'on'
```

Generators can have loops, conditional statements, variables and even `return` (just be careful about return! That still will stop!). 

What will the following code do?

```
function* countDownGenerator() {
    let a = 10;
    while(a > 0) {
      yield `Launching in ${a}`;
      a -= 1;
    }
    yield `Take off!`;
}
```


## Key takeaways
Generators remember where they were last time they were called. `yield` is how we pause the generator.

Redux Sagas use Generators as the backbone to help orchestrate and organize async operations, like AJAX calls, and all the side effects (Loading spinners, other AJAX calls, etc) that result. 

`yield` and  `function*` is the key vocab. Sagas take care of the `.next()` for us!
