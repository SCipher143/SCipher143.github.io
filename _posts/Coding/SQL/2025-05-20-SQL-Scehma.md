---
layout: post
title: "SQL Data (Rows and Tables)"
date: 2025-05-07 14:08:00 -0700
categories: [SQL]
tags: [SQL]
description: Learning about Updating Rows and Tables
---
## SQL Exercise 1: Inserting Rows

Before we begin let's talk about what a Schema is: 

A table in a database is like a grid with rows and columns. Each column holds a specific type of information (like a property), and each row is an entry or example of the item the table represents. In SQL, the structure of a table—what columns it has and what kind of data each column can store—is defined by something called the database schema.

**Inserting New Data** 

To add new data to a database, we use an `INSERT` statement. It tells the database which table to add the data to, which columns we’re filling, and the values we want to insert. Usually, you need to provide a value for each column in the table. You can also insert several rows at once by listing them one after another.

```text
INSERT INTO mytable
VALUES (value_or_expr, another_value_or_expr, …),
       (value_or_expr_2, another_value_or_expr_2, …),
       …;
```

In some cases, if you have incomplete data and the table contains columns that support default values, you can insert rows with only the columns of data you have by specifying them explicitly.

```text
INSERT INTO mytable
(column, another_column, …)
VALUES (value_or_expr, another_value_or_expr, …),
      (value_or_expr_2, another_value_or_expr_2, …),
      …;
```

When using an `INSERT` statement, the number of values must match the number of columns you list. Even though this can make the statement longer, it's more flexible in the long run. For example, if you add a new column with a default value later, you won’t need to update existing `INSERT` statements.

You can also use math or string expressions when inserting values. This helps make sure the data is formatted the way you want when it goes into the table.

**Example Insert statement with expressions**

```text
INSERT INTO boxoffice
(movie_id, rating, sales_in_millions)
VALUES (1, 9.9, 283742034 / 1000000);
```

![Desktop View](/assets/img/SQL/SQL-29.png){: width="700" height="400" }

![Desktop View](/assets/img/SQL/SQL-30.png){: width="700" height="400" }

### Exercise 2 Updating Rows

Besides adding new data, you’ll often need to change existing data. To do this, you use an UPDATE statement. Like INSERT, you need to say which table you're updating, which columns you're changing, and which rows should be updated. Also, make sure the new values match the correct data types for each column.

```text
UPDATE mytable
SET column = value_or_expr, 
    other_column = another_value_or_expr, 
    …
WHERE condition;
```

**Example**

![Desktop View](/assets/img/SQL/SQL-31.png){: width="700" height="400" }
**Before**

![Desktop View](/assets/img/SQL/SQL-32.png){: width="700" height="400" }
**After**

### Exercise 2 Deleting Rows

When you need to delete data from a table in the database, you can use a `DELETE` statement, which describes the table to act on, and the rows of the table to delete through the `WHERE` clause.

If you decide to leave out the `WHERE` constraint, then all rows are removed, which is a quick and easy way to clear out a table completely (if intentional).
{: .prompt-warning }

```text
DELETE FROM mytable
WHERE condition;
```

![Desktop View](/assets/img/SQL/SQL-32.png){: width="700" height="400" }

### Exercise 3 Creating Tables

When you have new entities and relationships to store in your database, you can create a new database table using the `CREATE TABLE` statement.

```text
CREATE TABLE IF NOT EXISTS mytable (
    column DataType TableConstraint DEFAULT default_value,
    another_column DataType TableConstraint DEFAULT default_value,
    …
);
```

A table’s structure is defined by its schema, which lists the columns in the table. Each column has:

- A name

- A data type (like number or text)

- Optional rules (like limits on what values are allowed)

- An optional default value if none is provided

If you try to create a table that already exists, most databases will show an error. To avoid this, you can use `IF NOT EXISTS` in your CREATE TABLE statement to skip creating the table if it's already there.

Common Data Types
Most databases support common data types like:

- Numbers (e.g., INTEGER, FLOAT)

- Text (e.g., VARCHAR, TEXT)

- Dates and times

- Booleans (TRUE or FALSE)

- Binary data (e.g., images or files)

These are useful when deciding how each column should store information.

| Data Type                            | Description                                                                                                                                           |
| :---------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------- |
| INTEGER, BOOLEAN                    | Stores whole numbers like counts or ages. In some systems, BOOLEAN is represented as 0 (false) or 1 (true).                                           |
| FLOAT, DOUBLE, REAL                 | Stores decimal numbers for more precise values like measurements. Use different types depending on the level of precision needed.                     |
| CHARACTER(n), VARCHAR(n), TEXT      | Stores text. CHARACTER and VARCHAR have a max length (longer values may be cut), which helps with performance. TEXT is used for longer strings.      |
| DATE, DATETIME                      | Stores date and time values. Useful for tracking events or timelines, but can be tricky when dealing with time zones.                                 |
| BLOB                                | Stores binary data like images or files. These are not human-readable and often require metadata to interpret them correctly later.                   |

Each column can have rules, called constraints, that control what kind of values are allowed. This isn’t a full list, but here are some common ones you might find helpful.

| Constraint         | Description                                                                                                                                           |
| :----------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------- |
| PRIMARY KEY        | Makes sure each value in this column is unique and can be used to identify each row in the table.                                                    |
| AUTOINCREMENT      | Automatically adds and increases a number in this column for each new row. Usually used with `INTEGER` types. Not supported in all databases.         |
| UNIQUE             | Makes sure all values in this column are different from one another. Similar to `PRIMARY KEY`, but doesn’t have to identify each row.                |
| NOT NULL           | Makes sure the column can’t be left empty (no `NULL` values allowed).                                                                                 |
| CHECK (expression) | Adds a rule to the column using a condition. For example, you can require that a number is positive or that text starts with a certain letter.        |
| FOREIGN KEY        | Links a column to a column in another table to make sure the data matches. This helps keep related data consistent between tables.                    |

**Example Create Table**

```text
CREATE TABLE movies (
    id INTEGER PRIMARY KEY,
    title TEXT,
    director TEXT,
    year INTEGER, 
    length_minutes INTEGER
);
```


