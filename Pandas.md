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



## Describe Data

Calculate count, mean, standard deviation, minimum, 25%, 50%, 75% and maximum for each column: `dataframe.describe()` or `df.describe()`

Obtain number of rows and columns, datatypes of columns and how many nulls in each column: `dataframe.info()` or `df.info()`



## Sorting Data

Sort data frame based on specific column: `dataframe.sort_values(['column_name'], ascending=X)` or `df.sort_values(['column_name'], ascending=X)` where X is True (default) or False.

Sort data frame based on multiple columns: `dataframe.sort_values(['column_name_1', 'column_name_2'], ascending=[X,X])` or `df.sort_values(['column_name_1', 'column_name_2'], ascending=[X,X])` where X is True (default) or False and can be represented by 1 or 0 respectively.



## Manipulating Data

### Create new column

Create new column: `dataframe['new_column_name'] = ...` or `df['new_column_name'] = ...`

For example, to create a new column called 'Total' which is the sum of columns 4 to 9: `dataframe['Total'] = dataframe.iloc[:, 4:10].sum(axis=1)`. axis has a default value of 0 meaning sum vertically whereas a value of 1 indicates a sum of each column across each row (sum horizontally). Remember the end integer location (iloc) is not inclusive.

### Remove column

Remove a specific column: `dataframe.drop(columns=['column_name'])` or `df.drop(columns=['column_name'])`. This does not directly manipulate the data frame so, a re-assignment is neccessary. For example, `dataframe = dataframe.drop(columns=['column_name'])` or `df = df.drop(columns=['column_name'])`

### Rearranging columns

Method 1: `dataframe = dataframe[['column_name_3', 'column_name_2', 'column_name_1']]`

Method 2 involves extracting the column headers and indexing those to re-order data frame:

```python
column_headers = list(dataframe.columns)
dataframe = dataframe[column_headers[0:4] + [column_headers[-1]] + column_headers[4:12]]
```

Note: using integers to index columns could lead to errors in future data sets. The specific column header's index should be found within column_headers and used to prevent errors in the future



## Writing Data

Write data frame to **csv** file: `dataframe.to_csv('some_filename.csv')` or  `df.to_csv('some_filename.csv')`. To avoid including the indexes, add an attribute `index=False`. For example, `dataframe.to_csv('some_filename.csv', index=False)`. This will save the csv file to the current directory

Write data frame to **xlsx** file: `dataframe.to_xlsx('some_filename.xlsx')` or  `df.to_xlsx('some_filename.csv')`. To avoid including the indexes, add an attribute `index=False`. For example, `dataframe.to_excel('some_filename.xlsx', index=False)`. This will save the excel file to the current directory

Write data frame to **txt** file: `dataframe.to_csv('some_filename.txt')` or  `df.to_csv('some_filename.txt')`. To avoid including the indexes, add an attribute `index=False` and add an attribute `sep='X'` where X is the column seperator. For example, `dataframe.to_csv('some_filename.csv', index=False, sep='\t')`. This will save the txt file to the current directory



















