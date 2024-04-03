# React State

This lecture will cover:

- Rendering local state in React
- Using `useState` to make and update React component state
- React event handler onClick

## Getting started
Starting repo:
https://github.com/PrimeAcademy/react-state-starter

Let's open up `src/App.jsx`.
```jsx

function App () {

    return (
        <div className="App">
            <p>Hello! My name is Luke.</p>
            <button>Click me!</button>
            <p>I've clicked the button 0 times.</p>
        </div>;
    )
}
```

## React State

What if we want to pull that _name_ out into a dynamic "variable"? Or the _count_? In React, we use _state_ to track variables within a component. If a variable should be shown on the DOM, it should live in state.

To achieve this, we cannot just make a variable, we have to use a `hook` called `useState`. React gives us this tool.

`import {useState} from 'react'`

This is Object destructuring -- a sort of shortcut to get a property of an object. Here, this is a shortcut for:
```js
import React from 'react'
const useState = React.useState
```


The pattern for useState:

```js
const [myStateVariable, setMyStateVariable] = useState('initial value');
```

Think of it this way: useState is a function, that you give the initial value to. It can be any datatype.

It returns to us an array, with two things inside. This syntax is called `destructuring`. 

Array destructuring, (similar to object destructuring). useState returns an array. It accepts the default value of the state variable.

Written in full it could look like:

```jsx
let whatUseStateReturns = useState(false);
let myThing = whatUseStateReturns[0];
let changeMyThing = whatUseStateReturns[1];
```

You get a state variable at index 0, and a function for how to change that state variable at index 1.

As written, you could read this as 

"Make two variables, grabbing the first thing out of the array and call it name, and grab the second thing out of the array and call it setName"

With useState -- the first thing in the array is ALWAYS the variable and the second thing is ALWAYS a function that sets that variable.



```jsx
import { useState } from 'react';

function App () {
    // destructured array
    //const [myStateVariable, setMyStateVariable] = useState('initial value');

    const [name, setName] = useState('Luke');
    const [count, setCount] = useState(0);

    return (
        <div>
            <h1>React Local State</h1>
            <p>Hello! My name is {name}.</p>
            <button>Click me!</button>
            <p>I've clicked the button {count} times.</p>
        </div>;
    )
}

```

----

## Changing state
Let's get that button working! When we click on it, we want to trigger some functionality! Let's start with a console log. With react, we use the html `onClick`!

```JSX
<button onClick={() => console.log('clicked!')}>Click me!</button>
```

Note -- this is arrow function shorthand -- you can only get it to do one thing this way. If you want a console.log AND other things, you have to write it with brackets and return.

So, now that we got a click logging, lets try to get the DOM to read how many times we've clicked the button. Remove the console.log. How might we do this? We have a state variable called count...

Your first instinct might be to just alter count-- cant do that! First, its a `const`, but even if it were a `let`, we wouldn't do that. 

React works in a very specific way.
We have to call the setter function that we were given for count. If we alter it directly, the DOM will not update!

```JSX
// NO
<button onClick={() => count += 1}>Click me!</button>

//YES
<button onClick={() => setCount(count + 1)}>Click me!</button>

```

## Named Function
If you wanted to do more than just setCount, you should use a named function. 

Something like:
```jsx
 const handleClick = () => {
    console.log('clicked');
    setCount(count + 1);
}
//...

<button onClick={handleClick}>Click me!</button>
```

## Each Component's State is an Island:
If you create a piece of React state inside a component, it is *scoped* to that component. (Because components are functions!) So, let's create a new Clicker component, and we can render multiple instances of it in App. Each Clicker component instance is only aware of its own state. I guess we don't want all of these to say Luke, so let's also pass each Clicker a name prop:

```js
// In App.jsx:
import Clicker from '../Clicker/Clicker.jsx';

function App () {
    return (
        <div>
            <h1>React Local State</h1>
            <Clicker name="Luke"/>
            <Clicker name="Some Other Person"/>
            <Clicker name="Yet Another Person"/>
        </div>;
    )
}
```

```js
// In Clicker.jsx:
import { useState } from 'react';

function Clicker(props) {
    const [count, setCount] = useState(0);

    const handleClick = () => {
        console.log('clicked');
        setCount(count + 1);
    }
    
    return (
      <div>
          <p>Hello! My name is {props.name}.</p>
          <button onClick={handleClick}>Click me!</button>
          <p>I've clicked the button {count} times.</p>
      </div>
    )
}

export default Clicker;
```

Most things that seem like extra special "React Superpowers" are actually just "JavaScript Functions" superpowers. Each of these Clicker components we're rendering has it's own private island of React state, but really, each component is *just a function with its own scope.*

## Quick Note on Passing React State as a Prop:
If you create a piece of React state in App, you can pass it to a child component as a prop. If that piece of state is modified in the parent component, the child component will update to reflect that change in state. Or, um, you could say it *Reacts* to the change in state. (Yes, this is why this library is called React 🙂.)

You can even pass a piece of state's *setter* function as a prop! Which is the last idea to share here, and also totally a hint for this next activity:


>> Activity: Rock Picker
