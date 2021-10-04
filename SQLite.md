# SQLite



brew install sqlite

```
brew install sqlite
```



## Queries

SELECT

To retrieve data from a SQL database

```sqlite
-- Select query for all columns with all rows
SELECT * FROM mytable;

-- Select query for specific columns with all rows
SELECT column, another_column, ... FROM mytable;
```



WHERE

To filter rows

Applied to each row of data by checking specific column values to determine whether it should be included in the result or not

```sqlite
-- Select query with constraints
SELECT column, another_column, ... FROM mytable WHERE condition AND/OR another_condition AND/OR ...;
```

For numerical data:

=, !=, <, <=, >, >=			Standard numerical operators			col_name operation value

BETWEEN ... AND ...			Number is within range of two values (inclusive)			col_name BETWEEN value_1 AND value_2

NOT BETWEEN ... AND ...			Number is not within range of two values (inclusive)			col_name NOT BETWEEN value_1 AND value_2

IN (...)			Number exists in a list			col_name IN (2, 4, 6)

NOT IN (...)			Number does not exist in a list			col_name NOT IN (1, 3, 5)







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



















