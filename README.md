## SQL 

### 1. Install SQL 
- [install - PostgreSQL](https://www.postgresql.org/)

#### What is SQL
- SQL (Structured Query Language), also pronounced “sequel”, is a programming language used to CRUD data stored in a relational database.
- Keep in Mind That...
    - SQL keywords are NOT case sensitive: select is the same as SELECT
    - Semicolon after SQL Statements. Some database systems require a semicolon at the end of each SQL statement.

### 2. Start PostgreSQL 
- Open a terminal session and type: `psql`. You’ll see your PostgreSQL version and psql’s prompt:
```bash
$ psql
psql (14.5)
Type "help" for help.
```
```bash
help -- general help
\?   -- help with psql commands
\h   -- help with SQL commands
\l   -- Lists all databases
\c   -- Connect to a database
\d   -- List tables in database
\q   -- exits psql
q    -- exits a psql list or dialogue

```
### 3. Create Database, tables and set up schema 
 - ### **Schema**

The structure of a particular database is defined by its **schema**.

Schemas define the database’s:

1. Tables, including the number and data type of each column
2. Indexes for efficient access of data
3. Constraints (rules, such as whether a field can be null or not)
![](https://github.com/miya-w/2024-SQL-note/blob/main/images/sql-table.png)
### **Rows (Records)**

A row in a table represents a single instance of the data entity.

For example a particular **artist** in the **artists** table.

### **Columns (Fields)**

The columns of a table have a:

- Name
- Data type (all data in a column must be of the same type)
- Optional constraints

PostgreSQL has [many data types](https://www.postgresql.org/docs/11/datatype.html) for columns, but common ones include:

- `integer`
- `decimal`
- `varchar` (variable-length strings)
- `text` (same as `varchar`)
- `date` (does not include time)
- `timestamp` (both date and time)
- `boolean`

Common constraints for a column include:

- `PRIMARY KEY`: column, or group of columns, uniquely identify a row
- `REFERENCES` (Foreign Key): value in column must match the primary key in another table
- `NOT NULL`: column must have a value, it cannot be empty (null)
- `UNIQUE`: data in this column must be unique among all rows in the table
![](https://github.com/miya-w/2024-SQL-note/blob/main/images/sql-datebase-pk-fk.png)
### **Primary Keys (PK) and Foreign Keys (FK)**

The field (or fields) that uniquely identify each row in table are know known as that table’s **primary key (PK)**.

Since only one type of data entity can be held in a single table, related data, for example, the **songs** for an **artist**, are stored in separate tables and “linked” via what is known as a **foreign key (FK)**. Note that foreign key fields hold the value of its related parent’s PK.

## Example : Band and Musicians 
```sql
-- Create Database -- Don't forget the semicolon!
CREATE DATABASE music; 

\l -- What changed?

\c music -- Connect to the music database

\d -- Lists all tables


--- Define a table's schema ---
CREATE TABLE bands (
  id serial PRIMARY KEY,  #serial is auto-incrementing integer
  name varchar NOT NULL,
  genre varchar
);

\d  --There should now be a bands table

```
### Insert value into bands table
```sql
SELECT * FROM bands; -- The * represents all fields

-- For text, use single quotes, not double
INSERT INTO bands (name) VALUES ('The Smiths');

INSERT INTO bands (name, genre) VALUES ('Rush', 'prog rock');

SELECT * FROM bands; -- Use the up arrow to access previous commands

```
### Create Musician table

**Creating a Table for a Related Data Entity**

```sql
-- The REFERENCES constraint is what makes a column a FK.
CREATE TABLE musicians (
  id serial PRIMARY KEY,
  name varchar NOT NULL,
  quote text,
  band_id integer NOT NULL REFERENCES bands (id)-- REFERENCES creates a FK constraint
);

\d musicians -- details for table
```
insert value
The REFERENCES constraint is what makes a column a FK.
```sql
-- Let's try again, but first, let's verify the ids of the bands
SELECT * FROM bands;

-- Assuming 'Rush' has an id of 2
INSERT INTO musicians (name, band_id) VALUES ('Geddy Lee', 2);

SELECT * FROM musicians;  -- There's Geddy!

-- Now let's add Neil
-- Use two single quotes to embed an apostrophe
INSERT INTO musicians (name, quote, band_id)
VALUES (
'Neil Peart',
'If you''ve got a problem, take it out on a drum',
2);
```
### **Querying Data using a `JOIN` Clause**

The `JOIN` clause is used with a `SELECT` to query for data from more than one table.
Let’s say we have the following data relationship: `Band ---< Musician`
*A Band has many Musicians*, and *a Musician belongs to a Band*

![](https://github.com/miya-w/2024-SQL-note/blob/main/images/sql-datebase-pk-fk.png)

```sql
-- table right of JOIN has the FKs
SELECT*FROM bands JOIN musicians ON bands.id= musicians.band_id;

 id | name |   genre   | id |    name    |                     quote                      | band_id 
----+------+-----------+----+------------+------------------------------------------------+---------
  2 | Rush | prog rock |  3 | Neil Peart | If you've got a problem, take it out on a drum |       2
  2 | Rush | prog rock |  2 | Geddy Lee  | I love to write, it's my first love.           |       2
(2 rows)
```
If we want to return all bands, regardless of whether or not there’s any matches for musicians, we use whats called a **LEFT JOIN**:
```sql
SELECT * FROM bands b LEFT JOIN musicians m ON b.id = m.band_id
WHERE b.name = 'Rush' AND m.name LIKE 'G%';


 id | name |   genre   | id |   name    |                quote                 | band_id 
----+------+-----------+----+-----------+--------------------------------------+---------
  2 | Rush | prog rock |  2 | Geddy Lee | I love to write, it's my first love. |       2
(1 row)
```
## SQl Syntax

```sql
SELECT * ,column1, column2, ... ---example
FROM table_name
WHERE condition;
```
### Some of The Most Important SQL Commands
- SELECT - extracts data from a database
- UPDATE - updates data in a database
- DELETE - deletes data from a database
- INSERT INTO - inserts new data into a database
CREATE DATABASE - creates a new database
ALTER DATABASE - modifies a database
CREATE TABLE - creates a new table
ALTER TABLE - modifies a table
DROP TABLE - deletes a table
CREATE INDEX - creates an index (search key)
DROP INDEX - deletes an index

```sql
-- create database ---
CREATE DATABASE databasename;
-- Drop database ---
DROP DATABASE databasename;
--- select all ---
SELECT * FROM tablename;

--- create table ---
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype,
   ....
);
CREATE TABLE Persons (
    PersonID int,
    LastName varchar(255),
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
);
---The PersonID column is of type int and will hold an integer.
--- The LastName, FirstName, Address, and City columns are of type varchar 
--- and will hold characters, and the maximum length for these fields is 255 characters.

```
### The SQL WHERE Clause

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;

SELECT * FROM Customers
WHERE Country='Mexico';
```

### Create

```sql
--- The INSERT INTO statement is used to insert new records in a table. 
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
-- ex. --
INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
VALUES ('Cardinal', 'Tom B. Erichsen', 'Skagen 21', 'Stavanger', '4006', 'Norway');
-- ex. --
INSERT INTO musicians (name, band_id) VALUES ('Geddy Lee', 2);
```
### Update
```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;

UPDATE Customers
SET ContactName = 'Alfred Schmidt', City= 'Frankfurt'
WHERE CustomerID = 1;

```
### Delete
```sql
DELETE FROM table_name WHERE condition;

DELETE FROM Customers WHERE CustomerName='Alfreds Futterkiste';
```
### JOIN
```
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
FROM Orders
INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID;
```


### Resources
---
- [W3C-SQL](https://www.w3schools.com/sql/)
- [SQL Basics Cheat Sheet](https://www.datacamp.com/cheat-sheet/sql-basics-cheat-sheet)
- [The 80 Top SQL Interview Questions and Answers for Beginners & Intermediate Practitioners](https://www.datacamp.com/blog/top-sql-interview-questions-and-answers-for-beginners-and-intermediate-practitioners)
- [40+ scenario-based SQL interview questions to ask programmers](https://www.testgorilla.com/blog/scenario-based-sql-interview-questions/)