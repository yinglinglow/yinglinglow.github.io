---
layout: post
title: "Working with large files (bash)"
date: 2019-06-24
---

__to count number of rows in file:__

```bash

wc -l largefile.csv

```

__to see the top 5 rows in file:__

```bash

head -n 5 largefile.csv

```

__to filter rows by certain column in file__ (e.g. column 5 > 20 and != 255) and output another file

```bash

awk '$5 >= 20 && $5 != 255{print $0}' largefile.csv > output.csv

```

modified from https://stackoverflow.com/questions/29503699/filtering-a-csv-file-with-awk

also, check out https://www.tim-dennis.com/data/tech/2016/08/09/using-awk-filter-rows.html for more details.

