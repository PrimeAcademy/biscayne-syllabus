# Redux Actions

Redux provides two things to our reducers: state and an action. 

We've talked about how `state` represents the current value being held in the redux store. Where does `action` come from?

React components can talk to our redux state by _dispatching actions_:

```jsx
// Import useDispatch
import { useSelector, useDispatch } from 'react-redux';

function App () {
    // Grab out count value from redux
    const count = useSelector(store => store.count)

    // "dispatch" is how we talk to redux from react
    const dispatch = useDispatch();

    return (
      <div>
        {/* Dispatching an action when a button is clicked*/}
        <button onClick={() => dispatch({ type: 'INCREASE' })}>Increase!</button>

        {/* Render the count from our redux store */}
        <p>Count: {count}</p>
      </div>
    );

}

export default App;
```

We are dispatching actions with our button click, and those actions are what cause our reducers to run. 

```js
const count = (state = 0, action) => {
    // Log the action
    console.log(`Hey!!! I'm a reducer y'all!!!`, action);

    return state;
};
```

Every time we click the _Increase!_ button, we'll see our action object in the console: `{ type: 'INCREASE' }`

## Action Type

Actions are objects. They need a property of `type` that tells our reducer what kind of action it is:

```js
// reducer!
const count = (state = 0, action) => {
    console.log(`Hey!!! I'm a reducer y'all!!!`, action);

    // We can inspect the action's "type" property, 
    // to see what our component is trying to do
    if (action.type === 'INCREASE') {
        console.log(`You clicked increase!`);
    }

    return state;
};
```


When the `App` component dispatches an action, it's telling the reducer what kind of change it wants to make to our global redux state. It's up to the reducer to actually make the change, by returning a different value for state:

```js
const count = (state = 0, action) => {
    console.log(`Hey!!! I'm a reducer y'all!!!`, action);

    if (action.type === 'INCREASE') {
        console.log(`You clicked increase!`);

        // The next value of `reduxStore.count` will be 1 more than the previous value
        return state + 1;
    }

    // If action.type is anything else, just return the last value of state.
    return state;
};
```

Try clicking on the _Increase!_ button again. You'll see the value of _Count_ increase on the DOM every time you click.

Let's add a new _Decrease_ button. 

Add the button in `App.js`:

```JSX
<button onClick={() => dispatch({ type: 'INCREASE' })}>Increase</button>
<button onClick={() => dispatch({ type: 'DECREASE' })}>Decrease</button>
```


Go ahead and try updating the `count` reducer to handle our new `DECREASE` action type.

The final result should look like:

```js
// reducer!
const count = (state = 0, action) => {
    console.log(`Hey!!! I'm a reducer y'all!!!`, action);

    if (action.type === 'INCREASE') {
        console.log(`You clicked increase!`);

        // The next value of `reduxStore.count` will be 1 more than the previous value
        return state + 1;
    } else if (action.type === 'DECREASE') {
        console.log(`You clicked decrease!`)

        // The next value of `reduxStore.count` will be 1 less than the previous value
        return state - 1;
    }

    // If action.type is anything else, just return the last value of state.
    return state;
};
```

Notice that the type is different for the two actions. We set that up in the button onClick, and can use that to know which button was clicked in the reducers. 
