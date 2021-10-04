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



















