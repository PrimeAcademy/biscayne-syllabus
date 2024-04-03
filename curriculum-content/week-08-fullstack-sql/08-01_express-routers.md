#08_01b Express Routers

[Express Router Starter](https://github.com/PrimeAcademy/express-routers-starter)  
[Express Router Final](https://github.com/PrimeAcademy/moonstone_express-routers-finished)

So far, we've been mostly dealing with one or two sets of data at a time in our projects. When I say set of data, I mean the dynamic data that we're GETting and POSTing to the server. Like *jokes* in the code challenge you just worked on, or *artists* and *songs* from the Get Jazzy assignment. But let's consider more complex examples! What if we're keeping track of many sets of data? We're going to talk about a tool that we can use to help us organize our code as things get more complex.

Our goal here is to keep our code clean. Can you imagine a situation where you're keeping track of many different sets of data, and you've got a GET and POST route for each of those sets of data, and your `server.js` file is all of a sudden 400 lines long? That's not ideal. It's harder to read.

We've got a tool to help us, courtesy of Express! Express routers (aka routing modules) can be used to organize code. They are very similar to how we would use a module to contain logic, classes, or data. In this case, though, routers are specifically meant to organize our **routes** (i.e. `app.get('/my-url'`, `app.get('my-other-url'`).  

## Practical example: `book_router.js`

Let's take a look at our starter repo. In our `server.js`, we have routes for two different slices of data: Books and Movies. To organize our code better, we're going to split out the routes for Books and Movies into their own files. 

For this example, we'll use the following routes:

- `GET` route [http://localhost:5000/book](http://localhost:5000/book)
- `POST` route [http://localhost:5000/book](http://localhost:5000/book)

First, we're going to create a new folder to keep track of our routes, called `routes`. Then inside that folder, let's create a file to keep track of all of our Books routes. We'll call this `books.js`. 

Now, our goal is to move the Books routes from `server.js` to `books.js`. Let's start in `books.js`. In order for our route code to actually work here, we need some way to access Express's magic. In `server.js` we were doing `app.get`, but `books.js` doesn't know about `app`. So that's not going to work. 

So we're going to use another Express tool: `express.Router()`. Let's add these lines to `books.js`:

```js
let express = require('express');
let router = express.Router();
```

Routers are like mini versions of Express applications. They provide functionality for handling route matching, requests, and sending responses, but they do not start a separate server or listen on their own ports. They'll allow us to handle client requests, but `server.js` will still be responsible for creating and starting the server.

From here, we can our Books routes in. We'll also bring over `bookList` to keep track of our books. There's one small change here: Now, instead of `app.get(...)`, we're going to use the router: `router.get(...)`. Remember, this file doesn't know about `app`.

```js
const bookList = [];

router.get('/', (req, res) => {
  res.send(bookList);
});

router.post('/', (req, res) => {
  bookList.push(req.body);
  res.sendStatus(200);
});
```

The last thing we need to do is *export* our router so we can use it in our `server.js`:

```js
module.exports = router;
```

## Practical example: `server.js`

Now over in `server.js`, we can pull in the Book router. First, we need to import it: 

```JavaScript
let bookRouter = require('./routes/book_router.js');
```

Now, the last thing we need to do is tell our server to direct any `'/books'` requests to our book router. Here's how we do that:

```js
// When the server gets a request for '/book', send them to bookRouter to handle that request
app.use('/book', bookRouter);
```

And voila! Our code is nice and organized. Now, you try it with the Movie router. (solo-ish practice for 10 minutes)

## App vs. Router

```
     app.js    │    product.js    
───────────────├────────────────
     app       │     route
 app.express() │ express.Router()
     ALL       │  SUB-FUNCTIONS
```

[http://stackoverflow.com/questions/11321635/nodejs-express-what-is-app-use#answer-11321828](http://stackoverflow.com/questions/11321635/nodejs-express-what-is-app-use#answer-11321828)
