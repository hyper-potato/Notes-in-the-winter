# Hypothesis Testing

https://online.stat.psu.edu/stat414/node/290/

We might be interested in testing whether *μ*, the mean body temperature of adults, is really 37 degrees Celsius. We'll attempt to answer such questions using a statistical method known as **hypothesis testing**. We'll derive good hypothesis tests for the usual **population parameters,** including:

- a population mean *μ*
- the difference in two population means, *μ*1*−*μ2, say
- a population variance *σ*2
- the ratio of two population variances, say, $\sigma^2_1/\sigma^2_2$



## General Logic

1. **Set up hypotheses**

2. **Assume H0 is true**

3. **P(sample result | H0 is true)**
4. **If the likelyhood is small, argues against H0, in favor of Ha**



### Set up hypotheses

- Null Hypothesis ( H0 )  (the opposite of Ha)

  The statement that there is not a difference in the population(s), denoted as H0

  NOTE: Statement of zero or no change. **The null hypothesis *always* includes the equal sign. The decision is based on the null hypothesis.**

  - If the original claim includes equality (<=, =, or >=), it is the null hypothesis. 
  - If the original claim does not include equality (<, not equal, >) then the null hypothesis is the complement of the original claim.

  

- Alternative Hypothesis (the claim we want to establish, we want to believe)

  The statement that there is some difference in the population(s), denoted as Ha or H1

  The type of test (left, right, or two-tail) is based on the alternative hypothesis.



![Screen Shot 2020-02-08 at 16.38.00.png](https://i.loli.net/2020/02/09/3kz4ORsKIx8ZoT6.png)

Type I error: Rejecting the null hypothesis when it is true (saying false when true). Usually the more serious error.

Type II error: Failing to reject the null hypothesis when it is false (saying true when false).

alpha: Probability of committing a Type I error.

beta: Probability of committing a Type II error.

Test statistic: Sample statistic used to decide whether to reject or fail to reject the null hypothesis.

Critical value(s): The value(s) which separate the critical region from the non-critical region. The critical values are determined independently of the sample statistics.

Significance level ( alpha ): The probability of rejecting the null hypothesis when it is true. alpha = 0.05 and alpha = 0.01 are common. If no level of significance is given, use alpha = 0.05. The level of significance is the complement of the level of confidence in estimation.

Decision: A statement based upon the null hypothesis. It is either "reject the null hypothesis" or "fail to reject the null hypothesis". We will never accept the null hypothesis.

Conclusion: A statement which indicates the level of evidence (sufficient or insufficient), at what level of significance, and whether the original claim is rejected (null) or supported (alternative).

