# Pandas Cheat Sheet

## Importing Pandas
```python
import pandas as pd
```

## Creating DataFrames
### From a dictionary
```python
data = {'column1': [1, 2], 'column2': [3, 4]}
df = pd.DataFrame(data)
```

### From a CSV file
```python
df = pd.read_csv('file.csv')
```

### From an Excel file
```python
df = pd.read_excel('file.xlsx', sheet_name='Sheet1')
```

### From a list of lists
```python
data = [[1, 2, 3], [4, 5, 6]]
df = pd.DataFrame(data, columns=['A', 'B', 'C'])
```

## Basic Operations
### Viewing Data
```python
df.head()       # First 5 rows
df.tail()       # Last 5 rows
df.sample(n=5)  # Random 5 rows
df.info()       # Summary of the DataFrame
df.describe()   # Statistical summary
```

### Selecting Data
```python
df['column']               # Select a single column
df[['column1', 'column2']] # Select multiple columns
df.iloc[0]                 # Select the first row
df.loc[0, 'column']        # Select the first row and a specific column
df[df['column'] > 0]       # Conditional selection
```

## Data Cleaning
### Handling Missing Values
```python
df.dropna()                         # Drop rows with any missing values
df.dropna(axis=1)                   # Drop columns with any missing values
df.fillna(0)                        # Fill missing values with 0
df.fillna(method='ffill')           # Forward fill missing values
df.fillna(method='bfill')           # Backward fill missing values
df['column'].fillna(df['column'].mean(), inplace=True)    # Fill with mean
df['column'].fillna(df['column'].median(), inplace=True)  # Fill with median
df['column'].fillna(df['column'].mode()[0], inplace=True) # Fill with mode
```

### Renaming Columns
```python
df.rename(columns={'old_name': 'new_name'}, inplace=True)
```

### Dropping Columns
```python
df.drop(columns=['column1', 'column2'], inplace=True)
```

## Data Transformation
### Adding New Columns
```python
df['new_column'] = df['column1'] + df['column2']
```

### Applying Functions
```python
df['column'] = df['column'].apply(lambda x: x + 1)
```

### Grouping Data
```python
df_grouped = df.groupby('column').mean()
```

### Merging DataFrames
```python
df_merged = pd.merge(df1, df2, on='key')
df_concat = pd.concat([df1, df2])
```

## Data Aggregation
### GroupBy
```python
df.groupby('column').sum()
df.groupby('column').agg({'column1': 'sum', 'column2': 'mean'})
```

## Date and Time
### Converting to datetime
```python
df['date_column'] = pd.to_datetime(df['date_column'])
```

### Extracting Date Components
```python
df['year'] = df['date_column'].dt.year
df['month'] = df['date_column'].dt.month
df['day'] = df['date_column'].dt.day
```

## Input/Output
### CSV
```python
df.to_csv('file.csv', index=False)
df = pd.read_csv('file.csv')
```

### Excel
```python
df.to_excel('file.xlsx', sheet_name='Sheet1', index=False)
df = pd.read_excel('file.xlsx', sheet_name='Sheet1')
```

### JSON
```python
df.to_json('file.json')
df = pd.read_json('file.json')
```

## Plotting
### Using matplotlib
```python
import matplotlib.pyplot as plt

df['column'].plot(kind='hist')
plt.show()
```

### Using pandas plotting
```python
df.plot(x='column1', y='column2', kind='scatter')
plt.show()
```

## Advanced Operations
### Pivot Tables
```python
pivot = df.pivot_table(index='column1', columns='column2', values='column3', aggfunc='mean')
```

### Handling Duplicates
```python
df.drop_duplicates(subset='column', keep='first', inplace=True)
```


| **EDA Operation**            | **Pandas Command**                                                     | **Description**                                                                                     |
|------------------------------|------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Loading Data**             | `pd.read_csv('file.csv')`                                              | Load data from a CSV file.                                                                          |
|                              | `pd.read_excel('file.xlsx')`                                           | Load data from an Excel file.                                                                       |
|                              | `pd.read_sql('SELECT * FROM table', connection)`                      | Load data from a SQL database.                                                                      |
|                              | `pd.read_json('file.json')`                                            | Load data from a JSON file.                                                                         |
|                              | `pd.read_html('file.html')`                                            | Parse tables from an HTML document.                                                                 |
|                              | `pd.read_pickle('file.pkl')`                                           | Load data from a pickled file.                                                                      |
|                              | `pd.read_parquet('file.parquet')`                                      | Load data from a Parquet file.                                                                      |
|                              | `pd.read_hdf('file.h5', 'key')`                                        | Load data from an HDF5 file.                                                                        |
| **Viewing Data**             | `df.head()`                                                           | Display the first few rows of the DataFrame.                                                        |
|                              | `df.tail()`                                                           | Display the last few rows of the DataFrame.                                                         |
|                              | `df.sample(n)`                                                        | Display a random sample of n rows from the DataFrame.                                               |
| **Summarizing Data**         | `df.info()`                                                           | Get a concise summary of the DataFrame, including data types and non-null counts.                   |
|                              | `df.describe()`                                                       | Generate descriptive statistics of the DataFrame.                                                   |
|                              | `df.columns`                                                          | Get the column names of the DataFrame.                                                              |
|                              | `df.index`                                                            | Get the row index (labels) of the DataFrame.                                                        |
|                              | `df.dtypes`                                                           | Get the data types of each column.                                                                  |
|                              | `df.memory_usage()`                                                   | Memory usage of the DataFrame.                                                                      |
| **Data Cleaning**            | `df.isnull().sum()`                                                   | Count the number of missing values in each column.                                                  |
|                              | `df.isna().sum()`                                                     | Another method to count missing values (synonym for `isnull()`).                                    |
|                              | `df.dropna()`                                                         | Remove rows with missing values.                                                                    |
|                              | `df.fillna(value)`                                                    | Fill missing values with a specified value.                                                         |
|                              | `df.fillna(df.mean())`                                                | Fill missing numeric values with the mean of each column.                                           |
|                              | `df['column'].fillna(method='ffill')`                                 | Forward fill missing values in a column.                                                            |
|                              | `df['column'].fillna(method='bfill')`                                 | Backward fill missing values in a column.                                                           |
|                              | `df.drop_duplicates()`                                                | Remove duplicate rows.                                                                              |
|                              | `df.drop_duplicates(subset=['column'], keep='first', inplace=True)`   | Remove duplicates based on a subset of columns, keeping the first occurrence, and modify in place.  |
|                              | `df.duplicated().sum()`                                               | Count the number of duplicated rows.                                                                |
|                              | `df.rename(columns={'old': 'new'})`                                   | Rename columns.                                                                                     |
|                              | `df.columns.str.strip()`                                              | Strip leading and trailing spaces from column names.                                                |
|                              | `df.astype({'column': 'dtype'})`                                      | Change the data type of a column.                                                                   |
|                              | `pd.to_datetime(df['column'])`                                        | Convert a column to datetime.                                                                       |
|                              | `pd.api.types.infer_dtype(df['column'])`                              | Infer the data type of a column.                                                                    |
|                              | `pd.to_numeric(df['column'], errors='ignore', downcast='integer')`    | Convert a column to a numeric type, downcasting to integer if possible.                             |
|                              | `df['column'].value_counts()`                                         | Count the unique values in a column.                                                                |
|                              | `df['column'].nunique()`                                              | Count the number of unique values in a column.                                                      |
|                              | `df['column'].unique()`                                               | Get the unique values of a column.                                                                  |
|                              | `pd.qcut(df['column'], q, labels=False)`                              | Discretize a column into q equal-sized bins.                                                        |
|                              | `pd.cut(df['column'], bins, labels=False)`                            | Discretize a column into specified bins.                                                            |
|                              | `df.select_dtypes(exclude=[np.number])`                               | Select columns of a DataFrame that are not numeric.                                                  |
| **Data Selection**           | `df['column']`                                                        | Select a single column.                                                                             |
|                              | `df[['col1', 'col2']]`                                                | Select multiple columns.                                                                            |
|                              | `df.loc[row_labels, column_labels]`                                   | Select rows and columns by labels.                                                                  |
|                              | `df.iloc[row_positions, column_positions]`                            | Select rows and columns by integer positions.                                                       |
|                              | `df.query('column > value')`                                          | Query the DataFrame using a string expression.                                                      |
|                              | `df.loc[df['column'].isin(['value1', 'value2'])]`                     | Select rows where column values are in a list of specified values.                                  |
| **Data Manipulation**        | `df.sort_values('column')`                                            | Sort the DataFrame by a specific column.                                                            |
|                              | `df.groupby('column').sum()`                                          | Group by a column and aggregate (e.g., sum).                                                        |
|                              | `df.pivot(index, columns, values)`                                    | Pivot the DataFrame to reshape data.                                                                |
|                              | `df.melt(id_vars, value_vars)`                                        | Unpivot the DataFrame from wide format to long format.                                              |
|                              | `df.apply(function)`                                                  | Apply a function along an axis of the DataFrame.                                                    |
|                              | `df.assign(new_column=df['column']*2)`                                | Add a new column or modify existing columns.                                                        |
|                              | `pd.concat([df1, df2], axis=0)`                                       | Concatenate DataFrames along rows or columns.                                                       |
|                              | `df.merge(other_df, on='key', how='inner')`                           | Merge two DataFrames on a key column.                                                               |
| **Visualization**            | `df.plot(kind='line')`                                                | Plot a line graph.                                                                                  |
|                              | `df['column'].hist()`                                                 | Plot a histogram.                                                                                   |
|                              | `df['column'].value_counts().plot(kind='bar')`                        | Plot a bar chart.                                                                                   |
|                              | `df.plot(kind='scatter', x='x_column', y='y_column')`                 | Plot a scatter plot.                                                                                |
| **Saving Data**              | `df.to_csv('file.csv')`                                               | Save the DataFrame to a CSV file.                                                                   |
|                              | `df.to_excel('file.xlsx')`                                            | Save the DataFrame to an Excel file.                                                                |
|                              | `df.to_json('file.json')`                                             | Save the DataFrame to a JSON file.                                                                  |
|                              | `df.to_sql('table', connection)`                                      | Save the DataFrame to a SQL database.                                                               |
|                              | `df.to_pickle('file.pkl')`                                            | Save the DataFrame as a pickled object.                                                             |
