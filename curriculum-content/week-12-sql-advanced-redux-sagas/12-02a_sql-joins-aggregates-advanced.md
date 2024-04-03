# SQL Advanced - Many - Many, Aggregates

- [SQL Min and Max](https://www.w3schools.com/sql/sql_min_max.asp)
- [SQL Count, Avg, Sum](https://www.w3schools.com/sql/sql_count_avg_sum.asp)

## Many to Many

### JOINS
```SQL
-- Create our tables
CREATE TABLE "person" (
  id SERIAL PRIMARY KEY,
  name VARCHAR(25)
);

INSERT INTO "person" (name)
VALUES ('Chris'), ('Logan'),
('Bri'), ('Lisa'), ('Sara');

-- SELECT column_name FROM table_name
SELECT "id","name" FROM "person";

CREATE TABLE "hobby" (
  id SERIAL PRIMARY KEY,
  description VARCHAR(100)
);

INSERT INTO "hobby" ("description")
VALUES ('Kayaking'), ('Pottery'),
('Biking'), ('Cooking'), ('Swing Dancing');

SELECT * FROM "hobby";

CREATE TABLE "person_hobby" (
  id SERIAL PRIMARY KEY,
  person_id INT REFERENCES "person",
  hobby_id INT REFERENCES "hobby",
  skill INT
);

INSERT INTO "person_hobby" ("person_id", "hobby_id", "skill") VALUES (5, 2, 3), (5, 3, 2), (5, 5, 5),
(4, 4, 2), (4, 5, 2), (2, 3, 1), (2, 1, 3),
(3, 2, 2), (3, 1, 1), (3, 4, 3), (1, 1, 2);

-- Alias id as person_id
SELECT "id" as "person_id" FROM "person_hobby";

SELECT * FROM "person" JOIN "person_hobby" ON
"person"."id" = "person_hobby"."person_id";

-- SELECT all data joined together
SELECT * FROM "person"
JOIN "person_hobby" ON "person"."id"="person_hobby"."person_id"
JOIN "hobby" ON "hobby"."id" = "person_hobby"."hobby_id";

-- SELECT hobbies for a specific user (person_id = 3)
SELECT "hobby"."description", "person_hobby"."skill"
FROM "hobby" JOIN "person_hobby"
ON "hobby"."id" = "person_hobby"."hobby_id"
WHERE "person_hobby"."person_id" = 3;
```

### AGGREGATES

```SQL
-- Aggregates
SELECT count(*) FROM "person";

-- Minimum value for skill
SELECT MIN("skill") FROM "person_hobby";

-- Maximum value for skill
SELECT MAX("skill") FROM "person_hobby";

SELECT AVG("skill") FROM "person_hobby";

SELECT SUM("skill") FROM "person_hobby";

SELECT MIN("skill"), MAX("skill") FROM "person_hobby";

-- How many users have each hobby?
SELECT "hobby"."description", count("person_hobby"."hobby_id") FROM "hobby"
JOIN "person_hobby"
ON "hobby"."id" = "person_hobby"."hobby_id"
GROUP BY "hobby"."description";
```


## Additional Resources

- [SQL Aliases](https://www.w3schools.com/sql/sql_alias.asp)
- [SQL Group By](https://www.w3schools.com/sql/sql_groupby.asp)