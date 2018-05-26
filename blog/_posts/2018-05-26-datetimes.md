---
layout: post
title: "dealing with datetimes"
date: 2018-05-26
---

if you are importing dates from excel and they are appearing in 6 digit numbers...

see here on how to solve it:
https://stackoverflow.com/questions/29387137/how-to-convert-a-given-ordinal-number-from-excel-to-a-date

```python

from datetime import date, timedelta

def from_excel_ordinal(ordinal, _epoch=date(1900, 1, 1)):
    if ordinal > 59:
        ordinal -= 1  # Excel leap year bug, 1900 is not a leap year!
    return _epoch + timedelta(days=ordinal - 1)  # epoch is day 1

```

---

```python

# preventing date parsing when importing
import pandas as pd
df = pd.read_csv('test.csv', parse_date=False)

# changing to datetime
df['date'] = pd.to_datetime(df['date'])

# to create dates from one date, with number of days offset
date = []
df['num_days'] = df['num_days'].astype(int)
for _, row in df.iterrows():
    date.append(df['date'][0] + pd.DateOffset(row['num_days']))
df['date'] = date

# to string format
df['date_str'] = df['date'].map(lambda x: x.strftime("%d-%b-%Y"))
```