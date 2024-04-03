# React & Redux


## Action Payload

At some point, we'll probably want to start passing more information to our reducer. 

We're going to use elements on the periodic table as our example:

```JSX
<button onClick={() => dispatch({ type: 'ADD_ELEMENT', payload: 'hydrogen'})}>Add Element</button>
```

Let's update our `elementList` reducer to pick up this dispatched action and log the payload.

```JavaScript
const elementList = (state = [], action) => {
    if(action.type === 'ADD_ELEMENT') {
        console.log(`The element was ${action.payload}`);
    }
    return state;
};
```

## Spread

For the elements data, we want to add to an array so we can see all the elements added. How have we added to an array in the past? 

```JavaScript
const elementList = (state = [], action) => {
  if (action.type === 'ADD_ELEMENT') {
    console.log(`The element was ${action.payload}`);
    state.push(action.payload);
    return state;
  }

  return state;
}
```

Let's try rendering those elements to the App component, and see if they show up:

```js
const elementList = useSelector(store => store.elementList);

// in return value
<ul>
{elementList.map((element, i) => (
    <li key={i}>{element}</li>
))}
</ul>
```

Does this work?

NO! If you try this out, the component will NOT re-render. What's happening here?!

> __Important__ Remember not to push into the array. For React/Redux to recognize the changes we need to return a new array/object.

`.push` mutates our original state, which means that we keep the original array and make changes to it. React, Redux, and others need us to avoid mutation, meaning we have to make new arrays/objects every time. This is pretty deep, and you can read more 
https://blog.bitsrc.io/understanding-javascript-mutation-and-pure-functions-7231cc2180d3


So, we need to...make a new array. It needs to have all our stuff that was in the old array, and add to it our old array.

Spread is a javascript operator that can help!
`...` -- Spread operator

In a way, what spread does is give us the contents of an array. This is useful so we keep the contents of the old array when we make a new one. Objects too!

```js
const myArray = [1,2,3];
console.log(...myArray); // --> 1 2 3
```

So breaking it down
New array: []
Old array contents: `...state`
new data: `action.payload`

```JavaScript
const elementList = (state = [], action) => {
    if (action.type === 'ADD_ELEMENT') {
        // Create a new array
        // which includes all the values from our previous array
        // AND ALSO the new value in action.payload
        console.log(`The element was ${action.payload}`);
        return [...state, action.payload];
    }

    return state;
};
```

## Using Redux with Forms


Now that we know how to send data to reducers using `action.payload`, lets setup a `<form>` to grab data from the user:

- Create a form with an input tied to local state
- On form submit, dispatch an `ADD_ELEMENT` action, with the element name as payload
- Render the element list to the DOM

```jsx
import { useSelector, useDispatch } from 'react-redux';
import { useState } from 'react';

function App() {
    // Grab our count value from the redux store
    const count = useSelector(store => store.count);

    // Grab the elementList array from the redux store
    const elementList = useSelector(store => store.elementList);

    // Track the new element to add in our "local" state
    // (yes, we can still use local state!)
    const [newElement, setNewElement] = useState('');

    // "dispatch" is how we talk to redux from react
    const dispatch = useDispatch();

    const handleSubmit = (event) => {
        // Don't reload on form submit
        event.preventDefault();

        // Tell redux that we want to add the new element
        dispatch({
            type: 'ADD_ELEMENT',
            // Pass in the element name, that we're tracking in state
            payload: newElement
        });

        // Clear the form field
        setNewElement('');
    };

    return (
      <div>
        {/* Dispatching an action when a button is clicked*/}
        <button onClick={() => dispatch({ type: 'INCREASE' })}>Increase!</button>
        <button onClick={() => dispatch({ type: 'DECREASE' })}>Decrease!</button>

        {/* Render the count from our redux store */}
        <p>Count: {count}</p>

        {/* Form to allow users to add a new element */}
        <form onSubmit={handleSubmit}>
            <input 
                type="text" 
                placeholder="Element Name"
                value={newElement}
                onChange={event => setNewElement(event.target.value)}
            />
            <button type="submit">Add!</button>
        </form>

        {/* Render the elements list from redux */}
        <ul>
            {elementList.map((element, i) => (
                <li key={i}>{element}</li>
            ))}
        </ul>
      </div>
    );

}

export default App;
```


## Child Components

The great thing about redux is that we have a single state object that's _shared_ with every component in our application. So there's no need to pass down props into child components.

Let's refactor our elements form to use multiple child components. First, we'll extract and `<ElementList />` component

```jsx
function ElementList() {
    // Grab the elementList from the redux store
    const elementList = useSelector(store => store.elementList);

    return (
        // Render the elements list from redux 
        <ul>
            {elementList.map((element, i) => (
                <li key={i}>{element}</li>
            ))}
        </ul>
    );
}
```

Because `ElementList` is grabbing data directly from the redux store, there's no need to pass down props from our `App` component:

```jsx
// in App.js

{/* Look ma, no props! */}
<ElementList />
```

We can do the same with an `<ElementForm />`, but this time we'll `dispatch()` directly to redux:

```jsx
function ElementForm() {
  // Track the new element to add in our "local" state
  const [newElement, setNewElement] = useState('');

  // "dispatch" is how we talk to redux from react
  const dispatch = useDispatch();

  const handleSubmit = (event) => {
      // Don't reload on form submit
      event.preventDefault();

      // Tell redux that we want to add the new element
      dispatch({
          type: 'ADD_ELEMENT',
          // Pass in the element name, that we're tracking in state
          payload: newElement
      });

      // Clear the form field
      setNewElement('');
  };

  return (
      // Form to allow users to add new element
      <form onSubmit={handleSubmit}>
          <input 
              type="text" 
              placeholder="Element Name"
              value={newElement}
              onChange={event => setNewElement(event.target.value)}
          />
          <button type="submit">Add!</button>
      </form>
  );
}
```

Our final `App` component will look like:

```jsx
function App () {

    return (
      <div>
        {/* Form to allow users to add new element */}
        <ElementForm />

        {/* Render the elements list from redux */}
        <ElementList />
      </div>
    );

}

export default App;
```

Now _that_ is one clean component!