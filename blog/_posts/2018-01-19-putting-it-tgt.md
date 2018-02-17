---
layout: post
title: "Putting it together - from data scraping to running the model on AWS"
date: 2018-01-19
---

with 80,000 images, the headache begins.
for convenience sake (and my own clarity lol), here is the workflow below.
pardon the extra step in extracting just the links #regret - it is possible to download the images directly with scrapy.

__1) Extract links from Wikipedia__
This saves all the links into 'items.csv' in a tab delimited csv, using my spider called 'myspider.py'.

```bash
scrapy runspider myspider.py -o items.csv -t csv
```

__2) Download pictures from links, crop and resize (50x50) and upload to AWS__

```bash
python3 uploading.py
```

__3) Resize (28x28) and resave it to another bucket in AWS__

```bash
python3 resize_28_upload.py
```

__4) Save pictures to an array in AWS__

```bash
python3 pickle_img_array.py
```

__5) Run test model on AWS__

```bash
python3 POC_adapted_28_aws.py
```
