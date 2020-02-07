# Correlation vs. Causation

## Threats to Causal Inference

Two clusters of threats: Selection and Endogeneity

### 1. Sample selection bias


### 2. Endogeneity

#### 2.1 Omitted variable bias (OVB)

**NOTE: $\text{Omitted variable} \neq \text{Omitted variable bias}$**

Only when the omiited variable is **correlated** with **both outcomes and treatments**, it is omitted variable **bias** that we want to avoid. There will always be **omitted variable.** 



Suppose the population regression equation is $Y = \beta_0 + \beta_{1} X +\beta_{2} Z + \epsilon$, but we have omitted variable Z when running the regression. In other words, we run a regression of Y only on X. Notice that $Y = \beta_0 + \beta_{1} X +(\beta_{2} Z + \varepsilon)$

<img src="https://i.loli.net/2020/02/05/csBqD72M89tWzpE.png" alt="image.png" style="zoom:50%;" />

A regression of Y on X effectively has an error term of $(\beta_{2} Z + \epsilon)$. Even though the error term ε may be completely exogenous, i.e., Cov(ε, X) = 0, if Z is correlated with X, then we still have $Cov(X,\beta_2{Z} + \varepsilon) \neq 0$ causing endogeneity.



The key takeaway here is that, for omitted variable to be an issue, two conditions need to be met:

1. A variable is omitted from the regression, AND,
2. This omitted variable is correlated with certain independent variable(s) included in the regression.

If the second condition is not met, then omitting the variable does not lead to endogeneity.



#### 2.2 Simultaneity bias
















#### 2.3 Measurement error

Note: Measurement error is dangerous to causal inference, but it is not necessarily true in prediction. 
In machine learning, sometimes, more variance, even noise of a variable can add more variety of the model, which can somehow make model more robust. (not generally proved)