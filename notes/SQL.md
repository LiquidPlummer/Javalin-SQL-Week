# SQL
SQL, or Structured Query Language, is a scripting language used to manipulate relational databases, commonly referred to as SQL databases. In the strictest sense SQL instructs a relational database management system in a similar way to how Java instructs the compiler and JS instructs the interpreter.

## SQL Sublanguages
The types of SQL commands used to query and manipulate data within a database can be categorized into 5 sub-languages (or dialects).

1. **DDL Data Definition Language:** Statements used to create tables and databases as well as defined properties.
    * `CREATE`, `ALTER`, `DROP`, `TRUNCATE`
2. **DML (Data Manipulation Language):** Statements used to insert or remove data from tables.
    * `INSERT`, `UPDATE`, `DELETE`
3. **DQL (Data Query Language):** Statements used to query data from a table.
    * `SELECT`
4. **DCL (Data Control Language):** Statements used to control who can access data.
    * `GRANT`, `REVOKE`
5. **TCL (Transaction Control Language):** Statements used to commit and restore data through transaction.  Transactions group a set of tasks into a single execution unit.
    * `COMMIT`, `ROLLBACK`, `SAVEPOINT`










<img src="./images/sublanguages-1.jpg" width="600"/>

## Creating Tables with DDL
Let's begin creating a **Users** table in our **public** schema by using **DDL**:

    *Note on conventions: Generally SQL commands are typed in caps, non-keywords tend to be lowercase. SQL does not use camel case, rather it uses snake_case. For most SQL syntax, it is not case-sensitive.*

1. Open a new SQL script editor by cicking on the icon in the top left-hand corner that says `New SQL Editor`.



2. In the editor type the following SQL statements. `--` denotes a comment:
* The *lowercase words* are the names of the columns.
* The *uppercase words* that follow them are the **datatypes** and **constraints**.
    * **Data Types** dictate what type of data can be inserted into that column. <br> PostgreSQL data types include:
        * Character types such as `CHAR` , `VARCHAR` , and `TEXT`
        * Numeric types such as `INTEGER`, `DECIMAL`, `SMALLINT`, `BIGINT`
        * Temporal types such as `DATE`, `TIME`, `TIMESTAMP`, and `INTERVAL`

    *  **Constraints** used to limit the type of data that can go into a table. This ensures the accuracy and reliability of the data.

```sql
CREATE TABLE IF NOT EXISTS users (
	id SERIAL PRIMARY KEY,
	first_name VARCHAR(30) NOT NULL,
	last_name VARCHAR(30),
	email VARCHAR(50) NOT NULL UNIQUE
);
```

Above we see an example of a few constraints applied to columns:
* `PRIMARY KEY` - A combination of a `NOT NULL` and `UNIQUE`. Uniquely identifies each row in a table.
    * `SERIAL` - autoincrements the primary key by 1 each time a new record is added
* `NOT NULL` - Ensures that a column cannot have a `NULL` value.
* `FOREIGN KEY` - Uniquely identifies a row/record in another table.
* `UNIQUE` - ensures that all values in a column are different.
    
3. Let's create another table called **posts**.  This table will include `FOREIGN KEY`'s which references the `user` that authors the particular post, as defined by its constraint.  

Enter the following SQL code into the existing script:

```sql
CREATE TABLE IF NOT EXISTS posts (
	id SERIAL PRIMARY KEY,
	author_id INTEGER REFERENCES users(id) NOT NULL,
	wall_user_id INTEGER REFERENCES users(id) NOT NULL,
	post_content VARCHAR(500) NOT NULL
);
```

4. Execute the entire script.  Right click on the `public` schema in the left hand side of your Database Navigator and click `refresh`.  You should have created your tables and your script will look like the following:

<img src="./images/script-1.png" width="600"/>


5. Now let's use *DDL* to modify the tables with `ALTER`.  Enter the following code into your editor:

```sql
-- ALTER TABLE - used to modify an existing table
ALTER TABLE users
	ADD COLUMN test INTEGER;
ALTER TABLE users
	ALTER COLUMN test SET DATA TYPE VARCHAR(2);
ALTER TABLE users
	ALTER COLUMN test SET DEFAULT 'hi';

-- UPDATE all null values to 'hi'
UPDATE users SET test = COALESCE(test, 'hi');

ALTER TABLE USERS
	ALTER COLUMN TEST SET NOT NULL;
ALTER TABLE users
	DROP COLUMN test;

-- TRUNCATE TABLE - Removes all table data, but leaves table
-- TRUNCATE TABLE users;

--DROP TABLE posts;

-- You cannot drop users while posts exists, as posts is dependent upon users

-- You can drop users and remove the FK relationship by using CASCADE.
-- DROP TABLE users CASCADE;
```

6.  You can also create a `likes` table with a `COMPOSITE KEY` which is a unique identifier that is a combination of multiple columns:

```sql
DROP TABLE IF EXISTS likes;

CREATE TABLE IF NOT EXISTS likes (
	user_id INTEGER REFERENCES users(id) NOT NULL,
	post_id INTEGER REFERENCES posts(id) NOT NULL,
	PRIMARY KEY (user_id, post_id)
);
```

7.  Highlight your most recent entry to create the `likes` table, run it, and right click on **public** schema.  Click `refresh`.

8. Right click on your schema again.  Click `View Diagram`.  You should open a view of your Entity Relationship Diagram which will look like the following.

<img src="./images/erd.png" width="600"/>

## Inserting & Updating Data with DML
To *insert* data into tables, we use the following syntax:

```sql
INSERT INTO table_name (col1_name, col2_name) VALUES (col1_value, col2_value), (...);
```
1. Insert a single record into the `users` table with the following statement:

```sql
INSERT INTO users (first_name, last_name, email) VALUES 
	('Hayden', 'Fields', 'hayden.fields@mail.com');
```

2. Insert multiple users into the `users` table with the following SQL statement:

```sql
INSERT INTO users (first_name, last_name, email) VALUES
    ('Harry', 'Smith', 'hsmith@mail.com'),
    ('Larry', 'King', 'lking@mail.com'),
    ('Mary', 'Shelley', 'mshelley@mail.com'),
    ('Terry', 'Miller', 'tmiller@mail.com');
```

3. Run the `INSERT` statement. Right click on the schema, click `refresh`.  Your `users` table should now reflect the following:

<img src="./images/users-table.png" width="600"/>


>You can use the `UPDATE` **DML** Statement along with a `WHERE` clause within your script to demo updating data that has already been persisted.  For example, if we want to update the email address for `Mary Shelley` to `frankenstein@mail.com`, we would run the following **DML** statement like so:

```sql
UPDATE users SET email = 'frankenstein@gmail.com' WHERE id = 4;
```

You don't have to, but demonstrate that we could also delete rows using the `DELETE` operation.
```SQL
DELETE FROM users WHERE id = 4;
```
Note: Again it is important to utilize WHERE.
`DELETE FROM users;` would delete all data in the table.

### Associate Exercise:
Associates will be tasked with inserting data into the other two tables.  Their tasks are the following:

* Harry should post a message on Hayden's walls, which Terry likes
* Mary should post a message on her own wall, which both Harry and Terry like
* Hayden will post a message on Terry's wall, which Mary and Hayden like

### Solution to Exercise:

```sql
INSERT INTO posts (author_id, wall_user_id, post_content) VALUES
    (2, 1, 'Hey Hayden, it''s Harry!'),
    (4, 4, 'I wrote Frankenstein.'),
    (1, 5, 'Happy Birthday, Terry!');

INSERT INTO likes (user_id, post_id) VALUES
    (5, 1),
    (2, 2),
    (5, 2),
    (4, 3),
    (1, 3);
```

## Querying Data with DQL
The `SELECT` statement is the main operation used when querying data. We filter the data we want to retrieve by using the `WHERE` clause.

1. Return all data within the `users` table with the following statement:

```sql
SELECT * FROM users;
```
2. You can specify the value's you're querying for like so:

```sql
SELECT first_name, last_name FROM users;
```

3. **Aliases** are temporary names for columns or tables that you can apply when querying data. You can use the `CONCAT` function in postgres to return the concatendated first and last name of each user under an alias. For example:

```sql
SELECT
    CONCAT (first_name, ' ', last_name) AS "Full Name"
FROM users;
```

4. There are many ways to filter your result sets:
    * `ORDER BY`: sorts result-sets in ascending or descending order.
    * `LIMIT`: limit result set to a specified amount.
    * `OFFSET`: specifies the number of rows to skip before starting to return rows from the query.

    >**Example**
    >```sql
    >SELECT * FROM users 
	>   WHERE first_name > 'g' 
	>   ORDER BY last_name 
	>   LIMIT 2 OFFSET 1;
    >```

<br>

### Continue to [this example](https://gitlab.com/revature_training/postgres-team/-/blob/master/postgres-sql-standard-examples/phase-1/inner-queries.md) to demonstrate advanced querying with **Inner Queries**, `GROUP BY `, `HAVING`, and `LIKE`.
