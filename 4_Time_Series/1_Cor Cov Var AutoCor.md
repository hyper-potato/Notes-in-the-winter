# Variance, Covariance and Correlation

## Variance - Squaring Expectations to Measure Change

$Var(X) = E(X^2) - E(X)^2$

*Variance is the difference between when we square the inputs to Expectation and when we square the Expectation itself.*

![](https://miro.medium.com/max/2560/1*TzQSHnkAazmLxnZkilORlQ.jpeg)

Now as we see, in the image above, ‘s²’ or sampled variance is basically the covariance of a variable with itself. This term can also be defined in the following manner:

![](https://miro.medium.com/max/2560/1*bgQq0eahJWAbTdahIcxMLA.jpeg)

The value (n-1) indicates the degrees of freedom.



## Covariance - Measuring the Variance between Two Variables

The covariance is a measure for how two variables are related to each other, i.e., how two variables vary with each other.

The **covariance** between X and Y is defined as

$Cov(X,Y) = E[(X−EX)(Y−EY)]= E[XY]−(EX)(EY)$

**Note** (The proof bugged me a little)

$\begin{align}\label{} \nonumber  E\big[(X-EX)(Y-EY)\big]&=E\big[XY-X(EY)-(EX)Y+(EX)(EY)\big]\\ \nonumber  &=E[XY]-(EX)(EY)-(EX)(EY)+(EX)(EY)\\ \nonumber  &=E[XY]-(EX)(EY) \end{align}$

Intuitively, the covariance between X and Y indicates how the values of X and Y move relative to each other. If large values of X tend to happen with large values of Y, then (X−EX)(Y−EY)  is positive on average. In this case, the covariance is positive and we say X and Y are positively correlated. On the other hand, if X tends to be small when Y is large, then (X−EX)(Y−EY) is negative on average. In this case, the covariance is negative and we say X and Y are negatively correlated.



**Properties**

Say that X and Y are arbitrary random variables:

1. $\textrm{Cov}(X,X)=\textrm{Var}(X)$

   <img src="https://i.loli.net/2020/01/24/qY5xH7nOWoGTw2a.png" alt="image.png" style="zoom:50%;" />

2. If X and Y are independent, then $\textrm{Cov}(X,Y)=0$

   Note that the converse is not necessarily true. That is, if Cov(X,Y)=0, X and Y may or may not be independent. 

3. $\textrm{Cov}(X,Y) =\textrm{Cov}(Y,X)$

4. $\textrm{Cov}(aX,Y)=a\textrm{Cov}(X,Y)$

5. $\textrm{Cov}(X+c,Y)=\textrm{Cov}(X,Y)$

6. $\textrm{Cov}(X+Y,Z)=\textrm{Cov}(X,Z)+\textrm{Cov}(Y,Z)$

   $\begin{align}\label{} \nonumber  \textrm{Cov}(X+Y,Z)&=E[(X+Y)Z]-E(X+Y)EZ\\ \nonumber  &=E[XZ+YZ]-(EX+EY)EZ\\ \nonumber  &=EXZ-EXEZ+EYZ-EYEZ\\ \nonumber  &=\textrm{Cov}(X,Z)+\textrm{Cov}(Y,Z). \end{align}$

7. More generally, 

   $\begin{align}\label{}  \nonumber  \textrm{Cov}\left(\sum_{i=1}^{m}a_iX_i, \sum_{j=1}^{n}b_jY_j\right)=\sum_{i=1}^{m} \sum_{j=1}^{n} a_ib_j \textrm{Cov}(X_i,Y_j).  \end{align}$

![image.png](https://i.loli.net/2020/01/24/KD3y81kq2SOnFsa.png)



### Variance of a sum:

One of the applications of covariance is finding the variance of a sum of several random variables. In particular, if Z=X+Y, then

$\begin{align}\label{} \nonumber  \textrm{Var}(Z)&=\textrm{Cov}(Z,Z)\\ \nonumber &=\textrm{Cov}(X+Y,X+Y)\\ \nonumber  &=\textrm{Cov}(X,X)+\textrm{Cov}(X,Y)+ \textrm{Cov}(Y,X)+\textrm{Cov}(Y,Y)\\ \nonumber  &=\textrm{Var}(X)+\textrm{Var}(Y)+2 \textrm{Cov}(X,Y). \end{align}$

More generally, for $a,b \in \mathbb{R}$

$\textrm{Var}(aX+bY)=a^2\textrm{Var}(X)+b^2\textrm{Var}(Y)+2ab \textrm{Cov}(X,Y) \hspace{20pt} (5.21) $



## Correlation - Normalizing the Covariance

The correlation, denoted by $\textrm{Corr}(X,Y)$,  $\rho_{XY}$ or  $\rho{(X,Y)}$ is obtained by normalizing the covariance. In particular, we define the correlation of two random variables X and Y as the covariance of the standardized versions of X and Y. 

Define the standardized versions of X and Y as 

$$U=\frac{X-EX}{\sigma_X}, \hspace{12pt} V=\frac{Y-EY}{\sigma_Y} \hspace{12pt}$$

Then

$\begin{align}\label{} \nonumber \rho_{XY}=\textrm{Cov}(U,V)&=\textrm{Cov}\left(\frac{X-EX}{\sigma_X},\frac{Y-EY}{\sigma_Y}\right)\\ \nonumber &=\textrm{Cov}\left(\frac{X}{\sigma_X},\frac{Y}{\sigma_Y}\right) &(\textrm{by Item 5 of Lemma 5.3})\\ \nonumber &=\frac{\textrm{Cov}(X,Y)}{\sigma_X \sigma_Y}. \end{align}$



$\begin{align}\label{} \nonumber \textrm{Corr}(X,Y) = \rho_{XY}=\rho(X,Y)=\frac{\textrm{Cov}(X,Y)}{\sqrt{\textrm{Var(X) Var(Y)}}}=\frac{\textrm{Cov}(X,Y)}{\sigma_X \sigma_Y} \end{align}$



A nice thing about the correlation is that it is always between −1 and 1. 



1. $-1 \leq \rho(X,Y) \leq 1$
2. If $\rho(X,Y) = 1$, then $Y=aX+b$, where a > 0
3. If $\rho(X,Y) = -1$, then $Y=aX+b$, where a < 0
4. $\rho(aX+b,cY+d)=\rho(X,Y)$, for a, c>0



**Definition** 
Consider two random variables X and Y:
\- If $\rho(X,Y)=0$, we say that X and Y are **uncorrelated**.
\- If $$\rho(X,Y)>0$$, we say that X and Y are **positively** correlated.
\- If $$\rho(X,Y)<0$$, we say that X and Y are **negatively** correlated.

Note that as we discussed previously, two independent random variables are always uncorrelated, but the converse is not necessarily true. That is, **if X and Y are uncorrelated, then X and Y may or may not be independent**. Also, note that if X and Y are uncorrelated from Equation 5.21, we conclude that $\textrm{Var}(X+Y)=\textrm{Var}(X)+\textrm{Var}(Y)$

<img src="https://i.loli.net/2020/01/24/ViwvCXpxbo5ZKOI.png" alt="image.png" style="zoom:50%;" />





## Autocorrelation

Let $x_t$ denote the value of a time series at time t. The ACF of the series gives correlations between  $x_t$  and  $x_{t-h}$  for h = 1, 2, 3, etc. Theoretically, the autocorrelation between xt and xt−h equals

$\dfrac{\text{Covariance}(x_t, x_{t-h})}{\text{Std.Dev.}(x_t)\text{Std.Dev.}(x_{t-h})} = \dfrac{\text{Covariance}(x_t, x_{t-h})}{\text{Variance}(x_t)}$

The denominator in the second formula occurs because the standard deviation of a stationary series is the same at all times.

Given measurements, *Y*1, *Y*2, ..., *YN* at time *X*1, *X*2, ..., *XN*, the lag *k* autocorrelation function is defined as:

$r_{k} = \frac{\sum_{i=1}^{N-k}(Y_{i} - \bar{Y})(Y_{i+k} -          \bar{Y})} {\sum_{i=1}^{N}(Y_{i} - \bar{Y})^{2} }$

Autocorrelation can be used for the following two purposes:

1. To detect non-randomness in data.
2. To identify an appropriate time series model if the data are not random.

Autocorrelation is a correlation coefficient. However, instead of correlation between two different variables, the correlation is between two values of the same variable at times **Xi** and **Xi+k**.

When the autocorrelation is used to detect non-randomness, it is **usually only the first (lag 1) autocorrelation** that is of interest. When the autocorrelation is used to identify an appropriate time series model, the autocorrelations are usually [plotted](https://www.itl.nist.gov/div898/handbook/eda/section3/autocopl.htm) for many lags.






https://www.probabilitycourse.com/chapter5/5_3_1_covariance_correlation.php

https://www.countbayesie.com/blog/2015/2/21/variance-co-variance-and-correlation

https://www.itl.nist.gov/div898/handbook/eda/section3/eda35c.htm