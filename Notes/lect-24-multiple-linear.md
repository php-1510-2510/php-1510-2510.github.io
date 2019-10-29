---
title       :  Multiple Linear Regression
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






# Multiple Linear Regression 



--- .class #id




## Multiple Regression

- We have been discussing simple models so far. 
- This works well when you have:
    - Randomized Data to test between specific groups (Treatment vs Control)
- In most situations we need look at more than just one relationship. 
- Think of this as needing more information to tell the entire story. 

--- .class #id



## Multiple Linear Regression with appearances



- First start with univariate models
- Then perform the multiple model





--- .class #id

## Multivariate Models



```r
mod3 <- lm(appearances~publisher + year, data=comic_characters)
tidy3 <- tidy(mod3, conf.int=T)[,-c(3:4)]
tidy3
```

```
## # A tibble: 3 x 5
##   term             estimate  p.value  conf.low conf.high
##   <chr>               <dbl>    <dbl>     <dbl>     <dbl>
## 1 (Intercept)      1265.    9.81e-78  1133.     1398.   
## 2 publisherMarvel    -9.54  1.24e-11   -12.3      -6.78 
## 3 year               -0.624 5.93e-75    -0.690    -0.557
```

--- .class #id

## Interpreting Multiple Coefficients

- The intercept is when all coefficients are zero. 
- Each other coefficient is interpreted in context to another. 


--- .class #id

## Interpreting Multiple Coefficients: Our Example

- Intercept: DC average appearances at year 0. ***Not Meaningful***
- Publisher Coefficient: If we consider 2 characters in the same year, the character from Marvel will have on average 9.54 less appearances than the character from DC. 
- Year Coefficient: If we consider 2 characters from the same publisher, an increase in 1 year will lead to on average 0.62 less appearances. 



--- .segue bg:grey


# Multiple Regression


--- .class #id

## Multiple Regression

- We have been discussing simple models so far. 
- This works well when you have:
    - Randomized Data to test between specific groups (Treatment vs Control)
- In most situations we need look at more than just one relationship. 
- Think of this as needing more information to tell the entire story. 


--- .class #id


## Motivating Example

- Health disparities are very real and exist across individuals and populations. 
- Before developing methods of remedying these disparities we need to be able to identify where there are disparities.In this homework we will consider a study by [(Asch & Armstrong, 2007)](http://www.ncbi.nlm.nih.gov/pubmed/17513818).  
- This paper considers 222 patients with localized prostate cancer. 


--- .class #id



## Motivating Example 

- The table below partitions patients by race, hospital and whether or not the patient received a prostatectomy. 

|       |   Race | Prostatectomy | No Prostatectomy | 
| -------------- | -------- | ---------- | ------------ |
University Hospital | White | 54 | 37 | 
|  | Black | 7 | 5  |
| VA Hospital | White | 11 | 29 | 
| | Black | 22 | 57 | 



--- .class #id






## Loading the Data


You can load this data into R with the code below:



```r
phil_disp <- read.table("https://drive.google.com/uc?export=download&id=0B8CsRLdwqzbzOXlIRl9VcjNJRFU", header=TRUE, sep=",")
```


--- .class #id



## The Data 

This dataset contains the following variables: 


| Variable |       Description | 
| ----------- | -------------------- | 
| hospital  |     0 - University Hospital |
|           |     1 - VA Hospital | 
| race   |      0 - White |
|      |        1 - Black | 
| surgery |       0 - No prostatectomy |
|          |    1 - Had Prostatectomy | 
| number    |    Count of people in Category |





--- .class #id

## Consider Prostatectomy by Race


```r
library(broom)
prost_race <- glm(surgery ~ race, weight=number, data= phil_disp,
                  family="binomial")
tidy(prost_race, exponentiate=T, conf.int=T)[,-c(3:4)]
```

```
## # A tibble: 2 x 5
##   term        estimate p.value conf.low conf.high
##   <chr>          <dbl>   <dbl>    <dbl>     <dbl>
## 1 (Intercept)    0.985 0.930      0.699     1.39 
## 2 race           0.475 0.00895    0.269     0.825
```



--- .class #id



## Consider Prostatectomy by Race

- What can we conclude? 
- What kind of policy might we want to invoke based on this discovery?



--- .class #id


## Consider Prostatectomy by Hospital


```r
prost_hosp <- glm(surgery ~ hospital, weight=number, data= phil_disp,
                  family="binomial")
tidy(prost_hosp, exponentiate =T, conf.int=T)[,-c(3:4)]
```

```
## # A tibble: 2 x 5
##   term        estimate    p.value conf.low conf.high
##   <chr>          <dbl>      <dbl>    <dbl>     <dbl>
## 1 (Intercept)    1.45  0.0627        0.984     2.16 
## 2 hospital       0.264 0.00000341    0.149     0.460
```


--- .class #id


## Consider Prostatectomy by Hospital

- What can we conclude? 


--- .class #id

## Multiple Regression of Prostatectomy



```r
prost <- glm(surgery ~ hospital + race, weight=number, data= phil_disp,
             family="binomial")
tidy(prost, exponentiate=T, conf.int=T)[,-c(3:4)]
```

```
## # A tibble: 3 x 5
##   term        estimate  p.value conf.low conf.high
##   <chr>          <dbl>    <dbl>    <dbl>     <dbl>
## 1 (Intercept)    1.45  0.0682      0.976     2.18 
## 2 hospital       0.264 0.000124    0.131     0.515
## 3 race           0.998 0.996       0.501     2.04
```




--- .class #id


## Multiple Regression of Prostatectomy

- What can We conclude?
- What happened here?
- Does this change our policy suggestion from before?




--- .class #id


## Benefits of Multiple Regression


- Multiple Regression helps us tell a more complete story. 
- Multiple regression controls for confounding. 




--- .class #id


## Confounding

- Associated with both the Exposure and the Outcome
- Even if the Exposure and Outcome are not related, unmeasured confounding can show that they are. 



--- .class #id



## What Do We Do with Confounding?

- We must add all confounders into our model. 
- Without adjusting for confounders are results may be highly biased. 
- Without adjusting for confounding we may make incorrect policies that do not fix the problem. 



--- .class #id


## Multiple Linear Regression with appearances



- First start with univariate models
- Then perform the multiple model






--- .class #id


## Multivariate Models



```r
library(broom)
library(fivethirtyeight)
mod3 <- lm(appearances~publisher + year, data=comic_characters)
tidy3 <- tidy(mod3, conf.int=T)[,-c(3:4)]
tidy3
```

```
## # A tibble: 3 x 5
##   term             estimate  p.value  conf.low conf.high
##   <chr>               <dbl>    <dbl>     <dbl>     <dbl>
## 1 (Intercept)      1265.    9.81e-78  1133.     1398.   
## 2 publisherMarvel    -9.54  1.24e-11   -12.3      -6.78 
## 3 year               -0.624 5.93e-75    -0.690    -0.557
```



--- .class #id

## Interpreting Multiple Coefficients

- The intercept is when all coefficients are zero. 
- Each other coefficient is interpreted in context to another. 




--- .class #id


## Interpreting Multiple Coefficients: Our Example

- Intercept: DC average appearances at year 0. ***Not Meaningful***
- Publisher Coefficient: If we consider 2 characters in the same year, the character from Marvel will have on average 9.54 less appearances than the character from DC. 
- Year Coefficient: If we consider 2 characters from the same publisher, an increase in 1 year will lead to on average 0.62 less appearances. 



--- .class #id

## Further Example: Bike Sharing Data


- We have hourly data spanning 2 years
- This dataset has the first 19 days of each month. 
- Goal is to find the total count of bike rented. 


--- .class #id


## Further Example: Bike Sharing Data

| Data |  Fields |
| :------: | :----------------------|
| datetime | hourly date + timestamp  |
| season |   1 = spring, 2 = summer, 3 = fall, 4 = winter  |
| holiday |  whether the day is considered a holiday |
| workingday |  whether the day is neither a weekend nor holiday |


--- .class #id


## Further Example: Bike Sharing Data

| Data |  Fields |
| :------: | :----------------------|
| weather |  1: Clear, Few clouds, Partly cloudy, Partly cloudy  |
| | 2: Mist + Cloudy, Mist + Broken clouds, Mist + Few clouds, Mist | 
| | 3: Light Snow, Light Rain + Thunderstorm + Scattered clouds, Light Rain + Scattered clouds |
| | 4: Heavy Rain + Ice Pallets + Thunderstorm + Mist, Snow + Fog |
| temp |  temperature in Celsius |


--- .class #id


## Further Example: Bike Sharing Data

| Data |  Fields |
| :------: | :----------------------|
| atemp |  "feels like" temperature in Celsius |
| humidity |  relative humidity |
| windspeed |  wind speed|
| casual |  number of non-registered user rentals initiated |
| registered |  number of registered user rentals initiated |
| count |  number of total rentals |


--- .class #id

## Univariate Regressions


```r
library(readr)
library(tidyverse)
bikes <- read_csv("../Notes/Data/bike_sharing.csv") %>%
        mutate(season = as.factor(season)) %>%
        mutate(weather=as.factor(weather)) 
```


--- .class #id

## Univariate Regressions


```r
mod1 <- lm(count~season, data=bikes)
mod2 <- lm(count~holiday, data=bikes)
mod3 <- lm(count~workingday, data=bikes)
mod4 <- lm(count~weather, data=bikes)
mod5 <- lm(count~temp, data=bikes)
mod6 <- lm(count~atemp, data=bikes)
mod7 <- lm(count~humidity, data=bikes)
mod8 <- lm(count~windspeed, data=bikes)
mod9 <- lm(count~casual, data=bikes)
mod10 <- lm(count~registered, data=bikes)
```


--- .class #id

## Univariate Regressions


```r
library(broom)
tidy1 <- tidy( mod1, conf.int=T )[-1, -c(3:4) ]
tidy2 <- tidy(mod2, conf.int=T )[-1, -c(3:4) ]
tidy3 <- tidy(mod3 , conf.int=T)[-1, -c(3:4) ]
tidy4 <- tidy(mod4 , conf.int=T)[-1, -c(3:4) ]
tidy5 <- tidy(mod5, conf.int=T)[-1, -c(3:4) ]
tidy6 <- tidy(mod6 , conf.int=T)[-1, -c(3:4) ]
tidy7 <- tidy(mod7 , conf.int=T)[-1, -c(3:4) ]
tidy8 <- tidy(mod8 , conf.int=T)[-1, -c(3:4) ]
tidy9 <- tidy(mod9, conf.int=T)[-1, -c(3:4) ]
tidy10 <- tidy(mod10, conf.int=T)[-1, -c(3:4) ]
bind_rows(tidy1, tidy2, tidy3, tidy4, tidy5, tidy6, tidy7, tidy8, tidy9, tidy10) 
```



--- .class #id

## Univariate Regressions


```
## # A tibble: 14 x 5
##    term       estimate   p.value conf.low conf.high
##    <chr>         <dbl>     <dbl>    <dbl>     <dbl>
##  1 season2       98.9  9.76e- 94    89.6     108.  
##  2 season3      118.   1.06e-131   109.      127.  
##  3 season4       82.6  2.13e- 66    73.3      92.0 
##  4 holiday       -5.86 5.74e-  1   -26.3      14.6 
##  5 workingday     4.51 2.26e-  1    -2.80     11.8 
##  6 weather2     -26.3  4.32e- 11   -34.1     -18.5 
##  7 weather3     -86.4  3.29e- 40   -99.1     -73.7 
##  8 weather4     -41.2  8.18e-  1  -393.      311.  
##  9 temp           9.17 0.            8.77      9.57
## 10 atemp          8.33 0.            7.96      8.70
## 11 humidity      -2.99 2.92e-253    -3.15     -2.82
## 12 windspeed      2.25 2.90e- 26     1.83      2.66
## 13 casual         2.50 0.            2.45      2.55
## 14 registered     1.16 0.            1.16      1.17
```


--- .class #id


## Multivariate


```r
mod.final <- lm(count~season+weather+humidity+windspeed, data=bikes)
tidy(mod.final)[-1,-c(3:4)]
glance(mod.final)
```

--- .class #id

## Multivariate


```
## # A tibble: 8 x 3
##   term      estimate   p.value
##   <chr>        <dbl>     <dbl>
## 1 season2    116.    1.40e-145
## 2 season3    148.    7.52e-227
## 3 season4    118.    1.74e-147
## 4 weather2    20.0   1.38e-  7
## 5 weather3     0.124 9.84e-  1
## 6 weather4   162.    3.19e-  1
## 7 humidity    -3.49  3.86e-273
## 8 windspeed    0.633 2.05e-  3
```

--- .class #id


## Multivariate


```
## # A tibble: 1 x 11
##   r.squared adj.r.squared sigma statistic p.value    df  logLik    AIC
##       <dbl>         <dbl> <dbl>     <dbl>   <dbl> <int>   <dbl>  <dbl>
## 1     0.195         0.194  163.      329.       0     9 -70865. 1.42e5
## # ... with 3 more variables: BIC <dbl>, deviance <dbl>, df.residual <int>
```

--- .class #id

## Multiple Variables



```r
library(readr)
library(tidyverse)
bikes <- read_csv("../Notes/Data/bike_sharing.csv") %>%
        mutate(season = as.factor(season)) %>%
        mutate(weather=as.factor(weather)) 
```


--- .class #id

## Multiple Variables


```r
mod.final <- lm(count~season+weather+humidity+windspeed, data=bikes)
tidy(mod.final)[-1,-c(3:4)]
glance(mod.final)
```

--- .class #id


## Multiple Variables


|term      |    estimate|   p.value|
|:---------|-----------:|---------:|
|season2   | 115.8007186| 0.0000000|
|season3   | 148.3532069| 0.0000000|
|season4   | 118.4943844| 0.0000000|
|weather2  |  19.9875113| 0.0000001|
|weather3  |   0.1237865| 0.9844830|
|weather4  | 162.2596870| 0.3185115|
|humidity  |  -3.4929513| 0.0000000|
|windspeed |   0.6328680| 0.0020498|
