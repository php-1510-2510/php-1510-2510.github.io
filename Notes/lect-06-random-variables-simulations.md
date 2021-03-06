---
title       : Random Variables
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

# Random Variables



--- .class #id

## Random Variables Simulation

- We have discussed the importance of simulations throughout the past few lectures. 
- They help us to:
    - Understand complex mathematical problems. 
    - Adapt real life situations in which we may not understand the exact probabilitiy formulas. 
    - Consider further variation and understand what may lead to a change in outcome. 
    


--- .class #id

## Binomial Simulation

- I want to use an example from a statistics textbook. 
- ***Disclaimer*** the example they use is not actually the basis of the case but none the less is interesting. 
- We will consider a 1978 Supreme Court Case
    - Ballew v. Georgia 
    - 435 U.S. 223, 236-237 (1978)
    


--- .class #id 

## Ballew v. Georgia (1978)

- The author stated that this case was about Ballew feeling his rights had been violated by having a 12 member jury of all white jurors. 
- I don't present problems without reading them myself, and found that the Ballew Case was actually about the size of the jury being 5 was a violation of his constitutional rights. 
- Reason for me stating this: Do not just trust a textbook. 


--- .class #id

## Racial Discrimination in Jury Selection

- Even though this was not the supreme court case I wanted to model, let's consider the problem at hand. 
- The author of the textbook states the following pieces of the problem:
    - A jury of 12 members was all white. 
    - The proportion of the community the jury was selected from was 90% white. 


--- .class #id

## With a Binomial

- We can do this fairly simply if we consider a success being that a non-white juror is selected, we can assume 10% probability for a success. 
- Then we have the case in which out of 12, we had 0 success. 
- This means by the binomial we have:
$$0.9^{12}= 0.2824295$$
- This suggests just shy of a 30% chance of not having a non-white person on a jury. 

--- .class ##

## With R

- Let's simulate these trials


```r
set.seed(100)
t <- 100
p <- 0.10
n <- 12

juries <- rbinom(n=t, size=n, prob=p)
tab <- table(juries)
prop.table(tab)
```

```
## juries
##    0    1    2    3    4 
## 0.24 0.42 0.24 0.08 0.02
```

- We see something very close to the actual probability of 28%. 

--- .class #id

## What if we change the parameters

- Ballew only had 5 jurors in his case


```r
set.seed(100)
t <- 100
p <- 0.10
n <- 5

juries <- rbinom(n=t, size=n, prob=p)
tab <- table(juries)
prop.table(tab)
```

```
## juries
##    0    1    2 
## 0.58 0.35 0.07
```


--- .class #id

## Court Decision

- Ballew should have had at least 6


```r
set.seed(100)
t <- 100
p <- 0.10
n <- 6

juries <- rbinom(n=t, size=n, prob=p)
tab <- table(juries)
prop.table(tab)
```

```
## juries
##    0    1    2    3 
## 0.51 0.39 0.09 0.01
```

--- .class #id

## Conclusions

- There would have been no evidence of a biased jury with 12 all white members.
- Do you believe that? 

--- .class #id

## Important Considerations

- If the pool of jurors is 90% white, then the decision of no bias would be correct. 
- This is an important fact. 
- When you want to attack biases, you need to attack the source of the problem.
- The source is the jury pool, not the particular jury that was selected.



--- .class #id

## Making a Function

- We keep repeating this experiment over and over by copying and pasting in values. 
- We can make a function in R that represents a binomial problem. 


```r
binom.sim <- function(t, n, p) {
juries <- rbinom(n=t, size=n, prob=p)
tab <- table(juries)

return(prop.table(tab))
}
```

--- .class #id

## Further Explorations

- Now we can explore by changing proportions of jury pool. 
- The lab will play with this function and consider what proportions might make for a more fair jury. 

--- .class #id

## Birthday Problem

- The birthday problem is common in statistics. 
- The goal how many people do we need to poll before we find common birthdays
- How do we do this? 
    - Ask a person their birthday
    - Check if we have that birthday
    - If not add it to our list and ask another. 
    - Stop when we have 2 in common
    

--- .class #id

## Uniform Distribution

- This is going to be a uniform distribution simulation. 
- We will assume that each birthday has a $1/366=0.00273224$ chance of happening. 
- We will then build the simulation with this concept. 

--- .class #id

## Birthday Function




```r
bday <- function(){
total_samp <- NULL

for (i in 1:100) {
samp <- sample(1:366, size=1)

if (samp %in% total_samp) {
  break
  
} else
{
total_samp[i] = samp
}
}
return(length(total_samp))
}
```



--- .class #id

## Simulate


```r
T <- 100000
n <- replicate(T, bday())
```


--- .class #id

## Plot Shape


```r
library(ggplot2)
ggplot(data=NULL, aes(x=n)) + 
    geom_bar(fill="red") + 
    theme_minimal() + 
    ylab("Number of Occurences")
```

--- .class #id

## Plot Shape

![plot of chunk bdays-uniform](figure/bdays-uniform-1.png)


--- .class #id

## Plot Probabilities




```r
ggplot(data=NULL, aes(x=n)) + 
    geom_bar(aes(y=..count../sum(..count..)), fill="red") + 
    theme_minimal() + 
    ylab("Probability of Occurences")
```


--- .class #id

## Plot Probabilities



![plot of chunk bdays-uniform-probs](figure/bdays-uniform-probs-1.png)

--- .class #id


## Actual Probabilities of Birthdays

- We have a dataset that is contained in the code for the notes.
- It contains a count of all the births in the US on each day of the year from 1994 to 2014. 
- We will first explore the probability of this. 

![plot of chunk bdays-time](figure/bdays-time-1.png)



--- .class #id


## What do we see?

- Multiple days of the year there are less births. 
    - New Years
    - Valentines Day
    - Christmas

        

--- .class #id

## Modify Birthday Function


```r
bday_real <- function(){
total_samp <- NULL

for (i in 1:100) {
samp <- sample(1:366, size=1, prob=days$prop)

if (samp %in% total_samp) {
  break
  
} else
{
total_samp[i] = samp
}
}
return(length(total_samp))
}
```

--- .class #id

## Simulate


```r
T <- 100000
n2 <- replicate(T, bday_real())
```

--- .class #id

## Plot Shape

![plot of chunk real-bdays ](figure/real-bdays -1.png)



--- .class #id

## Plot Both Together


![plot of chunk both-together](figure/both-together-1.png)

--- .class #id

## What do we see? 

- Turns out there is very little difference between the real and uniform probability. 
- Properties of real:
    - Expecation (mean): 23.62066 days
    - Variance: 148.0134413
- Properties of Uniform:
    - Expecation (mean): 23.65677 days
    - Variance: 150.3335665
