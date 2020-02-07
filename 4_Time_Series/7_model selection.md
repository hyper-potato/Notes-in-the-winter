In most cases, the best model turns out a model that uses either only AR terms or only MA terms, although in some cases a “mixed” model with both AR and MA terms may provide the best fit to the data. However, care must be exercised when fitting mixed models. It is possible for an AR term and an MA term to cancel each other’s effects, even though both may appear significant in the model (as judged by the t-statistics of their coefficients).





Stationary:

ARMA(p,q)

1. ACF: MA(q)

2. PACF: AR(q)

3.1 EACF

3.2 Auto.arima()



AIC and BIC, after taking the negative sign, the smaller, the better

AIC penelize the number of parameter

If AIC is smaller, theoritically, we dont need a test set because it is 



BIC prefers a smaller model. fewer terms, compact model