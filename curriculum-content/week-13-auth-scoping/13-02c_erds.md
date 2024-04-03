# ERDs 

An ERD is an Entity Relationship Diagram.

There are three components in an ERD.

- Entities: Objects; Tables in your database.
- Attributes: Object properties; table columns.
- Relationships: Describe how objects are related or linked together; tables references, foreign keys.

## Entities

Entities are objects or nouns and might be things, events, concepts, locations, or roles. For example: students, courses, books, campus, employees, payments, projects. 

A specific example of an entity is called an instance. Each isntance becomes a record or a row in a table. For example, the student Jo Smith might be a record in a table called students.

## Attributes

Attributes are facts you need to describe each object or entity. They are also often nouns and they will become the columns of the table. For example a student might have a first name, last name, start date and student id.

### Primary Key
The Primary Key or identifier is an attribute, or a set of attributes, that uniquely identifies an instance of that entity. For example, for a student entity, a student number might be used as the primary key since no two students have the same student number. 

- A table can have only one primary key. 
- It identify uniquely every row and it cannot be null.

### Foreign Key
A foreign key (sometimes called a referencing key) is a key used to link two tables together. Typically you take the primary key field from one table and insert it into the other table where it becomes a foreign key (it remains a primary key in the original table). 

- A table can have more than one foreign key.
- A foreign key can be null.


## Relationships
Relationships are  associations between entities. Three types of relationships are discussed in this lab. If you read or hear cardinality ratios, it also refers to types of relationships.

### One to One Relationship (1:1)
A single entity instance in one entity class is related to a single entity instance in another entity class.

For example:
Each student fills one seat and one seat is assigned to only one student.
Each professor has one office space.

### One to Many Relationship (1:M)
A single entity instance in one entity class (parent) is related to multiple entity instances in another entity class (child)

For example:
One instructor can teach many courses, but one course can only be taught by one instructor.
One instructor may teach many students in one class, but all the students have one instructor for that class.

### Many to Many Relationship (M:M)
Each entity instance in one entity class is related to multiple entity instances in another entity class; and vice versa.

For example:
Each student can take many classes, and each class can be taken by many students.
Each consumer can buy many products, and each product can be bought by many consumers.

> ERDs will use Crow's Foot Symbols to represent the relationships. [ERD Relationship CheatSheet](../supporting-documentation/ERD_Relationship_Symbols_Quick_Reference.pdf)

Many to many relationships are difficult to represent, so we decompose a many to many (M:M) relationship into two one-to-many (1:M) relationships.



Example: ![ERD Example](../images/erd-example.png)

Key things that you need to communicate:

- Tables - generally one for each type of object you are storing data for
- Data properties for each table (not shown in example & may be in text vs ERD image)
- Relationships between tables (the lines)
- What type of relationship: 1-1, 1-many, many-many

A couple of the ERD creation tools we like are:

## DB Designer Signup

https://dbdesigner.net

We'll use DB Designer to help make these ERDs.

You'll need to register an account with them. The base level is free -- BUT! 

To get a free year of their Unlimited access(!), you'll need to email them from the email you used to sign up.

---

<info@dbdesigner.net>

SUBJECT: Prime Digtial Academy Student Account

BODY: 
Hello! I'm a student at Prime Digital Academy, and I would like to upgrade my account. Kris Szafranski (kris@primeacademy.io) is my reference for verification. Thank you!

---

## DB Designer Demo

- Make a Postgres Schema
- Make a Table
- Make Columns and Datatypes
- Make Foreign Key / Relationship
- Export? Might only be on Unlimited, but can be nice!

