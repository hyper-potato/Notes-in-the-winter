## Autoregressive models (AR)

**AR(1) ➡️ MA(inf)**



$y_{t} = c + \phi_{1}y_{t-1} + \phi_{2}y_{t-2} + \dots + \phi_{p}y_{t-p} + \varepsilon_{t}$ 

where εt is white noise. This is like a multiple regression but with *lagged values* of yt as predictors. We refer to this as an **AR(p) model**, an autoregressive model of order p.

The term *auto*regression indicates that it is a regression of the variable against itself.



The ACF property defines a distinct pattern for the autocorrelations. For a positive value of ϕ1, the ACF exponentially decreases to 0 as the lag h increases. For negative ϕ1, the ACF also exponentially decays to 0 as the lag increases, but the algebraic signs for the autocorrelations alternate between positive and negative.

Following is the ACF of an AR(1) with ϕ1= 0.8, for the first 20 lags.

![image.png](https://i.loli.net/2020/02/06/nO7NHJabIsWZhFB.png)



The ACF of an AR(1) with ϕ1 = −0.9 follows. Note The alternating and tapering pattern.

![image.png](https://i.loli.net/2020/02/06/L26zkeUGDPA3jF9.png)



### Stationarity conditions

![image.png](https://i.loli.net/2020/02/06/rUqiE4JBOSKC6Vm.png)

![image.png](https://i.loli.net/2020/02/06/gchoKsdfuYbNOQX.png)



![image.png](https://i.loli.net/2020/02/06/BI5WEC8f6xpPF43.png)

### AR(1)

Consider the white noise $e_t$ and let’s define the following process:

$y_t = \phi_1y_{t-1}+e_t$ where $\phi_1$ is some constant such that $|\phi_1|$ < 1

- when ϕ1=0, yt is equivalent to white noise;
- when ϕ1=1 and c=0, yt is equivalent to a random walk;
- when ϕ1=1 and c≠0, yt is equivalent to a random walk with drift;
- when ϕ1<0, yt tends to oscillate around the mean.

#### Properties

- The (theoretical) **mean** of $y_t$ is E(xt) = μ = δ1−ϕ1

$E(y_t) = E(\phi_1y_{t-1}+e_t) = E(\phi_1y_{t-1}) + E(e_t) = \phi_1E(y_{t-1}) + 0$

With the stationary assumption, E(xt)=E(xt−1). Let μ denote this common mean. Thus $\mu = \delta + \phi_1\mu$ . Solve for μ to get $\mu = \dfrac{\delta}{1-\phi_1}$





- **variance**

  By independence of errors and values of y,

  $\text{Var}(x_t) = \text{Var}(\delta)+\text{Var}(\phi_1 x_{t-1})+\text{Var}(w_t)    \nonumber \\ =\phi_1^2 \text{Var}(x_{t-1})+\sigma^2_w$



By the stationary assumption, $\text{Var}(y_t) = \text{Var}(y_{t-1})$. Substitute $\text{Var}(y_t) = \text{Var}(y_{t-1})$ and then solve for Var(yt). Because Var(yt)>0, it follows that $(1-\phi^2_1)>0$ and therefore |ϕ1|<1.



Var(xt)=Var(δ)+Var(ϕ1xt−1)+Var(wt)=ϕ12Var(xt−1)+σw2



By the stationary assumption, Var(xt)=Var(xt−1). Substitute Var(xt) for Var(xt−1) and then solve for Var(xt). Because Var(xt)>0, it follows that (1−ϕ12)>0 and therefore |ϕ1|<1.

- The **correlation** between observations h time periods apart is

ρh=ϕ1h

This defines the theoretical ACF for a time series variable with an AR(1) model.

## Moving Average Models (MA models)

Rather than using past values of the forecast variable in a regression, a moving average model **uses past forecast errors** in a regression-like model. 

**MA(q) model**, qth-Order Moving Average Process:

$y_{t} = \varepsilon_t - \theta_{1}\varepsilon_{t-1} - \theta_{2}\varepsilon_{t-2} + \dots - \theta_{q}\varepsilon_{t-q}$,  where $\varepsilon_t$ is white noise, θ1,θ2,...,θq are some constants.

we do not *observe* the values of $\varepsilon_t$, so it is not really a regression in the usual sense.

It is possible to write any stationary AR(p) model as an MA(∞) model. For example, using repeated substitution, we can demonstrate this for an AR(1) model:

$y_t = \phi_1y_{t-1} + \varepsilon_t\\ = \phi_1(\phi_1y_{t-2} + \varepsilon_{t-1}) + \varepsilon_t\\ = \phi_1^2y_{t-2} + \phi_1 \varepsilon_{t-1} + \varepsilon_t\\ = \phi_1^3y_{t-3} + \phi_1^2\varepsilon_{t-2} + \phi_1 \varepsilon_{t-1} + \varepsilon_t\\ \text{etc.} $

Provided −1<ϕ1<1, the value of ϕ1k will get smaller as k gets larger. So eventually we obtain $y_t = \varepsilon_t + \phi_1 \varepsilon_{t-1} + \phi_1^2 \varepsilon_{t-2} + \phi_1^3 \varepsilon_{t-3} + \cdots,$  an MA(∞) process. 

### Mean

**MA model is always stationary**
$x_t = \mu + \varepsilon_t + \theta \varepsilon_{t-1}$
$E[x_t] = E[\mu] + E[\varepsilon_t] + \theta E[\varepsilon_{t-1}]$ = 0

### Variance

We have $\operatorname E X^2=\operatorname{Var}X=\sigma^2$ since E𝑋=0

$\operatorname{Var}X^2=\operatorname EX^4-(\operatorname EX^2)^2.$

For any random variable 𝑋 with a finite second moment,

$\operatorname{Var}X=\operatorname EX^2-(\operatorname EX)^2$, so that $\operatorname{Var}X=\operatorname EX^2$ if E𝑋=0.

In our case, E𝑋2=Var𝑋=1



### ACF 

MA(1) model first bar very high; MA(2) first two very high

![image.png](https://i.loli.net/2020/02/06/isBlGLfEda74ycH.png)

<img src="https://i.loli.net/2020/02/06/39DnQzEsdLpw7gJ.png" alt="image.png" style="zoom:50%;" />

**If we see something like this, be caution that it is not about counting bars anymore, this is not an MA model, it has longterm dependency. We dont feed in something like MA10 model, even though the result might be significant. Too much to pay for (Number of parameters and dependency). There could be false positive.** 





## ARMA

## ARIMA Model Parameters

The ARIMA model includes three main parameters — *p*, *q*, and *d*. The parameters represent the following ([4](https://people.duke.edu/~rnau/411arim.htm)):

- *p*: The order of the autoregressive model (the number of lagged terms), described in the AR equation above.
- *q*: The order of the moving average model (the number of lagged terms), described in the MA equation above.
- *d*: The number of differences required to make the time series stationary.