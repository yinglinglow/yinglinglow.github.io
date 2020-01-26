---
layout: post
title: "Cleaning Data"
date: 2020-01-26
---

__Table of Contents__
 * Data cleaning
    - [Handling null values](#null)
    - [Handling outliers](#outliers)
    - [Dealing with unwanted values](#illogical)

<a id="null"></a> 
## Handling null values

I usually drop columns with more than 40% null values.

```python

null_col = list(df.columns[df.isnull().sum() > len(df)*0.4])
df.drop(null_col, axis=1, inplace=True)

```

For columns with a few missing values, the easy way out is to fill with the mean/median. Of course there are more sophisticated ways of dealing with them (predicting their values with the other variables using regression) but let's keep it simple for now:

```python

# with mean
df[col].fillna(df[col].mean(), inplace=True)

# with median
df[col].fillna(df[col].median(), inplace=True)

# with mode
df[col].fillna(df[col].mode()[0], inplace=True)

```

Alternatively, you can fill them with values from another column.

```python

df[col1].fillna(df[col2]) # fill null values in col1 with values from col2, in the same row.

df[col1] = df[col1].combine_first(df[col2]) # combines the columns. takes col1 first, then col2 if col1 is null

```

A third method is to fill them with a specific value, or from the previous value (forwardfill) or the next value (backfill). This makes sense for timeseries data (for other types of data it might not make sense since the values you fill in can change drastically if you just shuffle the order).

```python

df.fillna(0) # fill all null values with 0
df.fillna('NA') # fill all null values with a string 'NA'
df.fillna(method='ffill') # forward fill. if backfill, replace with 'bfill'

```


<a id="outliers"></a> 
## Handling outliers

There are different methods to handling outliers:

1) Drop the entire row if you have sufficient data points to work with
2) Impute a new value if you think this is a data collection error; same methods as handling null values

These are very basic methods - read [here](https://www.neuraldesigner.com/blog/3_methods_to_deal_with_outliers) for more sophisticated models.

<a id="illogical"></a> 
## Dealing with unwanted values

Dropping the entire row.

```python

df.drop(df.index[2], inplace=True) # drops by index; [:3] works as well, drops top 3 rows.
df.drop(nameofrow, inplace=True) # drops by nameofrow

df[ df['col'] != 'testing' ] # only selects rows that are not 'testing'

```

Replace strings with null values.

```python

df[col].replace('NAN', np.nan, inplace=True)

```

Replacing all of a type of value to another value

```python

df[col] = df[col].map({'seventeen':17, 0:np.nan})

```

Replacing a value in one cell. For more info, see [here](https://stackoverflow.com/questions/13842088/set-value-for-particular-cell-in-pandas-dataframe-using-index)

```python

df.at['rowname', 'colname'] = 'newvalue' # this modifies df, and not just a copy of the df.

df.loc[df['col1'] == 3, 'col2'] = 6 # select row where col1 is 3, and change value of col2 (in that row) to 6.



```