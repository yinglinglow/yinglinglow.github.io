---
layout: post
title: "Exploring Data"
date: 2020-01-26
---

What are we looking out for when we are exploring our data?

First and foremost, we need our data to be complete and accurate. <br>
Hence we think about the below:<br>
1) How is the data structured - what does each row/column represent?<br>
2) Is the data complete? Look for missing data (null values)<br>
3) Is the data accurate? Look for illogical values (e.g. negative age)<br>

This is a very basic start - as you get more proficient in the Data field, you will discover that there is a lot more to data exploration, cleaning and manipulation!


__Table of Contents__
 * Data exploration
    - [Basic data structure](#structure)
    - [Numerical data exploration](#numerical)
    - [Categorical data exploration](#categorical)
    - [Data selection](#selection)

<a id="structure"></a> 
## Basic data structure

Look to understand how the data is structured, what are their types.

```python

df.info # returns info on shape, type of data, etc
df.head() # returns top 5 rows. for just 3 rows, use df.head(3)
df.tail() # returns bottom 5 rows - always helpful to check and make sure you don't have a 'total' row below!
df.columns # returns all column names in a list
df.index # returns index info

type(df['col'].iloc[0]) # takes one example from the column and identifies type of object in the column

df.describe(includes='all') # describes all types of data, both numberical and categorical

pd.set_option('max_columns', 30) # sets the max number of columns so you can see all of them

```

Look for null values, how many there are.

```python

df.isnull() # returns dataframe of same size, and a boolean for each value whether it is null or not
df.isnull().any() # returns a boolean for each column, whether it contains any null values or not
df.isnull().sum() # returns the number of null values in each column

```

<a id="numerical"></a> 
### Numerical data exploration

Try to understand the basic features of the data (do some basic visualisation if possible to better understand the distribution of the data), check for outliers.

```python

df.describe() # returns count, mean, median, std, max, min, quartiles
df['col'].nlargest(5) # returns top 5 values with their index
df['col'].idxmin() # returns just the index of the min value. idxmax does same for max

```

You can also use individual commands: `df.mean()`, `df.mode()`, `df.std()` etc.

For quantiles, if you want to get more than just 4 quantiles using `df.quantile([0.25, 0.5, 0.75])`, you can use the below:

```python

import numpy as np
quantiles = np.arange(0.1,1.0,0.1) # returns an array of numbers evenly spaced at a distance of 0.1, from 0.1 to 1.0
deciles = df.quantile(quantiles)

```

There are different methods to define outliers. One of them is using IQR (interquartile range), given by Q3 - Q1: < (Q1 - 1.5*IQR) OR > (Q3 + 1.5*IQR) 

```python

# get quantiles and iqr
q1, q2, q3 = df.quantile([0.25, 0.5, 0.75]
iqr = q3 - q1

# outlier rows
outliers_mask = [ (df[col] < (q1 - (1.5 * iqr))) | (df[col] > (q3 + (1.5 * iqr))) ]
outliers = df[outliers_mask]

# outliers removed
df = df[~outliers_mask]

```

Explore the correlation between variables.

```python

df.corr() # returns a correlation matrix for all variables. tells you the degree to which the variables move together.
df.cov() # returns a covariance matrix for all variables. does not use one standard unit of measurement, hence only tells you whether the variables are negatively or positively correlated.

```

To return a dataframe of top 5 highest correlation variable pairs, use the below:

```python

# save corr data into dataframe
corr_df = pd.DataFrame(df.corr().unstack()).reset_index(inplace=True)

# rename columns
corr_df.columns = ['var_1', 'var_2', 'corr']

# create extra column for absolute numbers
corr_df['corr_abs'] = corr_df['corr'].abs()

# remove corr = 1
corr_df = corr_df[corr_df['corr'] != 1]

# remove duplicates; select every alternate row
corr_df = corr_df.iloc[::2]

# show only top 5
corr_df.head()

```

Basic visualisation

```python

import pandas as pd
df['col1'].dropna().plot.hist() # plot histogram with na dropped
df[['col1','col2']].dropna().plot.hist(stacked=True) # stacked histogram to compare two variables
df.plot.box() # boxplot 

import seaborn as sns
sns.distplot(df[col], kde=False) # plots the distribution of the variable
sns.pairplot(df) # plots all variables against each other - not recommended if you have over 10 variables!

```

If you want to visualise several variables on one plot, it might be a good idea to normalise them to the same scale first. Quick note (because this confused me for a while too!): `apply` works on each row or column at a time, while `map` works on an element inside a column (one at a time). For more info, read [here](https://stackoverflow.com/questions/19798153/difference-between-map-applymap-and-apply-methods-in-pandas).

```python

def to_normalise(column):
    df_scaled = (df[column] - (df[column].mean()))/df[column].std()
    return df_scaled

df.apply(to_normalise) # applies this for each column

```

<a id="categorical"></a> 
### Categorical data exploration

```python

df['col'].value_counts(dropna=False) # returns the number of times each unique value occurs. for just top 5, use df['col'].value_counts().head()

df['col'].value_counts(normalize=True) # returns the same, in percentage

df['col'].unique() # returns all the unique values

df['col'].nunique() # returns the number of unique values

df['col'].dropna().astype(int).value_counts() # removes na, changes type from float to integers, and returns the counts.

```


<a id="selection"></a> 
## Data selection

```python

df.loc['2015-01-01':'2015-12-31'] # search by name of row and returns the corresponding rows. this example searches by datetime

df.iloc[0] # search by index. this returns first row
df.iloc[:,0] # this gives all rows, first column.

df.xs(level='name_of_level', key='name_of_col_in_level', axis=1) # default gets row in a multilevel dataframe. adding axis=1 takes column instead.

df[df['col'] == 'condition'] # returns rows in dataframe that fulfill the condition in 'col'
df[df['col'] == 'condition']['col2'] # returns rows in ['col2'] that fulfill the condition in 'col'

df[ (df['col']>2) & (df['col']<10) ] # selects and returns values that fulfill conditions - use & for multiple conditions and put () around each condition

df[(df['col'] == 1) | (df['col'] == 5)] # OR condition

```