# Postman
https://www.postman.com/downloads/

## Overview

As developers, we need to test our code to make sure it works. We can test GET routes from a browser URL bar. But that isn't possible for POST (nor PUT & DELETE).

We need a tool to help us. The standard across the industry is a tool called Postman!

Postman will allow us to make requests of all the different types to our server, no client needed! This speeds up development because the server-side can be tested without any front end completed!
No HTML or client-side javascript required!

## Installation
Its free!

(May want to give this link and have students download it, then take a break?)

https://www.getpostman.com/downloads/

Open the zip, drag to your application folder.


## Usage
The main area kinda resembles a browser -- theres a URL bar at the top, but there's a big GET dropdown to its left. If you click it, we can select any of those methods!

Let's test our GET:
GET `http://localhost:5000/quotes`

Then click send!

We see all the quotes below!

Let's test our POST
POST `http://localhost:5000/quotes`

If we submit, it seems to work, but if we run a get it looks like we got an `undefined`

POSTs need some data sent - remember that weird req.body and body-parser thing? We need to send the body!

Theres a tab, conveniently called Body! Click on it.
Look -- see the radio button urlencoded? Recognize it? Click that!

Then lets make a key called `quoteToAdd` and a value of `hello world`.
The key needs to be what we're using in our POST route. 

After sending that, let's do our GET again. Look, `hello world` is there!

Postman is a really great tool for developing -- but we can't expect someone using our app to know and use postman! We need a way to do something similar from our client!
