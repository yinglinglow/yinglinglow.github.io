---
layout: post
title: "Select non-numeric rows"
date: 2018-10-27
---

So, I have a value column in my dataframe (from file A) that holds everything as a string. meaning:

    __id__      __value__
    101    '123'
    102    '234'
    103    '345'
    104    'error'

now, i only want the numbers (in float). and for those values with strings (in this case, 'error'), i need to check the row ids (i.e. 104) with file B. if the file B contains the same id with a float value, i use the value from file B. else, i drop the row from file A.

below is the code i use:

```python

import sys
import pandas as pd

def select_non_numeric(current_df):
    current_df_num = current_df.copy()
    current_df_num['value_num'] = current_df_num['value'].apply(pd.to_numeric, errors='coerce')
    non_numeric_ids = list(current_df_num[current_df_num['value_num'].isnull()]['id'])
    return non_numeric_ids

def replace_non_numeric(current_df, previous_df):
    non_numeric_ids = select_non_numeric(current_df)

    for error_id in non_numeric_ids:
        if error_id in list(previous_df['id']):
            current_df.loc[current_df['id'] == error_id, 'value'] = previous_df.loc[previous_df['id'] == error_id, 'value']
        else:
            current_df = current_df[current_df['id'] != error_id]
    
    return current_df

if __name__ == '__main__':
    # for debug
    # filepath = 'C:\\blahblah\\filename.csv'
    # sys.stdout = open("C:/check/stdout.txt", "w")
    # sys.stderr = open("C:/check/stderr.txt", "w")
    # end debug
    
    # read in variables
    filepath_current = sys.argv[1]
    filepath_previous = sys.argv[2]
    
    # run function
    previous_df = pd.read_csv(filepath_previous)
    current_df = pd.read_csv(filepath_current)
    current_df = replace_non_numeric(current_df, previous_df)

```