---
layout: post
title: "Python Equivalent of ifelse in R"
date: 2018-08-30
---

When you teach people, you learn!

Look at things in a different way, from another perspective. And you become much more efficient!

The most efficient way to do an ifelse ala Excel:

In R, it would be:
```R

df$X <- if(df$A == "-1"){-df$X}else{df$X}

```


In Python, it would be: 

```python

import numpy as np
np.where(df['A'] < 1, -df['X'], df['X'])

```

check the full post out at stackoverflow: https://stackoverflow.com/questions/43711020/python-version-of-rs-ifelse-statement/43711056#43711056