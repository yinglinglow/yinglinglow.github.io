---
layout: post
title: "hadoop, data warehousing, tableau"
date: 2019-02-11
---

so, the challenge is:

where should i store my business logic? 

I don't want to store it in my SQL because it's crazy hard to maintain with all the nested queries and all. 
SQL should just be used to query WHHAT you need, not how you need it... right?

I also don't want to store it in Tableau, ALSO because its hard to maintain. The good thing is then the drilldown becomes straightforward; but then, how do I do testing to make sure the data manipulation still gives the correct numbers after aggregation and all?

---

hadoop is not quite a data warehouse - see the below: 
- https://www.kdnuggets.com/2017/06/hadoop-data-warehouse-kudu.html
- https://www.educba.com/data-warehouse-vs-hadoop/


side track a little (always helps to crystallise this):

https://www.thoughtspot.com/fact-and-dimension/dimensional-data-modeling-4-simple-steps


on using hadoop and dimensional modelling:

https://en.wikipedia.org/wiki/Dimensional_modeling#Dimensional_Models,_Hadoop,_and_Big_Data


---

ok after a million years of stumbling around - the answer is, get a data warehouse layer in-between Hadoop and Tableau!

or so i think...

https://blog.panoply.io/connect-to-data-warehouses-in-tableau

i guess snowflake is quite interesting:

https://medium.com/hashmapinc/snowflakes-cloud-data-warehouse-what-i-learned-and-why-i-m-rethinking-the-data-warehouse-75a5daad271c

---

ok final answer: use snowflake, redshift or cloudera as the data warehouse layer inbetween hadoop and tableau!

Tableau and Redshift: https://www.tableau.com/solutions/workbook/exploring-big-data-cloud

Tableau and Cloudera: https://onlinehelp.tableau.com/current/pro/desktop/en-us/examples_hadoop.htm

Tableau and Snowflake: https://www.tableau.com/solutions/snowflake

---

so actually... slide 4 of this deck could have solved all my problems: https://www.slideshare.net/RTTS/what-is-a-data-warehouse-and-how-do-i-test-it


and omg THIS!!!!

https://medium.com/python-pandemonium/develop-your-first-etl-job-in-python-using-bonobo-eaea63cc2d3c
