---
layout: post
title: "Getting Data In"
date: 2020-01-26
---

__Table of Contents__
 * Import data from a file 
    - [CSV](#csv)
    - [Excel](#excel)
    - [JSON](#json)

 * Connecting to servers
    - [MSSQL](#mssql)

 * Scraping data from the web
    - [Finance data (using Pandas Datareader)](#datareader)
    - [Tables (using Pandas)](#tables)
    - [Text (using BeautifulSoup)](#text)

 * Importing data from APIs
    - [API](#api)

## Importing data from a file

<a id="csv"></a> 
### CSV files  

```python

import pandas as pd
df = pd.read_csv('/Users/abc/Documents/folder/file.csv')

```

If somehow there is an error reading the file, try tweaking around with: header=None, sep='delimiter', skiprows=2 (this skips the first two rows which might not be data), or error_bad_lines=False (this skips errors).

To read in null values correctly, replace `'NAN'` with the current shortform for your NA values.

To read in dates as the datetime format, replace `'date_column'` with the name of your column containing dates. 

To set the first column as the index, use index_col=0.

```python

import pandas as pd
df = pd.read_csv('file.csv', na_values='NAN', parse_dates=['date_column'], error_bad_lines=False, header=None, index_col=0)

```


 <a id="excel"></a>
### Excel files

Default is `sheet_name = 0` which only takes in the first sheet of the excel. To read in all sheets, use `sheet_name = None` which returns all sheets as a dictionary of dataframes. You can also specify say [1,2,5] to return the second, third and 6th sheet as a dictionary of dataframes.

To read in null values correctly, replace `'NAN'` with the current shortform for your NA values.

```python

import pandas as pd
df = pd.read_excel('file.xlsx', na_values='NAN', sheet_name='0')

```


<a id="json"></a>
### JSON files 

```python

import pandas as pd
df = pd.read_json('file.json')
df

```

<a id="mssql"></a>
## Connecting to servers such as MSSQL

Check this out: [https://gist.github.com/hunterowens/08ebbb678255f33bba94](https://gist.github.com/hunterowens/08ebbb678255f33bba94)

Using SQLalchemy to create an engine to connect to SQLite/ PostgreSQL is also possible.

<a id="datareader"></a>
## Importing/ scraping data from the webdata from the web 

### Finance data (using Pandas Datareader)

Pandas Datareader is able to easily extract data from some sources, including: Yahoo!Finance, Google Finance, World Bank, and more. Find the full list [here](https://pandas-datareader.readthedocs.io/en/latest/remote_data.html)

```python

from pandas_datareader.data import DataReader 
from datetime import date 

# Set your variables
start = date(YYYY, MM, DD) # for example, 2010-1-1
end = date(YYYY, MM, DD) # default date is today
data_source = 'google' # the source of your data. find the full list from the above link
ticker = 'AAPL' # the ticker symbol of your stock

stock_prices = DataReader(ticker, data_source, start, end)

```

If you use a list for the ticker it works as well but the result is a panel (3d array). Unless you are very comfortable with unstacking, I would advise against using a list.

<a id="tables"></a>
### Tables (using Pandas)

This automatically converts all tables in the webpage of the given url into dataframes.
In this example I have saved them all to dfs.
To select a particular table after this, say I want the 5th table, I can call df[6].

```python

import pandas as pd
url = 'http://'
dfs = pd.read_html(url)

```

To loop over many urls, I break the url up:

```python

import pandas as pd
front_url = "https://maps.googleapis.com/maps/api/geocode/json?address="
end_url = "&components=country:SG&key=XXXX-XXXXX"

for row in df['Address']:
    url = front_url + row.replace(' ', '+') + end_url
    dfs = pd.read_html(url)

```
<a id="text"></a>
### Text (using BeautifulSoup)

```python

import pandas as pd
import requests
from bs4 import BeautifulSoup
url = 'http://'

resp = requests.get(url)
html_doc = resp.text
soup = BeautifulSoup(html_doc, 'html.parser')

```

All the information is now in the variable soup. If I want to extract certain information, I can do so like below:


```python

title = soup.title # gives the title, including the tags
title.text.strip() # strips the tags away leaving the text

box = soup.find(class_="graybox") # finds the input. works for many things including class, p, etc

links = soup.find_all('a') # finds all the links

```

For more ways to work the soup, go [here](https://www.crummy.com/software/BeautifulSoup/bs4/doc/#calling-a-tag-is-like-calling-find-all)

<a id="api"></a>
## Importing data from APIs

I usually request the API to return the information in JSON format. Hence, I read it just as I would a JSON file. Below is an example to loop over numerous urls

```python

front_url = "https://maps.googleapis.com/maps/api/geocode/json?address="
end_url = "&components=country:SG&key=XXXX-XXXXX"

for row in df['Address']:
    url = front_url + row.replace(' ', '+') + end_url
    address = pd.read_json(url)
    latitude = address['results'][0]['geometry']['location']['lat']
    longitude = address['results'][0]['geometry']['location']['lng']

```

If you struggle to decipher your JSON, some people find it helpful to go [here](http://jsonviewer.stack.hu/). I find it easier to just unwrap it layer by layer...