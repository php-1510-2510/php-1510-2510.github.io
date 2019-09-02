---
title       : Basic Probability
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






# Basic Probability

--- .class #id


## Why Basic Probability??

- When you first learn probability, you typically learn with dice and jars with mulitple color balls in them. 
- This gives a false sense of worthlessness with probability and the real world. 
- Probability is a the way in which mathematics can explain phenomenon in the world. 

--- .class #id

## The Start of Probability

- Probability started out with games of chance and the desire to understand strategies for winning these. 
- Many famous mathematicians would derive unique games or other games of chance and through letters discuss the mathematics behind them. 
- Probability theory is used to design the lottery, casino games, sports betting, treatment given in clinical trials, random advertisement choices you receive in social media, ...

--- .class #id

## Goal of Proabaility

- The goal of probability is understand how likely an event is to occur. 
- We have an event, `A` which could be having a heart attack. 
- Then we would say the `Pr(A)` is the probability of having a heart attack. 
- This has to be between 0 and 1. 
- Closer to 1 is more likely to occur. 

--- .class #id

## Statistician vs Practitioner

- Probability theory is the foundation of statistics and inference. 
- Two approaches exist for building the theory of inference
    - Statistician's Approach: Building Methods of modeling randomness using probability theory. 
    - Practitioner's Approach: Models based on intuition and experience. 
    


--- .class #id


## The two approaches

- Consider the data drawn from 1894-96 cholera innoculations in Calcutta, India. Of the 818 subjectes exposed to cholera 29 received an innoculation and 539 did not:
  
| | Infected | Not Infected |  | 
| ----- | ------- | -------- | ------- |
| Innoculated | 3 | 276 | 279 | 
| Not Innoculated | 66 | 473 | 539 | 
| | 69 | 749 | 818 | 


--- .class #id

## Research Questions

| | Infected | Not Infected |  | 
| ----- | ------- | -------- | ------- |
| Innoculated | 3 | 276 | 279 | 
| Not Innoculated | 66 | 473 | 539 | 
| | 69 | 749 | 818 | 

- Practitioner: Do Cholera Inoculations reduce the incidence of Cholera? 


--- .class #id

## Research Questions

| | Infected | Not Infected |  | 
| ----- | ------- | -------- | ------- |
| Innoculated | 3 | 276 | 279 | 
| Not Innoculated | 66 | 473 | 539 | 
| | 69 | 749 | 818 | 

- Statistician: Is there a difference between the following two conditional probabilities?
    - $$Pr(\text{Infected}|\text{Innoculated})$$
    - $$Pr(\text{Infected}|\text{Not Innoculated})$$

--- .class #id

## Questions Raised

- What is meant by "Conditional Probability"? 
- Could the observed proportions $3/279=0.01$ and $66/539=0.12$ differ enough that it is unlikely that chance alone caused the difference? 
- What is the single best summary of the outcome? 
- This course will hhelp us answer questions like this and more. 



--- .class #id


## Sample Spaces

- The set of points representing all possible outcome is called the **sample space** of the experiement. 
- In reality it can be hard to identify the entire sample space. 


--- .class #id

## Element

- An **element** of  sample space is one single point in the sample space. 
- When the sample space is continuous, there an infinite number of elements contained in it. 


--- .class #id

## 2 Dice

- Suppose we roll 2 dice, then the sample space would be 

![](https://qph.fs.quoracdn.net/main-qimg-6fe0ee70e3ca357140d0f4489e0cfb3f-c)

--- .class #id

## Event

- A subset of a sample space is called an **event**. 
- For example:
    - Let the event be the sum of the die being 7. 
    - This would give us 
    - (1,6), (2,5), (3,4), (4,3), (5,2), (6,1)
- This would be an example of a discrete event. 
- The probability of this is relatively simple, we had 36 events in the sample space and our event can happen 6 different ways. So the probability would be 
$6/36=1/6\approx 17\%$

--- .class #id

## Continuous Event

- Let's say that the human body temperature ranges from $90^oF$ to $110^oF$. Then the sample space of human body temperatures would be, all value $t$ such that
$$90^oF\le t \le 110^oF$$
- An event in this could be a normal temperature range:
$$97.7^oF\le t_1 \le 99.5^oF$$
- This is harder to consider the probability of this event. 
  

--- .class #id

## What do we really need? 

- Probability can be taught in multple courses. 
- From the simple calculating probabilities with basic algebra and calculus. 
- From there you can go into measure spaces and martingale theory. 
- We however, do not need all of that to understand statistics. 


--- .class #id

## Our Topics


- Independence
- Conditional Probability
- Product and Sum Rules
- Combinations and Permutations
- Probability Distributions



--- .class #id

## Probability in R

- Let's consider a group of individuals and we have requested information on gender. We then can say that we have collected this information and the R code below has this recorded: 


```r
gender <- rep(c("female", "male", "non-binary"), times=c(5250, 4850, 38))
table(gender)
```

```
## gender
##     female       male non-binary 
##       5250       4850         38
```
- Traditionally you would use formulas by hand to compute the probability of an individual in your data selecting one of these gender categories. 

--- .class #id

## Using R

- We can sample in R


```r
sample(gender,1)
```

```
## [1] "male"
```

- Each time we do this we can possibly get a different result

```r
sample(gender,1)
```

```
## [1] "male"
```


--- .class #id

## Multiple Runs

- Probability is about the long run. 
- We assume flipping a coin is 50/50, however we would need to flip a coin hundreds of times in the same fashion to fully determine this. 
- We can use R to `simulate` long run probabilities.
- We will replicate our selection of a random person and understanding the breakdown of gender in our study. 

--- .class #id

## Replication


```r
T <- 100000
events <- replicate(T, sample(gender, 1))
table(events)
```

```
## events
##     female       male non-binary 
##      51691      47934        375
```

--- .class #id

## Using R to calculate estimated Probabilities


```r
tab <- table(events)
prop.table(tab)
```

```
## events
##     female       male non-binary 
##    0.51691    0.47934    0.00375
```


--- .class #id

## Isn't That Cheating

- You may think that one should do things by hand, but the reality is most real life events cannot be worked out by hand. 
- We collect millions of pieces of data and use computers to solve much of the problems. 


--- .class #id

## Independence

- Outcome of an event has no affect on another event.
- A grade I got on a 3rd grade math test and a grade any of you got on a 3rd grade math test are independent. Why?
- We will learn how to understand this mathematically but we can think through the concept of independence. 

--- .class #id

# Independent or Not?

- A family has 3 children, one child has asthma, would the probability of the other 2 children having asthma be independent? 
- A family in South Dakota has a child with asthma, another family in Cork, Ireland is having a child, is the probability of their child having asthma independent of the family in South Dakota? 
- I love mathematics and statistics, is the probability of you loving it independent???


--- .segue bg:grey

# Counting

--- .class #id

## Counting

- We have seen how as things get more complicated that calculating probabilities gets even harder. 
- The can be said for counting. 
- As the problems get more and more complex, knowing the total number of elements in the same space gets harder and harder. 


--- .class #id

## Counting

- We will discuss counting with the following methods:
    - Multiplication Rule
    - Permutations
    - Combinations
    


--- .class #id

## Multiplication Rule 

- We have seen this rule play out already. 
- Consider 1 die:
    - Our Sample Space is:
    - 1, 2, 3, 4, 5, 6
- With 2 die:
    - The sample space became size 36. 


--- .class #id

## Multiplication Rule

- For 2 trials:
    - Trial one has $n_1$ ways of happening. 
    - Trial two has $n_2$ ways of happening.
    - Then the total number of ways the for the 2 trials to happen together is:
  
  $$ N=n_1*n_2$$


--- .class #id

## Multiplication Rule

- This is because for each number on the first die you could roll 6 numbers on the second die. 
- The picture below shows how this works
  
![](https://qph.fs.quoracdn.net/main-qimg-6fe0ee70e3ca357140d0f4489e0cfb3f-c)


--- .class #id

## General Multiplication Rule

- In general, if we assume that we have `r` decisions to be made. 
- If there are $n_1$ ways to decide Decision 1, $n_2$ ways to decide Decision 2, and so on
- The total number of ways to make all `r` decisions is:
$$N = n_1*n_2\cdots n_r$$


--- .class #id
### General Rule
$$Pr(A\cap B) = Pr(A)Pr(B|A)$$

### Independent Events

$$Pr(A\cap B) = Pr(A)Pr(B)$$

- Why does the independent events definition make sense? 



--- .class #id

## Addition Rule

$$ Pr(A\cup B) = Pr(A) + Pr(B) - Pr(A\cap B)$$

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-1.png)

```
## (polygon[GRID.polygon.1], polygon[GRID.polygon.2], polygon[GRID.polygon.3], polygon[GRID.polygon.4], text[GRID.text.5], text[GRID.text.6], text[GRID.text.7], text[GRID.text.8], text[GRID.text.9])
```

- Why do we subtract the intersection? 





