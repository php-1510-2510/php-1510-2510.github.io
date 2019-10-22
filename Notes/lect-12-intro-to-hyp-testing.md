---
title       : Introduction to Hypothesis Testing
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






# Introduction to Hypothesis Testing

--- .class #id

## Introduction to Hypothesis Testing






- In most statistics courses you get to hypothesis testing at the end of the course. 
- We will do things a little different. 
- We should consider important concepts when trying to understand testing hypothesis. 

--- .class #id

## What is hypothesis Testing? 

- Many times we want to be able to compare things. 
- We wish to test whether something is better than something else. 
    - Should you buy computer "A" vs computer "B". 
- We also wish to test the validity of statements. 
    - A friend says they can force a coin to get Heads.
    - You give them a coin and they flip three times in a row and get heads. 
    - Do you believe them?

--- .class #id

## Coin Toss


```r
coin <- c(1, 0)

flips <- replicate(100000, sum(sample(coin, 3, replace=T)))
tab <- table(flips)
prop.table(tab)
```

```
## flips
##       0       1       2       3 
## 0.12352 0.37410 0.37730 0.12508
```


--- .class #id

## Testing Our Friend

- The probability of 3 heads in 3 coin flips is 12.5%
- We may not believe that they can force heads. 
- Many times our hypothesis testing is about different groups. 


--- .class #id

## Big Question

- Does eating organic food improve your health? 
- Consider some evidence

--- .class #id

## Data Study 1

- This data comes from [NHANES: National Health and Nutrition Examination Survey](https://www.cdc.gov/nchs/nhanes/index.htm)
- Large national random sample
- 2009 – 2010 data
- n = 3777

--- .class #id

## Organic Food

"In the past 30 days, did you buy any food that had the word 'organic' on the package?"

![plot of chunk organic](figure/organic-1.png)


--- .class #id

## Current Health Status

"Would you say your health in general is Excellent, Very Good, Good, Fair or Poor"

![plot of chunk health-all](figure/health-all-1.png)


--- .class #id

## Current Health Status

![plot of chunk health](figure/health-1.png)


--- .class #id

## Health by Organic

![plot of chunk health-organic](figure/health-organic-1.png)



--- .class #id

## The Difference

- We could consider the difference between healthy with organic food vs healthy with conventional food. 


```
## # A tibble: 4 x 4
## # Groups:   organic [2]
##   organic health.bin              n  freq
##   <fct>   <fct>               <int> <dbl>
## 1 Yes     Very Good/Excellent   555 0.483
## 2 Yes     Poor/Fair/Good        593 0.517
## 3 No      Very Good/Excellent   562 0.369
## 4 No      Poor/Fair/Good        961 0.631
```

```
## [1] 0.114
```

--- .class #id

## Evaluating Evidence

- In this data, people who bought organic are healthier. 
- Possible Explanations
    - Eating organic improves health.
    - Groups Differed at Baseline.
    - <del>Random Chance.</del> 

--- .class #id

## Evaluating Evidence

- In this data, people who bout organic are healthier. 
- Possible Explanations
    - Eating organic improves health.
    - Groups Differed at Baseline.
    - <del>Random Chance.</del> 
- P-value: <0.00000000000002 (Will Explain Later)

--- .class #id

## People Who Buy Organic Make More Money

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-1.png)



--- .class #id


## People Make More are Healthier

![plot of chunk rich-box](figure/rich-box-1.png)

--- .class #id


## People Make More are Healthier



![plot of chunk rich-line](figure/rich-line-1.png)



--- .class #id

## People Make More are Healthier


![plot of chunk top-bottom](figure/top-bottom-1.png)


--- .class #id


## Evaluating Evidence

- In this data, people who bout organic are healthier. 
- Possible Explanations
    - Eating organic improves health.
    - Groups Differed at Baseline.
    - <del>Random Chance.</del> 
- **We cannot make a decision here with their being more than one option**

--- .class # id

## Causal Claims

- We cannot make causal claims with groups that are not comparable at baseline. 
- We can look within similar groups to compare though. 


--- .class #id

## Line Plots by Organic vs Conventional

![plot of chunk line-organ-conv](figure/line-organ-conv-1.png)


--- .class #id

## Evidence Enough?

- Does this satisfy our thoughts? 




--- .class #id

## Health by Smoking

- Poople who buy organic are less likely to smoke

![plot of chunk health-smoke](figure/health-smoke-1.png)

--- .class #id

## Health by Veggies

![plot of chunk health-veggies](figure/health-veggies-1.png)

--- .class #id

## Conclusions

- Cannot make comparisons on non-comparable groups. 
- What can happen? 
     - Baseline difference shifts effect. 
     - Baseline difference reverses effect. 
     - Baseline difference masks true effect. 
     - Baseline difference creates false effect. 


--- .class #id

## Still Not Convinced?

- Let's simulate organic based on income only. 
- Then organc food choice is only associated with income and not with health directly. 

--- .class #id

## Simulation





--- .class #id 


## Health by Organic Sim

- Difference exists even though we made up organic food purchases. 

![plot of chunk health-organic-sim](figure/health-organic-sim-1.png)


--- .class #id

## Simulated Line Plots by Organic vs Conventional

![plot of chunk line-organsim-conv](figure/line-organsim-conv-1.png)


--- .class #id

## What do we do? 

- Ideally We randomize at baseline so that we can make causal claims. 
- If not, there are methods to determine causal inference but they are much more difficult. 


--- .class #id

## Data Problem #2: Fruit Flies

- Fruit flies **randomly** divided into two groups of 500 each
- One group fed organic food, the other conventional food
- Measured longevity, fertility, stress resistance, activity
-  Study done by a 16-year-old girl for a science project!
- [Organically Grown Food Provides Health Benefits to Drosophila melanogaster](http://journals.plos.org/plosone/article?id=info:doi/10.1371/journal.pone.0052988)


--- .class #id

## Longevity by Organic Food

- *Data simulated to be similar to paper*

![plot of chunk fruit-flies](figure/fruit-flies-1.png)



--- .class #id

## Evaluating Evidence

- Fruit flies who eat organic food live longer in this sample. 
- Mean: $\bar{Y}_O - \bar{Y}_C = 4.23$
- Possible reasons?
    - Eating organic increases longevity. 
    - <delete>The groups differed at baseline</delete>
    - Just random chance. 


--- .class #id

## How do we check random chance?

- We can simulate this. 
- The next slide will detail the steps of this. 

--- .class #id

## Simulations

1. Assume no difference in days survived, regardless of food. 
2. Mimic random chance: Randomize into groups.
3. Compute the difference in means. 
4. Repeat the steps many times. 

--- .class #id

## Simulations

Start with the data:


```r
data3
```

```
## # A tibble: 500 x 2
##    Food     days
##    <chr>   <dbl>
##  1 organic  14.0
##  2 organic  20.5
##  3 organic  25.3
##  4 organic  10.4
##  5 organic  21.4
##  6 organic  21.8
##  7 organic  16.5
##  8 organic  16.6
##  9 organic  16.5
## 10 organic  15.2
## # ... with 490 more rows
```

--- .class #id

## What do we see?

- 500 rows of data. 
- Food in one column. 
- Days survived in another. 
- We can simulated 500 organic or conventional values. 

--- .class #id

## Simulations

- One simulation

```r
T = 500
Food.sim = replicate(T, sample(c("organic", "conventional"), 1))
Food.sim = enframe(Food.sim, name=NULL, value="Food.sim")
data3 <- bind_cols(data3, Food.sim)
test <- data3 %>% group_by(Food.sim) %>% summarise(m=mean(days))
difference = test$m[1] - test$m[2]
difference
```

```
## [1] 0.653801
```

--- .class #id

## Simulations


```r
diff_sim <- function(data){
new.col = replicate(500, sample(c("organic", "conventional"), 1))
new.col2 = enframe(new.col, name=NULL, value="Food.sim")
data <- bind_cols(data, new.col2)
test <- data %>% group_by(Food.sim) %>% summarise(m=mean(days))
difference = test$m[1] - test$m[2]
return(difference)
}

data3 <- gather(data2, key="Food", value="days")
diff = replicate(1000, diff_sim(data3))
```


--- .class #id

## Histogram of Simulated Differences

![plot of chunk fruit-hist](figure/fruit-hist-1.png)

--- .class #id

## How Likely is our value?


```r
mean(diff>4.231288)
```

```
## [1] 0
```


--- .class #id

## Evaluating Evidence

- Possible reasons?
    - Eating organic increases longevity. 
    - <delete>The groups differed at baseline</delete>
    - <delete>Just random chance.</delete>

--- .class #id

## Conclusion

- Eating organic lenthens life. 
- **For a fruit fly**

--- .class #id

## Hypothesis Testing

- With testing we are interested in comparing different groups and having a mathematical way to judge differences. 
- Traditionally this is done with 2 hypothesis:
    - **Null Hypothesis**
    - **Alternative Hypothesis**

--- .class #id

## Hypothesis

- **Null Hypothesis**: This is what we assume to be true. Typically:
      - Suggest no difference between groups. 
      - Set population value equal to some known quantity. 
- **Alternative Hypothesis**: This is what we are testing and what we believe about the population. 
    - We must provide enough evidence that this is true. 
    

--- .class #id

## Hypothesis Testing

- It is important to understand that we are testing an unknown value in the ***population*** using ***sample*** data. 
- This means we need to have a way to approximate the population distribution from the sampling distribution. 
- We will explore sampling distributions in the next set of notes. 


--- .class #id

## Consider US Court System

- In the US a person is presumed innocent until proven guilty. 
- This is also apart of the Unitd Nations Declaration of Human Rights. 
- The goal of the prosecutor is to present enough evidence that they jury believes guilt beyond a reasonable doubt. 
- The goal of the defense is to display the doubt. 

--- .class #id

## Hypothesis Testing as Court Case

- In science we also do something similar. 
- As a scientist many times you take the role of prosecution and your goal is to make a claim and provide enough evidence so people will believe your claim. 
- Other times as reviewers and commentors we play defense and suggest flaws in the evidence and reasons not to believe the research. 




