# Association Tables



How you choose to design the tables of your database is called the **database schema** and it is a crucial step in planning your application. Think of it as the wireframe for your data.

In this lesson, we'll learn about a few ways to organize your data and how to represent that organization through ERDs.

**Table of Contents**
- [Terms](#terms)
- [How should I create my database?](#how-should-i-create-my-database)
- [Two Tables and Foreign Keys](#two-tables-and-foreign-keys)
	- [Entity Relation Diagrams](#entity-relation-diagrams)
- [Association SQL Queries](#association-sql-queries)

## Terms

* **One-to-Many** - a relationship between two tables in which instances in one table can be referenced by many instances in another table.
* **Many-to-Many** - a relationship between two tables in which the instances of each table can be referenced by many instances in the other table.

## How should I create my database?

Imagine that you are building a social network app for people and their pets. Users can sign up and add pets to their accounts. 

To account for all of the data, you might make a single table:

**SQL:**
```sql
CREATE TABLE all_data (
	id SERIAL PRIMARY KEY,
	owner_name TEXT,
	pet_name TEXT,
	type TEXT
)
```

**`all_data` Table:**

| id  | owner_name     | pet_name   | type |
| --- | -------------- | ---------- | ---- |
| 1   | Ann Duong      | Bora       | bird |
| 2   | Ann Duong      | Tora       | dog  |
| 3   | Ann Duong      | Kora       | cat  |
| 4   | Ben Spector    |            |      |
| 5   | Reuben Ogbonna | Juan Pablo | dog  |
| 6   | Reuben Ogbonna | Pon Juablo | cat  |
| 7   | Carmen Salas   | Khalo      | dog  |
| 8   | Carmen Salas   | Frida      | cat  |

**<details><summary>Q: What are the issues with storing the data in this way?</summary>**
	* You may end up with holes in your database (such as user ben who doesn't have any pets yet)
	* There is a lot of duplicate data in the `owner_name` column
</details>

## Two Tables and Foreign Keys

Instead of storing all data in a single table, we can break up the table into two:
1. A `people` table to store data about people
2. A `pets` table to store data about pets with a _reference_ to the primary key of a person in the `people` table:

**`people` Table:**
| id  | name           |
| --- | -------------- |
| 1   | Ann Duong      |
| 2   | Reuben Ogbonna |
| 3   | Carmen Salas   |
| 4   | Ben Spector    |

**`pets` Table:**
| id  | name       | type | owner_id |
| --- | ---------- | ---- | -------- |
| 1   | Khalo      | dog  | 3        |
| 2   | Juan Pablo | dog  | 2        |
| 3   | Bora       | bird | 1        |
| 4   | Frida      | cat  | 3        |
| 5   | Tora       | dog  | 1        |
| 6   | Pon Juablo | cat  | 2        |
| 7   | Kora       | cat  | 1        |

Now, in order for each pet to "know" who its owner is, the `pets` table needs the `owner_id` column. This column serves as a **foreign key** (a reference to the primary key of another table).

**<details><summary style="color: purple">Q: What are the tradeoffs of this schema design?</summary>**
> We no longer have duplicate data
> It is not exactly clear anymore the name of the person who owns each pet
</details><br>

### Entity Relation Diagrams

With these two tables, a **one-to-many relationship** has been formed: a person can have many pets. This is one of the most common relationships between tables in a database. Some other examples include:
* A `user` has many `posts`
* A `restaurant` has many `reviews`
* An `album` has many `songs`
* Can you think of any more?

We can illustrate the relationships between tables with an **entity relation diagram (ERD)**:

![ERD with one to many and many to many relationships](./img/labeled-erd.png)

> *created using https://dbdiagram.io/*

Above, we've introduced a **many-to-many** relationship that is created using *three* tables: A student can be enrolled in many classes and a class can enroll many students.

The `enrollments` table in the middle is called an **association/junction table** because its entries represent the association of two separate entities. An association/junction table consists entirely of foreign keys.

**Q: Can you think of other many-to-many relationships in the apps you use?**

## Association SQL Queries

**Setup**

```sql
DROP TABLE IF EXISTS people;
DROP TABLE IF EXISTS pets;

CREATE TABLE people (id SERIAL PRIMARY KEY, first_name TEXT, last_name TEXT);
INSERT INTO people (name) VALUES ('Ann', 'Duong');
INSERT INTO people (name) VALUES ('Reuben', 'Ogbonna');
INSERT INTO people (name) VALUES ('Carmen', 'Salas');
INSERT INTO people (name) VALUES ('Ben', 'Spector');

CREATE TABLE pets (id SERIAL PRIMARY KEY, name TEXT, type TEXT, owner_id INTEGER);
INSERT INTO pets (name, type, owner_id) VALUES ('Khalo', 'dog', 3);
INSERT INTO pets (name, type, owner_id) VALUES ('Juan Pablo', 'dog', 2);
INSERT INTO pets (name, type, owner_id) VALUES ('Bora', 'bird', 1);
INSERT INTO pets (name, type, owner_id) VALUES ('Tora', 'dog', 1);
INSERT INTO pets (name, type, owner_id) VALUES ('Frida', 'cat', 3);
INSERT INTO pets (name, type, owner_id) VALUES ('Pon Juablo', 'cat', 2);
INSERT INTO pets (name, type, owner_id) VALUES ('Kora', 'cat', 1);
```

**Q: What is the primary key for each table? Are there any foreign keys?**

Now that we're set up, let's answer some questions! We'll start with a few simple ones

- Who are the cats in our database?
- How many cats are in our database?

Now let's try some harder ones:

- What are all the pets owned by Ann?
- Who is the owner of Frida?
- How many people own cats?
- How many pets does Carmen have?

In order to answer many of these questions, it will be helpful to use a `JOIN` 

```sql
SELECT * 
FROM people 
	JOIN pets 
		ON people.person_id = pets.owner_id;
```

| person.id | name           | pet.id | name        | type | owner_id |
| --------- | -------------- | ------ | ----------- | ---- | -------- |
| 1         | Ann Duong      | 3      | Bora        | bird | 1        |
| 1         | Ann Duong      | 4      | Tora        | dog  | 1        |
| 1         | Ann Duong      | 7      | Kora        | cat  | 1        |
| 2         | Reuben Ogbonna | 2      | Juan  Pablo | dog  | 2        |
| 2         | Reuben Ogbonna | 6      | Pon  Juablo | cat  | 2        |
| 3         | Carmen Salas   | 1      | Khalo       | dog  | 3        |
| 3         | Maya Salas     | 5      | Frida       | cat  | 3        |
| 4         | Ben Spector    | 5      | Frida       | cat  | 3        |


```sql
SELECT * 
	FROM people 
JOIN pets 
	ON people.id = pets.owner_id
WHERE 
	people.first_name = 'Ann';
```
