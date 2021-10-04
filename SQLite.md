# SQLite



brew install sqlite

```
brew install sqlite
```



## Query

### `SELECT`

To retrieve data from a SQL database

```sqlite
-- Select query for all columns with all rows
SELECT * FROM mytable;

-- Select query for specific columns with all rows
SELECT column, another_column, ... FROM mytable;
```



### `WHERE`

To filter rows

Applied to each row of data by checking specific column values to determine whether it should be included in the result or not

```sqlite
-- Select query with constraints
SELECT column, another_column, ... FROM mytable WHERE condition AND/OR another_condition AND/OR ...;
```

For numerical data:

| Operator                | Description                                          | Example                                  |
| ----------------------- | ---------------------------------------------------- | ---------------------------------------- |
| =, !=, <, <=, >, >=     | Standard numerical operators                         | col_name operation value                 |
| BETWEEN ... AND ...     | Number is within range of two values (inclusive)     | col_name BETWEEN value_1 AND value_2     |
| NOT BETWEEN ... AND ... | Number is not within range of two values (inclusive) | col_name NOT BETWEEN value_1 AND value_2 |
| IN (...)                | Number exists in a list                              | col_name IN (2, 4, 6)                    |
| NOT IN (...)            | Number does not exist in a list                      | col_name NOT IN (1, 3, 5)                |

For text data:

| Opertator    | Description                                                  | Example                                                      |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| =            | Case sensitive exact string comparison                       | col_name = "abc"                                             |
| != or <>     | Case sensitive exact string inequality comparison            | col_name != "abc"                                            |
| LIKE         | Case insensitive exact string comparison                     | col_name LIKE "ABC"                                          |
| NOT LIKE     | Case insensitive exact string inequality comparison          | col_name NOT LIKE "ABCD"                                     |
| %            | Used anywhere in a string to match a sequence of zero or more characters<br />(Only with LIKE or NOT LIKE) | col_name LIKE "%AT%"<br />(matches "<u>AT</u>", "<u>AT</u>TIC", "C<u>AT</u>") |
| _            | Used anywhere in a string to match a single character<br />(Only with LIKE or NOT LIKE) | col_name LIKE "AN_"<br />(matches "<u>AN</u>D", but not "<u>AN</u>") |
| IN (...)     | String exists in a list                                      | col_name IN ("A", "B", "C")                                  |
| NOT IN (...) | String does not exist in a list                              | col_name NOT IN ("D", "E", "F")                              |









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



















