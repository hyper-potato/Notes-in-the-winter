[ufldl.stanford.edu](http://ufldl.stanford.edu/tutorial/unsupervised/SparseCoding/ "Unsupervised Feature Learning and Deep Learning Tutorial")

# Unsupervised Feature Learning and Deep Learning Tutorial

Sparse coding is a class of unsupervised methods for learning sets of over-complete bases to represent data efficiently. The aim of sparse coding is to find a set of basis vectors $\mathbf{\phi}_i$ such that we can represent an input vector $\mathbf{x}$ as a linear combination of these basis vectors:

$\begin{align}\mathbf{x} = \sum_{i=1}^k a_i 4mathbf{phi}_{i} \end{align}$

While techniques such as Principal Component Analysis (PCA) allow us to learn a complete set of basis vectors efficiently, we wish to learn an **over-complete** set of basis vectors to represent input vectors $\mathbf{x}\in\mathbb{R}^n$ (i.e. such that k > n). The advantage of having an over-complete basis is that our basis vectors are better able to capture structures and patterns inherent in the input data. However, with an over-complete basis, the coefficients a_i are no longer uniquely determined by the input vector mathbf{x}. Therefore, in sparse coding, we introduce the additional criterion of **sparsity** to resolve the degeneracy introduced by over-completeness.

Here, we define sparsity as having few non-zero components or having few components not close to zero. The requirement that our coefficients a_i be sparse means that given a input vector, we would like as few of our coefficients to be far from zero as possible. The choice of sparsity as a desired characteristic of our representation of the input data can be motivated by the observation that most sensory data such as natural images may be described as the superposition of a small number of atomic elements such as surfaces or edges. Other justifications such as comparisons to the properties of the primary visual cortex have also been advanced.

We define the sparse coding cost function on a set of m input vectors as

$\begin{align} \text{minimize}_{a^{(j)}_i,\mathbf{phi}_{i}} \sum_{j=1}^{m} \left|\left| \mathbf{x}^{(j)} - \sum_{i=1}^k a^{(j)}_i \mathbf{phi}_{i}\right|\right|^{2} + \lambda sum_{i=1}^{k}S(a^{(j)}_i) \end{align}$

where S(.) is a sparsity cost function which penalizes a_i for being far from zero. We can interpret the first term of the sparse coding objective as a reconstruction term which tries to force the algorithm to provide a good representation of mathbf{x} and the second term as a sparsity penalty which forces our representation of mathbf{x} to be sparse. The constant lambda is a scaling constant to determine the relative importance of these two contributions.

Although the most direct measure of sparsity is the "L_0" norm (S(a_i) = mathbf{1}(|a_i|>0)), it is non-differentiable and difficult to optimize in general. In practice, common choices for the sparsity cost S(.) are the$ L_1$ penalty $S(a_i)=\left|a_i\right|_1$ and the log penalty $S(a_i)=log(1+a_i^2)$.

In addition, it is also possible to make the sparsity penalty arbitrarily small by scaling down a_i and scaling mathbf{phi}_i up by some large constant. To prevent this from happening, we will constrain $\left|\left|\mathbf{\phi}\right|\right|^2$ to be less than some constant C. The full sparse coding cost function including our constraint on $\mathbf{\phi}$ is



$\begin{array}{rc} \text{minimize}_{a^{(j)}_i,\mathbf{\phi}_{i}} & \sum_{j=1}^{m} \left|\left| \mathbf{x}^{(j)} - \sum_{i=1}^k a^{(j)}_i \mathbf{\phi}_{i}\right|\right|^{2} + \lambda \sum_{i=1}^{k}S(a^{(j)}_i)  \\ \text{subject to}  &  \left|\left|\mathbf{\phi}_i\right|\right|^2 \leq C, \forall i = 1,...,k  \\ \end{array}$



###  Probabilistic Interpretation

So far, we have considered sparse coding in the context of finding a sparse, over-complete set of basis vectors to span our input space. Alternatively, we may also approach sparse coding from a probabilistic perspective as a generative model.

Consider the problem of modelling natural images as the linear superposition of k independent source features $\mathbf{\phi}_i$ with some additive noise $\nu$:

$\begin{align} \mathbf{x} = \sum_{i=1}^k a_i \mathbf{\phi}_{i} + \nu(\mathbf{x}) \end{align}$

Our goal is to find a set of basis feature vectors mathbf{phi} such that the distribution of images $P(\mathbf{x}\mid\mathbf{\phi})$ is as close as possible to the empirical distribution of our input data $P^*(\mathbf{x})$. One method of doing so is to minimize the KL divergence between $P^*(\mathbf{x})$ and $P(\mathbf{x}\mid\mathbf{\phi})$ where the KL divergence is defined as:

$D(P^*(\mathbf{x})||P(\mathbf{x}\mid\mathbf{\phi})) = \int P^*(\mathbf{x}) \log \left(\frac{P^*(\mathbf{x})}{P(\mathbf{x}\mid\mathbf{\phi})}\right)d\mathbf{x}$



Since the empirical distribution $$P^*(\mathbf{x})$$is constant across our choice of $\phi$, this is equivalent to maximizing the log-likelihood of $P(\mathbf{x}\mid\mathbf{\phi})$

Assuming $\nu$ is Gaussian white noise with variance $\sigma^2$, we have that

$\begin{align} P(\mathbf{x} \mid \mathbf{a}, \mathbf{\phi}) = \frac{1}{Z} \exp\left(- \frac{(\mathbf{x}-\sum^{k}_{i=1} a_i \mathbf{\phi}_{i})^2}{2\sigma^2}\right) \end{align}$

In order to determine the distribution $P(\mathbf{x}\mid\mathbf{\phi})$, we also need to specify the prior distribution $P(\mathbf{a})$. Assuming the independence of our source features, we can factorize our prior probability as

$$P(\mathbf{a}) = \prod_{i=1}^{k} P(a_i)$$

At this point, we would like to incorporate our sparsity assumption – the assumption that any single image is likely to be the product of relatively few source features. Therefore, we would like the probability distribution of $a_i$ to be peaked at zero and have high kurtosis. A convenient parameterization of the prior distribution is

$P(a_i) = \frac{1}{Z}\exp(-\beta S(a_i))$



Where $S(a_i)$ is a function determining the shape of the prior distribution.

Having defined $P(\mathbf{x} \mid \mathbf{a}, \mathbf{\phi})$ and $P(\mathbf{a})$, we can write the probability of the data $P(\mathbf{x})$under the model defined by $\mathbf{\phi}$ as

$P(\mathbf{x} \mid \mathbf{\phi}) = \int P(\mathbf{x} \mid \mathbf{a}, \mathbf{\phi}) P(\mathbf{a}) d\mathbf{a}$

and our problem reduces to finding

$\mathbf{\phi}^*=\text{argmax}_{\mathbf{\phi}} E\left[ log(P(\mathbf{x} \mid \mathbf{\phi})) \right]$

Where $E\left[\cdot \right]$ denotes expectation over our input data.

Unfortunately, the integral over a to obtain $P(\mathbf{x} \mid \mathbf{\phi})$ is generally intractable. We note though that if the distribution of $P(\mathbf{x} \mid \mathbf{\phi})$ is sufficiently peaked (w.r.t. $\mathbf{a}$), we can approximate its integral with the maximum value of $P(\mathbf{x} \mid \mathbf{\phi})$ and obtain a approximate solution

$\mathbf{\phi}^{*'}=\text{argmax}_{\mathbf{\phi}} E\left[ \max_{\mathbf{a}} \log(P(\mathbf{x} \mid \mathbf{\phi})) \right]$

As before, we may increase the estimated probability by scaling down a_i and scaling up $\mathbf{\phi}$ (since $P(a_i)$ peaks about zero) , we therefore impose a norm constraint on our features mathbf{phi} to prevent this.

Finally, we can recover our original cost function by defining the energy function of this linear generative model

$\begin{array}{rl} E\left( \mathbf{x} , \mathbf{a} \mid \mathbf{\phi} \right) & := -\log \left( P(\mathbf{x}\mid \mathbf{\phi},\mathbf{a}\right)P(\mathbf{a})) \\ &= \sum_{j=1}^{m} \left|\left| \mathbf{x}^{(j)} - \sum_{i=1}^k a^{(j)}_i \mathbf{\phi}_{i}\right|\right|^{2} + \lambda \sum_{i=1}^{k}S(a^{(j)}_i)  \end{array}$

where $\lambda = 2\sigma^2\beta$ and irrelevant constants have been hidden. Since maximizing the log-likelihood is equivalent to minimizing the energy function, we recover the original optimization problem:

$\begin{align} \mathbf{\phi}^{*},\mathbf{a}^{*}=\text{argmin}_{\mathbf{\phi},\mathbf{a}} \sum_{j=1}^{m} \left|\left| \mathbf{x}^{(j)} - \sum_{i=1}^k a^{(j)}_i \mathbf{\phi}_{i}\right|\right|^{2} + \lambda \sum_{i=1}^{k}S(a^{(j)}_i)  \end{align}$

Using a probabilistic approach, it can also be seen that the choices of the L_1 penalty left|a_iright|_1 and the log penalty $\log(1+a_i^2)$ for S(.) correspond to the use of the $P(a_i) \propto \exp\left(-\beta|a_i|\right)$ and the Cauchy prior $P(a_i) \propto \frac{\beta}{1+a_i^2}$respectively.

###  Learning

Learning a set of basis vectors $\phi$ using sparse coding consists of performing two separate optimizations, the first being an optimization over coefficients a_i for each training example f{x} and the second an optimization over basis vectors $\phi$ across many training examples at once.

Assuming an $L_1$ sparsity penalty, learning $a^{(j)}_i$ reduces to solving a$L_1$ regularized least squares problem which is convex in $a^{(j)}_i$ for which several techniques have been developed (convex optimization software such as CVX can also be used to perform L1 regularized least squares). Assuming a differentiable S(.) such as the log penalty, gradient-based methods such as conjugate gradient methods can also be used.

Learning a set of basis vectors with a L_2 norm constraint also reduces to a least squares problem with quadratic constraints which is convex in $\phi$. Standard convex optimization software (e.g. CVX) or other iterative methods can be used to solve for $\phi$ although significantly more efficient methods such as solving the Lagrange dual have also been developed.

As described above, a significant limitation of sparse coding is that even after a set of basis vectors have been learnt, in order to "encode" a new data example, optimization must be performed to obtain the required coefficients. This significant "runtime" cost means that sparse coding is computationally expensive to implement even at test time especially compared to typical feedforward architectures.
