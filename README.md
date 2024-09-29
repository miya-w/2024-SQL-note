## SQL 

### 1. Install SQL 
- [install - PostgreSQL](https://www.postgresql.org/)

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

Let’s say we have the following data relationship: `Band ---< Musician`
*A Band has many Musicians*, and *a Musician belongs to a Band*
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


### Resources
---
- [SQL Basics Cheat Sheet](https://www.datacamp.com/cheat-sheet/sql-basics-cheat-sheet)
- [The 80 Top SQL Interview Questions and Answers for Beginners & Intermediate Practitioners](https://www.datacamp.com/blog/top-sql-interview-questions-and-answers-for-beginners-and-intermediate-practitioners)
- [40+ scenario-based SQL interview questions to ask programmers](https://www.testgorilla.com/blog/scenario-based-sql-interview-questions/)