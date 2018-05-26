---
layout: post
title: "filling data in from another column"
date: 2018-05-26
---

see here: https://stackoverflow.com/questions/29991862/copy-value-from-one-column-based-on-the-value-of-another-column

in short, if you want to replace all the column As with column Bs, but only in rows where column D is equivalent to 'Test', use:

```python

df_copy.loc[df['D']=='Test', 'A'] = df['B']

```

---

if you just wanted to fill nas from another column:

```python

df['A'] = df['A'].fillna(df['B'])

```