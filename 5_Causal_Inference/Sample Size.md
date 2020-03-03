# Sample Size Calculation

## Significance / Power





## Large Samples: Too Much of a Good Thing?

Typically we don't worry about sample size being too large, because that means we have more power, we can detect smaller effect with more significance. 

It's not a problem because of technical reasons but human motivation and interpretations. 



### Warning 1: Huge Samples Can Make the Insignificant...Significant

When sample size is too large, unless the effect is truly not there, unless the coefficient is truly zero, it doesn't matter how small the beta is, we will find it significant. If we have sufficient data, even the smallest possible effect will be captured. (A insignificant result will be significant if we multiply sample size by 10).

**Risk of that:**

People put too much intention on p-value / significance (the number of stars) without and forget whether the coefficient/magnitude of coefficient is meaningful, practically significant, or **how large / small the effect is (the value of coefficient).**  It is hinten on the issue of large sample size. 

At the end of the day, when  we implement the causal inference solution, it may not improve the outcome by that much because we are chasing something mynute. 

**Solution:**

Always report not only the significance level but also **what exactly the estimate is.**



Look at the 2-sample t-test results shown below. Notice that in both Examples 1 and 2 the means and the difference between them are nearly identical. The variability (StDev) is also fairly similar. But the sample sizes and the p-values differ dramatically: 

![](https://blog.minitab.com/hubfs/Imported_Blog_Media/ttest.jpg)















