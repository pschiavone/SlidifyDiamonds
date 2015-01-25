---
title       : Application For Predicting Diamond Prices
subtitle    : 
author      : Paul Schiavone
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Quick Pitch for Diamond Price Prediction App

### What:  Web based App that allows users reliable diamond pricing

### Why:  

1. Rediculously easy to use Interface
2. Web based.  Easy to update and deploy
3. Built with Shiny.  Enough said.

--- .class #id 

## Load and PreProcess the Dataset for Exploration, Analysis and Modeling


```r
library(ggplot2)
names(diamonds)
```

```
##  [1] "carat"   "cut"     "color"   "clarity" "depth"   "table"   "price"  
##  [8] "x"       "y"       "z"
```

```r
# reduce the variables to the 4 c's and price
dia.small <- select(diamonds, -c(x:z, depth, table))
dia.small$cut <- as.integer(dia.small$cut)
dia.small$color <- as.integer(dia.small$color)
dia.small$clarity <- as.integer(dia.small$clarity)
```

---

## Explore The Data

```r
qplot(x = carat, y = price, data = dia.small, color = cut, shape = clarity, size = color)
```

![plot of chunk unnamed-chunk-2](assets/fig/unnamed-chunk-2-1.png) 

---

## Build and Evaluate The Model

```r
inTrain <- createDataPartition(y = dia.small$price, p = .70, list=FALSE)
dia.train <- dia.small[inTrain, ]
dia.test <- dia.small[-inTrain, ]

model.fit <- train(price ~ ., data = dia.train, method = "lm")
coefficients(model.fit$finalModel)
```

```
## (Intercept)       carat         cut       color     clarity 
##  -4669.6482   8800.4682    159.0073   -327.1080    527.6170
```

```r
#summary(model.fit$finalModel)
preds <- predict(model.fit, newdata = dia.test)
#qplot(price, preds, data = dia.test)
```


