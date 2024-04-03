# Optional SQL Practice

## Overview

Students need to complete and ERD from given data and answer 4 SQL word problems based on the same table and data provided in [Part 1](../../rubrics/weekend12-saga-movies.md).

## Links

> SQL junction table and joins

- Starting repo: [repo](https://github.com/PrimeAcademy/weekend-movies-sql-practice)

## Example Feedback

Some decent examples of the kinds of feedback needed for this assignment.

- https://portal.primeacademy.io/#/assignments/7522

---

## Markdown Template

```
| SQL  | Complete? |
| --- | :---: |
| 1. Select statement retrieves all Adventure movies by using id | no |
| 2. Select count of movies for each genre | no |
| 3. Add `Superhero` genre to `Star Wars` | no |
| 4. Remove `Comedy` genre from `Titanic` | no |
| 5. STRETCH: All movies with all genres using aggregate | no |
| 6. STRETCH: Delete `The Martian` and related data | no |
```

## SQL Key

1. All adventure movies:

```sql
SELECT * FROM movies_genres
JOIN movies ON movies.id = movies_genres.movie_id
WHERE movies_genres.genre_id = 1;
```

2. Get the count of movies that have each genre. Ignore genres with 0 movies (like Disaster).

```sql
SELECT genres.name, COUNT(*) 
FROM movies_genres
JOIN genres ON genres.id = movies_genres.genre_id
GROUP BY genres.name;

Results:
Adventure	2
Musical	    2
Romantic	1
Epic	    1
Comedy  	6
...more

```

**BONUS IF YOU'RE ASKED**

Include genres with ZERO movies assigned (thanks Edan)

The trick is:

- You need a `FULL JOIN` or a `RIGHT JOIN` to get all the genres listed
- You need to pass a field from the `movies_genres` to `COUNT()`, so that only the items in that table are counted

```sql
-- count all include 0s
SELECT genres.name, COUNT(movies_genres.id)
FROM movies_genres
FULL JOIN genres ON genres.id = movies_genres.genre_id
GROUP BY genres.name;
```


3. Add the genre "Superhero" to "Star Wars"

```sql
INSERT INTO movies_genres (movie_id, genre_id)
VALUES (10, 13);
```

4. Remove the "Comedy" genre from "Titanic"

```sql
-- direct with junction id
DELETE FROM movies_genres
WHERE id = 21;

-- using movie id && genre_id
DELETE FROM movies_genres
WHERE movie_id = 13 AND genre_id = 4;
```

---

## STRETCH GOALS

5. All movies with all of their genres in single row (array_agg or json_agg):

```sql
SELECT movies.title, array_agg(genres.name) FROM movies 
JOIN movies_genres ON movies.id = movies_genres.movie_id
JOIN genres ON movies_genres.genre_id = genres.id
GROUP BY movies.title   -- required
ORDER BY movies.title;  -- nice to have

RESULTS (partial):
---
Avatar	{Adventure,Biographical,Comedy}
Beauty and the Beast	{Adventure,"Science Fiction",Space-Opera}
Captain Marvel	{Biographical}
<more>


-- Using json_agg() is the same except the aggregate name :)
RESULTS:
---
Avatar	["Adventure", "Biographical", "Comedy"]
Beauty and the Beast	["Adventure", "Science Fiction", "Space-Opera"]
Captain Marvel	["Biographical"]
<more>
```


6. Delete the movie "The Martian". It has associated genres data...

You can add ON CASCADE constraints to the table.

You can also simply delete the row in the junction table and then delete The Martian from the movies table. This can be done using sub-selects instead of 2 separate queries.

```sql
-- delete all genre entries from movies_genres for The Martian (id 11)
DELETE FROM movies_genres
WHERE movie_id = 11;

-- delete movie by id
DELETE FROM movies 
wHERE id = 11;
```
