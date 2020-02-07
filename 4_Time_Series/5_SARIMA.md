# Seasonal ARIMA Model

The seasonal ARIMA model incorporates both non-seasonal and seasonal factors in a multiplicative model. One shorthand notation for the model is

ARIMA $(p, d, q) \times (P, D, Q)S$

- Trend Elements: *p* = non-seasonal AR order, *d* = non-seasonal differencing, *q* = non-seasonal MA order
- Seasonal Elements: *P* = seasonal AR order, *D* = seasonal differencing, *Q* = seasonal MA order, and *S* = time span of repeating seasonal pattern

For example, an ARIMA(1,1,1)(1,1,1)4 model (without a constant) is for quarterly data (m=4), and can be written as

$(1 - \phi_{1}B)~(1 - \Phi_{1}B^{4}) (1 - B) (1 - B^{4})y_{t} =  (1 + \theta_{1}B)~ (1 + \Theta_{1}B^{4})\varepsilon_{t}.$



Without differencing operations, the model could be written more formally as

$\Phi(B^S)\phi(B)(x_t-\mu)=\Theta(B^S)\theta(B)w_t$

The non-seasonal components are:

- AR:  ϕ(B)=1 − ϕ1B −…− ϕpBp 
- MA:  θ(B)=1 + θ1B +… +  θqBq 

The seasonal components are:

- Seasonal AR: Φ(BS)=1 − Φ1BS−…− ΦPBPS 
- Seasonal MA: Θ(BS)=1 + Θ1BS+…+ ΘQBQS

Note: On the left side of equation (1) the seasonal and non-seasonal AR components multiply each other, and on the right side of equation (1) the seasonal and non-seasonal MA components multiply each other.