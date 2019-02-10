---
layout: post
title: "SQL Alchemy"
date: 2019-02-10
---

seriously? are we halfway into february ALREADY?! i feel like new year's was like, just last week?! this is insane.

also i have been neglecting blogging - only 3 posts since the start of december! oh no

---

ok let's actually do something useful today - SQL Alchemy!

credits to here for the below code: https://towardsdatascience.com/sqlalchemy-python-tutorial-79a577141a91

and because i am using Presto at work: https://github.com/dropbox/PyHive

```python

# SQLAlchemy with Presto & LDAP
import sqlalchemy as db
engine = db.create_engine(
    'hive://user:password@host:10000/database',
    connect_args={'auth': 'LDAP'},
)

# connect and get metadata
connection = engine.connect()
metadata = db.MetaData()

# autoload a table called census
census = db.Table('census', metadata, autoload=True, autoload_with=engine)

# print column names
print(census.columns.keys())

# print full census table metadata
print(repr(metadata.tables['census']))

# equivalent to 'SELECT * FROM census'
query = db.select([census])

# execute the query and returns an object
ResultProxy = connection.execute(query)

# get the results from the object
ResultSet = ResultProxy.fetchall()

# see top 3 results
ResultSet[:3]

# convert to dataframe
df = pd.DataFrame(ResultSet)
df.columns = ResultSet[0].keys()

```

and the below is a single example of the SQL code in SQLAlchemy. 
for more examples definitely check out https://towardsdatascience.com/sqlalchemy-python-tutorial-79a577141a91

and this is the actual repo: https://github.com/vinaykudari/hacking-datascience/tree/master/notebooks/sqlalchemy

## sample code

```python
SQL :
SELECT state, sex
FROM census
WHERE state IN (Texas, New York)

SQLAlchemy :
db.select([census.columns.state, census.columns.sex]).where(census.columns.state.in_(['Texas', 'New York']))
```

## get raw sql

and if you need to get the raw SQL out (SO IMPORTANT!!!)

thanks to code from here: https://stackoverflow.com/questions/2128717/sqlalchemy-printing-raw-sql-from-create

```python
# example for CREATE table
# compile with engine
print(CreateTable(Model.__table__).compile(engine))

# compile without engine
print(CreateTable(Model.__table__).compile(dialect=postgresql.dialect()))

```

## unit test

testing...!! also from the same link

```python
from sqlalchemy import create_engine
from sqlalchemy.schema import CreateTable
from model import Foo

sql_url = "sqlite:///:memory:"    
db_engine = create_engine(sql_url)

table_sql = CreateTable(Foo.table).compile(db_engine)
self.assertTrue("CREATE TABLE foos" in str(table_sql))
```


## dbschema changes

not yet sure how this comes into play but keeping the link here for rainy days lol

https://sqlalchemy-migrate.readthedocs.io/en/latest/


## creating views
https://stackoverflow.com/questions/9766940/how-to-create-an-sql-view-with-sqlalchemy


## nested queries
https://stackoverflow.com/questions/28090140/nested-select-using-sqlalchemy

https://medium.com/@kagiri/flask-sqlalchemy-nested-queries-64278f1ef39c

## 'with' query
https://stackoverflow.com/questions/31620469/sqlalchemy-select-with-clause-statement-pgsql


---

now after reading all this i have an inkling that this does not solve my problem at all...