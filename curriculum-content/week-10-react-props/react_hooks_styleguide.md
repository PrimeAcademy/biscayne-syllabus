# Prime Functional Components & Hooks Styleguide 

## General

- Must be using  >= `React 16.8` and a matching `react-dom` version
- Extensions: Use .jsx extension for React components.

## Components

Make components with declared functions: 

```js
import React from 'react';
//avoid
const Greeting = (props) => {
    return (
        <div>{props.hello}</div>
    );
}

// good
function Greeting({hello}){
    return (
        <div>{hello}</div>
    );

}
```

Inner functions use function expressions for declaration:

```js
const Greeting = (props) => {
    // avoid
    function hello(){
        return 'hello!';
    }

    // good
    const hello = () => {
        return 'hello!';
    }

    return (
            <div>{hello()}</div>
    );
}
```


### Props
Only declare props for components that use them.
Destructure props to their constituent parts.


```js
// avoid
function Greeting(props){
        return (
            <div>Hi!</div>
        );

}
// good
function Greeting({hello}){
        return (
            <div>{hello}</div>
        );

}
```

## React Hooks

### useState
- Destructure Arrays with `const`
- Use pattern [myStateValue, setMyStateValue] for naming
- Always include the function to set a state value, even if not used immediatly. 
- Give default value to useState, even if falsy, like empty string, or 0, or false

```js
import React, {useState} from 'react';

//good
function Greeting(){

    const [helloWorld, setHelloWorld] = useState('Hello Hooks');

    return (
        <div>{helloWorld}</div>
    );

}
```

### useEffect
- Use arrow callback function
- Include dependency array as second argument, not conditional around useEffect, to control execution.


```js
import React, {useEffect} from 'react';

//good
function Greeting(){

   
   useEffect(() => {
    //    apiCall, dispatch to saga, etc
    //    setData(results)
   }, [])

    return (
        <div>Hi!</div>
    );

}
```

## Redux Hooks
Use `useSelector` instead of `useStore`

```js
import React from 'react';
import {useSelector, useStore} from 'react-redux'


//avoid
function Greeting(){

    const store = useStore();

    return (
        <div>{store.counter}</div>
    );

}

//good
function Greeting(){

    const counter = useSelector(state => state.counter)

    return (
        <div>{counter}</div>
    );

}
```
