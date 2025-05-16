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

![Desktop View](/assets/img/SQL/SQL-17.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-18.png){: width="700" height="400" }

### SQL Exercise 7 NULLs

Sometimes, it's also not possible to avoid NULL values. In these cases, you can test a column for NULL values in a WHERE clause by using either the IS NULL or IS NOT NULL constraint.

```text
SELECT column, another_column, …
FROM mytable
WHERE column IS/IS NOT NULL
AND/OR another_condition
AND/OR …;
```

![Desktop View](/assets/img/SQL/SQL-19.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-20.png){: width="700" height="400" }

### SQL Exercise 8 Queries with expressions 

Besides selecting raw column data in SQL queries, you can also apply expressions to perform more advanced operations on that data. These expressions can include arithmetic calculations, string manipulation, and built-in functions, allowing you to modify or compute values dynamically as the query runs.

```text
SELECT particle_speed / 2.0 AS half_particle_speed
FROM physics_data
WHERE ABS(particle_position) * 10.0 > 500;
```

Using expressions in SQL can help reduce the need for additional processing after retrieving the data, making your workflow more efficient. However, complex expressions can impact readability, so it's a good practice to assign them clear and descriptive aliases using the AS keyword when writing them in the SELECT clause.

```text
SELECT col_expression AS expr_description, …
FROM mytable;
```

In addition to expressions, regular columns and even tables can also have aliases to make them easier to reference in the output and as a part of simplifying more complex queries.

```text
SELECT column AS better_column_name, …
FROM a_long_widgets_table_name AS mywidgets
INNER JOIN widget_sales
  ON mywidgets.id = widget_sales.widget_id;
```

![Desktop View](/assets/img/SQL/SQL-21.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-22.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-23.png){: width="700" height="400" }

### SQL Exercise 9 Queries with aggregates (Pt. 1)

In SQL, you can use special functions called aggregate functions to do things like add up numbers or count rows. These help you summarize data from many rows at once.

```text
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
FROM mytable
WHERE constraint_expression;
```

If you don’t group the data, an aggregate function will look at all the rows and give you just one result. Just like with other expressions, using an alias makes the result easier to understand.

| Function                      | Description                                                                                                      |
| :---------------------------- | :---------------------------------------------------------------------------------------------------------------- |
| COUNT(*), COUNT(column)       | Counts the number of rows in the group if no column is specified. If a column is given, it counts the rows with non-NULL values in that column. |
| MIN(column)                   | Finds the smallest numerical value in the specified column for all rows in the group.                            |
| MAX(column)                   | Finds the largest numerical value in the specified column for all rows in the group.                             |
| AVG(column)                   | Finds the average numerical value in the specified column for all rows in the group.                            |
| SUM(column)                   | Finds the sum of all numerical values in the specified column for the rows in the group.                        |

Instead of applying aggregate functions to all rows at once, you can use them to calculate results for smaller groups within the data.

Example:
If you have a list of sales data for different stores, you can use an aggregate function like SUM() to calculate the total sales for each store individually, rather than for all stores combined.

This would then create as many results as there are unique groups defined as by the GROUP BY clause.

```text
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
FROM mytable
WHERE constraint_expression
GROUP BY column;
```

The GROUP BY clause works by grouping rows that have the same value in the column specified.

![Desktop View](/assets/img/SQL/SQL-24.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-25.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-26.png){: width="700" height="400" }

### SQL Exercise 9 Queries with aggregates (Pt. 2)

Our SQL queries are starting to get more advanced, and we’ve covered most of the key parts of a SELECT statement. You might have noticed that the WHERE clause filters rows before they’re grouped with GROUP BY. So how can we filter the results after the grouping happens?

That’s where the HAVING clause comes in. It works alongside GROUP BY and lets you filter the grouped data in your final results.

```text
SELECT group_by_column, AGG_FUNC(column_expression) AS aggregate_result_alias, …
FROM mytable
WHERE condition
GROUP BY column
HAVING group_condition;
```

![Desktop View](/assets/img/SQL/SQL-27.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-28.png){: width="700" height="400" }

## Order of execution of a Query

**SELECT DISTINCT** column, **AGG_FUNC**(column_or_expression), …

**FROM** mytable
    
**JOIN** another_table
      
**ON** mytable.column = another_table.column
    
**WHERE** constraint_expression
    
**GROUP** BY column

**HAVING** constraint_expression
    
**ORDER BY** column ASC/DESC
    
**LIMIT** count OFFSET COUNT;