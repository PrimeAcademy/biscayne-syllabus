# React Components

## Objectives

- Build off the [React orientation lecture](./10-01_react-orientation.md) which intros Vite, folder structure, basic importing, etc.
- Understand of the concept of Components:
    - We compose entire applications with them
    - They are *functions*
- Use Components' return statements to render JSX on the DOM
- Create new Components: export, import, and render in App
- Understand passing primitive values as props

## Components

Everything in a React app should be broken down into components. This makes it easier to re-use code and test your code.

Think about the components that would make up a basic list app.

```plaintext
index
+-------------------------+
|App                      |
|+---------------------+  |
||Form Input & Button  |  |
|+---------------------+  |
|                         |
|+----------------------+ |
||List of Stuff         | |
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

## What are Components?

Describe what a component is from a *design* perspective. Visuals help with real-world examples: Facebook, Pinterest, X (Twitter), etc.

![Youtube Component Example](../images/components-youtube-example.png)

Each red box outlines a Component - a repeated, self-contained portion of the page.

Describe what a component is from a *code* perspective. It's just a function whose ultimate job is to return a chunk of JSX to render. Like functions, they can be provided with arguments...more on this later.

Traits and common use cases for `Functional Components` include:

- self-contained files of HTML, CSS and JS that can display on the DOM
- respond to user events (clicks, typing, etc.)
- intended to be reused
- can talk to one another
- are functions
- can receive arguments

Bottom line: A React Component is a *function* whose job is to render a small amount of HTML on the DOM.

### React Starter Project

We've already made our repo, so lets look closer.

The starter project comes with two components out of the box, `App.jsx` and `index.jsx`. Each component comes with a corresponding CSS file.

### index.js

Just like `server.js` is the entrypoint for running our server, `index.jsx` is the entrypoint for running the client.

```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.jsx'
import './index.css'

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)
```

We won't be touching any of the code in `index.jsx` this week, but it's good to understand what it's doing.

Most importantly:

- Imports our `App` component
- Selects the `<div id="root">` element from our `index.html`
- Renders our app as HTML to the `#root` element

Some other things going on in `index.jsx`:

- `<React.StrictMode>`: This tells React to give us warnings about potential bugs. It doesn't render anything to the DOM

### Get Organized & Paths

Let's start by creating a `components` folder with an `App` folder inside it.

Now we're going to move `App.jsx` into that folder. When we restart, things are broken! Why? What do we need to change?

We have to update `index.jsx` to the correct path.

### Creating Components

Let's create a `components/Header/Header.jsx` file. Inside, we'll render a simple Hello World in a `h1`. Lets import it in our App component and render it there:

```js
// Similar to:
//    const Header = require('../Header/Header');
// We are assigning the function from Header.jsx to 
// a variable called Header
import Header from '../Header/Header';

// ...

<div className="App">
  <header className="App-header">

    {/*  
    Header is a function that returns HTML. aka a "Component"

    Here we "render the Header component" by treating it like an HTML element.
    In reality, we are calling the header function!

    Header()
    */}
    <Header />
    <img src={logo} className="App-logo" alt="logo" />
    <p>
      Edit <code>src/App.js</code> and save to reload.
    </p>
    <a
      className="App-link"
      href="https://reactjs.org"
      target="_blank"
      rel="noopener noreferrer" 
      >
        Learn React
    </a>
  </header>
</div>
```

Let's create a `components/Panda/Panda.jsx` file. Inside, we'll render an `h3` wherein a panda introduces itself:

```js
function Panda() {
  return (
    <h3>Hey. I'm a friendly panda! 🐼 My name is Pandoot.</h3>
  )
}
```

Lets import the Panda component into our App component and render it there:

```js
<div className="App">
  <header className="App-header">
    <Header />
    <Panda />
    <img src={logo} className="App-logo" alt="logo" />
    <p>
      Edit <code>src/App.js</code> and save to reload.
    </p>
    <a
      className="App-link"
      href="https://reactjs.org"
      target="_blank"
      rel="noopener noreferrer" 
      >
        Learn React
    </a>
  </header>
</div>
```

Obviously, one panda is not sufficient. Let's render at least five pandas:

```js
<div className="App">
  <header className="App-header">
    <Header />

    {/*
    Every time we render <Panda />
    we are actually calling the Panda() function 
    */}
    <Panda />   {/* Panda() */}
    <Panda />   {/* Panda() */}
    <Panda />   {/* Panda() */}
    <Panda />   {/* Panda() */}
    <Panda />   {/* Panda() */}
    <img src={logo} className="App-logo" alt="logo" />
    <p>
      Edit <code>src/App.js</code> and save to reload.
    </p>
    <a
      className="App-link"
      href="https://reactjs.org"
      target="_blank"
      rel="noopener noreferrer" 
      >
        Learn React
    </a>
  </header>
</div>
```

Five pandas is a vast improvement, but it's a bummer that they're all identical. Our components are functions, and we can give them arguments to fix this.

Remember back in Tier 1 when we created functions like:

```js
// The greet function accepts two arguments: name and shoeColor
function greet(name, shoeColor) {
  return `Hello ${name}! Nice ${shoeColor} shoes!`
}

greet('Elvis', 'blue suede');
// or
greet('Dorothy', 'Ruby Red')
```

We can use function arguments to customize how a function behaves. And we can do the same with component functions!

Let's try giving our Panda component some arguments:

```jsx
// Pass Panda() two arguments: 
<Panda 
  disposition="friendly" 
  name="Pandoot" 
/>
```

It appears that we're adding attributes to an HTML element, but each `<Panda />` instance is actually a function that we are calling with unique arguments.

In React, we call these arguments **props** (as in **properties**). React bundles up these arguments into a single argument, as an object:

```jsx
<Panda 
  disposition="friendly" 
  name="Pandoot" 
/>

// is equivalent to:
Panda({
  disposition: "friendly" 
  name: "Pandoot" 
})
```

```js
// Our Panda component function receives a single argument (object), that we'll call `props`
function Panda(props) {
  return (
    // We can access individual props using object dot notation
    <h3>Hey. I'm a {props.disposition} panda! 🐼 My name is {props.name}.</h3>
  )
}
```

*Note the `{someVar}` for variable interpolation in JSX! Similar to the the `${someVar}` syntax in JS template strings (but only works in JSX).*

Now in our App, we can call (render) our Panda component with whatever arguments (props) we want.

```js
<div className="App">
  <header className="App-header">
    <Header />
    {/* Panda({ disposition: "friendly"  name: "Pandoot" }) */}
    <Panda disposition="friendly" name="Pandoot" />
    <Panda disposition="fluffy" name="Floof" />
    <Panda disposition="tiny" name="Lil' Paws" />
    <Panda disposition="dreamy" name="Snooze" />
    <Panda disposition="uniquely wonderful" name="Kris Szafranski" />

    <img src={logo} className="App-logo" alt="logo" />
    <p>
      Edit <code>src/App.js</code> and save to reload.
    </p>
    <a
      className="App-link"
      href="https://reactjs.org"
      target="_blank"
      rel="noopener noreferrer" 
      >
        Learn React
    </a>
  </header>
</div>
```

**Important distinction**: Just like function arguments, passing props to components is a *one-way street.* Props can only be passed from a parent component to a child component. (In this case, App is the parent and Panda is the child.)

### More Components

Let's practice making another component with props. This time, we'll extract some existing code into it's own component:

```js
// components/Instructions/Instructions.jsx
function Instructions() {
  return (
    <p>
      Edit <code>src/App.js</code> and save to reload.
    </p>
    <a
      className="App-link"
      href="https://reactjs.org"
      target="_blank"
      rel="noopener noreferrer" 
      >
        Learn React
    </a>
  );
}

// components/App/App.jsx
import Instructions from '../Instructions/Instructions.jsx';

function App() {
  return (
    // ...

    <Instructions />
  )
}
```

Uh oh, we have an error!

> Syntax error: Adjacent JSX elements must be wrapped in an enclosing tag (9:12)

What does that mean? Read the errors carefully. They are trying to help.

We need to only have ONE overall parent per component. We could wrap both elements in a div:

```jsx
function Instructions() {
  return (
    // 👇 <div> wrapper, so we only return one "parent" element
    <div>
      <p>
        Edit <code>src/App.js</code> and save to reload.
      </p>
      <a
        className="App-link"
        href="https://reactjs.org"
        target="_blank"
        rel="noopener noreferrer" 
        >
          Learn React
      </a>
    <div>
  );
}
```

But that extra div just makes our DOM messier, and might mess up our CSS. So instead we'll use a [Fragment](https://reactjs.org/docs/fragments.html)

```js
function Instructions() {
  return (
    // 👇 A fragment is an element with no name!
    // It's not actually rendered to the DOM (check the Elements panel)
    <>
      <p>
        Edit <code>src/App.js</code> and save to reload.
      </p>
      <a
        className="App-link"
        href="https://reactjs.org"
        target="_blank"
        rel="noopener noreferrer" 
        >
          Learn React
      </a>
    </>
  );
}
```

## Exercise

See you if you can modify this component to accept props to customize the link URL and link text.

eg:

```js
<Instructions
  linkText="Learn the Peruvian Plan Flute"
  url="https://www.youtube.com/watch?v=s-EmXw7JOVU"
>

{/* Should render to the DOM: */}
<a
  className="App-link"
  href="https://www.youtube.com/watch?v=s-EmXw7JOVU"
  target="_blank"
  rel="noopener noreferrer" 
  >
  Learn the Peruvian Plan Flute
</a>
```

...speaking of reading errors carefully...

> Exercise: [Rocket React Rascal](https://github.com/PrimeAcademy/rocket-react-rascal) - Read the instructions and look at the final gif!! You're not done until it looks like the gif.
