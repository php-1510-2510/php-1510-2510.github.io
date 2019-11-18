---
title       : Categorical Data
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




# Categorical Data Analysis

--- .class #id

## Categorical Data Analysis

- So far we have focused primarily on continuous outcomes and how to compare them. 
- Many times we have categorical data that we wish to compare. 
- We have seen this with a proportion test but there are other methods to doing this. 

--- .class #id

## Chi-Square Distribution


|     | Infected | Not Infected |  | 
| -------- | --------- | ---------- | ----------- | 
| Innoculated | 3 | 276 | 279  | 
| Not Innoculated | 66 | 473 | 539 | 
|  | 69 | 749 | 818 | 

