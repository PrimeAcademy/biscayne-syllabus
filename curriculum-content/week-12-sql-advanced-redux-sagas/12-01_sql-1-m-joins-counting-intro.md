# SQL JOINS

[Code from Lecture](https://github.com/PrimeAcademy/fomalhaut-sql-joins-intro/)

- [SQL Joins](https://www.w3schools.com/sql/sql_join.asp)

## Relational Data

SQL is a Relational Database. That means its a database structured to recognize relations among stored items of information. We so far have only used one table, but in SQL, we will almost always have multiple tables that have relationships with each other.

### Database Normalization

Normalization is the process of breaking down and organizing database tables that describe the relationships between them. We do this so that when we make changes it doesn't have to be painful! More information can be found [here](https://www.essentialsql.com/get-ready-to-learn-sql-database-normalization-explained-in-simple-english/) and [here](https://blog.udemy.com/normalization-in-database-with-example/).

## Relationships

In order to normalize your database, you have to think about the relationships between different data in your database.

What's the relationship between a person and a SSN? A person can only have one SSN, and a SSN is owned by one person. This is  an example of ONE-TO-ONE. 

Other examples -- Countries and Capital Cities. 

 

## One-to-One

```sql
CREATE TABLE "person" (
  "id" SERIAL PRIMARY KEY,
  "name" VARCHAR(50) NOT NULL,
);

CREATE TABLE "social_security" (
  "id" SERIAL PRIMARY KEY,
  "person_id" INT REFERENCES "student",
  "number" INT NOT NULL
);

INSERT INTO "person" ("name") VALUES ('dane');
INSERT INTO "social_security" ("number", "person_id") VALUES (1234, 1);


-- Get address for a student
SELECT * FROM "person"
JOIN "social_security" ON "person"."id" = "social_security"."person_id"
WHERE "student"."id" = 1;

```

## One-to-Many

What's the relationship between Owners and Pets? 
An owner can have many Pets, a pet can have only 1 owner. This is an example of ONE-TO-MANY. 

Other examples -- Students and cohorts.

```SQL
CREATE TABLE "cohort" (
  "id" SERIAL PRIMARY KEY,
  "name" VARCHAR(25),
  "start_date" DATE
);

CREATE TABLE "student" (
  "id" SERIAL PRIMARY KEY,
  "name" VARCHAR(50) NOT NULL,
  "cohort_id" INT REFERENCES "cohort"
);

INSERT INTO "cohort" ("name", "start_date")
VALUES ('Rigel', '8/27/2018'),
('Kochab', '1/1/2018');


INSERT INTO "student" ("name", "cohort_id")
VALUES ('Jake', 1),
('Ellen', 2),
('Manny', 1);

-- Select all cohorts, but no student info
SELECT * FROM "cohort";


-- Select everyone, but not their cohort info
SELECT * FROM "student";

-- Select everyone and see their cohort
SELECT * FROM "student"
JOIN "cohort" ON "cohort"."id"="student"."cohort_id";
```


NOTE duplicate column names! `id`, `name` are ambiguous. This is a problem for SQL but also a problem in the future when these results are smashed into a JS object.

Let's try:

```sql
-- let's try asking for specific columns
SELECT name, start_date FROM cohort
JOIN student ON cohort.id = student.cohort_id;
```

Oh no! Now SQL doesn't know which `name` column in which table! They are "ambiguous"

Once we havae more than one table we're selecting from, we need to specify which table and columns we want to select. And should ALIAS them:

```sql
-- table.column_name and aliased
SELECT cohort.name AS cohort_name, student.name AS student_name FROM cohort
JOIN student ON cohort.id = student.cohort_id;
```

### Counting

SQL has some built in functionality, as well! Let's explore `count`;


```SQL
-- Count all students
SELECT count(*) FROM "student";

-- Count all students in a cohort
SELECT count(*) FROM "student"
JOIN "cohort" ON "cohort"."id"="student"."cohort_id"
WHERE "cohort"."name" = 'Rigel';
```

Others: MAX, SUM, AVERAGE

Theres also a `SUM()` aggregate function -- it adds the selected fields together.

### Group By

When counting, sometimes we need to do more than count by itself. Say we want the names of the cohort and how many people are in each one. In order to handle the potential counting craziness, we need to tell SQL how to group our data together before it counts. 

The GROUP BY statement groups rows that **have the same values** into summary rows, like "find the number of customers in each country".

The GROUP BY statement is often used with aggregate functions (COUNT(), MAX(), MIN(), SUM(), AVG()) to group the result-set by one or more columns.


```SQL

-- Count all students in a cohort grouped by cohort name
SELECT "cohort"."name", count(*) FROM "student"
JOIN "cohort" ON "cohort"."id"="student"."cohort_id"
GROUP BY "cohort"."name";

```

Group and Order by:

```sql
SELECT "cohort"."name", count("student") FROM "student"
RIGHT JOIN "cohort" ON "cohort"."id"="student"."cohort_id"
GROUP BY "cohort"."name"
ORDER BY count ASC;
```


**OPTIONAL CONTENT**
Students at some point will need this, but every group is different.

### JOIN types
What if we make a new cohort? And it doesnt have any students (yet!).
`INSERT INTO "cohort" ("name", "start_date") VALUES ('alpha', '1-1-1970');`

Lets try counting! Same SQL as before:


```SQL

-- Count all students in a cohort grouped by cohort name
SELECT "cohort"."name", count(*) FROM "student"
JOIN "cohort" ON "cohort"."id"="student"."cohort_id"
GROUP BY "cohort"."name";

```

BUT! We DONT see the alpha cohort! Why?
https://www.w3schools.com/sql/sql_join.asp

JOIN is by default, an INNER JOIN. The venn diagram will get you all the intersecting data, but ignore cohorts without students.

LEFT JOIN means include all the FROM table, RIGHT JOIN means include all from the JOIN table. FULL OUTER JOIN means ALL THE THINGS. 

```SQL

-- Count all students in a cohort grouped by cohort name
SELECT "cohort"."name", count("student") FROM "student"
RIGHT JOIN "cohort" ON "cohort"."id"="student"."cohort_id"
GROUP BY "cohort"."name";

```



### Sorting

We can control the order of the results by adding ORDER BY and ASC or DESC. This will keep your results in a specific sorting order.

```SQL
-- Count all students in a cohort
SELECT count(*) FROM "student"
JOIN "cohort" ON "cohort"."id"="student"."cohort_id"
WHERE "cohort"."name" = 'Rigel'
ORDER BY "student"."name" DESC;
```
