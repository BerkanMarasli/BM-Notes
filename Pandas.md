# Pandas

Import pandas: `import pandas` or `import pandas as pd`



## Read File

Read **csv** file to data frame: `dataframe = pandas.read_csv('some_filename.csv')` or `df = pandas.read_csv('some_filename.csv')`

Read **xlsx** file to data frame: `dataframe = pandas.read_excel('some_filename.xlsx')` or `df = pandas.read_excel('some_filename.xlsx')`

Read **txt** file to data frame: `dataframe = pandas.read_csv('some_filename.txt', delimiter='\t')` or `df = pandas.read_csv('some_filename.txt', delimiter='\t')` where delimiter is the column seperator



## Reading Data

### Reading set number of rows

Access first 5 rows in data frame: `dataframe.head()` or `df.head()`

Access last 5 rows in data frame: `dataframe.tail()` or `df.tail()`

Access first/last X rows in data frame: `dataframe.head(X)` or `dataframe.tail(X)` where X is an integer

### Reading column headers

Read all column headers: `dataframe.columns` or  `df.columns`

### Read each column

Access specific column: `dataframe['column_name']` or  `df['column_name']`

Access multiple columns: `dataframe[['column_name_1', 'column_name_2']]` or  `df[['column_name_1', 'column_name_2']]`

Access set rows in specific column: `dataframe['column_name'][X:Y]` or  `df['column_name'][X:Y]` where X is the starting row number (inclusive) and Y is the ending row number (not inclusive). X and Y are integers. For example, to access first 6 rows of column 'Name': `dataframe['Name'][0:6]` or `df['Name'][0:6]`

### Read each row

Access specific row: `dataframe.iloc[X]` or  `df.iloc[X]` where X is the row integer location (iloc) as an integer

Access multiple rows: `dataframe.iloc[X:Y]` or  `df.iloc[X:Y]` where X is the starting row integer location (iloc) and Y is the ending row integer location (iloc). X and Y are integers

### Read specific (row, column)

Access specifc column value of specific row: `dataframe.iloc[X,Y]` or  `df.iloc[X,Y]` where X is the row integer location (iloc) as an integer and Y is the column integer location (iloc). X and Y are integers. For example, to access value of column 1 in row 2: `dataframe.iloc[2,1]` or `df.iloc[2,1]`

### Iterate through each row

Access each row:

```python
for index, row in dataframe.iterrows():
    print(index, row)
```

Access specific column in each row:

```python
for index, row in dataframe.iterrows():
    print(index, row['column_name'])
```

### Read rows with specific column value

Access all rows with specific column value: `dataframe.loc[dataframe['column_name'] == 'value']` or  `df.loc[df['column_name'] == 'value']`. For example, to access all rows within column 'Name' equal to 'John': `dataframe.loc[dataframe['Name'] == 'John']` or  `df.loc[df['Name'] == 'John']`































