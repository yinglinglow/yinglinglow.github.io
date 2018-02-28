---
layout: post
title: "From Python to R"
date: 2018-02-26
---

today i begin learning R LOL

no choice, gotta pick it up for work. i guess it's always good to learn more languages!

i begin by searching for a python/r cheatsheet - this is pretty good for a quick peek into the differences in the syntax:

[https://www.analyticsvidhya.com/blog/2015/09/full-cheatsheet-machine-learning-algorithms/](https://www.analyticsvidhya.com/blog/2015/09/full-cheatsheet-machine-learning-algorithms/)

this is more indepth (in particular basic R gives an interesting overview):
[http://www.datasciencefree.com/cheatsheets.html](http://www.datasciencefree.com/cheatsheets.html)

[http://www.datasciencefree.com/basicR.pdf]
(http://www.datasciencefree.com/basicR.pdf)


for tidyverse:

[http://datacamp-community.s3.amazonaws.com/e63a8f6b-2aa3-4006-89e0-badc294b179c](http://datacamp-community.s3.amazonaws.com/e63a8f6b-2aa3-4006-89e0-badc294b179c)

---

but for now...i need some real (basic) practice. 
datacamp, here i come!


---

most important takeaways so far:

R BEGINS WITH INDEX 1 INSTEAD OF 0!!! faints.

vector <- c(x, y, z) creates a vector, and assigns it to vector. 1:9 is a shortcut for creating a vector from 1 to 9.

name(vector) names the parts of the vector! interesting.

selection of x[1:5] INCLUDES both 1 and 5

matrix(1:9, byrow = TRUE, nrow = 3) constructs a 3 row matrix from the vector 1:9, filled in by rows (instead of by columns)

colnames(matrix), rownames(matrix) can be used to name the matrix

cbind(matrix, vector) can be used to join matrices/vectors n vectors together.

rbind(matrix, matrix) are for two matrices

factor(vector) changes it to a factor, and by changing the level(factor), you can change all the names (e.g. M,F to Male, Female)!!! homg

summary(factor) is the equivalent of .describe(), summary(vector) gives info about the length, type, etc

factor_speed_vector <- factor(speed_vector, ordered=TRUE, levels=c('slow', 'medium', 'fast')) this creates an ordinal factor, levels is the correct order.


str(dataframe) gives the structure of the df
head(dataframe) is equivalent to pandas df.head()

data.frame(x, y, z) creates a df

dataframe$col selects the col

subset(dataframe, subset = x < 1) selects the rows where x<1

use position <- order(df$col) to get the positions sorted by col, then df[position,] to get the sorted list

list(x, y, z) to create lists

list[[1]] to select first element from list

list[['actors']][2] selects the second element in the actors col


---

RStudio is like, the equivalent of Jupyter Notebook (I think!)

Download R, and then download RStudio.

i'm following this tutorial: [https://www.computerworld.com/article/2497143/business-intelligence/business-intelligence-beginner-s-guide-to-r-introduction.html?page=2](https://www.computerworld.com/article/2497143/business-intelligence/business-intelligence-beginner-s-guide-to-r-introduction.html?page=2)

as a start to actually get coding, and then trying stuff out on the Titanic Dataset.

[https://www.kaggle.com/mrisdal/exploring-survival-on-the-titanic](https://www.kaggle.com/mrisdal/exploring-survival-on-the-titanic)


install.packages('packagename') is like the equivalent of pip install

---

if you come across the 'tar: failed to set default locale error', use the solution here:
[https://stackoverflow.com/questions/3907719/how-to-fix-tar-failed-to-set-default-locale-error](https://stackoverflow.com/questions/3907719/how-to-fix-tar-failed-to-set-default-locale-error)

---

so i'm trying to do a `df.isnull().sum()` in R...

after a bit of a search, `sapply(df, function(x) sum(is.na(x)))` or `map(df, ~sum(is.na(.)))` seems the cleanest - the former requires no dependencies, the latter needs tidyverse. also i like the former's format a little better...

here's a summary: [https://sebastiansauer.github.io/sum-isna/](https://sebastiansauer.github.io/sum-isna/)

also, if NA for your string is represented by '', remember to add `na.strings = ''` when reading it in!

---

seq() is the equivalent of range()

rep(vector, times = 2) repeats the vector by the number of times, entirely. changing times to each repeats each element in the vector several times first, before going to the next element

is.*() checks for type

as.*() converts it to type e.g. as.data.table() but need to import library(data.table)

rev() reverses the order

grepl(patterns = regex, x = string) search for regex in the string, returns logical

grep() returns indices instead

sub() replaces the first match

gsub() replaces all

Sys.Date() returns a date object, today's date
Sys.time() current time
as.Date() to convert

---

learning dplyr: select, mutate, filter, arrange, summarize

names(df) is the equivalent of df.columns
n() gives the length of the df

```R
df_less <- df %>%
    select(df, 1:5, -2) # select columns 1 to 5, excluding 2.

df_lesser <- df %>%
    select(df, year, country, lifeExp:pop) # select columns by name
```

helper functions in select:

starts_with("X"); ends_with("X"); contains("X");<br>
matches("X") "X" can be a regular expression;<br>
num_range("x", 1:5) this gives the variables named x01, x02, x03, x04 and x05;<br>
one_of(x): every name that appears in x, which should be a character vector.<br>

```R
library(dplyr)
df %>% # pipes this result into the first argument in the next step
    filter(year == 1990, country == "United States") # to filter rows
    arrange(desc(year)) # to arrange
    mutate(lifeExpMonths = lifeExp * 12) # to create a new column

by_year_continent <- df %>% # assign the result to a variable
    filter(year <= 1990) %>%
    group_by(continent, year) %>% # group by
    summarize(meanLifeExp = mean(lifeExp), # to summarize
              totalPop = sum(pop))


```

learning ggplot2

```R
library(ggplot2)

# scatterplots
ggplot(df, aes(x = pop, y = gdpPercap, color = continent, size = pop)) + # color and size
  geom_point() + # geom_point is scatterplot
  scale_x_log10() + # puts x on log scale. if y, just use scale_y_log10()
  expand_limits(x=0) # sets x axis to start at 0

# subplots
ggplot(df, aes(x = pop, y = gdpPercap, color = continent, size = pop)) + # color and size
  geom_point() + 
  facet_wrap(~ continent) # creates subplots by continent
```

to plot a line plot, change `geom_point()` to `geom_line()`.

bar plot: `geom_col()`.

histogram: `geom_histogram(binwidth = 5)`

boxplot: `geom_boxplot()`

to add title: `+ ggtitle("Title")`


---

connecting to mysql

# Set up a connection to the mysql database
my_db <- src_mysql(dbname = "dplyr", 
                   host = "courses.xxx.us-east-1.rds.amazonaws.com", 
                   port = 3306, 
                   user = "student",
                   password = "xxx")

# Reference a table within that source: df
df <- tbl(my_db, "dplyr")


---

### linear regression
```R
model <- lm(y ~ x, data = df)
summary(model)
```

### tidying data up
```R
library(broom) # to tidy up the results from summary, turns model into a df
tidy(model)
bind_rows(tidy(model1), tiday(model2)) # combine results into one big df
```

```R
library(tidyr)
nested <- by_year_country %>%
    nest(-country) %>% # nest all except country column, into a data column (is a list). there is a tibble (df) for each country
    unnest(data) # brings it back up to the same level 
```

to retrieve one of the nested data, use indexing: `nested$data[[7]]`

```R
library(purrr)
by_year_country %>%
    nest(-country) %>%
    mutate(models = map(data, ~ lm(percent_yes ~ year, .))) %>% # map applies function to all items in the list. in this case the function is a linear model
    mutate(tidied = map(models,tidy)) %>% # tidies the models
    unnest(tidied) # unnest the tidied models

``` 