# React Router

## Objectives
1. Enact Client Side Routing
2. Programmatically change routes with `useHistory`

**We are using version 5 of react-router-dom**

Remind the students that version 6+ has breaking syntax changes!


## Starting Repo for Lecture

We're going to assume that you're comfortable with creating React Components at this point. So we're starting with a repo that has a bunch of pre-built React Components.

Start of lecture: https://github.com/PrimeAcademy/react-router-lecture

> Note: Cover Object Destructuring first

## SPAs!
In single page applications (SPAs), we often want to show new content without reloading the whole page. For Data, we use AJAX requests, and for view-related things we use whats called client-side routing. This is different than requesting a new HTML page every time we need to see different things on the DOM.

React-router utilizes "hash" urls (e.g. http://localhost:3000/#/home) to allow us to handle our routes on the client. This works because servers ignore everything after the #. With React-router, we can use what the browser ignores to show our users the content they requested.

Luckily, since everything is a component, basic routing is simply selecting which component to show for a given url.

We're building off of examples from here: https://reacttraining.com/react-router/web/example/basic


## Basic Routing

Let's add react router to our project:

```
npm install react-router-dom@5
```

In `App.js`, let's add a few of our page components:

```JSX
<Home />
<Plants />
<Animals />
```

Great! Now all three show up, but we only want to show one at a time depending on the URL. This is when we'll bring `react-router-dom` into our project:

```JSX
import { HashRouter as Router, Route } from "react-router-dom";
```

Now wrap everything in your render `return` inside of a `Router` tag:

```JSX
<Router>
    <div>
        <Home />
        <Plants />
        <Animals />
    </div>
</Router>
```

Now we're going to determine which view is shown based on the URL path:

```JSX
<Route path="/">
    <Home />
</Route>
<Route path="plants/">
    <Plants />
</Route>
<Route path="/animals">
    <Animals />
</Route>
```

You can think of these as `if` blocks. If our `path` matches, show that `component`. `path` is our URL matcher and `component` is the component we want to show when we match that route. One thing that's weird is that the `Home` Component is showing up everywhere. That's because `/` matches all of these (`/plants` and `/animals` both include `/`). If we only want it to show the home component when it matches `/` exactly, we have to include the word `exact`.

```JSX
<Route path="/" exact>
    <Home />
</Route>
```

## Linking

These routes should now be working when we go to `localhost:3000`, `localhost:3000/#/plants`, and `localhost:3000/#/animals`, but no user will type those in. We need links. Let's add `Link` to our `import`.

```JSX
import { HashRouter as Router, Route, Link } from "react-router-dom";
```

Now add links to the render and it should look like this:

```JSX
import React, { Component } from 'react';
import { HashRouter as Router, Route, Link } from "react-router-dom";

// Page components
import Home from './pages/Home/Home';
import Plants from './pages/Plants/Plants';
import Animals from './pages/Animals/Animals';

function App () {

  return (
    <div >
      <Router>
        <div>
          <ul>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/plants">Plants</Link>
            </li>
            <li>
              <Link to="/animals">Animals</Link>
            </li>
          </ul>

          <hr />
        <Route path="/">
            <Home />
        </Route>
        <Route path="plants/">
            <Plants />
        </Route>
        <Route path="/animals">
            <Animals />
        </Route>
        </div>
      </Router>
    </div>
  );
}

export default App;
```

## Change View Programmatically

`Link` components are great if all we want to do is change where we are. But what if we want to do other things, too? We'd need to change view after we do other things. Which a `Link` can't do...

Let's add a button to our Home Component.  `onClick` let's fire a function that makes an alert, and then changes where we are.

```jsx
import React from 'react';

function Home () {
    
    const handleClick = () => {
      alert("You are headed to animals");
      ///CHANGE LOCATION???
    };
    
    return (
      <div>
        <h2>Home</h2>
        <button onClick={handleClick} />
      </div>
    );
}
export default Home;
```


But how??

Thankfully, we have the ability to do so! We just need to hook into the proper place.

`import {useHistory} from 'react-router-dom'`

and

`const history = useHistory()`

`history` is the key here! It's a record of everywhere we've been, and the last entry is where we are!

To change where we are, we will just `push` a new location to the array!

```jsx
const handleClick = () => {
  alert("You are headed to animals");
  ///CHANGE LOCATION???
  history.push('/animals');
}; 
```



## Detail View & `useParams` Hook

Often, we want just one thing to be displayed. We can do this with our client side routing!

First, we need a route, and we need it to work with any id. 
            
```JSX
<Route path="/animals/:id" exact>
  <AnimalDetail />
</Route>
```
Component looks like this:

```JSX
import { useParams } from 'react-router-dom';
const AnimalDetail = () => {
   // the useParams hook will give us all of the url params
   // from the path (like /animals/:id) as an object. Use
   // object destructing to save the :id portion to a variable
   // with the name 'id'
   // if it were `:taco` in the route, then it would be `const {taco}`
   const { id } = useParams(); 
   return (
   <>
      <h1>Animal Details</h1>
      <p>Details for animal with id of {id}</p>
   </>);
}

export default AnimalDetail;
```
