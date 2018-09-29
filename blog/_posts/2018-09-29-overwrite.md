---
layout: post
title: "Overwrite rows in Pandas"
date: 2018-09-29
---

Because it took me unexpectedly long to do it (I was busy trying replace, np.where, combine_first,etc etc), here you go: how to overwrite certain rows in your current dataframe, if present in another dataframe (not necessarily a subset of your current dataframe). for all columns.

```python

# overwrite
df1.loc[df1['id'].isin(df2['id']), df1.columns] = df2.loc[df1['id'].isin(df2['id']), df1.columns]

```