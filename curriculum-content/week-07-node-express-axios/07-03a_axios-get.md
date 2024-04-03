# Axios

[Axios](https://axios-http.com/) is a promise-based HTTP client for JavaScript which can be used in both the browser and Node.js environments.

>It provides a simple API for making HTTP requests and is built on top of the [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) interface. With Axios, we can send **GET** requests to retrieve data from the server, or **POST** requests to send data to the server, among other HTTP methods.


👉🏽 **Get Started**: https://axios-http.com/docs/intro

> You'll notice in the official documentation that there are multiple ways to setup axios. We will primarily use something called a CDN, which is a way to include a library in our project without having to download it.
> (*A CDN just references a file that is hosted on a server somewhere else. In this case, we are referencing a file hosted on a server run by the axios team. This is a common way to include libraries in our projects, and we will use it for the rest of the course.*)

👉🏽 **Download the file manually** (Alternative)

> The alternative would be to download the library and include it in our project. This is a bit more complicated. You may occasionally see this in some projects or tutorials. If you want to download the file, you can just save this webpage as a `.js` file: https://cdn.jsdelivr.net/npm/axios/dist/axios.js

---

### Types of HTTP Requests

Method | a.k.a. | Description
--- | --- | ---
GET | READ | Requests data. This can be tested in the browser.
POST | CREATE | Send data to the server.

We'll be adding two more request types at a later point.

Awesome resource about the [anatomy of an HTTP request](https://rapidapi.com/comics/the-anatomy-of-http-requests).

## Client setup

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Quotes</title>
  <!-- Import Axios using CDN -->
  <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script> 
  <!-- Import Axios -->
  <script defer src="client.js"></script>
</head>
<body>
  <h1>Quotes!</h1>
  <ul id="output"></ul>
</body>
</html>

```
**client.js**
```JavaScript

function onReady() {
  getQuotes();
}

onReady()

function getQuotes() {
  // Get quotes from server - AXIOS!
}
```

## Using Axios

The basic use of Axios beings by calling the `axios()` method in your JavaScript file. This method takes an object as an argument. This object has a few properties that we can set to configure our request.

👉🏽 Documentation: `axios()` method: https://axios-http.com/docs/api_intro

### Example GET
Let's get our quote of the moment from the server:

```JavaScript
axios({
      method: 'GET',
      url: '/quotes'
    })
  .then(function(response) {
    console.log("SUCCESS!!!", response.data);
    // TODO - append quotes to the dom
  })
  .catch(function(error) {
    // notify the user
    alert('Request failed. Try again later.');
  });
```

The response has our quote in it... Let's render it to the DOM.

```JavaScript
function getQuotes() {
  // Get quotes from server using Axios
  axios({
      method: 'GET',
      url: '/quotes'
    })
    .then(function(response) {
      console.log("SUCCESS!!!", response.data);
      // append quotes to the dom
      renderToDOM(response.data);
    })
    .catch(function(error) {
      // notify the user
      alert('Request failed. Try again later.');
      console.error(error);
    });
}

function renderToDOM(quotes) {
  // Select the output element
  let outputElement = document.getElementById('output');
  // Empty the output element
  outputElement.innerHTML = '';

  for (let quote of quotes) {
    // Create a <li> for each quote and add it to outputElement
    outputElement.innerHTML += `
      <li>
        ${quote.text} by <i>${quote.author}</i>
      </li>
    `;
  }
}
```


---

### A deeper look at `axios()`

There are two basic formats for making requests with Axios. We can use the `axios()` method, or we can use an `alias` method.

#### `axios({config})`

> This is the format we used above. We pass an object with configuration options to the `axios()` method. These configurations are a way to customize our request. For example, we can set the `method` to `GET` or `POST`, or we can set the `url` to `/quotes` or `/quotes/random`.

```JavaScript
axios({
  method: 'get',
  url: '/user/12345'
});
```

#### `axios.get(url)`

> This format is called an `alias`. It is a shortcut for the above format. It is the same as calling `axios()` with an object that has a `method` property set to `GET`.


```JavaScript
axios.get('/user/12345');
```
```
