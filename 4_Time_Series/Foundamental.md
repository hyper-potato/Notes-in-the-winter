https://online.stat.psu.edu/stat510/lesson/1/1.1



1. Important Characteristics t

   Some important questions to first consider when first looking at a time series are:

   - Is there a **trend**, meaning that, on average, the measurements tend to increase (or decrease) over time?

   - Is there **seasonality**, meaning that there is a regularly repeating pattern of highs and lows related to calendar time such as seasons, quarters, months, days of the week, and so on?

   - Are there **outliers**? In regression, outliers are far away from your line. With time series data, your outliers are far away from your other data.

   - Is there a **long-run cycle** or period unrelated to seasonality factors?

   - Is there **constant variance** over time, or is the variance non-constant?

   - Are there any **abrupt changes** to either the level of the series or the variance?

     

1. Forecasting vs Prediction

A forecast refers to a calculation or an estimation which uses data from previous events, combined with recent trends to come up a future event outcome.

On the other hand, a prediction is an actual act of indicating that something will happen in the future with or without prior information. 

**Forecasting is a subset of prediction.** 



2. Seasonality, trend
3. Stochastic process: a series of populations

4. random walk
5. **distinguish linear trend model from random walk**

How to control the dependency of random walk?  

Take subtraction e(t) - e(t-1) will do the trick, but if we are almost sure the model is random walk, we get around it.



6. **Stationary** vs trend and residual

    A *stationary* time series is one whose statistical properties such as mean, variance, autocorrelation, etc. are all constant over time. 

   - Expected mean is of the time series is constant 
   - Standard deviation of mean, $\sigma$ is also constant
   - no seasonality (periodical behavior over time)

7. Making a time series stationary

   



6. For random walk, 
   1. covriance is not stationary
   2. Mean is always ZERO