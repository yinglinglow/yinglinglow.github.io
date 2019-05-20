---
layout: post
title: "Name Matching"
date: 2019-05-20
---

Cleaned up my code for name-matching!

Check out the documentation here:

https://github.com/yinglinglow/namematching

ok actually i can also just paste it below lol

---

### Name Matching: Using TF-IDF (cosine similarity), and fuzzywuzzy (levenshtein)


__What does the code do?__

Helps you match names easily across two lists of names. 

Takes in two lists of names (names_to_match, names_to_search_in) and a csv file cleaning_dict.csv

Returns a dataframe of names_matched.

You can add/remove cleaning rules in cleaning_dict.csv by using the first two columns (raw_string & clean_string). 

You can also add/remove a list of stopwords in the third column of cleaning_dict.csv

__Install Dependencies:__
- pip install sklearn
- pip install fuzzywuzzy
- pip install python-levenshtein (optional)

__To run:__ 

```python
# save entityresolution.py and cleaning_dict.csv in the same folder as your code
from entityresolution import NameMatching

# create an instance of NameMatching
cleaning_dict_fp = "cleaning_dict.csv"
name_matching = NameMatching(cleaning_dict_fp)

# pass in your lists of names to match 
names_to_match = ['guardian life insurance co', 'tokio marine insurance', 'good life insurance', 'aviva health insurance']
names_to_search_in = ['guardian life insurance co america', 'tokio marine life', 'aviva insurance']

# clean and match names. pass in the name of the algorithm to use: 'ngrams_tfidf', 'word_tfidf' or 'fuzz_levenshtein'
# if 'all' is given, all algorithms are run
name_matching.clean_and_matchnames(names_to_match, names_to_search_in, 'all')

# results are saved in a dataframe in names_matched
result = name_matching.names_matched

# get the potential matches, de-duplicated
result.sort_values('names_to_match').drop_duplicates(subset=['names_to_match', 'names_to_search_in'])

```

__Sample output__

| names_to_match | names_to_search_in | score | algorithm | names_to_match_clean | names_to_search_in_clean |
| --- | --- | --- | --- | --- | --- |
| Aviva Health Insurance | AVIVA INSURANCE | 0.95 | fuzz_levenshtein | aviva health insurance | aviva insurance |
| Good Life, Insurance | GUARDIAN LIFE INSURANCE CO AMERICA | 0.86 | fuzz_levenshtein | good life insurance | guardian life insurance co america |
| Guardian Life Insurance Co. | GUARDIAN LIFE INSURANCE CO AMERICA | 0.95 | fuzz_levenshtein | guardian life insurance co | guardian life insurance co america |
| Tokio Marine Insurance | GUARDIAN LIFE INSURANCE CO AMERICA | 0.86 | fuzz_levenshtein | tokio marine insurance | guardian life insurance co america |
| Tokio Marine Insurance | TOKIO MARINE LIFE | 0.778 | ngrams_tfidf | tokio marine insurance | tokio marine life |

---

__Older Version (To be updated)__

Takes in a csv source file and returns a csv file with the matched names and score. It first matches by exact match of the words (after cleaning the name strings), and then by averaging the score of cosine similarity between tf-idf scores of the names vectorised by word, by ngrams(3-character), and fuzzy partial ratio of the names.

__To run:__ 

```bash
main.py <source_filepath> <destination_filepath> <cleaning_dict_filepath>
```

Make sure you format the csv source file as below:
- __names_to_match__: this is the list of words you want to find matches for
- __names_to_search_in__: this is the list of words to search in
- __names_to_match_group__: (optional) this is the groupings for the names. if this is provided, only names from the same group can be matched. e.g. if a company belongs in the US, only other names of companies belonging in the US can potentially be matched.
- __names_to_search_in_group__: (optional) this is the groupings for the names.