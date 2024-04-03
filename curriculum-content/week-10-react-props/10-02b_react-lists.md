# React Lists & Inputs

This lecture will cover:

- Rendering lists of items using React
- New Array.forEach and Array.map JS methods
- React `key` attributes
- Adding an item to an array from an input

## Getting Started
Starting Repo:
`https://github.com/PrimeAcademy/react-lists-starter`

So far we've had `state` with a simple data type on it, but what about when we want an array? 
Let's start with this list of mythical creatureList:


App.jsx
```JSX
import {useState} from 'react';

const creatureData = [
  {name :'Unicorn', origin: 'Britain'},
  {name : 'Sphinx', origin: 'Egypt'},
  {name: 'Jackalope', origin: 'America'}
];

function App () {
 
  const [creatureList, setCreatureList] = useState(creatureData)
  
  return (
    <div>
      <p>
        Splatting to the DOM looks like this:
        <pre>
          {/* We can use JSON.stringify() to dump raw data into our DOM */}
          {JSON.stringify(creatureList)}
        </pre>
      </p>
    </div>
  );

}
```

Great! Let's get just one of the creatures on the DOM:

```JSX
<p>
  Getting one creature looks like this:
  {creatureList[0]}
</p>
```

Dang. Objects are not valid react children...??
A creature here is an object, cant just turn it into html. We have to drill into it!


```JSX
<p>
  Getting one creature looks like this:
  {creatureList[0].name} is from {creatureList[0].origin}
</p>
```

## Getting the list on the DOM

We got a single creature rendered onto the DOM -- now, how do we get them _all_ rendered? 

Let's start with what we know about using arrays in JS: `for` loops:

### Looping to make items


We want to start from a list of creatures
...and create a list of `<li>` elements:
`[<li>Unicorn is from England</li>, <li>Sphinx is from Egypt</li>]`


Manual `For` loops could be used, but it gets hard to work with in JSX.

.map to the rescue. 

### Array.map()

We're going to use an array method called `.map()`. It's a built-in method for arrays. It loops, and then returns a new array!


What this says is -- take the creatures array, loop over it. Use this function on each thing in the array. This function gets access to each item in the array, called creature, and returns a li containing the text of each creature, for each creature.


```JSX
creatureList.map(function(creature) {
    return (<li>{creature.name} is from {creature.origin}</li>);
}
```

You can see the syntax here is similar to `forEach()` -- we still have a "callback" function that gets called with each item in the `creatureList` array. But now instead of `push()`ing to a previously defined array, we just return the value we want in our new array. And then we actually assign `let listItemHtml` to the result of the entire `map()` call, which creates a new array with all of our return values.

We can use _arrow functions_ to make this super concise:

```JSX
    creatureList.map(creature => {
      return (<li>{creature.name} is from {creature.origin}</li>);
    }
```

And because we only have a single `return` statement in our arrow function, we can shorten it even further:

```JSX
creatureList.map(creature => (<li>{creature.name} is from {creature.origin}</li>);
```

Since it's so small let's put it directly in our JSX

## Unique Keys

Wait, what's this weird error in our browser console?

```
Warning: Each child in an array or iterator should have a unique "key" prop.
```
React needs a way to uniquely identify each element, 
so things that are repeated need a `key` attribute. Similarly to a database needing a unique ID.
There's a lot that react does behind the scenes:
https://reactjs.org/docs/faq-internals.html

```JSX
{creatureList.map(creature => <li key={creature.name}>{creature.name} is from {creature.origin}</li>)}
```

This line is starting to get a little long. Note that we add line breaks pretty much anywhere inside of JSX:

```JSX
{creatureList.map(creature => 
  (<li key={creature.name}>
    {creature.name} is from {creature.origin}
  </li>)
)}
```

This key is going to change to a database id or something else in the future, but as long as it is a unique identifier, it's all good. If you're interested in how this matters, this is a neat little example: https://codesandbox.io/s/ovl2x1joky

## List on DOM

Here's what our code looks like, with our list of creatures rendered to the DOM:

```JSX
import {useState} from 'react';

const creatureData = [
  {name :'Unicorn', origin: 'Britain'},
  {name : 'Sphinx', origin: 'Egypt'},
  {name: 'Jackalope', origin: 'America'}
];

function App () {
 
  const [creatureList, setCreatureList] = useState(creatureData)
  
  return (
    <div>
      <ul>
        {creatureList.map(creature => 
         (<li key={creature.name}>{creature.name} is from {creature.origin}</li>)
        )}
      </ul>
    </div>
  );

}
```



