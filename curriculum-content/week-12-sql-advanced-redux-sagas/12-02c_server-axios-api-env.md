# Server Side API Requests

## Lecture Starting Code
https://github.com/PrimeAcademy/server-axios-env


## Giphy API (Auth)

- Not all APIs allow you to just access them -- sometimes you need to have some sort of authentication or an account with the API provider.

- Let's look at Giphy [https://developers.giphy.com/]
    - Read docs
    - Sign up, make app, get API Key
    - Look at docs for `/trending`, how do I make the request?
    - Try a request in postman

Can't do this in Postman for users, how do we make this request with our apps?

We have 2 options -- Client side requests or Server side requests.

## Client or Server

You can make an HTTP/Ajax request to a 3rd party API in your React app -- technically you do it all the time. Any time you use `axios` you tell it where to go. 

But there's usually something special with 3rd party APIs. What did we have to make in order to work with Giphy? The Key!

Client side code is exposed -- meaning anything you give to the client (your auth keys!) could be read and then copied. No good! Plus making requests to other domains from your client side code has security limitations. (Google 'CORS')

We need our key to be private from the client. So lets make a request from our node server. We'll need a library to help. `axios` works here, too!

`npm install axios`
`const axios = require('axios');

```js
axios.get('http://api.giphy.com/v1/gifs/trending?api_key=asdfasdfasdf&limit=5').then((response) => {
  console.log(response.data);
});
```

Great. But, now, I want that (or part of it) on our client! How do we get it there?

We'll put the axios call inside a route, and then hit the route from our client!
```js
//server-side
router.get('/', (req, res) => {
  axios.get('http://api.giphy.com/v1/gifs/trending?api_key=asdfasdfasdf&limit=5').then((response) => {
    res.send(response.data);
  }).catch( err => {
    res.sendStatus(500);
  });
})

//client-side
  const handleClick = () => {
    axios.get('/gifs').then(response => {
      console.log(response)
    }).catch( err => {
      console.log(err)
    })
  } 
```



## Github and keys

So, we want our keys to be protected. We did that by moving our request to happen on the server, where the client can't see it. But what would happen if I commit and then push to github.

OH NOES! My keys!

This is a super common pitfall. So much so github will scan repos for keys and then alert you if they find any. So we need to remove the key from our code.

Wait, what?

## .env
'env' is for Environment Variable. We have seen these before when deploying to Heroku. Recall `process.env.PORT` and `process.env.DATABASE_URL`.

The idea is that the "environment" has the same variable names but with different values.

Here, we need our Giphy API Key for the code to work, but we don't want it in our repo. When we deploy this, we still need the key! [Heroku has a place to store your environment variables.](../supporting-documentation/deploy-heroku.md#app-updates-environment-variables) We'll explore this when we deploy our React apps next week.

Okay, so we can't have the actual key committed, but we need the key.

 `npm install dotenv`;

 Add this to our server.js:

 ```
require('dotenv').config();
```


.env files ARE NOT COMMITTED. This keeps our secrets, well, secret. 

The file has a few rules:

- Left is name of your variable, equals, right is the value/secret.
- One variable per line.
- You can have as many as you want!

```
GIPHY_API_KEY=asdfasdfasdf
```

This will protect our key from committing, but how do we get it out? Remember `process.env.PORT`? 

The .env library will `inject` variables into our node server on start that we can then access by name:

```
process.env.YOUR-VARIABLE-NAME
```


```js
//server-side
router.get('/', (req, res) => {
  axios.get(`http://api.giphy.com/v1/gifs/trending?api_key=api_key=${process.env.GIPHY_API_KEY}&limit=5`).then((response) => {
    res.send(response.data);
  }).catch( err => {
    res.sendStatus(500);
  });
})

```
 