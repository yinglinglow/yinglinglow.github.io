---
layout: post
title: "Outlier Detection Again"
date: 2018-07-03
---

Yes, same old, nothing new, just tidied the code up a little for linking to presto, and added labelling for the outliers.

```python

import pandas as pd
import numpy as np
from pyculiarity import detect_ts
import matplotlib.pyplot as plt
from pyhive import presto

def extract_from_presto(query):
    # connect to presto
    cursor =  presto.connect(
        host='', # insert your own credentials here!!
        port='',
        username='',
        catalog='',
        schema='').cursor()
    # query from presto
    cursor.execute(query)
    rows = cursor.fetchall()
    # put into dataframe
    df = pd.DataFrame(rows, columns=['cob', 'value'])
    return df

def run_model(df):
    # convert cob string to datetime, then to unix timestamp integer
    df['cob'] = pd.to_datetime(df['cob'])
    df = df.sort_values('cob')
    df['cob'] = df['cob'].astype(np.int64) // 10**9
    # run the model
    results = detect_ts(df, max_anoms=0.005, alpha=0.001, direction='both')
    # format the unix timestamp integer back to datetime
    df['cob'] = pd.to_datetime(df['cob'])
    return df, results

def plot_graphs(df, results, destination_filepath):
    # plot the graphs
    plt.figure(figsize=(20,10))
    plt.title('Detected Anomalies')
    plt.ylabel('Value')
    ax = plt.plot(df['cob'], df['value'], 'b')
    ax = plt.plot(results['anoms'].index, results['anoms']['anoms'], 'ro')

    labels = [x.strftime('%d-%b-%Y') for x in list(results['anoms'].index)]
    coordinates = zip(results['anoms'].index, results['anoms']['anoms'])

    for label, coordinates in zip(labels, coordinates):
        plt.annotate(label, coordinates)

    # save to filepath
    plt.savefig(destination_filepath)


if __name__ == '__main__':

    query = """
    SELECT cob, value
    FROM tablename
    """

    destination_filepath = 'graph_name.png'

    # extract data
    df = extract_from_presto(query)
    # run model and return results
    df, results = run_model(df)
    # plot graph
    plot_graphs(df, results, destination_filepath)

```