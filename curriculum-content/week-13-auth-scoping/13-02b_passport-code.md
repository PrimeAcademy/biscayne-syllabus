# Passport (SERVER SIDE FOCUS)

## Objectives

- Students know new vocab: `req.user`, `req.isAuthenticated()`
- Students know that the Auth concepts already explored are in place here
- Students can protect routes
- Students can access user information via session
- Students understand `req.user` is what is in user table
- Students understand how to make a different user `role` and how to protect based on that
- Students are aware that passwords are NOT stored in plain text, instead `salt` and `hashed`



## Starting Lecture Repo

[First Look](https://github.com/PrimeAcademy/solo-first-look-lecture)

## Passport Introduction

* Introduce the specific authentication framework we will use for application development.
    - **Note** Passport.js is specific to Node/Express.
* Passport is authentication `middleware` for Node/Express
* Remind them of other middleware we use (e.g. body-parser)
* Simplifies authentication process and supports a variety of authentication methods
* Provides the framework for authenticating users
* Optionally includes and works with sessions (which we will use)


## High Level Repo Overview & Auth Concepts

- server.js
  - Requiring several new files
  - Setup of Session Middleware, Passport Middleware
- strategies/user.strategy.js
  - serialize and deserialize the functions tell Passport.js how to get information from a user object to store in a session (serialize), and how to take that information and turn it back into a user object (deserialize).
 - passport.use is doing the actual work of logging in
    - Look At this codeblock
- modules/session-middleware.js
  - What makes & maintains our cookie so we can stay logged in
- modules/authentication-middleware.js
  - Custom middleware! Just checks req.isAuthenticated() for us.
 - encryption.js
   - Where the salting and hashing stuff lives
   - We don't store passwords in plain text!
- constants/warnings.js
  - Will put a warning in the console if you don't set a .env file and variable
  
## Logout, Login, Whats the Diff?
Let's use postman.

Hit this: GET http://localhost:5000/api/pets

- Empty Array!
- Look at the request cookies -- nothing special
- Look at the request logs in the server -- No `user`, `isAuthenticated` is false

Using Postman, lets import our base requests - Register, Login, etc. 
1. Start the server - `npm run server`
2. [Import the sample routes JSON file](./PostmanPrimeSoloRoutes.json) by clicking `Import` in Passport. Select the file.
3. Click `Collections` and `Send` the following three calls in order:
    1. `POST /api/user/register` registers a new user, see body to change username/password
    2. `POST /api/user/login` will login a user, see body to change username/password
    3. `GET /api/user` will get user information, by default it's not very much

We're logged in!
Now hit: GET http://localhost:5000/api/pets

Still an empty array (No pets in DB) but check your console again. We've got `req.user` and `req.isAuthenticated()` is true!!
 

## How to protect a Route

Passport middleware will add a special method to each request object -- isAuthenticated()
It will return true if the user is logged in and false if not. It does a lot of work, but it is based on the cookie.
No cookie, not logged in. 
Expired or invalid cookie, not logged in.


In order to protect a route, we need to build a fence, or more accuratly, an `if/else` block. Surround your code in the /api/pets route with:
```
if(req.isAuthenticated()) {...} else {res.send(403)}
```

Now try hitting `/api/pets` again -- what do you get? Now what about after you log out? 
Hit `POST /api/user/logout` and then try `/api/pets` again. 403!


## How to check who the user is

What if now I've got lots of people, and lots of pets. What if I want the user to only see THEIR pets? Your first instinct is that we'll need to send back and forth the userID or something like that. 

You already have all the user info for that request because of Passport! It's on `req.user`. Just like `req.body`. Passport attaches the user row from the database to the request (`req`) object before it is routed to your function. This happens for EVERY request to your server.
 
 ```
 if(req.isAuthenticated()) {
    let queryText = `INSERT INTO "pet" ("firstname", "person_id") 
                     VALUES ($1, $2)`;    
    pool.query(queryText, [req.body.firstname, req.user.id])
        .then((result) => {
            res.sendStatus(201);
        }).catch((err) => {
            res.sendStatus(500);
        });
} else {
    res.sendStatus(403);
}
```

## How to set a user type

How do I handle Admin? What if I want only admins to be able to delete/update/post etc.

Add a column in the table for role. 

In addition to checking for the `isAuthenticated()`, we'll have to check to see what role they are, too. This is AUTHORIZATION! 


Secure Submarine Activity
