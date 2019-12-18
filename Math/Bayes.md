# Bayes Thinking

I never liked Bayes, Bayesian Theorem, Prior, Poster, list can go on. They are not as rigorous as other math theorems, most importantly, they are not cool, not for cool kids. Ok, enough rambling. Let's jump into a streamlined version of Bayesian family in machine learning. Full disclosure, this blog is basically my study note, if anything is wrong please feel FREE to correct me or just close the window. 

As much as I admit that one can build good performace models without having much mathmatical background, putting pieces of math behind-the-bench together will definitely make our world of machine learning clear! 



The probability of data conditioned on a parameter value equals the likelyhood of parameter value given the same data. 

$$P(X|\theta) = L(\theta|X)$$

If we know the prob of a distribution of data, we can work out the likelihood of parameter, and vice versa. 



## Bayesians and Frequentists

Essentially, Bayesian means probabilistic. Bayes and Frequency are two approaches to probability. 

- Bayesians think of it as a measure of *belief*, so that probability is subjective and refers to the future. (Kind of opposite to the 'rigor fo math' impression right?)
- Frequentists, on the other hand, have a different view: they use probability to refer to past events - in this way it’s objective and doesn’t depend on one’s beliefs. The name comes from the method - for example: we tossed a coin 100 times, it came up heads 53 times, so the frequency/probability of heads is 0.53, period. 



## Priors, updates, and posteriors

So lets keep in mind, a Bayesian can almost never be certain about a conclusion, but they can be very confident. 

For example, we want to know the number of mint and vanilla popscals in a big box full of 100 popscals.

We can first assume there are equivlent number of both (our prior). Then randomly take 10 out, if six out of ten are mint and four are vanilla, we can now change our result by updating the prior. Then we can keep testing on a large number of popscals, and if cumlutive sum of mint is more than vanilla, we can feel more *confident* about our result, but still not certain. 

Bayesian inference works identically: we update our beliefs about an outcome; rarely can we be absolutely sure unless we rule out all other alternatives.

Therefore, as Bayesians, we start with a belief, called a prior. Then we obtain some data and use it to update our belief. The outcome is called a posterior. Should we obtain even more data, the old posterior becomes a new prior and the cycle repeats.

This process employs the **Bayes rule**:

```
P( A | B ) = P( B | A ) * P( A ) / P( B )
```

`P( A | B )`, read as “probability of A given B”, indicates a conditional probability: how likely is A if B happens.

We all learn this from high school right?  This is the formula of conditional probability that appears in basic math textbooks, however, the simplty of the idea made me overlook the important of the whole Bayesian system built on it. 



### Example

We have a population of M&M’s, and in this population the percentage of yellow M&M’s is either 10% or 20%. You have been hired as a statistical consultant to decide whether the true percentage of yellow M&M’s is 10% or 20%.

Payoffs/losses: You are being asked to make a decision, and there are associated payoff/losses that you should consider. If you make the correct decision, your boss gives you a bonus. On the other hand, if you make the wrong decision, you lose your job.

Data: You can “buy” a random sample from the population – You pay \$200 for each M&M, and you must buy in ​\$1,000 increments (5 M&Ms at a time). You have a total of $4,000 to spend, i.e., you may buy 5, 10, 15, or 20 M&Ms.

Remark: Remember that the cost of making a wrong decision is high, so you want to be fairly confident of your decision. At the same time, though, data collection is also costly, so you don’t want to pay for a sample larger than you need. If you believe that you could actually make a correct decision using a smaller sample size, you might choose to do so and save money and resources.

Let’s start with the frequentist inference.

- Hypothesis: H0 is 10% yellow M&Ms, and HA is >10% yellow M&Ms.

- Significance level: α=0.05.

- Sample: red, green, **yellow**, blue, orange

- Observed data: k=1,n=5

- P-value: $P(k \geq 1 | n=5, p=0.10) = 1 - P(k=0 | n=5, p=0.10) = 1 - 0.90^5 \approx 0.41$ 

  (Note that the p-value is the probability of observed or more extreme outcome given that the null hypothesis is true.

Therefore, we fail to reject H0 and conclude that the data do not provide convincing evidence that the proportion of yellow M&M’s is greater than 10%. This means that if we had to pick between 10% and 20% for the proportion of M&M’s, even though this hypothesis testing procedure does not actually confirm the null hypothesis, we would likely stick with 10% since we couldn’t find evidence that the proportion of yellow M&M’s is greater than 10%.



The Bayesian inference works differently as below.

- Hypotheses: H1 is 10% yellow M&Ms, and H2 is 20% yellow M&Ms.

- Prior: $P(H_1) = P(H_2) = 0.5$

- Sample: red, green, **yellow**, blue, orange

- Observed data: k=1, n=5

- Likelihood: (equal to prob of data given the parameter(H1 or H2))

  $\begin{aligned} P(k=1 | H_1) &= \left( \begin{array}{c} 5 \\ 1 \end{array} \right) \times 0.10 \times 0.90^4 \approx 0.33 \\ P(k=1 | H_2) &= \left( \begin{array}{c} 5 \\ 1 \end{array} \right) \times 0.20 \times 0.80^4 \approx 0.41 \end{aligned}$



- Posterior: (Given the oberseved data, now what is our updated prob of H1 and H2)

  $\begin{aligned} P(H_1 | k=1) &= \frac{P(H_1)P(k=1 | H_1)}{P(k=1)} = \frac{0.5 \times 0.33}{0.5 \times 0.33 + 0.5 \times 0.41} \approx 0.45 \\ P(H_2 | k=1) &= 1 - 0.45 = 0.55 \end{aligned}$



The **posterior probabilities of whether H1 orH2** is correct are close to each other. As a result, with equal priors and a low sample size, it is difficult to make a decision with a strong confidence, given the observed data. **However, H2 has a higher posterior probability than H1**, so if we had to make a decision at this point, we should pick H2, i.e., the proportion of yellow M&Ms is 20%. *Note that this decision contradicts with the decision based on the frequentist approach.*



Table summarizes what the results would look like if we had chosen larger sample sizes. Under each of these scenarios, the frequentist method yields a higher p-value than our significance level, so we would fail to reject the null hypothesis with any of these samples. On the other hand, the Bayesian method always yields a higher posterior for the second model where p is equal to 0.20. So the decisions that we would make are contradictory to each other.

|               | Frequentist                | Bayesian H_1          | Bayesian H_2          |
| ------------- | -------------------------- | --------------------- | --------------------- |
| Observed Data | P(k or more \| 10% yellow) | P(10% yellow \| n, k) | P(20% yellow \| n, k) |
| n = 5, k = 1  | 0.41                       | 0.45                  | 0.55                  |
| n = 10, k = 2 | 0.26                       | 0.39                  | 0.61                  |
| n = 15, k = 3 | 0.18                       | 0.34                  | 0.66                  |
| n = 20, k = 4 | 0.13                       | 0.29                  | 0.71                  |

However, if we had set up our framework differently in the frequentist method and set our null hypothesis to be p=0.20 and our alternative to be p<0.20, we would obtain different results. This shows that **the frequentist method is highly sensitive to the null hypothesis**, while in the Bayesian method, our results would be the same regardless of which order we evaluate our models.





## Inferring model parameters from data

In Bayesian machine learning we use the Bayes rule to infer model parameters (theta) from data (D):

```
P( theta | D ) = P( D | theta ) * P( theta ) / P( data )
```

All components of this are probability distributions.

`P( data )` is something we generally cannot compute, but since it’s just a normalizing constant, it doesn’t matter that much. When comparing models, **we’re mainly interested in expressions containing theta**, because `P( data )` stays the same for each model.

`P( theta )` is a prior, or our belief of what the model parameters might be. Most often our opinion in this matter is rather vague and if we have enough data, we simply don’t care. Inference should converge to probable theta as long as it’s not zero in the prior. One specifies a prior in terms of a parametrized distribution - see [Where priors come from](http://zinkov.com/posts/2015-06-09-where-priors-come-from/).

`P( D | theta )` is called likelihood of data given model parameters. The formula for likelihood is model-specific. People often use likelihood for evaluation of models: a model that gives higher likelihood to real data is better.

Finally, `P( theta | D )`, a posterior, is what we’re after. It’s a **probability distribution over model parameters** obtained from prior beliefs and data.

When one uses likelihood to get point estimates of model parameters, it’s called [maximum-likelihood estimation](https://en.wikipedia.org/wiki/Maximum_likelihood), or MLE. If one also takes the prior into account, then it’s [maximum a posteriori estimation](https://en.wikipedia.org/wiki/Maximum_a_posteriori_estimation) (MAP). MLE and MAP are the same if the prior is uniform.

Note that choosing a model can be seen as separate from choosing model (hyper)parameters. In practice, though, they are usually performed together, by validation.

Equation: 

$\begin{align} P( A | X ) = & \frac{ P(X | A) P(A) } {P(X) } \\\\[5pt] & \propto P(X | A) P(A)\;\; (\propto \text{is proportional to }) \end{align}$

## Model vs inference

**Inference** refers to how you **learn parameters of your model**. A model is separate from how you train it, especially in the Bayesian world.

Consider deep learning: you can train a network using Adam, RMSProp or a number of other optimizers. However, they tend to be rather similar to each other, all being variants of Stochastic Gradient Descent. In contrast, Bayesian methods of inference differ from each other more profoundly.

The two most important methods are [Monte Carlo sampling](https://en.wikipedia.org/wiki/Monte_Carlo_method) and variational inference. Sampling is a gold standard, but slow. The [excerpt from The Master Algorithm](http://fastml.com/an-excerpt-from-the-master-algorithm/) has more on MCMC.

Variational inference is a method designed explicitly to trade some accuracy for speed. It’s drawback is that it’s model-specific, but there’s light at the end of the tunnel - see the [section on software](http://fastml.com/bayesian-machine-learning/#software) below and [Variational Inference: A Review for Statisticians](http://arxiv.org/abs/1601.00670).





In the spectrum of Bayesian methods, there are two main flavours. Let’s call the first *statistical modelling* and the second *probabilistic machine learning*. The latter contains the so-called nonparametric approaches.

## Statistical modelling

Modelling happens when data is scarce and precious and hard to obtain, for example in social sciences and other settings where it is difficult to conduct a large-scale controlled experiment. Imagine a statistician meticulously constructing and tweaking a model using what little data he has. In this setting you spare no effort to make the best use of available input.

Also, with small data it is important to quantify uncertainty and that’s precisely what Bayesian approach is good at.

Bayesian methods - specifically MCMC - are usually computationally costly. This again goes hand-in-hand with small data.



## Probabilistic machine learning

Let’s try replacing “Bayesian” with “probabilistic”. From this perspective, it doesn’t differ as much from other methods. As far as classification goes, most classifiers are able to output probabilistic predictions. Even SVMs, which are sort of an antithesis of Bayesian.

By the way, these probabilities are only statements of belief from a classifier. Whether they correspond to real probabilities is another matter completely and it’s called [calibration](http://fastml.com/classifier-calibration-with-platts-scaling-and-isotonic-regression/).

### LDA, a generative model

[Latent Dirichlet Allocation](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation) is a method that one throws data at and allows it to sort things out (as opposed to manual modelling). It’s similar to matrix factorization models, especially non-negative MF. You start with a matrix where rows are documents, columns are words and each element is a count of a given word in a given document. LDA “factorizes” this matrix of size *n x d* into two matrices, documents/topics (*n x k*) and topics/words (*k x d*).

The difference from factorization is that you can’t multiply those two matrices to get the original, but since the appropriate rows/columns sum to one, you can “generate” a document. To get the first word, one samples a topic, then a word from this topic (the second matrix). Repeat this for a number of words you want. Notice that this is a bag-of-words representation, not a proper sequence of words.

The above is an example of a **generative** model, meaning that one can sample, or generate examples, from it. Compare with classifiers, which usually model `P( y | x )` to discriminate between classes based on *x*. A generative model is concerned with joint distribution of *y* and *x*, `P( y, x )`. It’s more difficult to estimate that distribution, but it allows sampling and of course one can get `P( y | x )` from `P( y, x )`.

## Bayesian nonparametrics

While there’s no exact definition, the name means that the number of parameters in a model can grow as more data become available. This is similar to Support Vector Machines, for example, where the algorithm chooses support vectors from the training points. Nonparametrics include Hierarchical Dirichlet Process version of LDA, where the number of topics chooses itself automatically, and Gaussian Processes.

### Gaussian Processes

Gaussian processes are somewhat similar to Support Vector Machines - both use kernels and have similar scalability (which has been vastly improved throughout the years by using approximations). A natural formulation for GP is regression, with classification as an afterthought. For SVM it’s the other way around.

Another difference is that GP are probabilistic from the ground up (providing error bars), while SVM are not. You can observe this in regression. Most “normal” methods only provide point estimates. Bayesian counterparts, like Gaussian processes, also output uncertainty estimates.

![img](http://fastml.com/images/bayesian-machine-learning/homoscedastic_noise.png)
Credit: Yarin Gal’s [Heteroscedastic dropout uncertainty ](https://github.com/yaringal/HeteroscedasticDropoutUncertainty)and [What my deep model doesn’t know](http://mlg.eng.cam.ac.uk/yarin/blog_3d801aa532c1ce.html)

Unfortunately, it’s not the end of the story. Even a sophisticated method like GP normally operates on an assumption of homoscedasticity, that is, uniform noise levels. In reality, noise might differ across input space (be heteroscedastic) - see the image below.

![img](http://fastml.com/images/bayesian-machine-learning/heteroscedastic_noise.png)

A relatively popular application of Gaussian Processes is hyperparameter optimization for machine learning algorithms. The data is small, both in dimensionality - usually only a few parameters to tweak, and in the number of examples. Each example represents one run of the target algorithm, which might take hours or days. Therefore we’d like to get to the good stuff with as few examples as possible.

Most of the research on GP seems to happen in Europe. English have done some interesting work on making GP easier to use, culminating in the [automated statistician](http://www.automaticstatistician.com/index/), a project led by Zoubin Ghahramani.

Watch the first 10 minutes of this [video](https://www.youtube.com/watch?v=BS4Wd5rwNwE) for an accessible intro to Gaussian Processes.





### Resources

To solidify your understanding, you might go through Radford Neal’s tutorial on [Bayesian Methods for Machine Learning](http://www.cs.toronto.edu/~radford/ftp/bayes-tut.pdf). It corresponds 1:1 to the subject of this post.

We found Kruschke’s [Doing Bayesian Data Analysis](https://sites.google.com/site/doingbayesiandataanalysis/what-s-new-in-2nd-ed), known as the puppy book, most readable. The author goes to great lengths to explain all the ins and outs of modelling.

[Statistical rethinking](http://xcelab.net/rm/statistical-rethinking/) appears to be of the similar kind, but newer. It has examples in R + Stan. The author, Richard McElreath, published a [series of lectures](https://www.youtube.com/playlist?list=PLDcUM9US4XdMdZOhJWJJD4mDBMnbTWw_z) on YouTube.

In terms of machine learning, both books only only go as far as linear models. Likewise, Cam Davidson-Pylon’s [Probabilistic Programming & Bayesian Methods for Hackers](http://camdavidsonpilon.github.io/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/#contents) covers the *Bayesian* part, but not the *machine learning* part.

The same goes to Alex Etz’ series of articles on [understanding Bayes](http://alexanderetz.com/understanding-bayes/).

For those mathematically inclined, [Machine Learning: a Probabilistic Perspective](https://www.cs.ubc.ca/~murphyk/MLbook/) by Kevin Murphy might be a good book to check out. You like hardcore? No problemo, Bishop’s [Pattern Recognition and Machine Learning](http://research.microsoft.com/en-us/um/people/cmbishop/prml/) got you covered. One recent [Reddit thread](https://www.reddit.com/r/MachineLearning/comments/4c5gqk/is_pattern_recognition_and_machine_learning_still/) briefly discusses these two.

[Bayesian Reasoning and Machine Learning](http://web4.cs.ucl.ac.uk/staff/D.Barber/pmwiki/pmwiki.php?n=Brml.HomePage) by David Barber is also popular, and freely available online, as is [Gaussian Processes for Machine Learning](http://www.gaussianprocess.org/gpml/), the classic book on the matter.

As far as we know, there’s no MOOC on Bayesian machine learning, but *mathematicalmonk* explains [machine learning](https://www.youtube.com/playlist?list=PLD0F06AA0D2E8FFBA) from the Bayesian perspective.

Stan has an extensive [manual](http://mc-stan.org/documentation/), PyMC a [tutorial](https://pymc-devs.github.io/pymc3/getting_started.html) and quite a few examples.

