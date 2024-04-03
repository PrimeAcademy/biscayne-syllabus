# Introduction to SQL

[Code from Lecture](https://github.com/PrimeAcademy/fomalhaut-sql-basics)


## SQL
Structured Query Language (SQL) is a standard computer language for relational database management and data manipulation. SQL is used to query, insert, update and modify data.


## Intro to Relational Databases
We will be using Postgres for our database and Postico for our client. Keep in mind you can use Postgres with Node.js, SQLite for mobile or MySQL for PHP. SQL is common across all of these different platforms.

Designed to be able to efficiently read and write large amounts of data when properly configured

 - Provides ACID guarantees:
  - Atomicity - each transaction is all or nothing
  - Consistency - each tx brings DB from valid state to a new valid state. All constraints still hold
  - Isolation - concurrent transactions should appear serial to the user, no in progress tx should know about data from another in progress tx
  - Durability - withstand crashes and power failures

## Intro to Postico

![Postico Postgres Client](images/postico-client.jpg)

- Start your database
Open `postgres.app`, ensure it is running and started

- Open Postico
- Create a database named `music_library`

```
CREATE DATABASE "music_library";
```

- Create your first table

```SQL
CREATE TABLE "songs" (
  "id" serial primary key,
  "rank" integer,
  "artist" varchar(80) not null,
  "track" varchar(120) not null,
  "published" date
);
  
CREATE TABLE name (
  column-name data-type constraints,
  ...
);  
```

![Tables in Database](images/tables-in-database.jpg)

- Discuss [data types](https://www.postgresql.org/docs/8.1/static/datatype.html)
  - `serial` auto increment
  - `integer` number
  - `varchar(##)` string with max characters ##
  - `date` date
- `cmd-r` to refresh, our table exists!
- Manually enter data (GUI)


  > 1 - Add Billy Joel - We Didn't Start the Fire - 1/1/1989

- Click on **Show SQL**
- Edit inline
- Navigate around
- Go back to the SQL Query

### INSERT
Add a new row to the database. Only fields marked as NOT NULL are required. Column names are entered using `""` and values are entered with `''`.

```
INSERT INTO "songs" ("id", "rank", "track", "artist", "published") 
VALUES (1, 357, 'Wonderwall', 'Oasis', '1-1-1996');
```

- What went wrong? **Our id must be unique.**
- Let's try that again without an **id** (it will auto increment)

```SQL
INSERT INTO "songs" ("rank", "track", "artist", "published") 
VALUES (357, 'Wonderwall', 'Oasis', '1-1-1996');
```

- INSERT multiple records

```SQL
INSERT INTO "songs" ("rank", "track", "artist", "published") 
VALUES (357, 'Wonderwall', 'Oasis', '1-1-1996'),
(102, 'Under the Bridge', 'Red Hot Chili Peppers', '1-1-1992');
```


- `cmd-r` to refresh, our song exists!

#### Import Data
This could take a while... lets add a bunch of new songs from [the songs.sql file](/curriculum-content/supporting-documentation/songs.sql).

> Share the songs.sql file from this repo via slack. Have students Load Query through Postico that file. Sharing via copy/paste can get messy.

### SELECT
Queries for data in the database.

```SQL
/* Wildcard for select, returns all rows & columns */
SELECT * FROM "songs";

/* Pick specific columns */
SELECT "track", "artist" FROM "songs";

/* LIMIT the number of rows, must be last! */
SELECT * FROM "songs" LIMIT 10; 

/* WHERE selects specific data */
SELECT * FROM "songs" 
WHERE "id" = 1;

/* Results that match... % match all. */
SELECT "track", "artist" FROM "songs" 
WHERE "track" LIKE '%Yak%' LIMIT 10;

/* SELECT all columns */
SELECT * FROM "songs" WHERE "track" ILIKE '%yak%';

SELECT "track", "artist", "published" FROM "songs" 
WHERE "published" > '1/1/2016' LIMIT 10;

/* ORDER BY column_name ASC or DESC */
SELECT "track", "artist", "published" FROM "songs" 
WHERE "published" > '1/1/1980'
ORDER BY "published" DESC LIMIT 20;

/* 
WHERE conditions can be joined with AND, OR & NOT 
SELECT all columns from songs where the artist has an 'a' in the name AND was published after 1/1/2000 OR track has fire in the name.
*/
SELECT * FROM "songs"
WHERE ("artist" LIKE '%a%' AND "published" > '1/1/2000')
OR "id" > 400;

/* Items in [] are optional. In summary:
SELECT column
FROM table
[WHERE conditions]
[ORDER BY column [ ASC | DESC ]]
LIMIT number_rows [ OFFSET offset_value ];
*/
```

### UPDATE
Updates an existing record.

`UPDATE "songs" SET "artist"='Chris Black' WHERE id = 1;`

   > **DON'T FORGET THE WHERE!**

Update Wonderwall to rank #1

`UPDATE "songs" SET "rank"=1 WHERE "track" = 'Wonderwall';`

More update examples.


### DELETE
Deletes an existing record.

```SQL
/* Test before DELETE */
SELECT * FROM "songs" WHERE "artist" LIKE 'Kanye West';
/* Actually DELETE */
DELETE FROM "songs" WHERE "artist" LIKE 'Kanye West';

/* In summary 
DELETE FROM table
[WHERE conditions];
*/
```


### Changing Table Structure

There are a series of commands for "altering" table structures which are used for changing column names, types, adding or remove columns, etc.

[Postgres Alter Table docs with examples.](https://www.postgresql.org/docs/7.4/static/sql-altertable.html)


### Drop a table

Completely removes a table and all of its rows/data. **This is Permanent.**

```sql
DROP TABLE songs;
```
