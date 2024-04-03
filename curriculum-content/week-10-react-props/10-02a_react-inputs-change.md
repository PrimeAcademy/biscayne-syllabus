# Inputs and onChange

This lecture will cover:

- Using inputs and state
- Common form patterns
- React event handler onChange

## Getting started
This starts with where we left off in interaction-state:

```jsx
import { useState } from 'react';

function App () {
    // destructured array
    //const [myStateVariable, setMyStateVariable] = useState('initial value');

    let [name, setName] = useState('Luke');
    let [count, setCount] = useState(0);


    const handleClick = () => {
        console.log(('clicked!'));
        setCount(count+1);
    }

    return (
        <div>
            <h1>React Local State</h1>
            Hello! My name is {name}.
            <button onClick={handleClick}>Click me!</button>
            I've clicked the button {count} times.
        </div>;
    )
}

```




Now what if we want to be able to change the value of `name`? We can render an input for the user to set their name.

Let's add an input, where the user can type in their name. Lets have it log any time someone types. We'll use onChange! It works like onClick, but instead of handling clicks, it handles keystrokes.

```JSX
<input onChange={() => console.log(('name changed!'))} placeholder="Name"/>
```
COOL! It logs on every keystroke!

Now, how could I see the contents of the users input in the log. We'll need it when we change the name state variable.

```JSX
<input onChange={(event) => console.log(('name changed!', event.target.value))} placeholder="Name"/>
```

Now that we can see the input value, we need to do something with it. A simple thing could be to setName as we type!

```JSX
<input onChange={(event) => setName(event.target.value)} placeholder="Name"/>
```

AWESOME!! IT AUTO UPDATES!
This is a vital part of react -- as you change a state variable, a component will re-render itself!

## Common Form Pattern

Now, we could add a button and change the name on click. But this adds complexity. We have to think about when we want name to change, and what to do until then...

Lets add a button for our submission. Lets make a function to handle the submit right away.
```JSX
    const handleSubmit  = () => {
        console.log('clicked submit');
    }
    ///...

    <button onClick={handleSubmit}>Submit</button>
```

Now how do we deal with this? We want to call `setName` when we click this button, but before we had `event.target.value` but that is only on inputs.

We need something else. Often, forms have their own holding place for what people type, and then when we click a button, we use that temporary storage elsewhere. 
Think this way -- you don't submit a credit card every keystroke!

So, we need some sort of state for temporary variable holding, and then on click, we grab that temporary value and use it.

So we need to:
Create a new state variable, call it nameInput.
Change our onChange function to update that with setNameInput
Change our onClick function to call setName and use the value in nameInput

Lets work through it! 


```jsx
import { useState } from 'react';

function App () {

    let [name, setName] = useState('Luke');
    let [count, setCount] = useState(0);
    let [nameInput, setNameInput] = useState('');


    const handleClick = () => {
        console.log(('clicked!'));
        setCount(count+1);
    }

    const handleSubmit  = () => {
        console.log('clicked submit');
        setName(nameInput);
    }

    return (
        <div>
            <h1>React Local State</h1>
            Hello! My name is {name}.
            <input onChange={(event) => setNameInput(event.target.value)} placeholder="Name"/>
            <button onClick={handleSubmit}>Submit</button>

            <button onClick={handleClick}>Click me!</button>
            I've clicked the button {count} times.
        </div>;
    )
}

```

## Clear inputs
After submit, lets clear the input!
Try `setNameInput('')` -- does that work?

It works, but the DOM is not cleared. We cleared state, which isnt tied to the input. Dang! This input is Uncontrolled.

To make the two communicate, we need to tell the input that it gets its value from state. This makes it a Controlled Input, as its value is always controlled by state.

```jsx
<input
    value= {nameInput}
    onChange={ (event) => setNameInput(event.target.value)} placeholder="Name"/>
```