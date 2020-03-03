# Instrumental Variable

## Description

Instrumental Variables (IV) estimation is used when the model has endogenous X’s. IV can thus be used to address the following important threats to internal validity:

\1. Omitted variable bias from a variable that is correlated with X but is unobserved, so cannot be included in the regression
\2. Errors-in-variables bias (X is measured with error)
\3. Simultaneous causality bias (endogenous explanatory variables; X causes Y, Y causes X)

Instrumental variables regression can eliminate bias from these three sources

- Sources of Bias – Omitted Variable, Measurement Error, Simultaneous Relationship
  Omitted Variable
  Consider the following regression model

  ![](https://www.mailman.columbia.edu/sites/default/files/jpg/omitted.jpg)

which conforms with standard OLS assumptions. Suppose that the variable x2 is not observed. The estimated regression model is therefore

![](https://www.mailman.columbia.edu/sites/default/files/jpg/omitted2.jpg)

- where $u_i=x_{i2}+b_2+v_i$. Regressors $x_k$ in x1 are therefore correlated with the error term u if they are correlated with the omitted variable $x_2$. In case $x_{i1}$ and $x_{i2}$ are scalars, cov(xik, ui) = b2cov(xik, xi2).

- Measurement Error
  Consider the true regression model ![](https://www.mailman.columbia.edu/sites/default/files/jpg/measurement1.jpg) which conforms the standard OLS assumptions. Suppose that the variable $x^*$ only observed with an error ![](https://www.mailman.columbia.edu/sites/default/files/jpg/measurement2.jpg)  where the error v is uncorrelated with $x^*$ and with $u_i^*$. The estimated regression model uses x as a proxy for x^*
  ![img](https://www.mailman.columbia.edu/sites/default/files/jpg/measurement2.jpg)
  where $ui = u_i^*-b_1v_i$. The regressor x is therefore correlated with the error term u as both depend on v. Assuming independence between v and u*, the covariance in the above example is

  ![img](https://www.mailman.columbia.edu/sites/default/files/jpg/measurement4.jpg)

- Simultaneous Relationship
  Consider the following system of equations
  ![img](https://www.mailman.columbia.edu/sites/default/files/jpg/sem1.jpg)
  where we assume that both z1 and z2 are uncorrelated with both u1 and u2. This system is called a structural simultaneous equation system since y1 and y2 are simultenously determined. The regressor y2 depends on y1 through the second equation. As y1 is directly dependent on u1, the regressor y2 is also correlated with u1 and hence endogenous in the first equation. Assuming that u1 and u2 are uncorrelated, then
  ![img](https://www.mailman.columbia.edu/sites/default/files/jpg/sem2.jpg)
  The above equation system is also described as reversed causality because the dependent variable y1 has a feedback effect on the regressor y2. In the above example z2 and z1 are straightforward instruments for IV estimation of the first and second equation, respectively.





### Instrumental Variables: Intuition

- An instrumental variable, Z is uncorrelated with the disturbance $\varepsilon$ but is correlated with X(e.g., proximity to college might be correlated with schooling but not with wage residuals)
- With this new variable, the IV estimator should capture only the effects on Y of shifts in X induced by whereas the OLS estimator captures not only the direct effect of on but also the effect of the included measurement error and/or endogeneity
- IV is not as efficient as OLS (especially if Z only weakly correlated with X, i.e. when we have so-called ‘weak instruments’) and only has large sample properties (consistency)
- IV results in biased coefficients. The bias can be large in the case of weak instruments



### Identification and Estimation

Compliance Status from Potential Outcome Framework

- If we assume a situation where an experimenter implemented a randomized experiment where the participants are preschool children, in which the treatment is Watching Sesame Street TV Program, and the outcome of interest is score on letter recognition test
- In this experiment, watching itself cannot be randoimzed but only encouragement to watch the show can be randomly assigned
- Taking advantage of the randomization of encouragement, could esitmate a causal effect of watching for at least some of the people in the study
- As shown above in the below, the children in the trial could be categorized according to their compliance status

| Status        | Xi(1) | Xi(0) |
| ------------- | ----- | ----- |
| Always Takers | 1     | 1     |
| Never Takers  | 0     | 0     |
| Compliers     | 1     | 0     |
| Defilers      | 0     | 1     |



- Compliers are the only children for whom we will make inferences about the effect of watching Sesame Street and this effect is referred as the Complier Average Causal Effect (CACE)

### Four Major Assumptions for IV

1. Ignorability of the instrument: The instrument should be randomized or conditionally randomized with respect to the outcome and treatment variables
2. Nonzero associaiton between IV and treatment variable: The instrument must have an effect on the treatment
3. Monotonicity: Assume that there were no children who would watch if they were not encouraged but who would not watch if they were encouraged (no defiers)
4. Exclusion restriction: The instrument has no direct effect on the outcome, except indirectly through the treatment

Wald Estimator and Two-Stage Least Squares Estimator: From the Sesame Street Example

| Unit | Xi(0) | Xi(1) | Status       | Z    | Yi(0) | Yi(1) | Yi(1)-Yi(0) |
| ---- | ----- | ----- | ------------ | ---- | ----- | ----- | ----------- |
| 1    | 0     | 1     | Complier     | 0    | 67    | 76    | 9           |
| 2    | 0     | 1     | Complier     | 0    | 72    | 80    | 8           |
| 3    | 0     | 0     | Never Taker  | 0    | 68    | 68    | 0           |
| 4    | 1     | 1     | Always Taker | 0    | 76    | 76    | 0           |
| 5    | 1     | 1     | Always Taker | 0    | 74    | 74    | 0           |
| 6    | 0     | 1     | Complier     | 1    | 67    | 76    | 9           |
| 7    | 0     | 1     | Complier     | 1    | 72    | 80    | 8           |
| 8    | 0     | 0     | Never Taker  | 1    | 68    | 68    | 0           |
| 9    | 1     | 1     | Always Taker | 1    | 76    | 76    | 0           |
| 10   | 1     | 1     | Always Taker | 1    | 74    | 74    | 0           |

The Intent-to-treat effect (ITT) in the hypothetical table above for the 10 observations is an average of the effects for the 4 induced watchers, along with 6 zeros corresponding to the encouragement effects for the always takers and never takers:

ITT = (9+8+0+0+0+9+8+0+0+0)/10 = 8.5*(4/10) + 0*(6/10) = 3.4

The effect of watching Sesame Street for the complier is 8.5 points and this is algebraically the same as the intent-to-treat effect (3.4) divided by the proportion of compliers (4/10). This ratio is called the Wald estimate. 

But two-stage least squares is a more general estimation strategy with a regression framework, which allows for controlling covariates. And the required steps are follows:

– Regress the treatment variable on the randomized instrument
– Plug predicted values into the equation predicting the outcome

### Some Issues for IV

Where do Valid Instruments Come from?

- Valid instruments are 1) relevant and 2) exogenous
- One general way to find instruments is to look for exogenous variation – variation that is ‘as if’ randomly assigned in a randomized experiment – that affects.
  – Rainfall shifts the supply curve for butter but not the demand curve; rainfall is ‘as if’ randomly assigned
  – Sales tax shifts the supply curve for cigarettes but not the demand curve; sales taxes are ‘as if’ randomly assigned

Weak Instruments

- If cov(Z,X) is weak, IV no longer has such desirable asymptotic properties
- IV estimates are not unbiased, and the bias tends to be larger when instruments are weak (even with very large datasets)
- Weak instruments tend to bias the results towards the OLS estimates
- Adding more and more instruments to improve asymptotic efficiency does not solve the problem
- Recommendation always test the ‘strength’ of your instrument(s) by reporting the F-test on the instruments in the first stage regression

## Summary

- A valid instrument lets us isolate a part of X that is uncorrelated with , and that part can be used to estimate the effect of a change in X on Y
- IV hinges on having valid instruments: A valid instrument isolates variation in that is ‘as if’ randomly assigned