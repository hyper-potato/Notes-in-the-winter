## Conditional independence probabilistically

The intuition of Conditional Independence:

Let’s say **A** is **the height of a child** and **B** is **the number of words that the child knows**. It seems when **A** is high, **B** is high too. There is **a single piece of information that will make A and B completely independent.** What would that be?

> The child’s age.

The height and the # of words known by the kid are **NOT independent**, but they are **conditionally independent** if you provide the kid’s age.

<img src='https://miro.medium.com/max/2502/1*EuHD5NO9vzIbG-KToFWc4g.png'>

Conditional independence is basically the concept of independence **P(A ∩ B) = P(A) \* P(B)** applied to the conditional model.



<img src="https://i.loli.net/2019/11/26/KdCpMfYzW3tokb1.png" alt="image.png" style="zoom: 33%;" />



## Joint probability distributions 

$p(x_1,\ldots,x_n) = \prod_{i\in V} p(x_i|x_{\text{parents}(i)}).$



Bayes Nets are a more compact representation of the joint probability distribution: we only need to store a number of probabilities exponential in the number of parents per node, not the total number of nodes.

![image.png](https://i.loli.net/2019/11/27/gGPCFioWXArlItJ.png)

**flow from parents to the end**



§ Know how to compute the number of parameters required to estimate either 

- To score a structure, learn all parameters from the training data by maximum likelihood.
- Then compute the log-likelihood of the training dataset given the structure and parameters.
- $Score = log-likelihood – \lambda k$, where $\lambda$ is a constant and k is the total number of parameters.



o Be able to read a Bayesian Network and know which variables are conditionally independent of which others.





o Understand how to preform inference given a Bayesian Network 

joint probabilities using a Bayes Net

arbitrary conditional probabilities 



o Know how can they be used in anomaly detection.

We can also use Bayes Nets to detect anomalies, by finding points with low probabilities given the Bayes Net.



o Be able to articulate what are their benefits and drawbacks.

Bayes Nets provide a useful graphical representation of the probabilistic relationships between many variables.

Automatic learning of Bayes net structure can be used for exploratory analysis of datasets with many attributes.

We can often improve the performance of model-based classification by moving from Naïve Bayes to Bayes Nets.

We can also use Bayes Nets to detect anomalies, by finding points with low probabilities given the Bayes Net.

Bayes Nets provide a compact structure which enables us to efficiently compute probability distributions for any unobserved variables given observations of other variables.

Medical diagnosis

Failure troubleshooting

Drug discovery

Environmental modeling

Computational biology





Drawbacks:

1. Computationally expensive. Eg: Approximate structure learning is too NP-Complete
2. Bayesian networks tend to perform poorly on **high dimensional data.** 
3. Forces random variables to be in a cause-effect relationship. As a result, it does not depicts variables which are correlated. 
4. Adding to point 2, BN is a DAG that said. If the data was generated from a model where there at least 3 variables correlated to each other (cyclic relationship) then Bayesian networks (BNs) will not be able to model this relationship.
5. One of the most important issues with BNs is that some of the sophisticated scoring functions require reliable priors in order to find a structure closer to the original model.