---
layout: post
title: "SQL"
date: 2025-05-07 14:08:00 -0700
categories: [SQL]
tags: [SQL]
description: Learning SQL Basics
---
## SQL Intro 

**SQL: Structured Query Language**
Lets you access and maniputalte databases

To build a web site that shows data from a database, you will need:

An RDBMS database program (i.e. MS Access, SQL Server, MySQL)
Use a server-side scripting language, like PHP or ASP
Use SQL to get the data you want
Use HTML / CSS to style the page

RDBMS: Relational Database Management System
RDBMS is the basis for SQL, and for all modern database systems such as MS SQL Server, IBM DB2, Oracle, MySQL, and Microsoft Access.

Example:
Select all records from the Customers Tables:
SELECT * FROM Customers;

SQL keywords are NOT case sensitive: select is the same as SELECT

Some database systems require a semicolon at the end of each SQL statement.

Semicolon is the standard way to separate each SQL statement in database systems that allow more than one SQL statement to be executed in the same call to the server.

Some of The Most Important SQL Commands

- **SELECT** - extracts data from a database

- **UPDATE** - updates data in a database

- **DELETE** - deletes data from a database

- **INSERT INTO** - inserts new data into a database

- **CREATE DATABASE** - creates a new database

- **ALTER DATABASE** - modifies a database

- **CREATE TABLE** - creates a new table

- **ALTER TABLE** - modifies a table

- **DROP TABLE** - deletes a table

- **CREATE INDEX** - creates an index (search key)

- **DROP INDEX** - deletes an index

### SQL Exercise 1 Select statement

![Desktop View](/assets/img/SQL/SQL-1.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-2.png){: width="700" height="400" }

### SQL Exercise 2 Queries with Constraints 

```text
Select query with constraints
SELECT column, another_column, …
FROM mytable
WHERE condition
    AND/OR another_condition
    AND/OR …;
```

![Desktop View](/assets/img/SQL/SQL-3.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-4.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-5.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-6.png){: width="700" height="400" }

### SQL Exercise 4 Filtering and sorting Query results

SQL offers a straightforward method to eliminate rows with duplicate values in a specific column by using the DISTINCT keyword.

SQL allows you to organize query results by a specified column in either ascending or descending order using the ORDER BY clause.

Another commonly used set of clauses with ORDER BY are LIMIT and OFFSET, which help optimize queries by telling the database exactly which portion of the results you're interested in.
The LIMIT clause restricts the number of rows returned, while the optional OFFSET clause defines the starting point from which rows are counted.

![Desktop View](/assets/img/SQL/SQL-7.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-8.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-9.png){: width="700" height="400" }

**More Practice**

![Desktop View](/assets/img/SQL/SQL-10.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-11.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-12.png){: width="700" height="400" }

### SQL Exercise 5 Multi-table queries with JOINs

In real-world scenarios, information about an entity is typically divided and stored across several related tables—a method referred to as normalization.

**Understanding Database Normalization**

Normalization is beneficial because it reduces redundancy within any given table and enables various parts of the database to expand independently (for example, new engine types can be added without altering the car models table). However, this separation of data means that querying becomes more complex, and performance can be impacted when dealing with numerous large tables.

To retrieve complete information about an entity whose data is distributed across multiple normalized tables, it’s necessary to construct queries that can merge this data effectively.

**Working with JOINs in Multi-Table Queries**

When different tables store data about the same entity, they must include a primary key that uniquely identifies that entity across the database. This key is often an auto-incrementing number for efficiency, but it could also be a unique string or hash.

To combine data from multiple tables, SQL provides the JOIN clause. One common type is the INNER JOIN, which merges rows from two tables based on a shared key, allowing for the retrieval of related data from both tables.

```text
SELECT column, another_table_column, …
FROM mytable
INNER JOIN another_table 
    ON mytable.id = another_table.id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

**Understanding INNER JOIN**

An INNER JOIN combines rows from two tables based on a matching value in a specified column (as indicated by the ON condition). When a match is found, a new row is created in the result set that includes columns from both tables. Once the join is completed, any additional clauses—such as WHERE, ORDER BY, or GROUP BY—are applied to filter or sort the results.

In many SQL queries, you’ll notice that INNER JOIN is often shortened to just JOIN—they function the same way.

![Desktop View](/assets/img/SQL/SQL-13.png){: width="700" height="400" }
**This will be the 2 tables I will be working with based off of Pixar Movies and their Box Ofiice**

![Desktop View](/assets/img/SQL/SQL-14.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-15.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-16.png){: width="700" height="400" }

### SQL Exercise 6 OUTER JOINs

The INNER JOIN we used previously may not always be the best choice, especially if your analysis requires data that isn't shared between both tables. This is because an INNER JOIN only includes records that exist in both tables.

In cases where the data is unbalanced—for instance, when entries are added at different times or in different stages—you may need to use a LEFT JOIN, RIGHT JOIN, or FULL JOIN instead. These types of joins help ensure that important information isn't excluded from your query results.

```text
SELECT column, another_column, …
FROM mytable
INNER/LEFT/RIGHT/FULL JOIN another_table 
    ON mytable.id = another_table.matching_id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

Just like with an INNER JOIN, these three additional types of joins also require you to define the column used to connect the tables.

When performing a LEFT JOIN from table A to table B, all rows from table A are retained—even if there's no corresponding match in table B. A RIGHT JOIN works in the opposite direction, keeping all rows from table B regardless of matches in table A. A FULL JOIN includes all rows from both tables, whether or not a match exists in the other table.

Because these joins can produce unmatched rows, you’ll often need to handle NULL values in your query results and account for any related constraints or logic.