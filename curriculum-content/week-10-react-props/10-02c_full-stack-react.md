# Full Stack React (Client and Server)

**Starter Repo:** [FS React Starter](https://github.com/PrimeAcademy/fs-react-starter)

We will be adding a backend to our starter repo. This application has a working UI, but no backend working with the countries data. Let's get that setup:

## Database Setup

Make sure your database is running! Make a database called `fs-react-starter-db`, and run the commands found in the `database.sql` file to setup your database.

## Getting Started

Typically we use `npm start` to startup our apps. With our full-stack react apps we are going to do things a little differently.

So previously, when we ran `npm start` the command that was actually run for us was `react-scripts start`. This starts the React process that makes our client available on port 3000 and watches our `src` files to do the auto-refresh when they change.

When we add server side code, we also need another port listening for our server requests. If you look at the starter code provided you'll see the following scripts in `package.json`:

```json
"scripts": {
  "start": "node server/server.js",
  "build": "react-scripts build",
  "client": "react-scripts start",
  "server": "nodemon server/server.js",
  "eject": "react-scripts eject"
},
```

The `client` script is now setup to run the same process we were running earlier this week. To run this script, open the terminal and enter:

```shell
npm run client
```

This will start the application development server on port 3000.

We also need to kick off the server. We're gonna need more terminal tabs! In a new terminal tab enter:

```shell
npm run server
```

This will start our node server listening on port 5001 with [Nodemon](https://www.npmjs.com/package/nodemon).

So what's up with `npm start`?

The application development server is for just that - development. It is not part of the final product. When we eventually get to Heroku, everything will be served from our node/express server and `npm start` is setup to do that.

> **NOTE:** The only thing changing here is how we are starting the React application for development. State, Components, and our React knowledge is the same. Everything we already know about our server remains the same. Routers, modules, databases, etc. don't change.
## Axios Setup & Usage

Now that we have things up and running, let's get our client talking to our server!

React is a front-end framework. Let's hook it up to the back end! Unlike jQuery, React doesn't have a way to make HTTP (AJAX) requests out of the box. We need an additional library to make HTTP requests.

We'll be using the [Axios](https://www.npmjs.com/package/axios) library to make requests to our server for data:

```shell
npm install axios
```

The top level component(s) should be responsible for fetching data. In our case, this is the `App` Component, So we need to import Axios into that component.

```jsx
import axios from 'axios';
```

## React Client - GET Request for Countries

Our Node server has a `/api/countries` route so let's make a function that requests our countries so we can show them on the DOM! We're going to have to call it for now.

Add the following function to your `App` component:

```jsx
const fetchCountries = () => {
  axios({
    method: 'GET',
    url: '/api/countries',
  }).then(response => {
    console.log('Full response from server:', response);
    console.log('Data array only in response from server:', response.data);
  }).catch(error => {
    console.log('Got an error from server:', error);
  });
};

// For now, lets call it right away.
fetchCountries();
```

Axios looks pretty similar to jQuery `$.ajax()`, but it's a little different as we get back more info! Look at the response. We get a lot more back, while our data that we sent is on the key `data`. So when you want to gain access to it, you have to use `response.data`.

## Proxy Setup

Our client is looking at port 3000. Our server is on port 5001.

How does the Axios request which is made from port 3000 know to go to port 5001?

BLACK MAGIC! Or not. We setup a `proxy` for our call in the `package.json`. Notice this line in our `package.json`:

```json
"proxy" : "http://localhost:5001",
```

Basically, our application is being told that if anyone comes knocking at door for port 3000 with an Ajax request, it should send them to port 5001 instead.

## React Lifecycle & useEffect Hook

Ok, so when do we call our `fetchCountries` function? In jQuery, we had `$(document).ready()`, but in React, that's not a thing.

We know that we want to do the GET request once at load. Not repeatedly, but also after a POST. (Or DELETE or PUT)

React components have a sequence of events called a lifecycle that determines the order of execution. Essentially, each event triggers the next event to happen, creating a chain of predefined steps.

Basic Lifecycle
Mount -> Render (return) -> Effects
Update State or Props -> Render (return) -> Effects

To hook into that lifecycle, we need a new hook, called `useEffect`.

The `useEffect` hook is a function to initiate our call to our server when the component loads for the first time, for starters. It can do more, but that will be the most common thing.

So we need to import the `useEffect` hook into our `App` component.

```jsx
import { useEffect } from 'react';
```

Now have it call your `fetchCountries` function:

```jsx
// This gets called when the component loads.  
// React equivalent of jQuery's `$( document ).ready()`.
useEffect(() => {
  fetchCountries();
}, []);
```

> **NOTE:** Make sure to remove the `fetchCountries` function from being called below it's declaration now that we have a `useEffect` setup.
The way to think about this is that `useEffect` will run when we first render our component. The empty array is a funky bit, but that is responsible for when the effect will rerun. In this case, it's empty, so it will only run at load! If you forget the array, you could end up with infinite loops!

## Alternate Syntax for Axios Requests

Axios also allows you to use methods that specify the request method:

```jsx
const fetchCountries = () => {
  // Optionally, the request above could also be done like this.
  axios.get('/api/countries')
    .then(response => {
      console.log('Full response from server:', response);
      console.log('Data array only in response from server:', response.data);
    })
    .catch(error => {
      console.log('Got an error from server:', error);
    });
};
```

You could similarly use this syntax for post, put or delete.

## Saving in State using useState Hook

How will we use `response` in the `.then`? What do we need to do to see it on the DOM?

We will need to create a `useState` hook to keep the countries in a variable. We need to import the `useState` hook into our `App` component:

```jsx
import { useEffect, useState } from 'react';
```

Inside our `App` component, let's create a `useState` hook and set it to an empty array:

```jsx
const [countriesList, setCountriesList] = useState([]);
```

Now let's set the hook after our response is back in our `fetchCountries` function:

```jsx
setCountriesList(response.data);
```

Add a `ul` element and a `map` function in our `App` component under the `header` element so we can display the countries on the DOM after the `fetchCountries` function is setup:

```jsx
<ul>
  {countriesList.map(country => (
    <li key={country.id}>The country {country.name} is located on the continent of {country.continent}</li>
  ))}
</ul>
```

> **NOTE:** Good time to take a break.
## Creating a Form & Updating State

Now for the full circle part! Let's add a form and button below the `header` element (above the `ul` element) to add a new country. We'll need two inputs; one for the country name and one for the continent. We'll also need two pieces of state that correspond to those inputs; along with the button sending the data to the server when clicked!

```jsx
const [newCountryName, setNewCountryName] = useState('');
const [newCountryContinent, setNewCountryContinent] = useState('');

const addCountry = (event) => {
  event.preventDefault();
  // Where the POST request goes.
  console.log('Submitted the new country!');
};

<h2><u>Add Country</u></h2>
<form onSubmit={addCountry}>
  <label htmlFor="name">Name:</label>
  <input id="name" value={newCountryName} onChange={(event) => setNewCountryName(event.target.value)} />
  <label htmlFor="continent">Continent:</label>
  <input id="continent" value={newCountryContinent} onChange={(event) => setNewCountryContinent(event.target.value)} />
  <button type="submit">Submit Country</button>
</form>
```

## React Client - POST Request

How would you write a POST with Axios? Data has to be an object, so we'll need to assemble our data a bit and add to our `addCountry` function:

```jsx
axios({
  method: 'POST',
  url: '/api/countries',
  data: {
    name: newCountryName,
    continent: newCountryContinent
  }
})
.then(response => {
  console.log('Full response from server:', response);
  setNewCountryName('');
  setNewCountryContinent('');
  fetchCountries();
})
.catch(error =>{
  console.log('Got an error from server:', error);
});
```

Optionally the request above could also be done as:

```jsx
axios.post(`/api/countries`, {name: newCountryName, continent: newCountryContinent})
  .then(response => {
    console.log('Full response from server:', response);
    setNewCountryName('');
    setNewCountryContinent('');
    fetchCountries();
  })
  .catch(error => {
    console.log('Got an error from server:', error);
  });
```
