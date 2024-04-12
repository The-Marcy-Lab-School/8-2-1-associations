# 8-2-2-associations
SQL - Association Tables

### What is a relational database?

PostgreSQL is an **object-relational DBMS**, meaning that in addition to storing its data as objects, it allows for relationships between objects across tables.

Here is another table in the same database called `actor`:

![](./img/actor-table.png)

Every table has a column that serves as the **primary key**, a unique value assigned to each row in the table. 

> **Q: What is the primary key in the tables above?**
> 
> <details><summary>Answer</summary>
> <br>
>
> `actor_id` is the primary key for the `actor` table. `film_id` is thep primary key for the `film` table.
> 
> </details>

Relationshps can be established between two tables through an **association table**. This table is called `film_actor`:

![](./img/film-actor-association-table.png)

When the primary keys of another table are used in a association table, they are called **foreign keys**.

> **Q: I want to know what movie `actor_id = 3` is in. How would I find it?**
> 
> <details><summary>Answer</summary>
> 
> 1. Look at the `film_actor` table and find the row where `actor_id = 3`. 
> 2. Take note of the `film_id`. 
> 3. Then, in the `film` table, find the row with the `film_id` you found earlier. 
> 4. Then look at the `title` column!
> 
> </details>



### What is a relational database?

PostgreSQL is an **object-relational DBMS**, meaning that in addition to storing its data as objects, it allows for relationships between objects across tables.

Here is another table in the same database called `actor`:

![](./img/actor-table.png)

Every table has a column that serves as the **primary key**, a unique value assigned to each row in the table. 

> **Q: What is the primary key in the tables above?**
> 
> <details><summary>Answer</summary>
> <br>
>
> `actor_id` is the primary key for the `actor` table. `film_id` is thep primary key for the `film` table.
> 
> </details>

Relationshps can be established between two tables through an **association table**. This table is called `film_actor`:

![](./img/film-actor-association-table.png)

When the primary keys of another table are used in a association table, they are called **foreign keys**.

> **Q: I want to know what movie `actor_id = 3` is in. How would I find it?**
> 
> <details><summary>Answer</summary>
> 
> 1. Look at the `film_actor` table and find the row where `actor_id = 3`. 
> 2. Take note of the `film_id`. 
> 3. Then, in the `film` table, find the row with the `film_id` you found earlier. 
> 4. Then look at the `title` column!
> 
> </details>


### Entity Relation Diagrams

An **Entity Relation Diagram (ERD)** illistrates the properties of tables (a.k.a. "entities") and their relationships with other tables/entities.

![](./img/primary-key-foreign-key.png)