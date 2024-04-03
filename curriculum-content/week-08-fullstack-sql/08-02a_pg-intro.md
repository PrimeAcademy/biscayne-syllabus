## Instructor Notes
Optional starting repo: [https://github.com/PrimeAcademy/pg-express-starter](https://github.com/PrimeAcademy/pg-express-starter)

> This starting repo starts with a working express server with routing module and GET and POST routes dealing with an array (no DB present)


# Full Stack with PG

[PG](https://www.npmjs.com/package/pg) is a node module that allows us to communicate with our PostgreSQL database.

PG lives between the server and database:

```
,________,         .------,              .------,                  .------.
|________|       ,'_____,'|   req >    ,'_____,'|                 (        )
|        |       |      | |            |      | |       PG        |~------~|
|        |       |      | |  - HTTP -  |      | |    <------->    |~------~|
|        |       |      | ;            |      | ;                 |~------~|
|________|       |______|'    < res    |______|'                  `.______.'
 HTML/CSS        JavaScript         Node / Express               PostgreSQL

```

## Accessing our database from Node with PG
From our code's point of view, we need a way to talk to our new database server and tables. We need to connect to our database server before issuing queries. We will be using an npm package called `pg`.

Add it to our application dependencies: `$ npm install pg`


The routes need `pg` to make SQL queries to fulfill incoming requests. For now, let's put this connection into the `routes/musicLibrary.js` file:
```JavaScript
// Setup PG to connect to the database
const express = require('express');
const router = express.Router();
const pg = require('pg'); // Import just what we need


const pool = new pg.Pool({
    // The name of your database. This will change for every app!
    database: 'songs', 
    // Where is your database? localhost === On your computer
    host: 'localhost',
    // Postgres listens for network connections on port 5432, by default
    port: 5432,
});


router.get('/', (req, res) => {
 // router code
});
```

## GET ROUTE
Let's setup a GET route to send back all of the songs in our database.

```JavaScript
router.get('/', (req, res) => {
    let queryText = 'SELECT * FROM songs;';
    pool.query(queryText)
        .then((result) => {
            res.send(result.rows);
        })
        .catch((err) => {
            console.log(`Error making query ${queryText}`, err);
            res.sendStatus(500);
        });
});
```

> We can test this route using Postman!


## POST Route
Let's setup a POST route to add a new song to the database.

We could do something like this...
```JavaScript
router.post('/', (req, res) => {
    const newSong = req.body;
    const queryText = `
        INSERT INTO songs (artist, track, published, rank)
        VALUES ('${newSong.artist}', '${newSong.track}', '${newSong.published}', ${newSong.rank});
    `;
    pool.query(queryText)
        .then((result) => {
            res.sendStatus(201);
        })
        .catch((err) => {
            console.log(`Error making query ${queryText}`, err);
            res.sendStatus(500);
        });
});
```

### SQL Injection

Let's put on our hacker hat, and see if we can break this app 😈. Pop open the app in your browser, and try filling out the forms like:

|  |  |
| --- | --- |
| **Artist** | `Paul Simon` |
| **Track** | `Graceland', '1986-10-25', 5); DELETE FROM "songs";--"` |
| **Published** | `1986-10-25` |
| **Rank** | `100` |

Woah... what just happened! Quick, check Postico... Oh no, all of our songs have been deleted from the database!

What the heck just happened?

Let's add some `console.log`s to our code, to see if we can track down this exploit:

```js
router.post('/', (req, res) => {
    const newSong = req.body;
    const queryText = `
        INSERT INTO songs 
            (artist, track, published, rank)
        VALUES 
            ('${newSong.artist}', '${newSong.track}', '${newSong.published}', ${newSong.rank});
    `;
    console.log(`The query we're sending to postgres:`, queryText);
    pool.query(queryText)
        .then((result) => {
            res.sendStatus(201);
        })
        .catch((err) => {
            console.log(`Error making query ${queryText}`, err);
            res.sendStatus(500);
        });
});
```

If we run our "hack" again, we'll see a server log like:

```
The query we're sending to postgres:

INSERT INTO songs (artist, track, published, rank)
VALUES ('Paul Simon', 'Graceland', '1986-10-25', 5); DELETE FROM "songs";--"', '1986-10-25', 100);
```

Or, with some added whitespace:

```sql
INSERT INTO songs (artist, track, published, rank)
VALUES ('Paul Simon', 'Graceland', '1986-10-25', 5);

-- What's this?! A second SQL command!
DELETE FROM "songs";
--"', '1986-10-25', 100);
```

Do you see how `${newSong.track}` was replaced with not only the track, but also a new, malicious, command?!

This is a well-known hack, called "SQL Injection". As in., a malicious user has _injected_ some unwanted SQL code onto our server, and tricked us into executing it. This is super dangerous, as it could expose sensitive data to bad actors (think bank info, health records, etc). Or disable our app by corrupting our database.

> Classic xkcd - Bobby Tables: https://xkcd.com/327/

Moral of the story -- never trust the client! All incoming data should be "sanitized" before we pass it onto other systems. PG does sanitization nicely for us, so we just need to structure our query like this:

```JavaScript
    const newSong = req.body;
    const queryText = `INSERT INTO songs (artist, track, published, rank)
        VALUES ($1, $2, $3, $4);`;
    pool.query(queryText, [newSong.artist, newSong.track, newSong.published, newSong.rank])
```

This is called a [Parameterized Query](https://node-postgres.com/features/queries#parameterized-query), and it prevents any SQL code from being injected into our query. If we try running our hack agin, we'll see it just adds a new song called `Graceland', '1986-10-25', 5); DELETE FROM "songs";--"` to the database.

## Modularizing

### Making a Connection Module
This code could be in *any* module we wanted to talk to our database, but centralizing it in its own module is the D-R-Y way.

> You'll need to use environment variables in order to deploy your pg-pool configuration to deploy to Heroku. That is not covered here.

**modules/pool.js**

```js
// The pg library makes it easy for us to 
// connect and send SQL to a PostgreSQL database
const pg = require('pg');

// A `pool` represents a network connect to our database.
// We send SQL to the database using `pool.query()`
let pool = new pg.Pool({
    host: 'localhost',
    port: 5432,
    database: 'songs', 
});

module.exports = pool;
```

### Making a Router Module

**routes/shoes-router.js**
```JavaScript
// bring in our modules
const pool = require('../modules/pool.js');
const express = require('express');
const router = express.Router();

// When using a router, the part of the path we used to get here is removed.
// We called http://localhost:5000/songs/ to get here (e.g. '/').
router.get('/', (req, res) => {
    let queryText = 'SELECT * FROM songs;';
    pool.query(queryText)
        .then((result) => {
            res.send(result.rows);
        })
        .catch((err) => {
            console.log(`Error making query ${queryText}`, err);
            res.sendStatus(500);
        });
}); // END GET route


router.post('/', (req, res) => {
    const newSong = req.body;
    const queryText = `INSERT INTO songs (artist, track, published, rank)
        VALUES ($1, $2, $3, $4);`;
    pool.query(queryText, [newSong.artist, newSong.track, newSong.published, newSong.rank])
        .then((result) => {
            res.sendStatus(201);
        })
        .catch((err) => {
            console.log(`Error making query ${queryText}`, err);
            res.sendStatus(500);
        });
}); // END POST route

module.exports = router;
```
