## Seasonal ARIMA Model

The seasonal ARIMA model incorporates both non-seasonal and seasonal factors in a multiplicative model. One shorthand notation for the model is

ARIMA (p,d,q)×(P,D,Q)S

with *p* = non-seasonal AR order, *d* = non-seasonal differencing, *q* = non-seasonal MA order, *P* = seasonal AR order, *D* = seasonal differencing, *Q* = seasonal MA order, and *S* = time span of repeating seasonal pattern.

Without differencing operations, the model could be written more formally as

$\Phi(B^S)\phi(B)(x_t-\mu)=\Theta(B^S)\theta(B)w_t$

The non-seasonal components are:

- AR:  ϕ(B)=1 − ϕ1B −…− ϕpBp 
- MA:  θ(B)=1 + θ1B +… +  θqBq 

The seasonal components are:

- Seasonal AR: Φ(BS)=1 − Φ1BS−…− ΦPBPS 
- Seasonal MA: Θ(BS)=1 + Θ1BS+…+ ΘQBQS

**Note!**
On the left side of equation (1) the seasonal and non-seasonal AR components multiply each other, and on the right side of equation (1) the seasonal and non-seasonal MA components multiply each other.