# Redux State

Let's start from here:

`index.js`
```JavaScript
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './components/App/App';
import { createStore, combineReducers } from 'redux';
import { Provider } from 'react-redux';

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


const elementList = (state = [], action) => {
    return state;
};

// The store only accepts one reducer
const storeInstance = createStore(
    combineReducers(
        {
            count,
            elementsList,
        }
    )
);


const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <Provider store={storeInstance}>
        <App />
    </Provider>
  </React.StrictMode>
);
```

## Logger

State is hard to conceptualize, and tends to look behind, so let's get a tool to help:

```
npm install redux-logger@3
```

```js
// Import applyMiddleware from redux
import { applyMiddleware, createStore, combineReducers } from 'redux';

// Import logger from our new redux-logger lib
import logger from 'redux-logger';


const storeInstance = createStore(
    combineReducers(
        {
            count,
            elements,
        }
    ),
    // Tell redux that we want to use our new logger
    applyMiddleware(
        logger
    )
);
```

Now, when you click the button, you get some NICE logs, previous state, action, and next state!
