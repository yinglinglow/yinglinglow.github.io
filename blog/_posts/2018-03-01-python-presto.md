---
layout: post
title: "Python and Presto (SQL)"
date: 2018-03-01
---

before we jump in - ARE WE ALREADY IN MARCH?!?! this is crazy - i haven't done ANYTHING at all and a quarter of 2018 has just flown by like that. well i guess time flies when you're having fun, but this is wayyy too crazy guys.

ok back to work LOL

---

__What is Presto?__
It is a query engine. Meaning, it runs your query to extract data from your database. IT IS NOT A DATABASE.

__Why use Presto?__
- It is fast - Facebook developed Presto to handle huge amounts of data, quickly. We are talking 30k queries a day, 300PB warehouse.

- Presto can access all data sources regardless of the underlying data platform (even relational databases like mySQL, Oracle, etc) and combine them in a single query(!!). Check out [this](https://www.linkedin.com/learning/presto-essentials-data-science/combine-data-sources) video to see how it works.

__How to use Presto?__
Presto is extremely well-documented; check [this](https://prestodb.io/docs/current/) out - it can be a little much! If you ever do need to set up, [this] (http://getindata.com/tutorial-using-presto-to-combine-data-from-hive-and-mysql-in-one-sql-like-query/) seems helpful.

I haven't exactly tried it out for now, but future note if I run into issues using presto-python-client:
[http://www.swethasubramanian.com/2017/12/29/connecting-to-presto-database-using-python/](http://www.swethasubramanian.com/2017/12/29/connecting-to-presto-database-using-python/)

Also, Presto SQL uses ANSI-SQL: basically a standard for SQL syntax, so that it can talk to many different databases! There might be potentially some conflicts, but minor ones if I understood correctly...

---

now, practising my SQL skills on datacamp! hmmm....

---

doing two inner joins (ALWAYS double check your data to make sure it makes sense!)
ORDER BY must always be behind!!!

```SQL
SELECT c.code, c.name, c.region, e.year, p.fertility_rate, e.unemployment_rate
FROM countries AS c
INNER JOIN populations AS p
ON c.code = p.country_code 
INNER JOIN economies AS e
ON c.code = e.code AND p.year = e.year;
```

doing an inner join with itself to retrieve different year data

```SQL
SELECT p1.country_code, 
       p1.size AS size2010,
       p2.size AS size2015,
       ((p2.size-p1.size)/p1.size)*100.0 AS growth_perc
FROM populations AS p1
INNER JOIN populations AS p2
ON p1.country_code = p2.country_code AND p1.year = p2.year - 5;
```

if else in SQL to create new column

```SQL
SELECT name, continent, code, surface_area,
    CASE WHEN surface_area > 2000000
            THEN 'large'
       WHEN surface_area > 350000
            THEN 'medium'
       ELSE 'small' END
       AS geosize_group
FROM countries
WHERE year = 2015;
```

piping in SQL; two queries!

```SQL
SELECT country_code, size,
    CASE WHEN size > 50000000
        THEN 'large'
    WHEN size > 1000000
        Then 'medium'
    ELSE 'small' END
    AS popsize_group
INTO pop_plus
FROM populations
WHERE year = 2015;
SELECT c.name, c.continent, c.geosize_group, p.popsize_group
FROM countries_plus AS c
INNER JOIN pop_plus AS p
ON c.code = p.country_code
ORDER BY c.geosize_group DESC;
```

groupby

```SQL
SELECT region, AVG(gdp_percapita) AS avg_gdp
FROM countries AS c
LEFT JOIN economies AS e
ON c.code = e.code
WHERE year = 2010
GROUP BY region
ORDER BY avg_gdp DESC;
```

other conditions

```SQL
SELECT countries.name, code, languages.name AS language
FROM languages
FULL JOIN countries
USING (code)
WHERE countries.name LIKE 'V%la' OR countries.name IS NULL
ORDER BY countries.name, year
LIMIT 5;
```

union (set theory). removes duplicates. to include, use `UNION ALL` instead.<br>
for intersection, use `INTERSECT`. for no overlaps, use `EXCEPT`

```SQL
SELECT c1.country_code AS country_code
FROM cities AS c1
UNION
SELECT c2.code AS country_code
FROM currencies AS c2
ORDER BY country_code;
```

semi-join

```SQL
SELECT DISTINCT name
FROM languages
WHERE code IN
    (SELECT code
    FROM countries
    WHERE region = 'Middle East')
ORDER BY name;
```

subquery

```SQL
SELECT name AS country,
  (SELECT COUNT(*)
  FROM cities
  WHERE countries.code = cities.country_code) AS cities_num
FROM countries
ORDER BY cities_num DESC, country
LIMIT 9;
```

```SQL
SELECT DISTINCT c.name, e.total_investment, e.imports
FROM countries AS c
LEFT JOIN economies AS e
ON (c.code = e.code
    AND c.code IN (
        SELECT l.code
        FROM languages as l
        WHERE official = 'true'))
WHERE c.region = 'Central America' AND e.year = 2015
ORDER BY c.name;
```


---

progressed to hackerrank to practice SQL! it's quite difficult... _gulps_

---

i guess postgresql is very different from Oracle!!!
for selecting even numbers. if not Oracle, use `ID % 2 = 0` instead!

```SQL
SELECT DISTINCT city
FROM STATION
WHERE mod(ID,2) = 0;
```

for selecting the number of distinct values in the city column

```SQL
SELECT count(DISTINCT city)
FROM station;
```

to select the max and min length of characters in the city name

```SQL
SELECT City, LENGTH(City)
FROM (SELECT City
      FROM Station
     ORDER BY LENGTH(City), City)
WHERE ROWNUM = 1;
SELECT City, LENGTH(City)
FROM (SELECT City
      FROM Station
     ORDER BY LENGTH(City) DESC, City)
WHERE ROWNUM = 1;
```