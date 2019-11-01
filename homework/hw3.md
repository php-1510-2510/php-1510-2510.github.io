---
title: "Homework 3"
author: "Your Name"
date: "Due: November 12, 2019 at 11:59pm"
output:
  html_document: default
  word_document: default
  pdf_document: default
---


<style type="text/css">
.table {

    width: 80%;
    margin-left:10%; 
    margin-right:10%;
}
</style>

### Homework Policies:

*You are encouraged to discuss problem sets with your fellow students (and with the Course Instructor of course), but you must write your own final answers, in your own words. Solutions prepared ``in committee'' or by copying someone else's paper are not acceptable.  This violates the Brown standards of plagiarism, and you will not have the benefit of having thought about and worked the problem when you take the examinations.*

*All answers must be in complete sentences and all graphs must be properly labeled.*

***In this homework you will be required to use .Rmd to do it., you can then knit to a word document of PDF to turn it in.***

***For the PDF Version of this assignment: [PDF](https://raw.githubusercontent.com/php-1510-2510/php-1510-2510.github.io/master/homework/hw3.pdf)***

***For the R Markdown Version of this assignment: [RMarkdown](https://raw.githubusercontent.com/php-1510-2510/php-1510-2510.github.io/master/homework/hw3.Rmd)***

### Turning the Homework in:

*Please turn the homework in through canvas. You may use a pdf, html or word doc file to turn the assignment in.*

[PHP 1510 Assignment Link](https://canvas.brown.edu/courses/1078851/assignments/7746994)

[PHP 2510 Assignment Link](https://canvas.brown.edu/courses/1078852/assignments/7746995)




## Central Limit Theorem and Confidence Intervals. 

1. What does the Law of Large Numbers tell us?  
2. What does the Central Limit Theorem tell us? 
3. How do the law of large numbers and the Central Limit Theorem work together? 
4. What are 3 reasons we would want to create a confidence interval? 
5. How do we know whether or not we can get our critical value in a confidence interval from the $Z$ distribution of a $t$ distribution. 
6. Generate 1000 random values from a $N(2, 4)$ distribution. 
    a. Create and interpret a 90% confidence interval using the $Z$ distribution. 
    b. Create and interpret a 90% confidence interval using the $t$ distribution.
    c. How do these compare? 


### Data Problems 

We will work with the same data in which we used for lectures 13 and 15 as well as your midterm. 


```r
load("/path/to/data/brfs.rda")
```

```
## Warning in readChar(con, 5L, useBytes = TRUE): cannot open compressed file
## '/path/to/data/brfs.rda', probable reason 'No such file or directory'
```

```
## Error in readChar(con, 5L, useBytes = TRUE): cannot open the connection
```



7. What is the mean number of poor mental health days (`menthlth`)? 

8. Graph and describe the distribution of poor mental health days. 

9. Create and interpret a confidence interval for poor mental health days
    a. Using the $t$ distribution.
    b. Using the bootstrap approach with 1000 bootstraps. 
    
10. Consider the variable of Binary General Health, `genhlth_bin`, and the relationship it has with poor mental health days, `menthlth`. 
    a. Bootstrap a confidence interval for the mean number of poor mental health days for those who are generally healthy. 
    b. Bootstrap a confidence interval for the mean number of poor mental health days for those who are not generally healthy. 
    c. Interpret and discuss what connections you see between these confidence intervals.
    d. Bootstrap and interpret a confidence interval for the difference in mean poor mental health days between those who are generally healthy and those who are not. 


