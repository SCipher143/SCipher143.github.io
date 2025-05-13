---
layout: post
title: "SQL"
date: 2025-05-07 14:08:00 -0700
categories: [SQL]
tags: [SQL]
description: Learning SQL Basics
---
## SQL Intro 

SQL: Structured Query Language
Lets you access and maniputalte databases

To build a web site that shows data from a database, you will need:

An RDBMS database program (i.e. MS Access, SQL Server, MySQL)
To use a server-side scripting language, like PHP or ASP
To use SQL to get the data you want
To use HTML / CSS to style the page

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

#### SQL Exercise 1 Select statement

![Desktop View](/assets/img/SQL/SQL-1.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-2.png){: width="700" height="400" }

#### SQL Exercise 2 Queries with Constraints 

Select query with constraints
SELECT column, another_column, …
FROM mytable
WHERE condition
    AND/OR another_condition
    AND/OR …;

![Desktop View](/assets/img/SQL/SQL-3.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-4.png){: width="700" height="400" }