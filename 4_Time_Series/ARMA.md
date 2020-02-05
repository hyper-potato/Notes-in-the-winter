## Autoregressive models

$y_{t} = c + \phi_{1}y_{t-1} + \phi_{2}y_{t-2} + \dots + \phi_{p}y_{t-p} + \varepsilon_{t}$ 

where εt is white noise. This is like a multiple regression but with *lagged values* of yt as predictors. We refer to this as an **AR(p) model**, an autoregressive model of order p.

The term *auto*regression indicates that it is a regression of the variable against itself.



For an AR(1) model:

- when ϕ1=0, yt is equivalent to white noise;
- when ϕ1=1 and c=0, yt is equivalent to a random walk;
- when ϕ1=1 and c≠0, yt is equivalent to a random walk with drift;
- when ϕ1<0, yt tends to oscillate around the mean.



## Moving Average Models (MA models)

Rather than using past values of the forecast variable in a regression, a moving average model **uses past forecast errors** in a regression-like model. 

 **MA(q) model**, qth-Order Moving Average Process:

$y_{t} = \varepsilon_t - \theta_{1}\varepsilon_{t-1} - \theta_{2}\varepsilon_{t-2} + \dots - \theta_{q}\varepsilon_{t-q}$,  where $\varepsilon_t$ is white noise, θ1,θ2,...,θq are some constants.

we do not *observe* the values of $\varepsilon_t$, so it is not really a regression in the usual sense.

It is possible to write any stationary AR(p) model as an MA(∞) model. For example, using repeated substitution, we can demonstrate this for an AR(1) model:

$y_t = \phi_1y_{t-1} + \varepsilon_t\\ = \phi_1(\phi_1y_{t-2} + \varepsilon_{t-1}) + \varepsilon_t\\ = \phi_1^2y_{t-2} + \phi_1 \varepsilon_{t-1} + \varepsilon_t\\ = \phi_1^3y_{t-3} + \phi_1^2\varepsilon_{t-2} + \phi_1 \varepsilon_{t-1} + \varepsilon_t\\ \text{etc.} $

Provided −1<ϕ1<1, the value of ϕ1k will get smaller as k gets larger. So eventually we obtain $y_t = \varepsilon_t + \phi_1 \varepsilon_{t-1} + \phi_1^2 \varepsilon_{t-2} + \phi_1^3 \varepsilon_{t-3} + \cdots,$  an MA(∞) process. 



**MA model is always stationary**
$x_t = \mu + \varepsilon_t + \theta \varepsilon_{t-1}$
$E[x_t] = E[\mu] + E[\varepsilon_t] + \theta E[\varepsilon_{t-1}]$ = 0





## ARMA

## ARIMA Model Parameters

The ARIMA model includes three main parameters — *p*, *q*, and *d*. The parameters represent the following ([4](https://people.duke.edu/~rnau/411arim.htm)):

- *p*: The order of the autoregressive model (the number of lagged terms), described in the AR equation above.
- *q*: The order of the moving average model (the number of lagged terms), described in the MA equation above.
- *d*: The number of differences required to make the time series stationary.