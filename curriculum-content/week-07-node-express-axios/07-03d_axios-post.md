# Axios

[Axios](https://axios-http.com/) is a promise-based HTTP client for JavaScript which can be used in both the browser and Node.js environments. It facilitates communication between the client and the server through JavaScript code. Using Axios, we can easily send HTTP requests such as **GET** to retrieve data from the server or **POST** to send data to the server, among other HTTP methods.

### Types of Requests

Method | a.k.a. | Description
--- | --- | ---
GET | READ | Requests data from the server. This can be tested in the browser.
POST | CREATE | Sends data to the server for creation or updating resources.
PUT | UPDATE | Updates existing data on the server.
DELETE | DELETE | Removes data from the server.

Axios supports various HTTP methods to cater to the needs of client-server communication.

### Example POST
We've already done the GET portion on our client. Now for the POST.
Let's setup a form to enter a new quote!

When we click submit, we'll make an Axios POST request to the server, sending the new quote as data.

### Example POST
We've already done the GET portion on our client. Now for the POST.
Let's setup a form to enter a new quote!

When we click submit, we'll make an Axios POST request to the server, sending the new quote as data.

```html
<!-- index.html -->
...
<button onClick="addQuote()">Add Quote</button>
...
```

```JavaScript
// client.js

function addQuote() {
  // Make a POST request to the server
  axios({
    method: 'post',
    url: '/quotes',
    data: {
      quoteToAdd: {
        text: 'hello',
        author: 'me',
      }}
  })
  .then(function(response) {
    console.log("SUCCESS!!!");
    // refresh quotes
    getQuotes();
  })
  .catch(function(error) {
    // notify the user
    alert('request failed');
    console.error(error);
  });
}


> The name `quoteToAdd` is not significant -- you can call it whatever you want, so long as it matches what the server is looking for on `req.body`!
> You should always send an object on data -- other data types sometimes have issues

This practice of calling a `POST` (or `UPDATE` or `DELETE` for that matter) and then immediately calling our `GET` request again is going to be a standard procedure for us to update our client with the data from the server. 

The `WHY` behind this will become clearer when we add our database, but essentially, having one `GET` request instead of returning data on every `POST`, `PUT`, and `DELETE` makes the server-side code much cleaner.
