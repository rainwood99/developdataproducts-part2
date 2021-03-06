---
title       : Motor Trend Data Analysis Report
subtitle    :  
author      : Yi
job         : Developing Data Products Presentation
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [bootstrap]   # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Exploratory Data Analysis
In this report, we will analyze `mtcars` data set and explore the relationship between a set of variables and miles per gallon (MPG).
First, we load the data set `mtcars` and change some variables from `numeric` class to `factor` class.   

```r
library(ggplot2)
data(mtcars)
mtcars[1:3, ] # Sample Data
dim(mtcars)
mtcars$cyl <- as.factor(mtcars$cyl)
mtcars$vs <- as.factor(mtcars$vs)
mtcars$am <- factor(mtcars$am)
mtcars$gear <- factor(mtcars$gear)
mtcars$carb <- factor(mtcars$carb)
attach(mtcars)
```
Then, we do some basic exploratory data analyses.

--- .class #id 

## Inference  
At this step, we make the null hypothesis as the MPG of the automatic and manual transmissions are from the same population (assuming the MPG has a normal distribution). We use the two sample T-test to show it.  

```r
result <- t.test(mpg ~ am)
result$p.value
```

```
## [1] 0.001373638
```

```r
result$estimate
```

```
## mean in group 0 mean in group 1 
##        17.14737        24.39231
```
Since the p-value is 0.00137, we reject our null hypothesis. So, the automatic and manual transmissions are from different populations.  

---

## Regression Analysis  
First, we fit the full model as the following.  

```r
fullModel <- lm(mpg ~ ., data=mtcars)
summary(fullModel) # results hidden
```
Then, we have the following model including the interaction term between wt and am:  

```r
amIntWtModel<-lm(mpg ~ wt + qsec + am + wt:am, data=mtcars)
summary(amIntWtModel) # results hidden
```
Finally, we select the final model.  

```r
anova(fullModel, amIntWtModel) 
confint(amIntWtModel) # results hidden
```
We end up selecting the model with the highest Adjusted R-squared value, "mpg ~ wt + qsec + am + wt:am".  

---

## Residual Analysis and Diagnostics  
The result shows that when "wt" (weight lb/1000) and "qsec" (1/4 mile time) remain constant, cars with manual transmission add 14.079 + (-4.141)*wt more MPG (miles per gallon) on average than cars with automatic transmission. That is, a manual transmitted car that weighs 2000 lbs have 5.797 more MPG than an automatic transmitted car that has both the same weight and 1/4 mile time.  

