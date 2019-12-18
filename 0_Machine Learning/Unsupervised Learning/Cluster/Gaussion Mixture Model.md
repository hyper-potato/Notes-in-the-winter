

## k-Means

prioster model

minimix rmse

## Mixture model data generation

__maximax likelyhood__



data point is density plots, which have means

assuming sample is very close to population

Goal: get probibility of each data point belonging to which group, distribution
find the right set of pi s


- question: if we have overlap, how  could sum of pi is 1?

how to evaluate how good our guess on a given point is:
P(X|C) is the likelyhood of the data

pi is the prior probibility



Gaussian Mxiture 

## expectation maximization(EM)

<img src='https://www.statisticshowto.datasciencecentral.com/wp-content/uploads/2016/05/Gaussian-mixture-example.svg_.png'>

*The EM algorithm can be used to estimate latent variables, like ones that come from mixture distributions (you know they came from a mixture, but not which specific distribution).*



<img src='https://storage.ning.com/topology/rest/1.0/file/get/1397514377?profile=original'>

The "E-Step" finds probabilities for the assignment of data points, based on a set of hypothesized probability density functions; 

The "M-Step" updates the original hypothesis with new data. The cycle repeats until the parameters stabilize.



We have yet to address the fact that **we need the parameters of each Gaussian (i.e. variance, mean and weight)** in order to cluster our data but we need to know which sample belongs to what Gaussian in order to estimate those very same parameters.

This is where ***e*xpectation maximization** comes in to play. At a high level, the expectation maximization algorithm can be described as follows:

1. Start with random Gaussian parameters (θ)
2. Repeat the following until we converge:

a) **Expectation Step**: Compute p(zi = k | xi, θ). In other words, does sample *i* look like it came from cluster k?

b) **Maximization Step**: Update the Gaussian parameters (θ) to fit points assigned to them.



![image-20191015175103769.png](https://i.loli.net/2019/11/27/HWJikKU67hZCQYI.png)



![image-20191015174544446.png](https://i.loli.net/2019/11/27/8MNkKY37lOEpC6c.png)

![image-20191015174816898.png](https://i.loli.net/2019/11/27/7Ff5E4BkCRd8gHJ.png)





Although Maximum Likelihood Estimation (MLE) and EM can both find “best-fit” parameters, *how* they find the models are very different. MLE accumulates all of the data first and then uses that data to construct the most likely model. EM takes a guess at the parameters first — accounting for the missing data — then tweaks the model to fit the guesses and the observed data. The basic steps for the algorithm are:

1. An initial guess is made for the model’s parameters and a probability distribution is created. This is sometimes called the “E-Step” for the “**E**xpected” distribution.
2. Newly observed data is fed into the model.
3. **The probability distribution from the E-step is tweaked to include the new data**. This is sometimes called the “M-step.”
4. Steps 2 through 4 are repeated until stability (i.e. a distribution that doesn’t change from the E-step to the M-step) is reached.

The EM Algorithm *always* improves a parameter’s estimation through this multi-step process. However, it sometimes needs a few random starts to find the best model because the algorithm can hone in on a [local maxima](https://www.statisticshowto.datasciencecentral.com/how-to-find-the-maximum-profit-in-calculus/) that isn’t that close to the (optimal) global maxima. In other words, it can perform better if you force it to restart and take that “initial guess” from Step 1 over again. From all of the possible parameters, you can then choose the one with the greatest maximum likelihood.

In reality, the steps involve some pretty heavy [calculus ](https://calculushowto.com/)(integration) and [conditional probabilities](https://www.statisticshowto.datasciencecentral.com/what-is-conditional-probability/), which is beyond the scope of this article. If you need a more technical (i.e. calculus-based) breakdown of the process, I highly recommend you read [Gupta and Chen’s 2010 paper.](http://www.mayagupta.org/publications/EMbookGuptaChen2010.pdf)



## Applications

The EM algorithm has many applications, including:

- Dis-entangling superimposed signals,
- Estimating Gaussian mixture models (GMMs),
- Estimating hidden Markov models (HMMs),
- Estimating parameters for compound [Dirichlet distributions](https://www.statisticshowto.datasciencecentral.com/dirichlet-distribution/),
- Finding optimal [mixtures ](https://www.statisticshowto.datasciencecentral.com/mixture-distribution/)of fixed models.





## Limitations

The EM algorithm can very very slow, even on the fastest computer. It works best when you only have a small percentage of missing data and the [dimensionality ](https://www.statisticshowto.datasciencecentral.com/dimensionality/)of the data isn’t too big. The higher the dimensionality, the slower the E-step; for data with larger dimensionaloty, you may find the E-step runs extremely slow as the procedure approaches a local maximum.



### Diff between K-mean and GMM

1. First and foremost, **k-means does not account for variance.**  By variance, we are referring to the width of the bell shape curve.

<img src="https://miro.medium.com/max/1600/0*tBFK650dBxOxqn2H.png" alt="img" style="zoom:50%;" />



2. The second difference between k-means and Gaussian mixture models is that the former performs **hard classification** whereas the latter performs **soft classification.**

3. Kmean is guaranteed to converge in a good sense;

   Although an EM iteration does increase the observed data (i.e., marginal) likelihood function, no guarantee exists that the sequence converges to a [maximum likelihood estimator](https://en.wikipedia.org/wiki/Maximum_likelihood_estimator). For [multimodal distributions](https://en.wikipedia.org/wiki/Bimodal_distribution), this means that an EM algorithm may converge to a [local maximum](https://en.wikipedia.org/wiki/Local_maximum) of the observed data likelihood function, depending on starting values.