# Make Time Series Stationary

**De-trending** is fundamental. This includes regressing against covariates other than time.

**Seasonal adjustment** is a version of taking differences but could be construed as a separate technique.

**Transformation** of the data implicitly converts a difference operator into something else; e.g., differences of the logarithms are actually ratios.

Some **EDA smoothing** techniques (such as removing a moving median) could be construed as non-parametric ways of detrending. They were used as such by Tukey in his book on EDA. Tukey continued by detrending the residuals and iterating this process for as long as necessary (until he achieved residuals that appeared stationary and symmetrically distributed around zero).



## Log transformation

The logarithm ‘compresses’ larger numbers relatively more than smaller numbers, so it's useful to stabilise the variance over a time series. The box cox transformation basically does the same thing, but is simply adjustable in how strongly it stabilises the variance.



## Differencing

https://people.duke.edu/~rnau/411log.htm





## BoxCox transformation

It turns out there is something in between the log and the absolute values. 

**Def.** BoxCox transformation of time series Yt into Zt with a parameter ⁄ is

Y] l o g ( Y t ) , i f ⁄ = 0

##  Calendar adjustment

normalize it by the number of days 

