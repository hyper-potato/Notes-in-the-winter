# Bagging and Pasting

## Motivation

(from textbook)

Ensemble methods work best when the predictors are as independent from one another as possible. 

- One way to get diverse classifiers is to train them using very different algorithms. This increases the chance that they will make very different types of errors, improving the ensemble’s accuracy.

- Another approach is to __use the same training algorithm for every predictor__, but to train them on **different random subsets of the training set**. 

  When sampling is performed *with* replacement, this method is called *bagging*1 (short for*bootstrap aggregating*). When sampling is performed *without* replacement, it is called *pasting*. In other words, both bagging and pasting allow training instances to be sampled several times across multiple predictors, but **only bagging allows training instances to be sampled several times for the same predictor.** 

Ensemble has a similar bias but a lower variance than a single predictor trained on the original training set.



## Setting

when n goes infinity, only 63% data from original data will show in a sample, which ensures the diversity of sub models

(1-1/N)^N







Bootstrapping introduces a bit more diversity in the subsets that each predictor is trained on, so bagging ends up with a slightly **higher bias than pasting**, but this also means that predictors end up being **less** correlated so the ensemble’s **variance** is reduced. 

## Bagging Reduces Variance

Bias / Variance decomposition:

$$
\ \underbrace{\mathbb{E}[(h_D(x) - y)^2]}_\mathrm{Error} = \underbrace{\mathbb{E}[(h_D(x)-\bar{h}(x))^2]}_\mathrm{Variance} + \underbrace{\mathbb{E}[(\bar{h}(x)-\bar{y}(x))^2]}_\mathrm{Bias} + \underbrace{\mathbb{E}[(\bar{y}(x)-y(x))^2]}_\mathrm{Noise} 
$$

  

- $h_D(x)$: classifier i learn
- $\bar{h}(x)$: expected classifier if they integrated out D

if variance problem, means variance term is large

Our goal is to reduce the variance term: $\mathbb{E}[(h_D(x)-\bar{h}(x))^2]$. For this, we want $h_D \to \bar{h}$

### Weak law of large numbers

The weak law of large numbers says (roughly) for i.i.d. (Independent and identically distributed random variables) random variables xi with mean $\bar{x}$, we have,

$$\frac{1}{m}\sum_{i = 1}^{m}x_i \rightarrow \bar{x} \textrm{ as }  m\rightarrow \infty$$

Apply this to classifiers: Assume we have m training sets  drawn from $P^n$. Train a classifier on each one and average result:

$\hat{h} = \frac{1}{m}\sum_{i = 1}^m h_{D_i} \to \bar{h} \qquad as\  m \to \infty$

We refer to such an average of multiple classifiers as an **ensemble**  of classifiers.

<u>Good news</u>: If h^→h¯, the variance component of the error must also vanish, *i.e.* $\mathbb{E}[(h_D(x)-\bar{h}(x))^2]$ →0
<u>Problem:</u> We don't have m data sets $D_1, ...., D_m$, we only have D.

### Solution: Bagging (Bootstrap Aggregating)

Simulate drawing from P by drawing uniformly with replacement from the set D.
i.e. let Q(X, Y|D) be a probability distribution that picks a training sample (xi,yi)from D uniformly at random. More formally, $Q((\mathbf{x_i}, y_i)|D) = \frac{1}{n} \qquad\forall (\mathbf{x_i}, y_i)\in D$ , with n = |D|  
We sample the set $D_i\sim Q^n$, i.e. |Di|=n, and Di is picked with replacement from Q|D

**Q**: What is $\mathbb{E}[|D\cap D_i|]$

Bagged classifier: $\hat{h}_D = \frac{1}{m}\sum_{i = 1}^{m}h_{D_i}$

Notice: (cannot use W.L.L.N here, W.L.L.N only works for i.i.d. samples). However, in practice bagging still reduces variance very effectively.  



#### Bagging summarized

1. Sample mm data sets $D_1,\dots,D_m$ from D with replacement
2. For each Dj train a classifier $h_j()$
3. The final classifier is $h(\mathbf{x})=\frac{1}{m}\sum_{j=1}^m h_j(\mathbf{x})$



## Advantages of Bagging

- Easy to implement

- Reduces variance, so has a strong beneficial effect on high variance classifiers.

- As the prediction is an average of many classifiers, you obtain a mean score *and variance*. Latter can be interpreted as the uncertainty of the prediction. Especially in regression tasks, such uncertainties are otherwise hard to obtain. For example, imagine the prediction of a house price is \$300,000. If a buyer wants to decide how much to offer, it would be very valuable to know if this prediction has standard deviation +-\$10,000 or +-$50,000

- Bagging provides an <u>unbiased</u> estimate of the test error, which we refer to as the *out-of-bag error*. The idea is that each training point was not picked and all the data sets Dk. If we average the classifiers $h_k$ of all such data sets, we obtain a classifier $h_k$ (with a slightly smaller m) that was not trained on (xi,yi) ever and it is therefore equivalent to a test sample. If we compute the error of all these classifiers, we obtain an estimate of the true test error. The beauty is that we can do this without reducing the training set. We just run bagging as it is intended and obtain this so called out-of-bag error for free.

  More formally, for each training point (xi,yi)∈D, let $S_i=\{k| (\mathbf{x}_i,y_i)\notin D_k\}$ in other words Si is a set of all the training sets Dk, which do not contain (xk,yk). Let the averaged classifier over all these data sets be

  $$\tilde h_i(\mathbf{x})=\frac{1}{|S_i|}\sum_{k\in S_i}h_k(\mathbf{x})$$

  The out-of-bag error becomes simply the average error/loss that all these classifiers yield

  $$\epsilon_\mathrm{OOB}=\frac{1}{n}\sum_{(\mathbf{x}_i, y_i) \in D}l(\tilde h_i(\mathbf{x_i}),y_i).$$

  

  This is an estimate of the test error, because for each training point we used the subset of classifiers that never saw that training point during training. if m is sufficiently large, the fact that we take out some classifiers has no significant effect and the estimate is pretty reliable.

  

- 

















