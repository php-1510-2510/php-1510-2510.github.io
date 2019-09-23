---
title       : Transforming Data with `dplyr`
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








# The `dplyr` Package


--- .class #id

## The `dplyr` Package

- When we work with data, we need to be able to clean it and modify it as needed.  
- We may wish to  do things like:
    - add additional variables.
    - Scale variables
    - Collapse variables 
- The `dplyr` package allows us to further work with our data. 

--- .class #id


## `dplyr` Functionality

- With `dplyr` we have five basic verbs that we will learn to work with:
  - `filter()`
  - `select()`
  - `arrange()`
  - `mutate()`
  - `summarize()`


  


---  .segue bg:grey


# Filtering 


--- .class #id

## Filtering


- At this point we will consider how we pick the rows of the data that we wish to work with. 
- If you consider many modern data sets, we have so much information that we may not want to bring it all in at once. 
- R brings data into the RAM of your computer. This means you can be limited for what size data you can bring in at once. 
- Very rarely do you need the entire data set. 
- We will focus on how to pick the rows or observations we want now.


--- .class #id

## Enter the `filter()` Function

- The `filter()` function chooses rows that meet a specific criteria. 
- We can do this with Base R functions or with   `dplyr`. 


```r
library(dplyr)
```

--- .class #id

## The Data

- We will begin by looking at some data from [FiveThirtyEight](https://fivethirtyeight.com/)
- They have a wealth of data stored with an R package and lots of data analysis done online. 
- We will work with a dataset behind the story ["Comic Books Are Still Made By Men, For Men And About Men"](https://fivethirtyeight.com/features/women-in-comic-books/).
- *I know this is not public health but it has some interesting attributes to the data*.



--- .class #id

## Exploring the Data

- It is always good to learn how to explore the data and we will learn this more as we go. 
- We will start with a simple command to look at the structure of the data. 


--- .class #id

## Structure of the Data


```r
library(tidyverse)
library(fivethirtyeight)

str(comic_characters)
```


--- .class #id

## Structure of the Data


```
## Classes 'tbl_df', 'tbl' and 'data.frame':	23272 obs. of  16 variables:
##  $ publisher       : chr  "Marvel" "Marvel" "Marvel" "Marvel" ...
##  $ page_id         : int  1678 7139 64786 1868 2460 2458 2166 1833 29481 1837 ...
##  $ name            : chr  "Spider-Man (Peter Parker)" "Captain America (Steven Rogers)" "Wolverine (James \\\"Logan\\\" Howlett)" "Iron Man (Anthony \\\"Tony\\\" Stark)" ...
##  $ urlslug         : chr  "\\/Spider-Man_(Peter_Parker)" "\\/Captain_America_(Steven_Rogers)" "\\/Wolverine_(James_%22Logan%22_Howlett)" "\\/Iron_Man_(Anthony_%22Tony%22_Stark)" ...
##  $ id              : chr  "Secret Identity" "Public Identity" "Public Identity" "Public Identity" ...
##  $ align           : Ord.factor w/ 4 levels "Bad Characters"<..: 4 4 NA 4 4 4 4 4 NA 4 ...
##  $ eye             : chr  "Hazel Eyes" "Blue Eyes" "Blue Eyes" "Blue Eyes" ...
##  $ hair            : chr  "Brown Hair" "White Hair" "Black Hair" "Black Hair" ...
##  $ sex             : chr  "Male Characters" "Male Characters" "Male Characters" "Male Characters" ...
##  $ gsm             : chr  NA NA NA NA ...
##  $ alive           : chr  "Living Characters" "Living Characters" "Living Characters" "Living Characters" ...
##  $ appearances     : int  4043 3360 3061 2961 2258 2255 2072 2017 1955 1934 ...
##  $ first_appearance: chr  "1962, August" "1941, March" "1974, October" "1963, March" ...
##  $ month           : chr  "August" "March" "October" "March" ...
##  $ year            : int  1962 1941 1974 1963 1950 1961 1961 1962 1963 1961 ...
##  $ date            : Date, format: "1962-08-01" "1941-03-01" ...
```



--- .class #id

## Filtering Example

- Filtering is the idea of considering only certain observations. 
- THis means filtering is done at the observation/person level.
- Let's say we only want to consider "Marvel Characters"
- Traditionally in R, this was done with what is called indexing but we will use the `dplyr` tools for this. 

--- .class #id

## Filtering Example


```r
library(tidyverse)
comic_characters %>%
  filter(publisher=="Marvel")
```

--- .class #id

## Filtering Example


```
## # A tibble: 16,376 x 16
##    publisher page_id name  urlslug id    align eye   hair  sex   gsm  
##    <chr>       <int> <chr> <chr>   <chr> <ord> <chr> <chr> <chr> <chr>
##  1 Marvel       1678 Spid~ "\\/Sp~ Secr~ Good~ Haze~ Brow~ Male~ <NA> 
##  2 Marvel       7139 Capt~ "\\/Ca~ Publ~ Good~ Blue~ Whit~ Male~ <NA> 
##  3 Marvel      64786 "Wol~ "\\/Wo~ Publ~ <NA>  Blue~ Blac~ Male~ <NA> 
##  4 Marvel       1868 "Iro~ "\\/Ir~ Publ~ Good~ Blue~ Blac~ Male~ <NA> 
##  5 Marvel       2460 Thor~ "\\/Th~ No D~ Good~ Blue~ Blon~ Male~ <NA> 
##  6 Marvel       2458 Benj~ "\\/Be~ Publ~ Good~ Blue~ No H~ Male~ <NA> 
##  7 Marvel       2166 Reed~ "\\/Re~ Publ~ Good~ Brow~ Brow~ Male~ <NA> 
##  8 Marvel       1833 Hulk~ "\\/Hu~ Publ~ Good~ Brow~ Brow~ Male~ <NA> 
##  9 Marvel      29481 Scot~ "\\/Sc~ Publ~ <NA>  Brow~ Brow~ Male~ <NA> 
## 10 Marvel       1837 Jona~ "\\/Jo~ Publ~ Good~ Blue~ Blon~ Male~ <NA> 
## # ... with 16,366 more rows, and 6 more variables: alive <chr>,
## #   appearances <int>, first_appearance <chr>, month <chr>, year <int>,
## #   date <date>
```

--- .class #id

## More than One Variable Filtered

- We can also filter on more than just one variable. 
- We could filter by characters before 1970 and from Marvel. 


```r
comic_characters %>%
    filter(publisher=="Marvel" & year<1970)
```


--- .class #id

## More than One Variable Filtered

- We can also chain together more than one filter command and achieve the same results:


```r
comic_characters %>%
    filter(publisher=="Marvel") %>%
    filter(year<1970)
```


--- .class #id

## Results


```
## # A tibble: 3,118 x 16
##    publisher page_id name  urlslug id    align eye   hair  sex   gsm  
##    <chr>       <int> <chr> <chr>   <chr> <ord> <chr> <chr> <chr> <chr>
##  1 Marvel       1678 Spid~ "\\/Sp~ Secr~ Good~ Haze~ Brow~ Male~ <NA> 
##  2 Marvel       7139 Capt~ "\\/Ca~ Publ~ Good~ Blue~ Whit~ Male~ <NA> 
##  3 Marvel       1868 "Iro~ "\\/Ir~ Publ~ Good~ Blue~ Blac~ Male~ <NA> 
##  4 Marvel       2460 Thor~ "\\/Th~ No D~ Good~ Blue~ Blon~ Male~ <NA> 
##  5 Marvel       2458 Benj~ "\\/Be~ Publ~ Good~ Blue~ No H~ Male~ <NA> 
##  6 Marvel       2166 Reed~ "\\/Re~ Publ~ Good~ Brow~ Brow~ Male~ <NA> 
##  7 Marvel       1833 Hulk~ "\\/Hu~ Publ~ Good~ Brow~ Brow~ Male~ <NA> 
##  8 Marvel      29481 Scot~ "\\/Sc~ Publ~ <NA>  Brow~ Brow~ Male~ <NA> 
##  9 Marvel       1837 Jona~ "\\/Jo~ Publ~ Good~ Blue~ Blon~ Male~ <NA> 
## 10 Marvel      15725 Henr~ "\\/He~ Publ~ Good~ Blue~ Blue~ Male~ <NA> 
## # ... with 3,108 more rows, and 6 more variables: alive <chr>,
## #   appearances <int>, first_appearance <chr>, month <chr>, year <int>,
## #   date <date>
```




--- .class #id


## Further Filtering

- `filter()` supports the use of multiple conditions where we can use Boolean. 
- For example if we wanted to consider only observations that have a  first year of between 1980 and 1985:


```r
comic_characters %>% 
    filter(year>=1980 & year<=1985)
```

--- .class #id


## Further Filtering


```
## # A tibble: 2,054 x 16
##    publisher page_id name  urlslug id    align eye   hair  sex   gsm  
##    <chr>       <int> <chr> <chr>   <chr> <ord> <chr> <chr> <chr> <chr>
##  1 Marvel       2527 Kath~ "\\/Ka~ Secr~ Good~ Haze~ Brow~ Fema~ <NA> 
##  2 Marvel      37690 Jenn~ "\\/Je~ Publ~ Good~ Gree~ Brow~ Fema~ <NA> 
##  3 Marvel       3765 Emma~ "\\/Em~ Publ~ <NA>  Blue~ Brow~ Fema~ <NA> 
##  4 Marvel       1276 Samu~ "\\/Sa~ Secr~ Good~ Blue~ Blon~ Male~ <NA> 
##  5 Marvel       2626 Robe~ "\\/Ro~ Secr~ Good~ Brow~ Blac~ Male~ <NA> 
##  6 Marvel       2007 Rahn~ "\\/Ra~ Secr~ Good~ Gree~ Red ~ Fema~ <NA> 
##  7 Marvel       1395 Dani~ "\\/Da~ Secr~ Good~ Brow~ Blac~ Fema~ <NA> 
##  8 Marvel       1404 Alis~ "\\/Al~ Publ~ Good~ Blue~ Stra~ Fema~ <NA> 
##  9 Marvel      18186 Veno~ "\\/Ve~ Know~ <NA>  Vari~ No H~ Agen~ <NA> 
## 10 Marvel       1973 Jame~ "\\/Ja~ Secr~ Good~ Brow~ Blac~ Male~ <NA> 
## # ... with 2,044 more rows, and 6 more variables: alive <chr>,
## #   appearances <int>, first_appearance <chr>, month <chr>, year <int>,
## #   date <date>
```

--- .class #id


## Further Filtering

- We can also use the `filter()` function to remove missing data for us. 
- On Datacamp you learned a class of functions called `is.`*foo*`()` where *foo* represents a data type. 
- We could choose to only use observations that have gender stated. 
- That means we wish to not have missing data for gender. 



```r
comic_characters %>% filter(!is.na(sex))
```

--- .class #id


## On Your Own: RStudio Practice

- These slides are for you to practice on your own when you are at home. 
- I will state the problem and data on one slide.
- On the next I will show what your answer should look like. 
- Code for the solutions can be found on github. 

--- .class #id

## On Your Own: RStudio Practice

- For these data we will consider the gapminder dataset. 
- You will need the `gapminder` package.
- You can run `install.packages("gapminder")` to get it. 

--- .class #id 

## On Your Own: RStudio Practice

Using the `filter()` function and chaining: 

- Choose only rows associated with
  - Uganda
  - Rwanda

--- .class #id


## On Your Own: RStudio Practice


Your end result should be:


```
## # A tibble: 24 x 6
##    country continent  year lifeExp     pop gdpPercap
##    <fct>   <fct>     <int>   <dbl>   <int>     <dbl>
##  1 Rwanda  Africa     1952    40   2534927      493.
##  2 Rwanda  Africa     1957    41.5 2822082      540.
##  3 Rwanda  Africa     1962    43   3051242      597.
##  4 Rwanda  Africa     1967    44.1 3451079      511.
##  5 Rwanda  Africa     1972    44.6 3992121      591.
##  6 Rwanda  Africa     1977    45   4657072      670.
##  7 Rwanda  Africa     1982    46.2 5507565      882.
##  8 Rwanda  Africa     1987    44.0 6349365      848.
##  9 Rwanda  Africa     1992    23.6 7290203      737.
## 10 Rwanda  Africa     1997    36.1 7212583      590.
## # ... with 14 more rows
```

---  .segue bg:grey



# Selecting


--- .class #id

## Selecting

- The next logical step would be to select the columns we want as well. 
- Many times we have so many columns that we are not interested in for a particular analysis. 
- Instead of slowing down your analysis by continuing to run through extra data, we could just select the columns we care about. 

--- .class #id


## Enter the `select()` Function

- The `select()` function chooses columns that we specify. 
- Again we can do this with base functions or with `dplyr`. 
- We feel that as you continue on with your R usage that you will most likely want to go the route of `dplyr` functions instead.



--- .class #id

## Selecting Example 

- If we explore the comics data further, we can see that there may be some variables we are more interested than others. 
- Consider character attributes, we may wish to just consider name, gender, eye color and hair color. 

We could then do the following


```r
comic_characters %>%
    select(name, sex, eye, hair)
```


--- .class #id

## Selecting Example Continued



```
## # A tibble: 23,272 x 4
##    name                                  sex            eye       hair     
##    <chr>                                 <chr>          <chr>     <chr>    
##  1 Spider-Man (Peter Parker)             Male Characte~ Hazel Ey~ Brown Ha~
##  2 Captain America (Steven Rogers)       Male Characte~ Blue Eyes White Ha~
##  3 "Wolverine (James \\\"Logan\\\" Howl~ Male Characte~ Blue Eyes Black Ha~
##  4 "Iron Man (Anthony \\\"Tony\\\" Star~ Male Characte~ Blue Eyes Black Ha~
##  5 Thor (Thor Odinson)                   Male Characte~ Blue Eyes Blond Ha~
##  6 Benjamin Grimm (Earth-616)            Male Characte~ Blue Eyes No Hair  
##  7 Reed Richards (Earth-616)             Male Characte~ Brown Ey~ Brown Ha~
##  8 Hulk (Robert Bruce Banner)            Male Characte~ Brown Ey~ Brown Ha~
##  9 Scott Summers (Earth-616)             Male Characte~ Brown Ey~ Brown Ha~
## 10 Jonathan Storm (Earth-616)            Male Characte~ Blue Eyes Blond Ha~
## # ... with 23,262 more rows
```


--- .class #id

## Removing Columns


- We may wish to pick certain columns that we wish to have but we also may want to remove certain columns. 
- It is quite common to de-identify a dataset before actually distributing it to a research team. 
- The `select()` function will also remove columns. 



--- .class #id

## Removing Columns

- Lets say that we wished to remove the `page_id` and `urlslug` of the characters:


```r
comic_characters %>%
  select(-page_id,-urlslug)
```

--- .class #id

## Removing Columns

We also could use a vector for this:


```r
cols <- c("url_slug", "page_id")
gapminder %>%
  select(-one_of(cols))
```



--- .class #id

## On Your Own: RStudio Practice

- Consider the gapminder data: `gapminder`. 
  1. Select all but the `pop` column.
  2. Remove the year and lifeExp from them.
  3. Select values which contain "p" in them (hint: look up `contains()` function). 
  4. Chain these together so that you run a command and it does all of these things. 

--- .class #id

## On Your Own: RStudio Practice


Your answer should look like:


```
## # A tibble: 1,704 x 1
##    gdpPercap
##        <dbl>
##  1      779.
##  2      821.
##  3      853.
##  4      836.
##  5      740.
##  6      786.
##  7      978.
##  8      852.
##  9      649.
## 10      635.
## # ... with 1,694 more rows
```



--- .segue bg:grey

# Arranging the Data

--- .class #id

## Arranging the Data

- We also have need to make sure the data is ordered in a certain manner. This can be easily done in R with the `arrange()` function. 
- Again we can do this in base R but this is not always a clear path. 

--- .class #id

## Arranging the Data Example

- Let's say that we wish to look characters year of entry and total appearances. 
- Then say further we want to sort it by appearances. 


--- .class #id

## Arranging the Data Example Continued


```r
comic_characters %>%
    select(name,year,  appearances) %>%
    arrange(appearances)
```

--- .class #id

## Arranging the Data Example Continued


```
## # A tibble: 23,272 x 3
##    name                                   year appearances
##    <chr>                                 <int>       <int>
##  1 Bardak (Earth-616)                     1939           1
##  2 Bends (Masked Raider) (Earth-616)      1939           1
##  3 Blackie Ross (Earth-616)               1939           1
##  4 Bleck (Earth-616)                      1939           1
##  5 Cal Brunder (Earth-616)                1939           1
##  6 Constance Rand (Earth-616)             1939           1
##  7 Dead Shot (Masked Raider) (Earth-616)  1939           1
##  8 Dr. Lang (The Big Boss) (Earth-616)    1939           1
##  9 Dutch Hansen (Earth-616)               1939           1
## 10 Ganya (Earth-616)                      1939           1
## # ... with 23,262 more rows
```

--- .class #id

## Arranging the Data Example Continued

- With `arrange()` we first use `select()` to pick the only columns that we want and then we arrange by the `appearances`. 
- If we had wished to order them in a descending manner we could have simply used the `desc()` function:


```r
comic_characters %>%
    select(name,year,  appearances) %>%
    arrange(desc(appearances))
```

--- .class #id


## More Complex Arrange

- Lets consider that we wish to look at the top 3 Appearances for each year.
- Then we wish to order them from largest to smallest Appearances. 
- We then need to do the following:
    1. Group by year.
    2. Pick the top 3 appearance.
    3. order them largest to smallest.
    



--- .class #id

## More Complex Arrange Continued



```r
comic_characters %>% 
  group_by(year) %>%  
  top_n(3, appearances) %>% 
  arrange(desc(appearances))
```

- Where
    - `group_by()` is a way to group data. This way we perform operations on a group. So top 3 appearances are grouped by year. 
    - `top_n()`takes a tibble and returns a specific number of rows based on a chosen value. 


--- .class #id

## More Complex Arrange Continued



```
## # A tibble: 241 x 16
## # Groups:   year [80]
##    publisher page_id name  urlslug id    align eye   hair  sex   gsm  
##    <chr>       <int> <chr> <chr>   <chr> <ord> <chr> <chr> <chr> <chr>
##  1 Marvel       1678 Spid~ "\\/Sp~ Secr~ Good~ Haze~ Brow~ Male~ <NA> 
##  2 Marvel       7139 Capt~ "\\/Ca~ Publ~ Good~ Blue~ Whit~ Male~ <NA> 
##  3 DC           1422 Batm~ "\\/wi~ Secr~ Good~ Blue~ Blac~ Male~ <NA> 
##  4 Marvel      64786 "Wol~ "\\/Wo~ Publ~ <NA>  Blue~ Blac~ Male~ <NA> 
##  5 Marvel       1868 "Iro~ "\\/Ir~ Publ~ Good~ Blue~ Blac~ Male~ <NA> 
##  6 DC          23387 Supe~ "\\/wi~ Secr~ Good~ Blue~ Blac~ Male~ <NA> 
##  7 Marvel       2460 Thor~ "\\/Th~ No D~ Good~ Blue~ Blon~ Male~ <NA> 
##  8 Marvel       2458 Benj~ "\\/Be~ Publ~ Good~ Blue~ No H~ Male~ <NA> 
##  9 Marvel       2166 Reed~ "\\/Re~ Publ~ Good~ Brow~ Brow~ Male~ <NA> 
## 10 Marvel       1833 Hulk~ "\\/Hu~ Publ~ Good~ Brow~ Brow~ Male~ <NA> 
## # ... with 231 more rows, and 6 more variables: alive <chr>,
## #   appearances <int>, first_appearance <chr>, month <chr>, year <int>,
## #   date <date>
```



--- .class #id

## On Your Own: RStudio Practice

- Perform the following operations:
  - Group by year. 
  - use `sample_n()` to pick 1 observation per year 
  - Arrange by longest to smallest life expectancy. 



--- .class #id





## On Your Own: RStudio Practice


Your answer ***may*** look like:


```
## # A tibble: 12 x 6
## # Groups:   year [12]
##    country           continent  year lifeExp       pop gdpPercap
##    <fct>             <fct>     <int>   <dbl>     <int>     <dbl>
##  1 Sweden            Europe     1997    79.4   8897619    25267.
##  2 Sweden            Europe     1992    78.2   8718867    23880.
##  3 Oman              Asia       2002    74.2   2713462    19775.
##  4 Sweden            Europe     1962    73.4   7561588    12329.
##  5 Bahrain           Asia       1982    69.1    377967    19211.
##  6 Ecuador           Americas   1972    58.8   6298651     5281.
##  7 Pakistan          Asia       1987    58.2 105186881     1705.
##  8 Vietnam           Asia       1967    47.8  39463910      637.
##  9 Nigeria           Africa     2007    46.9 135031164     2014.
## 10 Morocco           Africa     1952    42.9   9939217     1688.
## 11 Equatorial Guinea Africa     1977    42.0    192675      959.
## 12 Sierra Leone      Africa     1957    31.6   2295678     1004.
```

---  .segue bg:grey


# Adding New Variables


--- .class #id

## Adding New Variables

- There is usually no way around needing a new variable in your data. 
- For example, most medical studies have height and weight in them, however many times what a researcher is interested in using is Body Mass Index (BMI). 
- We would need to add BMI in. 


--- .class #id

## Adding New Variables

- Using the `tidyverse` we can add new variables in multiple ways
  - `mutate()`
  - `transmute()`


--- .class #id

## Example

- Consider appearances, this can be deceiving as some characters have been around for 50 years more than others. 
- A more logical way might be to consider average appearances per year. 
- This means we could create 2 variables for this. 
    - Total Years
    - Average Appearances per year. 

--- .class #id

## Adding Appearances


- We can do this with the `mutate()` function. 


```r
comic_characters %>%
    select(name, year, appearances) %>%
    mutate(total_years = 2014-year) %>%
    mutate(avg_appearances = appearances/total_years) %>%
    arrange(desc(total_years))
```

--- .class #id

## Example: Mutate


```
## # A tibble: 23,272 x 5
##    name                        year appearances total_years avg_appearances
##    <chr>                      <int>       <int>       <dbl>           <dbl>
##  1 Richard Occult (New Earth)  1935         125          79          1.58  
##  2 Merlin (New Earth)          1936          92          78          1.18  
##  3 Franklin Delano Roosevelt~  1936          52          78          0.667 
##  4 Arthur Pendragon (New Ear~  1936          41          78          0.526 
##  5 Lancelot (New Earth)        1936          18          78          0.231 
##  6 Guinevere (New Earth)       1936          15          78          0.192 
##  7 Lady of the Lake (New Ear~  1936          13          78          0.167 
##  8 Gawain (New Earth)          1936           8          78          0.103 
##  9 Gareth (New Earth)          1936           1          78          0.0128
## 10 Bedivere (New Earth)        1936          NA          78         NA     
## # ... with 23,262 more rows
```


--- .class #id

## Changing the sort


```
## # A tibble: 23,272 x 5
##    name                        year appearances total_years avg_appearances
##    <chr>                      <int>       <int>       <dbl>           <dbl>
##  1 Eva Bell (Earth-616)        2013          53           1              53
##  2 Christopher Muse (Earth-6~  2013          49           1              49
##  3 Benjamin Deeds (Earth-616)  2013          40           1              40
##  4 Fabio Medina (Earth-616)    2013          39           1              39
##  5 Isabel Kane (Earth-616)     2013          31           1              31
##  6 Emily Preston (Earth-616)   2013          28           1              28
##  7 Iara Dos Santos (Earth-61~  2013          28           1              28
##  8 Kevin Connor (Earth-616)    2013          28           1              28
##  9 Anna Maria Marconi (Earth~  2013          26           1              26
## 10 H.E.L.E.N. (Earth-616)      2013          24           1              24
## # ... with 23,262 more rows
```


--- .class #id

## What can we see? 

- Our initial premise of more appearances is based on more time may not be accurate when weighting by year.
- Why?
    - Some might be characters that died and we didnt calculate this. 
    - Some may have been introduced and then dropped. 
    - Maybe more comics are produced now than in 1935. 
