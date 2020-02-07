https://online.stat.psu.edu/stat510/lesson/1/1.1



## Forecasting vs Prediction

A forecast refers to a calculation or an estimation which uses data from previous events, combined with recent trends to come up a future event outcome.

On the other hand, a prediction is an actual act of indicating that something will happen in the future with or without prior information. 

**Forecasting is a subset of prediction.** 

##  Stationarity

A stationary time series is one whose **properties do not depend on the time** **at which the series is observed.** Thus, time series with trends, or with seasonality, are not stationary — the trend and seasonality will affect the value of the time series at different times. On the other hand, a white noise series is stationary — it does not matter when you observe it, it should look much the same at any point in time.

![image.png](https://i.loli.net/2020/02/06/bZLh132dSYJOmnI.png)

Which of these series are stationary? 

(a) Google stock price for 200 consecutive days; 

(b) Daily change in the Google stock price for 200 consecutive days; 

(c) Annual number of strikes in the US; 

(d) Monthly sales of new one-family houses sold in the US; 

(e) Annual price of a dozen eggs in the US (constant dollars); 

(f) Monthly total of pigs slaughtered in Victoria, Australia; 

(g) Annual total of lynx trapped in the McKenzie River district of north-west Canada; 

(h) Monthly Australian beer production; 

(i) Monthly Australian electricity production.



Obvious seasonality rules out series (d), (h) and (i). 

Trends and **changing levels** rules out series (a), (c), (e), (f) and (i). 

Increasing variance also rules out (i). That leaves only (b) and (g) as stationary series.



At first glance, the strong cycles in series (g) might appear to make it non-stationary. But these cycles are aperiodic — they are caused when the lynx population becomes too large for the available feed, so that they stop breeding and the population falls to low numbers, then the regeneration of their food sources allows the population to grow again, and so on. In the long-term, the timing of these cycles is not predictable. Hence the series is stationary.



## ACF vs Stationarity

For a stationary time series, the ACF will drop to zero relatively quickly, while the ACF of non-stationary data decreases slowly. Also, for non-stationary data, the value of r1 is often large and positive.

![image.png](https://i.loli.net/2020/02/06/13SKm9wUJiLdQyP.png)

The *ACF* of the Google stock price (left) and of the *daily changes* in Google stock price (right).



The ACF of the differenced Google stock price looks just like that of a white noise series. There are no autocorrelations lying outside the 95% limits. This suggests that the *daily change* in the Google stock price is essentially a random amount which is uncorrelated with that of previous days.

## Random walk model

The differenced series is the *change* between consecutive observations in the original series, and can be written as $y'_t = y_t - y_{t-1}$. The differenced series will have only T−1 values, since it is not possible to calculate a difference  $y_1'$ for the first observation.

When the differenced series is white noise, the model for the original series can be written as $y_t - y_{t-1} = \varepsilon_t$, where εt denotes white noise. Rearranging this leads to the “random walk” model $y_t = y_{t-1} + \varepsilon_t$ 

Random walk models are widely used for non-stationary data, particularly financial and economic data. 

Random walks typically have:

- long periods of apparent trends up or down
- sudden and unpredictable changes in direction.

The forecasts from a random walk model are equal to the last observation, as future movements are unpredictable, and are equally likely to be up or down. 

A closely related model allows the differences to have a non-zero mean. Then $y_t - y_{t-1} = c + \varepsilon_t\quad\text{or}\quad {y_t = c + y_{t-1} + \varepsilon_t}\: $



The value of c is the average of the changes between consecutive observations. If c is positive, then the average change is an increase in the value of yt. Thus, yt will tend to drift upwards. However, if c is negative, yt will tend to drift downwards.

**How to control the dependency of random walk?**  

Take subtraction e(t) - e(t-1) will do the trick, but if we are almost sure the model is random walk, we get around it.





https://otexts.com/fpp2/stationarity.html#fn14



## Differencing

Note that the Google stock price was non-stationary in fig (a), but the daily changes were stationary in fig (b). This shows one way to make a non-stationary time series stationary — compute the differences between consecutive observations. This is known as **differencing**.

**Transformations** such as logarithms can help to **stabilise the variance of a time series.** 

**Differencing** can help **stabilise the mean of a time series** by removing changes in the level of a time series, and therefore **eliminating (or reducing) trend and seasonality.**

### Backshift notation

https://otexts.com/fpp2/backshift.html

The backward shift operator B is a useful notational device when working with time series lags: $B y_{t} = y_{t - 1}$.

In other words, B, operating on yt, has the effect of shifting the data back one period. Two applications of B to yt shifts the data back two periods $B(By_{t}) = B^{2}y_{t} = y_{t-2}$. For monthly data, if we wish to consider “the same month last year,” the notation is $B^{12}y_{t}$

The backward shift operator is convenient for describing the process of *differencing*. A first difference can be written as  $y'_{t} = y_{t} - y_{t-1} = y_t - By_{t} = (1 - B)y_{t}$

In general, a dth-order difference can be written as $(1 - B)^{d} y_{t}.$

Backshift notation is particularly useful when **combining differences,** as the operator can be treated using ordinary algebraic rules. In particular, terms involving B can be multiplied together. For example, a seasonal difference followed by a first difference can be written as

$\begin{align*} (1-B)(1-B^m)y_t &= (1 - B - B^m + B^{m+1})y_t \\ &= y_t-y_{t-1}-y_{t-m}+y_{t-m-1}, \end{align*}$

### Seasonal differencing

https://online.stat.psu.edu/stat510/lesson/4/4.1

Seasonal differencing is defined as a difference between a value and a value with lag that is a multiple of *S*.

- With *S* = 12, which may occur with monthly data, a seasonal difference is $\left( 1 - B ^ { 12 } \right) x _ { t } = x _ { t } - x _ { t - 12 }$

The differences (from the previous year) may be about the same for each month of the year giving us a stationary series.

- With *S* = 4, which may occur with quarterly data, a seasonal difference is $\left( 1 - B ^ { 4 } \right) x _ { t } = x _ { t } - x _ { t - 4 }$

Seasonal differencing removes seasonal trend and can also get rid of a seasonal random walk type of nonstationarity.

### Non-seasonal differencing

If trend is present in the data, we may also need non-seasonal differencing. Often (not always) a first difference (non-seasonal) will “detrend” the data. That is, we use $( 1 - B ) x _ { t } = x _ { t } - x _ { t - 1 }$ in the presence of trend.

### Differencing for Trend and Seasonality

When both trend and seasonality are present, we may need to apply both a non-seasonal first difference and a seasonal difference. That is, we may need to examine the ACF and PACF of $\left( 1 - B ^ { 12 } \right) ( 1 - B ) x _ { t } = \left( x _ { t } - x _ { t - 1 } \right) - \left( x _ { t - 12 }  - x _ { t - 13 } \right)$

Removing trend doesn't mean that we have removed the dependency. We may have removed the mean, μt, part of which may include a periodic component. In some ways we are breaking the dependency down into recent things that have happened and long-range things that have happened.

##Important Characteristics to Consider First

Some important questions to first consider when first looking at a time series are:

- Is there a **trend**, meaning that, on average, the measurements tend to increase (or decrease) over time?
- Is there **seasonality**, meaning that there is a regularly repeating pattern of highs and lows related to calendar time such as seasons, quarters, months, days of the week, and so on?
- Are there **outliers**? In regression, outliers are far away from your line. With time series data, your outliers are far away from your other data.
- Is there a **long-run cycle** or period unrelated to seasonality factors?
- Is there **constant variance** over time, or is the variance non-constant?
- Are there any **abrupt changes** to either the level of the series or the variance?





