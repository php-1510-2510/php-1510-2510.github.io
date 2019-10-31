---
title       : Confidence Intervals - part 2
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




# Continuing Confidence Intervals

--- .class #id

## More Difficult Confidence Intervals 

- Consider the difference in poor mental health among different groups. 
- We can consider the difference between `emtsuprt_bin`. 
- We may wish to know if those with emotional support have less poor mental health days than those without emotional support. 

--- .class #id

## Graphing Relationship

![plot of chunk emt-supt-metnhlth](figure/emt-supt-metnhlth-1.png)

--- .class #id

## What Can we see?

- The median appears to be higher in the group without as much emotional support. 
- It can be hard to tell if there is a difference with the variation between the groups. 
-  We can see a relationship but are unsure if it is due to chance or not. 

--- .class #id

## Hypothesis Testing

- What can be happening? 
    - Emotional Support influences mental health days. 
    - Groups differed at Baseline. 
    - Random Chance


--- .class #id

## Random Chance

- We can test this with the permutations as we did in lecture 13 and on the exam. 
- Let's do this with a permutation test. 

--- .class #id

## Proportions


```r
brfss2 %>% 
    group_by(emtsuprt_bin) %>% 
    summarise(n=n()) %>% 
    mutate(freq=n/sum(n))
```

```
## # A tibble: 2 x 3
##   emtsuprt_bin        n  freq
##   <fct>           <int> <dbl>
## 1 Always/Usually   5555 0.828
## 2 Sometimes-Never  1151 0.172
```

--- .class #id

## The Code




--- .class #id

## The outcome

![plot of chunk emtsuprt-menthlth-diff](figure/emtsuprt-menthlth-diff-1.png)



--- .class #id

## The Difference in the Data


```
## # A tibble: 2 x 2
##   emtsuprt_bin    `mean(menthlth, na.rm = T)`
##   <fct>                                 <dbl>
## 1 Always/Usually                         4.52
## 2 Sometimes-Never                       10.3 
## [1] -5.78
## [1] 0
```



--- .class #id

## What do we see? 

- If we randomly assign people to emotional support or no emotional support, the average difference in poor mental health days is centered around 0 days. 
- However, we saw that those with support have on average $\approx 6$ days. 
- What other ways could we consider this? 


--- .class #id

## In Comes Confidence Intervals

- Let's create our confidence intervals. 


```r
brfss2 %>%
    group_by(emtsuprt_bin) %>%
    summarise(mean(menthlth, na.rm=T), sd(menthlth, na.rm=T))
```

```
## # A tibble: 2 x 3
##   emtsuprt_bin    `mean(menthlth, na.rm = T)` `sd(menthlth, na.rm = T)`
##   <fct>                                 <dbl>                     <dbl>
## 1 Always/Usually                         4.52                      7.71
## 2 Sometimes-Never                       10.3                      11.1
```

--- .class #id

## What can we do? 

- Our normal way of doing this gives us 2 means and 2 standard deviations. 
- We do not know how to handle this. 
- We could think of the mean of the differences but we cannot find the SD of this. 
- Why cant we find the SD?

--- .class #id

## Bootstrapping Saves the Day

- Create 10000 bootstraps


```r
set.seed(123)
bt_data <- bootstraps(brfss3, times = 10000)
```

```
## Error in bootstraps(brfss3, times = 10000): could not find function "bootstraps"
```

```r
bt_data
```

```
## Error in eval(expr, envir, enclos): object 'bt_data' not found
```



--- .class #id

## Bootstraps 


```r
bt_data$splits[[2]]
```

```
## Error in eval(expr, envir, enclos): object 'bt_data' not found
```

- 6585 samples in each split. 
- 2390 original observations in the second split. 
- 6585 in the original data. 

--- .class #id

## The Function


```r
get_diff <- function(split){
## get a bootstrap
split_data <- analysis(split)
## Calculate the sample mean value
yes = split_data$emtsuprt_bin=="Always/Usually"
no = split_data$emtsuprt_bin=="Sometimes-Never"
#mean_yes <- mean(split_data$menthlth[split_data$emtsuprt_bin=="Always/Usually",], na.rm=TRUE)
#mean_no <- mean(split_data$menthlth[,], na.rm=TRUE)

#diff = mean_yes - mean_no
                    
return(sum(yes) + sum(no))
}
```


--- .class #id

## Differences


```r
bt_data <- bt_data %>%
    mutate(bt_means = map_dbl(bt_data$splits, get_diff))
```

```
## Error in eval(lhs, parent, parent): object 'bt_data' not found
```

```r
bt_data
```

```
## Error in eval(expr, envir, enclos): object 'bt_data' not found
```
