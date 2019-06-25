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

head -5 largefile.csv

```

__to filter rows by certain column in file__ (e.g. column 5 > 20 and != 255) and output another file

```bash

awk '$5 >= 20 && $5 != 255{print $0}' largefile.csv > output.csv

```

__to filter rows by certain column (numerical only - e.g. col 3 == 99) in file and output to another file, with header (pipe-delimited)__

```bash

head -1 largefile.csv > output.csv |
awk -F "|" '$3 == 99 {print $0}' largefile.csv >> output.csv

```

__to filter rows by certain column (string - e.g. col 1 == 'dec') in file and output to another file, with header (pipe-delimited)__

```bash

head -1 largefile.csv > output.csv |
awk -F "|" 'match($1,/dec/) {print $0}' largefile.csv >> output.csv

```

__to filter rows containing string and output to another file, with header__

```bash

head -1 largefile.csv > output.csv |
awk '/dec/' largefile.csv >> output.csv

```

modified from https://stackoverflow.com/questions/29503699/filtering-a-csv-file-with-awk

also, check out https://en.wikibooks.org/wiki/An_Awk_Primer/Awk_Command-Line_Examples for more details.

https://www.theurbanpenguin.com/filtering-with-awk/ is also very helpful!!
