---
title       : Confidence Intervals
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




# Confidence Intervals


--- .class #id

## Advantages

- Very simple once you get used to it. 
- Straight forward and does not require extra data. 
- Works with extremely complex things where you may not understand the math enough to find a margin of error. 
- Naturally checks for stability. 
- Many times more accurate than a normal distribution assumption. 

--- .class #id

## Disadvantages

- Works well with small data but does not have any guarantees with small data. 
- Hidden assumptions:
    - independence of samples. 
    - Population is large enough that the sample effect is not too big. 
    - Others depending on what statistic you are bootstrapping. 
    


--- .class #id

## What does it do?

- Basically, bootstrapping treats the data as a population. 
- The we repeatedly draw independent samples to create *bootstrapped* datasets. 
- We sample with replacement, allowing observations to be sampled more than once. 



--- .class #id



![](bootstrap.png)



--- .class #id

## How do we do this?

- Each bootstrap data set $$Z^{*1}, Z^{*2}, \dots, Z^{*B}$$ contains *n* observations, sampled with replacement from the original data set.  
- Each bootstrap is used to compute the estimated statistic we are interested in $\hat\alpha^*$. 


--- .class #id

## Then what?

- We can then use all the bootstrapped data sets to compute the standard error of $$\hat\alpha^{*1}, \hat\alpha^{*2}, \dots, \hat\alpha^{*B}$$ desired statistic as
$$ SE_B(\hat\alpha) = \sqrt{\frac{1}{B-1}\sum^B_{r=1}\bigg(\hat\alpha^{*r}-\frac{1}{B}\sum^B_{r'=1}\hat\alpha^{*r'}\bigg)^2}  \tag{4} $$
- $$SE_B(\hat\alpha)$$ serves as an estimate of the standard error of $$\hat\alpha$$ estimated from the original data set. 

--- .class #id

## Example 1: Estimating the accuracy of a single statistic

- Performing a bootstrap analysis in R entails two steps:
    1. Create a function that computes the statistic of interest.
    2. Use the `boot` function from the [`boot`](http://cran.r-project.org/web/packages/boot/index.html) package to perform the boostrapping

--- .class #id


## The Data

- This data comes from out midterm. 
- We were considering whether or not the number of poor mental health days in the past 30 days was different for those apart of the transgender experience. 
- We first might be interested in understanding the average number of days for both groups. 



--- .class #id


## Doing this with tidyverse

- You will need the following packages:
   

```r
library(boot)
library(ggplot2)
library(dplyr)
load("Data/brfss.rda")
```

--- .class #id

## The Data


```r
brfss3 <- brfss2 %>%
      filter(trnsgndr_bin=="no") %>%
      select(menthlth)
```

--- .class #id

## Creating our function

- Lets say we wish to have 10000 bootstraps of the data. 


```r
get_mean <- function(data, indices){
  d <- data[indices,]
  return(mean(d, na.rm=TRUE))
}
```


--- .class #id

## Generating Bootstraps

- Now we can create the bootstraps
- We will do 1000 bootstraps










--- .class #id

## Confidence Intervals


```r
boot.ci(results, type="basic")
```

```
## BOOTSTRAP CONFIDENCE INTERVAL CALCULATIONS
## Based on 1000 bootstrap replicates
## 
## CALL : 
## boot.ci(boot.out = results, type = "basic")
## 
## Intervals : 
## Level      Basic         
## 95%   ( 5.295,  5.720 )  
## Calculations and Intervals on Original Scale
```

--- .class #id

## What about traditional ways?


```r
brfss3 %>% 
    summarise(mean(menthlth, na.rm=T), sd(menthlth, na.rm=T))
 qt(0.975, df=6662)

5.498013 - 1.96032 * 8.639915
5.498013 + 1.96032 * 8.639915
```

```
##   mean(menthlth, na.rm = T) sd(menthlth, na.rm = T)
## 1                  5.498013                8.639915
## [1] 1.96032
## [1] -11.43899
## [1] 22.43501
```


--- .class #id

## What do we notice?

- Big difference in confidence intervals. 
- Value of t is the same as the normal. 


--- .class #id

## Graph of this


![plot of chunk boostrap-ci](figure/boostrap-ci-1.png)
