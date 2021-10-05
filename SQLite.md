# SQLite



relational databases

MySQL, Postgres, SQLite, Microsoft SQL server



brew install sqlite

```
brew install sqlite
```



## Simple Queries

### `SELECT`

Retrieve data from a SQL database

```sqlite
-- Select query for all columns with all rows
SELECT * FROM mytable;

-- Select query for specific columns with all rows
SELECT column, another_column, ... FROM mytable;
```

### `WHERE`

Filter rows of a SQL database. Condition applied to each row of data to determine whether it should be included or not in result

```sqlite
-- Select query with constraints
SELECT column, another_column, ... FROM mytable WHERE condition AND/OR another_condition AND/OR ...;
```

For numerical data:

| Operator                | Description                                          | Condition Example                        |
| ----------------------- | ---------------------------------------------------- | ---------------------------------------- |
| =, !=, <, <=, >, >=     | Standard numerical operators                         | col_name operation value                 |
| BETWEEN ... AND ...     | Number is within range of two values (inclusive)     | col_name BETWEEN value_1 AND value_2     |
| NOT BETWEEN ... AND ... | Number is not within range of two values (inclusive) | col_name NOT BETWEEN value_1 AND value_2 |
| IN (...)                | Number exists in a list                              | col_name IN (2, 4, 6)                    |
| NOT IN (...)            | Number does not exist in a list                      | col_name NOT IN (1, 3, 5)                |

For text data:

| Opertator    | Description                                                  | Condition Example                                            |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| =            | Case sensitive exact string comparison                       | col_name = "abc"                                             |
| != or <>     | Case sensitive exact string inequality comparison            | col_name != "abc"                                            |
| LIKE         | Case insensitive exact string comparison                     | col_name LIKE "ABC"                                          |
| NOT LIKE     | Case insensitive exact string inequality comparison          | col_name NOT LIKE "ABCD"                                     |
| %            | Used anywhere in a string to match a sequence of zero or more characters<br />(Only with LIKE or NOT LIKE) | col_name LIKE "%AT%"<br />(matches "<u>AT</u>", "<u>AT</u>TIC", "C<u>AT</u>") |
| _            | Used anywhere in a string to match a single character<br />(Only with LIKE or NOT LIKE) | col_name LIKE "AN_"<br />(matches "<u>AN</u>D", but not "<u>AN</u>") |
| IN (...)     | String exists in a list                                      | col_name IN ("A", "B", "C")                                  |
| NOT IN (...) | String does not exist in a list                              | col_name NOT IN ("D", "E", "F")                              |

Filter and sort query result of a SQL database:

### `DISTINCT`

Discard rows that have a duplicate column value

```sqlite
-- Select query with unique results
SELECT DISTINCT column, another_column, ... FROM mytable WHERE condition(s);
```

Since the DISTINCT keyword blindly removes duplicate rows; to discard duplicates based on specific columns, the GROUP BY clause should be used

### `ORDER BY`

Sort result by a given column in ascending or descending order

```sqlite
-- Select query with ordered results
SELECT column, another_column, ... FROM mytable WHERE condition(s) ORDER BY column ASC/DESC;
```

### `LIMIT`and `OFFSET`

LIMIT will reduce the number of rows to return and the optional OFFSET will specify where to begin counting the number of rows from

```sqlite
-- Select query with limited rows
SELECT column, another_column, ... FROM mytable WHERE condition(s) ORDER BY column ASC/DESC LIMIT num_limit OFFSET num_offset;
```



## Multi-table Queries

Tables that share information about a single entity need to have a primary key that identifies that entity uniquely across the database

### Database normalisation

Database normalisation is useful because it minimises duplicate data in any single table, and allows for data in the database to grow independently of each other. As a trade-off, queries get slightly more complex since they have to be able to find data from different parts of the database, and performance issues can arise when working with many large tables. This information needs combining so that the relevant information can be extracted

### `JOIN`

INNER JOIN: matches rows from the first table and the second table which have the same key (defined by the ON constraint) to create a result row with the combined columns from both tables.

```sqlite
-- Select query with INNER JOIN on mulitple tables
SELECT column, another_column, ... FROM mytable INNER JOIN another_table ON mytable.id = another_table.id WHERE condition(s) ORDER BY column ASC/DESC LIMIT num_limit OFFSET num_offset;
```

### `Outer JOINs`

Like the INNER JOIN these three joins have to specify which column to join the data on. When joining table A to table B, a LEFT JOIN simply includes rows from A regardless of wehther a matching row is found in B. The RIGHT JOIN is the same, but reversed, keeping rows in B regardless of whether a match is found in A. Finally, a FULL JOIN simply means that rows from both tables are kept, regardless of whether a matching row exists in the other table

Additional logic may be needed to deal with NULLs in the result

```sqlite
-- Select query with LEFT/RIGHT/FULL JOINs on multiple tables
SELECT column, another_column, ... FROM mytable INNER/LEFT/RIGHT/FULL JOIN another_table ON mytable.id = another_table.id WHERE condition(s) ORDER BY column ASC/DESC LIMIT num_limit OFFSET num_offset;
```



## NULLs

An alternative to NULL values in your database is to have data-type appropiate default values, like 0 for numerical data. But if your database needs to store incomplete data, then NULL values can be approbate if the default values will skew analysis later

A column can be tested for NULL in a WHERE clause by using either the IS NULL or IS NOT NULL constraint

```sqlite
-- Select query with constraints on NULL values
SELECT column, another_column, ...
FROM mytable
WHERE column IS/IS NOT NULL
AND/OR another_condition;
```



## Queries With Expressions

### `AS`

Each database has its own supported set of mathematical, string, and date functions that can be used in a query, which you can find in their own respective docs. These should be given a descriptive alias using the AS keyword

```sqlite
-- Select query with expression aliases
SELECT col_expression AS expr_description, ...
FROM mytable;

-- Example query with expressions
SELECT particle_speed / 2.0 AS half_particle_speed
FROM physics_data
WHERE ABS(particle_position) * 10.0 > 500;

-- Another example query with expressions
SELECT column AS better_column_name, ...
FROM a_long_widgets_table_name AS mywidgets
INNER JOIN widget_sales ON mywidgets.id = widget_sales.widget_id;
```



## Queries With Aggregates

Without a specified grouping, each aggregrate function is going to run on the whole set of result rows and return a single value

```sqlite
-- Select query with aggregate functions over all rows
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, ...
FROM mytable
WHERE constraint_expression;
```

Common aggregate functions:

| Function                    | Description                                                  |
| --------------------------- | ------------------------------------------------------------ |
| COUNT(*)<br />COUNT(column) | A common function used to count the number of rows in the group if no column name is specifed.<br />Otherwise, count the number of rows in the group with non-NULL values in the specific column. |
| MIN(column)                 | Finds the smallest numerical value in the specified column for all rows in the group. |
| MAX(column)                 | Finds the largest numerical value in the specified column for all rows in the group. |
| AVG(column)                 | Finds the average numerical value in the specified column for all rows in the group. |
| SUM(column)                 | Finds the sum of all numerical values in the specified column for the rows in the group. |

### `GROUP BY`

In addition to aggregating across all the rows, you can instead apply the aggregate functions to individual groups of data within that group. This would create as many result as there are unique groups defined as by the GROUP BY clause. The GROUP BY clause works by grouping rows that have the same value in the column specified

```sqlite
-- Select query with aggregate functions over groups
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, ...
FROM mytable
WHERE constraint_expression
GROUP BY column;
```

### `HAVING`

Used specifically with GROUP BY to allow for grouped rows from the result set to be filtered

```sqlite
-- Select query with aggregate functions over groups with HAVING constraint
SELECT group_by_column, AGG_FUNC(column_or_expression) AS aggregate_description, ...
FROM mytable
WHERE constraint_expression
GROUP BY column
HAVING group_condition;
```



## Order Of Execution - Querying

```sqlite
-- Complete SELECT query
SELECT DISTINCT column, AGG_FUNC(column_or_expression), ...
FROM mytable
	JOIN another_table
		ON mytable.column = another_table.column
	WHERE constraint_expression
	GROUP BY column
	HAVING constraint_expression
	ORDER BY column ASC/DESC
	LIMIT num_limit OFFSET num_offset;
```

| Section of query | Description                                                  |
| :--------------- | :----------------------------------------------------------- |
| FROM and JOINs   | The FORM clause, and subsequent JOINs are first executed to determine the total working set of data that is being queried. This includes subqueries in this clause, and can cause temporary tables to be created under the hood containing all the columns and rows of the tables being joined. |
| WHERE            | Once we have the total working set of data, the first-pass WHERE constraints are applied ot the individual rows, and rows that do not satisfy the constraints are discarded. Each of the constraints can only access columns directly from the tables requested in the FROM clause. Aliases in the SELECT part of the query are not accessible in most databases since tey may include expressions dependent on parts of the query that have not yet executed. |
| GROUP BY         | The remaining rows after the WHERE constraints are applied are then grouped based on common values in the column specified in the GROUP BY clause. As a result of the grouping, there will only be as many rows as there are unique values in that column. Implicity, this means that you should only need to use this when you have aggregate functions in your query. |
| HAVING           | If the query has a GROUP BY clause, then the constraints in the HAVING clause are then applied to the grouped rows, discard the grouped rows that don't satisfy the constraint. Like the WHERE clause, aliases are also not accessible from this step in most databases. |
| SELECT           | Any expressions in the SELECT part of the query are finally computed. |
| DISTINCT         | Of the remaining rows, rows with duplicate values in the column marked as DISTINCT will be discarded. |
| ORDER BY         | If any order is specified by the ORDER BY clause, the rows are then sorted by the specified data in either ascending or descending order. Since all expressions in the SELECT part of the query have been computed, you cna reference aliases in this clause. |
| LIMIT / OFFSET   | Finally, the rows that fall outside the range specified by the LIMIT and OFFSET are discarded, leaving the final set of rows to be returned from the query. |





## Schema

Database schema refers to the structure of each database table, and the datatypes that each column of the table can contain. The fixed structure is what allows a database to be efficient, and consistent despite storing millions of rows

### `INSERT`

The INSERT declares which table to write into, the columns of data that we are filling, and one or more rows of data to insert. In general, each row of data you insert should contain values for every corresponding column in the table. You can insert multiple rows at a time by just listing them sequentially

```sqlite
-- Insert statement with values for all columns
INSERT INTO mytable
VALUES (value_or_expr, another_value_or_expr, ...),
	   (value_or_expr, another_value_or_expr, ...),
	   ...;
```

In some cases, if you have incomplete data and the table contains columns that support default values, you can insert rows with only the columns of data you have by specifying them explicitly. The number of values should match the number of columns specified

```sqlite
-- Insert statement with specific columns
INSERT INTO mytable
(column, another_column, ...)
VALUES (value_or_expr, another_value_or_expr, ...),
	   (value_or_expr, another_value_or_expr, ...),
	   ...;
```

In addition, mathematical and string expressions can be usde with the values that are inserting

### `UPDATE`

Specify exactly which table, columns, and rows to update. In addition, the data you are updating has to match the data type of the columns in the table schema

The statement works by taking mulitple column/value pairs, and applying those changes to each and every row that satisfies the constraint in the WHERE clause. Omitting the WHERE clause will apply update to all rows

```sqlite
-- Update statement with values
UPDATE mytable
SET column = value_or_expr,
	other_column = another_value_or_expr
	...
WHERE condition;
```

Tip: always write the constraint first and test it in a `SELECT` query to make sure you are updating the right rows, and only then writing the column/value pairs to update



















creating a table

```sqlite

```

adding rows

```sqlite

```

selecting columns

```sqlite

```

selecting records based on qualities

```sqlite

```



















