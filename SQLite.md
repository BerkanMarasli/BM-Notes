# SQLite



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



Filter and sort query result of a SQL database

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



















