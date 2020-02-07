



## Stochastic process

*A* stochastic process is a parametrized collection of **random variables** $\{X_t\}_{t\in T}$ defined on a probability space $(\Omega, \mathcal{F}, \mathcal{P})$ and assuming values in $\mathbb{R}^n$.

The parameter space 𝑇 is usually the halfline $[0,\infty)$ but it may also be an interval $[a,b]$, the non-negative integers, and even subsets of $\mathbb{R}^n$ for 𝑛≥1

Some examples of stochastic processes:

- White noise 
- Moving-average 
- Random walk



## Sample path

Sample path of a stochastic process is just one sample (realization) of that stochastic process (a sequence of outcomes of random variables).





## Gaussian processes

Gaussian processes are distributions over functions f(x)f(x) of which the distribution is defined by a mean function m(x)m(x) and [positive definite ](https://en.wikipedia.org/wiki/Positive-definite_matrix)covariance function k(x,x′)k(x,x′), with xx the function values and (x,x′)(x,x′) all possible pairs in the input [domain ](https://en.wikipedia.org/wiki/Domain_of_a_function):

f(x)∼GP(m(x),k(x,x′))f(x)∼GP(m(x),k(x,x′))

where for any finite subset X={x1…xn}X={x1…xn} of the domain of xx, the [marginal distribution ](https://en.wikipedia.org/wiki/Marginal_distribution)is a [multivariate Gaussian ](https://peterroelants.github.io/posts/multivariate-normal-primer/)distribution:

f(X)∼N(m(X),k(X,X))f(X)∼N(m(X),k(X,X))

with mean vector μ=m(X)μ=m(X) and covariance matrix Σ=k(X,X)Σ=k(X,X).

While the multivariate Gaussian caputures a finte number of jointly distributed Gaussians, the Gaussian process doesn't have this limitation. Its mean and covariance are defined by a [function ](https://en.wikipedia.org/wiki/Function_(mathematics)). Each input to this function is a variable correlated with the other variables in the input domain, as defined by the covariance function. Since functions can have an infinite input domain, the Gaussian process can be interpreted as an infinite dimensional Gaussian random variable.









