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

