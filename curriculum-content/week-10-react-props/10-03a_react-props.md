# Using React Props to Refactor a Form and List:

[Starter Repo](https://github.com/PrimeAcademy/react-props-starter)


## Topics

- Practice extracting components using props
- Destructuring props
- Passing callback functions as props
- Working with form components
  


Uses the Mythical Creatures App with a List and Form components. Goals:

- make a header and pass it props
- move list to a component
- move individual list items to a component
- move form to a component

----

Our `App.js` is getting a little crowded. We should probably break these out into components now:

```
index
+-------------------------+
|App                      |
|                         |
|+---------------------+  |
||Header               |  |
|+---------------------+  |
|+---------------------+  |
||Form Input & Button  |  |
|+---------------------+  |
|                         |
|+----------------------+ |
||List of things        | |
||+--------------------+| |
|||List Item           || |
||+--------------------+| |
||+--------------------+| |
|||List Item           || |
||+--------------------+| |
|+----------------------+ |
|                         |
+-------------------------+
```

## Props
Remember: *Components are functions.* Functions can take in arguments. In React, props are merely the way that we can provide arguments to our components.
[Flow Diagram](https://www.techdiagonal.com/reactjs_courses/beginner/reactjs-component-state/)

## Header
Lets start by making a new  file `components/Header/Header.js`

```JSX

function Header (props) {
    console.log(props)
    return (
        <>
            <h1>Hi!</h1>
        </>
    );
  }

export default Header;
```

And in `App.js` we need to import:
```JSX
import Header from '../Header/Header';
```

And we need to render the component:
```JSX
<Header />
```
So to use props, we will pass data from the parent (App) to the child (Header). Props (just like function parameters!) can be named anything you want!

```JSX
<Header myProp={"YAY"} isCool={true} title={"Hello Props!"} />
```

Check out your console log from header -- what do you see?

Let's render the title prop in our header:

```js
 return (
        <>
            <h1>{props.title}</h1>
        </>
    );
```


## List Component
Often with props, we share existing variables! Let's make a list!

Create a new file `components/CreatureList/CreatureList.js` 

```JSX

function CreatureList (props) {

    return (
        <ul>
          {
            props.list.map(creature => (
              <li key={creature.id}>
                The {creature.name} originated in {creature.origin}.
              </li>
            ))
          }
        </ul>
    );
  

export default CreatureList;
```

And in `App.js` we need to import:
```JSX
import CreatureList from '../CreatureList/CreatureList';
```

And we need to pass the correct props:
```JSX
<CreatureList list={creatureList}/>
```

## Destructuring Props
It's getting annoying to always have to type `props.thing`, and we can make it a little simpler. If we `destructure` our props, we can get access to the stuff directly. You've already done this, actually:

`import {useState} from 'react` -- useState is being destructured out of react. Its a shortcut for `react.useState`

So, if we know the name of the prop we want, we can just grab it by its name!

`function CreatureList ({list}) {`

This is extra groovy, because now our components *communicate* to us what props (or arguments) they expect to receive.

## Item

Look at that list! It's great! But, the `<li>` itself might be a good candidate for a component, so we can make it happen with props!
Create a new file `components/CreatureItem/CreatureItem.js` 


```JSX

function CreatureItem({creature}) {

    //This component could have local state too!

    return (
      <li>
        The {creature.name} originated in {creature.origin}.
      </li>
    );
  

export default CreatureItem;
```

And use in the `.map` in CreatureList:

```JSX
 return (
        <ul>
          {
            list.map(creature => (
              <CreatureItem key={creature.id} creature={creature} />
            ))
          }
        </ul>
    );
```

Note, `key={creature.id}` and `creature={creature}` look like they are doing the same thing, but they are not.
`key={creature.id}` is assigning a unique key (label) to each CreatureItem component that's retuned, so react can keep track of it.
`creature={creature}` is passing data to the CreatureItem child component, in a property labeled `creature` on the props object.
This concept of the same set of symbols doing two different things is called "overloading" and it makes reading code harder.

## Form Component

Now the tricky one...

We can pass *anything* through props. Not just arrays or strings, but also functions. (Remember that Rock Pickers stretch goal?) We're also going to move the state of the form into the Form component.

Create a new file `components/CreatureForm/CreatureForm.js`

```JSX

function CreatureForm ({ addNewCreature }) {

    const [newCreatureName, setNewCreatureName] = useState('')
    const [newCreatureOrigin, setNewCreatureOrigin] = useState('')
    
    const handleSubmit = (event) => {
        event.preventDefault();
        
        const newCreature = {
            name : newCreatureName,
            origin: newCreatureOrigin
        };
        
        // Pass the new creature up to the App component
        addNewCreature(newCreature);

        // Clear form inputs
        setNewCreatureName('');
        setNewCreatureOrigin('');
    }
    
    return (
      <form onSubmit={handleSubmit}>
          <input 
            value={newCreatureName} 
            onChange={(event) => setNewCreatureName(event.target.value)} 
          />
          <input 
            value={newCreatureOrigin} 
            onChange={(event) => setNewCreatureOrigin(event.target.value)} 
          />
          <button type="submit">Add Creature</button>
      </form>
    );
  }


export default CreatureForm;
```


This new `CreatureForm` component will be able to call the `addNewCreature` function that we've passed down as a prop.

Think of `addNewCreature` as a machine that will always send a POST request to '/creature', then run the `fetchCreatures` function. To do its job, it just needs to be handed an object that looks like:
```js
{
    name : 'Some Creature Name',
    origin: 'Some Location'
}
```

This `addNewCreature` function needs to live in our `App` component, because that's where the `fetchCreatures` function lives. This is a big concept to wrap your head around. Don't think of it as "a tricky thing about React," though. The concept is that *functions have access to the outer scope of where they were defined*.



```js
// in App.jsx
// This is the function that we pass down to CreatureForm.
// We'll call it when we want to add a new creature.
const addNewCreature = (newCreature) => {
   axios({
      method: 'POST',
      url: '/creature',
      // Send the new creature object to the server
      data: newCreature
    })
      .then( (response) => {
        console.log('Response:', response);
        fetchCreatures();
      })
      .catch(function (error) {
        console.log('Error on add:', error);
      });
}
```

Here's how we pass this function down to `CreatureForm` as a prop:

```JSX
// In App.js
// Import the CreatureForm
import CreatureForm from '../CreatureForm/CreatureForm';

// ... 


// And render it
<CreatureForm 
  // pass the addNewCreature function as a prop
  addNewCreature={addNewCreature} 
/>
```

We can also remove these bits of state from `App`, since they are now managed by the `CreatureForm`:

```js
// Get rid of these in App.jsx
// const [newCreatureName, setNewCreatureName] = useState('');
// const [newCreatureOrigin, setNewCreatureOrigin] = useState('');
```

## Final Version

Check out how clean `App.jsx` looks now! This is a big win. Look especially closely at App's return statement. It now more clearly communicates what it does. We don't have to read a billion lines of code to see what's happening. In human language, we can now see that App is a `<div>` that contains:
  - A `<CreatureForm>` that depends on a function called `addNewCreature` to do its job.
  - A `<CreatureList>` that depends on a piece of state called `creatureList` to do its job.

Compared to where we started with `App.jsx`, this code does the same thing...but it takes much less time for a human to look at it and have an idea of what it does. React gives us the ability to *compose* applications that more clearly communicate what they do. 🙂

```jsx
import {useEffect, useState} from 'react';
import axios from 'axios';

import './App.css';

function App () {
  const [creatureList, setCreatureList] = useState([]);

  const fetchCreatures = () => {
    axios({
      method: 'GET',
      url: '/creature'
    })
      .then( (response) => {
        setCreatureList(response.data);
      })
      .catch(function (error) {
        console.log('Error on get:', error);
      });
  }

  const addNewCreature = (newCreature) => {
    axios({
        method: 'POST',
        url: '/creature',
        data: newCreature
      })
        .then( (response) => {
          console.log('Response:', response);
          fetchCreatures();
        })
        .catch(function (error) {
          console.log('Error on add:', error);
        });
  }

  useEffect( () => {
    fetchCreatures();
  }, [])
  
  return (
    <div className="App">

      <CreatureForm 
        addNewCreature={addNewCreature} 
      />

      <h2>All Creatures</h2>
      <CreatureList list={creatureList}/>
    </div>
  );

}

export default App
```
