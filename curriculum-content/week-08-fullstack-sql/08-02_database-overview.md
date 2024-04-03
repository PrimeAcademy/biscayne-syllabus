# Database Overview

Welcome to the full stack.

Databases are the fourth component of our full stack environment.

```
,________,         .------,          .------,         .------.
|________|       ,'_____,'|        ,'_____,'|        (        )
|        |       |      | |        | ____ | |        |~------~|
|        |       |      | |        | ____ | |        |~------~|
|        |       |      | ;        | ____ | ;        |~------~|
|________|       |______|'         |______|'         `.______.'
 HTML/CSS          CLIENT            SERVER           DATABASE
```

## Why?

So far, we've used our node servers for our routes, and to help persist our data beyond the life of a browser refresh. But what happens when we shut down the server and restart it? We run into the same problem. Sure, servers don't shut down all that often, but can you imagine if your bank server shut down? It just doesn't work!

Up to this point, all the variables and data you've used, collected, and manipulated, have been just in the computers memory. A database, at it's most basic, allows us to save information to a hard drive and allow it to persist past the life of the server, and even survive the database restarting.

- Data persistance
- Access from multiple sources
- Scales much better

## Wait, what?

Think of your computer. You have RAM and a Hard Drive. RAM is like our variables -- restart the computer, close the application, etc, and it all evaporates. But the information on the Hard Drive lasts until you remove it, or it crashes, but lets not hope for that!

So too is our Database. It will hold all the data you need to persist. Form input values? Database. Likes? Database. Users? Database. Every single shard of info that needs to be used at a later date needs to be saved in the Database.

## Okay... so that's neat. What's a Database?

A database is a few things all at once. 
First, its where we store our data.
We'll also refer to it as the Database Server. Technically, you have to have something *running* in order for it to accept new commands.

Think of one of the most common ways information in an office is stored: Excel Spreadsheets. This is a good starting reference for postgreSQL, which is like Excel. There are columns and rows in SQL, just the same as in Excel.

## POSTGRESQL

SQL stands for Structured Query Language. It is not Javascript!

There are many flavors of SQL -- MYSQL, MSSQL, etc. They all are slightly different, but the basics are the same between them all.

PostgreSQL is a SQL database. It uses a different language to query your rows and columns. It has it's own syntax. You have to install it, like node. 


## Tables, Rows, Columns, Datatypes

SQL, conceptually, is like a Excel Spreadsheet. 

```
+----+------+------------+---------------+----------+
| id | rank |   artist   |     track     |   date   |
+----+------+------------+---------------+----------+
|  1 |    3 | The Doors  | Light My Fire | 1/1/1967 |
|  2 |    7 | Oasis      | Wonderwall    | 1/1/1996 |
|  3 |   10 | Neil Young | Heart of Gold | 1/1/1972 |
+----+------+------------+---------------+----------+
```

The whole thing would be referred to as a *table*.
*columns* are the headings.
*rows* are the entries.

The big thing we need to think about is data types -- think back to your weekend project -- you could conceivably push objects into your array that have different keys & types, but that would be super confusing! SQL would NOT EVEN BEGIN TO ALLOW THIS. This makes SQL super dependable, but puts a lot of the responsibility on us to think about what we'll be saving. 

Each column has a *data-type* associated with it.

To make it manageable, we'll need to be thinking ahead of what data we need to collect.

### What do we need to collect, what is it going to be?

Let's look at a common form. Let's look at all the data that we'll need to save. 

Make a list of the generic things you'll be saving. These are your *columns*

Now, let's think of the data type for each of them. Our columns will be of that type.

BOOM! You've made an idea of a *table*.

It describes what we're expecting this data to look like.

What might one row look like? What does it represent?


### Takeaways

- DBs are how we store our data
- DBs are a different part of our stack
- SQL conceptually looks like an Excel Spreadsheet *table*, with *rows* and *columns*
- SQL has *data types* associated with columns
- Everything that persists needs to be stored in a database

