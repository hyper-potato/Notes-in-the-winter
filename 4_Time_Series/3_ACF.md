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







## PACF

AR(1) ➡️ MA(inf)