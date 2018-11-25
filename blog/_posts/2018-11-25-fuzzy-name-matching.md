---
layout: post
title: "Fuzzy Name Matching with TF-IDF and ngrams"
date: 2018-11-25
---

check out my new repo with code for name matching! hohoho

https://github.com/yinglinglow/namematching

### Name Matching (Using TF-IDF and cosine similarity)

__What is it?__

This is code to help you match names easily. It takes in a csv source file and returns a csv file with the matched names and score. It first matches by word, then by ngrams(5-character).


__To run:__ 

```bash
main.py <source_filepath> <destination_filepath>
```

Make sure you format the source filepath as below:
- two columns, without column names
- first column is the list of words you want to find matches for
- second column is the list of words to search in