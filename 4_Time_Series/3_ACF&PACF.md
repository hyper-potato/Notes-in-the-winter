# ACF & PACF

## How to use the Autocorreation Function (ACF)?

It is used to determine stationarity and seasonality.

Stationarity:

This refers to **whether the series is “going anywhere”** over time. Stationary series have a constant value over time.

Below is what a non-stationary series looks like. Note the changing mean.
<img src='https://coolstatsblog.files.wordpress.com/2013/08/j1.png'>And below is what a stationary series looks like. 
<img src='https://coolstatsblog.files.wordpress.com/2013/08/k.png'>

This is the first difference of the above series,

The above time series provide strong indications of (non) stationary, but the ACF helps us ascertain this indication.

If a series is non-stationary (moving), its ACF may look a little like this:
<img src='https://coolstatsblog.files.wordpress.com/2013/08/l.png?w=596&h=444'>
ACF of non-stationary series

The above ACF is “decaying”, or decreasing, very slowly, and remains well above the significance range (dotted blue lines). **This is indicative of a non-stationary series.**

On the other hand, observe the ACF of a stationary (not going anywhere) series:
<img src='https://coolstatsblog.files.wordpress.com/2013/08/m.png?w=300&h=104&zoom=2'>
ACF of stationary series

Note that the ACF shows exponential decay. This is indicative of a stationary series.
Consider the case of a simple stationary series, like the process shown below:
$Y_t = \epsilon_t$

We do not expect the ACF to be above the significance range for lags 1, 2, … This is intuitively satisfactory, because the above  process is purely random, and therefore whether you are looking at a lag of 1 or a lag of 20, the correlation should be theoretically zero, or at least insignificant.

https://coolstatsblog.com/2013/08/07/how-to-use-the-autocorreation-function-acf/







## Partial Autocorrelation Function (PACF)

为什么需要偏自相关系数？

在AR(1)模型中，**即使y(t-2)没有直接出现在模型中，但是y(t)和y(t-2)之间也相关，**

偏相关系数是在排除了其他变量的影响之后两个变量之间的相关系数。

证明：

![](https://images2015.cnblogs.com/blog/917868/201604/917868-20160419231950788-179235816.png)

In general, a partial correlation is a conditional correlation. It is the correlation between two variables under the assumption that we know and take into account the values of some other set of variables. For instance, consider a regression context in which *y* is the response variable and x1, x2, and x3 are predictor variables. The partial correlation between *y* and x3 is the correlation between the variables determined taking into account how both *y* and x3 are related to x1 and x2.

In regression, this partial correlation could be found by correlating the residuals from two different regressions:

1. Regression in which we predict *y* from x1 and x2,
2. regression in which we predict x3 from x1 and x2. Basically, we correlate the “parts” of *y* and x3 that are not predicted by x1 and x2.

More formally, we can define the partial correlation just described as

$\dfrac{\text{Covariance}(y, x_3|x_1, x_2)}{\sqrt{\text{Variance}(y|x_1, x_2)\text{Variance}(x_3| x_1, x_2)}}$



**Note!**
That this is also how the parameters of a regression model are interpreted. Think about the difference between interpreting the regression models:

$y = \beta_0 + \beta_1x^2 \text{ and } y = \beta_0+\beta_1x+\beta_2x^2$

In the first model, β1 can be interpreted as the linear dependency between $x^2$ and *y*. In the second model, β2 would be interpreted as the linear dependency between $x^2$ and *y* **WITH** the dependency between *x* and *y* already accounted for.

For a time series, the partial autocorrelation between xt and $x_{t−h}$ is defined as the conditional correlation between xt and $x_{t−h}$, conditional on $x_{t−h+1}$, ... ,$x_{t−1}$, the set of observations that come between the time points t and t−h.

- The 1st order partial autocorrelation will be defined to equal the 1st order autocorrelation.
- The 2nd order (lag) partial autocorrelation is

$\dfrac{\text{Covariance}(x_t, x_{t-2}| x_{t-1})}{\sqrt{\text{Variance}(x_t|x_{t-1})\text{Variance}(x_{t-2}|x_{t-1})}}$

This is the correlation between values two time periods **apart conditional on knowledge of the value in between.** (By the way, the two variances in the denominator will equal each other in a stationary series.)

- The 3rd order (lag) partial autocorrelation is

$\dfrac{\text{Covariance}(x_t, x_{t-3}| x_{t-1}, x_{t-2})}{\sqrt{\text{Variance}(x_t|x_{t-1},x_{t-2})\text{Variance}(x_{t-3}|x_{t-1},x_{t-2})}}$

And, so on, for any lag.

Typically, matrix manipulations having to do with the covariance matrix of a multivariate distribution are used to determine estimates of the partial autocorrelations.



## Some Useful Facts About PACF and ACF Patterns

**Identification of an AR model is often best done with the PACF.**

- For an AR model, the theoretical PACF “shuts off” past the order of the model. The phrase “shuts off” means that in theory the partial autocorrelations are equal to 0 beyond that point. Put another way, the number of non-zero partial autocorrelations gives the order of the AR model. By the “order of the model” we mean the most extreme lag of *x* that is used as a predictor.

**Example**: In [Lesson 1.2](https://online.stat.psu.edu/stat510/lesson/1/1.2), we identified an AR(1) model for a time series of annual numbers of worldwide earthquakes having a seismic magnitude greater than 7.0. Following is the sample PACF for this series. Note that the first lag value is statistically significant, whereas partial autocorrelations for all other lags are not statistically significant. This suggests a possible AR(1) model for these data.

![graph](https://online.stat.psu.edu/onlinecourses/sites/stat510/files/L02/graph_25.gif)

**Identification of an MA model is often best done with the ACF rather than the PACF.**

For an MA model, the theoretical PACF does not shut off, but instead tapers toward 0 in some manner. A clearer pattern for an MA model is in the ACF. The ACF will have non-zero autocorrelations only at lags involved in the model.

The following sample ACF for a simulated MA(1) series. Note that the first lag autocorrelation is statistically significant whereas all subsequent autocorrelations are not. This suggests a possible MA(1) model for the data.

**Theory Note!**
The model used for the simulation was $x_t=10+w_t+0.7w_{t-1}$. In theory, the first lag autocorrelation $\theta_1 / (1+\theta_1^2) = .7/(1+.7^2) = .4698$ and autocorrelations for all other lags = 0.

![graph](https://online.stat.psu.edu/onlinecourses/sites/stat510/files/L02/graph_26.gif)

The underlying model used for the MA(1) simulation in [Lesson 2.1](https://online.stat.psu.edu/stat510/lesson/2/2.1) was xt=10+wt+0.7wt−1. Following is the theoretical PACF (partial autocorrelation) for that model. Note that the pattern gradually tapers to 0.

![graph](https://online.stat.psu.edu/onlinecourses/sites/stat510/files/L02/graph_27.gif)


The PACF just shown was created in R with these two commands:

```r
ma1pacf = ARMAacf(ma = c(.7),lag.max = 36, pacf=TRUE)
plot(ma1pacf,type="h", main = "Theoretical PACF of MA(1) with theta = 0.7") 
```

------

![](https://images2015.cnblogs.com/blog/917868/201604/917868-20160419231959976-2031808557.png)

Links:

1. https://online.stat.psu.edu/stat510/lesson/1/1.2
2. https://online.stat.psu.edu/stat510/lesson/2/2.1