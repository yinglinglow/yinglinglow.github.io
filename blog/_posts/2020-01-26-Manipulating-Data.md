---
layout: post
title: "Manipulating Data"
date: 2020-01-26
---

__Table of Contents__
 * Data structure
    - [Changing data structure](#structure)
 * Data type
    - [Changing data type](#type)
 * Code snippets 
    - [Link commands together](#link)


<a id="structure"></a> 
## Changing data structure

Creating a dataframe from a dictionary

```python

df = pd.DataFrame(dictionary)

```

Joining dataframes/ adding rows/ adding columns
```python

import pandas as pd
pd.concat([df, df2]) #appends df2 at the bottom of df as additional rows. to have labels, include labels as a list to keys. to append as new columns instead of rows, include axis=1 like so: pd.concat([df,df2], axis=1, keys=[tablename1, tablename2])

pd.merge(df_left,df_right,on=['key1','key2'], how='inner') #for users familiar with sql method of merging
df_left.join(df_right) #an alternative to the above

df['new_col'] = 'ABC' #broadcasting: creates a new column, all values will be 'ABC'. 
df['new_col'] = list #creates new column with values corresponding to the list.

zipped = zip(df['col_a'], df['col_b']) #returns a list of tuples, with values from col_a and col_b. to display in a list, use list(zip(df['col_a'], df['col_b']))
zip(*zipped) #to unzip. to display it in a list, use list(zip(*zipped))

d = {'male': 1, 'female': 0}
df['Sex_numerical'] = df['Sex'].map(d) #creates a new column, where male is represented by 1, and female by 0.

```

Dropping rows/ columns
```python
df.drop('col', axis=1, inplace=True) #drops the specified column. inplace=True automatically saves it to df without an additional step, axis=1 sets it as a column instead of row.

df_nonull = df[pd.notnull(df['col'])]
#only takes rows that are not null in the column.

```

Sorting
```python
df.sort_values('col') #sorts values according to 'col'. for descending order, include ascending=False like so: df.sort_values('col', ascending=False) 

```

Pivot table
```python
df.pivot_table(values='D', index['A','B'], columns=['C']) #gives a multi-index by columns A and B; takes C as the list of columns

```

<a id="type"></a> 
## Changing data type

```python
float('123') #changes an integer or string to a float

import pandas as pd
pd.to_numeric(df['col']) #converts to numeric

df.values #makes it into a numpy array

```

<a id="link"></a> 
## Code snippets

Below are some useful snippets of code:

Renaming columns

```python
df.columns = ['col_1','col_2','col_3'] #rename cols according to names in new list
df.rename(columns={'col_1': 'col_1_new', 'col_2': 'col_2_new'}, inplace=True) #rename specific columns - specify old names and new names. inplace=True automatically saves changes to df

```

Breaking down datetime into individual columns

```python
import datetime
df['Hour'] = df['timeStamp'].apply(lambda time: time.hour)
df['Month'] = df['timeStamp'].apply(lambda time: time.month)
df['Date'] = df['timeStamp'].apply(lambda time: time.date())

df['Day of Week'] = df['timeStamp'].apply(lambda time: time.dayofweek) #returns integers
dmap = {0:'Mon',1:'Tue',2:'Wed',3:'Thu',4:'Fri',5:'Sat',6:'Sun'}
df['Day of Week'] = df['Day of Week'].map(dmap) #map integers to actual day using dictionary

```
Creating a new column for different units

```python
df['Income per Capita (,000)'] = df['Income per Capita'] // 1000 #creates a new column in thousands

```

Creating a new column for percentage change

```python
import pandas as pd
returns = pd.DataFrame() #create empty dataframe named return

for tick in tickers:
    returns[tick+' Return'] = bank_stocks[tick]['Close'].pct_change() #creates new column, with the percentage change in Close

```

If the value is a list, create a new column for each item in the list.<br>
Note: isinstance checks the type of an item, and is better than type() because it supports inheritance (read more [here](https://stackoverflow.com/questions/1549801/what-are-the-differences-between-type-and-isinstance))

```python

# for each activity in the activity_log list, create a new column
def expand(row, col):
    activities = row[col] if isinstance(row[col], list) else [row[col]]
    s = pd.Series(row['user_id'], index=list(set(activities)))
    return s

# melt the columns so that each activity forms one additional row for each user
df_melted = df.apply(expand, col='activity_log', axis=1).stack().reset_index()
```