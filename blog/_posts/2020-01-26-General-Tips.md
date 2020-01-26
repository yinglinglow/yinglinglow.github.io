---
layout: post
title: "General Tips and Tricks"
date: 2020-01-26
---

Random formats for reference, useful or interesting tips that I find helpful/speeds things up!

Basic commands:

```python

df = pd.DataFrame() #creates an empty dataframe

print('MAE: {0}, MSE: {1}, RMSE: {2}'.format(MAE,MSE,RMSE)) #a different format for printing with variables, instead of %s or %d

print(f'MAE: {MAE}, MSE: {MSE}, RMSE: {RMSE}') #f-strings format for printing

```

Time-savers:

```python

df.apply(lambda column: column.function()) #applies a function across columns

df['Email'].apply(lambda x: x.split('@')[1]) #splits emails by @ and takes the username

np.arange(0.1,1.0,0.1) #returns an array of numbers evenly spaced at a distance of 0.1, from 0.1 to 1.0

for column in df.columns:
    sns.distplot(df[column], hist=False, label=column) #iterates over each col and plots it

```

For data visualisation:

```python
sns.heatmap(df.isnull(), ytickslabels=False, cbar=False, cmap='viridis') #a quick visual representation of all null values

```

General libraries used in treating a dataset:

```python
import pandas as pd
import numpy as np

import matplotlib as plt
import seaborn as sns
%matplotlib inline #to display plots inline in jupyter notebook

```