---
title       : Summarization of Data
author      : Adam J Sullivan 
job         : Assistant Professor of Biostatistics
work        : Brown University
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js # {highlight.js, prettify, highlight}
hitheme     :  github     # 
widgets     : [mathjax, quiz, bootstrap, interactive] # {mathjax, quiz, bootstrap}
ext_widgets : {rCharts: [libraries/nvd3, libraries/leaflet, libraries/dygraphs]}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
logo        : publichealthlogo.png
biglogo     : publichealthlogo.png
assets      : {assets: ../../assets}
---  .segue bg:grey







# Summarizing Data

--- .class #id


## Piping or Chaining

- We will discuss a concept that will help us greatly when it comes to working with our data. 
- The usual way to perform multiple operations in one line is by nesting. 

--- .class #id

## Piping or Chaining

To consider an example we will look at the data provided in the gapminder package:


```r
library(gapminder)
head(gapminder)
```

```
## # A tibble: 6 x 6
##   country     continent  year lifeExp      pop gdpPercap
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Afghanistan Asia       1952    28.8  8425333      779.
## 2 Afghanistan Asia       1957    30.3  9240934      821.
## 3 Afghanistan Asia       1962    32.0 10267083      853.
## 4 Afghanistan Asia       1967    34.0 11537966      836.
## 5 Afghanistan Asia       1972    36.1 13079460      740.
## 6 Afghanistan Asia       1977    38.4 14880372      786.
```

--- .class #id

## Nesting vs Chaining

- Let's say that we want to have the GDP per capita and life expectancy Kenya.
- Traditionally speaking we could do this in a nested manner:



```r
filter(select(gapminder, country, lifeExp, gdpPercap), country=="Kenya")
```

--- .class #id

## Nesting vs Chaining

- It is not easy to see exactly what this code was doing but we can write this in a manner that follows our logic much better. 
- The code below represents how to do this with chaining. 


```r
gapminder %>%
    select(country, lifeExp, gdpPercap) %>%
    filter(country=="Kenya")
```

--- .class #id

## Breaking Down the Code

- We now have something that is much clearer to read.
- Here is what our chaining command says:

1. Take the `gapminder` data
2. Select the variables: `country`, `lifeExp` and `gdpPercap`.
3. Only keep information from Kenya. 

- The nested code says the same thing but it is hard to see what is going on if you have not been coding for very long.


--- .class #id

## Breaking Down the Code

- The result of this search is below: 


```
# A tibble: 12 x 3
   country lifeExp gdpPercap
    <fctr>   <dbl>     <dbl>
 1   Kenya  42.270  853.5409
 2   Kenya  44.686  944.4383
 3   Kenya  47.949  896.9664
 4   Kenya  50.654 1056.7365
 5   Kenya  53.559 1222.3600
 6   Kenya  56.155 1267.6132
 7   Kenya  58.766 1348.2258
 8   Kenya  59.339 1361.9369
 9   Kenya  59.285 1341.9217
10   Kenya  54.407 1360.4850
11   Kenya  50.992 1287.5147
12   Kenya  54.110 1463.2493
```

--- .class #id

## What is `%>%`

- In the previous code we saw that we used `%>%` in the command you can think of this as saying ***then***.
- For example:


```r
gapminder %>%
    select(country, lifeExp, gdpPercap) %>%
    filter(country=="Kenya")
```

--- .class #id

## What Does this Mean?

- This translates to:
    - Take Gapminder ***then*** select these columns select(country, lifeExp, gdpPercap) ***then*** filter out so we only keep Kenya


--- .class #id

## Why Chain?

- We still might ask why we would want to do this. 
- Chaining increases readability significantly when there are many commands. 
- With many packages we can replace the need to perform nested arguments. 
- The chaining operator is automatically imported from the [magrittr](https://github.com/smbache/magrittr) package. 

--- .class #id

## User Defined Function


- Let's say that we wish to find the Euclidean distance between two vectors say, `x1` and `x2`. 
- We could use the math formula:

\[\sqrt{sum(x1-x2)^2}\]

--- .class #id

## User Defined Function

- In the nested manner this would be:


```r
x1 <- 1:5; x2 <- 2:6
sqrt(sum((x1-x2)^2))
```

--- .class #id

## User Defined Function


- However, if we chain this we can see how we would perform this mathematically. 

```r
# chaining method
(x1-x2)^2 %>% sum() %>% sqrt()
```
- If we did it by hand we would perform elementwise subtraction of `x2` from `x1` ***then*** we would sum those elementwise values ***then*** we would take the square root of the sum. 


--- .class #id

## User Defined Function



```r
# chaining method
(x1-x2)^2 %>% sum() %>% sqrt()
```

```
## [1] 2.236068
```

- Many of us have been performing calculations by this type of method for years, so that chaining really is more natural for us. 



--- .class #id

## Summarizing Data

- As you have seen in your own work, being able to summarize information is crucial. 
- We need to be able to take out data and summarize it as well. 
- We will consider doing this using the `summarise()` function. 

--- .class #id

## Summarizing Data

- We will begin with a simple summary:
    1. Create a table grouped by `gender`.
    2. Summarize each group by taking mean of `appearances`.

--- .class #id

## Renaming Variable

- First we need to correct the data and rename the variable `sex` to `gender`. 


```r
library(fivethirtyeight)
comic_characters <- comic_characters %>%
                      mutate(gender=sex)
```


--- .class #id

## Summarizing Data Example 



```r
comic_characters %>%
    group_by(gender) %>%
    summarise(avg_appear = mean(appearances, na.rm=TRUE))
```



--- .class #id

## Summarizing Data Example 



```
## # A tibble: 7 x 2
##   gender                 avg_appear
##   <chr>                       <dbl>
## 1 Agender Characters          19.7 
## 2 Female Characters           21.0 
## 3 Genderfluid Characters     282.  
## 4 Genderless Characters       12.8 
## 5 Male Characters             19.0 
## 6 Transgender Characters       4   
## 7 <NA>                         5.13
```

--- .class #id


## Another Example

- Lets say that we would like to have more than just the averages but we wish to have the minimum and the maximum year of entry by gender:


```r
comic_characters %>%
    group_by(gender) %>%
    summarise_each(funs(min(., na.rm=TRUE), max(., na.rm=TRUE)), year)
```


--- .class #id


## Another Example




```
## # A tibble: 7 x 3
##   gender                   min   max
##   <chr>                  <dbl> <dbl>
## 1 Agender Characters      1964  2013
## 2 Female Characters       1936  2013
## 3 Genderfluid Characters  1949  2005
## 4 Genderless Characters   1961  2007
## 5 Male Characters         1935  2013
## 6 Transgender Characters  2009  2009
## 7 <NA>                    1939  2013
```

--- .class #id

## On Your Own: RStudio Practice 

- The following is a new function:
  - Helper function `n()` counts the number of rows in a group
- Then for each year and continent:
  - count total lifeExp
  - Sort in descending order. 

--- .class #id

## On Your Own: RStudio Practice 

Your answer should look like:


```
## # A tibble: 60 x 3
## # Groups:   continent [5]
##    continent  year lifeExp_count
##    <fct>     <int>         <int>
##  1 Africa     1952            52
##  2 Africa     1957            52
##  3 Africa     1962            52
##  4 Africa     1967            52
##  5 Africa     1972            52
##  6 Africa     1977            52
##  7 Africa     1982            52
##  8 Africa     1987            52
##  9 Africa     1992            52
## 10 Africa     1997            52
## # ... with 50 more rows
```


We could also have used what is called the  `tally()` function:


```r
gapminder %>%
    group_by(country, year) %>%
    tally(sort = TRUE)
```

```
## # A tibble: 1,704 x 3
## # Groups:   country [142]
##    country      year     n
##    <fct>       <int> <int>
##  1 Afghanistan  1952     1
##  2 Afghanistan  1957     1
##  3 Afghanistan  1962     1
##  4 Afghanistan  1967     1
##  5 Afghanistan  1972     1
##  6 Afghanistan  1977     1
##  7 Afghanistan  1982     1
##  8 Afghanistan  1987     1
##  9 Afghanistan  1992     1
## 10 Afghanistan  1997     1
## # ... with 1,694 more rows
```

--- .class #id

## Further Summaries

- We have so far discussed how one could find the basic number summaries:
  - mean
  - median
  - standard deviation
  - variance
  - minimum 
  - maximum
- However there are many more operations that you may wish to do for summarizing data. 
- In fact many of the following examples are excellent choices for working with categorical data which does not always make sense to do the above summaries for. 


--- .class #id

## Further Summaries

- We will consider:
  1. Grouping and Counting
  2. Grouping, Counting and Sorting
  3. Other Groupings
  4. Counting Groups


--- .class #id



## Grouping and Counting


- We have seen the functions `tally()` and `count()`. 
- Both of these can be used for grouping and counting. 
- They also are very concise in how they are called. 


--- .class #id



## Grouping and Counting


- For example if we wished to know how many characters there were by year, we would use `tally()` in this manner: 


```r
comic_characters %>%
  group_by(year) %>% 
  tally()
```

--- .class #id



## Grouping and Counting

- Where as we could do the same thing with `count()`


```r
comic_characters %>% 
  count(year)
```

*Notice: `count()` allowed for month to be called inside of it, removing the need for the `group_by()` function. 


--- .class #id



## Grouping, counting and sorting.

- Both `tally()` and `count()` have an argument called `sort()`. 
- This allows you to go one step further and group by, count and sort at the same time. 
- For `tally()` this would be:


```r
comic_characters %>% 
    group_by(year) %>% 
    tally(sort=TRUE)
```

--- .class #id

## Grouping, counting and sorting: `tally()`


```
## # A tibble: 80 x 2
##     year     n
##    <int> <int>
##  1    NA   884
##  2  1993   763
##  3  1994   715
##  4  2006   684
##  5  1992   633
##  6  2010   603
##  7  1988   590
##  8  1989   587
##  9  2008   571
## 10  1990   532
## # ... with 70 more rows
```




--- .class #id

## Grouping with other functions

- We can also sum over other values rather than just counting the rows like the above examples. 
- For example let us say we were interested in knowing the total appearances for characters in a given gender and publisher.
- We could do this with the `summarise()` function, `tally()` function or the `count()` function:


```r
comic_characters %>% 
  group_by(gender, publisher) %>% 
  summarise(total_app = sum(appearances))
```


--- .class #id

## Grouping with other functions



```
## # A tibble: 10 x 3
## # Groups:   gender [?]
##    gender                 publisher total_app
##    <chr>                  <chr>         <int>
##  1 Agender Characters     Marvel           NA
##  2 Female Characters      DC               NA
##  3 Female Characters      Marvel           NA
##  4 Genderfluid Characters Marvel          565
##  5 Genderless Characters  DC               NA
##  6 Male Characters        DC               NA
##  7 Male Characters        Marvel           NA
##  8 Transgender Characters DC                4
##  9 <NA>                   DC               NA
## 10 <NA>                   Marvel           NA
```


--- .class #id



--- .class #id

## Grouping with other functions

- For  `tally()` we could do:


```r
comic_characters %>% 
  group_by(gender, publisher) %>% 
  tally(wt = appearances)
```

*Note: in `tally()` the `wt` stands for weight and allows you to weight the sum based on the appearances*. 

--- .class #id

## Grouping with other functions

- With the `count()` function we also use `wt`:



```r
comic_characters %>% count(gender, publisher, wt = appearances)
```

```
## # A tibble: 10 x 3
##    gender                 publisher      n
##    <chr>                  <chr>      <int>
##  1 Agender Characters     Marvel       826
##  2 Female Characters      DC         42271
##  3 Female Characters      Marvel     73005
##  4 Genderfluid Characters Marvel       565
##  5 Genderless Characters  DC           244
##  6 Male Characters        DC        110911
##  7 Male Characters        Marvel    182601
##  8 Transgender Characters DC             4
##  9 <NA>                   DC          1102
## 10 <NA>                   Marvel      3273
```

--- .class #id

## Counting Groups


- We may want to know how large our groups are. To do this we can use the following functions:
  - `group_size()` is a function that returns counts of group. 
  - `n_groups()` returns the number of groups

--- .class #id

## Counting Groups

- So if wanted to count the number of characters by hair color, we could group by hair color and find the groups size using `group_size()`:


```r
comic_characters %>% 
  group_by(hair) %>% 
  group_size()
```


--- .class #id

## Counting Groups




```
##  [1]   78  838 5329 2326   97    1 3487    1   13  159  688    6    5 1176
## [15]    3   64   42    2   79 1081    6    3   19   75   32    4 1100   20
## [29] 6538
```


--- .class #id

## Counting Groups


- If we just wished to know how many years were represented in our data we could use the `n_groups()` function:



```r
comic_characters %>% 
  group_by(year) %>% 
  n_groups()
```



--- .class #id

## Counting Groups


```
## [1] 80
```

