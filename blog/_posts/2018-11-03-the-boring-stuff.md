---
layout: post
title: "Creating folders, moving and unzipping files with Python"
date: 2018-11-03
---

When you run daily reports and stuff, there's the folder you need to create everyday.
When you get csv files from other teams (no api), there's the file you need to transfer everyday.
All's fine and dandy, until one day you realise you've been doing something wrong for the past 2 months and you need to rerun everything...

---

Now, you can do all this using the command line but work is Windows (also, even though we are on Windows 10, I don't have Bash for Windows installed yet!! I must remember to request for it...) and home is bash, and bash is where the heart is... lol

more like, i was lazy to learn the commands for command prompt/powershell lol

so python here we come!

```python

# to create dates
for month in range(8,10):
    for day in range(1,3):
        print('2018' + '{:02}'.format(month) + '{:02}'.format(day))

```

```python

# to create folders
import os

for date in datelist:
    dest_folder = f'C:/xxx/Data/{date}/'
    if not os.path.exists(dest_folder):
        os.makedirs(dest_folder)

```

```python

# to copy files
import shutil

for date in datelist:
    dest = f'C:/xxx/Data/{date}/file.txt'
    src = f'C:/xxx2/Data/{date}/file.txt'
    shutil.copyfile(src, dest)

```

```python

# to move files
import shutil

for date in datelist:
    dest = f'C:/xxx/Data/{date}/file.txt'
    src = f'C:/xxx2/Data/{date}/file.txt'
    shutil.move(src, dest)

```

```python

# to unzip zipfiles
import zipfile

for date in datelist:
    dest_folder = f'C:/xxx/Data/{date}/'
    src = f'C:/xxx2/Data/{date}/file_{date}.zip'
    filename = f'file_{date}.txt'

    try:
        with zipfile.ZipFile(src) as zf:
            zf.extract(filename, path=dest_folder)
            
    except:
        print(src)

```

```python

# to unzip gzipfiles
import shutil
import gzip

for date in datelist:
    dest = f'C:/xxx/Data/{date}/file_{date}.txt'
    src = f'C:/xxx2/Data/{date}/file_{date}.zip'

    try:
        with gzip.open(src, 'rb') as f_in:
            with open(dest, 'wb') as f_out:
                shutil.copyfileobj(f_in, f_out)
            
    except:
        print(src)

```