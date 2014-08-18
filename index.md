---
title       : MPG Prediction
subtitle    : Developing Data Products Course Project
author      : William Rowell
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Fuel Efficiency

Customers are very concscious with fuel efficiency, for various reasons:
- rising cost of fuel
- increasing commute times
- increasing concern for the environment

It's important to be able to predict the fuel efficiency of new cars during the
design stage. Many factors can effect the fuel efficiency, including:
- number of cylinders
- gross horsepower
- weight
- engine displacement
- rear axle ratio
- transmission type

---

## Using the mtcars dataset

The ```mtcars``` dataset can be used to build a model of the effects of car 
characteristics on fuel efficiency.  An example of the data is below:

```r
data(mtcars); library(xtable)
print(xtable(mtcars[1:8,]), type = "html")
```

<!-- html table generated in R 3.0.2 by xtable 1.7-3 package -->
<!-- Sun Aug 17 23:19:50 2014 -->
<TABLE border=1>
<TR> <TH>  </TH> <TH> mpg </TH> <TH> cyl </TH> <TH> disp </TH> <TH> hp </TH> <TH> drat </TH> <TH> wt </TH> <TH> qsec </TH> <TH> vs </TH> <TH> am </TH> <TH> gear </TH> <TH> carb </TH>  </TR>
  <TR> <TD align="right"> Mazda RX4 </TD> <TD align="right"> 21.00 </TD> <TD align="right"> 6.00 </TD> <TD align="right"> 160.00 </TD> <TD align="right"> 110.00 </TD> <TD align="right"> 3.90 </TD> <TD align="right"> 2.62 </TD> <TD align="right"> 16.46 </TD> <TD align="right"> 0.00 </TD> <TD align="right"> 1.00 </TD> <TD align="right"> 4.00 </TD> <TD align="right"> 4.00 </TD> </TR>
  <TR> <TD align="right"> Mazda RX4 Wag </TD> <TD align="right"> 21.00 </TD> <TD align="right"> 6.00 </TD> <TD align="right"> 160.00 </TD> <TD align="right"> 110.00 </TD> <TD align="right"> 3.90 </TD> <TD align="right"> 2.88 </TD> <TD align="right"> 17.02 </TD> <TD align="right"> 0.00 </TD> <TD align="right"> 1.00 </TD> <TD align="right"> 4.00 </TD> <TD align="right"> 4.00 </TD> </TR>
  <TR> <TD align="right"> Datsun 710 </TD> <TD align="right"> 22.80 </TD> <TD align="right"> 4.00 </TD> <TD align="right"> 108.00 </TD> <TD align="right"> 93.00 </TD> <TD align="right"> 3.85 </TD> <TD align="right"> 2.32 </TD> <TD align="right"> 18.61 </TD> <TD align="right"> 1.00 </TD> <TD align="right"> 1.00 </TD> <TD align="right"> 4.00 </TD> <TD align="right"> 1.00 </TD> </TR>
  <TR> <TD align="right"> Hornet 4 Drive </TD> <TD align="right"> 21.40 </TD> <TD align="right"> 6.00 </TD> <TD align="right"> 258.00 </TD> <TD align="right"> 110.00 </TD> <TD align="right"> 3.08 </TD> <TD align="right"> 3.21 </TD> <TD align="right"> 19.44 </TD> <TD align="right"> 1.00 </TD> <TD align="right"> 0.00 </TD> <TD align="right"> 3.00 </TD> <TD align="right"> 1.00 </TD> </TR>
  <TR> <TD align="right"> Hornet Sportabout </TD> <TD align="right"> 18.70 </TD> <TD align="right"> 8.00 </TD> <TD align="right"> 360.00 </TD> <TD align="right"> 175.00 </TD> <TD align="right"> 3.15 </TD> <TD align="right"> 3.44 </TD> <TD align="right"> 17.02 </TD> <TD align="right"> 0.00 </TD> <TD align="right"> 0.00 </TD> <TD align="right"> 3.00 </TD> <TD align="right"> 2.00 </TD> </TR>
  <TR> <TD align="right"> Valiant </TD> <TD align="right"> 18.10 </TD> <TD align="right"> 6.00 </TD> <TD align="right"> 225.00 </TD> <TD align="right"> 105.00 </TD> <TD align="right"> 2.76 </TD> <TD align="right"> 3.46 </TD> <TD align="right"> 20.22 </TD> <TD align="right"> 1.00 </TD> <TD align="right"> 0.00 </TD> <TD align="right"> 3.00 </TD> <TD align="right"> 1.00 </TD> </TR>
  <TR> <TD align="right"> Duster 360 </TD> <TD align="right"> 14.30 </TD> <TD align="right"> 8.00 </TD> <TD align="right"> 360.00 </TD> <TD align="right"> 245.00 </TD> <TD align="right"> 3.21 </TD> <TD align="right"> 3.57 </TD> <TD align="right"> 15.84 </TD> <TD align="right"> 0.00 </TD> <TD align="right"> 0.00 </TD> <TD align="right"> 3.00 </TD> <TD align="right"> 4.00 </TD> </TR>
  <TR> <TD align="right"> Merc 240D </TD> <TD align="right"> 24.40 </TD> <TD align="right"> 4.00 </TD> <TD align="right"> 146.70 </TD> <TD align="right"> 62.00 </TD> <TD align="right"> 3.69 </TD> <TD align="right"> 3.19 </TD> <TD align="right"> 20.00 </TD> <TD align="right"> 1.00 </TD> <TD align="right"> 0.00 </TD> <TD align="right"> 4.00 </TD> <TD align="right"> 2.00 </TD> </TR>
   </TABLE>

Henderson and Velleman (1981), Building multiple regression models interactively. *Biometrics*, 37, 391–411.

---

## Modelling predictor contribution

Using a linear model, I determined the effect of gross horsepower, number of 
cylinders, and weight on fuel efficiency (mpg), and used this model to create a
function to predict fuel efficiency based on these factors.

```r
modelFit <- lm(mpg ~ hp + cyl + wt, data=mtcars)
modelFit$coefficients
```

```
## (Intercept)          hp         cyl          wt 
##    38.75179    -0.01804    -0.94162    -3.16697
```

```r
mpg <- function(hp, cyl, wt) {
    modelFit$coefficients[1] + modelFit$coefficients[2] * hp + 
        modelFit$coefficients[3] * cyl + modelFit$coefficients[4] * wt
}
```

---

## MPG Prediction Shiny App

![MPG Prediction App](./assets/img/shinyapp.png)

The user inputs predictor values in the left sidebar, fuel effiency predictions
are output to the main panel, and plots below the prediction show how the mpg
compares with data from the mtcars dataset.
